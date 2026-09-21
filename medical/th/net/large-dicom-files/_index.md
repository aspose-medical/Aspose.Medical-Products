---
title: ทำงานกับไฟล์ DICOM ขนาดใหญ่ใน C# .NET | Aspose.Medical
weight: 11500

description: เปิดการศึกษาหลายเฟรมและรูปภาพสไลด์ทั้งหมดใน C# โดยไม่ต้องโหลดเข้าเมโมรี อ่านเมทาดาต้าโดยไม่มีข้อมูลพิกเซล เลื่อนการอ่านองค์ประกอบขนาดใหญ่จนกว่าจะมีการเรียกใช้ และย้ายไฟล์ทั้งหมดผ่านสตรีมและพายป์
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="ไฟล์ DICOM ขนาดใหญ่ใน .NET C#" h2="อ่านเมทาดาต้าของการศึกษาหลายเฟรมโดยไม่ต้องโหลดพิกเซล เลื่อนการอ่านองค์ประกอบขนาดใหญ่จนกว่าจะมีการร้องขอ และย้ายไฟล์ทั้งหมดผ่านสตรีมและพายป์" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="ไฟล์มีขนาดใหญ่ ส่วนคำถามมักจะเล็ก">}}

<p>ภาพสไลด์เต็ม (whole slide image), ซีรีส์ CT ยาว หรือโวลูม OCT มีขนาดหลายร้อยเมกะไบต์ และส่วนใหญ่เป็นข้อมูลพิกเซล งานที่แอปพลิเคชันทำจริงมักจะเล็กกว่ามาก: รายการสิ่งที่อยู่ในโฟลเดอร์, ตรวจสอบรหัสผู้ป่วย, นับจำนวนเฟรม, ตัดสินใจว่าการศึกษานั้นควรไปที่ไหน การโหลดทุกไบต์เพื่อให้ได้คำตอบเหล่านี้เป็นสิ่งที่ทำให้งานง่ายกลายเป็นปัญหาเรื่องหน่วยความจำ</p>

<p><strong>Aspose.Medical for .NET</strong> ให้ผู้เรียกกำหนดว่าต้องการอ่านไฟล์เท่าไหร่ การเลือกนี้เป็นอาร์กิวเมนต์หนึ่งของ <code>DicomFile.Open</code> และใช้ได้กับไฟล์, สตรีม และพายป์เช่นเดียวกัน</p>

<p>วัดผลบนการศึกษาขนาด 14 MB ที่มี 128 เฟรมจากชุดทดสอบของเรา โดยใช้เครื่องและไฟล์เดียวกัน:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>กลยุทธ์การอ่าน</th>
<th>เวลาในการเปิด</th>
<th>หน่วยความจำที่จัดสรร</th>
</tr>
</thead>
<tbody>
<tr><td>ทั้งหมด, ค่าเริ่มต้น</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>ข้ามองค์ประกอบขนาดใหญ่</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>รอคอยองค์ประกอบขนาดใหญ่</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>ช่องว่างนี้เพิ่มขึ้นตามขนาดไฟล์ โฟลเดอร์ที่มีการศึกษา 10,000 ฉบับเป็นกรณีที่มันไม่ใช่แค่การปรับจูนระดับจิ๋วอีกต่อไป</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="อ่านเมทาดาต้า, เว้นพิกเซลไว้">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> จะละเว้นทุกองค์ประกอบที่มีขนาดเกินเกณฑ์จากการอ่าน ชุดข้อมูลที่ส่งกลับมาจะมีแท็กที่ดัชนีหรือเร้าท์เตอร์ต้องการเท่านั้น</p>

<div class="codeblock" id="code">
 <h3>อ่านการศึกษาโดยไม่รวมข้อมูลพิกเซล - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>ค่าเกณฑ์เริ่มต้นที่ 64 kB และรับค่าหน่วยเป็นกิโลไบต์ ดังนั้น workflow ที่พิจารณา 8 kB ว่าใหญ่สามารถกำหนดได้ตามต้องการ</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="รอคอยแทนการข้าม">}}

