---
title: UPS-RS i C# .NET - DICOMweb Unified Procedure Step Workitems | Aspose.Medical
weight: 9400

description: Skapa, ta anspråk på och slutföra DICOM UPS workitems från C#, ta emot tillståndsändringar via WebSocket, eller kör din egen UPS‑RS‑server på ASP.NET Core med Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS för .NET C#" h2="Distribuera bildbehandlingsarbete mellan system med DICOM Unified Procedure Step: skapa workitems, låt arbetare ta anspråk på och slutföra dem, och följ varje förändring, som klient eller som server." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="En arbetskö som talar DICOM">}}

<p>En AI‑modell måste bearbeta varje ny bröst‑CT. En teleradiologitjänst tilldelar studier till läsare. ett 3D‑laboratorium tar emot efterbehandlingsorder. Var och en av dessa är en kö av uppgifter mellan system, och DICOM definierar en sådan: Unified Procedure Step (UPS), som erbjuds via HTTP av UPS‑RS‑tjänsten i DICOMweb.</p>

<p>Ett UPS‑workitem anger vad som ska göras, för vilken patient och med vilken indata. Det är SCHEDULED när det skapas. En arbetare tar anspråk på det och gör det IN PROGRESS, avslutar det sedan som COMPLETED, eller så avslutas det som CANCELED. Andra system prenumererar och tar emot varje tillståndsändring via en WebSocket, så ingen behöver polla.</p>

<p><strong>Aspose.Medical for .NET</strong> implementerar båda sidor i hanterad C#: en typad klient för vilken UPS‑RS‑server som helst, och serverdelen för ASP.NET Core, där biblioteket kontrollerar UPS‑reglerna och din kod lagrar workitems.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Schemalägg ett workitem">}}

<p>Systemet som vill ha arbetet utfört skapar workitemet. Dess SOP Instance UID identifierar det därefter; etiketten, worklist‑etiketten och prioriteten talar om för arbetare vad det är.</p>

<div class="codeblock" id="code">
 <h3>Skapa ett UPS‑workitem - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Ta anspråk på det, utför arbetet, slutför det">}}

<p>En arbetare söker i worklistan efter vad den kan göra och tar anspråk på ett workitem genom att flytta det till IN PROGRESS med ett transaktions‑UID som den har genererat. Från det ögonblicket kan bara ägaren av detta UID uppdatera eller slutföra workitemet, så två arbetare bearbetar aldrig samma uppgift.</p>

<div class="codeblock" id="code">
 <h3>Hitta, ta anspråk på och slutför schemalagda workitems - C#</h3>
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

<p>COMPLETED kräver en redogörelse för arbetet: vem som utförde det, vad som utfördes, start‑ och sluttid samt resultatet. Arbetaren lagrar det med en uppdatering innan tillståndet ändras.</p>

<div class="codeblock" id="code">
 <h3>Data för utförd procedur - C#</h3>
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

<p>En server byggd med Aspose.Medical upprätthåller dessa regler. En andra arbetare som försöker ta anspråk på ett workitem i arbete får HTTP 409, och detsamma gäller för ett tillståndsbyte på ett slutfört. En COMPLETED‑begäran utan data för den utförda proceduren får HTTP 400. Klienten rapporterar sådana svar som ett <code>DicomWebClientException</code> med HTTP‑svaret.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Följ worklistan via WebSocket">}}

<p>En instrumentpanel, systemet som beställde arbetet eller en arbetare som väntar på nya uppgifter prenumererar med sitt AE‑namn och öppnar en notifikationsanslutning. Varje händelserapport anländer som ett dataset med workitem‑UID och dess nya tillstånd.</p>

<div class="codeblock" id="code">
 <h3>Ta emot UPS‑händelserapporter - C#</h3>
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

<p>Förutom hela worklistan kan en klient prenumerera på ett enskilt workitem med <code>SubscribeWorkitemAsync</code> eller på en filtrerad del av worklistan med <code>SubscribeFilteredWorklistAsync</code>, om servern stödjer filter. Att avbryta en mottagning stänger anslutningen; öppna en ny med <code>ConnectNotificationsAsync</code> för att fortsätta.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Din egen UPS‑RS‑server">}}

<p>Paketet <code>Aspose.Medical.Dicom.Web.AspNetCore</code> lägger till UPS‑RS i en ASP.NET Core‑tjänst. Biblioteket hanterar varje begäran: det kontrollerar nyttjandet av ett nytt workitem, tillståndsövergångarna, arbetarnas transaktions‑UID och den data som COMPLETED kräver, samt tillämpar DICOM‑matchningsreglerna på sökningar. Din kod lagrar workitems.</p>

<div class="codeblock" id="code">
 <h3>Program.cs för en UPS‑RS‑server - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Lagring av workitems - C#</h3>
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

<p>Varje lagrat workitem har en revision. <code>TryReplaceAsync</code> ersätter det endast när revisionen fortfarande är den som biblioteket läste, så två begäranden som ändrar samma workitem samtidigt kan inte skriva över varandra; biblioteket försöker igen eller rapporterar en konflikt. Ersätt ordboken med din databas och behåll den kontrollen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Notifikationer på servern">}}

<p>För att skicka händelserapporter, registrera en <code>IDicomWebSubscriptionProvider</code> och anropa <code>app.UseWebSockets()</code> före <code>MapDicomWeb</code>. Leverantören behåller prenumerationerna och en kö av händelserapporter per prenumerant, och biblioteket ger den en händelserapport när ett workitem ändras. Leverantören informerar också biblioteket om vilket AE‑namn en autentiserad anropar har, eftersom händelserapporterna namnger systemet som ändrade tillståndet.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">Guiden för arbetsflödesleverantörer</a> innehåller en komplett in‑memory‑prenumerationsleverantör, och <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core‑guiden</a> täcker WebSocket‑inställningarna.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL och MPPS">}}

<p>UPS täcker vad den äldre Modality Worklist och MPPS gör mellan ett RIS och en modalitet, och lägger till arbetare som inte är modaliteter, med anspråk och notifikationer. Många modaliteter talar fortfarande bara MWL och MPPS över DIMSE; <a href="/medical/net/modality-worklist/">sidan för modality worklist</a> visar det arbetsflödet med samma bibliotek.</p>

<p>Relaterade sidor: <a href="/medical/net/dicomweb-client/">DICOMweb‑klient</a> för de andra DICOMweb‑tjänsterna, <a href="/medical/net/dicomweb-server/">DICOMweb‑server</a> för QIDO‑RS, WADO‑RS och STOW‑RS på samma server, och <a href="/medical/net/dicom-networking/">DICOM‑nätverk</a> för DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Utvecklarguide" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="API‑referenser" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktstöd" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betald support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blogg" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Varför Aspose.Medical för .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundlista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Framgångshistorier" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
