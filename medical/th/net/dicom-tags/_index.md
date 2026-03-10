---
title: อ่านและแก้ไขแท็ก DICOM ใน C# .NET | Aspose.Medical
weight: 15000
description: อ่าน, เพิ่ม, ปรับปรุงและลบแท็ก DICOM ใน C# .NET. เข้าถึงเมทาดาต้าแท็ก, ทำงานกับแท็กส่วนตัว, นับแท็กตามกลุ่ม, และจัดการลำดับโดยใช้ Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="อ่านและแก้ไขแท็ก DICOM ใน .NET C#" h2="เข้าถึง, เพิ่ม, ปรับปรุงและลบองค์ประกอบข้อมูล DICOM ด้วย API ที่ปลอดภัยต่อชนิดข้อมูล. ทำงานกับแท็กมาตรฐาน, แท็กส่วนตัว, ลำดับ, และพจนานุกรมแท็ก DICOM ทั้งหมด." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="การเข้าถึงแท็ก DICOM แบบปลอดภัยต่อชนิดข้อมูล">}}

<p><strong>Aspose.Medical for .NET</strong> ให้ API ที่ครอบคลุมและปลอดภัยต่อชนิดข้อมูลสำหรับทำงานกับแท็ก DICOM ผ่านคลาส <code>Dataset</code>. แต่ละองค์ประกอบข้อมูล DICOM แสดงโดย <code>Tag</code> &mdash; คู่ของหมายเลขกลุ่มและหมายเลของค์ประกอบที่ระบุตัวแปรอย่างเฉพาะเจาะจง. API นำเสนอการเข้าถึงแบบชนิดที่แน่นหนาพร้อมเมธอด generic, รูปแบบการดึงข้อมูลแบบปลอดภัยที่หลีกเลี่ยงข้อยกเว้น, และรองรับค่าตัวเดียว, อาเรย์, และการเข้าถึงแบบเชิงดัชนี.</p>

<p>แท็ก DICOM ที่พบบ่อยมีให้เป็นฟิลด์ static บนคลาส <code>Tag</code> (เช่น <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), ทำให้ไม่จำเป็นต้องจดจำรหัสตัวเลขของแท็ก. แต่ละแท็กมี <code>TagMetadata</code> ที่บรรจุคีย์เวิร์ด, คำอธิบาย, Value Representation (VR), จำนวนค่าที่เป็นไปได้, และสถานะการเลิกใช้.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="อ่านแท็ก DICOM ใน C#">}}

<p>คลาส <code>Dataset</code> มีเมธอดหลายแบบสำหรับดึงค่าของแท็กตามความต้องการของคุณ &mdash; ค่าตัวเดียว, ค่าทั้งหมด, การเข้าถึงแบบเชิงดัชนี, หรือการดึงข้อมูลแบบปลอดภัยพร้อมค่าเริ่มต้น:</p>

<div class="codeblock" id="code">
 <h3>อ่านค่าตัวแท็ก DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Get a single value (throws if tag is missing or multi-valued)
string patientName = dataset.GetSingleValue&lt;string&gt;(Tag.PatientName);

// Safe retrieval with a default value
string institution = dataset.GetSingleValueOrDefault&lt;string&gt;(
    Tag.InstitutionName, "Unknown");

// Get all values from a multi-valued tag
double[] pixelSpacing = dataset.GetValues&lt;double&gt;(Tag.PixelSpacing);

// Get a value by index (supports negative indexing with ^)
double firstPosition = dataset.GetValue&lt;double&gt;(Tag.ImagePositionPatient, 0);

// Check if a tag exists before accessing it
if (dataset.Contains(Tag.PatientBirthDate))
{
    var birthDate = dataset.GetSingleValue&lt;DateOnly&gt;(Tag.PatientBirthDate);
}

// TryGet pattern for safe access without exceptions
if (dataset.TryGetSingleValue&lt;string&gt;(Tag.PatientID, out var patientId))
{
    // Use patientId
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เพิ่มและอัปเดตแท็ก DICOM">}}

<p>ใช้ <code>AddOrUpdate</code> เพื่อตั้งค่าของแท็ก. เมธอดนี้จะสร้างองค์ประกอบหากแท็กไม่มีอยู่, หรือแทนที่ค่าของมันหากมีอยู่แล้ว. คุณสามารถตั้งค่าตัวเดียว, อาเรย์, หรือเพิ่มองค์ประกอบที่กำหนดชนิดอย่างครบถ้วน:</p>

<div class="codeblock" id="code">
 <h3>เพิ่มและอัปเดตแท็ก DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Set a single value
dataset.AddOrUpdate(Tag.PatientName, "John Doe");
dataset.AddOrUpdate(Tag.PatientAge, new Age { Number = 42, Units = Age.Unit.Years });

// Set multiple values on a multi-valued tag
dataset.AddOrUpdate&lt;double&gt;(Tag.PixelSpacing, [0.5, 0.5]);

// Add multiple elements at once using typed element classes
dataset.AddOrUpdateRange(new IElement[]
{
    new PersonName(Tag.PatientName, ["John Doe"]),
    new LongString(Tag.PatientID, ["64AD6A3F"]),
    new Date(Tag.PatientBirthDate, [new DateOnly(2002, 10, 10)])
});

// Save the modified file
dicomFile.Save("updated.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ลบแท็ก DICOM">}}

