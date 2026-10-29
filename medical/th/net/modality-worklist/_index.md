---
title: Modality Worklist และ MPPS ใน C# .NET - DICOM C-FIND, N-CREATE, N-SET | Aspose.Medical
weight: 9300

description: สืบค้น Modality Worklist ของ DICOM ด้วย C-FIND และรายงาน MPPS จาก C# หรือรันเซิร์ฟเวอร์ Worklist ของคุณเอง. DIMSE และ DICOMweb ในไลบรารี .NET เดียว
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Modality Worklist และ MPPS สำหรับ .NET C#" h2="ดึงการตรวจสอบของวันจาก RIS, รายงานแต่ละรายการว่าเริ่มและเสร็จสิ้น, หรือเป็นเซิร์ฟเวอร์ worklist ด้วยตนเองผ่าน DIMSE หรือ DICOMweb." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="จากกำหนดการของ RIS ไปสู่การตรวจที่ทำแล้ว">}}

<p>ก่อนทำการสแกน, เทคโนโลจิสต์ไม่ควรพิมพ์ชื่อผู้ป่วย, หมายเลขผู้ป่วยและหมายเลข accession ลงในโมดัลลิตี้. RIS ได้จัดตารางการตรวจไว้แล้ว, และโมดัลลิตี้จะขอข้อมูลนี้: การสืบค้น Modality Worklist จะส่งคืนขั้นตอนการทำรายการที่กำหนดไว้สำหรับเครื่องสแกนนี้และวันนั้น. โมดัลลิตี้คัดลอกข้อมูลผู้ป่วยและคำสั่งลงในภาพ, ทำให้การศึกษาไปถึง PACS พร้อมกับแมพกับคำสั่งแล้ว.</p>

<p>ขณะทำการตรวจ, โมดัลลิตี้จะรายงานกลับด้วย MPPS (Modality Performed Procedure Step): IN PROGRESS เมื่อการสแกนเริ่ม, COMPLETED หรือ DISCONTINUED เมื่อสิ้นสุด, พร้อมกับซีรีส์ที่สร้างขึ้น. RIS ใช้รายงานเหล่านี้เพื่ออัปเดตตาราง, แจ้งให้รังสีแพทย์ทราบว่าภาพกำลังมาถึง, และสำหรับการเรียกเก็บเงิน.</p>

<p><strong>Aspose.Medical for .NET</strong> ครอบคลุมทั้งสองด้านของเวิร์กโฟลว์นี้ใน C# ที่จัดการ: โมดัลลิตี้ที่สืบค้นและรายงาน, และเซิร์ฟเวอร์ worklist ที่ตอบกลับ. ทั้งสองทำงานผ่าน DIMSE, โปรโตคอลเครือข่าย DICOM คลาสสิก, และผ่าน DICOMweb.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="สืบค้น worklist ด้วย C-FIND">}}

<p>การสืบค้น worklist คือคำขอ C-FIND ในโมเดลข้อมูล Modality Worklist. <code>CFindRequest.CreateWorklistQuery</code> สร้างมันด้วยคีย์การคืนค่าที่ปกติ: ข้อมูลประชากรผู้ป่วย, หมายเลข accession, ขั้นตอนการตรวจที่ร้องขอและขั้นตอนการทำรายการที่กำหนด. ตัวกรองที่โมดัลลิตี้ตั้งค่านั้นคือ AE title ของตนเอง, โมดัลลิตี้และวันที่, ทั้งหมดอยู่ในขั้นตอนการทำรายการที่กำหนด.</p>

<div class="codeblock" id="code">
 <h3>ดึงการตรวจสอบของวันนี้สำหรับเครื่อง CT - C#</h3>
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

<p>แต่ละผลลัพธ์เป็น dataset ที่มีแอตทริบิวต์ผู้ป่วยและคำสั่งในระดับบน และขั้นตอนการทำรายการที่กำหนดใน sequence. หากต้องการคีย์การคืนค่าเพิ่มเติม, เพิ่มลงใน <code>query.Identifier</code> พร้อมค่าเปล่าก่อนที่คำขอจะถูกจัดคิว.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="รายงานการตรวจด้วย MPPS">}}

<p>MPPS ประกอบด้วยสองการดำเนินการ DIMSE ที่ทำให้มาตรฐานบนคลาส SOP Modality Performed Procedure Step: N-CREATE เมื่อการตรวจเริ่มและ N-SET เมื่อสิ้นสุด. โมดัลลิตี้เลือก UID ของขั้นตอนที่ทำและใช้มันทั้งสองครั้ง. แอตทริบิวต์ N-CREATE ระบุว่าใครถูกตรวจและขั้นตอนที่กำหนดใดที่ทำ; ส่วนใหญ่ได้มาจากรายการ worklist โดยตรง.</p>

<div class="codeblock" id="code">
 <h3>แอตทริบิวต์ N-CREATE จากรายการ worklist - C#</h3>
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
 <h3>แอตทริบิวต์ N-SET ที่ทำให้ขั้นตอนเสร็จสมบูรณ์ - C#</h3>
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
 <h3>ส่ง IN PROGRESS, จากนั้น COMPLETED - C#</h3>
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

<p>การตรวจที่หยุดก่อนเวลาจะจบด้วยสถานะ DISCONTINUED แทน COMPLETED, และ N-SET จะบรรจุสาเหตุใน Sequence Performed Procedure Step Discontinuation Reason Code.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เวิร์กโฟลว์เดียวกันผ่าน DICOMweb">}}

