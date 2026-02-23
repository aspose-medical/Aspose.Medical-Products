---
title: Convert DICOM to JPG in C# .NET | Aspose.Medical
weight: 7000
description: Render DICOM images to pixel data in C# .NET with window/level control, overlay rendering, and multi-frame support. Use the full DICOM LUT pipeline with Aspose.Medical API and save to JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convert DICOM to JPG in .NET C#" h2="Render DICOM images to raw pixel data with full grayscale pipeline support including window/level, modality LUT, VOI LUT, and overlay rendering. Save to JPG using any .NET imaging library." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM Image Rendering Pipeline">}}

<p><strong>Aspose.Medical for .NET</strong> renders DICOM images through the standard DICOM grayscale pipeline defined in the DICOM specification. The rendering process applies a chain of Look-Up Tables (LUTs) to transform raw stored pixel values into displayable images:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; converts stored pixel values to manufacturer-independent values using Rescale Slope/Intercept or a LUT Sequence.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; applies window/level (Window Center and Window Width) to select the range of values to display, with support for Linear, Linear Exact, and Sigmoid transformations.</li>
<li><strong>Presentation LUT</strong> &mdash; maps the final values to displayable P-Values, including support for INVERSE (image inversion).</li>
<li><strong>Output LUT</strong> &mdash; converts grayscale values to RGB for display, with optional colorization using custom color maps or Palette Color tables.</li>
</ol>

<p>The rendered output is an <code>IRawImage</code> &mdash; a BGRA32 pixel buffer (8-bit per channel) with <code>Width</code>, <code>Height</code>, and a <code>Pixels</code> array. You can then save this pixel data to JPG using any .NET imaging library such as <code>System.Drawing</code>, <code>SkiaSharp</code>, or <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render DICOM to Pixel Data in C#">}}

<p>Load a DICOM file and render a frame to an <code>IRawImage</code> containing BGRA32 pixel data:</p>

<div class="codeblock" id="code">
 <h3>Render DICOM image - C#</h3>
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

<p>The <code>RenderImage</code> method automatically applies the full grayscale pipeline using the window/level values and LUT parameters stored in the DICOM file.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Save Rendered Image as JPG">}}

<p>The <code>IRawImage</code> provides raw BGRA32 pixel data that can be saved to JPG using any .NET imaging library. Here is an example using <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Save rendered DICOM frame as JPG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Custom Window/Level (Windowing)">}}

<p>Window/level (also called windowing or W/L) is the most important parameter for DICOM image rendering. It controls which range of pixel values is mapped to the visible grayscale range. The <strong>Window Center</strong> defines the midpoint and the <strong>Window Width</strong> defines the range of values displayed:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; high contrast, fewer gray levels displayed (useful for soft tissue).</li>
<li><strong>Wide window</strong> &mdash; lower contrast, more gray levels displayed (useful for bone or lung).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Render with custom window/level - C#</h3>
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

<p>Common CT window presets:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tissue Type</th>
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

{{< blocks/products/pf/feature-page-section h2="Grayscale Render Options">}}

<p>The <code>GrayscaleRenderOptions</code> class provides full control over the grayscale rendering pipeline:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Property</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Center of the displayed value range (horizontal midpoint of the VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Width of the displayed value range (controls contrast)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Modality LUT rescale slope &mdash; multiplied with stored pixel values</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Modality LUT rescale intercept &mdash; added after slope multiplication</td></tr>
<tr><td><code>Invert</code></td><td>Inverts the grayscale image (applies INVERSE Presentation LUT shape)</td></tr>
<tr><td><code>BitDepth</code></td><td>Bit depth information: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Inverted grayscale rendering - C#</h3>
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

<p>Use <code>RenderOptionsFactory.Create(pixelData)</code> to automatically populate render options from the DICOM dataset's pixel data attributes, including bit depth, rescale parameters, window/level, and inversion settings.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Overlay Rendering">}}

<p>DICOM images can contain overlay planes &mdash; graphics or text annotations burned into the image. The <code>RenderOptions</code> class controls whether overlays are rendered and what color they appear in:</p>

<div class="codeblock" id="code">
 <h3>Render with overlay control - C#</h3>
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

<p>DICOM supports two overlay types: <strong>Graphics</strong> overlays (annotations, measurements) and <strong>Region of Interest</strong> (ROI) overlays. Both types are controlled by the <code>ShowOverlays</code> setting.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Multi-Frame DICOM Rendering">}}

<p>Many DICOM files from CT, MRI, and ultrasound contain multiple frames (slices). You can check the number of frames and render each one individually:</p>

<div class="codeblock" id="code">
 <h3>Render all frames from multi-frame DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="VOI LUT Transformation Types">}}

<p>The DICOM standard defines several VOI LUT transformation functions that control how the window/level mapping is applied. Aspose.Medical for .NET supports all standard types:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Type</th>
<th>Description</th>
<th>Use Case</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Standard linear mapping with offset from center</td><td>Most common, used for general CT/MR imaging</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Linear mapping without offset from center</td><td>When precise pixel value mapping is required</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>S-curve transformation for smoother contrast transitions</td><td>Soft tissue visualization, mammography</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>Custom LUT defined as a lookup table in the DICOM dataset</td><td>Vendor-specific or modality-specific display curves</td></tr>
</tbody>
</table>

<p>The rendering engine automatically selects the appropriate VOI LUT transformation based on the DICOM dataset attributes. When using custom <code>GrayscaleRenderOptions</code>, the Linear transformation is applied by default.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Supported Modalities">}}

<p>Aspose.Medical for .NET supports rendering DICOM images from all common medical imaging modalities:</p>

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

<p>Both grayscale (MONOCHROME1/MONOCHROME2) and color (RGB, YBR_FULL, PALETTE COLOR) photometric interpretations are supported. The rendering engine automatically handles the appropriate color space conversion.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Learning Resources" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Source Code" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API References" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Product Support" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Free Support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Paid Support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Why Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Customers List" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Success Stories" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
