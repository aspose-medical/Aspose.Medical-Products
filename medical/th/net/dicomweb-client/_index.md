---
title: ไคลเอนต์ DICOMweb สำหรับ C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: ค้นหา, ดาวน์โหลดและอัปโหลดการศึกษา DICOM ผ่าน HTTP จาก .NET ไคลเอนต์ DICOMweb ที่เขียนด้วย C# แท้สำหรับ QIDO-RS, WADO-RS และ STOW-RS ซึ่งทำงานร่วมกับ PACS, VNA และบริการภาพคลาวด์
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="ไคลเอนต์ DICOMweb สำหรับ .NET C#" h2="ค้นหา, ดาวน์โหลดและอัปโหลดการศึกษาใด ๆ บนเซิร์ฟเวอร์ DICOMweb ด้วย QIDO-RS, WADO-RS และ STOW-RS ใน C# ที่จัดการได้โดยไม่ต้องติดตั้งอะไรบนเครื่อง" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM ผ่าน HTTP จากโค้ดของคุณเอง">}}

<p>DICOMweb คือรูปแบบ HTTP ของ DICOM แทนการเชื่อมต่อ DIMSE ที่พอร์ต 104 แอปพลิเคชันของคุณจะส่งคำขอ HTTPS ธรรมดา: QIDO-RS เพื่อค้นหา, WADO-RS เพื่อดาวน์โหลด, STOW-RS เพื่ออัปโหลด บริการภาพคลาวด์เช่น Azure Health Data Services DICOM service และ Google Cloud Healthcare API ทำงานในลักษณะนี้ และคลังข้อมูลเช่น Orthanc และ dcm4chee ก็ทำเช่นเดียวกัน</p>

<p><strong>Aspose.Medical for .NET</strong> มีไคลเอนต์ DICOMweb ตั้งแต่เวอร์ชัน 26.10 <code>DicomWebClient</code> แร็ปทุกการทำธุรกรรมมาตรฐานในเมธอดแบบมีประเภท และผลลัพธ์จะมาในรูปแบบอ็อบเจ็กต์ <code>Dataset</code> และ <code>DicomFile</code> เดียวกันกับที่ไลบรารีส่วนอื่นอ่านและเขียน เซิร์ฟเวอร์ฝั่งตรงข้ามอาจเป็นการนำไปใช้ DICOMweb ใด ๆ ก็ได้; ไม่จำเป็นต้องสร้างด้วย Aspose.Medical</p>

<p>ไคลเอนต์เป็น C# ที่จัดการบน <code>HttpClient</code> ไม่มีชุดเครื่องมือเนทีฟและไม่มีบริการพร็อกซี ดังนั้นโค้ดเดียวกันจึงทำงานบน Windows, Linux และในคอนเทนเนอร์ได้</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เชื่อมต่อกับเซิร์ฟเวอร์ DICOMweb">}}

<p>สร้างไคลเอนต์จาก <code>HttpClient</code> และ URL รากของบริการของเซิร์ฟเวอร์ การรับรองตัวตน การตั้งค่า TLS พร็อกซี และการจัดคิวการเชื่อมต่อทั้งหมดอยู่บน <code>HttpClient</code> ซึ่งแอปพลิเคชันของคุณได้กำหนดค่าไว้แล้ว ไคลเอนต์ไม่ได้เป็นเจ้าของ <code>HttpClient</code> และไม่ได้ทำการ dispose ตัวมัน</p>

<div class="codeblock" id="code">
 <h3>สร้างไคลเอนต์ DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>หากการปรับใช้หนึ่งตัวให้บริการ WADO-URI หรือบริการอื่นจาก URL ที่แตกต่าง <code>ServiceBaseUris</code> จะเขียนทับรากสำหรับบริการนั้นเท่านั้น</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ค้นหาการศึกษาโดยใช้ QIDO-RS">}}

