---
title: การบีบอัด DICOM JPEG 2000 ใน C# .NET | Aspose.Medical
weight: 2000
description: อ่าน, เขียน, และแปลงไฟล์ DICOM ด้วยการบีบอัด JPEG 2000 ใน C# .NET รองรับภาพ 8‑bit และ 16‑bit โหมด lossless และ lossy ข้อมูลหลายคอมโพเนนต์ด้วย Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="การสนับสนุน DICOM JPEG 2000 ใน .NET C#" h2="อ่าน, เขียน, และแปลงไฟล์ DICOM ด้วยการบีบอัด JPEG 2000 โหมด lossless และ lossy, ข้อมูลพิกเซล 8‑bit และ 16‑bit, ภาพหลายคอมโพเนนต์ — ทั้งหมดใน .NET แท้" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 ในการประมวลผลภาพทางการแพทย์">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) เป็นมาตรฐานการบีบอัดแบบเวฟเล็ตที่ใช้กันอย่างแพร่หลายที่สุดในภาพการแพทย์ แตกต่างจาก JPEG แบบดั้งเดิม เนื่องจากให้การบีบอัดทั้ง lossless และ lossy ใน codec ตัวเดียว การถอดรหัสแบบ progressive สำหรับการเข้าถึงส่วนที่สนใจ และอัตราการบีบอัดที่ดีกว่า &mdash; ทำให้เหมาะสำหรับการเก็บข้อมูลการศึกษาใหญ่และการส่งภาพผ่านเครือข่ายที่จำกัด</p>

<p><strong>Aspose.Medical for .NET</strong> มีการนำเสนอการทำงานของ codec JPEG 2000 ด้วย pure C# โดยไม่มีการพึ่งพา native library ไลบรารีนี้สามารถอ่าน, แสดงผล, และแปลงไฟล์ DICOM ที่บีบอัดด้วย transfer syntax JPEG 2000 ใด ๆ ทั้งสี่แบบมาตรฐาน</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transfer Syntaxes ของ JPEG 2000 ที่รองรับ">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>โหมด</th>
<th>อ่าน</th>
<th>เขียน</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>8-bit และ 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy หรือ lossless</td><td>8-bit และ 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>8-bit และ 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy หรือ lossless</td><td>8-bit และ 16-bit</td><td>8-bit</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ข้อมูลพิกเซล 8‑Bit และ 16‑Bit">}}

<p>ภาพการแพทย์มักใช้ 16 บิตต่อสแปลเพื่อบันทึกช่วงไดนามิกเต็มของอุปกรณ์เช่น CT (โดยทั่วไปเก็บเป็น 12‑bit ใน 16‑bit) และ MRI Aspose.Medical รองรับความลึกบิตทั้งสองสำหรับ JPEG 2000:</p>

<ul>
<li><strong>การอ่าน (การแตกบีบอัด)</strong>: รองรับอย่างเต็มรูปแบบสำหรับไฟล์ DICOM ที่บีบอัดด้วย JPEG 2000 ทั้งแบบ 8‑bit และ 16‑bit ไลบรารีจะแปลงข้อมูลพิกเซลอย่างถูกต้องโดยไม่คำนึงถึงค่า Bits Allocated, Bits Stored, และ High Bit ดั้งเดิม</li>
<li><strong>การเขียน (การบีบอัด)</strong>: ปัจจุบันรองรับภาพ 8‑bit การรองรับการเขียน 16‑bit กำลังอยู่ในแผนสำหรับรุ่นต่อไป</li>
</ul>

<div class="codeblock" id="code">
 <h3>อ่านและตรวจสอบ DICOM ที่บีบอัดด้วย JPEG 2000 - C#</h3>
 <pre><code class="cs">// Open a JPEG 2000 compressed DICOM file (8-bit or 16-bit)
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Check transfer syntax
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax
Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");

// Inspect pixel data bit depth
PixelData pixelData = PixelData.Create(dicomFile.Dataset);
Console.WriteLine($"Bits Allocated: {pixelData.BitsAllocated}");
Console.WriteLine($"Bits Stored: {pixelData.BitsStored}");
Console.WriteLine($"High Bit: {pixelData.HighBit}");
Console.WriteLine($"Samples Per Pixel: {pixelData.SamplesPerPixel}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="แปลงเป็น JPEG 2000">}}

<p>ใช้เมธอด <code>Transcode</code> เพื่อบีบอัดไฟล์ DICOM ใด ๆ เป็น JPEG 2000 หรือเพื่อแปลงระหว่างโหมด JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>บีบอัด DICOM เป็น JPEG 2000 Lossless - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>บีบอัด DICOM เป็น JPEG 2000 Lossy - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="แตกบีบอัดไฟล์ DICOM JPEG 2000">}}