<p>RIS รุ่นใหม่และแพลตฟอร์มคลาวด์ให้บริการ worklist และ MPPS ผ่าน HTTP. <a href="/medical/net/dicomweb-client/">ไคลเอนต์ DICOMweb</a> ของ Aspose.Medical ค้นหาขั้นตอนการทำรายการที่กำหนดและรายงานขั้นตอนที่ทำด้วย datasets เหมือนข้างต้น. คีย์ภายในขั้นตอนการทำรายการที่กำหนดถูกเขียนเป็นเส้นทาง, เช่น <code>ScheduledProcedureStepSequence.Modality</code>.</p>

<div class="codeblock" id="code">
 <h3>สืบค้น worklist และรายงาน MPPS ผ่าน DICOMweb - C#</h3>
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

<p>ไคลเอนต์ทำงานบน <code>HttpClient</code> ของคุณเอง, ดังนั้นการพิสูจน์ตัวตนและ TLS จะถูกตั้งค่าที่นั่น.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เซิร์ฟเวอร์ worklist ของคุณเองผ่าน DIMSE">}}

<p>โบรกเกอร์ worklist, RIS ขนาดเล็ก หรือระบบทดสอบสำหรับการบูรณาการโมดัลลิตี้ต้องตอบกลับด้านตรงข้าม. ลงทะเบียนตัวจัดการ C-FIND สำหรับ worklist และตัวจัดการ N-CREATE และ N-SET สำหรับ MPPS, แล้วแอปพลิเคชันจะกลายเป็น worklist SCP และ MPPS SCP.</p>

<div class="codeblock" id="code">
 <h3>เริ่มต้นเซิร์ฟเวอร์ worklist และ MPPS - C#</h3>
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

<p>ตัวจัดการนี้อ่านการตรวจสอบที่กำหนดจากไฟล์ <code>.wl</code>, หนึ่ง dataset ต่อไฟล์, และเปรียบเทียบคีย์สามตัวที่โมดัลลิตี้มักส่ง. ผ่าน DIMSE ตัวจัดการรับผิดชอบการแมชชิ่ง: wildcard, ช่วงวันที่และการเลือกคีย์การคืนค่าเขียนไว้ในนั้น.</p>

<div class="codeblock" id="code">
 <h3>ตัวจัดการ worklist C-FIND - C#</h3>
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
 <h3>ตัวจัดการ MPPS N-CREATE และ N-SET - C#</h3>
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

<p>ที่นี่ขั้นตอนถูกเก็บไว้ในหน่วยความจำและสถานะถูกพิมพ์ออก. MPPS SCP จริงจะเก็บขั้นตอน, ปฏิเสธการเปลี่ยนแปลงขั้นตอนที่มีสถานะ COMPLETED หรือ DISCONTINUED แล้ว, และส่งผลลัพธ์ไปยัง RIS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เซิร์ฟเวอร์ worklist ของคุณเองผ่าน DICOMweb">}}

<p>ผ่าน DICOMweb ไลบรารีทำงานของเซิร์ฟเวอร์ส่วนมาก. สำหรับการค้นหา worklist มันใช้กฎการแมชชิ่งของ DICOM, รวมถึง wildcard และช่วงวันที่, แล้วแบ่งผลลัพธ์เป็นหน้า. สำหรับ MPPS มันตรวจสอบแอตทริบิวต์ที่จำเป็นและการเปลี่ยนแปลงสถานะ. คลาสของคุณจะให้ข้อมูล.</p>

<div class="codeblock" id="code">
 <h3>Program.cs ของเซิร์ฟเวอร์ DICOMweb worklist และ MPPS - C#</h3>
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
 <h3>การจัดเก็บ worklist และ MPPS - C#</h3>
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

<p>เซิร์ฟเวอร์ต้องการแพ็กเกจ <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. <a href="/medical/net/dicomweb-server/">หน้าสำหรับ DICOMweb server</a> แสดงวิธีเพิ่มบริการ Studies ไปยังแอปพลิเคชันเดียวกัน, และ <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">คู่มือผู้ให้บริการเวิร์กโฟลว์</a> อธิบายสัญญาการแก้ไขของ <code>TryReplaceAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklist ที่เกินขอบเขตโมดัลลิตี้">}}

<p>MWL และ MPPS เชื่อมต่อโมดัลลิตี้กับ RIS. สำหรับงานที่เคลื่อนย้ายระหว่างระบบอื่น ๆ เช่น การประมวลผล AI, การมอบหมายการอ่านหรือการประมวลผลหลัง, DICOM กำหนด Unified Procedure Step. <a href="/medical/net/ups-rs/">หน้าสำหรับ UPS-RS</a> แสดงวิธีสร้าง, ทำเคลมและเสร็จสิ้น workitem เหล่านั้นด้วยไลบรารีเดียวกัน.</p>

<p>หน้าที่เกี่ยวข้อง: <a href="/medical/net/dicom-networking/">DICOM networking</a> สำหรับ C-ECHO, C-STORE และบริการ DIMSE อื่น ๆ, <a href="/medical/net/dicomweb-client/">ไคลเอนต์ DICOMweb</a> และ <a href="/medical/net/dicomweb-server/">เซิร์ฟเวอร์ DICOMweb</a> สำหรับ studies ผ่าน HTTP, และ <a href="/medical/net/dicom-tags/">แท็ก DICOM</a> สำหรับอ่านเนื้อหาของรายการ worklist.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือผู้พัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนแบบเสียค่าใช้จ่าย" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้อง Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
