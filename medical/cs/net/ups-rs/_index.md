---
title: UPS-RS v C# .NET - DICOMweb Unified Procedure Step Workitems | Aspose.Medical
weight: 9400

description: Vytvářejte, přebírejte a dokončujte DICOM UPS workitems pomocí C#, přijímejte změny stavů přes WebSocket nebo provozujte vlastní server UPS-RS na ASP.NET Core s Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS pro .NET C#" h2="Rozdělujte zobrazovací úlohy mezi systémy pomocí DICOM Unified Procedure Step: vytvářejte workitems, nechte pracovníky je přebírat a dokončovat a sledujte každou změnu jako klient nebo jako server." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Fronta úloh, která mluví DICOM">}}

<p>AI model musí zpracovat každé nové CT hrudníku. Služba teleradiologie přiřazuje studie čtenářům. 3D laboratoř přijímá objednávky na postprocessing. Každý z těchto případů je frontou úloh mezi systémy a DICOM pro ni definuje: Unified Procedure Step (UPS), nabízený přes HTTP službou UPS-RS v DICOMweb.</p>

<p>UPS workitem určuje, co se má provést, pro kterého pacienta a s jakým vstupem. Po vytvoření je ve stavu SCHEDULED. Pracovník si jej přebírá a mění ho na IN PROGRESS, poté jej dokončí jako COMPLETED, nebo jej ukončí jako CANCELED. Ostatní systémy se přihlašují a přijímají každou změnu stavu přes WebSocket, takže nikdo nemusí provádět polling.</p>

<p><strong>Aspose.Medical pro .NET</strong> implementuje obě strany v řízeném C#: typovaný klient pro libovolný UPS-RS server a serverovou část pro ASP.NET Core, kde knihovna kontroluje pravidla UPS a váš kód ukládá workitems.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Naplánovat workitem">}}

<p>Systém, který požaduje provedení práce, vytvoří workitem. Jeho SOP Instance UID jej od té chvíle identifikuje; štítek, štítek pracovního seznamu a priorita informují pracovníky o jeho povaze.</p>

<div class="codeblock" id="code">
 <h3>Vytvořit UPS workitem - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Převzít jej, vykonat práci, dokončit jej">}}

<p>Pracovník prohledá worklist, aby našel, co může udělat, a přebírá workitem přesunutím do IN PROGRESS s transakčním UID, které vygeneroval. Od té chvíle může workitem aktualizovat nebo dokončit jen vlastník tohoto UID, takže dva pracovníci nikdy nepracují na stejné úloze.</p>

<div class="codeblock" id="code">
 <h3>Najít, přebírat a dokončit naplánované workitems - C#</h3>
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

<p>COMPLETED vyžaduje záznam o práci: kdo ji provedl, co bylo provedeno, čas začátku a konce a výstup. Pracovník jej uloží při aktualizaci před změnou stavu.</p>

<div class="codeblock" id="code">
 <h3>Data o provedeném zákroku - C#</h3>
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

<p>Server postavený s Aspose.Medical vynucuje tato pravidla. Druhý pracovník, který se pokusí přebírat workitem ve stavu IN PROGRESS, získá HTTP 409, stejně tak změna stavu u dokončeného workitemu. Požadavek COMPLETED bez dat o provedeném zákroku vrátí HTTP 400. Klient hlásí takové odpovědi jako <code>DicomWebClientException</code> s HTTP odpovědí.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sledujte worklist přes WebSocket">}}

<p>Dashboard, systém, který zadal práci, nebo pracovník čekající na nové úkoly se přihlásí pomocí svého AE title a otevře notifikační spojení. Každá zpráva o události přijde jako dataset s UID workitemu a jeho novým stavem.</p>

<div class="codeblock" id="code">
 <h3>Přijímat UPS event reporty - C#</h3>
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

<p>Kromě celého worklistu se může klient přihlásit k jednomu workitemu pomocí <code>SubscribeWorkitemAsync</code> nebo k filtrované části worklistu pomocí <code>SubscribeFilteredWorklistAsync</code>, pokud server podporuje filtry. Zrušení příjmu uzavře spojení; otevřete nové pomocí <code>ConnectNotificationsAsync</code> pro pokračování.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vlastní server UPS-RS">}}

<p>Balíček <code>Aspose.Medical.Dicom.Web.AspNetCore</code> přidává UPS-RS do služby ASP.NET Core. Knihovna zpracovává každý požadavek: kontroluje payload nového workitemu, přechody stavů, transakční UID pracovníka a data, která COMPLETED vyžaduje, a aplikuje DICOM pravidla pro shodu při vyhledávání. Váš kód ukládá workitems.</p>

<div class="codeblock" id="code">
 <h3>Program.cs UPS-RS serveru - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Úložiště workitemů - C#</h3>
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

<p>Každý uložený workitem má revizi. <code>TryReplaceAsync</code> jej nahradí pouze pokud je revize stále ta, kterou knihovna načetla, takže dva požadavky měnící stejný workitem ve stejném okamžiku se nemohou navzájem přepsat; knihovna se pokusí znovu nebo nahlásí konflikt. Nahraďte slovník svou databází a zachovejte tuto kontrolu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Notifikace na serveru">}}

<p>Pro odesílání event reportů zaregistrujte <code>IDicomWebSubscriptionProvider</code> a zavolejte <code>app.UseWebSockets()</code> před <code>MapDicomWeb</code>. Provider udržuje předplatné a jednu frontu event reportů pro každého odběratele a knihovna mu předá event report kdykoli se workitem změní. Provider také informuje knihovnu, jaký AE title má autentizovaný volající, protože event reporty uvádějí systém, který změnil stav.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">Průvodce poskytovateli workflow</a> obsahuje kompletní paměťově založený subscription provider a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">průvodce ASP.NET Core</a> popisuje nastavení WebSocketů.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL a MPPS">}}

<p>UPS pokrývá to, co starší Modality Worklist a MPPS dělají mezi RIS a modalitou, a přidává pracovníky, kteří nejsou modality, s přebíráním a notifikacemi. Mnoho modalit stále komunikuje jen pomocí MWL a MPPS přes DIMSE; <a href="/medical/net/modality-worklist/">stránka modality worklist</a> ukazuje tento workflow se stejnou knihovnou.</p>

<p>Související stránky: <a href="/medical/net/dicomweb-client/">DICOMweb klient</a> pro ostatní DICOMweb služby, <a href="/medical/net/dicomweb-server/">DICOMweb server</a> pro QIDO-RS, WADO-RS a STOW-RS na stejném serveru a <a href="/medical/net/dicom-networking/">DICOM networking</a> pro DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Učební zdroje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Průvodce vývojáře" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="Reference API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Podpora produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Bezplatná podpora" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Placená podpora" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Proč Aspose.Medical pro .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Seznam zákazníků" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Úspěšné příběhy" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
