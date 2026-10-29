---
title: ไคลเอนท์ DICOMweb ใน C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: ค้นหา ดาวน์โหลดและอัปโหลดการศึกษา DICOM ผ่าน HTTP จาก .NET. ไคลเอนท์ DICOMweb ที่เขียนด้วย C# เพียวสำหรับ QIDO-RS, WADO-RS และ STOW-RS ที่ทำงานร่วมกับ PACS, VNA และบริการภาพคลาวด์.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="ไคลเอนท์ DICOMweb สำหรับ .NET C#" h2="ค้นหา ดาวน์โหลดและอัปโหลดการศึกษาใด ๆ บนเซิร์ฟเวอร์ DICOMweb ด้วย QIDO-RS, WADO-RS และ STOW-RS ด้วย C# ที่จัดการได้โดยไม่ต้องติดตั้งอะไรบนเครื่อง" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM ผ่าน HTTP จากโค้ดของคุณเอง">}}

<p>DICOMweb คือรูปแบบ HTTP ของ DICOM. แทนการเชื่อมต่อ DIMSE บนพอร์ต 104 แอปพลิเคชันของคุณจะส่งคำขอ HTTPS ปกติ: QIDO-RS เพื่อค้นหา, WADO-RS เพื่อดาวน์โหลด, STOW-RS เพื่ออัปโหลด. บริการภาพคลาวด์เช่น Azure Health Data Services DICOM service และ Google Cloud Healthcare API ทำงานในลักษณะนี้เช่นกัน และรวมถึงคลังข้อมูลเช่น Orthanc และ dcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> มีไคลเอนท์ DICOMweb ตั้งแต่เวอร์ชัน 26.10. <code>DicomWebClient</code> หุ้มการทำธุรกรรมมาตรฐานทั้งหมดในเมธอดที่กำหนดประเภท, และผลลัพธ์จะมาถึงเป็นอ็อบเจกต์ <code>Dataset</code> และ <code>DicomFile</code> เดียวกันที่ส่วนอื่นของไลบรารีอ่านและเขียน. เซิร์ฟเวอร์ด้านตรงข้ามสามารถเป็นการทำงานของ DICOMweb ใดก็ได้; ไม่จำเป็นต้องสร้างด้วย Aspose.Medical.</p>

<p>ไคลเอนท์เป็น C# ที่จัดการอยู่บน <code>HttpClient</code>. ไม่มีชุดเครื่องมือพื้นเมืองและไม่มีบริการพร็อกซี่, ดังนั้นโค้ดเดียวกันจึงทำงานได้บน Windows, Linux และในคอนเทนเนอร์.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เชื่อมต่อกับเซิร์ฟเวอร์ DICOMweb">}}

<p>สร้างไคลเอนท์จาก <code>HttpClient</code> และ URL รากของบริการของเซิร์ฟเวอร์. การรับรองตัวตน, การตั้งค่า TLS, พร็อกซี่และการจัดคิวการเชื่อมต่ออยู่ใน <code>HttpClient</code>, ที่แอปพลิเคชันของคุณได้ตั้งค่าไว้แล้ว. ไคลเอนท์ไม่เป็นเจ้าของ <code>HttpClient</code> และไม่ทำการ dispose มัน.</p>

<div class="codeblock" id="code">
 <h3>สร้างไคลเอนท์ DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>หากการปรับใช้หนึ่งให้บริการ WADO-URI หรือบริการอื่นจาก URL ที่แตกต่าง, <code>ServiceBaseUris</code> จะทับค่ารากสำหรับบริการนั้นเท่านั้น.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ค้นหาการศึกษาด้วย QIDO-RS">}}

<p>การค้นหารับพารามิเตอร์การจับคู่โดยใช้คีย์เวิร์ด DICOM เป็นชื่อ, ฟิลด์ที่จะคืนค่า, และการควบคุมการแบ่งหน้า. ผลการจับคู่มาถึงเป็นสตรีมของ dataset, อ่านทีละรายการขณะที่การตอบกลับยังคงดาวน์โหลด.</p>