<p>การค้นหาต้องการพารามิเตอร์การจับคู่โดยใช้คีย์เวิร์ด DICOM เป็นชื่อ, ฟิลด์ที่ต้องการคืนค่า, และการควบคุมการแบ่งหน้า ผลลัพธ์ที่ตรงกันจะมาสู่เป็นสตรีมของ datasets, อ่านทีละรายการขณะยังคงดาวน์โหลดการตอบกลับ</p>

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

<p><code>SearchSeriesAsync</code> และ <code>SearchInstancesAsync</code> ค้นหาลึกลงหนึ่งระดับ ทั้งในคลังทั้งหมดหรือภายในการศึกษา หรือซีรีส์เดียว ตัวเลือกเดียวกันยังสามารถตั้งค่าการจับคู่แบบ fuzzy, การจับคู่ค่าว่าง, และการเยื้องของผลลัพธ์สำหรับการแบ่งหน้า</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ดาวน์โหลดการศึกษาด้วย WADO-RS">}}

<p>การศึกษาอาจมีหลายพันอินสแตนซ์ <code>RetrieveStudyAsync</code> คืนค่าพวกมันเป็นสตรีม ดังนั้นแต่ละไฟล์จะถูกแยกวิเคราะห์เมื่อส่วนของการตอบกลับมาถึงและสามารถบันทึกได้ก่อนที่ไฟล์ถัดไปจะถูกอ่าน การใช้หน่วยความจำจะไม่เพิ่มขึ้นตามขนาดของการศึกษา</p>

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

<p><code>RetrieveSeriesAsync</code> และ <code>RetrieveInstanceAsync</code> ดาวน์โหลดหนึ่งซีรีส์หรือหนึ่งอินสแตนซ์ในลักษณะเดียวกัน</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="รับภาพย่อหรือภาพที่เรนเดอร์">}}

<p>ผู้ดูหรือหน้าจอ worklist มักต้องการรูปภาพ ไม่ใช่ไฟล์ DICOM เซิร์ฟเวอร์ทำการเรนเดอร์และส่งเป็น JPEG หรือ PNG ดังนั้นไคลเอนต์จึงไม่ต้องดาวน์โหลดหรือถอดรหัสข้อมูลพิกเซลดั้งเดิม</p>

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

<p><code>RetrieveRenderedInstanceAsync</code> และ <code>RetrieveRenderedFramesAsync</code> จะคืนค่าภาพขนาดเต็มแทน <code>DicomWebRenderOptions</code> ส่งการควบคุมการเรนเดอร์มาตรฐานไปยังเซิร์ฟเวอร์: คุณภาพภาพ, ขนาด viewport, หน้าต่างและ ICC profile</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="อัปโหลดอินสแตนซ์ด้วย STOW-RS">}}

<p>STOW-RS ส่งอินสแตนซ์จำนวนใดก็ได้ในคำขอเดียว เซิร์ฟเวอร์ตอบกลับด้วยรายงานที่แสดงรายการ อินสแตนซ์ต่ออินสแตนซ์ว่าอะไรที่ถูกจัดเก็บและอะไรที่ถูกปฏิเสธ</p>

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

<p>บางเซิร์ฟเวอร์อาจตอบด้วยข้อผิดพลาด HTTP เช่น 409 เมื่อหนึ่งหรือหลายอินสแตนซ์ล้มเหลว การเรียกนั้นจึงโยน <code>DicomWebClientException</code> ตามที่ส่วนต่อไปแสดง และรายงานเดียวกันจะอยู่ใน body ของข้อยกเว้น</p>

<p><code>StoreStudyAsync</code> อัปโหลดเข้าไปยังการศึกษาที่กำหนดชื่อ ทั้งสองเมธอดยังรับ <code>IAsyncEnumerable&lt;DicomFile&gt;</code> ด้วย ดังนั้นไฟล์สามารถสร้างขึ้นขณะส่งคำขอได้</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="จัดการข้อผิดพลาดของเซิร์ฟเวอร์">}}

