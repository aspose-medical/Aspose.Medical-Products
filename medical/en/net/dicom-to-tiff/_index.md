---
title: Convert DICOM to TIFF in C# .NET | Aspose.Medical
weight: 12000
description: Convert DICOM to TIFF in C# .NET with multi-page support for multi-frame DICOM files. Render medical images to TIFF with window/level control and overlay rendering using Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convert DICOM to TIFF in .NET C#" h2="Render DICOM images to TIFF format with multi-page support for multi-frame files. Apply window/level settings, overlay rendering, and grayscale pipeline control." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM to TIFF Conversion">}}

<p><strong>Aspose.Medical for .NET</strong> converts DICOM images to TIFF using the standard DICOM grayscale rendering pipeline. TIFF is a versatile format widely used in medical imaging, scientific research, and publishing because it supports lossless compression, high bit depths, and &mdash; most importantly &mdash; <strong>multi-page documents</strong>. This makes TIFF the natural choice for converting multi-frame DICOM files (CT, MRI, ultrasound) into a single output file that preserves all slices.</p>

<p>The rendering process applies the full DICOM LUT chain (Modality LUT, VOI LUT, Presentation LUT, Output LUT) to produce correctly windowed 8-bit BGRA32 images.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Convert DICOM to TIFF in C#">}}

<p>Convert a single-frame DICOM file to TIFF:</p>

<div class="codeblock" id="code">
 <h3>Convert DICOM to TIFF - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("xray.dcm");

// Render the first frame
var image = dicomFile.RenderImage(0);

// Save as TIFF
image.Save("xray.tiff");</code></pre>
</div>

<p>The <code>RenderImage</code> method automatically applies window/level values and LUT parameters from the DICOM dataset.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Multi-Page TIFF from Multi-Frame DICOM">}}

<p>Multi-frame DICOM files (e.g., CT or MRI series) contain multiple image slices in a single file. TIFF's multi-page capability allows you to store all frames in one output file, preserving the complete study in a universally readable format:</p>

<div class="codeblock" id="code">
 <h3>Convert multi-frame DICOM to multi-page TIFF - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;
Console.WriteLine($"Total frames: {totalFrames}");

// Render all frames
var frames = new List&lt;IRawImage&gt;();
for (int i = 0; i < totalFrames; i++)
{
    frames.Add(dicomFile.RenderImage(i));
}

// Save as multi-page TIFF (all frames in one file)
frames.SaveAsMultiPageTiff("ct_series.tiff");</code></pre>
</div>

<p>You can also save each frame as a separate TIFF file when individual slices are needed:</p>

<div class="codeblock" id="code">
 <h3>Convert each frame to a separate TIFF - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

for (int i = 0; i < dicomFile.NumberOfFrames; i++)
{
    var image = dicomFile.RenderImage(i);
    image.Save($"frame_{i:D4}.tiff");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Custom Window/Level">}}

<p>Apply custom window/level settings to control contrast and brightness when rendering DICOM images to TIFF:</p>

<div class="codeblock" id="code">
 <h3>Render with custom window/level - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
dicomFile.RenderImage(softTissue, 0).Save("soft_tissue.tiff");

// Bone window
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
dicomFile.RenderImage(bone, 0).Save("bone.tiff");</code></pre>
</div>

<p>The <code>GrayscaleRenderOptions</code> class also provides <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code>, and <code>BitDepth</code> properties for complete control over the rendering pipeline.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Overlay Rendering">}}

<p>Control overlay visibility when converting DICOM to TIFF. DICOM overlays contain annotations, measurements, and regions of interest that may or may not be needed in the output:</p>

<div class="codeblock" id="code">
 <h3>Control overlay rendering - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Include overlays in the output
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
dicomFile.RenderImage(withOverlays, 0).Save("with_overlays.tiff");

// Exclude overlays for a clean image
var noOverlays = new RenderOptions { ShowOverlays = false };
dicomFile.RenderImage(noOverlays, 0).Save("clean.tiff");</code></pre>
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

<p>TIFF is the preferred format when you need to preserve a complete multi-frame DICOM study in a single file or when the output is intended for archival, printing, or further image processing in third-party tools.</p>

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