<div class="codeblock" id="code">
 <h3>ค้นหาการศึกษาของผู้ป่วย - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> และ <code>SearchInstancesAsync</code> ค้นหาระดับหนึ่งระดับลง, ครอบคลุมทั้งคลังข้อมูลทั้งหมดหรือภายในการศึกษา หรือซีรีส์เดียว. ตัวเลือกเดียวกันยังตั้งค่าการจับคู่แบบ fuzzy, การจับคู่ค่าว่าง, และการเลื่อนผลลัพธ์สำหรับการแบ่งหน้า.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ดาวน์โหลดการศึกษาโดยใช้ WADO-RS">}}

<p>การศึกษาหนึ่งอาจมีหลายพันอินสแตนซ์. <code>RetrieveStudyAsync</code> คืนค่าพวกมันเป็นสตรีม, ดังนั้นแต่ละไฟล์จะถูกวิเคราะห์เมื่อส่วนของการตอบกลับมาถึงและสามารถบันทึกได้ก่อนที่ไฟล์ถัดไปจะอ่าน. การใช้หน่วยความจำไม่เพิ่มตามขนาดของการศึกษา.</p>

<div class="codeblock" id="code">
 <h3>บันทึกทุกอินสแตนซ์ของการศึกษา - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> และ <code>RetrieveInstanceAsync</code> ดาวน์โหลดซีรีส์หนึ่งหรืออินสแตนซ์หนึ่งในลักษณะเดียวกัน.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="รับภาพย่อหรือภาพที่เรนเดอร์แล้ว">}}

<p>โปรแกรมดูหรือหน้าจอรายการงานมักต้องการภาพ, ไม่ใช่ไฟล์ DICOM. เซิร์ฟเวอร์ทำการเรนเดอร์และส่ง JPEG หรือ PNG, ดังนั้นไคลเอนท์จึงไม่ต้องดาวน์โหลดหรือถอดรหัสข้อมูลพิกเซลต้นฉบับ.</p>

<div class="codeblock" id="code">
 <h3>บันทึกภาพย่อ JPEG ของอินสแตนซ์ - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p><code>RetrieveRenderedInstanceAsync</code> และ <code>RetrieveRenderedFramesAsync</code> คืนค่าภาพขนาดเต็มแทน. <code>DicomWebRenderOptions</code> ส่งการควบคุมการเรนเดอร์มาตรฐานไปยังเซิร์ฟเวอร์: คุณภาพของภาพ, ขนาด viewport, หน้าต่างและ ICC profile.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="อัปโหลดอินสแตนซ์ด้วย STOW-RS">}}

<p>STOW-RS ส่งอินสแตนซ์จำนวนใดก็ได้ในคำขอเดียว. เซิร์ฟเวอร์ตอบกลับด้วยรายงานที่ระบุ, อินสแตนซ์ต่ออินสแตนซ์, ว่าเก็บอะไรไว้และอะไรที่ถูกปฏิเสธ.</p>

<div class="codeblock" id="code">
 <h3>อัปโหลดโฟลเดอร์ของไฟล์ DICOM - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>บางเซิร์ฟเวอร์ตอบกลับด้วยข้อผิดพลาด HTTP, เช่น 409, เมื่อหนึ่งหรือหลายอินสแตนซ์ล้มเหลว. การเรียกนั้นจะโยน <code>DicomWebClientException</code>, ตามที่ส่วนต่อไปแสดง, และรายงานเดียวกันอยู่ใน body ของข้อยกเว้น.</p>

<p><code>StoreStudyAsync</code> อัปโหลดเข้าไปในการศึกษาที่มีชื่อตำแหน่งเดียว. ทั้งสองเมธอดยังรับ <code>IAsyncEnumerable&lt;DicomFile&gt;</code> ด้วย, ดังนั้นไฟล์สามารถผลิตขณะคำขอกำลังส่ง.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="จัดการข้อผิดพลาดของเซิร์ฟเวอร์">}}

<p>แต่ละการดำเนินการที่กำหนดประเภทจะตรวจสอบสถานะ HTTP ก่อนคืนค่า. เมื่อเซิร์ฟเวอร์ปฏิเสธคำขอ, การเรียกจะโยน <code>DicomWebClientException</code>, ซึ่งบรรจุรหัสสถานะ, ส่วนหัวและส่วนเริ่มต้นของ body การตอบกลับสำหรับบันทึก.</p>

