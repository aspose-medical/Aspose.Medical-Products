---
title: การแปลง Transfer Syntax ของ DICOM ใน C# .NET | Aspose.Medical
weight: 16000
description: แปลงรหัสไฟล์ DICOM ระหว่าง Transfer Syntax ต่าง ๆ ใน C# .NET รองรับ JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG‑LS, RLE และรูปแบบที่ไม่มีการบีบอัดด้วย Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="การแปลง Transfer Syntax ของ DICOM ใน .NET C#" h2="แปลงรหัสไฟล์ DICOM ระหว่าง Transfer Syntax ที่ไม่มีการบีบอัด, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG‑LS และ RLE. ไลบรารี .NET แท้ที่ไม่มีการพึ่งพา native" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Transfer Syntax คืออะไร?">}}

<p>A <strong>Transfer Syntax</strong> กำหนดวิธีการเข้ารหัสข้อมูล DICOM สำหรับการจัดเก็บและการส่งต่อ มันระบุสามประเด็นสำคัญ: ลำดับไบต์ (endianness), ว่า Value Representation ถูกกำหนดอย่างชัดเจนหรือโดยอ้อม, และอัลกอริธึมการบีบอัดที่ใช้กับข้อมูลพิกเซล ทุกไฟล์ DICOM จะประกาศ Transfer Syntax ของมันในส่วนหัว File Meta Information</p>

<p>อุปกรณ์การแพทย์ต่าง ๆ, เซิร์ฟเวอร์ PACS, และแอปพลิเคชันการดูภาพสนับสนุนชุด Transfer Syntax ที่แตกต่างกัน <strong>Aspose.Medical for .NET</strong> มีเมธอด <code>Transcode</code> เพื่อแปลงระหว่าง Transfer Syntax ทำให้เกิดการทำงานร่วมกัน, การเพิ่มประสิทธิภาพการจัดเก็บ, และความเข้ากันได้กับเครื่องมือประมวลผล — ทั้งหมดในไลบรารี .NET แท้ที่ไม่มีการพึ่งพา native</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="แปลงรหัสไฟล์ DICOM ด้วย C#">}}

<p>เมธอด <code>DicomFile.Transcode</code> แปลงไฟล์ DICOM จาก Transfer Syntax ปัจจุบันของมันไปยัง Syntax ปลายทางที่สนับสนุน เมธอดจะคืนค่าอินสแตนซ์ <code>DicomFile</code> ใหม่ — ไฟล์ต้นฉบับยังคงไม่เปลี่ยนแปลง:</p>

<div class="codeblock" id="code">
 <h3>การแปลง DICOM เบื้องต้น - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>คุณสามารถแปลงที่ระดับ <code>Dataset</code> ได้โดยตรงเช่นกัน:</p>

<div class="codeblock" id="code">
 <h3>แปลง Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transfer Syntax ที่รองรับ">}}

<p>ตารางต่อไปนี้แสดง Transfer Syntax มาตรฐานทั้งหมดของข้อมูลภาพ DICOM และสถานะการสนับสนุนในปัจจุบันของ Aspose.Medical for .NET. ตัวเข้ารหัสที่สนับสนุนทั้งหมดถูกทำงานใน C# แท้และเป็นอิสระต่อแพลตฟอร์มอย่างสมบูรณ์</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>ประเภท</th>
<th>สถานะ</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>ไม่บีบอัด</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>ไม่บีบอัด</td><td>สนับสนุน</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>ไม่บีบอัด</td><td>สนับสนุน</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>ไม่บีบอัด (หยุดใช้)</td><td>สนับสนุน</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>ไม่บีบอัด</td><td>ไม่สนับสนุน</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>สนับสนุน</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>บีบอัดแบบเสียข้อมูล, 8-bit</td><td>สนับสนุน</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>บีบอัดแบบเสียข้อมูล, 12-bit</td><td>ไม่สนับสนุน</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>ไม่เสียข้อมูล</td><td>สนับสนุน (เฉพาะ 8-bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>ไม่เสียข้อมูล</td><td>สนับสนุน (เฉพาะ 8-bit)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>ไม่เสียข้อมูล</td><td>สนับสนุน</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>เกือบไม่เสียข้อมูล</td><td>สนับสนุน</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>ไม่เสียข้อมูล</td><td>สนับสนุน (อ่านสี 8-bit และ monochrome 16-bit; เขียน monochrome 16-bit หรือ RGB 8-bit)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>บีบอัดแบบเสียข้อมูลหรือไม่เสียข้อมูล</td><td>สนับสนุน (อ่านสี 8-bit และ monochrome 16-bit; เขียน monochrome 16-bit หรือ RGB 8-bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>ไม่เสียข้อมูล</td><td>ไม่สนับสนุน</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>บีบอัดแบบเสียข้อมูลหรือไม่เสียข้อมูล</td><td>ไม่สนับสนุน</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>ไม่เสียข้อมูล</td><td>สนับสนุน</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>ไม่เสียข้อมูล</td><td>สนับสนุน</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>ไม่เสียข้อมูล</td><td>สนับสนุน</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>บีบอัดแบบเสียข้อมูลหรือไม่เสียข้อมูล</td><td>สนับสนุน</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>ไม่เสียข้อมูล</td><td>สนับสนุน</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>ไม่เสียข้อมูล</td><td>เฉพาะการถอดรหัส (การเข้ารหัสต้องใช้สตรีม JPEG ต้นฉบับ ไม่ใช่ข้อมูลพิกเซล)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>บีบอัดแบบเสียข้อมูลหรือไม่เสียข้อมูล</td><td>สนับสนุน (โหมดเสียข้อมูล)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="สถานการณ์การแปลงรหัสที่พบบ่อย">}}

