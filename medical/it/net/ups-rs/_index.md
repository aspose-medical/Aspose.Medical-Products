---
title: UPS-RS in C# .NET - Elementi di lavoro DICOMweb Unified Procedure Step | Aspose.Medical
weight: 9400

description: Crea, richiedi e completa gli elementi di lavoro DICOM UPS da C#, ricevi i cambiamenti di stato tramite WebSocket, oppure esegui il tuo server UPS-RS su ASP.NET Core con Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS per .NET C#" h2="Distribuisci il lavoro di imaging tra i sistemi con il DICOM Unified Procedure Step: crea gli elementi di lavoro, consenti ai lavoratori di reclamarli e completarli, e monitora ogni modifica, sia come client che come server." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Una coda di lavoro che parla DICOM">}}

<p>Un modello di IA deve elaborare ogni nuova TC toracica. Un servizio di teleradiologia assegna gli studi ai lettori. Un laboratorio 3D riceve ordini di post‑elaborazione. Ognuno di questi è una coda di attività tra sistemi, e DICOM ne definisce una: il Unified Procedure Step (UPS), offerto tramite HTTP dal servizio UPS-RS di DICOMweb.</p>

<p>Un elemento di lavoro UPS indica cosa fare, per quale paziente e con quale input. È SCHEDULED quando viene creato. Un operatore lo reclama e lo porta a IN PROGRESS, poi lo conclude come COMPLETED, oppure termina come CANCELED. Altri sistemi si iscrivono e ricevono ogni cambiamento di stato tramite WebSocket, così nessuno deve effettuare polling.</p>

<p><strong>Aspose.Medical per .NET</strong> implementa entrambi i lati in C# gestito: un client tipizzato per qualsiasi server UPS-RS e il lato server per ASP.NET Core, dove la libreria verifica le regole UPS e il tuo codice memorizza gli elementi di lavoro.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Programma un elemento di lavoro">}}

<p>Il sistema che richiede il lavoro crea l'elemento di lavoro. Il suo SOP Instance UID lo identifica d'ora in poi; l'etichetta, l'etichetta della worklist e la priorità indicano ai lavoratori di cosa si tratta.</p>

<div class="codeblock" id="code">
 <h3>Crea un elemento di lavoro UPS - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// What to do, for which patient, and with which input
Dataset input = new();
input.AddOrUpdate(Tag.StudyInstanceUID, "1.2.826.0.1.3680043.10.1.100");

Dataset workitem = new();
workitem.AddOrUpdate(Tag.SOPInstanceUID, Uid.CreateDerivedFromUuid());
workitem.AddOrUpdate(Tag.ProcedureStepState, "SCHEDULED");
workitem.AddOrUpdate(Tag.TransactionUID, string.Empty);
workitem.AddOrUpdate(Tag.InputReadinessState, "READY");
workitem.AddOrUpdate(Tag.ProcedureStepLabel, "Lung nodule detection");
workitem.AddOrUpdate(Tag.WorklistLabel, "AI");
workitem.AddOrUpdate(Tag.ScheduledProcedureStepPriority, "HIGH");
workitem.AddOrUpdate(Tag.PatientName, "DOE^JANE");
workitem.AddOrUpdate(Tag.PatientID, "P001");
workitem.AddOrUpdate(new Sequence(Tag.InputInformationSequence, [input]));

DicomWebClientResult&lt;CreateWorkitemResult&gt; created = await client.Worklist.CreateAsync(workitem);
Console.WriteLine($"Scheduled {created.Value.WorkitemUid}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Richiedilo, esegui il lavoro, completalo">}}

<p>Un operatore ricerca nella worklist ciò che può fare e reclama un elemento di lavoro spostandolo a IN PROGRESS con un transaction UID che ha generato. Da quel momento solo il detentore di questo UID può aggiornare o terminare l'elemento di lavoro, così due operatori non elaborano mai lo stesso compito.</p>

<div class="codeblock" id="code">
 <h3>Trova, reclama e completa gli elementi di lavoro programmati - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// Scheduled workitems on the AI worklist
DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters =
    [
        new DicomWebQueryParameter("ProcedureStepState", "SCHEDULED"),
        new DicomWebQueryParameter("WorklistLabel", "AI")
    ]
};

