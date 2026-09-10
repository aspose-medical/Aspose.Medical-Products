---
title: แปลง DICOM เป็น XML ใน C# .NET | Aspose.Medical
weight: 3000
description: ทำการซีเรียลไลซ์ชุดข้อมูล DICOM เป็นรูปแบบ DICOM XML มาตรฐานใน C# .NET กำหนดการจัดการ Bulk Data, การประมวลผลแบบสตรีม, และการทำงานแบบอะซิงโครนัสด้วย Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="แปลง DICOM เป็น XML ใน .NET C#" h2="ทำการซีเรียลไลซ์ชุดข้อมูล DICOM ให้เป็นการแสดงผล DICOM XML มาตรฐาน (PS3.19) กำหนดการอ้างอิง Bulk Data, การส่งออกแบบสตรีม, และการประมวลผลแบบอะซิงโครนัสด้วยไลบรารี .NET แท้" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="การซีเรียลไลซ์ DICOM XML ตามมาตรฐาน">}}

<p><strong>Aspose.Medical for .NET</strong> ซีรียลไลซ์ข้อมูล DICOM เป็น XML ตาม <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a>. นี่คือมาตรฐานอย่างเป็นทางการสำหรับการแสดงชุดข้อมูล DICOM ในรูปแบบ XML ใช้โดยบริการ DICOMweb, แพลตฟอร์มการบูรณาการ, และระบบที่ต้องการการแสดงผลที่อ่านง่ายโดยมนุษย์และตรวจสอบตามสคีมาของเมทาดาต้าภาพการแพทย์</p>

<p>คลาส <code>DicomXmlSerializer</code> ให้เมธอดแบบ static สำหรับการซีเรียลไลซ์และดีซีเรียลไลซ์ ทั้งสอง วิธีการ ไม่เหมือนกับการดัมพ์แท็กแบบง่าย ผลลัพธ์สอดคล้องกับสคีมา DICOM XML ซึ่งแต่ละองค์ประกอบจะแสดงด้วยแท็ก, VR, และค่าที่จัดรูปแบบอย่างถูกต้อง &mdash; ทำให้การแปลงรอบกลับแบบไม่มีการสูญเสียระหว่าง DICOM ไบนารีและ XML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ซีเรียลไลซ์ DICOM เป็น XML ใน C#">}}

<p>ใช้คลาส <code>DicomXmlSerializer</code> เพื่อแปลงชุดข้อมูล DICOM เป็นสตริง XML วิธีที่ง่ายที่สุดจะสร้างเอกสาร XML ที่สอดคล้องกับมาตรฐาน:</p>

<div class="codeblock" id="code">
 <h3>แปลง DICOM เป็น XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การซีเรียลไลซ์แบบสตรีมและแบบอะซิงโครนัส">}}

<p>สำหรับไฟล์ DICOM ขนาดใหญ่หรือกรณีการทำงานที่มีอัตราเร็วสูง ให้ทำการซีเรียลไลซ์โดยตรงไปยังสตรีมเพื่อหลีกเลี่ยงการจัดสรรสตริงขนาดใหญ่ในหน่วยความจำ มีเมธอดแบบ synchronous และ async ให้ใช้ได้:</p>

<div class="codeblock" id="code">
 <h3>การซีเรียลไลซ์สตรีมแบบ synchronous - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>การซีเรียลไลซ์สตรีมแบบ async - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การสตรีมแบบ Pipeline สำหรับ Study ขนาดใหญ่">}}

<p>Study ทั้งหมดไม่จำเป็นต้องโหลดทั้งหมดในหน่วยความจำ <code>DicomXmlSerializer</code> เขียนไปยัง <code>PipeWriter</code> และอ่านจาก <code>PipeReader</code> ทำให้ XML สามารถสร้างและใช้ได้ตามที่ไหล และชุดข้อมูลลำดับต่อเนื่องสามารถอ่านทีละชุดผ่าน <code>DeserializeAsyncEnumerable</code> ทุกเมธอดรับพารามิเตอร์ <code>CancellationToken</code></p>

<div class="codeblock" id="code">
 <h3>ซีเรียลไลซ์และดีซีเรียลไลซ์ผ่าน pipe - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>อ่านลำดับชุดข้อมูลทีละหนึ่ง - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ตัวเลือกการซีเรียลไลซ์">}}

