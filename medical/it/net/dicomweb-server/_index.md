---
title: Server DICOMweb in C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Crea il tuo server DICOMweb o PACS in C#. Aspose.Medical implementa QIDO-RS, WADO-RS, STOW-RS e i servizi di worklist su ASP.NET Core; tu colleghi lo storage.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Server DICOMweb per .NET C#" h2="Aggiungi endpoint QIDO-RS, WADO-RS e STOW-RS a un servizio ASP.NET Core. La libreria parla il protocollo; il tuo codice decide dove vengono archiviati gli studi." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Il tuo server DICOMweb in C#">}}

<p>Un PACS, un VNA, un archivio cloud o una piattaforma di ricerca deve rispondere alle richieste DICOMweb: ricerche QIDO-RS, download WADO-RS e upload STOW-RS, e spesso anche i servizi di worklist. Ognuno di essi ha i propri percorsi, tipi di media, corpi multipart, regole di corrispondenza e codici di stato.</p>

<p><strong>Aspose.Medical for .NET</strong> implementa quella parte del protocollo sin dalla versione 26.10. Legge ogni richiesta, la verifica rispetto allo standard, corrisponde le chiavi di ricerca secondo le regole DICOM, pagina i risultati, converte le transfer syntax e scrive la risposta. Il tuo codice fa ciò che solo tu puoi fare: memorizzare un'istanza, trovare i candidati per una ricerca e restituire i file memorizzati. Visualizzatori, modalità e altri archivi funzionano quindi con il tuo server come con qualsiasi altro server DICOMweb, così come il <a href="/medical/net/dicomweb-client/">client DICOMweb</a> della stessa libreria.</p>

<p>Il server è gestito in C#. Funziona ovunque ASP.NET Core funzioni: su Windows, su Linux e in un container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Un server completo in ASP.NET Core">}}

<p>Installa il pacchetto <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Porta con sé Aspose.Medical e collega i servizi DICOMweb all'iniezione delle dipendenze e al routing degli endpoint. Questo <code>Program.cs</code> è un server Studies funzionante; le due classi che registra sono mostrate di seguito.</p>

<div class="codeblock" id="code">
 <h3>Program.cs di un server DICOMweb - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>Ogni route Studies ora risponde sotto <code>/dicomweb</code>: <code>/dicomweb/studies</code> per la ricerca e l'upload, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> per il recupero, e le route per metadata, frame, bulk data e immagini renderizzate. Il documento delle capacità nella radice del servizio elenca esattamente le transazioni supportate dal server.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Collega il tuo storage">}}

<p>Il servizio Studies richiede un'interfaccia con tre metodi: memorizzare un'istanza, restituire i file memorizzati e restituire i candidati della ricerca. Questa implementazione conserva ogni istanza come file in una cartella per studio e serie.</p>

<div class="codeblock" id="code">
 <h3>Un archivio a cartelle dietro QIDO-RS, WADO-RS e STOW-RS - C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>La libreria verifica gli UID in ogni percorso di richiesta e in ogni istanza caricata prima di chiamare il provider. Un'istanza senza UID validi è segnalata come fallita e non raggiunge mai <code>StoreAsync</code>.</p>

<p>Una cartella è sufficiente per provare il server. Un archivio di produzione mantiene gli stessi tre metodi e mette uno storage di oggetti dietro <code>StoreAsync</code> e <code>RetrieveAsync</code> e un indice di database dietro <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ricerca: la libreria applica le regole di corrispondenza DICOM">}}

<p>Il matching QIDO-RS è più di un semplice confronto di stringhe: caratteri jolly, intervalli di data e ora, elenchi di UID, nomi di persona fuzzy, valori vuoti e chiavi all'interno di sequenze. Il provider non implementa nulla di ciò. Restituisce i candidati in ordine stabile e può restituire più risultati di quanti ne corrispondano. La libreria applica la corrispondenza DICOM esatta a ciascun candidato, conserva i campi richiesti e applica <code>offset</code> e <code>limit</code>. Una risposta contiene al massimo 1000 risultati per impostazione predefinita, e <code>DicomWebStandardServiceOptions</code> modifica tale limite.</p>

<p>Per mantenere le ricerche veloci su un archivio grande, passa le chiavi selettive al database. <code>search.Plan.Filter</code> contiene le chiavi di ricerca compilate, con l'attributo, il tipo di corrispondenza e i valori di ciascuna:</p>

<div class="codeblock" id="code">
 <h3>Leggi un ID Paziente esatto da una ricerca - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Una query al database costruita in questo modo può restituire righe extra, ad esempio quando ignora una chiave che non può tradurre. La corrispondenza finale nella libreria le rimuove. Non deve scartare corrispondenze vere, poiché la libreria non può reinserirle.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Memorizzazione: un risultato per ogni istanza">}}

<p>Una richiesta STOW-RS può contenere centinaia di istanze. La libreria analizza il corpo multipart e chiama <code>StoreAsync</code> per ciascuna istanza a turno, così l'intero upload non viene mai tenuto interamente in memoria. Per ogni istanza il provider restituisce <code>Stored</code>, <code>Warning</code> per un'istanza memorizzata con modifiche, o <code>Failed</code> con un codice di motivo DICOM opzionale.</p>

