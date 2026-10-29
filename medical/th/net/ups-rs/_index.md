---
title: UPS-RS ใน C# .NET - งานขั้นตอนกระบวนการแบบรวมของ DICOMweb | Aspose.Medical
weight: 9400

description: สร้าง, เรียกสิทธิ์และทำงานเสร็จสิ้นของ DICOM UPS workitems จาก C#, รับการเปลี่ยนแปลงสถานะผ่าน WebSocket, หรือรันเซิร์ฟเวอร์ UPS-RS ของคุณเองบน ASP.NET Core ด้วย Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS สำหรับ .NET C#" h2="มอบงานการสร้างภาพระหว่างระบบด้วย DICOM Unified Procedure Step: สร้าง workitems, ให้ worker เรียกสิทธิ์และทำงานให้เสร็จ, แล้วติดตามการเปลี่ยนแปลงทุกอย่าง ไม่ว่าจะเป็นในฐานะไคลเอนต์หรือเซิร์ฟเวอร์." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="คิวงานที่ใช้ DICOM">}}

<p>โมเดล AI ต้องประมวลผล CT หน้าอกใหม่ทุกกรณี บริการเทเลราดีโอลอจีจะมอบการศึกษาให้ผู้อ่าน ห้องปฏิบัติการ 3D จะรับคำสั่งหลังการประมวลผล แต่ละกรณีเป็นคิวของงานระหว่างระบบ และ DICOM ได้กำหนดคิวนี้ไว้: Unified Procedure Step (UPS) ซึ่งให้บริการผ่าน HTTP โดยบริการ UPS-RS ของ DICOMweb.</p>

<p>UPS workitem ระบุว่าต้องทำอะไร สำหรับผู้ป่วยใดและใช้ข้อมูลเข้าอะไร มันอยู่ในสถานะ SCHEDULED เมื่อสร้าง Worker จะเรียกสิทธิ์และเปลี่ยนเป็น IN PROGRESS แล้วทำให้เสร็จเป็น COMPLETED หรืออาจสิ้นสุดเป็น CANCELED ระบบอื่นๆ จะสมัครรับและได้รับการเปลี่ยนแปลงสถานะทุกครั้งผ่าน WebSocket จึงไม่ต้องทำการ polling.</p>

<p><strong>Aspose.Medical for .NET</strong> ทำการนำเสนอทั้งสองด้านใน C# ที่จัดการ: ไคลเอนต์ที่พิมพ์ชนิดสำหรับเซิร์ฟเวอร์ UPS-RS ใดๆ, และด้านเซิร์ฟเวอร์สำหรับ ASP.NET Core, โดยไลบรารีจะตรวจสอบกฎของ UPS และโค้ดของคุณจะเก็บ workitems.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="กำหนด workitem">}}

<p>ระบบที่ต้องการให้ทำงานจะสร้าง workitem ค่ะ SOP Instance UID ของมันจะใช้ระบุตัวตนต่อจากนั้น; ป้าย, ป้าย worklist และความสำคัญจะบอกว่า worker ต้องทำอะไร.</p>

<div class="codeblock" id="code">
 <h3>สร้าง UPS workitem - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="เรียกสิทธิ์, ทำงาน, ทำให้เสร็จ">}}

<p>Worker จะค้นหา worklist เพื่อหาสิ่งที่ทำได้และเรียกสิทธิ์ workitem โดยย้ายไปยัง IN PROGRESS ด้วย transaction UID ที่มันสร้างขึ้น จากจุดนั้นต่อไปเฉพาะผู้ถือ UID นี้เท่านั้นที่สามารถอัปเดตหรือเสร็จสิ้น workitem ได้ ดังนั้น worker สองคนจะไม่มีการประมวลผลงานเดียวกัน.</p>

<div class="codeblock" id="code">
 <h3>ค้นหา, เรียกสิทธิ์และทำงานเสร็จของ workitems ที่กำหนดเวลา - C#</h3>
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

<p>COMPLETED จำเป็นต้องมีบันทึกของงาน: ผู้ทำ, สิ่งที่ทำ, เวลาเริ่มและสิ้นสุด, และผลลัพธ์ Worker จะบันทึกข้อมูลเหล่านี้ด้วยการอัปเดตก่อนที่สถานะจะเปลี่ยน.</p>

<div class="codeblock" id="code">
 <h3>ข้อมูลขั้นตอนที่ทำ - C#</h3>
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

<p>เซิร์ฟเวอร์ที่สร้างด้วย Aspose.Medical จะบังคับใช้กฎเหล่านี้ Worker คนที่สองที่พยายามเรียกสิทธิ์ workitem ที่อยู่ระหว่างดำเนินการจะได้รับ HTTP 409, เช่นเดียวกับการเปลี่ยนแปลงสถานะของ workitem ที่เสร็จแล้ว คำขอ COMPLETED โดยไม่มีข้อมูลขั้นตอนที่ทำจะได้รับ HTTP 400 ลูกค้าจะรายงานข้อผิดพลาดเหล่านี้เป็น <code>DicomWebClientException</code> พร้อมกับการตอบกลับ HTTP.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ติดตาม worklist ผ่าน WebSocket">}}

