---
title: แปลง DICOM เป็น PNG ใน C# .NET | Aspose.Medical
weight: 11000
description: เรนเดอร์ภาพ DICOM ไปยังข้อมูลพิกเซลใน C# .NET ด้วยคุณภาพแบบไม่มีการสูญเสีย, การควบคุม window/level, และการรองรับหลายเฟรม. ใช้ pipeline LUT ของ DICOM เต็มรูปแบบกับ Aspose.Medical API และบันทึกเป็น PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="แปลง DICOM เป็น PNG ใน .NET C#" h2="เรนเดอร์ภาพ DICOM ไปยังข้อมูลพิกเซลดิบด้วยการสนับสนุน pipeline เกรayscale เต็มรูปแบบ. รักษาคุณภาพภาพด้วยการควบคุม window/level, การเรนเดอร์ overlay, และการประมวลผลหลายเฟรม. บันทึกเป็น PNG แบบไม่มีการสูญเสียโดยใช้ไลบรารีการประมวลผลภาพ .NET ใดก็ได้." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="การแปลง DICOM เป็น PNG">}}

<p><strong>Aspose.Medical for .NET</strong> เรนเดอร์ภาพ DICOM ผ่าน pipeline เกรayscale มาตรฐานของ DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT) เพื่อสร้างข้อมูลพิกเซล BGRA32 ที่มีการปรับ window อย่างถูกต้อง. PNG เป็นรูปแบบที่ไม่มีการสูญเสีย ทำให้เป็นตัวเลือกที่ต้องการเมื่อต้องรักษาคุณภาพภาพโดยไม่ให้เกิด artifacts จากการบีบอัด &mdash; จำเป็นสำหรับการรีวิววินิจฉัย, การจัดเก็บระยะยาว, และกรณีการวิจัย.</p>

<p>ผลลัพธ์ที่เรนเดอร์เป็น <code>IRawImage</code> &mdash; แบฟเฟอร์พิกเซล BGRA32 ที่มี <code>Width</code>, <code>Height</code> และอาเรย์ <code>Pixels</code>. คุณสามารถบันทึกข้อมูลพิกเซลนี้เป็น PNG โดยใช้ไลบรารีการประมวลผลภาพ .NET ใดก็ได้ เช่น <code>System.Drawing</code>, <code>SkiaSharp</code> หรือ <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เรนเดอร์ DICOM เป็นข้อมูลพิกเซลใน C#">}}

<p>โหลดไฟล์ DICOM, เรนเดอร์เฟรมที่ต้องการ, และเข้าถึงข้อมูลพิกเซล:</p>

<div class="codeblock" id="code">
 <h3>เรนเดอร์ภาพ DICOM - C#</h3>
 <pre><code class="cs">using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;

// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Render the first frame (index 0) through the full LUT pipeline
IRawImage image = dicomFile.RenderImage(0);

// Access the rendered pixel data
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = image.Pixels; // BGRA32 pixel buffer</code></pre>
</div>

<p>เมธอด <code>RenderImage</code> จะประยุกต์ค่า window/level และพารามิเตอร์ LUT ที่เก็บไว้ในชุดข้อมูล DICOM โดยอัตโนมัติ</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="บันทึกภาพที่เรนเดอร์เป็น PNG">}}

<p><code>IRawImage</code> ให้ข้อมูลพิกเซล BGRA32 ดิบที่สามารถบันทึกเป็น PNG แบบไม่มีการสูญเสียโดยใช้ไลบรารีการประมวลผลภาพ .NET ใดก็ได้. ตัวอย่างต่อไปนี้ใช้ <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>บันทึกเฟรม DICOM ที่เรนเดอร์เป็น PNG - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");
IRawImage image = dicomFile.RenderImage(0);

// Create a Bitmap from the BGRA32 pixel data
using var bitmap = new Bitmap(image.Width, image.Height, PixelFormat.Format32bppArgb);
var bitmapData = bitmap.LockBits(
    new Rectangle(0, 0, image.Width, image.Height),
    ImageLockMode.WriteOnly,
    PixelFormat.Format32bppArgb);