<p>เมื่อพิกเซลอาจจำเป็นต้องใช้ในภายหลัง (แต่โดยส่วนมากไม่ต้องใช้ทั้งหมด) <code>ReadLargeOnDemand</code> คืออีกครึ่งหนึ่งของคู่เลือก การเปิดไฟล์ใช้เวลาเท่ากับการข้าม และองค์ประกอบขนาดใหญ่จะถูกอ่านเมื่อโค้ดเข้าถึงมัน</p>

<div class="codeblock" id="code">
 <h3>โหลดเฟรมเมื่อจำเป็นต้องใช้เท่านั้น - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>การอ่านแบบรอคอยเป็นฟีเจอร์ที่ต้องมีลิขสิทธิ์; กลยุทธ์อื่น ๆ ยังทำงานได้ในโหมดประเมินผลเช่นกัน</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ทำดัชนีโฟลเดอร์โดยไม่ต้องสัมผัสพิกเซล">}}

<p>กลยุทธ์เดียวกันใช้กับสตรีม ซึ่งเป็นสิ่งที่การสแกนอาร์ไคฟ์หรือคลาวด์ออบเจ็กต์สโตร์ทำจากโค้ด</p>

<div class="codeblock" id="code">
 <h3>สแกนอาร์ไคฟ์ - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="สตรีมและพายป์, เข้าและออก">}}

<p>การอ่านและการเขียนรองรับสตรีมทั้งสองแบบ และจุดเข้าที่ทำงานแบบอะซิงโครนัสก็รองรับประเภท <code>System.IO.Pipelines</code> ด้วย การศึกษาอาจเดินทางจากการตอบสนองเครือข่ายไปยังที่เก็บโดยไม่ต้องให้กระบวนการถือไฟล์ทั้งหมดเป็นอาเรย์เดียว</p>

<div class="codeblock" id="code">
 <h3>อ่านและเขียนผ่านสตรีม - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>แนวคิดเดียวกันครอบคลุมการแสดงผลเป็นข้อความ: เอกสารที่มีหลายชุดข้อมูลจะถูกอ่านทีละชุดบนหน้า <a href="/medical/net/json-to-dicom/">JSON to DICOM</a> และ <a href="/medical/net/xml-to-dicom/">XML to DICOM</a></p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เฟรมต่อเฟรม">}}

<p>ข้อมูลหลายเฟรมจะถูกอ้างอิงต่อเฟรม ดังนั้นซีรีส์ 500 เฟรมจะใช้การอ่านหนึ่งเฟรมต่อครั้งแทนการอ่านข้อมูลพิกเซลทั้งหมดในครั้งเดียว</p>

<div class="codeblock" id="code">
 <h3>เดินผ่านเฟรม - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เมื่อสิ่งนี้กำหนดการออกแบบ">}}

<ul>
<li>การทำดัชนีและการย้ายอาร์ไคฟ์: ล้านไฟล์และเพียงส่วนหัวเท่านั้นที่สำคัญจนกว่าจะมีการย้ายบางอย่าง</li>
<li>เร้าท์เตอร์และโหนดเก็บข้อมูล: รับการศึกษา, อ่านสิ่งที่จำเป็นเพื่อทำการส่งต่อ, ส่งต่อไบต์ต่อไป</li>
<li>ไพพ์ไลน์ AI: สร้าง manifest จากเมทาดาต้า แล้วดึงเฟรมสำหรับส่วนย่อยที่ใช้ฝึกจริง</li>
<li>คอนเทนเนอร์ที่มีขีดจำกัดหน่วยความจำ: ชุดทำงานตามกลยุทธ์ ไม่ใช่ตามขนาดไฟล์</li>
<li>ข้อมูล whole slide และ OCT: ไฟล์ที่การอ่านทั้งหมดไม่ใช่ตัวเลือก</li>
</ul>

<p>คู่มือ <a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">การจัดการหน่วยความจำ</a> อธิบายกลยุทธ์โดยละเอียด, และ <a href="/medical/net/dicom-networking/">DICOM networking</a> แสดงข้อมูลเดียวกันที่มาถึงผ่าน DIMSE</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งข้อมูลการเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือสำหรับนักพัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
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