<p>กระบวนการทำงานที่แตกต่างกันต้องการกลยุทธ์การแปลงรหัสที่แตกต่างกัน นี่คือตัวอย่างที่พบบ่อยที่สุด:</p>

<div class="codeblock" id="code">
 <h3>ถอดรหัสเพื่อประมวลผล - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>บีบอัดเพื่อจัดเก็บในคลังข้อมูล - C#</h3>
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

<div class="codeblock" id="code">
 <h3>ใช้โค้ดเดกใหม่ที่สุด: HTJ2K และ JPEG XL - C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ตรวจสอบคุณสมบัติของ Transfer Syntax">}}

<p>คลาส <code>TransferSyntax</code> เปิดเผยคุณสมบัติที่อธิบายลักษณะการเข้ารหัส ใช้เพื่อตรวจสอบ Transfer Syntax ปัจจุบันของไฟล์หรือเพื่อเลือก Syntax ปลายทางที่เหมาะสม:</p>

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
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>ว่าการแทนค่าตัวแปร (Value Representation) ถูกเข้ารหัสอย่างชัดเจนหรือไม่</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>ว่าลำดับไบต์เป็นแบบ little endian หรือไม่</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>ว่าข้อมูลพิกเซลถูกห่อหุ้ม (บีบอัด) หรือไม่</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>ว่าวิธีการบีบอัดเป็นแบบเสียข้อมูลหรือไม่</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>ว่ารูปแบบนี้ใช้การบีบอัดแบบ deflate หรือไม่</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>ว่าการ Transfer Syntax นี้ถูกยกเลิกโดยมาตรฐาน DICOM หรือไม่</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>ตัวระบุมาตรฐาน ISO ของวิธีการบีบอัดแบบเสียข้อมูล</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การบีบอัดแบบเสียข้อมูล vs ไม่เสียข้อมูล">}}

<p>การเข้าใจความแตกต่างระหว่างการบีบอัดแบบเสียข้อมูลและไม่เสียข้อมูลเป็นสิ่งสำคัญเมื่อทำการแปลงรหัสไฟล์ DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>แง่มุม</th>
<th>ไม่เสียข้อมูล</th>
<th>เสียข้อมูล</th>
</tr>
</thead>
<tbody>
<tr><td>คุณภาพภาพ</td><td>Pixel-perfect &mdash; ข้อมูลต้นฉบับถูกเก็บไว้เต็มรูปแบบ</td><td>ข้อมูลบางส่วนสูญหายอย่างถาวรเพื่อให้ได้ขนาดที่เล็กลง</td></tr>
<tr><td>อัตราการบีบอัด</td><td>โดยทั่วไป 2:1 ถึง 3:1</td><td>โดยทั่วไป 10:1 ถึง 30:1 หรือสูงกว่า</td></tr>
<tr><td>ปลอดภัยต่อการรอบกลับ</td><td>ใช่ &mdash; ถอดรหัสแล้วได้พิกเซลเดิม</td><td>ไม่ &mdash; การเข้ารหัสแบบเสียข้อมูลซ้ำทำให้คุณภาพเสื่อมลงต่อเนื่อง</td></tr>
<tr><td>กรณีการใช้งาน</td><td>จัดเก็บระยะยาว, การวินิจฉัย, บันทึกทางกฎหมาย</td><td>การตรวจสอบเบื้องต้น, การแพทย์ทางไกล, การส่งผ่านเครือข่าย</td></tr>
<tr><td>โค้ดเดกที่สนับสนุน</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>สำคัญ:</strong> การแปลงรหัสจากไฟล์ที่บีบอัดแบบเสียข้อมูลไปยัง Syntax ที่ไม่เสียข้อมูลไม่สามารถกู้คืนข้อมูลที่สูญหายได้ การลดคุณภาพจากการบีบอัดแบบเสียข้อมูลเดิมเป็นถาวร</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารอธิบาย" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="ซอร์สโค้ด" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
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