<p>เมธอด <code>Remove</code> รองรับการลบแท็กเดี่ยว, แท็กหลายรายการพร้อมกัน, หรือแท็กที่ตรงกับเงื่อนไข &mdash; มีประโยชน์สำหรับการลบกลุ่มองค์ประกอบทั้งหมด:</p>

<div class="codeblock" id="code">
 <h3>ลบแท็ก DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Remove a single tag
dataset.Remove(Tag.PatientAddress);

// Remove multiple tags at once
dataset.Remove(Tag.PatientName, Tag.PatientID, Tag.PatientBirthDate);

// Remove all tags in a specific group (e.g., group 0x0002 - File Meta Information)
dataset.Remove(element => element.Tag.Group == 0x0002);

dicomFile.Save("cleaned.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="นับแท็กตามกลุ่ม">}}

<p>แท็ก DICOM ถูกจัดเป็นกลุ่ม — เช่น กลุ่ม <code>0x0010</code> มีข้อมูลผู้ป่วย, กลุ่ม <code>0x0008</code> มีการระบุการศึกษา, และกลุ่ม <code>0x0028</code> มีคุณลักษณะพิกเซลของภาพ. ใช้ <code>EnumerateGroup</code> เพื่อวนผ่านทุกองค์ประกอบในกลุ่มที่ระบุ:</p>

<div class="codeblock" id="code">
 <h3>นับองค์ประกอบตามกลุ่ม - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Enumerate all patient-related tags (group 0x0010)
foreach (IElement element in dataset.EnumerateGroup(0x0010))
{
    Tag tag = element.Tag;
    string keyword = tag.Metadata.Keyword;
    string vr = element.ValueRepresentation.ToString();

    Console.WriteLine($"({tag.Group:X4},{tag.Element:X4}) {keyword} [{vr}]");
}

// Iterate over all elements in the dataset
foreach (IElement element in dataset)
{
    // Process each element
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การจัดการข้อมูลระดับองค์ประกอบ">}}

<p>แต่ละองค์ประกอบ DICOM มีเมธอดสำหรับอ่าน, เพิ่ม, แทรก, ลบ, และแทนที่ค่าต่าง ๆ ภายในองค์ประกอบนั้น. สิ่งนี้มีประโยชน์สำหรับแท็กหลายค่า ที่คุณต้องการการควบคุมละเอียดของรายการค่า:</p>

<div class="codeblock" id="code">
 <h3>จัดการค่าขององค์ประกอบ - C#</h3>
 <pre><code class="cs">// Create an element with multiple values
var element = new FloatingPointDouble(
    Tag.TableOfYBreakPoints, [50.1, 100.2, 150.3]);

// Access values by index (supports ^ for indexing from end)
double first = element.Get&lt;double&gt;(0);
double last = element.Get&lt;double&gt;(^1);

// Safe access with default
double safe = element.GetOrDefault&lt;double&gt;(10); // returns 0.0 if out of range

// Get all values or a range
double[] all = element.GetValues&lt;double&gt;();
double[] subset = element.GetValues&lt;double&gt;(1..3);

// Modify element values
element.Add(200.4);
element.AddRange([300.5, 400.6]);
element.Insert(2, 75.0);
element.RemoveAt(0);
element.Replace([1.0, 2.0, 3.0]);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="พจนานุกรมแท็กและเมทาดาต้า">}}

<p>คลาส <code>TagDictionary</code> ให้ทะเบียนของแท็ก DICOM ที่รู้จักพร้อมเมทาดาต้า — คีย์เวิร์ด, คำอธิบาย, Value Representation, จำนวนค่าที่เป็นไปได้, และสถานะการเลิกใช้. คุณสามารถค้นหาแท็กโดยคีย์เวิร์ดหรือสืบตรวจเมทาดาต้าของมันโดยโปรแกรม:</p>

<div class="codeblock" id="code">
 <h3>สอบถามพจนานุกรมแท็ก - C#</h3>
 <pre><code class="cs">// Look up a tag by its keyword
