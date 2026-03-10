---
title: แปลง DICOM เป็น TIFF ด้วย C# .NET | Aspose.Medical
weight: 12000
description: เรนเดอร์ภาพ DICOM เป็นข้อมูลพิกเซลใน C# .NET และบันทึกเป็น TIFF พร้อมรองรับหลายหน้า สำหรับไฟล์ DICOM หลายเฟรม ใช้ pipeline LUT ของ DICOM เต็มรูปแบบกับ Aspose.Medical API และบันทึกเป็น TIFF ด้วยไลบรารีการประมวลผลภาพใด ๆ ของ .NET
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="แปลง DICOM เป็น TIFF ด้วย .NET C#" h2="เรนเดอร์ภาพ DICOM เป็นข้อมูลพิกเซลดิบพร้อมการสนับสนุน pipeline grayscale เต็มรูปแบบ รักษาคุณภาพภาพด้วยการควบคุม window/level การเรนเดอร์ overlay และการประมวลผลหลายเฟรม บันทึกเป็น TIFF &mdash; รวมถึง multi-page TIFF &mdash; ด้วยไลบรารีการประมวลผลภาพใด ๆ ของ .NET" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="การแปลง DICOM เป็น TIFF">}}

<p><strong>Aspose.Medical for .NET</strong> เรนเดอร์ภาพ DICOM ผ่าน pipeline grayscale มาตรฐานของ DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT) เพื่อสร้างข้อมูลพิกเซล BGRA32 ที่มีการปรับ window อย่างถูกต้อง TIFF เป็นรูปแบบที่อเนกประสงค์และใช้กันอย่างกว้างขวางในด้านการทำภาพทางการแพทย์ งานวิจัยทางวิทยาศาสตร์ และการตีพิมพ์ เนื่องจากรองรับการบีบอัดแบบ lossless ความลึกบิตสูง และ &mdash; ที่สำคัญที่สุด &mdash; <strong>เอกสารหลายหน้า</strong> สิ่งนี้ทำให้ TIFF เป็นตัวเลือกที่ธรรมชาติสำหรับการแปลงไฟล์ DICOM หลายเฟรม (CT, MRI, อัลตราซาวด์) ให้เป็นไฟล์ผลลัพธ์เดียวที่รักษาทุกรอบภาพไว้</p>

<p>ผลลัพธ์ที่เรนเดอร์เป็น <code>IRawImage</code> &mdash; บัฟเฟอร์พิกเซล BGRA32 ที่มี <code>Width</code>, <code>Height</code> และอาเรย์ <code>Pixels</code> คุณสามารถบันทึกข้อมูลพิกเซลนี้เป็น TIFF ด้วยไลบรารีการประมวลผลภาพใด ๆ ของ .NET เช่น <code>System.Drawing</code>, <code>SkiaSharp</code> หรือ <code>ImageSharp</code></p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เรนเดอร์ DICOM เป็นข้อมูลพิกเซลด้วย C#">}}

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

<p>เมธอด <code>RenderImage</code> จะทำการใช้ค่า window/level และพารามิเตอร์ LUT ที่จัดเก็บในชุดข้อมูล DICOM โดยอัตโนมัติ</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="บันทึกภาพที่เรนเดอร์เป็น TIFF">}}

<p><code>IRawImage</code> ให้ข้อมูลพิกเซล BGRA32 ดิบที่สามารถบันทึกเป็น TIFF ด้วยไลบรารีการประมวลผลภาพใด ๆ ของ .NET นี่คือตัวอย่างการใช้ <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>บันทึกเฟรม DICOM ที่เรนเดอร์เป็น TIFF - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("xray.dcm");
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

// Save as TIFF
bitmap.Save("xray.tiff", ImageFormat.Tiff);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Multi-Page TIFF จาก Multi-Frame DICOM">}}

<p>ไฟล์ DICOM แบบหลายเฟรม (เช่น ชุด CT หรือ MRI) มีหลายสไลซ์ภาพอยู่ในไฟล์เดียว ความสามารถ multi-page ของ TIFF ช่วยให้คุณจัดเก็บทุกเฟรมในไฟล์ผลลัพธ์เดียว รักษาการศึกษาแบบครบถ้วนในรูปแบบที่สามารถอ่านได้ทั่วโลก ใช้ encoder TIFF ของ <code>System.Drawing</code> พร้อมพารามิเตอร์หลายเฟรม:</p>

<div class="codeblock" id="code">
 <h3>แปลง Multi-Frame DICOM เป็น Multi-Page TIFF - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
int totalFrames = dicomFile.NumberOfFrames;

// Get the TIFF codec
var tiffCodec = ImageCodecInfo.GetImageEncoders()
    .First(c =&gt; c.FormatID == ImageFormat.Tiff.Guid);

// Multi-frame parameters
var multiFrameParams = new EncoderParameters(2);
multiFrameParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.MultiFrame);
multiFrameParams.Param[1] = new EncoderParameter(
    Encoder.Compression, (long)EncoderValue.CompressionLZW);

// Parameter for adding subsequent frames
var frameAddParams = new EncoderParameters(1);
frameAddParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.FrameDimensionPage);

