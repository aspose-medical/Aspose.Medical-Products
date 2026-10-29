---
title: การสื่อสารเครือข่าย DICOM ใน C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: เชื่อมต่อแอปพลิเคชัน .NET ของคุณกับ PACS ตรวจสอบด้วย C‑ECHO ส่งภาพด้วย C‑STORE สืบค้นด้วย C‑FIND และรับภาพด้วย SCP ของคุณเอง ลูกค้าและเซิร์ฟเวอร์ DIMSE ใน C# แท้
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="การสื่อสารเครือข่าย DICOM ใน .NET C#" h2="สื่อสารกับ PACS จากแอปพลิเคชันของคุณเอง: C‑ECHO, C‑STORE, C‑FIND, C‑MOVE และ C‑GET ทั้งในฐานะคลไอเอนต์และเซิร์ฟเวอร์ โดยใช้ C# ที่จัดการได้โดยไม่ต้องติดตั้งอะไรบนเครื่อง" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="เชื่อมต่อกับ PACS จากโค้ดของคุณเอง">}}

<p>การอ่านไฟล์ DICOM เป็นส่วนที่ง่ายของการทำภาพทางการแพทย์ ทันทีที่แอปพลิเคชันของคุณต้องทำงานกับระบบโรงพยาบาลจริง ระบบต้องสนับสนุน DIMSE: เปิดการเชื่อมต่อกับ PACS ส่งภาพ ถามว่ามีการศึกษาอะไรบ้าง และตอบกลับเมื่อระบบอื่นส่งข้อมูลกลับมา</p>

<p><strong>Aspose.Medical for .NET</strong> มีโปรโตคอลนั้นเป็นส่วนหนึ่งของไลบรารี <code>Aspose.Medical.Dicom.Network</code> ให้คุณใช้งานคลไอเอนต์และเซิร์ฟเวอร์ DIMSE ทั้งสองเขียนด้วย C# ที่จัดการได้ ไม่ต้องมีชุดเครื่องมือเนทีฟติดตั้ง ไม่มีบริการให้ตั้งค่า และไม่จำกัดแพลตฟอร์ม ดังนั้นโค้ดเดียวกันสามารถทำงานบน Windows, Linux และในคอนเทนเนอร์ได้</p>

<p>สามสิ่งครอบคลุมการรวมส่วนใหญ่ และแต่ละอย่างใช้เพียงไม่กี่บรรทัด: ตรวจสอบการเชื่อมต่อด้วย C‑ECHO ผลักดันภาพด้วย C‑STORE และค้นหาสิ่งที่อยู่ฝั่งตรงข้ามด้วย C‑FIND</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เริ่มต้นด้วย C‑ECHO">}}

<p>C‑ECHO คือการ ping ของ DICOM มันยืนยันว่าโฮสต์, พอร์ตและสอง AE Title ถูกต้องก่อนที่จะตรวจสอบสิ่งอื่นใด สร้างคลไอเอนต์หนึ่งครั้ง ให้ handler เพื่อติดตามคำตอบ แล้วส่งคำขอ</p>

<div class="codeblock" id="code">
 <h3>ตรวจสอบการเชื่อมต่อด้วย C-ECHO - C#</h3>
 <pre><code class="cs">AssociationNegotiationOptions negotiation = new AssociationNegotiationOptions()
    .WithPresentationContext(new PresentationContext
    {
        AbstractSyntax = Uid.Verification,
        Role = null,
        TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
    });

DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = negotiation
    })
    .AddCEchoHandler((request, response, cancellationToken) =>
    {
        Console.WriteLine($"C-ECHO answered with status 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(new CEchoRequest());

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>สถานะ <code>0x0000</code> หมายถึงสำเร็จ คำร้องจะถูกจัดคิวและส่งผ่านการเชื่อมต่อเดียว ดังนั้นงานหลายรายการจะไม่ได้เปิดการเชื่อมต่อแยกสำหรับแต่ละรายการ</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ส่งภาพด้วย C‑STORE">}}

<p>C‑STORE คือสิ่งที่แอปพลิเคชันทำหลังจากสร้างหรือรับภาพแล้ว: จะผลักดันอินสแตนซ์ไปยังคลังข้อมูล จัดคิวคำร้องหนึ่งรายการต่ออินสแตนซ์แล้วส่งพร้อมกัน</p>

<div class="codeblock" id="code">
 <h3>ส่งไฟล์ DICOM ไปยัง PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Presentation contexts ที่คุณกำหนดจะเป็นตัวกำหนดว่าคลังข้อมูลรับอะไร หากต้องการ syntax ที่บีบอัด ให้ทำการแปลงรูปแบบก่อนส่งตามที่หน้าตัวอย่าง <a href="/medical/net/dicom-transfer-syntax-conversion/">การแปลง syntax การถ่ายโอน</a> แสดงไว้ หรือระบุทางเลือกใน <code>AdditionalTransferSyntaxes</code> แล้วให้การเจรจาเลือกหนึ่ง</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ค้นหาการศึกษาด้วย C‑FIND">}}

<p>C‑FIND ตอบคำถามว่า "คลังข้อมูลมีอะไรบ้าง" ผลการจับคู่จะมาทีละรายการ แต่ละรายการมีชุดข้อมูลระบุตัวตนของตนเอง และการตอบกลับสุดท้ายจะปิดการสืบค้น</p>

<div class="codeblock" id="code">
 <h3>สืบค้นการศึกษาโดยผู้ป่วย - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.StudyRootQueryRetrieveInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddCFindHandler((request, response, cancellationToken) =>
    {
        // A match arrives with an identifier; the final response carries the status only
        if (response.Identifier is not null)
            Console.WriteLine(response.Identifier.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty));

        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(CFindRequest.CreateStudyQuery(
    patientId: "PATIENT-001",
    patientName: null,
    studyDateTime: null,
    accession: null,
    studyId: null,
    modalitiesInStudy: null,
    studyInstanceUid: null,
    priority: DimsePriority.Medium));

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Factory เดียวกันสร้างระดับการสืบค้นอื่น ๆ: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> และ <code>CreateImageQuery</code> <code>CreateWorklistQuery</code> สร้างการสืบค้น worklist ของอุปกรณ์ ซึ่งเป็นการสืบค้นที่อุปกรณ์ใช่ก่อนการสแกน; <a href="/medical/net/modality-worklist/">หน้ารายการงานของอุปกรณ์</a> แสดงพร้อมกับ MPPS และฝั่งเซิร์ฟเวอร์ของ worklist</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="รับภาพ: SCP เก็บของคุณเอง">}}

<p>ไลบรารีนี้ยังสามารถทำหน้าที่เป็นเซิร์ฟเวอร์ได้ ลงทะเบียน handler สำหรับบริการที่คุณต้องการให้บริการ เริ่มรับฟัง และแอปพลิเคชันของคุณจะกลายเป็นโหนด DICOM ที่อุปกรณ์หรือ PACS อื่น ๆ สามารถส่งข้อมูลไปให้ได้</p>

<div class="codeblock" id="code">
 <h3>รับภาพเข้ามา - C#</h3>
 <pre><code class="cs">DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Any, 11112)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.SecondaryCaptureImageStorage,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddSingletonCStoreHandler(new StoreHandler())
    .Build();

await server.StartAsync(CancellationToken.None);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Handler ที่เก็บข้อมูลที่มาถึง - C#</h3>
 <pre><code class="cs">public sealed class StoreHandler : ICStoreRequestHandler
{
    public ValueTask&lt;CStoreResponse&gt; Handle(CStoreRequest request, CancellationToken cancellationToken)
    {
        // Write what arrived, then answer Success
        new DicomFile(request.Dataset).Save($"{request.AffectedSopInstanceUid}.dcm");

        CStoreResponse response = new();
        response.Command.AddOrUpdate(Tag.Status, (ushort)0x0000);
        return ValueTask.FromResult(response);
    }
}</code></pre>
</div>

<p>สิ่งที่ handler ทำกับชุดข้อมูลเป็นการตัดสินใจของคุณ: เขียนลงดิสก์, ใส่คิว, ทำการลบข้อมูลส่วนบุคคลก่อนด้วย <a href="/medical/net/anonymization/">API การทำให้เป็นนิรนาม</a> หรือแปลงเป็น syntax ของคลังข้อมูล</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="สิ่งอื่น ๆ ที่ API การสื่อสารเครือข่ายครอบคลุม">}}

<p>สามบริการข้างต้นเป็นบริการที่พบบ่อย ส่วนที่เหลือของ DIMSE ก็พร้อมใช้งานเช่นกัน:</p>

<ul>
<li>การดึงข้อมูล: C‑MOVE และ C‑GET พร้อมกับการนับจำนวน sub‑operation ที่รายงานในการตอบกลับ</li>
<li>เซอร์วิส N: N‑CREATE, N‑SET, N‑GET, N‑ACTION, N‑DELETE และ N‑EVENT‑REPORT ซึ่งเป็นพื้นฐานของ storage commitment และ MPPS</li>
<li>การควบคุมการเชื่อมต่อ: presentation contexts, บทบาทของ service class, การเจรจาขยาย, หน้าต่างการดำเนินการแบบอะซิงโครนัส, การเจรจา user identity, และ hook นโยบายที่สามารถปฏิเสธการเชื่อมต่อโดยอ้างอิง AE title</li>
<li>TLS ทั้งสองฝั่ง ผ่าน <code>TlsInitiatorAuthenticator</code> และ <code>TlsAcceptorAuthenticator</code> พร้อมการตรวจสอบใบรับรองของคุณเองหากต้องการ</li>
<li>การกำหนดเวลา timeout สำหรับแต่ละขั้นตอน ตั้งแต่การเชื่อมต่อ TCP จนถึงการปล่อย และการแจ้งเตือนสำหรับวงจรชีวิตของการเชื่อมต่อ เพื่อให้โหนดที่ทำงานต่อเนื่องสามารถบันทึกเหตุการณ์ที่เกิดขึ้นได้</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">คู่มือการสื่อสารเครือข่าย DICOM</a> บันทึกรายการทุกตัวเลือกและทุก handler</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pure .NET, จาก socket ถึงข้อมูลพิกเซล">}}

<p>ทุกอย่างในหน้านี้เป็นโค้ดที่จัดการจากแพ็กเกจเดียวกันที่อ่านและเขียนไฟล์ การเชื่อมต่อ, codec และ parser มาจากไลบรารีเดียวกัน ดังนั้นการศึกษาที่รับผ่านเครือข่ายจึงสามารถทำให้เป็นนิรนาม, แปลงรูปแบบหรือทำ serialization ได้โดยไม่ต้องออกจากกระบวนการและไม่มีการพึ่งพาเนทีฟใด ๆ ในสายงาน</p>

<p>หน้าที่เกี่ยวข้อง: <a href="/medical/net/dicom-transfer-syntax-conversion/">การแปลง transfer syntax</a> สำหรับสิ่งที่จะส่ง, <a href="/medical/net/anonymization/">การทำให้เป็นนิรนาม</a> สำหรับสิ่งที่ต้องลบก่อน, และ <a href="/medical/net/dicom-tags/">DICOM tags</a> สำหรับการอ่านสิ่งที่มาถึง</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสาร" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือผู้พัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="เอกสารอ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบจ่ายค่าใช้จ่าย" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องเลือก Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายการลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
