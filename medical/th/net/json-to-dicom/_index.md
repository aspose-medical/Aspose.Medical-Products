---
title: แปลง JSON เป็น DICOM ใน C# .NET | Aspose.Medical
weight: 6000

description: สร้างไฟล์ DICOM จากโมเดล DICOM JSON มาตรฐาน (PS3.18) ใน C# .NET. อ่าน JSON จากสตริง, สตรีมหรือพายป์, สตรีมลำดับของชุดข้อมูล, และแก้ไขการอ้างอิง bulk data ด้วย Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="แปลง JSON เป็น DICOM ใน .NET C#" h2="อ่านโมเดล DICOM JSON มาตรฐาน (PS3.18) กลับเป็นชุดข้อมูลและไฟล์ DICOM. ทำงานจากสตริง, สตรีม หรือพายป์, สตรีมลำดับของ study, และแก้ไขการอ้างอิง bulk data." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="จาก DICOM JSON ไปยังไฟล์ DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> อ่าน <a href=\"https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html\">DICOM PS3.18 JSON Model</a>, ซึ่งเป็นรูปแบบที่ใช้โดยบริการ DICOMweb และระบบที่แลกเปลี่ยน study ผ่าน HTTP. สิ่งที่มาถึงเป็น JSON จะกลายเป็น <code>Dataset</code> และ <code>Dataset</code> จะถูกเขียนลงดิสก์เป็นไฟล์ DICOM.</p>

<p>นี่คือทิศทางตรงข้ามของหน้าที่ <a href=\"/medical/net/dicom-to-json/\">DICOM to JSON</a>, และทั้งสองใช้คลาสเดียวกันคือ <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>สร้างไฟล์ DICOM จาก JSON - C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>ชุดข้อมูลที่ไม่มี File Meta Information จะถูกเขียนด้วย transfer syntax เริ่มต้น Implicit VR Little Endian เมื่อมันถูกหุ้มไว้ใน <code>DicomFile</code>.</p>

<p>การอ่าน DICOM JSON เป็นฟีเจอร์ที่ต้องมีใบอนุญาต. หากไม่ได้ทำการใช้ใบอนุญาต on-premise ตัวอ่านจะโยน <code>MedicalApiException</code>, ดังนั้นให้ใช้ใบอนุญาตก่อน, ตามที่ <a href=\"https://docs.aspose.com/medical/net/getting-started/licensing/\">คู่มือการให้ใบอนุญาต</a> อธิบาย.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="คงไว้ File Meta Information">}}

<p><code>Deserialize</code> ส่งคืนชุดข้อมูลเพียงอย่างเดียว. เมื่อเอกสาร JSON ยังมีกลุ่ม File Meta Information ด้วย, ตัวอย่างเช่นเพราะถูกสร้างจากไฟล์ DICOM ฉบับเต็ม, <code>DeserializeFile</code> จะส่งคืน <code>DicomFile</code> ที่มีกลุ่มนั้นครบถ้วน, รวมถึง transfer syntax ที่ไฟล์ระบุ.</p>

<div class="codeblock" id="code">
 <h3>อ่านไฟล์ DICOM ฉบับเต็มจาก JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="สตรีม, พายป์ และ async">}}

<p>ทุกจุดเข้าถึงมี overload แบบสตรีมและแบบ asynchronous, และแบบ asynchronous ยังรับ <code>PipeReader</code> ด้วย. เอกสารที่มาจากการตอบสนองเว็บหรือจากดิสก์จะถูกอ่านโดยไม่ต้องแปลงเป็นสตริงก่อน, ซึ่งสำคัญเมื่อ JSON มีข้อมูลพิกเซล.</p>

<div class="codeblock" id="code">
 <h3>อ่าน JSON จากสตรีม - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ลำดับของชุดข้อมูล ทีละหนึ่งชุด">}}

<p>การสอบถาม DICOMweb จะตอบกลับด้วยอาร์เรย์ของชุดข้อมูล, และเอกสารเช่นนั้นอาจมีขนาดใหญ่. <code>DeserializeList</code> อ่านอาร์เรย์ทั้งหมดเข้าหน่วยความจำ; <code>DeserializeAsyncEnumerable</code> ส่งคืนชุดข้อมูลทีละหนึ่ง, ทำให้เอกสารไม่ต้องถูกเก็บไว้ทั้งหมด.</p>

<div class="codeblock" id="code">
 <h3>สตรีมอาร์เรย์ของชุดข้อมูล - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การอ้างอิง bulk data">}}

<p>โมเดล DICOM JSON ไม่ได้บรรจุข้อมูลพิกเซลในตัวเอง. ค่าขนาดใหญ่จะถูกแทนที่ด้วย <code>BulkDataURI</code> ที่ชี้ไปที่ไบต์, ทำให้เอกสาร JSON มีขนาดเล็ก. เพื่อแก้ไขการอ้างอิงเหล่านี้ขณะอ่าน, ให้ serializer มี bulk data loader. <code>DefaultBulkDataLoader</code> ดึง <code>file</code>, <code>http</code> และ <code>https</code> URI โดยไม่มีการตรวจสอบสิทธิ์; สำหรับคลังที่ต้องการข้อมูลรับรอง, ให้คุณทำการ implement <code>IBulkDataLoader</code> หรือ <code>IAsyncBulkDataLoader</code> เอง.</p>

<div class="codeblock" id="code">
 <h3>แก้ไข BulkDataURI ขณะอ่าน - C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="รอบการทำงานกลับกับ DICOM to JSON">}}

<p>ทั้งสองทิศทางออกแบบให้ใช้ร่วมกัน: study จะออกเป็น JSON, เดินทางผ่านเว็บเซอร์วิส, แล้วกลับมาเป็นไฟล์ DICOM. กระบวนการไม่มีส่วนที่พึ่งพาโค้ดเนทีฟ, ดังนั้นรอบการทำงานเดียวกันทำงานได้บน Windows, Linux และ macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM to JSON แล้วกลับ - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>สำหรับตัวเลือกที่ควบคุมรูปแบบของ JSON, ดูหน้าที่ <a href=\"/medical/net/dicom-to-json/\">DICOM to JSON</a>. คู่เดียวกันนี้มีสำหรับ XML: <a href=\"/medical/net/dicom-to-xml/\">DICOM to XML</a> และ <a href=\"/medical/net/xml-to-dicom/\">XML to DICOM</a>. <a href=\"https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/\">คู่มือการทำ JSON serialization</a> ครอบคลุม API ทั้งหมด.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารอ้างอิง" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือผู้พัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนแบบชำระเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องเลือก Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}