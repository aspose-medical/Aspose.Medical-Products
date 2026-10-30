---
title: การแปลง Transfer Syntax ของ DICOM ใน C# .NET | Aspose.Medical
weight: 16000
description: แปลงรหัสไฟล์ DICOM ระหว่าง Transfer Syntax ต่าง ๆ ใน C# .NET รองรับ JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, RLE และรูปแบบที่ไม่มีการบีบอัดด้วย Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="การแปลง Transfer Syntax ของ DICOM ใน .NET C#" h2="แปลงไฟล์ DICOM ระหว่าง Transfer Syntax แบบไม่บีบอัด, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, และ RLE ไลบรารี .NET แบริสุทธิ์ที่ไม่มีการพึ่งพา native" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Transfer Syntax คืออะไร?">}}

<p><strong>Transfer Syntax</strong> กำหนดวิธีการเข้ารหัสข้อมูล DICOM สำหรับการจัดเก็บและการส่งข้อมูล ระบุสามประเด็นสำคัญ: การจัดลำดับไบต์ (endianness), การกำหนดว่าการแสดงค่า (Value Representations) เป็นแบบ explicit หรือ implicit, และอัลกอริทึมการบีบอัดที่ใช้กับข้อมูลพิกเซล ทุกไฟล์ DICOM จะประกาศ Transfer Syntax ของมันในส่วนหัว File Meta Information</p>

<p>อุปกรณ์การแพทย์, เซิร์ฟเวอร์ PACS, และแอปพลิเคชันการดูภาพต่าง ๆ รองรับชุด Transfer Syntax ที่แตกต่างกัน <strong>Aspose.Medical for .NET</strong> มีเมธอด <code>Transcode</code> สำหรับแปลงระหว่าง Transfer Syntax เพื่อให้เกิดการทำงานร่วมกัน, การเพิ่มประสิทธิภาพการจัดเก็บ, และความเข้ากันได้กับเครื่องมือประมวลผล &mdash; ทั้งหมดอยู่ในไลบรารี .NET แบริสุทธิ์ที่ไม่มีการพึ่งพา native</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="แปลงไฟล์ DICOM ใน C#">}}

<p>เมธอด <code>DicomFile.Transcode</code> แปลงไฟล์ DICOM จาก Transfer Syntax ปัจจุบันไปยัง Transfer Syntax เป้าหมายที่รองรับ เมธอดจะคืนค่าเป็นอ็อบเจ็กต์ <code>DicomFile</code> ใหม่ &mdash; ไฟล์เดิมจะไม่ถูกเปลี่ยนแปลง:</p>

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

<p>คุณสามารถแปลงรหัสที่ระดับ <code>Dataset</code> ได้โดยตรงเช่นกัน:</p>

<div class="codeblock" id="code">
 <h3>แปลงรหัส Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transfer Syntax ที่รองรับ">}}

<p>ตารางต่อไปนี้แสดง Transfer Syntax มาตรฐานทั้งหมดสำหรับข้อมูลภาพ DICOM พร้อมสถานะการรองรับปัจจุบันใน Aspose.Medical for .NET ทุก codec ที่รองรับถูกพัฒนาขึ้นด้วย C# แบริสุทธิ์และเป็นอิสระต่อแพลตฟอร์มอย่างเต็มที่</p>

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
<tr><td colspan="4"><strong>Uncompressed</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Uncompressed</td><td>รองรับ</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Uncompressed</td><td>รองรับ</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Uncompressed (retired)</td><td>รองรับ</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Uncompressed</td><td>ไม่รองรับ</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>รองรับ</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>บีบอัดแบบสูญเสีย, 8-bit</td><td>รองรับ</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>บีบอัดแบบสูญเสีย, 12-bit</td><td>รองรับ</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>ไม่มีการสูญเสีย</td><td>รองรับ</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>ไม่มีการสูญเสีย</td><td>รองรับ</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>ไม่มีการสูญเสีย</td><td>รองรับ</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>เกือบไม่มีการสูญเสีย</td><td>รองรับ</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>ไม่มีการสูญเสีย</td><td>รองรับ</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>บีบอัดแบบสูญเสียหรือไม่มีการสูญเสีย</td><td>รองรับ</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>ไม่มีการสูญเสีย</td><td>ไม่รองรับ</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>บีบอัดแบบสูญเสียหรือไม่มีการสูญเสีย</td><td>ไม่รองรับ</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>ไม่มีการสูญเสีย</td><td>รองรับ</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>ไม่มีการสูญเสีย</td><td>รองรับ</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>ไม่มีการสูญเสีย</td><td>รองรับ</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>บีบอัดแบบสูญเสียหรือไม่มีการสูญเสีย</td><td>รองรับ</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>ไม่มีการสูญเสีย</td><td>รองรับ</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>ไม่มีการสูญเสีย</td><td>ถอดรหัสเท่านั้น (การเข้ารหัสต้องใช้สตรีม JPEG ต้นฉบับ, ไม่ใช่ข้อมูลพิกเซล)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>บีบอัดแบบสูญเสียหรือไม่มีการสูญเสีย</td><td>รองรับ (โหมดบีบอัดแบบสูญเสีย)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="กรณีการแปลงรหัสที่พบบ่อย">}}

