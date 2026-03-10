---
title: แปลง DICOM เป็น JSON ใน C# .NET | Aspose.Medical
weight: 4000
description: ทำการซีเรียลไลซ์ไฟล์ DICOM เป็นรูปแบบ DICOM JSON มาตรฐานใน C# .NET กำหนดการจัดการตัวเลข, URI ของ bulk data, การแสดงคีย์เวิร์ด, และการประมวลผลสตรีมแบบอะซิงค์ด้วย Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="แปลง DICOM เป็น JSON ใน .NET C#" h2="ทำการซีเรียลไลซ์ชุดข้อมูลและไฟล์ DICOM เป็นโมเดล DICOM JSON มาตรฐาน (PS3.18) กำหนดการจัดการตัวเลข, อ้างอิง bulk data, การแสดงคีย์เวิร์ด, และการประมวลผลแบบสตรีมแบบอะซิงค์" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="โมเดล DICOM JSON มาตรฐาน">}}

<p><strong>Aspose.Medical for .NET</strong> ทำการซีเรียลไลซ์ข้อมูล DICOM เป็น JSON ตาม <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a> นี่เป็นมาตรฐานอย่างเป็นทางการสำหรับการแทนชุดข้อมูล DICOM ในรูปแบบ JSON ซึ่งใช้โดยบริการ DICOMweb, ทรัพยากร FHIR ImagingStudy, และ API การดูแลสุขภาพสมัยใหม่</p>

<p>ในโมเดล DICOM JSON, แต่ละแอตทริบิวต์จะแสดงด้วยแท็กของมันเป็นคีย์ของ JSON (เช่น <code>"00100010"</code> สำหรับ Patient Name) พร้อมด้วย Value Representation และอาร์เรย์ของค่าเป็นคุณสมบัติที่ซ้อนกัน:</p>

<div class="codeblock" id="code">
 <h3>รูปแบบ DICOM JSON Model</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ทำการซีเรียลไลซ์ DICOM เป็น JSON ใน C#">}}

<p>ใช้คลาส <code>DicomJsonSerializer</code> เพื่อแปลงไฟล์หรือชุดข้อมูล DICOM เป็น JSON วิธีที่ง่ายที่สุดจะสร้างผลลัพธ์ที่กระชับและสอดคล้องกับมาตรฐาน:</p>

<div class="codeblock" id="code">
 <h3>แปลง DICOM เป็น JSON - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p>คุณสามารถทำการซีเรียลไลซ์ <code>Dataset</code>, <code>DicomFile</code> ที่สมบูรณ์ (รวมถึง File Meta Information) หรืออาร์เรย์ของชุดข้อมูลได้:</p>

<div class="codeblock" id="code">
 <h3>ซีเรียลไลซ์ DicomFile และอาร์เรย์ของ dataset - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การซีเรียลไลซ์แบบสตรีมและแบบอะซิงค์">}}

<p>สำหรับไฟล์ DICOM ขนาดใหญ่หรือกรณีใช้ความเร็วสูง, ทำการซีเรียลไลซ์โดยตรงไปยังสตรีม UTF-8 เพื่อหลีกเลี่ยงการจัดสรรสตริงขนาดใหญ่ในหน่วยความจำ ทั้งเมธอดแบบซิงโครนัสและอะซิงค์พร้อมให้ใช้:</p>

<div class="codeblock" id="code">
 <h3>การซีเรียลไลซ์แบบสตรีมและอะซิงค์ - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ตัวเลือกการซีเรียลไลซ์">}}

