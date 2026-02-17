---
title: Convert DICOM to PNG in C# .NET | Aspose.Medical
weight: 11000
description: Convert DICOM to PNG in C# .NET with lossless quality, window/level control, and multi-frame support. Render medical images to PNG using the full DICOM LUT pipeline with Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convert DICOM to PNG in .NET C#" h2="Render DICOM images to lossless PNG format with full grayscale pipeline support. Preserve image quality with window/level control, overlay rendering, and multi-frame processing." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Lossless DICOM to PNG Conversion">}}

<p><strong>Aspose.Medical for .NET</strong> converts DICOM images to PNG using the standard DICOM grayscale rendering pipeline. PNG is a lossless format, making it the preferred choice when image quality must be preserved without compression artifacts &mdash; essential for diagnostic review, archival, and research use cases where every pixel matters.</p>

<p>The rendering process applies the full chain of DICOM Look-Up Tables (Modality LUT, VOI LUT, Presentation LUT, and Output LUT) to transform raw stored pixel values into displayable 8-bit BGRA32 images that can be saved directly as PNG files.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Convert DICOM to PNG in C#">}}

<p>Load a DICOM file, render the desired frame, and save it as PNG:</p>

<div class="codeblock" id="code">
 <h3>Convert DICOM to PNG - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Render the first frame (index 0)
var image = dicomFile.RenderImage(0);

// Save as lossless PNG
image.Save("ct_scan.png");</code></pre>
</div>

<p>The <code>RenderImage</code> method automatically applies window/level values and LUT parameters stored in the DICOM dataset. For most images, this produces a correctly windowed result without manual configuration.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Custom Window/Level for PNG Output">}}

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
dicomFile.RenderImage(softTissue, 0).Save("soft_tissue.png");

// Bone window
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
dicomFile.RenderImage(bone, 0).Save("bone.png");

// Lung window
var lung = new GrayscaleRenderOptions
{
    WindowCenter = -600,
    WindowWidth = 1500
};
dicomFile.RenderImage(lung, 0).Save("lung.png");</code></pre>
</div>

<p>The <code>GrayscaleRenderOptions</code> class also exposes <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code>, and <code>BitDepth</code> properties for full control over the rendering pipeline.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Multi-Frame DICOM to PNG">}}

<p>DICOM files from CT, MRI, and ultrasound often contain multiple frames (slices). Convert all frames to individual PNG files in a single loop:</p>

<div class="codeblock" id="code">
 <h3>Convert all frames to PNG - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;
Console.WriteLine($"Total frames: {totalFrames}");

// Convert each frame to a separate PNG file
for (int i = 0; i < totalFrames; i++)
{
    var image = dicomFile.RenderImage(i);
    image.Save($"frame_{i:D4}.png");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Overlay Control">}}

<p>DICOM images may contain overlay planes with annotations, measurements, or regions of interest. Control overlay visibility and color when rendering to PNG:</p>

<div class="codeblock" id="code">
 <h3>Render with and without overlays - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible (default)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
dicomFile.RenderImage(withOverlays, 0).Save("with_overlays.png");

// Render without overlays for a clean image
var noOverlays = new RenderOptions
{
    ShowOverlays = false
};
dicomFile.RenderImage(noOverlays, 0).Save("clean.png");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Inverted Grayscale Rendering">}}

<p>Some modalities (e.g., X-ray) store images with MONOCHROME1 photometric interpretation where white represents minimum values. Use the <code>Invert</code> option to flip the grayscale mapping:</p>

<div class="codeblock" id="code">
 <h3>Render inverted grayscale to PNG - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray.dcm");

var options = new GrayscaleRenderOptions
{
    WindowCenter = 2048,
    WindowWidth = 4096,
    Invert = true
};

dicomFile.RenderImage(options, 0).Save("xray_inverted.png");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG for Medical Images">}}

<p>Choosing between PNG and JPG depends on the use case:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Characteristic</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Compression</strong></td><td>Lossless</td><td>Lossy</td></tr>
<tr><td><strong>Image Quality</strong></td><td>Pixel-perfect &mdash; no artifacts</td><td>Minor compression artifacts possible</td></tr>
<tr><td><strong>File Size</strong></td><td>Larger (typically 2&ndash;10x vs JPG)</td><td>Smaller</td></tr>
<tr><td><strong>Transparency</strong></td><td>Supported (alpha channel)</td><td>Not supported</td></tr>
<tr><td><strong>Best For</strong></td><td>Diagnostic review, archival, research, overlays</td><td>Web sharing, thumbnails, previews</td></tr>
<tr><td><strong>Regulatory</strong></td><td>Preferred for quality-critical workflows</td><td>Acceptable for non-diagnostic use</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET uses the same rendering pipeline for both formats. The only difference is the output container &mdash; the DICOM image processing (windowing, LUT application, overlay rendering) is identical.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Access Rendered Pixel Data">}}

<p>The rendered <code>IRawImage</code> provides direct access to BGRA32 pixel data for custom post-processing, analysis, or integration with other imaging libraries:</p>

<div class="codeblock" id="code">
 <h3>Access pixel data from rendered image - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("scan.dcm");
IRawImage image = dicomFile.RenderImage(0);

// Dimensions
Console.WriteLine($"Size: {image.Width}x{image.Height}");

// Access a specific pixel
Bgra32 pixel = image[100, 100];
Console.WriteLine($"R={pixel.R}, G={pixel.G}, B={pixel.B}");

// Access the full pixel buffer for bulk processing
Bgra32[] pixels = image.Pixels;</code></pre>
</div>

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