<p>La libreria costruisce il report STOW-RS da questi risultati e sceglie lo stato: 200 quando ogni istanza è memorizzata, 202 quando alcune hanno avvisi o falliscono, 409 quando tutte falliscono. Le istanze in una richiesta per uno studio denominato che appartengono a un altro studio sono rifiutate prima che il provider sia chiamato.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Recupero: transfer syntax, frame e immagini renderizzate">}}

<p>Il provider restituisce i file memorizzati, e la libreria li consegna nella forma richiesta dal client. Un client WADO-RS può richiedere una transfer syntax nell'header <code>Accept</code>, e la libreria converte ciascun file prima di inviarlo, con i propri codec, HTJ2K e JPEG XL inclusi. Metadati in DICOM JSON o XML, bulk data e singoli frame provengono dallo stesso provider, senza codice aggiuntivo.</p>

<p>Le immagini renderizzate richiedono un'altra classe, perché solo tu decidi quali immagini il server può produrre. Questa offre JPEG per istanze renderizzate, frame renderizzati e thumbnail, e la libreria li renderizza:</p>

<div class="codeblock" id="code">
 <h3>Offri immagini JPEG renderizzate e thumbnail - C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>Il renderer integrato genera JPEG da un'istanza a singolo frame o dai frame selezionati da una richiesta. Un'istanza multi-frame renderizzata nella sua interezza, un'immagine più larga o più alta di 65500 pixel (i thumbnail superano tali immagini), altri formati immagine, e i parametri di viewport, window e profilo ICC necessitano del tuo <code>RetrieveAsync</code> in questa classe.</p>

<p>I thumbnail sono gestiti con attenzione per la privacy del paziente. Il renderer integrato crea un thumbnail solo da un'istanza i cui attributi Burned In Annotation e Recognizable Visual Features sono entrambi <code>NO</code>, il che significa che l'immagine stessa dichiara di non mostrare dati del paziente. Molte immagini non possiedono questi attributi, e per esse il server risponde 406. Se il tuo archivio necessita di thumbnail di tali immagini, produrle nel tuo <code>RetrieveAsync</code> e assicurati che nascondano qualsiasi dato del paziente.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklist, fasi di procedura e gli altri servizi">}}

<p>Gli altri servizi DICOMweb seguono lo stesso modello: registra il servizio, implementa la sua interfaccia di storage.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> worklist con <code>AddStandardWorklistService</code>: crea, cerca e aggiorna workitem e cambia il loro stato, con le regole di stato verificate dalla libreria e notifiche di evento inviate tramite WebSocket.</li>
<li>La <a href="/medical/net/modality-worklist/">modality worklist</a> (MWL-RS) con <code>AddStandardModalityWorklistService</code>, così le modalità possono interrogare le fasi di procedura pianificate del tuo RIS.</li>
<li>Fasi di procedura eseguite dalla modalità (MPPS-RS), impegno di storage e istanze non-paziente come palette di colori e hanging protocol.</li>
<li>Recupero WADO-URI per visualizzatori più vecchi, fornito dal servizio Studies.</li>
</ul>

<p>Quando il comportamento standard di una transazione non è adatto, registra il tuo gestore per essa con <code>AddHandler</code>. La libreria continua a instradare e verificare la richiesta, e le altre transazioni mantengono i loro gestori standard.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sicurezza e hosting rimangono nelle tue mani">}}

<p>Gli endpoint DICOMweb sono normali endpoint ASP.NET Core. Autenticazione, autorizzazione, CORS, TLS, header inoltrati e limiti di dimensione delle richieste sono configurati allo stesso modo del resto del tuo servizio:</p>

<div class="codeblock" id="code">
 <h3>Richiedi un utente autenticato - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>La parte protocollo non dipende da ASP.NET Core. <code>DicomWebApplication</code> trasforma una <code>DicomWebRequest</code> in una risposta, così gli stessi servizi possono essere eseguiti dietro un altro server HTTP o in un test senza rete:</p>

<div class="codeblock" id="code">
 <h3>Lo stesso server senza ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Client e server da una sola libreria">}}

<p>Il <a href="/medical/net/dicomweb-client/">client DICOMweb</a> di Aspose.Medical testa il tuo server da un test unitario o da un programma console, e ogni file che il server riceve è un normale <code>DicomFile</code> che può essere anonimizzato, transcodificato o renderizzato nello stesso processo.</p>

<p>La <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">guida per sviluppatori DICOMweb</a> copre ogni servizio e interfaccia di storage, e la <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">guida ASP.NET Core</a> tratta il mapping degli endpoint e i WebSocket.</p>

<p>Pagine correlate: <a href="/medical/net/dicom-networking/">DICOM networking</a> per i sistemi che inviano studi via DIMSE invece di HTTP, <a href="/medical/net/anonymization/">anonimizzazione</a> degli studi in ingresso, <a href="/medical/net/dicom-transfer-syntax-conversion/">conversione di transfer syntax</a>, e <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> per il formato utilizzato dai risultati QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Risorse di apprendimento" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentazione" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guida per sviluppatori" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="Riferimenti API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Supporto prodotto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Supporto gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Supporto a pagamento" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Perché Aspose.Medical per .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Elenco clienti" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Storie di successo" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