<p>คลาส <code>DicomXmlSerializerOptions</code> ควบคุมการแสดงผลข้อมูล DICOM ใน XML การกำหนดค่าหลักคือการจัดการ Bulk Data สำหรับค่าที่ย่อยขนาดใหญ่:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>คุณสมบัติ</th>
<th>ประเภท</th>
<th>รายละเอียด</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>ตัวแปลงแบบกำหนดเองสำหรับเขียนข้อมูลขนาดใหญ่ (เช่น pixel data) เป็นอ้างอิง BulkData URI แทนการฝังใน XML</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>ตัวโหลดแบบกำหนดเองสำหรับแก้ไข BulkData URI ระหว่างการดีซีเรียลไลซ์</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>อินสแตนซ์ตัวเลือกเริ่มต้นที่ใช้เมื่อไม่มีการระบุตัวเลือกแบบกำหนดเอง</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>ซีเรียลไลซ์ด้วยตัวเลือกแบบกำหนดเอง - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การจัดการ Bulk Data">}}

<p>ค่าที่ย่อยขนาดใหญ่ (pixel data, waveforms, เอกสารที่บรรจุ) สามารถทำเป็นอ้างอิง BulkData URI แทนการฝังในผลลัพธ์ XML ได้ การนี้สอดคล้องกับข้อกำหนด <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">DICOM PS3.19 BulkData element</a></p>

<p>ทำการ implement <code>IBulkDataConverter</code> เพื่อทำให้ข้อมูลขนาดใหญ่เป็น Externalized ระหว่างการซีเรียลไลซ์ และ <code>IBulkDataLoader</code> เพื่อแก้ไข URI ระหว่างการดีซีเรียลไลซ์ ในกรณีทั่วไปไม่จำเป็นต้องเขียน loader เอง: <code>DefaultBulkDataLoader.Instance</code> สามารถ resolve URI ประเภท <code>file</code>, <code>http</code> และ <code>https</code> และยัง implement <code>IAsyncBulkDataLoader</code> ทำให้ Bulk Data ถูกดึงแบบอะซิงโครนัสในเส้นทางสตรีม</p>

<div class="codeblock" id="code">
 <h3>การจัดการ Bulk Data แบบกำหนดเอง - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
public class FileBulkDataConverter : IBulkDataConverter
{
    public string? GetBulkDataUri(IElement element)
    {
        // Externalize pixel data to a separate file
        if (element.Tag == Tag.PixelData)
            return "file:///bulk/pixeldata.raw";
        return null; // inline all other elements
    }
}

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ดีซีเรียลไลซ์ XML เป็น DICOM">}}

<p>แปลง DICOM XML กลับเป็นอ็อบเจ็กต์ Dataset รองรับการป้อนข้อมูลแบบสตริง, สตรีม, และการทำงานแบบอะซิงโครนัส:</p>

<div class="codeblock" id="code">
 <h3>ดีซีเรียลไลซ์ XML เป็น DICOM - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การซีเรียลไลซ์ XML vs JSON">}}

<p>Aspose.Medical รองรับการซีเรียลไลซ์ทั้ง DICOM XML (PS3.19) และ DICOM JSON (PS3.18) ทั้งสองรูปแบบให้การแปลงรอบกลับแบบไม่มีการสูญเสีย แต่เหมาะกับสถานการณ์การบูรณาการที่แตกต่างกัน:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>คุณลักษณะ</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>มาตรฐาน</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>การตรวจสอบสคีมา</td><td>XML Schema (XSD) available</td><td>No formal schema</td></tr>
<tr><td>เหมาะสำหรับ</td><td>Enterprise integration, HL7 CDA, audit logs, XDS registries</td><td>DICOMweb, REST APIs, FHIR ImagingStudy</td></tr>
<tr><td>ความอ่านง่ายของมนุษย์</td><td>Verbose but self-describing</td><td>Compact and widely supported</td></tr>
<tr><td>ข้อมูล Bulk</td><td>BulkData element with URI</td><td>BulkDataURI property</td></tr>
<tr><td>คลาส Serializer</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสาร" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="ซอร์สโค้ด" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนแบบจ่ายเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องเลือก Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
