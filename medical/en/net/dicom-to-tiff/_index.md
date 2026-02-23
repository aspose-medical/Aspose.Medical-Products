---
title: Convert DICOM to TIFF in C# .NET | Aspose.Medical
weight: 12000
description: Render DICOM images to pixel data in C# .NET and save to TIFF with multi-page support for multi-frame DICOM files. Use the full DICOM LUT pipeline with Aspose.Medical API and save to TIFF using any .NET imaging library.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convert DICOM to TIFF in .NET C#" h2="Render DICOM images to raw pixel data with full grayscale pipeline support. Preserve image quality with window/level control, overlay rendering, and multi-frame processing. Save to TIFF &mdash; including multi-page TIFF &mdash; using any .NET imaging library." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM to TIFF Conversion">}}

<p><strong>Aspose.Medical for .NET</strong> renders DICOM images through the standard DICOM grayscale pipeline (Modality LUT, VOI LUT, Presentation LUT, Output LUT) to produce correctly windowed BGRA32 pixel data. TIFF is a versatile format widely used in medical imaging, scientific research, and publishing because it supports lossless compression, high bit depths, and &mdash; most importantly &mdash; <strong>multi-page documents</strong>. This makes TIFF the natural choice for converting multi-frame DICOM files (CT, MRI, ultrasound) into a single output file that preserves all slices.</p>

<p>The rendered output is an <code>IRawImage</code> &mdash; a BGRA32 pixel buffer with <code>Width</code>, <code>Height</code>, and a <code>Pixels</code> array. You can save this pixel data to TIFF using any .NET imaging library such as <code>System.Drawing</code>, <code>SkiaSharp</code>, or <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render DICOM to Pixel Data in C#">}}

<p>Load a DICOM file, render the desired frame, and access the pixel data:</p>

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

<p>The <code>RenderImage</code> method automatically applies window/level values and LUT parameters stored in the DICOM dataset.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Save Rendered Image as TIFF">}}

<p>The <code>IRawImage</code> provides raw BGRA32 pixel data that can be saved to TIFF using any .NET imaging library. Here is an example using <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Save rendered DICOM frame as TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Multi-Page TIFF from Multi-Frame DICOM">}}

<p>Multi-frame DICOM files (e.g., CT or MRI series) contain multiple image slices in a single file. TIFF's multi-page capability allows you to store all frames in one output file, preserving the complete study in a universally readable format. Use the <code>System.Drawing</code> TIFF encoder with multi-frame parameters:</p>

<div class="codeblock" id="code">
 <h3>Convert multi-frame DICOM to multi-page TIFF - C#</h3>
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

<p>You can also save each frame as a separate TIFF file when individual slices are needed:</p>

<div class="codeblock" id="code">
 <h3>Convert each frame to a separate TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Custom Window/Level">}}

<p>Control which range of pixel values is rendered by setting the Window Center and Window Width. Different window settings reveal different anatomical structures from the same DICOM data:</p>

<div class="codeblock" id="code">
 <h3>Render with custom window/level - C#</h3>
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

<p>The <code>GrayscaleRenderOptions</code> class also exposes <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code>, and <code>BitDepth</code> properties for full control over the rendering pipeline.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Multi-Frame DICOM Rendering">}}

<p>DICOM files from CT, MRI, and ultrasound often contain multiple frames (slices). Render all frames to pixel data:</p>

<div class="codeblock" id="code">
 <h3>Render all frames from multi-frame DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Overlay Control">}}

<p>DICOM images may contain overlay planes with annotations, measurements, or regions of interest. Control overlay visibility and color when rendering:</p>

<div class="codeblock" id="code">
 <h3>Render with and without overlays - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG for Medical Images">}}

<p>Each output format serves different use cases:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Characteristic</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Compression</strong></td><td>Lossless (LZW, ZIP) or uncompressed</td><td>Lossless</td><td>Lossy</td></tr>
<tr><td><strong>Multi-Page</strong></td><td>Supported &mdash; all frames in one file</td><td>Not supported</td><td>Not supported</td></tr>
<tr><td><strong>Image Quality</strong></td><td>Pixel-perfect</td><td>Pixel-perfect</td><td>Compression artifacts possible</td></tr>
<tr><td><strong>File Size</strong></td><td>Largest</td><td>Medium</td><td>Smallest</td></tr>
<tr><td><strong>Transparency</strong></td><td>Supported</td><td>Supported</td><td>Not supported</td></tr>
<tr><td><strong>Best For</strong></td><td>Archival, multi-frame series, print, research</td><td>Diagnostic review, web with quality</td><td>Web sharing, thumbnails</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET uses the same rendering pipeline for all output targets. The <code>IRawImage</code> pixel data is identical regardless of the target format &mdash; the format choice only affects the final save step performed by your imaging library. TIFF is the preferred format when you need to preserve a complete multi-frame DICOM study in a single file or when the output is intended for archival, printing, or further image processing.</p>

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
