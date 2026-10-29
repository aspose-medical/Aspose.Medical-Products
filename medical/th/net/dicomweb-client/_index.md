---
title: ไคลเอนต์ DICOMweb ใน C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: ค้นหา ดาวน์โหลด และอัปโหลดการศึกษา DICOM ผ่าน HTTP จาก .NET. ไคลเอนต์ DICOMweb ที่เขียนด้วย C# อย่างเดียวสำหรับ QIDO-RS, WADO-RS และ STOW-RS ที่ทำงานร่วมกับ PACS, VNA และบริการภาพคลาวด์.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="ไคลเอนต์ DICOMweb สำหรับ .NET C#" h2="ค้นหา ดาวน์โหลด และอัปโหลดการศึกษาในเซิร์ฟเวอร์ DICOMweb ใดๆ ด้วย QIDO-RS, WADO-RS และ STOW-RS ใน C# ที่จัดการได้โดยไม่ต้องติดตั้งอะไรบนเครื่อง." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM ผ่าน HTTP จากโค้ดของคุณเอง">}}

<p>DICOMweb คือรูปแบบ HTTP ของ DICOM. แทนการเชื่อมต่อ DIMSE บนพอร์ต 104, แอปพลิเคชันของคุณจะส่งคำขอ HTTPS ปกติ: QIDO-RS เพื่อค้นหา, WADO-RS เพื่อดาวน์โหลด, STOW-RS เพื่ออัปโหลด. บริการภาพคลาวด์เช่น Azure Health Data Services DICOM service และ Google Cloud Healthcare API ทำงานในลักษณะนี้, รวมถึงคลังข้อมูลเช่น Orthanc และ dcm4chee ด้วย.</p>

<p><strong>Aspose.Medical for .NET</strong> มีไคลเอนต์ DICOMweb ตั้งแต่เวอร์ชัน 26.10. <code>DicomWebClient</code> ห่อหุ้มการทำธุรกรรมมาตรฐานทั้งหมดในเมธอดที่มีชนิดกำหนด, และผลลัพธ์จะส่งกลับเป็นอ็อบเจกต์ <code>Dataset</code> และ <code>DicomFile</code> ที่ไลบรารีส่วนอื่นอ่านและเขียน. เซิร์ฟเวอร์ฝั่งตรงข้ามสามารถเป็นการนำเข้า DICOMweb ใดก็ได้; ไม่จำเป็นต้องสร้างด้วย Aspose.Medical.</p>

<p>ไคลเอนต์เป็น C# ที่จัดการบน <code>HttpClient</code>. ไม่มีชุดเครื่องมือเนทีฟและไม่มีบริการพร็อกซี, ดังนั้นโค้ดเดียวกันทำงานได้บน Windows, Linux และในคอนเทนเนอร์.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เชื่อมต่อกับเซิร์ฟเวอร์ DICOMweb">}}

<p>สร้างไคลเอนต์จาก <code>HttpClient</code> และ URL รากของบริการของเซิร์ฟเวอร์. การตรวจสอบสิทธิ์, การตั้งค่า TLS, พร็อกซีและการจัดการการเชื่อมต่อยังคงอยู่บน <code>HttpClient</code>, ที่แอปพลิเคชันของคุณได้กำหนดค่าไว้แล้ว. ไคลเอนต์ไม่เป็นเจ้าของ <code>HttpClient</code> และไม่ทำการ disposing.</p>

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

<p>หากการปรับใช้หนึ่งให้บริการ WADO-URI หรือบริการอื่นจาก URL ที่แตกต่าง, <code>ServiceBaseUris</code> จะครอบคลุมรากสำหรับบริการนั้นเท่านั้น.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ค้นหาการศึกษาด้วย QIDO-RS">}}

<p>การค้นหารับพารามิเตอร์ที่ตรงกันโดยใช้คีย์เวิร์ด DICOM เป็นชื่อ, ระบุฟิลด์ที่ต้องการส่งคืน, และควบคุมการแบ่งหน้า. ผลการจับคู่จะมาที่เป็นสตรีมของ dataset, อ่านทีละชุดขณะตอบกลับยังคงดาวน์โหลดอยู่.</p>

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

