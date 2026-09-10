---
title: การบีบอัด DICOM JPEG 2000 ใน C# .NET | Aspose.Medical
weight: 2000
description: อ่าน, เขียน และแปลงไฟล์ DICOM ด้วยการบีบอัด JPEG 2000 ใน C# .NET รองรับภาพสี 8‑บิตและภาพโมโนโครม 16‑บิต ทั้งโหมด lossless และ lossy พร้อม HTJ2K ด้วย API ของ Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="การสนับสนุน DICOM JPEG 2000 ใน .NET C#" h2="อ่าน, เขียน และแปลงไฟล์ DICOM ด้วยการบีบอัด JPEG 2000 โหมด lossless และ lossy, ข้อมูลพิกเซลสี 8‑บิตและโมโนโครม 16‑บิต รวม HTJ2K - ทั้งหมดใน .NET แท้" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 ในการถ่ายภาพทางการแพทย์">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) เป็นมาตรฐานการบีบอัดแบบเวฟเล็ตที่ใช้กันอย่างแพร่หลายที่สุดในภาพการแพทย์ ต่างจาก JPEG แบบดั้งเดิม มันให้ทั้งการบีบอัด lossless และ lossy ในโค้ดเดียว การถอดรหัสแบบก้าวหน้าเพื่อการเข้าถึงส่วนที่สนใจ (region‑of‑interest) และอัตราการบีบอัดที่เหนือกว่า &mdash; ทำให้เหมาะสำหรับการจัดเก็บการศึกษาใหญ่และการส่งภาพผ่านเครือข่ายที่มีข้อจำกัด</p>

<p><strong>Aspose.Medical for .NET</strong> ให้การนำไปใช้ด้วย C# แท้ของโค้ดเด็ค JPEG 2000 โดยไม่มีการพึ่งพาเนทีฟ ไลบรารีสามารถอ่าน, เรนเดอร์, และแปลงไฟล์ DICOM ที่บีบอัดด้วยใดๆ ในสี่รูปแบบการส่งข้อมูลมาตรฐานของ JPEG 2000</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="รูปแบบการส่งข้อมูล JPEG 2000 ที่รองรับ">}}

<table class="table table-bordered">
<thead>
<tr>
<th>รูปแบบการส่งข้อมูล</th>
<th>UID</th>
<th>โหมด</th>
<th>อ่าน</th>
<th>เขียน</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 Lossless เท่านั้น</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>RGB 8‑บิต, โมโนโครม 16‑บิต</td><td>โมโนโครม 16‑บิต, RGB 8‑บิต</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy หรือ lossless</td><td>RGB 8‑บิต, โมโนโครม 16‑บิต</td><td>โมโนโครม 16‑บิต, RGB 8‑บิต</td></tr>
<tr><td>JPEG 2000 Part 2 Multi‑component Lossless เท่านั้น</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>ไม่รองรับ</td><td>ไม่รองรับ</td></tr>
<tr><td>JPEG 2000 Part 2 Multi‑component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy หรือ lossless</td><td>ไม่รองรับ</td><td>ไม่รองรับ</td></tr>
<tr><td>HTJ2K Lossless เท่านั้น</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>โมโนโครมและสี</td><td>โมโนโครมและสี</td></tr>
<tr><td>HTJ2K พร้อม RPCL Options Lossless เท่านั้น</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>โมโนโครมและสี</td><td>โมโนโครมและสี</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy หรือ lossless</td><td>โมโนโครมและสี</td><td>โมโนโครมและสี</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ข้อมูลพิกเซล 8‑บิตและ 16‑บิต">}}

<p>ภาพการแพทย์มักใช้ 16‑บิตต่อข้อมูลย่อยเพื่อเก็บช่วงไดนามิกเต็มของโมดัลลิตี้เช่น CT (โดยทั่วไป 12‑บิตที่เก็บในรูป 16‑บิต) และ MRI. Aspose.Medical จัดการทั้งสองความลึกบิตสำหรับ JPEG 2000:</p>

<ul>
<li><strong>Reading (decompression)</strong>: ไฟล์โมโนโครม 16‑บิต (CT, MRI, X‑ray) และไฟล์สีสามคอมโพเนนท์ 8‑บิต (RGB, YBR_RCT, YBR_ICT) สตรีมโค้ดสี Palette, CMYK, ICC‑profile และการย่อยสีที่ถูก sub‑sampled จะถูกปฏิเสธด้วยข้อยกเว้นที่ชัดเจนแทนการแสดงภาพที่ผิดพลาดอย่างเงียบ</li>
<li><strong>Writing (compression)</strong>: ภาพโมโนโครม 16‑บิตและภาพ RGB 8‑บิต การเข้ารหัสโมโนโครม 8‑บิตและสี 16‑บิตไม่พร้อมใช้งาน; ใช้ HTJ2K หรือ JPEG XL สำหรับกรณีนั้น ทั้งสองรองรับโมโนโครมและสีในทุกความลึกบิต</li>
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

