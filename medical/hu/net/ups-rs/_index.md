---
title: UPS-RS C# .NET-ben - DICOMweb Unified Procedure Step Workitems | Aspose.Medical
weight: 9400

description: Hozzon létre, igényeljen és fejezzen be DICOM UPS workitemeket C#-ból, fogadja az állapotváltozásokat WebSocket-en keresztül, vagy futtassa saját UPS-RS szerverét ASP.NET Core-on az Aspose.Medical segítségével.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS .NET C#-hoz" h2="Ossza szét a képalkotási feladatokat a rendszerek között a DICOM Unified Procedure Step segítségével: hozza létre a workitemeket, engedje, hogy a dolgozók igényeljék és befejezzék őket, és kövesse nyomon minden változást, akár kliensként, akár szerverként." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Egy DICOM-ot beszélő munkasor">}}

<p>Egy MI-modellnek minden új mellkas CT-t fel kell dolgoznia. Egy teleradiológiai szolgáltatás feladatokat oszt ki az olvasók között. Egy 3D labor posztprocesszálási megbízásokat kap. Ezek mind feladat-sorok a rendszerek között, és a DICOM definiál egy ilyet: az Unified Procedure Step (UPS), amelyet a DICOMweb UPS-RS szolgáltatása HTTP-n keresztül kínál.</p>

<p>Egy UPS workitem megadja, mit kell tenni, melyik betegnek és milyen bemenettel. Létrehozáskor SCHEDULED (Ütemezett). Egy dolgozó igényli és IN PROGRESS (Folyamatban) állapotba helyezi, majd COMPLETED (Befejezett) állapotra vált, vagy CANCELED (Megszakított). Más rendszerek feliratkoznak, és minden állapotváltozást WebSocket-en kapnak, így senkinek sem kell lekérdezést végeznie.</p>

<p><strong>Aspose.Medical for .NET</strong> mindkét oldalt megvalósítja menedzselt C#-ban: egy típusos klienst bármely UPS-RS szerverhez, és a szerveroldalt az ASP.NET Core-hoz, ahol a könyvtár ellenőrzi az UPS szabályokat, és a kódja tárolja a workitemeket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Workitem ütemezése">}}

<p>Az a rendszer, amelyik meg akarja a munkát, létrehozza a workitemet. SOP Instance UID-je azóta azonosítja; a címke, a worklist címke és a prioritás megmondja a dolgozóknak, mi az.</p>

<div class="codeblock" id="code">
 <h3>UPS workitem létrehozása - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Igényelje, végezze el a feladatot, fejezze be">}}

<p>Egy dolgozó átnézi a worklistet, hogy mit tud elvégezni, és igényel egy workitemet úgy, hogy IN PROGRESS állapotba helyezi egy általa generált tranzakció UID-vel. Ettől a pillanattól csak ennek az UID-nek a birtokosa frissítheti vagy befejezheti a workitemet, így két dolgozó soha nem dolgozik ugyanazon feladaton.</p>

<div class="codeblock" id="code">
 <h3>Ütemezett workitemek keresése, igénylése és befejezése - C#</h3>
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

<p>A COMPLETED állapothoz szükség van a munka nyilvántartására: ki végezte, mit végzett, a kezdő és befejező idő, valamint a kimenet. A dolgozó frissítésként tárolja ezt, mielőtt megváltoztatná az állapotot.</p>

<div class="codeblock" id="code">
 <h3>A végrehajtott eljárás adatai - C#</h3>
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

<p>Egy Aspose.Medical-al felépített szerver érvényesíti ezeket a szabályokat. Egy másik dolgozó, aki egy folyamatban lévő workitemet próbál igényelni, HTTP 409-et kap, ugyanígy egy állapotváltozás egy befejezett workitemen. A COMPLETED kérés, ha hiányzik a végrehajtott eljárás adat, HTTP 400-at eredményez. A kliens ezeket a válaszokat <code>DicomWebClientException</code>-ként jelenti a HTTP válasszal.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kövesse a worklistet WebSocket-en keresztül">}}