<p>แต่ละโอเปอร์เรชันแบบมีประเภทตรวจสอบสถานะ HTTP ก่อนคืนค่า เมื่อเซิร์ฟเวอร์ปฏิเสธคำขอ การเรียกจะโยน <code>DicomWebClientException</code> ซึ่งบรรจุรหัสสถานะ, headers และส่วนเริ่มของ response body สำหรับบันทึก</p>

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

<p>เมธอดที่คืนค่าการตอบสนองดิบ เช่น ภาพย่อด้านบน จะปล่อยให้คุณตรวจสอบสถานะด้วย <code>IsSuccessStatusCode</code> หรือ <code>EnsureSuccessStatusCode</code></p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="คุณลักษณะอื่น ๆ ที่ไคลเอนต์ครอบคลุม">}}

<p>การค้นหา, ดาวน์โหลดและอัปโหลดเป็นการเรียกใช้ทั่วไป ไคลเอนต์มีกลุ่มโอเปอร์เรชันสำหรับบริการ DICOMweb อื่น ๆ อีก:</p>

<ul>
<li>ข้อมูล Bulk, ข้อมูลพิกเซล และเฟรมที่เลือกของอินสแตนซ์ ตามหมายเลขเฟรมแบบ 1‑based ตามที่เซิร์ฟเวอร์ส่ง</li>
<li>WADO-URI การเรียกคืนแบบ URL เดี่ยวแบบเก่าที่ผู้ชมและคลังข้อมูลหลายแห่งยังใช้ สำหรับไฟล์ DICOM และภาพที่เรนเดอร์</li>
<li>UPS‑RS worklists: สร้าง, ค้นหา, อัปเดตและรับงาน (workitems), เปลี่ยนสถานะ, ขอยกเลิก, และสมัครรับเหตุการณ์ที่มาผ่าน WebSocket</li>
<li>Modality worklist (MWL‑RS), ขั้นตอนกระบวนการที่ทำโดยโมดาลิตี้ (MPPS‑RS) และการมอบหมายการจัดเก็บ</li>
<li>อินสแตนซ์ที่ไม่เกี่ยวกับผู้ป่วย เช่น พาเลตสีและโปรโตคอลการแสดงผล, รวมถึงเอกสารความสามารถของเซิร์ฟเวอร์</li>
<li><code>SendAsync</code> สำหรับธุรกรรมมาตรฐานใด ๆ ด้วยค่า route ของคุณเอง, พารามิเตอร์ query, headers และ body, เมื่อเมธอดแบบมีประเภทไม่เหมาะสม</li>
</ul>

<p><a href=\"https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/\">คู่มือไคลเอนต์ DICOMweb</a> บันทึกรายการทุกโอเปอร์เรชันและตัวเลือก, และ <a href=\"https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/\">อ้างอิงการทำงานของไคลเอนต์</a> แสดงรายการเมธอดทั้งหมด</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ไลบรารีเดียวจากการตอบสนอง HTTP ถึงข้อมูลพิกเซล">}}

<p>ไฟล์ที่ดาวน์โหลดด้วยไคลเอนต์เป็น <code>DicomFile</code> ปกติ สามารถทำการไม่ระบุตัวตน, แปลงรหัส, เรนเดอร์ หรือซีเรียลไลซ์ในกระบวนการเดียวโดยไม่มีขั้นตอนการแปลงและไม่มีไลบรารีที่สองในโซ่</p>

<p>หน้าที่เกี่ยวข้อง: <a href=\"/medical/net/dicom-networking/\">DICOM networking</a> สำหรับคลังข้อมูลที่สื่อสารด้วย DIMSE แทน HTTP, <a href=\"/medical/net/anonymization/\">anonymization</a> ก่อนการศึกษาออกจากโรงพยาบาล, <a href=\"/medical/net/dicom-transfer-syntax-conversion/\">transfer syntax conversion</a> สำหรับสิ่งที่เซิร์ฟเวอร์จัดเก็บ, และ <a href=\"/medical/net/dicom-to-json/\">DICOM to JSON</a> สำหรับรูปแบบที่ผลการค้นหา QIDO-RS ใช้</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือผู้พัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบชำระค่าใช้จ่าย" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องใช้ Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