List&lt;string&gt; scheduled = [];
await using (DicomWebClientStream&lt;Dataset&gt; found = await client.Worklist.SearchAsync(search))
{
    await foreach (Dataset item in found)
        scheduled.Add(item.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID));
}

foreach (string workitemUid in scheduled)
{
    // Claim the workitem; only the holder of this transaction UID can update or finish it
    string transactionUid = Uid.CreateDerivedFromUuid().Code;
    await client.Worklist.ChangeStateAsync(workitemUid, WorkitemState.InProgress, transactionUid);

    // ... run the job ...

    await client.Worklist.UpdateAsync(workitemUid, transactionUid, Performed());
    await client.Worklist.ChangeStateAsync(workitemUid, WorkitemState.Completed, transactionUid);
}</code></pre>
</div>

<p>COMPLETED richiede un registro del lavoro: chi lo ha eseguito, cosa è stato eseguito, l'ora di inizio e di fine, e il risultato. L'operatore lo memorizza con un aggiornamento prima che cambi lo stato.</p>

<div class="codeblock" id="code">
 <h3>I dati della procedura eseguita - C#</h3>
 <pre><code class="cs">// What COMPLETED requires: who did the work, what was done, when, and the output (here none)
public static Dataset Performed()
{
    Dataset performed = new();
    performed.AddOrUpdate(new Sequence(Tag.PerformedStationNameCodeSequence, [Code("AI-NODE-1", "99LOCAL", "AI node 1")]));
    performed.AddOrUpdate(new Sequence(Tag.PerformedWorkitemCodeSequence, [Code("NODULE", "99LOCAL", "Lung nodule detection")]));
    performed.AddOrUpdate(Tag.PerformedProcedureStepStartDateTime, DateTimeOffset.Now.AddMinutes(-2).ToString("yyyyMMddHHmmss"));
    performed.AddOrUpdate(Tag.PerformedProcedureStepEndDateTime, DateTimeOffset.Now.ToString("yyyyMMddHHmmss"));
    performed.AddOrUpdate(new Sequence(Tag.OutputInformationSequence, []));

    Dataset update = new();
    update.AddOrUpdate(new Sequence(Tag.UnifiedProcedureStepPerformedProcedureSequence, [performed]));
    return update;
}

private static Dataset Code(string value, string scheme, string meaning)
{
    Dataset code = new();
    code.AddOrUpdate(Tag.CodeValue, value);
    code.AddOrUpdate(Tag.CodingSchemeDesignator, scheme);
    code.AddOrUpdate(Tag.CodeMeaning, meaning);
    return code;
}</code></pre>
</div>

<p>Un server costruito con Aspose.Medical applica queste regole. Un secondo operatore che tenta di reclamare un elemento di lavoro in corso riceve HTTP 409, così come una modifica di stato su uno completato. Una richiesta COMPLETED senza i dati della procedura eseguita riceve HTTP 400. Il client segnala tali risposte come una <code>DicomWebClientException</code> con la risposta HTTP.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Segui la worklist via WebSocket">}}

<p>Una dashboard, il sistema che ha ordinato il lavoro o un operatore in attesa di nuovi compiti si iscrive con il proprio AE title e apre una connessione di notifica. Ogni report di evento arriva come dataset con l'UID dell'elemento di lavoro e il suo nuovo stato.</p>

<div class="codeblock" id="code">
 <h3>Ricevi i report di eventi UPS - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// Every change on the worklist, pushed over a WebSocket
await client.Worklist.SubscribeWorklistAsync("DASHBOARD");
await using DicomWebNotificationConnection connection = await client.ConnectNotificationsAsync("DASHBOARD");

while (await connection.ReceiveAsync(cancellationToken) is Dataset report)
{
    Console.WriteLine($"{report.GetSingleValueOrDefault(Tag.AffectedSOPInstanceUID, string.Empty)}: " +
                      $"{report.GetSingleValueOrDefault(Tag.ProcedureStepState, string.Empty)}");
}</code></pre>
</div>

<p>Oltre all'intera worklist, un client può iscriversi a un singolo elemento di lavoro con <code>SubscribeWorkitemAsync</code> o a una parte filtrata della worklist con <code>SubscribeFilteredWorklistAsync</code>, se il server supporta i filtri. Annullare una ricezione chiude la connessione; aprine una nuova con <code>ConnectNotificationsAsync</code> per continuare.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Il tuo server UPS-RS">}}

