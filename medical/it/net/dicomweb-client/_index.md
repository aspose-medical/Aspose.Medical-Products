---
title: Client DICOMweb in C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Cerca, scarica e carica studi DICOM via HTTP da .NET. Un client DICOMweb in puro C# per QIDO-RS, WADO-RS e STOW-RS che funziona con PACS, VNA e servizi di imaging cloud.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Client DICOMweb per .NET C#" h2="Cerca, scarica e carica studi su qualsiasi server DICOMweb con QIDO-RS, WADO-RS e STOW-RS, in C# gestito senza necessità di installare nulla sulla macchina." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM via HTTP, dal tuo codice">}}

<p>DICOMweb è la forma HTTP di DICOM. Invece di un'associazione DIMSE sulla porta 104, la tua applicazione invia normali richieste HTTPS: QIDO-RS per la ricerca, WADO-RS per il download, STOW-RS per il caricamento. I servizi di imaging cloud come Azure Health Data Services DICOM service e Google Cloud Healthcare API funzionano in questo modo, così come archivi quali Orthanc e dcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> include un client DICOMweb dalla versione 26.10. <code>DicomWebClient</code> incapsula ogni transazione standard in un metodo tipizzato, e i risultati arrivano come gli stessi oggetti <code>Dataset</code> e <code>DicomFile</code> che il resto della libreria legge e scrive. Il server dall'altra parte può essere qualsiasi implementazione DICOMweb; non è necessario che sia costruito con Aspose.Medical.</p>

<p>Il client è C# gestito sopra <code>HttpClient</code>. Non esiste alcun toolkit nativo né servizio proxy, quindi lo stesso codice funziona su Windows, su Linux e in un container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Connetti a un server DICOMweb">}}

<p>Crea il client a partire da un <code>HttpClient</code> e dall'URL radice del servizio del server. L'autenticazione, le impostazioni TLS, i proxy e il pooling delle connessioni rimangono sul <code>HttpClient</code>, dove la tua applicazione li configura già. Il client non possiede il <code>HttpClient</code> e non lo elimina.</p>

<div class="codeblock" id="code">
 <h3>Crea un client DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Se una distribuzione fornisce WADO-URI o un altro servizio da un URL diverso, <code>ServiceBaseUris</code> sovrascrive la radice solo per quel servizio.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Cerca studi con QIDO-RS">}}

<p>Una ricerca accetta parametri di corrispondenza con la parola chiave DICOM come nome, i campi da restituire e i controlli di paginazione. Le corrispondenze arrivano come un flusso di dataset, letti uno alla volta mentre la risposta è ancora in fase di download.</p>

<div class="codeblock" id="code">
 <h3>Trova gli studi di un paziente - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> e <code>SearchInstancesAsync</code> cercano un livello più in basso, nell'intero archivio o all'interno di uno studio o serie. Le stesse opzioni impostano inoltre il fuzzy matching, la corrispondenza di valori vuoti e l'offset del risultato per la paginazione.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Scarica uno studio con WADO-RS">}}

<p>Uno studio può contenere migliaia di istanze. <code>RetrieveStudyAsync</code> le restituisce come un flusso, così ogni file viene analizzato man mano che la sua parte della risposta arriva e può essere salvato prima che il successivo venga letto. L'uso della memoria non aumenta con la dimensione dello studio.</p>

<div class="codeblock" id="code">
 <h3>Salva ogni istanza di uno studio - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> e <code>RetrieveInstanceAsync</code> scaricano una serie o un'istanza nello stesso modo.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ottieni una miniatura o un'immagine renderizzata">}}

<p>Un visualizzatore o una schermata della lista di lavoro spesso richiedono un'immagine, non un file DICOM. Il server la renderizza e invia un JPEG o PNG, così il client non scarica né decodifica i dati pixel originali.</p>

