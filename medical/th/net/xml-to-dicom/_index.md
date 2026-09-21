---
title: แปลง XML เป็น DICOM ใน C# .NET | Aspose.Medical
weight: 5000

description: สร้างไฟล์ DICOM จาก Native DICOM Model XML ของ PS3.19 ใน C# .NET. อ่าน XML จากสตริง, สตรีม หรือ pipe, สตรีมเอกสารต่อเนื่อง, และแก้ไขการอ้างอิง bulk data ด้วย Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="แปลง XML เป็น DICOM ใน .NET C#" h2="อ่าน Native DICOM Model XML ของ PS3.19 กลับเป็น datasets และไฟล์ DICOM. ทำงานจากสตริง, สตรีม หรือ pipe, สตรีมเอกสารต่อเนื่อง, และแก้ไขการอ้างอิง bulk data." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="XML มาตรฐานของ Native DICOM Model">}}

<p><strong>Aspose.Medical for .NET</strong> อ่าน <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a> ที่กำหนดใน DICOM PS3.19. นี่เป็นการแสดงผล XML ที่บันทึกในมาตรฐานเอง ไม่ใช่รูปแบบที่ Aspose สร้างขึ้น ซึ่งทำให้มันมีประโยชน์สำหรับการบูรณาการ: ระบบที่แลกเปลี่ยน DICOM เป็น XML อยู่แล้วจะสร้างเอกสารที่ไลบรารีนี้รับได้.</p>

<p>รูทของเอกสารคือ <code>NativeDicomModel</code> และแต่ละ attribute คือองค์ประกอบ <code>DicomAttribute</code> ที่บรรจุ tag, value representation และ keyword ของมัน:</p>

<div class="codeblock" id="code">
 <h3>รูปแบบ Native DICOM Model</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>หน้านี้เป็นทิศทางย้อนกลับของ <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> และทั้งสองใช้คลาสเดียวกันคือ <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="สร้างไฟล์ DICOM จาก XML ใน C#">}}

<p><code>Deserialize</code> แปลงเอกสารเป็น <code>Dataset</code> และ dataset จะถูกเขียนลงดิสก์เป็นไฟล์ DICOM.</p>

<div class="codeblock" id="code">
 <h3>สร้างไฟล์ DICOM จาก XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Native DICOM Model ไม่มีกลุ่ม File Meta Information ดังนั้น transfer syntax จึงไม่ได้เป็นส่วนของเอกสาร. Dataset ที่ห่อหุ้มด้วย <code>DicomFile</code> จะถูกเขียนด้วย transfer syntax เริ่มต้นคือ Implicit VR Little Endian. เพื่อจัดเก็บไฟล์ด้วย transfer syntax อื่น ให้ทำการแปลง (transcode) ตามที่หน้า <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> แสดง.</p>

<p>การอ่าน DICOM XML เป็นฟีเจอร์ที่ต้องมีใบอนุญาต. หากไม่ได้ใช้ใบอนุญาตบนเครื่อง อ่านจะโยนข้อยกเว้น <code>MedicalApiException</code> ดังนั้นให้ทำการตั้งค่าใบอนุญาตก่อน ตามที่ <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">คู่มือการให้ใบอนุญาต</a> อธิบาย.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streams, pipes และ async">}}

<p>ทุก entry point มี overload แบบ stream และแบบ asynchronous, และแบบ asynchronous ยังรับ <code>PipeReader</code> อีกด้วย. เอกสารที่มาจากการตอบสนองของเว็บจะถูกพาร์สขณะอ่านโดยไม่ต้องแปลงเป็นสตริงก่อน.</p>

<div class="codeblock" id="code">
 <h3>อ่าน XML จากสตรีม - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เอกสารต่อเนื่องในสตรีมเดียว">}}

<p>การส่งออกจากระบบอื่นมักจะเก็บเอาองค์ประกอบ <code>NativeDicomModel</code> หนึ่งต่อหนึ่งในสตรีมเดียว. <code>DeserializeAsyncEnumerable</code> ให้ผลลัพธ์เป็น dataset หนึ่งต่อหนึ่งองค์ประกอบตามลำดับอินพุต, ดังนั้นสตรีมจะถูกประมวลผลโดยไม่ต้องเก็บในหน่วยความจำ. องค์ประกอบต่อเนื่องกันโดยตรง: การประกาศ XML จะอนุญาตได้เฉพาะที่จุดเริ่มต้นเท่านั้น เช่นเดียวกับ XML ใด ๆ.</p>

<div class="codeblock" id="code">
 <h3>สตรีมเอกสารต่อเนื่อง - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="อ้างอิง bulk data">}}

<p>ค่าขนาดใหญ่เช่น pixel data จะไม่ถูกเขียนแบบอินไลน์. จะปรากฏเป็นองค์ประกอบ <code>BulkData</code> พร้อม URI ที่ชี้ไปยังไบต์ ซึ่งทำให้เอกสารมีขนาดเล็ก. เพื่อแก้ไขการอ้างอิงเหล่านี้ขณะอ่าน, ให้ serializer มี bulk data loader. <code>DefaultBulkDataLoader</code> ดึง URI แบบ <code>file</code>, <code>http</code> และ <code>https</code> โดยไม่ต้องพิสูจน์ตัวตน; สำหรับ archive ที่ต้องการข้อมูลรับรอง ให้ทำการ implement <code>IBulkDataLoader</code> หรือ <code>IAsyncBulkDataLoader</code> ด้วยตนเอง.</p>

<div class="codeblock" id="code">
 <h3>แก้ไข bulk data ขณะอ่าน - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ราวด์ทริปกับ DICOM to XML">}}

<p>สองทิศทางนี้ออกแบบให้ใช้ร่วมกัน: การศึกษาออกเป็น XML, ผ่านระบบที่สื่อสารด้วย XML, แล้วกลับมาเป็นไฟล์ DICOM. ทุกอย่างทำงานบน .NET, ดังนั้นราวด์ทริปเดียวกันทำงานได้บน Windows, Linux และ macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM to XML และกลับ - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>สำหรับตัวเลือกที่กำหนดรูปแบบ XML ดูที่หน้า <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>. คู่เดียวกันมีสำหรับ JSON: <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> และ <a href="/medical/net/json-to-dicom/">JSON to DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">คู่มือการ serialization</a> ครอบคลุม API ทั้งหมด.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือผู้พัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
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