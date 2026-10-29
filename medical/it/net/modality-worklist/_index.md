---
title: Worklist di Modalità e MPPS in C# .NET - DICOM C-FIND, N-CREATE, N-SET | Aspose.Medical
weight: 9300

description: Interroga la worklist della modalità DICOM con C-FIND e segnala MPPS da C#, oppure esegui il tuo server worklist. DIMSE e DICOMweb in una singola libreria .NET.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Worklist di Modalità e MPPS per .NET C#" h2="Ottieni gli esami del giorno dal RIS, segnala ciascuno come iniziato e completato, oppure diventa tu stesso il server worklist, via DIMSE o DICOMweb." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Dal programma RIS all'esame eseguito">}}

<p>Prima di una scansione, il tecnico non dovrebbe inserire manualmente il nome del paziente, l'ID paziente e il numero di accession nella modalità. Il RIS ha già programmato l'esame, e la modalità lo richiede: una query Modality Worklist restituisce i passaggi di procedura programmati per questo scanner e per questa giornata. La modalità copia i dati del paziente e dell'ordine nelle immagini, così lo studio arriva nel PACS già associato al relativo ordine.</p>

<p>Durante l'esecuzione dell'esame, la modalità lo segnala con MPPS (Modality Performed Procedure Step): IN PROGRESS quando la scansione inizia, COMPLETED o DISCONTINUED quando termina, includendo le serie prodotte. Il RIS utilizza questi report per aggiornare il programma, informare il radiologo che le immagini stanno arrivando e per la fatturazione.</p>

<p><strong>Aspose.Medical per .NET</strong> copre entrambe le parti di questo flusso di lavoro in C# gestito: la modalità che interroga e segnala, e il server worklist che risponde. Entrambi operano su DIMSE, il classico protocollo di rete DICOM, e su DICOMweb.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Interroga la worklist con C-FIND">}}

<p>Una query worklist è una richiesta C-FIND nel modello informativo Modality Worklist. <code>CFindRequest.CreateWorklistQuery</code> la costruisce con le consuete chiavi di ritorno: dati demografici del paziente, numero di accession, procedura richiesta e il passaggio di procedura programmato. I filtri impostati da una modalità sono il proprio titolo AE, la sua modalità e la data, tutti all'interno del Scheduled Procedure Step.</p>

<div class="codeblock" id="code">
 <h3>Ottieni gli esami di oggi per uno scanner CT - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "RIS_AE",
        Calling = "CT01",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.20"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityWorklistInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddCFindHandler((request, response, cancellationToken) =&gt;
    {
        // Every scheduled exam arrives as one identifier; the final response has none
        if (response.Identifier is Dataset item)
        {
            Dataset step = item.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);
            Console.WriteLine($"{item.GetSingleValueOrDefault(Tag.AccessionNumber, string.Empty)} " +
                              $"{item.GetSingleValueOrDefault(Tag.PatientName, string.Empty)} " +
                              $"{step.GetSingleValueOrDefault(Tag.ScheduledProcedureStepDescription, string.Empty)}");
        }

        return ValueTask.CompletedTask;
    })
    .Build();

// CT exams for this scanner
CFindRequest query = CFindRequest.CreateWorklistQuery(
    patientId: null,
    patientName: null,
    stationAe: "CT01",
    stationName: null,
    modality: "CT",
    scheduledDateTime: null);

// Scheduled for today
query.Identifier.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence)
    .AddOrUpdate(Tag.ScheduledProcedureStepStartDate, DateTimeOffset.Now.ToString("yyyyMMdd"));

client.QueueRequest(query);

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Ogni corrispondenza è un dataset con gli attributi del paziente e dell'ordine al livello superiore e il Scheduled Procedure Step in una sequenza. Per richiedere ulteriori chiavi di ritorno, aggiungile a <code>query.Identifier</code> con un valore vuoto prima che la richiesta venga accodata.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Segnala l'esame con MPPS">}}

<p>MPPS sono due operazioni DIMSE normalizzate sulla classe SOP Modality Performed Procedure Step: N-CREATE quando l'esame inizia e N-SET quando termina. La modalità sceglie l'UID del passaggio eseguito e lo utilizza per entrambi. Gli attributi N-CREATE indicano chi è stato esaminato e quale passaggio programmato è stato eseguito; la maggior parte proviene direttamente dall'elemento worklist.</p>

