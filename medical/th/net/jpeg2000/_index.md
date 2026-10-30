---
title: การบีบอัด DICOM JPEG 2000 ใน C# .NET | Aspose.Medical
weight: 2000
description: อ่าน, เขียนและแปลงไฟล์ DICOM ด้วยการบีบอัด JPEG 2000 ใน C# .NET รองรับภาพโมโนโครมและสีจาก 1 ถึง 16 บิต, โหมด lossless และ lossy, พร้อม HTJ2K ผ่าน Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="การสนับสนุน DICOM JPEG 2000 ใน .NET C#" h2="อ่าน, เขียนและแปลงไฟล์ DICOM ด้วยการบีบอัด JPEG 2000 โหมด lossless และ lossy, ข้อมูลพิกเซลโมโนโครมและสีจาก 1 ถึง 16 บิต, รวม HTJ2K - ทั้งหมดใน .NET แบบ pure." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 ในการถ่ายภาพการแพทย์">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) เป็นมาตรฐานการบีบอัดแบบคลื่นเวฟเล็ตที่ใช้กันอย่างแพร่หลายที่สุดในงานการแพทย์ ต่างจาก JPEG แบบดั้งเดิม มันให้การบีบอัด lossless และ lossy ในโคเดกเดียว การถอดรหัสแบบก้าวหน้าเพื่อการเข้าถึงส่วนที่สนใจ และอัตราการบีบอัดที่เหนือกว่า &mdash; ทำให้เหมาะสำหรับการจัดเก็บงานวิจัยขนาดใหญ่และการส่งภาพผ่านเครือข่ายที่มีข้อจำกัด.</p>

<p><strong>Aspose.Medical for .NET</strong> ให้การนำไปใช้แบบ pure C# ของโคเดก JPEG 2000 โดยไม่มีการพึ่งพาเนทีฟ ไลบรารีสามารถอ่าน, แสดงผล, และแปลงไฟล์ DICOM ที่บีบอัดด้วยทั้งโครงสร้างการส่ง JPEG 2000 Part 1 ทั้งแบบ lossless และ lossy.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="โครงสร้างการส่ง JPEG 2000 ที่สนับสนุน">}}

<table class="table table-bordered">
<thead>
<tr>
<th>โครงสร้างการส่ง</th>
<th>UID</th>
<th>โหมด</th>
<th>อ่าน</th>
<th>เขียน</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 Lossless เท่านั้น</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>โมโนโครมและสี, 1 ถึง 16 บิต</td><td>โมโนโครม, พาเล็ตสีและ RGB, 1 ถึง 16 บิต</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy หรือ lossless</td><td>โมโนโครมและสี, 1 ถึง 16 บิต</td><td>โมโนโครมและ RGB, 1 ถึง 16 บิต</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless เท่านั้น</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>ไม่รองรับ</td><td>ไม่รองรับ</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy หรือ lossless</td><td>ไม่รองรับ</td><td>ไม่รองรับ</td></tr>
<tr><td>HTJ2K Lossless เท่านั้น</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>โมโนโครมและสี</td><td>โมโนโครมและสี</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless เท่านั้น</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>โมโนโครมและสี</td><td>โมโนโครมและสี</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy หรือ lossless</td><td>โมโนโครมและสี</td><td>โมโนโครมและสี</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ข้อมูลพิกเซลจาก 1 ถึง 16 บิต">}}

<p>ภาพการแพทย์มักใช้ 16 บิตต่อ 샘플 เพื่อบันทึกช่วงไดนามิกเต็มของโมดาลิตี้เช่น CT (โดยทั่วไป 12‑bit เก็บใน 16‑bit) และ MRI. Aspose.Medical จัดการความลึกทั้งหมดตั้งแต่ 1 ถึง 16 บิตสำหรับ JPEG 2000, ทั้งตัวอย่าง 8‑bit หรือ 16‑bit:</p>

