---
title: แปลง DICOM เป็น JPG ด้วย C# .NET | Aspose.Medical
weight: 7000
description: เรนเดอร์ภาพ DICOM เป็นข้อมูลพิกเซลด้วย C# .NET พร้อมการควบคุม window/level, การเรนเดอร์ overlay, และการสนับสนุนหลายเฟรม ใช้กระบวนการ LUT ของ DICOM อย่างเต็มรูปแบบกับ Aspose.Medical API และบันทึกเป็น JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="แปลง DICOM เป็น JPG ด้วย .NET C#" h2="เรนเดอร์ภาพ DICOM เป็นข้อมูลพิกเซลดิบพร้อมการสนับสนุน pipeline เกรayscaleเต็มรูปแบบ รวมถึง window/level, modality LUT, VOI LUT, และการเรนเดอร์ overlay บันทึกเป็น JPG ด้วยห้องสมุดภาพ .NET ใดก็ได้" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="กระบวนการเรนเดอร์ภาพ DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> เรนเดอร์ภาพ DICOMผ่าน pipeline เกรayscaleมาตรฐานของ DICOMที่กำหนดในสเปค DICOM กระบวนการเรนเดอร์จะใช้ชุด Look-Up Tables (LUTs) เพื่อแปลงค่าพิกเซลที่เก็บไว้เป็นภาพที่สามารถแสดงได้:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; แปลงค่าพิกเซลที่เก็บไว้เป็นค่าที่ไม่ขึ้นกับผู้ผลิตโดยใช้ Rescale Slope/Intercept หรือ LUT Sequence.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; ใช้ window/level (Window Center และ Window Width) เพื่อเลือกช่วงค่าที่จะแสดง พร้อมด้วยการสนับสนุน Linear, Linear Exact, และ Sigmoid.</li>
<li><strong>Presentation LUT</strong> &mdash; แมปค่าขั้นสุดท้ายเป็น P-Values ที่แสดงได้ รวมถึงการสนับสนุน INVERSE (การกลับภาพ).</li>
<li><strong>Output LUT</strong> &mdash; แปลงค่ากรayscaleเป็น RGB เพื่อแสดงผล พร้อมการทำสีเพิ่มเติมโดยใช้แผนที่สีกำหนดเองหรือ Palette Color tables.</li>
</ol>

<p>ผลลัพธ์ที่เรนเดอร์คือ <code>IRawImage</code> &mdash; บัฟเฟอร์พิกเซล BGRA32 (8 บิตต่อช่อง) พร้อมกับ <code>Width</code>, <code>Height</code> และอาร์เรย์ <code>Pixels</code> คุณสามารถบันทึกข้อมูลพิกเซลนี้เป็น JPG ด้วยห้องสมุดภาพ .NET ใดก็ได้ เช่น <code>System.Drawing</code>, <code>SkiaSharp</code> หรือ <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เรนเดอร์ DICOM เป็นข้อมูลพิกเซลด้วย C#">}}

<p>โหลดไฟล์ DICOM และเรนเดอร์เฟรมเป็น <code>IRawImage</code> ที่บรรจุข้อมูลพิกเซล BGRA32:</p>

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

<p>เมธอด <code>RenderImage</code> จะใช้ pipeline เกรayscaleเต็มรูปแบบโดยอัตโนมัติ ด้วยค่าต่าง ๆ ของ window/level และพารามิเตอร์ LUT ที่เก็บในไฟล์ DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="บันทึกภาพที่เรนเดอร์เป็น JPG">}}

<p><code>IRawImage</code> ให้ข้อมูลพิกเซล BGRA32 ดิบที่สามารถบันทึกเป็น JPG ด้วยห้องสมุดภาพ .NET ใดก็ได้ ตัวอย่างต่อไปนี้ใช้ <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>บันทึกเฟรม DICOM ที่เรนเดอร์เป็น JPG - C#</h3>
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

// Save as JPG
bitmap.Save("ct_scan.jpg", ImageFormat.Jpeg);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Window/Level แบบกำหนดเอง (Windowing)">}}