<p>แดชบอร์ด, ระบบที่สั่งงาน หรือ worker ที่รอคอยงานใหม่จะสมัครรับด้วย AE title ของตนและเปิดการเชื่อมต่อแจ้งเตือน ทุกเหตุการณ์รายงานจะมาถึงเป็น dataset พร้อมกับ workitem UID และสถานะใหม่ของมัน.</p>

<div class="codeblock" id="code">
 <h3>รับรายงานเหตุการณ์ UPS - C#</h3>
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

<p>นอกจาก worklist ทั้งหมดแล้ว ไคลเอนต์สามารถสมัครรับข้อมูลของ workitem เดียวด้วย <code>SubscribeWorkitemAsync</code> หรือส่วนที่กรองของ worklist ด้วย <code>SubscribeFilteredWorklistAsync</code> หากเซิร์ฟเวอร์รองรับตัวกรอง การยกเลิกการรับจะปิดการเชื่อมต่อ; เปิดการเชื่อมต่อใหม่ด้วย <code>ConnectNotificationsAsync</code> เพื่อดำเนินต่อ.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เซิร์ฟเวอร์ UPS-RS ของคุณเอง">}}

<p>แพ็กเกจ <code>Aspose.Medical.Dicom.Web.AspNetCore</code> เพิ่ม UPS-RS ให้กับบริการ ASP.NET Core ไลบรารีจัดการทุกคำขอ: ตรวจสอบ payload ของ workitem ใหม่, การเปลี่ยนแปลงสถานะ, transaction UID ของ worker, และข้อมูลที่ COMPLETED ต้องการ, และใช้กฎการแม็ชของ DICOM กับการค้นหา โค้ดของคุณจะเก็บ workitems.</p>

<div class="codeblock" id="code">
 <h3>Program.cs ของเซิร์ฟเวอร์ UPS-RS - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>การจัดเก็บ workitem - C#</h3>
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

<p>แต่ละ workitem ที่เก็บมี revision. <code>TryReplaceAsync</code> จะแทนที่เมื่อ revision ยังเป็นค่าที่ไลบรารีอ่านอยู่ ดังนั้นคำขอสองคำขอที่พยายามเปลี่ยน workitem เดียวในเวลาเดียวกันจะไม่สามารถเขียนทับกันได้; ไลบรารีจะลองใหม่หรือรายงานความขัดแย้ง. แทนที่ dictionary ด้วยฐานข้อมูลของคุณและรักษาการตรวจสอบนี้.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การแจ้งเตือนบนเซิร์ฟเวอร์">}}

<p>เพื่อส่งรายงานเหตุการณ์ ให้ลงทะเบียน <code>IDicomWebSubscriptionProvider</code> และเรียก <code>app.UseWebSockets()</code> ก่อน <code>MapDicomWeb</code>. ผู้ให้บริการจะรักษาการสมัครรับและคิวหนึ่งรายการของรายงานเหตุการณ์ต่อผู้สมัคร, และไลบรารีจะส่งรายงานเหตุการณ์เมื่อ workitem มีการเปลี่ยนแปลง. ผู้ให้บริการยังบอกไลบรารีว่า AE title ของผู้เรียกที่ผ่านการตรวจสอบคืออะไร เนื่องจากรายงานเหตุการณ์ระบุระบบที่เปลี่ยนสถานะ.</p>

<p>The <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">คู่มือผู้ให้บริการ workflow</a> มี provider การสมัครรับแบบในหน่วยความจำที่สมบูรณ์, และ <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">คู่มือ ASP.NET Core</a> ครอบคลุมการตั้งค่า WebSocket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL และ MPPS">}}

<p>UPS ครอบคลุมสิ่งที่ Modality Worklist เก่าและ MPPS ทำระหว่าง RIS กับอุปกรณ์ภาพ, และเพิ่ม worker ที่ไม่ใช่อุปกรณ์ภาพ โดยให้การเรียกสิทธิ์และการแจ้งเตือน. หลายอุปกรณ์ภาพยังคงใช้ MWL และ MPPS ผ่าน DIMSE; <a href="/medical/net/modality-worklist/">หน้ารายการงานอุปกรณ์ภาพ</a> แสดง workflow นี้ด้วยไลบรารีเดียวกัน.</p>

<p>หน้าที่เกี่ยวข้อง: <a href="/medical/net/dicomweb-client/">ไคลเอนต์ DICOMweb</a> สำหรับบริการ DICOMweb อื่น ๆ, <a href="/medical/net/dicomweb-server/">เซิร์ฟเวอร์ DICOMweb</a> สำหรับ QIDO‑RS, WADO‑RS และ STOW‑RS บนเซิร์ฟเวอร์เดียวกัน, และ <a href="/medical/net/dicom-networking/">เครือข่าย DICOM</a> สำหรับ DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งการเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสาร" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือสำหรับนักพัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบชำระเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องเลือก Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