<div class="codeblock" id="code">
 <h3>Gli attributi N-CREATE da un elemento worklist - C#</h3>
 <pre><code class="cs">// N-CREATE attributes of a performed procedure step, filled from the worklist item the modality works on
public static Dataset InProgress(Dataset item, string stationAe)
{
    Dataset step = item.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);

    Dataset scheduled = new();
    Copy(item, scheduled, Tag.StudyInstanceUID, Tag.AccessionNumber, Tag.RequestedProcedureID, Tag.RequestedProcedureDescription);
    Copy(step, scheduled, Tag.ScheduledProcedureStepID, Tag.ScheduledProcedureStepDescription);
    scheduled.AddOrUpdate(new Sequence(Tag.ReferencedStudySequence, []));
    scheduled.AddOrUpdate(new Sequence(Tag.ScheduledProtocolCodeSequence, []));

    Dataset mpps = new();
    mpps.AddOrUpdate(new Sequence(Tag.ScheduledStepAttributesSequence, [scheduled]));
    Copy(item, mpps, Tag.PatientName, Tag.PatientID, Tag.PatientBirthDate, Tag.PatientSex, Tag.StudyID);
    Copy(step, mpps, Tag.Modality);
    mpps.AddOrUpdate(new Sequence(Tag.ReferencedPatientSequence, []));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepID, step.GetSingleValueOrDefault(Tag.ScheduledProcedureStepID, string.Empty));
    mpps.AddOrUpdate(Tag.PerformedStationAETitle, stationAe);
    mpps.AddOrUpdate(Tag.PerformedStationName, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedLocation, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedProcedureStepStartDate, DateTimeOffset.Now.ToString("yyyyMMdd"));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepStartTime, DateTimeOffset.Now.ToString("HHmmss"));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepStatus, "IN PROGRESS");
    mpps.AddOrUpdate(Tag.PerformedProcedureStepDescription, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedProcedureTypeDescription, string.Empty);
    mpps.AddOrUpdate(new Sequence(Tag.ProcedureCodeSequence, []));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepEndDate, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedProcedureStepEndTime, string.Empty);
    mpps.AddOrUpdate(new Sequence(Tag.PerformedProtocolCodeSequence, []));
    mpps.AddOrUpdate(new Sequence(Tag.PerformedSeriesSequence, []));
    return mpps;
}