<p>Il pacchetto <code>Aspose.Medical.Dicom.Web.AspNetCore</code> aggiunge UPS-RS a un servizio ASP.NET Core. La libreria gestisce ogni richiesta: controlla il payload di un nuovo elemento di lavoro, le transizioni di stato, il transaction UID dell'operatore e i dati richiesti da COMPLETED, e applica le regole di corrispondenza DICOM alle ricerche. Il tuo codice memorizza gli elementi di lavoro.</p>

<div class="codeblock" id="code">
 <h3>Program.cs di un server UPS-RS - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>La memorizzazione degli elementi di lavoro - C#</h3>
 <pre><code class="cs">// Workitems with a revision each; the library applies the UPS state rules and the search matching
public sealed class WorkitemStore : IDicomWebWorkitemProvider
{
    private readonly ConcurrentDictionary&lt;string, DicomWebVersionedDataset&gt; _items = new();

    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string uid in _items.Keys.Order(StringComparer.Ordinal))
            if (_items.TryGetValue(uid, out DicomWebVersionedDataset? item))
                yield return new Dataset(item.Dataset);
        await Task.CompletedTask;
    }

    public ValueTask&lt;DicomWebVersionedDataset?&gt; GetAsync(string workitemUid, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryGetValue(workitemUid, out DicomWebVersionedDataset? item) ? Copy(item) : null);

    public ValueTask&lt;bool&gt; TryCreateAsync(string workitemUid, Dataset workitem, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryAdd(workitemUid, NewRevision(workitem)));

    // Replaces the workitem only if nobody changed it since it was read
    public ValueTask&lt;bool&gt; TryReplaceAsync(string workitemUid, string expectedRevision, Dataset workitem,
        CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryGetValue(workitemUid, out DicomWebVersionedDataset? current)
                             &amp;&amp; current.Revision == expectedRevision
                             &amp;&amp; _items.TryUpdate(workitemUid, NewRevision(workitem), current));

    private static DicomWebVersionedDataset NewRevision(Dataset dataset) =&gt;
        new() { Dataset = new Dataset(dataset), Revision = Guid.NewGuid().ToString("N") };

    private static DicomWebVersionedDataset Copy(DicomWebVersionedDataset item) =&gt;
        new() { Dataset = new Dataset(item.Dataset), Revision = item.Revision };
}</code></pre>
</div>

<p>Ogni elemento di lavoro memorizzato ha una revisione. <code>TryReplaceAsync</code> lo sostituisce solo quando la revisione è ancora quella letta dalla libreria, così due richieste che modificano lo stesso elemento allo stesso momento non possono sovrascriversi; la libreria riprova o segnala un conflitto. Sostituisci il dizionario con il tuo database e mantieni tale verifica.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Notifiche sul server">}}

<p>Per inviare report di eventi, registra un <code>IDicomWebSubscriptionProvider</code> e chiama <code>app.UseWebSockets()</code> prima di <code>MapDicomWeb</code>. Il provider mantiene le sottoscrizioni e una coda di report di eventi per ogni abbonato, e la libreria gli consegna un report di evento ogni volta che un elemento di lavoro cambia. Il provider informa anche la libreria del AE title di chi chiama autenticato, poiché i report di evento indicano il sistema che ha modificato lo stato.</p>

<p>La <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">guida ai provider di workflow</a> contiene un provider di sottoscrizione interamente in memoria, e la <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">guida ASP.NET Core</a> copre le impostazioni WebSocket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL e MPPS">}}

<p>UPS copre ciò che le precedenti Modality Worklist e MPPS fanno tra un RIS e una modalità, e aggiunge operatori che non sono modalità, consentendo reclami e notifiche. Molte modalità parlano ancora solo MWL e MPPS su DIMSE; la <a href="/medical/net/modality-worklist/">pagina della modality worklist</a> mostra quel flusso di lavoro con la stessa libreria.</p>

<p>Pagine correlate: <a href="/medical/net/dicomweb-client/">client DICOMweb</a> per gli altri servizi DICOMweb, <a href="/medical/net/dicomweb-server/">server DICOMweb</a> per QIDO-RS, WADO-RS e STOW-RS sullo stesso server, e <a href="/medical/net/dicom-networking/">rete DICOM</a> per DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Risorse di apprendimento" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentazione" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guida per sviluppatori" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="Riferimenti API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Assistenza prodotto" tabId="support" >}}
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