Tag? tag = TagDictionary.Default.FindByKeyword("PatientName");

// Access tag metadata from the dictionary
TagMetadata meta = TagDictionary.Default[Tag.PatientName];
Console.WriteLine($"Keyword: {meta.Keyword}");
Console.WriteLine($"Name: {meta.Name}");
Console.WriteLine($"VR: {string.Join("/", meta.ValueRepresentations)}");
Console.WriteLine($"VM: {meta.Multiplicity}");
Console.WriteLine($"Retired: {meta.Retired}");

// Access metadata directly from a tag instance
TagMetadata info = Tag.Rows.Metadata;

// Parse a tag from its string representation
Tag parsed = Tag.Parse("0010,0010");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การสนับสนุนแท็กส่วนตัว">}}

<p>DICOM อนุญาตให้ผู้ขายและองค์กรกำหนดแท็กส่วนตัวสำหรับข้อมูลที่เป็นกรรมสิทธิ์. Aspose.Medical for .NET รองรับการอ่าน, เขียน, และลงทะเบียนแท็กส่วนตัวอย่างเต็มที่. คุณสามารถโหลดพจนานุกรมแท็กกำหนดเองจากไฟล์ XML เพื่อให้จัดการองค์ประกอบส่วนตัวได้อย่างถูกต้อง:</p>

<div class="codeblock" id="code">
 <h3>ทำงานกับแท็กส่วนตัว - C#</h3>
 <pre><code class="cs">// Load a custom private tag dictionary from XML
// XML format:
// &lt;dictionaries&gt;
//   &lt;dictionary creator="MY ORGANIZATION"&gt;
//     &lt;tag group="3011" element="xx00" vr="SL" vm="1"&gt;Custom Value&lt;/tag&gt;
//   &lt;/dictionary&gt;
// &lt;/dictionaries&gt;
TagDictionary.Default.Load("custom-tag-dictionary.xml");

// Check if a tag is private
Tag tag = Tag.GetOrCreate(0x3011, 0x0010);
bool isPrivate = tag.IsPrivate;

// Get a valid private tag (creates Private Creator element if needed)
Dataset dataset = dicomFile.Dataset;
Tag privateTag = dataset.GetPrivateTag(tag);

// Register a private creator programmatically
PrivateCreator creator = new("MY ORGANIZATION");
TagDictionary privateDictionary =
    TagDictionary.Default.GetPrivateDictionary(creator);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ทำงานกับลำดับ DICOM">}}

<p>ลำดับ DICOM (ประเภท VR SQ) มีชุดข้อมูลซ้อนกัน, สร้างโครงสร้างต้นไม้ภายในไฟล์ DICOM. คลาส <code>Sequence</code> ให้การเข้าถึงแบบชนิดของรายการลำดับ:</p>

<div class="codeblock" id="code">
 <h3>อ่านและสร้างลำดับ DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Read a sequence element
IElement seqElement = dataset.GetOrDefault(Tag.ReferencedStudySequence);
if (seqElement is Sequence sequence)
{
    // Iterate over sequence items (each item is a Dataset)
    foreach (Dataset item in sequence.Items)
    {
        string uid = item.GetSingleValueOrDefault&lt;string&gt;(
            Tag.ReferencedSOPInstanceUID, "");
    }

    // Access an item by index
    Dataset firstItem = sequence.Get(0);
}

// Create a new sequence
var items = new List&lt;Dataset&gt;
{
    new(new IElement[]
    {
        new UniqueIdentifier(Tag.ReferencedSOPClassUID, ["1.2.840.10008.5.1.4.1.1.2"]),
        new UniqueIdentifier(Tag.ReferencedSOPInstanceUID, ["1.2.3.4.5.6.7.8.9"])
    })
};
dataset.Add(new Sequence(Tag.ReferencedStudySequence, items));</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="กลุ่มแท็ก DICOM ที่พบบ่อย">}}

<p>ตารางต่อไปนี้แสดงกลุ่มแท็ก DICOM ที่ใช้งานบ่อยและตัวอย่างแท็กที่เข้าถึงได้ผ่านคลาส <code>Tag</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>กลุ่ม</th>
<th>ประเภท</th>
<th>ตัวอย่างแท็ก</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>ข้อมูลเมตาไฟล์</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>การระบุการศึกษา</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>ข้อมูลผู้ป่วย</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>พารามิเตอร์การได้มา</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>การจัดตำแหน่งภาพ</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>คุณลักษณะพิกเซลของภาพ</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>ข้อมูลพิกเซล</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="ซอร์สโค้ด" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบจ่ายเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมถึงเลือก Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="กรณีศึกษาความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