<p>ใช้เมธอด <code>Transcode</code> เพื่อบีบอัดไฟล์ DICOM ใดๆ เป็น JPEG 2000 หรือแปลงระหว่างโหมด JPEG 2000:</p>

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

{{< blocks/products/pf/feature-page-section h2="ถอดรหัสไฟล์ DICOM JPEG 2000">}}

<p>ถอดรหัสไฟล์ JPEG 2000 เป็นรูปแบบการส่งข้อมูลที่ไม่บีบอัดเพื่อการประมวลผล, การวิเคราะห์ หรือความเข้ากันได้กับระบบที่ไม่รองรับ JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>ถอดรหัส JPEG 2000 เป็นแบบไม่บีบอัด - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>คุณยังสามารถถอดรหัสและแปลงเป็นรูปแบบการบีบอัดอื่นในขั้นตอนเดียว:</p>

<div class="codeblock" id="code">
 <h3>แปลงระหว่างรูปแบบการบีบอัด - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เรนเดอร์ภาพ DICOM JPEG 2000">}}

<p>ไฟล์ DICOM ที่บีบอัดด้วย JPEG 2000 สามารถเรนเดอร์เป็นข้อมูลพิกเซลเพื่อการแสดงหรือส่งออกได้เช่นเดียวกับรูปแบบการส่งข้อมูลอื่นใด:</p>

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
<tr><td>รูปแบบการส่งข้อมูล</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>คุณภาพภาพ</td><td>พิกเซลสมบูรณ์ &mdash; เหมือนต้นฉบับ</td><td>คล้ายทางสายตา, ข้อมูลบางส่วนสูญหายถาวร</td></tr>
<tr><td>อัตราการบีบอัด</td><td>ประมาณ 2:1 ถึง 3:1</td><td>ประมาณ 10:1 ถึง 30:1 หรือสูงกว่า</td></tr>
<tr><td>เหมาะสำหรับ</td><td>การจัดเก็บเชิงวินิจฉัย, บันทึกทางกฎหมาย, การอ่านหลัก</td><td>การตรวจสอบเบื้องต้น, โทรเวชกรรม, การส่งผ่านเครือข่าย</td></tr>
<tr><td>ปลอดภัยเมื่อทำรอบวงจร</td><td>ใช่</td><td>ไม่ &mdash; การเข้ารหัสซ้ำจะทำให้คุณภาพลดลงต่อไป</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) แทนที่ตัวเข้ารหัสอาริธเมติกที่ช้าของ JPEG 2000 ด้วยบล็อกโค้ดเดอร์ที่เร็วขึ้น มันรักษาการแปลงเวฟเล็ต, ลำดับการก้าวหน้าและคุณภาพเดิมไว้, และทำการถอดรหัสและเข้ารหัสเร็วหลายเท่า Aspose.Medical implement ทั้งสามรูปแบบการส่งข้อมูล DICOM HTJ2K ใน .NET แท้, สำหรับภาพโมโนโครมและสี, และแปลงระหว่าง HTJ2K กับรูปแบบอื่นที่รองรับทั้งหมด:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; lossless เท่านั้น</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; lossless พร้อมลำดับการก้าวหน้า RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; lossy หรือ lossless</li>
</ul>

<div class="codeblock" id="code">
 <h3>แปลง JPEG 2000 เป็น HTJ2K และกลับกัน - C#</h3>
 <pre><code class="cs">// Transcode a JPEG 2000 file to HTJ2K, and back to classic JPEG 2000
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile htj2kFile = j2kFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2kFile.Save("htj2k_lossless.dcm");

// HTJ2K with RPCL progression order, lossless
DicomFile rpclFile = j2kFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
rpclFile.Save("htj2k_rpcl.dcm");

// HTJ2K lossy
DicomFile htj2kLossy = j2kFile.Transcode(TransferSyntax.HTJ2K);
htj2kLossy.Save("htj2k_lossy.dcm");

// Any HTJ2K file decodes back to an uncompressed transfer syntax
DicomFile uncompressed = htj2kFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decoded.dcm");</code></pre>
</div>

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

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้อง Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