<p>คลาส <code>DicomJsonSerializerOptions</code> กำหนดวิธีการที่ข้อมูล DICOM จะแสดงในรูปแบบ JSON:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>คุณสมบัติ</th>
<th>ประเภท</th>
<th>คำอธิบาย</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>กำหนดว่าค่า VR ตัวเลข (IS, DS, SV, UV) จะถูกเขียนเป็นตัวเลข JSON หรือเป็นสตริง</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>ใช้คีย์เวิร์ด DICOM (เช่น <code>"PatientName"</code>) แทนแท็ก (<code>"00100010"</code>) เป็นคีย์ของ JSON หมายเหตุ: ไม่เป็นมาตรฐาน</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>รวมคีย์เวิร์ด DICOM เป็นแอตทริบิวต์ JSON เพิ่มเติม</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>รวมชื่อแท็ก DICOM เป็นแอตทริบิวต์ JSON เพิ่มเติม</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>คอนเวอร์เตอร์ที่กำหนดเองสำหรับการเขียนข้อมูลขนาดใหญ่ (เช่น pixel data) เป็นการอ้างอิง URI</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>โหลดเดอร์ที่กำหนดเองสำหรับแก้ไข URI ของ bulk data ในระหว่างการดีซีเรียลไลซ์</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>กำหนดค่าตัวเลือกการซีเรียลไลซ์ - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การจัดการตัวเลข">}}

<p>Value Representation ตัวเลขของ DICOM (IS, DS, SV, UV) สามารถถูกซีเรียลไลซ์เป็นตัวเลข JSON หรือสตริง JSON การทำเช่นนี้สำคัญต่อการทำงานร่วมกัน เนื่องจากบางการใช้งาน DICOM เก็บตัวเลขโดยมีช่องว่างนำหน้า หรือรูปแบบที่ไม่เป็นมาตรฐาน:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>โหมด</th>
<th>ผลลัพธ์ JSON</th>
<th>กรณีการใช้</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>สอดคล้องกับมาตรฐาน, เหมาะสำหรับการคำนวณ. จะโยนข้อยกเว้นหากค่ไม่สามารถแยกวิเคราะห์ได้.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>รักษาการแสดงผลสตริงเดิมไว้. ปลอดภัยมากขึ้นสำหรับการวนกลับข้อมูลที่ไม่เป็นมาตรฐาน.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การจัดการ Bulk Data">}}

<p>ค่าทีนฐานขนาดใหญ่ (pixel data, waveforms, เอกสารที่ห่อหุ้ม) สามารถจัดการเป็นการอ้างอิง bulk data แทนการฝังในผลลัพธ์ JSON ซึ่งสอดคล้องตามสเปค <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a></p>

<p>ดำเนินการ <code>IBulkDataConverter</code> เพื่อแยกข้อมูลขนาดใหญ่ออกระหว่างการซีเรียลไลซ์, และ <code>IBulkDataLoader</code> เพื่อแก้ไข URI ระหว่างการดีซีเรียลไลซ์:</p>

<div class="codeblock" id="code">
 <h3>การจัดการ bulk data กำหนดเอง - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ดีซีเรียลไลซ์ JSON เป็น DICOM">}}

<p>แปลง DICOM JSON กลับเป็นอ็อบเจ็กต์ Dataset หรือ DicomFile รองรับการรับข้อมูลเป็นสตริง, สตรีม, และการดำเนินการสตรีมแบบอะซิงค์:</p>

<div class="codeblock" id="code">
 <h3>ดีซีเรียลไลซ์ JSON เป็น DICOM - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การบูรณาการกับ System.Text.Json">}}

<p>Aspose.Medical มี <code>DatasetJsonConverter</code> และ <code>DicomFileJsonConverter</code> เพื่อบูรณาการกับไพป์ไลน์การซีเรียลไลซ์มาตรฐานของ <code>System.Text.Json</code> ซึ่งทำให้สามารถซีเรียลไลซ์อ็อบเจ็กต์ DICOM ควบคู่กับประเภท .NET อื่น ๆ ในโค้ดประมวลผล JSON ที่มีอยู่ของคุณได้:</p>

<div class="codeblock" id="code">
 <h3>การบูรณาการ System.Text.Json - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารอธิบาย" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="ซอร์สโค้ด" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบชำระค่าบริการ" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องเลือก Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