<div class="codeblock" id="code">
 <h3>Salva una miniatura JPEG di un'istanza - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p><code>RetrieveRenderedInstanceAsync</code> e <code>RetrieveRenderedFramesAsync</code> restituiscono invece un'immagine a grandezza naturale. <code>DicomWebRenderOptions</code> passa i controlli di rendering standard al server: qualità dell'immagine, dimensione del viewport, finestra e profilo ICC.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Carica istanze con STOW-RS">}}

<p>STOW-RS invia qualsiasi numero di istanze in una singola richiesta. Il server risponde con un report che elenca, istanza per istanza, ciò che ha memorizzato e ciò che ha rifiutato.</p>

<div class="codeblock" id="code">
 <h3>Carica una cartella di file DICOM - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>Alcuni server rispondono con un errore HTTP, ad esempio 409, quando una o più istanze falliscono. La chiamata lancia quindi <code>DicomWebClientException</code>, come mostra la sezione successiva, e lo stesso report è nel corpo della risposta dell'eccezione.</p>

<p><code>StoreStudyAsync</code> carica in uno studio con nome. Entrambi i metodi accettano anche un <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, così i file possono essere prodotti mentre la richiesta è in corso di invio.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gestisci gli errori del server">}}

<p>Ogni operazione tipizzata controlla lo stato HTTP prima di restituire. Quando il server rifiuta una richiesta, la chiamata lancia <code>DicomWebClientException</code>, che porta il codice di stato, le intestazioni e l'inizio del corpo della risposta per il registro.</p>

<div class="codeblock" id="code">
 <h3>Segnala una richiesta fallita - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>Metodi che restituiscono una risposta grezza, come la miniatura sopra, lasciano a te il controllo dello stato tramite <code>IsSuccessStatusCode</code> o <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Altre funzionalità coperte dal client">}}

<p>Ricerca, download e caricamento sono le chiamate comuni. Il client ha anche un gruppo di operazioni per ciascuno degli altri servizi DICOMweb:</p>

<ul>
<li>Dati bulk, dati pixel e fotogrammi selezionati di un'istanza, per numero di fotogramma a base uno, così come li invia il server.</li>
<li>WADO-URI, il più vecchio recupero a singolo URL che molti visualizzatori e archivi usano ancora, per file DICOM e immagini renderizzate.</li>
<li>Liste di lavoro UPS-RS: creare, cercare, aggiornare e rivendicare workitems, cambiare il loro stato, richiedere la cancellazione e sottoscrivere eventi che arrivano via WebSocket.</li>
<li>La lista di lavoro della modalità (MWL-RS), i passi di procedura eseguiti dalla modalità (MPPS-RS) e l'impegno di storage.</li>
<li>Istanze non-paziente come palette di colori e protocolli di hanging, e il documento delle capacità del server.</li>
<li><code>SendAsync</code> per qualsiasi transazione standard con i propri valori di percorso, parametri di query, intestazioni e corpo, quando un metodo tipizzato non è appropriato.</li>
</ul>

<p>La <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">guida al client DICOMweb</a> documenta ogni operazione e opzione, e il <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">riferimento delle operazioni del client</a> elenca tutti i metodi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Una libreria dalla risposta HTTP ai dati pixel">}}

<p>Un file scaricato con il client è un normale <code>DicomFile</code>. Può essere anonimizzato, transcodificato, renderizzato o serializzato nello stesso processo, senza passaggi di conversione e senza una seconda libreria nella catena.</p>

<p>Pagine correlate: <a href="/medical/net/dicomweb-server/">server DICOMweb</a> per l'altro lato del protocollo nel tuo servizio ASP.NET Core, <a href="/medical/net/dicom-networking/">networking DICOM</a> per archivi che usano DIMSE invece di HTTP, <a href="/medical/net/anonymization/">anonimizzazione</a> prima che uno studio lasci l'ospedale, <a href="/medical/net/dicom-transfer-syntax-conversion/">conversione della transfer syntax</a> per ciò che il server memorizza, e <a href="/medical/net/dicom-to-json/">DICOM a JSON</a> per il formato usato nei risultati di ricerca QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Risorse di apprendimento" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentazione" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guida per sviluppatori" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
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