// Copy BGRA32 pixels to the bitmap
MemoryMarshal.AsBytes(image.Pixels.AsSpan())
    .CopyTo(new Span&lt;byte&gt;(
        (void*)bitmapData.Scan0,
        bitmapData.Stride * bitmapData.Height));

bitmap.UnlockBits(bitmapData);

// Save as lossless PNG
bitmap.Save("ct_scan.png", ImageFormat.Png);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Window/Level แบบกำหนดเอง">}}

<p>ควบคุมช่วงค่าพิกเซลที่เรนเดอร์โดยตั้งค่า Window Center และ Window Width. การตั้งค่า window ที่แตกต่างกันจะเปิดเผยโครงสร้างกายวิภาคที่ต่างกันจากข้อมูล DICOM เดียวกัน:</p>

<div class="codeblock" id="code">
 <h3>เรนเดอร์ด้วย window/level แบบกำหนดเอง - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);</code></pre>
</div>

<p>คลาส <code>GrayscaleRenderOptions</code> ยังเปิดเผยคุณสมบัติ <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> และ <code>BitDepth</code> เพื่อให้คุณควบคุม pipeline การเรนเดอร์ได้อย่างเต็มที่</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การเรนเดอร์ DICOM แบบหลายเฟรม">}}

<p>ไฟล์ DICOM จาก CT, MRI, และอัลตราซาวนด์มักมีหลายเฟรม (slice). เรนเดอร์ทุกเฟรมเป็นข้อมูลพิกเซล:</p>

<div class="codeblock" id="code">
 <h3>เรนเดอร์ทุกเฟรมจาก DICOM แบบหลายเฟรม - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as PNG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การควบคุม Overlay">}}

<p>ภาพ DICOM อาจมี plane overlay ที่มีคำอธิบาย, การวัด, หรือพื้นที่ที่สนใจ. ควบคุมการมองเห็นและสีของ overlay ขณะเรนเดอร์:</p>

<div class="codeblock" id="code">
 <h3>เรนเดอร์พร้อมและไม่มี overlay - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible (default)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions { ShowOverlays = false };
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG สำหรับภาพการแพทย์">}}

<p>การเลือกใช้ระหว่าง PNG และ JPG ขึ้นอยู่กับกรณีการใช้งาน:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>ลักษณะ</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Compression</strong></td><td>ไม่มีการสูญเสีย</td><td>สูญเสีย</td></tr>
<tr><td><strong>Image Quality</strong></td><td>Pixel-perfect &mdash; ไม่มี artifacts</td><td>อาจมี artifacts จากการบีบอัดเล็กน้อย</td></tr>
<tr><td><strong>File Size</strong></td><td>ใหญ่กว่า (โดยปกติ 2&ndash;10x เทียบกับ JPG)</td><td>เล็กกว่า</td></tr>
<tr><td><strong>Transparency</strong></td><td>รองรับ (alpha channel)</td><td>ไม่รองรับ</td></tr>
<tr><td><strong>Best For</strong></td><td>การรีวิววินิจฉัย, การจัดเก็บ, การวิจัย, overlays</td><td>การแชร์บนเว็บ, ภาพย่อ, ตัวอย่าง</td></tr>
<tr><td><strong>Regulatory</strong></td><td>แนะนำสำหรับกระบวนการที่ต้องการคุณภาพสูง</td><td>ยอมรับได้สำหรับการใช้งานที่ไม่ใช่วินิจฉัย</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET ใช้ pipeline การเรนเดอร์เดียวกันสำหรับทั้งสองเป้าหมายการส่งออก. ข้อมูลพิกเซล <code>IRawImage</code> มีความเท่าเทียมกันไม่ว่ารูปแบบเป้าหมายจะเป็นอะไร — การเลือกรูปแบบจะมีผลต่อขั้นตอนการบันทึกขั้นสุดท้ายที่ทำโดยไลบรารีการประมวลผลภาพของคุณเท่านั้น.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="ซอร์สโค้ด" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบชำระเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องใช้ Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายการลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