// Copies the attributes the source has; a missing one is sent empty
private static void Copy(Dataset source, Dataset target, params Tag[] tags)
{
    foreach (Tag tag in tags)
    {
        if (source.GetOrDefault(tag) is IElement element)
            target.AddOrUpdate(element);
        else
            target.AddOrUpdate(tag, string.Empty);
    }
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Gli attributi N-SET che completano il passaggio - C#</h3>
 <pre><code class="cs">// N-SET attributes that close the step and list the series the modality produced
public static Dataset Completed(string seriesInstanceUid, string protocolName)
{
    Dataset series = new();
    series.AddOrUpdate(Tag.SeriesInstanceUID, seriesInstanceUid);
    series.AddOrUpdate(Tag.ProtocolName, protocolName);
    series.AddOrUpdate(Tag.SeriesDescription, string.Empty);
    series.AddOrUpdate(Tag.PerformingPhysicianName, string.Empty);
    series.AddOrUpdate(Tag.OperatorsName, string.Empty);
    series.AddOrUpdate(Tag.RetrieveAETitle, string.Empty);
    series.AddOrUpdate(new Sequence(Tag.ReferencedImageSequence, []));
    series.AddOrUpdate(new Sequence(Tag.ReferencedNonImageCompositeSOPInstanceSequence, []));

    Dataset completed = new();
    completed.AddOrUpdate(Tag.PerformedProcedureStepStatus, "COMPLETED");
    completed.AddOrUpdate(Tag.PerformedProcedureStepEndDate, DateTimeOffset.Now.ToString("yyyyMMdd"));
    completed.AddOrUpdate(Tag.PerformedProcedureStepEndTime, DateTimeOffset.Now.ToString("HHmmss"));
    completed.AddOrUpdate(new Sequence(Tag.PerformedSeriesSequence, [series]));
    return completed;
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Invia IN PROGRESS, poi COMPLETED - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "RIS_AE",
        Calling = "CT01",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.20"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityPerformedProcedureStepSOPClass,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddNCreateHandler((request, response, cancellationToken) =&gt;
    {
        Console.WriteLine($"N-CREATE: 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .AddNSetHandler((request, response, cancellationToken) =&gt;
    {
        Console.WriteLine($"N-SET: 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .Build();

// The modality chooses the UID of the performed step; N-SET refers to it later
Uid mppsUid = Uid.CreateDerivedFromUuid();

client.QueueRequest(new NCreateRequest(Uid.ModalityPerformedProcedureStepSOPClass, mppsUid, InProgress(item, "CT01")));
await client.SendAsync(CancellationToken.None);

// ... the scan runs ...

client.QueueRequest(new NSetRequest(Uid.ModalityPerformedProcedureStepSOPClass, mppsUid, Completed("1.2.826.0.1.3680043.10.1.1", "CHEST")));
await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Un esame che si interrompe anticipatamente termina con DISCONTINUED anziché COMPLETED, e il N-SET trasporta quindi il motivo nella sequenza Performed Procedure Step Discontinuation Reason Code.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lo stesso flusso di lavoro su DICOMweb">}}

<p>I più recenti RIS e piattaforme cloud offrono la worklist e MPPS via HTTP. Il <a href="/medical/net/dicomweb-client/">client DICOMweb</a> di Aspose.Medical ricerca i Scheduled Procedure Steps e segnala il passo eseguito con gli stessi dataset sopra. Una chiave all'interno del Scheduled Procedure Step è scritta come percorso, ad esempio <code>ScheduledProcedureStepSequence.Modality</code>.</p>

<div class="codeblock" id="code">
 <h3>Interroga la worklist e segnala MPPS su DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://ris.example.org/dicomweb/")
});

// The same keys as the C-FIND query, as query parameters
DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters =
    [
        new DicomWebQueryParameter("ScheduledProcedureStepSequence.Modality", "CT"),
        new DicomWebQueryParameter("ScheduledProcedureStepSequence.ScheduledStationAETitle", "CT01"),
        new DicomWebQueryParameter("ScheduledProcedureStepSequence.ScheduledProcedureStepStartDate", DateTimeOffset.Now.ToString("yyyyMMdd"))
    ]
};

List&lt;Dataset&gt; worklist = [];
await using (DicomWebClientStream&lt;Dataset&gt; steps = await client.ModalityWorklist.SearchAsync(search))
{
    await foreach (Dataset step in steps)
        worklist.Add(step);
}

// The same MPPS attributes go to the procedure step resource
Dataset item = worklist[0];
string mppsUid = Uid.CreateDerivedFromUuid().Code;
await client.ModalityPerformedProcedureSteps.CreateAsync(mppsUid, InProgress(item, "CT01"));
await client.ModalityPerformedProcedureSteps.UpdateAsync(mppsUid, Completed("1.2.826.0.1.3680043.10.1.2", "CHEST"));</code></pre>
</div>

<p>Il client viene eseguito sul tuo <code>HttpClient</code>, quindi l'autenticazione e TLS sono configurati lì.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Il tuo server worklist su DIMSE">}}

<p>Un broker worklist, un piccolo RIS o un sistema di test per l'integrazione della modalità deve rispondere all'altro lato. Registra un handler C-FIND per la worklist e gli handler N-CREATE e N-SET per MPPS, così l'applicazione diventa un worklist SCP e un MPPS SCP.</p>

<div class="codeblock" id="code">
 <h3>Avvia un server worklist e MPPS - C#</h3>
 <pre><code class="cs">// One handler instance answers N-CREATE and N-SET, so both see the same steps
MppsHandler mpps = new MppsHandler();

DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Any, 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityWorklistInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityPerformedProcedureStepSOPClass,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddSingletonCFindHandler(new WorklistHandler("worklist"))
    .AddSingletonNCreateHandler(mpps)
    .AddSingletonNSetHandler(mpps)
    .Build();

await server.StartAsync(CancellationToken.None);</code></pre>
</div>

<p>Questo handler legge gli esami programmati da file <code>.wl</code>, un dataset per file, e confronta le tre chiavi che una modalità solitamente invia. Su DIMSE l'handler gestisce il matching: caratteri jolly, intervalli di date e la selezione delle chiavi di ritorno sono definiti al suo interno.</p>

<div class="codeblock" id="code">
 <h3>L'handler C-FIND della worklist - C#</h3>
 <pre><code class="cs">// Answers worklist queries from the .wl files in a folder, one scheduled exam per file
public sealed class WorklistHandler(string folder) : ICFindRequestHandler
{
    public async IAsyncEnumerable&lt;CFindResponse&gt; Handle(CFindRequest request,
        [EnumeratorCancellation] CancellationToken cancellationToken)
    {
        // A modality filters inside the scheduled procedure step: its AE title, its modality, the date
        Dataset keys = request.Identifier.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);
        Tag[] filters = [Tag.ScheduledStationAETitle, Tag.Modality, Tag.ScheduledProcedureStepStartDate];

        foreach (string path in Directory.EnumerateFiles(folder, "*.wl"))
        {
            DicomFile file = await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
            Dataset step = file.Dataset.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);
            if (filters.All(tag =&gt; Matches(keys, step, tag)))
                yield return WithStatus(new CFindResponse(file.Dataset), 0xFF00);
        }

        yield return WithStatus(new CFindResponse(null), 0x0000);
    }

    // Single value matching: an empty key matches every value
    private static bool Matches(Dataset keys, Dataset step, Tag tag)
    {
        string key = keys.GetSingleValueOrDefault(tag, string.Empty)!;
        return key.Length == 0 || key == step.GetSingleValueOrDefault(tag, string.Empty);
    }

    private static CFindResponse WithStatus(CFindResponse response, ushort status)
    {
        response.Command.AddOrUpdate(Tag.Status, status);
        return response;
    }
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>L'handler MPPS N-CREATE e N-SET - C#</h3>
 <pre><code class="cs">// Receives the performed procedure steps the modalities report
public sealed class MppsHandler : INCreateRequestHandler, INSetRequestHandler
{
    private readonly ConcurrentDictionary&lt;string, Dataset&gt; _steps = new();

    // N-CREATE: the exam has started
    public ValueTask&lt;NCreateResponse&gt; Handle(NCreateRequest request, CancellationToken cancellationToken)
    {
        bool created = _steps.TryAdd(request.AffectedSopInstanceUid.Code, request.Attributes ?? new Dataset());

        NCreateResponse response = new(null);
        response.Command.AddOrUpdate(Tag.Status, created ? (ushort)0x0000 : (ushort)0x0111); // 0x0111: duplicate instance
        return ValueTask.FromResult(response);
    }

    // N-SET: the exam is COMPLETED or DISCONTINUED
    public ValueTask&lt;NSetResponse&gt; Handle(NSetRequest request, CancellationToken cancellationToken)
    {
        NSetResponse response = new(null);
        if (!_steps.TryGetValue(request.RequestedSopInstanceUid.Code, out Dataset? step))
        {
            response.Command.AddOrUpdate(Tag.Status, (ushort)0x0112); // 0x0112: no such instance
            return ValueTask.FromResult(response);
        }

        step.AddOrUpdateRange(request.ModificationList);
        Console.WriteLine($"{step.GetSingleValueOrDefault(Tag.PatientID, string.Empty)}: " +
                          $"{step.GetSingleValueOrDefault(Tag.PerformedProcedureStepStatus, string.Empty)}");

        response.Command.AddOrUpdate(Tag.Status, (ushort)0x0000);
        return ValueTask.FromResult(response);
    }
}</code></pre>
</div>

<p>Qui i passaggi sono mantenuti in memoria e lo stato viene stampato. Un vero MPPS SCP memorizza il passaggio, rifiuta modifiche a un passaggio già COMPLETED o DISCONTINUED, e passa il risultato al RIS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Il tuo server worklist su DICOMweb">}}

<p>Su DICOMweb la libreria gestisce più operazioni di server. Per una ricerca worklist applica le regole di matching DICOM, includendo caratteri jolly e intervalli di date, e pagina i risultati. Per MPPS verifica gli attributi richiesti e le transizioni di stato. Le tue classi forniscono i dati.</p>

<div class="codeblock" id="code">
 <h3>Program.cs di un server worklist e MPPS DICOMweb - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// The scheduled exams and the reported procedure steps
builder.Services.AddSingleton&lt;IDicomWebModalityWorklistProvider&gt;(new FolderWorklist("worklist"));
builder.Services.AddSingleton&lt;IDicomWebMppsProvider, MppsStore&gt;();
builder.Services.AddDicomWeb()
    .AddStandardModalityWorklistService()
    .AddStandardModalityPerformedProcedureStepService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Archiviazione worklist e MPPS - C#</h3>
 <pre><code class="cs">// The scheduled exams for DICOMweb searches; the library applies the matching rules and the paging
public sealed class FolderWorklist(string folder) : IDicomWebModalityWorklistProvider
{
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Directory.EnumerateFiles(folder, "*.wl").Order(StringComparer.Ordinal))
            yield return (await DicomFile.OpenAsync(path, cancellationToken: cancellationToken)).Dataset;
    }
}

