---
title: DICOM JPEG 2000 Compression in C# .NET | Aspose.Medical
weight: 2000
description: Read, write, and transcode DICOM files with JPEG 2000 compression in C# .NET. Support for 8-bit color and 16-bit monochrome images, lossless and lossy modes, plus HTJ2K with Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM JPEG 2000 Support in .NET C#" h2="Read, write, and transcode DICOM files with JPEG 2000 compression. Lossless and lossy modes, 8-bit color and 16-bit monochrome pixel data, HTJ2K included - all in pure .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 in Medical Imaging">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) is the most widely used wavelet-based compression standard in medical imaging. Unlike traditional JPEG, it offers both lossless and lossy compression in a single codec, progressive decoding for region-of-interest access, and superior compression ratios &mdash; making it ideal for archiving large studies and transmitting images over constrained networks.</p>

<p><strong>Aspose.Medical for .NET</strong> provides a pure C# implementation of the JPEG 2000 codec with no native dependencies. The library can read, render, and transcode DICOM files compressed with any of the four standard JPEG 2000 transfer syntaxes.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Supported JPEG 2000 Transfer Syntaxes">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Mode</th>
<th>Read</th>
<th>Write</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>8-bit RGB, 16-bit monochrome</td><td>16-bit monochrome, 8-bit RGB</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy or lossless</td><td>8-bit RGB, 16-bit monochrome</td><td>16-bit monochrome, 8-bit RGB</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Not supported</td><td>Not supported</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy or lossless</td><td>Not supported</td><td>Not supported</td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Monochrome and color</td><td>Monochrome and color</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Monochrome and color</td><td>Monochrome and color</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy or lossless</td><td>Monochrome and color</td><td>Monochrome and color</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8-Bit and 16-Bit Pixel Data">}}

<p>Medical images often use 16 bits per sample to capture the full dynamic range of modalities like CT (typically 12-bit stored in 16-bit) and MRI. Aspose.Medical handles both bit depths for JPEG 2000:</p>

<ul>
<li><strong>Reading (decompression)</strong>: 16-bit monochrome files (CT, MRI, X-ray) and 8-bit three-component color files (RGB, YBR_RCT, YBR_ICT). Palette, CMYK, ICC-profile and sub-sampled color code streams are rejected with a clear exception instead of a silently wrong image.</li>
<li><strong>Writing (compression)</strong>: 16-bit monochrome and 8-bit RGB images. 8-bit monochrome and 16-bit color encoding are not available; use HTJ2K or JPEG XL for those, both accept monochrome and color at either bit depth.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Read and inspect JPEG 2000 compressed DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Transcode to JPEG 2000">}}

<p>Use the <code>Transcode</code> method to compress any DICOM file to JPEG 2000 or to convert between JPEG 2000 modes:</p>

<div class="codeblock" id="code">
 <h3>Compress DICOM to JPEG 2000 Lossless - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Compress DICOM to JPEG 2000 Lossy - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Decompress JPEG 2000 DICOM Files">}}

<p>Decompress JPEG 2000 files to an uncompressed transfer syntax for processing, analysis, or compatibility with systems that do not support JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Decompress JPEG 2000 to uncompressed - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>You can also decompress and transcode to other compression formats in a single step:</p>

<div class="codeblock" id="code">
 <h3>Transcode between compression formats - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render JPEG 2000 DICOM Images">}}

<p>JPEG 2000 compressed DICOM files can be rendered to pixel data for display or export, just like any other transfer syntax:</p>

<div class="codeblock" id="code">
 <h3>Render a JPEG 2000 compressed frame - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Lossless vs Lossy JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Aspect</th>
<th>JPEG 2000 Lossless</th>
<th>JPEG 2000 Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Transfer Syntax</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Image quality</td><td>Pixel-perfect &mdash; identical to original</td><td>Visually similar, some data permanently lost</td></tr>
<tr><td>Compression ratio</td><td>Typically 2:1 to 3:1</td><td>Typically 10:1 to 30:1 or higher</td></tr>
<tr><td>Best for</td><td>Diagnostic archival, legal records, primary reading</td><td>Preliminary review, telemedicine, network transmission</td></tr>
<tr><td>Round-trip safe</td><td>Yes</td><td>No &mdash; re-encoding further degrades quality</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) replaces the slow arithmetic coder of JPEG 2000 with a faster block coder. It keeps the same wavelet transform, progression orders and quality, and decodes and encodes several times faster. Aspose.Medical implements all three DICOM HTJ2K transfer syntaxes in pure .NET, for monochrome and color images, and transcodes between HTJ2K and every other supported syntax:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; lossless only</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; lossless with RPCL progression order</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; lossy or lossless</li>
</ul>

<div class="codeblock" id="code">
 <h3>Transcode JPEG 2000 to HTJ2K and back - C#</h3>
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
