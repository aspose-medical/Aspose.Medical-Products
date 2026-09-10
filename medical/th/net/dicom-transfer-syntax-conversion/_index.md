---
title: การแปลง Transfer Syntax ของ DICOM ใน C# .NET | Aspose.Medical
weight: 16000
description: แปลงรหัสไฟล์ DICOM ระหว่าง Transfer Syntax ต่าง ๆ ใน C# .NET รองรับ JPEG, JPEG 2000, JPEG-LS, RLE และรูปแบบที่ไม่บีบอัดด้วย Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="การแปลง Transfer Syntax ของ DICOM ใน .NET C#" h2="แปลงรหัสไฟล์ DICOM ระหว่าง Transfer Syntax ที่ไม่บีบอัด, JPEG, JPEG 2000, JPEG-LS และ RLE. ไลบรารี .NET แท้โดยไม่มีการพึ่งพา native ใด ๆ." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Transfer Syntax คืออะไร?">}}

<p>A <strong>Transfer Syntax</strong> กำหนดวิธีการเข้ารหัสข้อมูล DICOM สำหรับการจัดเก็บและการส่งต่อ มันระบุสามแง่มุมสำคัญ: การเรียงลำดับไบต์ (endianness), การที่ Value Representations ถูกระบุอย่างชัดเจนหรือโดยปริยาย, และอัลกอริธึมการบีบอัดที่ใช้กับข้อมูลพิกเซล ทุกไฟล์ DICOM จะประกาศ Transfer Syntax ของมันในส่วนหัว File Meta Information</p>

<p>อุปกรณ์การแพทย์, เซิร์ฟเวอร์ PACS, และแอปพลิเคชันการดูภาพต่าง ๆ รองรับชุด Transfer Syntax ที่แตกต่างกัน <strong>Aspose.Medical for .NET</strong> มีเมธอด <code>Transcode</code> เพื่อแปลงระหว่าง Transfer Syntax ทำให้เกิดการทำงานร่วมกัน, การเพิ่มประสิทธิภาพการจัดเก็บ, และความเข้ากันได้กับเครื่องมือประมวลผล &mdash; ทั้งหมดในไลบรารี .NET แท้โดยไม่มีการพึ่งพา native ใด ๆ</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="แปลงรหัสไฟล์ DICOM ใน C#">}}

<p>เมธอด <code>DicomFile.Transcode</code> แปลงไฟล์ DICOM จาก Transfer Syntax ปัจจุบันไปยัง Transfer Syntax ปลายทางที่สนับสนุน เมธอดจะคืนค่าอินสแตนซ์ใหม่ของ <code>DicomFile</code> &mdash; ไฟล์เดิมยังคงไม่เปลี่ยนแปลง</p>

<div class="codeblock" id="code">
 <h3>การแปลง DICOM พื้นฐาน - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>คุณสามารถแปลงรหัสได้ที่ระดับ <code>Dataset</code> โดยตรงเช่นกัน:</p>

<div class="codeblock" id="code">
 <h3>แปลงรหัส Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transfer Syntax ที่สนับสนุน">}}