<div class="codeblock" id="code">
 <h3>รายงานคำขอที่ล้มเหลว - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>เมธอดที่คืนค่าการตอบกลับดิบ, เช่นภาพย่อด้านบน, ให้คุณทำการตรวจสอบสถานะด้วยตนเองผ่าน <code>IsSuccessStatusCode</code> หรือ <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="คุณสมบัติเพิ่มเติมที่ไคลเอนท์ครอบคลุม">}}

<p>การค้นหา, ดาวน์โหลดและอัปโหลดเป็นการเรียกใช้ทั่วไป. ไคลเอนท์มีกลุ่มการทำงานสำหรับบริการ DICOMweb อื่น ๆ อีกเช่น:</p>

<ul>
<li>ข้อมูล bulk, ข้อมูลพิกเซลและเฟรมที่เลือกของอินสแตนซ์, ตามหมายเลขเฟรมที่เริ่มจากหนึ่ง, ตามที่เซิร์ฟเวอร์ส่งมา</li>
<li>WADO-URI, วิธีการดึงข้อมูลแบบ URL เดียวแบบเก่าที่โปรแกรมดูและคลังข้อมูลหลายแห่งยังใช้, สำหรับไฟล์ DICOM และภาพที่เรนเดอร์</li>
<li>UPS-RS worklists: สร้าง, ค้นหา, ปรับปรุงและอ้างสิทธิ์ workitem, เปลี่ยนสถานะ, ขอยกเลิก, และสมัครรับเหตุการณ์ที่มาถึงผ่าน WebSocket</li>
<li>รายการงานโมดาลิตี้ (MWL-RS), ขั้นตอนกระบวนการที่ทำโดยโมดาลิตี้ (MPPS-RS) และการมอบหมายการจัดเก็บ</li>
<li>อินสแตนซ์ที่ไม่ใช่ผู้ป่วยเช่น พาเล็ตสีและโปรโตคอลแสดงผล, รวมถึงเอกสารความสามารถของเซิร์ฟเวอร์</li>
<li><code>SendAsync</code> สำหรับการทำธุรกรรมมาตรฐานใด ๆ ด้วยค่า route, พารามิเตอร์ query, ส่วนหัวและ body ของคุณเอง, เมื่อตัวเมธอดที่กำหนดประเภทไม่เหมาะสม</li>
</ul>

<p>คู่มือไคลเอนท์ DICOMweb <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb client guide</a> ให้รายละเอียดทุกการดำเนินการและตัวเลือก, และ <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">client operation reference</a> แสดงรายการทุกเมธอด.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ไลบรารีเดียวจากการตอบสนอง HTTP ถึงข้อมูลพิกเซล">}}

<p>ไฟล์ที่ดาวน์โหลดด้วยไคลเอนท์เป็น <code>DicomFile</code> ปกติ. สามารถทำการละบังความเป็นส่วนตัว, แปลงรูปแบบ, เรนเดอร์หรือทำการ serialize ในกระบวนการเดียวกัน, โดยไม่มีขั้นตอนแปลงและไม่มีไลบรารีที่สองในสายงาน.</p>

<p>หน้าที่เกี่ยวข้อง: <a href="/medical/net/dicomweb-server/">เซิร์ฟเวอร์ DICOMweb</a> สำหรับด้านตรงข้ามของโปรโตคอลในบริการ ASP.NET Core ของคุณ, <a href="/medical/net/dicom-networking/">เครือข่าย DICOM</a> สำหรับคลังข้อมูลที่สื่อสารด้วย DIMSE แทน HTTP, <a href="/medical/net/anonymization/">การลับข้อมูล</a> ก่อนการศึกษาจากโรงพยาบาล, <a href="/medical/net/dicom-transfer-syntax-conversion/">การแปลงรูปแบบการส่งข้อมูล</a> สำหรับสิ่งที่เซิร์ฟเวอร์จัดเก็บ, และ <a href="/medical/net/dicom-to-json/">DICOM เป็น JSON</a> สำหรับรูปแบบที่ผลการค้นหา QIDO-RS ใช้.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือผู้พัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนแบบชำระเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องเลือก Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายการลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