<p><code>SearchSeriesAsync</code> และ <code>SearchInstancesAsync</code> ค้นหาตำแหน่งหนึ่งระดับลง, ทั้งทั่วคลังข้อมูลหรือภายในการศึกษา หรือ ซีรีส์เดียว. ตัวเลือกเดียวกันยังสามารถตั้งค่าการจับคู่แบบ fuzziness, การจับคู่ค่าว่าง, และการชดเชยผลลัพธ์สำหรับการแบ่งหน้า.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ดาวน์โหลดการศึกษาด้วย WADO-RS">}}

<p>การศึกษาอาจมีหลายพันอินสแตนซ์. <code>RetrieveStudyAsync</code> ส่งคืนเป็นสตรีม, ดังนั้นแต่ละไฟล์จะถูกวิเคราะห์เมื่อส่วนของการตอบกลับมาถึงและสามารถบันทึกได้ก่อนที่ไฟล์ถัดไปจะอ่าน. การใช้หน่วยความจำจะไม่เพิ่มตามขนาดของการศึกษา.</p>

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

<p><code>RetrieveSeriesAsync</code> และ <code>RetrieveInstanceAsync</code> ดาวน์โหลดซีรีส์หรืออินสแตนซ์หนึ่งในแบบเดียวกัน.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="รับภาพย่อหรือภาพที่เรนเดอร์">}}

<p>ตัวดูหรือหน้าจอรายการงานมักต้องการรูปภาพ ไม่ใช่ไฟล์ DICOM. เซิร์ฟเวอร์ทำการเรนเดอร์และส่ง JPEG หรือ PNG, ทำให้ไคลเอนต์ไม่ต้องดาวน์โหลดหรือถอดรหัสข้อมูลพิกเซลต้นฉบับ.</p>

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

<p><code>RetrieveRenderedInstanceAsync</code> และ <code>RetrieveRenderedFramesAsync</code> จะส่งคืนภาพเต็มขนาดแทน. <code>DicomWebRenderOptions</code> ส่งผ่านการควบคุมการเรนเดอร์มาตรฐานไปยังเซิร์ฟเวอร์: คุณภาพภาพ, ขนาด viewport, window และ ICC profile.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="อัปโหลดอินสแตนซ์ด้วย STOW-RS">}}

<p>STOW-RS ส่งอินสแตนซ์จำนวนใดก็ได้ในคำขอเดียว. เซิร์ฟเวอร์ตอบด้วยรายงานที่แสดงรายการโดยอินสแตนซ์ว่าอะไรถูกจัดเก็บและอะไรถูกปฏิเสธ.</p>

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

<p>เซิร์ฟเวอร์บางตัวอาจตอบกลับด้วยข้อผิดพลาด HTTP เช่น 409 เมื่อหนึ่งหรือหลายอินสแตนซ์ล้มเหลว. การเรียกนั้นจะโยน <code>DicomWebClientException</code> ตามที่ส่วนถัดไปแสดง, และรายงานเดียวกันอยู่ใน body ของการตอบกลับในข้อยกเว้น.</p>

<p><code>StoreStudyAsync</code> อัปโหลดเข้าสู่การศึกษาที่กำหนดชื่อหนึ่ง. ทั้งสองเมธอดยังรับ <code>IAsyncEnumerable&lt;DicomFile&gt;</code> ได้, ทำให้ไฟล์สามารถสร้างได้ขณะส่งคำขอ.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="จัดการข้อผิดพลาดของเซิร์ฟเวอร์">}}

<p>การทำงานที่ระบุชนิดแต่ละอย่างจะตรวจสอบสถานะ HTTP ก่อนคืนค่า. เมื่อเซิร์ฟเวอร์ปฏิเสธคำขอ, การเรียกจะโยน <code>DicomWebClientException</code>, ซึ่งบรรจุรหัสสถานะ, เฮดเดอร์และส่วนต้นของ body การตอบกลับสำหรับบันทึก.</p>

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