// Parameter for flushing the final frame
var flushParams = new EncoderParameters(1);
flushParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.Flush);

Bitmap? tiffBitmap = null;

for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var frameBitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = frameBitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    frameBitmap.UnlockBits(bitmapData);

    if (i == 0)
    {
        // Save the first frame and start multi-page TIFF
        tiffBitmap = (Bitmap)frameBitmap.Clone();
        tiffBitmap.Save("ct_series.tiff", tiffCodec, multiFrameParams);
    }
    else
    {
        // Add subsequent frames
        tiffBitmap!.SaveAdd(frameBitmap, frameAddParams);
    }
}

// Flush and close the multi-page TIFF
tiffBitmap?.SaveAdd(flushParams);
tiffBitmap?.Dispose();</code></pre>
</div>

<p>คุณยังสามารถบันทึกแต่ละเฟรมเป็นไฟล์ TIFF แยกกันได้เมื่อต้องการสไลซ์แต่ละอัน:</p>

<div class="codeblock" id="code">
 <h3>แปลงแต่ละเฟรมเป็น TIFF แยกไฟล์ - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

for (int i = 0; i &lt; dicomFile.NumberOfFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var bitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = bitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    bitmap.UnlockBits(bitmapData);
    bitmap.Save($"frame_{i:D4}.tiff", ImageFormat.Tiff);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="กำหนด Window/Level เอง">}}

<p>ควบคุมช่วงค่าพิกเซลที่จะแสดงโดยตั้งค่า Window Center และ Window Width การตั้งค่า window ที่ต่างกันจะทำให้มองเห็นโครงสร้างทางกายวิภาคที่แตกต่างกันจากข้อมูล DICOM เดียวกัน</p>

<div class="codeblock" id="code">
 <h3>เรนเดอร์ด้วยการกำหนด window/level เอง - C#</h3>
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
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

<p>คลาส <code>GrayscaleRenderOptions</code> ยังให้เข้าถึงคุณสมบัติ <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> และ <code>BitDepth</code> เพื่อควบคุม pipeline การเรนเดอร์อย่างเต็มที่</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การเรนเดอร์ Multi-Frame DICOM">}}

<p>ไฟล์ DICOM จาก CT, MRI และอัลตราซาวนด์มักมีหลายเฟรม (สไลซ์) เรนเดอร์ทุกเฟรมเป็นข้อมูลพิกเซล:</p>

<div class="codeblock" id="code">
 <h3>เรนเดอร์ทุกเฟรมจาก Multi-Frame DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as TIFF using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การควบคุม Overlay">}}

<p>ภาพ DICOM อาจมีชั้น overlay ที่มีคำอธิบาย การวัด หรือบริเวณสนใจต่าง ๆ ควบคุมการแสดงผลและสีของ overlay ขณะเรนเดอร์ได้</p>

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
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG สำหรับภาพทางการแพทย์">}}

<p>แต่ละรูปแบบผลลัพธ์เหมาะกับกรณีการใช้งานที่ต่างกัน:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>ลักษณะ</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>การบีบอัด</strong></td><td>Lossless (LZW, ZIP) หรือไม่บีบอัด</td><td>Lossless</td><td>Lossy</td></tr>
<tr><td><strong>Multi-Page</strong></td><td>รองรับ &mdash; ทุกเฟรมในไฟล์เดียว</td><td>ไม่รองรับ</td><td>ไม่รองรับ</td></tr>
<tr><td><strong>คุณภาพภาพ</strong></td><td>Pixel-perfect</td><td>Pixel-perfect</td><td>อาจมีศูนย์ศตมุตรจากการบีบอัด</td></tr>
<tr><td><strong>ขนาดไฟล์</strong></td><td>ใหญ่ที่สุด</td><td>ปานกลาง</td><td>เล็กที่สุด</td></tr>
<tr><td><strong>ความโปร่งแสง</strong></td><td>รองรับ</td><td>รองรับ</td><td>ไม่รองรับ</td></tr>
<tr><td><strong>เหมาะที่สุดสำหรับ</strong></td><td>การเก็บถาวร, ชุดหลายเฟรม, การพิมพ์, งานวิจัย</td><td>การตรวจวินิจฉัย, เว็บที่ต้องการคุณภาพ</td><td>การแชร์บนเว็บ, รูปย่อ</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET ใช้ pipeline การเรนเดอร์เดียวกันสำหรับทุกเป้าหมายการส่งออก ข้อมูลพิกเซล <code>IRawImage</code> จะเหมือนกันไม่ว่าจะเป็นรูปแบบใด การเลือกรูปแบบจะส่งผลต่อขั้นตอนการบันทึกสุดท้ายที่ไลบรารีการประมวลผลภาพของคุณทำเท่านั้น TIFF เป็นรูปแบบที่แนะนำเมื่อคุณต้องการเก็บการศึกษา DICOM แบบหลายเฟรมครบถ้วนในไฟล์เดียวหรือเมื่อต้องการผลลัพธ์เพื่อการเก็บถาวร, การพิมพ์, หรือการประมวลผลภาพต่อไป</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="ซอร์สโค้ด" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนแบบชำระเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องเลือก Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="กรณีความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