<ul>
<li><strong>Reading (decompression)</strong>: ไฟล์โมโนโครม (CT, MRI, X-ray), ไฟล์พาเล็ตสีและไฟล์สีสามส่วนประกอบ (RGB, YBR_RCT, YBR_ICT, และป้ายสีอื่น ๆ เช่น YBR_FULL, ถอดรหัสเป็นโค้ด) ที่มี 1 ถึง 16 บิตต่อ 샘플, มีการเซ็นหรือไม่มีเซ็น. CMYK, ICC‑profile, JP2 palette และสตรีมสีที่ทำ sub‑sampling จะถูกปฏิเสธด้วยข้อยกเว้นที่ชัดเจนแทนการแสดงภาพผิดพลาดโดยเงียบ.</li>
<li><strong>Writing (compression)</strong>: ภาพโมโนโครมและ RGB ที่มี 1 ถึง 16 บิตต่อ 샘플, มีการเซ็นหรือไม่มีเซ็น, และภาพพาเล็ตสีด้วย JPEG 2000 Lossless. ภาพ YBR จะไม่ถูกเข้ารหัส หากไม่ได้ถอดรหัสเป็น RGB, เช่นเดียวกับไฟล์ JPEG YBR แบบ lossy.</li>
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

{{< blocks/products/pf/feature-page-section h2="ถอดรหัสไฟล์ DICOM JPEG 2000">}}

<p>ถอดรหัสไฟล์ JPEG 2000 ไปยังโครงสร้างการส่งที่ไม่ได้บีบอัดสำหรับการประมวลผล, การวิเคราะห์, หรือความเข้ากันได้กับระบบที่ไม่รองรับ JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>ถอดรหัส JPEG 2000 ไปเป็นแบบไม่มีการบีบอัด - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>คุณยังสามารถถอดรหัสและแปลงเป็นรูปแบบการบีบอัดอื่น ๆ ในขั้นตอนเดียวได้:</p>

<div class="codeblock" id="code">
 <h3>แปลงระหว่างรูปแบบการบีบอัด - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เรนเดอร์ภาพ DICOM JPEG 2000">}}

<p>ไฟล์ DICOM ที่บีบอัดด้วย JPEG 2000 สามารถเรนเดอร์เป็นข้อมูลพิกเซลเพื่อการแสดงผลหรือส่งออกได้เช่นเดียวกับโครงสร้างการส่งอื่น ๆ:</p>

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
<th>ลักษณะ</th>
<th>JPEG 2000 Lossless</th>
<th>JPEG 2000 Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>โครงสร้างการส่ง</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>คุณภาพภาพ</td><td>Pixel-perfect &mdash; ตรงกับต้นฉบับ</td><td>คล้ายกันในเชิงภาพ, มีข้อมูลบางส่วนสูญหายอย่างถาวร</td></tr>
<tr><td>อัตราการบีบอัด</td><td>ประมาณ 2:1 ถึง 3:1</td><td>ประมาณ 10:1 ถึง 30:1 หรือมากกว่า</td></tr>
<tr><td>เหมาะสำหรับ</td><td>การเก็บรักษาการวินิจฉัย, เอกสารทางกฎหมาย, การอ่านหลัก</td><td>การตรวจสอบเบื้องต้น, การแพทย์ทางไกล, การส่งผ่านเครือข่าย</td></tr>
<tr><td>ปลอดภัยต่อการรอบทริป</td><td>ใช่</td><td>ไม่ &mdash; การเข้ารหัสซ้ำทำให้คุณภาพลดลงต่อไป</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) แทนที่ตัวเข้ารหัสอาริธเมติกที่ช้าของ JPEG 2000 ด้วยตัวเข้ารหัสบล็อกที่เร็วกว่า มันรักษา wavelet transform, ขั้นตอนการก้าวหน้าและคุณภาพเดียวกัน, และถอดรหัสและเข้ารหัสได้เร็วหลายเท่า Aspose.Medical ใช้การดำเนินการทั้งสามโครงสร้างการส่ง DICOM HTJ2K ใน .NET แบบ pure, สำหรับภาพโมโนโครมและสี, และแปลงระหว่าง HTJ2K กับโครงสร้างการส่งอื่น ๆ ที่สนับสนุนทั้งหมด:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; lossless only</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; lossless with RPCL progression order</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; lossy or lossless</li>
</ul>

<div class="codeblock" id="code">
 <h3>แปลง JPEG 2000 เป็น HTJ2K และกลับ - C#</h3>
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
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบบอกรับค่าใช้จ่าย" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="เหตุผลที่เลือก Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="กรณีศึกษา" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