<p>เมธอดที่ส่งคืนการตอบสนองดิบ เช่น ภาพย่อยด้านบน, ให้คุณทำการตรวจสอบสถานะด้วย <code>IsSuccessStatusCode</code> หรือ <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="สิ่งอื่นที่ไคลเอนต์ครอบคลุม">}}

<p>การค้นหา, ดาวน์โหลดและอัปโหลดเป็นการเรียกใช้งานทั่วไป. ไคลเอนต์มีกลุ่มการทำงานสำหรับบริการ DICOMweb อื่นๆ ด้วย:</p>

<ul>
<li>ข้อมูลแบบ bulk, ข้อมูลพิกเซลและเฟรมที่เลือกของอินสแตนซ์ตามหมายเลขเฟรมที่เริ่มจาก 1, ตามที่เซิร์ฟเวอร์ส่งมา.</li>
<li>WADO-URI, การเรียกดึงข้อมูลด้วย URL เดียวรุ่นเก่าที่หลายตัวดูและคลังข้อมูลยังใช้, สำหรับไฟล์ DICOM และภาพที่เรนเดอร์.</li>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> worklists: สร้าง, ค้นหา, ปรับปรุงและอ้างสิทธิ์ workitems, เปลี่ยนสถานะ, ร้องขอยกเลิก, และสมัครรับเหตุการณ์ที่มาถึงผ่าน WebSocket.</li>
<li><a href="/medical/net/modality-worklist/">modality worklist</a> (MWL-RS), ขั้นตอนกระบวนการที่ทำโดย modality (MPPS-RS) และการคอมมิทการจัดเก็บ.</li>
<li>อินสแตนซ์ที่ไม่ใช่ผู้ป่วยเช่นพาเลตสีและโปรโตคอลการแสดงผล, รวมถึงเอกสารความสามารถของเซิร์ฟเวอร์.</li>
<li><code>SendAsync</code> สำหรับธุรกรรมมาตรฐานใดๆ ด้วย route values, พารามิเตอร์ query, เฮดเดอร์และ body ของคุณเอง, เมื่อเมธอดที่กำหนดชนิดไม่เหมาะสม.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">คู่มือไคลเอนต์ DICOMweb</a> บันทึกการทำงานและตัวเลือกทั้งหมด, และ <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">อ้างอิงการทำงานของไคลเอนต์</a> แสดงรายการเมธอดทั้งหมด.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ไลบรารีเดียวจากการตอบสนอง HTTP ไปจนถึงข้อมูลพิกเซล">}}

<p>ไฟล์ที่ดาวน์โหลดด้วยไคลเอนต์เป็น <code>DicomFile</code> ปกติ. สามารถทำการไม่เปิดเผยตัว, แปลงรูปแบบ, เรนเดอร์หรือซีเรียลไลซ์ในกระบวนการเดียว, โดยไม่มีขั้นตอนการแปลงและไม่มีไลบรารีอื่นในโซ่.</p>

<p>หน้าที่เกี่ยวข้อง: <a href="/medical/net/dicomweb-server/">เซิร์ฟเวอร์ DICOMweb</a> สำหรับฝั่งอื่นของโปรโตคอลในบริการ ASP.NET Core ของคุณ, <a href="/medical/net/dicom-networking/">DICOM networking</a> สำหรับคลังข้อมูลที่ใช้ DIMSE แทน HTTP, <a href="/medical/net/anonymization/">การไม่เปิดเผยตัว</a> ก่อนการศึกษาออกจากโรงพยาบาล, <a href="/medical/net/dicom-transfer-syntax-conversion/">การแปลง transfer syntax</a> สำหรับสิ่งที่เซิร์ฟเวอร์จัดเก็บ, และ <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> สำหรับรูปแบบผลการค้นหา QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารอธิบาย" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือผู้พัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบชำระเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้อง Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายการลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