// The performed procedure steps; the library checks the attributes and the state changes
public sealed class MppsStore : IDicomWebMppsProvider
{
    private readonly ConcurrentDictionary&lt;string, DicomWebVersionedDataset&gt; _steps = new();

    public ValueTask&lt;DicomWebVersionedDataset?&gt; GetAsync(string mppsUid, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_steps.TryGetValue(mppsUid, out DicomWebVersionedDataset? step) ? Copy(step) : null);

    public ValueTask&lt;bool&gt; TryCreateAsync(string mppsUid, Dataset procedureStep, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_steps.TryAdd(mppsUid, NewRevision(procedureStep)));

    // Replaces the step only if nobody changed it since it was read
    public ValueTask&lt;bool&gt; TryReplaceAsync(string mppsUid, string expectedRevision, Dataset procedureStep,
        CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_steps.TryGetValue(mppsUid, out DicomWebVersionedDataset? current)
                             &amp;&amp; current.Revision == expectedRevision
                             &amp;&amp; _steps.TryUpdate(mppsUid, NewRevision(procedureStep), current));

    private static DicomWebVersionedDataset NewRevision(Dataset dataset) =&gt;
        new() { Dataset = new Dataset(dataset), Revision = Guid.NewGuid().ToString("N") };

    private static DicomWebVersionedDataset Copy(DicomWebVersionedDataset step) =&gt;
        new() { Dataset = new Dataset(step.Dataset), Revision = step.Revision };
}</code></pre>
</div>

