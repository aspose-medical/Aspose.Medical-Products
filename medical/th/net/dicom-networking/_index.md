---
title: การทำงานเครือข่าย DICOM ใน C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: เชื่อมต่อแอปพลิเคชัน .NET ของคุณกับ PACS ตรวจสอบด้วย C-ECHO ส่งภาพด้วย C-STORE คิวรีด้วย C-FIND และรับภาพด้วย SCP ของคุณเอง ไคลเอนต์และเซิร์ฟเวอร์ DIMSE ด้วย C# แท้
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="การทำงานเครือข่าย DICOM ใน .NET C#" h2="สื่อสารกับ PACS จากแอปพลิเคชันของคุณ: C-ECHO, C-STORE, C-FIND, C-MOVE และ C-GET ทั้งในฐานะไคลเอนต์และเซิร์ฟเวอร์ ด้วย C# ที่จัดการได้โดยไม่ต้องติดตั้งอะไรบนเครื่อง" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="เชื่อมต่อกับ PACS จากโค้ดของคุณ">}}

<p>การอ่านไฟล์ DICOM เป็นครึ่งหนึ่งที่ง่ายของการแพทย์ภาพ เมื่อแอปพลิเคชันของคุณต้องทำงานกับระบบโรงพยาบาลจริง มันต้องสื่อสารด้วย DIMSE: เปิดการเชื่อมโยงกับ PACS ส่งภาพ ถามว่ามีการศึกษามาใดบ้าง และตอบกลับเมื่อระบบอื่นส่งข้อมูลกลับมา.</p>

<p><strong>Aspose.Medical for .NET</strong> มีโปรโตคอลดังกล่าวเป็นส่วนหนึ่งของไลบรารี <code>Aspose.Medical.Dicom.Network</code> ให้คุณมีไคลเอนต์ DIMSE และเซิร์ฟเวอร์ DIMSE ทั้งสองเขียนด้วย C# ที่จัดการได้ ไม่ต้องติดตั้งชุดเครื่องมือเนทีฟ ไม่ต้องกำหนดค่าบริการใด ๆ และไม่มีความเฉพาะแพลตฟอร์ม ดังนั้นโค้ดเดียวกันจึงทำงานได้บน Windows, Linux และในคอนเทนเนอร์</p>

<p>สามสิ่งครอบคลุมการเชื่อมต่อส่วนใหญ่ และแต่ละอย่างใช้ไม่กี่บรรทัด: ตรวจสอบการเชื่อมต่อด้วย C-ECHO ส่งภาพด้วย C-STORE และค้นหาสิ่งที่อยู่ฝั่งตรงข้ามด้วย C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เริ่มต้นด้วย C-ECHO">}}

<p>C-ECHO คือการ ping ของ DICOM มันยืนยันว่าฮอสท์, พอร์ต และสอง AE Title ถูกต้องก่อนที่จะมีข้อผิดพลาดอื่นใด สร้างไคลเอนต์หนึ่งครั้ง มอบแฮนด์เลอร์ที่สังเกตผลตอบกลับ แล้วส่งคำขอ</p>

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

<p>สถานะ <code>0x0000</code> หมายถึงสำเร็จ คำขอจะถูกจัดคิวและส่งผ่านการเชื่อมต่อเดียวกัน ดังนั้นชุดงานจะไม่เปิดการเชื่อมต่อต่อรายการ</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ส่งภาพด้วย C-STORE">}}

<p>C-STORE คือสิ่งที่แอปพลิเคชันทำหลังจากสร้างหรือรับภาพแล้ว: มันผลักดันอินสแตนซ์ไปยังคลังข้อมูล จัดคิวหนึ่งคำขอต่ออินสแตนซ์และส่งพร้อมกัน</p>

<div class="codeblock" id="code">
 <h3>ส่งไฟล์ DICOM ไปยัง PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Presentation contexts ที่คุณเสนอกำหนดว่าคลังข้อมูลจะรับอะไร หากต้องการ syntax ที่บีบอัด ให้ทำการแปลงโค้ดก่อนส่ง ตามที่หน้า <a href="/medical/net/dicom-transfer-syntax-conversion/">การแปลง transfer syntax</a> แสดง หรือระบุทางเลือกใน <code>AdditionalTransferSyntaxes</code> แล้วให้การต่อรองเลือกหนึ่ง</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ค้นหาการศึกษาด้วย C-FIND">}}

<p>C-FIND ตอบคำถาม "คลังข้อมูลมีอะไรบ้าง" ผลลัพธ์จะมาทีละรายการ แต่ละรายการมีชุดข้อมูล identifier ของตนเอง และการตอบสุดท้ายจะปิดการค้นหา</p>