<p>กระบวนการทำงานที่แตกต่างกันต้องการกลยุทธ์การแปลงรหัสที่แตกต่าง นี่คือสถานการณ์ที่พบบ่อยที่สุด:</p>

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
 <h3>บีบอัดสำหรับการส่งผ่านเครือข่าย - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>ใช้ codec ล่าสุด: HTJ2K และ JPEG XL - C#</h3>
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

<p>คลาส <code>TransferSyntax</code> เปิดเผย property ที่อธิบายลักษณะการเข้ารหัส ใช้เพื่อตรวจสอบ Transfer Syntax ปัจจุบันของไฟล์หรือเลือก Transfer Syntax ปลายทางที่เหมาะสม:</p>

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
<th>Type</th>
<th>คำอธิบาย</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>ตัวระบุแบบยูนิกของ Transfer Syntax</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>ระบุว่าการแสดงค่า (Value Representations) ถูกเข้ารหัสอย่างชัดเจนหรือไม่</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>ระบุว่าการเรียงลำดับไบต์เป็นแบบ little endian หรือไม่</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>ระบุว่าข้อมูลพิกเซลถูก encapsulated (บีบอัด) หรือไม่</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>ระบุว่าวิธีการบีบอัดเป็นแบบสูญเสียหรือไม่</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>ระบุว่า syntax ใช้การบีบอัดแบบ deflate หรือไม่</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>ระบุว่า Transfer Syntax นี้ถูกยกเลิกโดยมาตรฐาน DICOM หรือไม่</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>ตัวระบุมาตรฐาน ISO ของวิธีการบีบอัดแบบสูญเสีย</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การบีบอัดแบบสูญเสีย vs ไม่มีการสูญเสีย">}}

<p>การทำความเข้าใจความแตกต่างระหว่างการบีบอัดแบบสูญเสียและไม่มีการสูญเสียเป็นสิ่งสำคัญเมื่อต้องแปลงรหัสไฟล์ DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>แง่มุม</th>
<th>ไม่มีการสูญเสีย</th>
<th>สูญเสีย</th>
</tr>
</thead>
<tbody>
<tr><td>คุณภาพภาพ</td><td>Pixel-perfect — ข้อมูลดั้งเดิมถูกเก็บไว้ครบถ้วน</td><td>ข้อมูลบางส่วนสูญหายถาวรเพื่อให้ได้ขนาดที่เล็กลง</td></tr>
<tr><td>อัตราการบีบอัด</td><td>Typically 2:1 to 3:1</td><td>Typically 10:1 to 30:1 or higher</td></tr>
<tr><td>ปลอดภัยต่อการทำซ้ำ</td><td>ใช่ — ถอดรหัสแล้วได้พิกเซลเหมือนเดิม</td><td>ไม่ — การเข้ารหัสแบบสูญเสียซ้ำต่อเนื่องทำให้คุณภาพลดลง</td></tr>
<tr><td>กรณีการใช้งาน</td><td>เก็บถาวร, การวินิจฉัย, บันทึกทางกฎหมาย</td><td>การตรวจเชิงแรกรอบ, โทรเวช, การส่งผ่านเครือข่าย</td></tr>
<tr><td>Codec ที่รองรับ</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG Extended, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>สำคัญ:</strong> การแปลงรหัสจากไฟล์ที่บีบอัดแบบสูญเสียไปเป็น Transfer Syntax แบบไม่มีการสูญเสียจะไม่ทำให้ข้อมูลที่สูญหายกลับคืน คุณภาพที่ลดลงจากการบีบอัดแบบสูญเสียเดิมเป็นถาวร</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งข้อมูลการเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารอธิบาย" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="ซอร์สโค้ด" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนแบบชำระเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องเลือก Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายการลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