<p>Window/level (หรือเรียกอีกอย่างว่า windowing หรือ W/L) เป็นพารามิเตอร์ที่สำคัญที่สุดสำหรับการเรนเดอร์ภาพ DICOM มันควบคุมว่าช่วงค่าพิกเซลใดจะถูกแมปเป็นช่วงเกรayscaleที่มองเห็นได้ <strong>Window Center</strong> กำหนดตำแหน่งกึ่งกลาง และ <strong>Window Width</strong> กำหนดช่วงค่าที่แสดง:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; คอนทราสต์สูง, แสดงระดับสีเทาน้อย (เหมาะกับเนื้อเยื่ออ่อน).</li>
<li><strong>Wide window</strong> &mdash; คอนทราสต์ต่ำ, แสดงระดับสีเทามากขึ้น (เหมาะกับกระดูกหรือปอด).</li>
</ul>

<div class="codeblock" id="code">
 <h3>เรนเดอร์ด้วย window/level แบบกำหนดเอง - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window: center=40, width=400
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window: center=400, width=1800
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Lung window: center=-600, width=1500
var lung = new GrayscaleRenderOptions
{
    WindowCenter = -600,
    WindowWidth = 1500
};
IRawImage lungImage = dicomFile.RenderImage(lung, 0);</code></pre>
</div>

<p>ค่าพรีเซ็ต window ของ CT ที่พบบ่อย:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>ประเภทเนื้อเยื่อ</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Soft Tissue</td><td>40</td><td>400</td></tr>
<tr><td>Lung</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Bone</td><td>400</td><td>1800</td></tr>
<tr><td>Brain</td><td>40</td><td>80</td></tr>
<tr><td>Liver</td><td>60</td><td>150</td></tr>
<tr><td>Mediastinum</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ตัวเลือกการเรนเดอร์เกรayscale">}}

