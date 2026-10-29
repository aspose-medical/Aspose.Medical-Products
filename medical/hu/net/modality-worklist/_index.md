---
title: Modality Worklist és MPPS C# .NET-ben - DICOM C-FIND, N-CREATE, N-SET | Aspose.Medical
weight: 9300

description: Kérdezze le a DICOM modality worklist-et C-FIND használatával, és jelentsen MPPS-t C#‑ból, vagy üzemeltesse saját worklist szerverét. DIMSE és DICOMweb egyetlen .NET könyvtárban.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Modality Worklist és MPPS .NET C#-hoz" h2="Szerezze meg a nap vizsgálatait a RIS‑ből, jelentse mindegyiket indítottként és befejezettként, vagy maga legyen a worklist szerver, DIMSE vagy DICOMweb használatával." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="A RIS ütemtervtől a végrehajtott vizsgálatig">}}

<p>Mielőtt a szkennelés megkezdődik, a technológusnak nem kell beírnia a beteg nevét, betegazonosítóját és a hozzáférési számot a modality‑ba. A RIS már ütemezte a vizsgálatot, és a modality lekéri azt: egy Modality Worklist lekérdezés visszaadja a tervezett eljáráslépéseket ehhez a szkennerhez és ehhez a naphoz. A modality a beteg és a rendelési adatokat átmásolja a képekbe, így a vizsgálat már a PACS‑ba a rendeléshez illeszkedve érkezik.</p>

<p>A vizsgálat során a modality MPPS‑sel (Modality Performed Procedure Step) jelenti vissza: IN PROGRESS, amikor a szkennelés elindul, COMPLETED vagy DISCONTINUED, amikor befejeződik, a létrehozott sorozatokkal együtt. A RIS ezeket a jelentéseket használja az ütemezés frissítésére, a radiológus tájékoztatására, hogy a képek érkeznek, valamint a számlázáshoz.</p>

<p><strong>Aspose.Medical for .NET</strong> mindkét oldalát lefedi ennek a munkafolyamatnak kezelt C#‑ban: a lekérdező és jelentő modality‑t, valamint a válaszadó worklist szervert. Mindkettő a klasszikus DICOM hálózati protokoll, a DIMSE, és a DICOMweb felett működik.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklist lekérdezése C-FIND használatával">}}

<p>A worklist lekérdezés egy C-FIND kérés a Modality Worklist információs modellben. <code>CFindRequest.CreateWorklistQuery</code> a szokásos visszatérő kulcsokkal építi fel: a beteg demográfiai adatai, a hozzáférési szám, a kért eljárás és a tervezett eljáráslépés. A modality által beállított szűrők a saját AE címe, a modality típusa és a dátum, mind a tervezett eljáráslépésen belül.</p>

<div class="codeblock" id="code">
 <h3>A CT szkenner mai vizsgálatainak lekérése - C#</h3>
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

<p>Minden egyezés egy adatkészlet, amely felső szinten a beteg és a rendelés attribútumait, valamint egy sorozatban a tervezett eljáráslépést tartalmazza. További visszatérő kulcsok kéréséhez adja hozzá őket a <code>query.Identifier</code> elemhez üres értékkel, mielőtt a kérés sorba kerülne.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vizsgálat jelentése MPPS-szel">}}

<p>Az MPPS két normalizált DIMSE művelet a Modality Performed Procedure Step SOP osztályon: N-CREATE, amikor a vizsgálat elindul, és N-SET, amikor befejeződik. A modality kiválasztja a végrehajtott lépés UID‑ját, és mindkét művelethez felhasználja. Az N-CREATE attribútumok megadják, hogy kit vizsgáltak és mely tervezett lépés volt végrehajtva; ezek többsége közvetlenül a worklist elemből származik.</p>

<div class="codeblock" id="code">
 <h3>A worklist elem N-CREATE attribútumai - C#</h3>
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
 <h3>A lépést befejező N-SET attribútumok - C#</h3>
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
 <h3>Küldje IN PROGRESS, majd COMPLETED - C#</h3>
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

<p>Egy korán leálló vizsgálat DISCONTINUED állapottal fejeződik be a COMPLETED helyett, és az N-SET ilyenkor a Performed Procedure Step Discontinuation Reason Code Sequence‑ben tartalmazza az okot.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ugyanaz a munkafolyamat DICOMweb-en keresztül">}}