<p>Il server richiede il pacchetto <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. La <a href="/medical/net/dicomweb-server/">pagina del server DICOMweb</a> mostra come aggiungere i servizi Studies alla stessa applicazione, e la <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">guida ai provider di workflow</a> spiega il contratto di revisione di <code>TryReplaceAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklist oltre la modalità">}}

<p>MWL e MPPS collegano una modalità a un RIS. Per lavori che si spostano tra altri sistemi, come l'elaborazione AI, le assegnazioni di lettura o il post‑processing, DICOM definisce lo Unified Procedure Step. La <a href="/medical/net/ups-rs/">pagina UPS‑RS</a> mostra come creare, rivendicare e completare tali workitem con la stessa libreria.</p>

<p>Pagine correlate: <a href="/medical/net/dicom-networking/">DICOM networking</a> per C-ECHO, C-STORE e gli altri servizi DIMSE, <a href="/medical/net/dicomweb-client/">client DICOMweb</a> e <a href="/medical/net/dicomweb-server/">server DICOMweb</a> per gli studi via HTTP, e <a href="/medical/net/dicom-tags/">DICOM tags</a> per leggere cosa contiene un elemento worklist.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Risorse di apprendimento" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentazione" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guida per sviluppatori" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="Riferimenti API" href="https://reference.aspose.com/medical/net/" >}}
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