<p>คลาส <code>GrayscaleRenderOptions</code> ให้การควบคุมเต็มรูปแบบเหนือ pipeline การเรนเดอร์เกรayscale:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Property</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>จุดศูนย์กลางของช่วงค่าที่แสดง (จุดกึ่งกลางแนวนอนของ VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>ความกว้างของช่วงค่าที่แสดง (ควบคุมคอนทราสต์)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>ค่าความชันของ Modality LUT &mdash; คูณกับค่าพิกเซลที่เก็บไว้</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>ค่าตัดของ Modality LUT &mdash; เพิ่มหลังจากคูณค่าความชัน</td></tr>
<tr><td><code>Invert</code></td><td>กลับสีภาพเกรayscale (ใช้รูปแบบ INVERSE Presentation LUT)</td></tr>
<tr><td><code>BitDepth</code></td><td>ข้อมูลความลึกบิต: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>การเรนเดอร์เกรayscaleแบบกลับสี - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray.dcm");

// Render with inverted grayscale (white-on-black becomes black-on-white)
var options = new GrayscaleRenderOptions
{
    WindowCenter = 2048,
    WindowWidth = 4096,
    Invert = true
};

IRawImage image = dicomFile.RenderImage(options, 0);</code></pre>
</div>

<p>ใช้ <code>RenderOptionsFactory.Create(pixelData)</code> เพื่อเติมค่าตัวเลือกการเรนเดอร์จากคุณลักษณะข้อมูลพิกเซลของ DICOM dataset อย่างอัตโนมัติ รวมถึงความลึกบิต, พารามิเตอร์ rescale, window/level, และการตั้งค่าการกลับสี.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การเรนเดอร์ Overlay">}}

<p>ภาพ DICOM อาจมี overlay plane &mdash; กราฟิกหรือคำอธิบายข้อความที่ฝังในภาพ คลาส <code>RenderOptions</code> ควบคุมว่าจะแสดง overlay หรือไม่และสีที่แสดงเป็นสีใด:</p>

<div class="codeblock" id="code">
 <h3>เรนเดอร์ด้วยการควบคุม overlay - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible in white (default behavior)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions
{
    ShowOverlays = false
};
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

<p>DICOM รองรับ overlay สองประเภท: overlay <strong>Graphics</strong> (คำอธิบาย, การวัด) และ overlay <strong>Region of Interest</strong> (ROI) ทั้งสองประเภทควบคุมโดยการตั้งค่า <code>ShowOverlays</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การเรนเดอร์ DICOM แบบหลายเฟรม">}}

<p>ไฟล์ DICOM จำนวนมากจาก CT, MRI, และอัลตราซาวด์มีหลายเฟรม (สไลซ์) คุณสามารถตรวจสอบจำนวนเฟรมและเรนเดอร์แต่ละเฟรมแยกกันได้:</p>

<div class="codeblock" id="code">
 <h3>เรนเดอร์ทุกเฟรมจาก DICOM แบบหลายเฟรม - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;
Console.WriteLine($"Total frames: {totalFrames}");

// Render all frames to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as JPG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ประเภทการแปลง VOI LUT">}}

<p>มาตรฐาน DICOM กำหนดฟังก์ชันการแปลง VOI LUT หลายประเภทที่ควบคุมวิธีการใช้การแมป window/level Aspose.Medical for .NET รองรับประเภทมาตรฐานทั้งหมด:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Type</th>
<th>Description</th>
<th>Use Case</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>การแมปเชิงเส้นมาตรฐานที่มีออฟเซ็ตจากศูนย์กลาง</td><td>ใช้บ่อยที่สุด, ใช้สำหรับการถ่ายภาพ CT/MR ทั่วไป</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>การแมปเชิงเส้นโดยไม่มีออฟเซ็ตจากศูนย์กลาง</td><td>เมื่อจำเป็นต้องแมปค่าพิกเซลอย่างแม่นยำ</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>การแปลงรูปโค้ง S เพื่อการเปลี่ยนคอนทราสต์ที่ราบรื่นขึ้น</td><td>การแสดงผลเนื้อเยื่ออ่อน, mammography</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>LUT กำหนดเองที่ระบุเป็นตารางค้นหาใน DICOM dataset</td><td>โค้งการแสดงผลที่เฉพาะผู้ผลิตหรือเฉพาะโหมด</td></tr>
</tbody>
</table>

<p>เอนจินการเรนเดอร์จะเลือกการแปลง VOI LUT ที่เหมาะสมโดยอัตโนมัติตามคุณลักษณะของ DICOM dataset เมื่อใช้ <code>GrayscaleRenderOptions</code> แบบกำหนดเอง การแปลง Linear จะถูกใช้เป็นค่าเริ่มต้น.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="โหมดที่สนับสนุน">}}

<p>Aspose.Medical for .NET รองรับการเรนเดอร์ภาพ DICOM จากโหมดการถ่ายภาพทางการแพทย์ที่พบทั่วไปทั้งหมด:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modality</th>
<th>Description</th>
<th>Typical Format</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Computed Tomography</td><td>Grayscale, multi-frame, 12&ndash;16 bit</td></tr>
<tr><td><strong>MR</strong></td><td>Magnetic Resonance</td><td>Grayscale, multi-frame, 12&ndash;16 bit</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Computed / Digital Radiography</td><td>Grayscale, single frame, 10&ndash;16 bit</td></tr>
<tr><td><strong>US</strong></td><td>Ultrasound</td><td>Grayscale or RGB, multi-frame</td></tr>
<tr><td><strong>MG</strong></td><td>Mammography</td><td>Grayscale, high resolution, 12&ndash;16 bit</td></tr>
<tr><td><strong>XA</strong></td><td>X-Ray Angiography</td><td>Grayscale, multi-frame</td></tr>
<tr><td><strong>NM</strong></td><td>Nuclear Medicine</td><td>Grayscale, multi-frame</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Positron Emission Tomography)</td><td>Grayscale, multi-frame</td></tr>
</tbody>
</table>

<p>รองรับการตีความภาพทั้งเกรayscale (MONOCHROME1/MONOCHROME2) และสี (RGB, YBR_FULL, PALETTE COLOR) เอนจินการเรนเดอร์จะจัดการการแปลงสีสเปซที่เหมาะสมโดยอัตโนมัติ.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="โค้ดต้นฉบับ" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบชำระค่าบริการ" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องเลือก Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