<div class="codeblock" id="code">
 <h3>ค้นหาการศึกษาโดยผู้ป่วย - C#</h3>
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

<p>แฟกทอรีเดียวกันสร้างระดับการค้นหาอื่น ๆ: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> และ <code>CreateImageQuery</code> <code>CreateWorklistQuery</code> สร้างการค้นหา worklist ของอุปกรณ์ ซึ่งเป็นการค้นหาที่อุปกรณ์ร้องขอก่อนทำการสแกน</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="รับภาพ: SCP ที่เก็บของคุณเอง">}}

<p>ไลบรารีนี้ยังทำหน้าที่เป็นเซิร์ฟเวอร์ด้วย ลงทะเบียนแฮนด์เลอร์สำหรับบริการที่คุณต้องการนำเสนอ เริ่มรับฟัง และแอปพลิเคชันของคุณจะกลายเป็นโหนด DICOM ที่อุปกรณ์หรือ PACS อื่นสามารถส่งข้อมูลไปยังได้.</p>

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
 <h3>แฮนด์เลอร์ที่เก็บข้อมูลที่เข้ามา - C#</h3>
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

<p>สิ่งที่แฮนด์เลอร์ทำกับชุดข้อมูลขึ้นกับคุณ: เขียนลงดิสก์, ใส่ในคิว, ทำให้เป็นนามนิรนามก่อนด้วย <a href="/medical/net/anonymization/">API การทำให้เป็นนามนิรนาม</a> หรือแปลงโค้ดเป็น syntax ของคลังข้อมูล</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="สิ่งอื่น ๆ ที่ API เครือข่ายครอบคลุม">}}

<p>สามบริการข้างต้นเป็นบริการทั่วไป ส่วนที่เหลือของ DIMSE ก็มีเช่นกัน:</p>

<ul>
<li>การดึงข้อมูล: C-MOVE และ C-GET พร้อมตัวนับการดำเนินการย่อยที่รายงานในคำตอบ</li>
<li>บริการ N: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE และ N-EVENT-REPORT ซึ่งเป็นพื้นฐานของ storage commitment และ MPPS</li>
<li>การควบคุมการเชื่อมต่อ: presentation contexts, บทบาทของ service class, การต่อรองขยาย, หน้าต่างการดำเนินการแบบอะซิงโครนัส, การต่อรองอัตลักษณ์ผู้ใช้, และ hook นโยบายที่สามารถปฏิเสธการเชื่อมต่อโดยอ้างถึง AE title</li>
<li>TLS ด้านทั้งสองผ่าน <code>TlsInitiatorAuthenticator</code> และ <code>TlsAcceptorAuthenticator</code> พร้อมการตรวจสอบใบรับรองของคุณเองหากต้องการ</li>
<li>การกำหนดเวลา timeout สำหรับทุกขั้นตอน ตั้งแต่การเชื่อมต่อ TCP ถึงการปล่อยเชื่อมต่อ และการแจ้งเตือนสำหรับวงจรชีวิตของการเชื่อมต่อเพื่อให้โหนดที่ทำงานนานสามารถบันทึกเหตุการณ์ที่เกิดขึ้น</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">คู่มือการทำงานเครือข่าย DICOM</a> บันทึกรายละเอียดของทุกตัวเลือกและทุกแฮนด์เลอร์.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pure .NET, ตั้งแต่ซ็อกเก็ตจนถึงข้อมูลพิกเซล">}}

<p>ทุกอย่างในหน้านี้เป็นโค้ดที่จัดการจากแพ็กเกจเดียวกันที่อ่านและเขียนไฟล์ การเชื่อมต่อ, codecs และ parser มาจากไลบรารีเดียวกัน ดังนั้นการศึกษาที่รับผ่านเครือข่ายจึงสามารถทำให้นามนิรนาม, แปลงโค้ด หรือทำการ serialization ได้โดยไม่ต้องออกจากกระบวนการและไม่มีการพึ่งพาเนทีฟใด ๆ ในโซ่</p>

<p>หน้าเกี่ยวข้อง: <a href="/medical/net/dicom-transfer-syntax-conversion/">การแปลง transfer syntax</a> สำหรับสิ่งที่ต้องส่ง, <a href="/medical/net/anonymization/">การทำให้นามนิรนาม</a> สำหรับสิ่งที่ต้องลบก่อน, และ <a href="/medical/net/dicom-tags/">DICOM tags</a> สำหรับอ่านสิ่งที่เข้ามา.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารอธิบาย" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือผู้พัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบชำระเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้อง Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