<p>ตารางต่อไปนี้แสดงรายการ Transfer Syntax ของข้อมูลภาพ DICOM มาตรฐานทั้งหมดและสถานะการสนับสนุนปัจจุบันใน Aspose.Medical for .NET โค้ดเคสที่สนับสนุนทั้งหมดถูกดำเนินการใน C# แท้และไม่มีการขึ้นกับแพลตฟอร์มใด</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Type</th>
<th>Status</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>ไม่บีบอัด</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>ไม่บีบอัด</td><td>สนับสนุน</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>ไม่บีบอัด</td><td>สนับสนุน</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>ไม่บีบอัด</td><td>สนับสนุน</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>สนับสนุน</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Lossy, 8-bit</td><td>สนับสนุน</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Lossy, 12-bit</td><td>ไม่สนับสนุน</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Lossless</td><td>สนับสนุน (เฉพาะ 8-bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Lossless</td><td>สนับสนุน (เฉพาะ 8-bit)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Lossless</td><td>สนับสนุน</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Near-lossless</td><td>สนับสนุน</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>สนับสนุน (อ่าน 8/16-bit, เขียน 8-bit)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy or lossless</td><td>สนับสนุน (อ่าน 8/16-bit, เขียน 8-bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>สนับสนุน (อ่าน 8/16-bit, เขียน 8-bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy or lossless</td><td>สนับสนุน (อ่าน 8/16-bit, เขียน 8-bit)</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Lossless</td><td>สนับสนุน</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>เร็ว ๆ นี้</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>เร็ว ๆ นี้</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy or lossless</td><td>เร็ว ๆ นี้</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Lossless</td><td>เร็ว ๆ นี้</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Lossless</td><td>เร็ว ๆ นี้</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Lossy or lossless</td><td>เร็ว ๆ นี้</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="สถานการณ์การแปลงรหัสที่พบบ่อย">}}

<p>กระบวนการทำงานที่แตกต่างกันต้องการกลยุทธ์การแปลงรหัสที่ต่างกัน ต่อไปนี้เป็นสถานการณ์ที่พบบ่อยที่สุด:</p>

<div class="codeblock" id="code">
 <h3>ถอดรหัสเพื่อประมวลผล - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>บีบอัดเพื่อเก็บถาวร - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>บีบอัดเพื่อส่งผ่านเครือข่าย - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ตรวจสอบคุณสมบัติของ Transfer Syntax">}}

<p>คลาส <code>TransferSyntax</code> เปิดเผยคุณสมบัติที่บรรยายลักษณะการเข้ารหัส ใช้คุณสมบัติเหล่านี้เพื่อสำรวจ Transfer Syntax ปัจจุบันของไฟล์หรือเพื่อเลือก Transfer Syntax ปลายทางที่เหมาะสม:</p>

<div class="codeblock" id="code">
 <h3>อ่านคุณสมบัติของ Transfer Syntax - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>คุณสมบัติ</th>
<th>ประเภท</th>
<th>คำอธิบาย</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>ตัวระบุที่ไม่ซ้ำของ Transfer Syntax</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>ว่าการแสดงค่า Value Representations ถูกเข้ารหัสอย่างชัดเจนหรือไม่</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>ว่าการจัดลำดับไบต์เป็นแบบ little endian หรือไม่</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>ว่าข้อมูลพิกเซลถูกหุ้ม (บีบอัด) หรือไม่</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>ว่าวิธีการบีบอัดเป็นแบบเสียข้อมูลหรือไม่</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>ว่า Transfer Syntax ใช้การบีบอัดแบบ deflate หรือไม่</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>ว่าการถ่ายโอน Syntax นี้ถูกยกเลิกโดยมาตรฐาน DICOM หรือไม่</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>ตัวระบุมาตรฐาน ISO ของวิธีการบีบอัดแบบเสียข้อมูล</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การบีบอัดแบบเสียข้อมูล vs ไม่เสียข้อมูล">}}

<p>การเข้าใจความแตกต่างระหว่างการบีบอัดแบบเสียข้อมูลและไม่เสียข้อมูลเป็นสิ่งสำคัญเมื่อต้องแปลงรหัสไฟล์ DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>แง่มุม</th>
<th>ไม่เสียข้อมูล</th>
<th>เสียข้อมูล</th>
</tr>
</thead>
<tbody>
<tr><td>คุณภาพของภาพ</td><td>Pixel-perfect &mdash; ข้อมูลต้นฉบับถูกเก็บไว้ครบถ้วน</td><td>ข้อมูลบางส่วนสูญเสียอย่างถาวรเพื่อให้ได้ขนาดเล็กลง</td></tr>
<tr><td>อัตราการบีบอัด</td><td>โดยทั่วไป 2:1 ถึง 3:1</td><td>โดยทั่วไป 10:1 ถึง 30:1 หรือสูงกว่า</td></tr>
<tr><td>ปลอดภัยสำหรับการรอบกลับ</td><td>ใช่ &mdash; ถอดรหัสแล้วได้พิกเซลที่เหมือนกัน</td><td>ไม่ &mdash; การเข้ารหัสเสียข้อมูลซ้ำแต่ละครั้งทำให้คุณภาพลดลง</td></tr>
<tr><td>กรณีการใช้งาน</td><td>การเก็บถาวร, การวินิจฉัย, บันทึกทางกฎหมาย</td><td>การตรวจสอบเบื้องต้น, โทรเวชกรรม, การส่งผ่านเครือข่าย</td></tr>
<tr><td>โค้ดเคสที่สนับสนุน</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>สำคัญ:</strong> การแปลงรหัสจากไฟล์ที่บีบอัดแบบเสียข้อมูลไปยัง Transfer Syntax ที่ไม่เสียข้อมูลไม่สามารถคืนข้อมูลที่สูญหายได้ การลดคุณภาพจากการบีบอัดแบบเสียข้อมูลเดิมเป็นถาวร</p>

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
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบชำระเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้อง Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายการลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="กรณีศึกษา" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