<p>แตกบีบอัดไฟล์ JPEG 2000 ไปเป็น transfer syntax ที่ไม่มีการบีบอัดเพื่อการประมวลผล, การวิเคราะห์, หรือความเข้ากันได้กับระบบที่ไม่สนับสนุน JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>แตกบีบอัด JPEG 2000 เป็นรูปแบบไม่มีการบีบอัด - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>คุณยังสามารถแตกบีบอัดและแปลงเป็นรูปแบบการบีบอัดอื่นในขั้นตอนเดียวได้:</p>

<div class="codeblock" id="code">
 <h3>แปลงรูปแบบการบีบอัดระหว่างกัน - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="แสดงผลภาพ DICOM JPEG 2000">}}

<p>ไฟล์ DICOM ที่บีบอัดด้วย JPEG 2000 สามารถเรนเดอร์เป็นข้อมูลพิกเซลเพื่อแสดงหรือส่งออกได้เช่นเดียวกับ transfer syntax ใด ๆ</p>

<div class="codeblock" id="code">
 <h3>เรนเดอร์เฟรมที่บีบอัดด้วย JPEG 2000 - C#</h3>
 <pre><code class="cs">// Open JPEG 2000 DICOM file
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Render the first frame — decompression is handled automatically
using PixelImage&lt;Bgra32&gt; image = dicomFile.RenderImage(0);

// Copy the BGRA32 pixel data for display or export
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = new Bgra32[width * height];
image.CopyPixelsTo(pixels);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossless กับ Lossy JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>แง่มุม</th>
<th>JPEG 2000 Lossless</th>
<th>JPEG 2000 Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Transfer Syntax</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>คุณภาพภาพ</td><td>Pixel-perfect &mdash; ตรงตามต้นฉบับ</td><td>คล้ายกันเชิงภาพ, มีข้อมูลบางส่วนสูญหายถาวร</td></tr>
<tr><td>อัตราการบีบอัด</td><td>โดยทั่วไป 2:1 ถึง 3:1</td><td>โดยทั่วไป 10:1 ถึง 30:1 หรือสูงกว่า</td></tr>
<tr><td>เหมาะสำหรับ</td><td>การเก็บข้อมูลวินิจฉัย, เอกสารทางกฎหมาย, การอ่านพื้นฐาน</td><td>การตรวจสอบเบื้องต้น, โทรเวช, การส่งผ่านเครือข่าย</td></tr>
<tr><td>ปลอดภัยต่อการทำ Round‑trip</td><td>ใช่</td><td>ไม่ — การเข้ารหัสใหม่ทำให้คุณภาพลดลงต่อเนื่อง</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 Multi-Component">}}

<p>JPEG 2000 Part 2 (ISO/IEC 15444-2) ขยาย codec มาตรฐานด้วยความสามารถในการแปลงหลายคอมโพเนนต์ ซึ่งใช้สำหรับภาพการแพทย์สีและอุปกรณ์ที่สร้างข้อมูลหลายช่อง Aspose.Medical รองรับทั้งสอง transfer syntax ของ Part 2:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; การบีบอัด lossless ด้วยการแยกความสัมพันธ์ระหว่างคอมโพเนนต์เพื่อบีบอัดข้อมูลหลายช่องอย่างเหมาะสม</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; การบีบอัด lossy หรือ lossless ด้วยการแปลงหลายคอมโพเนนต์</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K) — กำลังจะเปิดตัว">}}

<p>HTJ2K (ISO/IEC 15444-15) เป็นส่วนขยายรุ่นต่อไปของ JPEG 2000 ที่ออกแบบให้มีความเร็วในการเข้ารหัสและถอดรหัสสูงขึ้นอย่างมากในขณะที่รักษาประสิทธิภาพการบีบอัดเดียวกัน คาดว่าจะเป็น codec ที่ต้องการสำหรับกระบวนการทำงานภาพการแพทย์แบบเวลาจริง</p>

<p>Aspose.Medical จะเพิ่มการสนับสนุน HTJ2K ในรุ่นต่อไป ครอบคลุมสาม transfer syntax:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; Lossless เท่านั้น</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; Lossless พร้อมลำดับการส่ง RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; Lossy หรือ lossless</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="โค้ดต้นฉบับ" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนแบบชำระเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องใช้ Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="กรณีสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