<p>Az újabb RIS és felhőplatformok a worklist-et és az MPPS-t HTTP-n keresztül kínálják. Az Aspose.Medical <a href="/medical/net/dicomweb-client/">DICOMweb kliens</a> a tervezett eljáráslépéseket keresve jelenti a végrehajtott lépést ugyanazokkal az adatkészletekkel, mint fent. A tervezett eljáráslépésen belüli kulcsot útvonalként írják, például <code>ScheduledProcedureStepSequence.Modality</code>.</p>

<div class="codeblock" id="code">
 <h3>Worklist lekérdezése és MPPS jelentése DICOMweb-en keresztül - C#</h3>
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

<p>A kliens a saját <code>HttpClient</code> példányán fut, így az autentikáció és a TLS ott van beállítva.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Saját worklist szerver DIMSE-en keresztül">}}

<p>Egy worklist brókernek, egy kis RIS-nek vagy egy modality integrációs tesztrendszernek kell válaszolnia a másik félnek. Regisztráljon egy C-FIND kezelőt a worklisthez, valamint N-CREATE és N-SET kezelőket az MPPS-hez, és az alkalmazás worklist SCP‑vé és MPPS SCP‑vé válik.</p>

<div class="codeblock" id="code">
 <h3>Worklist és MPPS szerver indítása - C#</h3>
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

<p>Ez a kezelő a tervezett vizsgálatokat <code>.wl</code> fájlokból olvassa, fájlonként egy adatkészletet, és összehasonlítja a modality által általában küldött három kulcsot. DIMSE-n keresztül a kezelő végzi a megfeleltetést: helyettesítő karakterek, dátumtartományok és a visszatérő kulcsok kiválasztása ebben van megadva.</p>

<div class="codeblock" id="code">
 <h3>A worklist C-FIND kezelő - C#</h3>
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
 <h3>Az MPPS N-CREATE és N-SET kezelő - C#</h3>
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

<p>Itt a lépések memóriában vannak tárolva és az állapot kiírásra kerül. Egy valós MPPS SCP tárolja a lépést, megtagadja a már COMPLETED vagy DISCONTINUED állapotú lépés módosítását, és továbbítja az eredményt a RIS-nek.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Saját worklist szerver DICOMweb-en keresztül">}}

<p>DICOMweb-en a könyvtár több szerver feladatot ellát. Worklist kereséskor alkalmazza a DICOM egyezési szabályokat, a helyettesítő karaktereket és a dátumtartományokat, és lapozza az eredményeket. MPPS esetén ellenőrzi a szükséges attribútumokat és állapotátmeneteket. Az Ön osztályai biztosítják az adatokat.</p>

<div class="codeblock" id="code">
 <h3>A DICOMweb worklist és MPPS szerver Program.cs fájlja - C#</h3>
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
 <h3>A worklist és MPPS tároló - C#</h3>
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

<p>A szervernek szüksége van a <code>Aspose.Medical.Dicom.Web.AspNetCore</code> csomagra. A <a href="/medical/net/dicomweb-server/">DICOMweb szerver oldal</a> bemutatja, hogyan lehet a Studies szolgáltatásokat ugyanabba az alkalmazásba hozzáadni, és a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">workflow providers útmutató</a> elmagyarázza a <code>TryReplaceAsync</code> felülvizsgálati szerződését.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklist-ek a modality-n kívül">}}

<p>Az MWL és az MPPS a modality-t egy RIS-szel kötik össze. Olyan munkák esetén, amelyek más rendszerek között mozognak, például AI feldolgozás, olvasási feladatok vagy utófeldolgozás, a DICOM definiálja az Unified Procedure Step-et. A <a href="/medical/net/ups-rs/">UPS-RS oldal</a> bemutatja, hogyan hozhatók létre, igényelhetők és fejezhetők be ilyen munkatelelemek ugyanazzal a könyvtárral.</p>

<p>Kapcsolódó oldalak: <a href="/medical/net/dicom-networking/">DICOM hálózat</a> a C-ECHO, C-STORE és egyéb DIMSE szolgáltatásokhoz, <a href="/medical/net/dicomweb-client/">DICOMweb kliens</a> és <a href="/medical/net/dicomweb-server/">DICOMweb szerver</a> tanulmányokhoz HTTP-n keresztül, valamint <a href="/medical/net/dicom-tags/">DICOM tagek</a> a worklist elem tartalmának megtekintéséhez.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Fejlesztői útmutató" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetett támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyféllista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikeres történetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
