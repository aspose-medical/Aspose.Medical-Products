---
title: UPS-RS in C# .NET - DICOMweb Unified Procedure Step Werkitems | Aspose.Medical
weight: 9400

description: Maak, claim en voltooi DICOM UPS-werkitems vanuit C#, ontvang statuswijzigingen via WebSocket, of draai uw eigen UPS-RS-server op ASP.NET Core met Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS voor .NET C#" h2="Verdeel beeldvormingswerk tussen systemen met de DICOM Unified Procedure Step: maak werkitems aan, laat workers ze claimen en voltooien, en volg elke wijziging, als client of als server." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Een werkqueue die DICOM ondersteunt">}}

<p>Een AI‑model moet elke nieuwe chest‑CT verwerken. Een teleradiologie‑service wijst studies toe aan lezers. Een 3D‑lab ontvangt post‑processing‑opdrachten. Elk hiervan is een wachtrij van taken tussen systemen, en DICOM definieert er één voor: de Unified Procedure Step (UPS), aangeboden via HTTP door de UPS‑RS‑service van DICOMweb.</p>

<p>Een UPS‑werk-item geeft aan wat te doen is, voor welke patiënt en met welke invoer. Het is SCHEDULED wanneer het wordt aangemaakt. Een worker claimt het en zet het IN PROGRESS, voltooit het vervolgens als COMPLETED, of het eindigt als CANCELED. Andere systemen abonneren zich en ontvangen elke statuswijziging via een WebSocket, zodat niemand hoeft te pollen.</p>

<p><strong>Aspose.Medical voor .NET</strong> implementeert beide kanten in beheerde C#: een getypeerde client voor elke UPS‑RS‑server, en de serverkant voor ASP.NET Core, waar de bibliotheek de UPS‑regels controleert en uw code de werkitems opslaat.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Plan een werkitem">}}

<p>Het systeem dat het werk wil laten uitvoeren maakt het werkitem aan. Zijn SOP Instance UID identificeert het voortaan; het label, het worklist‑label en de prioriteit vertellen workers wat het is.</p>

<div class="codeblock" id="code">
 <h3>Maak een UPS-werkitem - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Claim het, voer het werk uit, voltooi het">}}

<p>Een worker doorzoekt de worklist naar taken die hij kan uitvoeren en claimt een werkitem door het naar IN PROGRESS te verplaatsen met een door hem gegenereerde transaction UID. Vanaf dat moment kan alleen de houder van deze UID het werkitem bijwerken of voltooien, zodat twee workers nooit dezelfde taak verwerken.</p>

<div class="codeblock" id="code">
 <h3>Zoek, claim en voltooi geplande werkitems - C#</h3>
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

<p>COMPLETED vereist een verslag van het werk: wie het heeft uitgevoerd, wat er is uitgevoerd, de start‑ en eindtijd, en de output. De worker slaat dit op met een update voordat de status verandert.</p>

<div class="codeblock" id="code">
 <h3>De uitgevoerde procedure‑gegevens - C#</h3>
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

<p>Een server gebouwd met Aspose.Medical handhaaft deze regels. Een tweede worker die probeert een werkitem in uitvoering te claimen krijgt HTTP 409, en hetzelfde gebeurt bij een statuswijziging van een voltooid item. Een COMPLETED‑verzoek zonder de uitgevoerde procedure‑gegevens krijgt HTTP 400. De client meldt dergelijke antwoorden als een <code>DicomWebClientException</code> met de HTTP‑respons.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Volg de worklist via WebSocket">}}

<p>Een dashboard, het systeem dat het werk bestelde of een worker die wacht op nieuwe taken, abonneert zich met zijn AE‑title en opent een notificatieverbinding. Elk gebeurtenisrapport komt binnen als een dataset met de werkitem‑UID en de nieuwe status.</p>

<div class="codeblock" id="code">
 <h3>Ontvang UPS‑gebeurtenisrapporten - C#</h3>
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

<p>Naast de volledige worklist kan een client zich abonneren op één werkitem met <code>SubscribeWorkitemAsync</code> of op een gefilterd deel van de worklist met <code>SubscribeFilteredWorklistAsync</code>, indien de server filters ondersteunt. Het annuleren van een ontvangst sluit de verbinding; open een nieuwe met <code>ConnectNotificationsAsync</code> om door te gaan.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Uw eigen UPS-RS-server">}}

<p>Het <code>Aspose.Medical.Dicom.Web.AspNetCore</code>-pakket voegt UPS‑RS toe aan een ASP.NET Core‑service. De bibliotheek verwerkt elk verzoek: hij controleert de payload van een nieuw werkitem, de statusovergangen, de transaction UID van de worker en de gegevens die COMPLETED vereist, en hij past de DICOM‑matchingregels toe op zoekopdrachten. Uw code slaat de werkitems op.</p>

<div class="codeblock" id="code">
 <h3>Program.cs van een UPS‑RS‑server - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>De werkitem‑opslag - C#</h3>
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

<p>Elke opgeslagen werkitem heeft een revisie. <code>TryReplaceAsync</code> vervangt deze alleen wanneer de revisie nog dezelfde is als die de bibliotheek heeft gelezen, zodat twee verzoeken die tegelijkertijd hetzelfde werkitem wijzigen elkaar niet kunnen overschrijven; de bibliotheek probeert het opnieuw of rapporteert een conflict. Vervang de dictionary door uw database en behoud die controle.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Meldingen op de server">}}

<p>Om gebeurtenisrapporten te verzenden, registreer een <code>IDicomWebSubscriptionProvider</code> en roep <code>app.UseWebSockets()</code> aan vóór <code>MapDicomWeb</code>. De provider bewaart de abonnementen en één wachtrij met gebeurtenisrapporten per abonnee, en de bibliotheek geeft het een gebeurtenisrapport wanneer een werkitem verandert. De provider vertelt de bibliotheek ook welke AE‑title een geauthenticeerde aanroeper heeft, omdat de gebeurtenisrapporten het systeem benoemen dat de status heeft gewijzigd.</p>

<p>De <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">workflow‑providers‑gids</a> bevat een volledige in‑memory abonnementprovider, en de <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core‑gids</a> behandelt de WebSocket‑instellingen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL en MPPS">}}

<p>UPS dekt wat de oudere Modality Worklist en MPPS doen tussen een RIS en een modality, en voegt workers toe die geen modalities zijn, met claimen en meldingen. Veel modalities communiceren nog steeds alleen via MWL en MPPS over DIMSE; de <a href="/medical/net/modality-worklist/">modality worklist‑pagina</a> toont die workflow met dezelfde bibliotheek.</p>

<p>Gerelateerde pagina's: <a href="/medical/net/dicomweb-client/">DICOMweb‑client</a> voor de andere DICOMweb‑services, <a href="/medical/net/dicomweb-server/">DICOMweb‑server</a> voor QIDO‑RS, WADO‑RS en STOW‑RS op dezelfde server, en <a href="/medical/net/dicom-networking/">DICOM‑netwerken</a> voor DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leermiddelen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Ontwikkelaarsgids" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="API‑referenties" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Productondersteuning" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis ondersteuning" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betaalde ondersteuning" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Waarom Aspose.Medical voor .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Klantlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