<p>Egy irányítópult, a munkát megrendelő rendszer vagy egy új feladatokra váró dolgozó feliratkozik saját AE címével, és nyit egy értesítési kapcsolatot. Minden eseményjelentés adathalmazként érkezik a workitem UID-vel és az új állapotával.</p>

<div class="codeblock" id="code">
 <h3>UPS eseményjelentések fogadása - C#</h3>
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

<p>A teljes worklist mellett a kliens feliratkozhat egy workitemre a <code>SubscribeWorkitemAsync</code> segítségével, vagy a worklist szűrt részére a <code>SubscribeFilteredWorklistAsync</code> használatával, ha a szerver támogatja a szűrőket. A fogadás megszakítása lezárja a kapcsolatot; egy új nyitása <code>ConnectNotificationsAsync</code>-vel folytatható.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Saját UPS-RS szerver">}}

<p>A <code>Aspose.Medical.Dicom.Web.AspNetCore</code> csomag UPS-RS-t ad az ASP.NET Core szolgáltatáshoz. A könyvtár minden kérést kezel: ellenőrzi az új workitem terhelését, az állapotátmeneteket, a dolgozó tranzakció UID-jét és a COMPLETED-nek igényelt adatokat, valamint alkalmazza a DICOM egyezési szabályokat a keresésekre. A kódja tárolja a workitemeket.</p>

<div class="codeblock" id="code">
 <h3>UPS-RS szerver Program.cs - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>A workitem tárolása - C#</h3>
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

<p>Minden tárolt workitemnek van revíziója. A <code>TryReplaceAsync</code> csak akkor cseréli le, ha a revízió még mindig az, amit a könyvtár beolvasott, így két kérés, amely egyidejűleg ugyanazt a workitemet módosítja, nem írhatja felül egymást; a könyvtár újrapróbálja vagy konfliktusról számol be. Cserélje le a szótárt adatbázisra, és tartsa meg ezt az ellenőrzést.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Értesítések a szerveren">}}

<p>Eseményjelentések küldéséhez regisztráljon egy <code>IDicomWebSubscriptionProvider</code>-t, és hívja meg a <code>app.UseWebSockets()</code>-t a <code>MapDicomWeb</code> előtt. A szolgáltató tárolja az előfizetéseket és egy eseményjelentési sort feliratkozónként, a könyvtár pedig eseményjelentést ad neki, amikor egy workitem változik. A szolgáltató továbbá közli a könyvtárral, melyik AE című hitelesített hívóval rendelkezik, mivel az eseményjelentések megnevezik a rendszert, amely megváltoztatta az állapotot.</p>

<p>A <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">workflow providers útmutató</a> teljes memóriabeli előfizető szolgáltatót tartalmaz, és az <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core útmutató</a> a WebSocket beállításokat tárgyalja.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL és MPPS">}}

<p>Az UPS lefedi azt, amit a régebbi Modality Worklist és MPPS végez egy RIS és egy modality között, és hozzáad olyan dolgozókat, akik nem modality-k, igénylést és értesítéseket. Sok modality még mindig csak MWL-t és MPPS-t támogat DIMSE-n keresztül; a <a href="/medical/net/modality-worklist/">modality worklist oldal</a> ugyanazzal a könyvtárral mutatja ezt a munkafolyamatot.</p>

<p>Kapcsolódó oldalak: <a href="/medical/net/dicomweb-client/">DICOMweb kliens</a> a többi DICOMweb szolgáltatáshoz, <a href="/medical/net/dicomweb-server/">DICOMweb szerver</a> a QIDO-RS, WADO-RS és STOW-RS-hez ugyanazon a szerveren, valamint <a href="/medical/net/dicom-networking/">DICOM hálózat</a> a DIMSE-hez.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Fejlesztői útmutató" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetős támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyféllista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
