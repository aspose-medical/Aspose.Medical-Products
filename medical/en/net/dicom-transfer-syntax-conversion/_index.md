---
title: DICOM Transfer Syntax Conversion in C# .NET | Aspose.Medical
weight: 16000
description: Transcode DICOM files between transfer syntaxes in C# .NET. Support for JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, RLE, and uncompressed formats with Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM Transfer Syntax Conversion in .NET C#" h2="Transcode DICOM files between uncompressed, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, and RLE transfer syntaxes. Pure .NET library with no native dependencies." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="What Is Transfer Syntax?">}}

<p>A <strong>Transfer Syntax</strong> defines how DICOM data is encoded for storage and transmission. It specifies three key aspects: byte ordering (endianness), whether Value Representations are explicit or implicit, and the compression algorithm applied to pixel data. Every DICOM file declares its transfer syntax in the File Meta Information header.</p>

<p>Different medical devices, PACS servers, and viewing applications support different sets of transfer syntaxes. <strong>Aspose.Medical for .NET</strong> provides the <code>Transcode</code> method to convert between transfer syntaxes, enabling interoperability, storage optimization, and compatibility with processing tools &mdash; all in a pure .NET library with no native dependencies.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transcode a DICOM File in C#">}}

<p>The <code>DicomFile.Transcode</code> method converts a DICOM file from its current transfer syntax to any supported target syntax. The method returns a new <code>DicomFile</code> instance &mdash; the original remains unchanged:</p>

<div class="codeblock" id="code">
 <h3>Basic DICOM transcoding - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>You can also transcode at the <code>Dataset</code> level directly:</p>

<div class="codeblock" id="code">
 <h3>Transcode a Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Supported Transfer Syntaxes">}}

<p>The following table lists all standard DICOM image data transfer syntaxes and their current support status in Aspose.Medical for .NET. All supported codecs are implemented in pure C# and are fully platform-independent.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Type</th>
<th>Status</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Uncompressed</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Uncompressed</td><td>Supported</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Uncompressed</td><td>Supported</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Uncompressed (retired)</td><td>Supported</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Uncompressed</td><td>Not supported</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Supported</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Lossy, 8-bit</td><td>Supported</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Lossy, 12-bit</td><td>Not supported</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Lossless</td><td>Supported (8-bit only)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Lossless</td><td>Supported (8-bit only)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Near-lossless</td><td>Supported</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>Supported (read 8-bit color and 16-bit monochrome; write 16-bit monochrome or 8-bit RGB)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy or lossless</td><td>Supported (read 8-bit color and 16-bit monochrome; write 16-bit monochrome or 8-bit RGB)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Not supported</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy or lossless</td><td>Not supported</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy or lossless</td><td>Supported</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Lossless</td><td>Decode only (encoding needs a JPEG source stream, not pixel data)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Lossy or lossless</td><td>Supported (lossy mode)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Common Transcoding Scenarios">}}

<p>Different workflows require different transcoding strategies. Here are the most common scenarios:</p>

<div class="codeblock" id="code">
 <h3>Decompress for processing - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Compress for archival storage - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Compress for network transmission - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Use the newest codecs: HTJ2K and JPEG XL - C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Inspect Transfer Syntax Properties">}}

<p>The <code>TransferSyntax</code> class exposes properties that describe the encoding characteristics. Use these to inspect a file's current transfer syntax or to select an appropriate target syntax:</p>

<div class="codeblock" id="code">
 <h3>Read transfer syntax properties - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>Property</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>The unique identifier of the transfer syntax</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Whether Value Representations are explicitly encoded</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Whether byte ordering is little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Whether pixel data is encapsulated (compressed)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Whether the compression method is lossy</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Whether the syntax uses deflate compression</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Whether the transfer syntax is retired by the DICOM standard</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>The ISO standard identifier of the lossy compression method</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossy vs Lossless Compression">}}

<p>Understanding the difference between lossy and lossless compression is critical when transcoding DICOM files:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Aspect</th>
<th>Lossless</th>
<th>Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Image quality</td><td>Pixel-perfect &mdash; original data fully preserved</td><td>Some data permanently lost to achieve smaller size</td></tr>
<tr><td>Compression ratio</td><td>Typically 2:1 to 3:1</td><td>Typically 10:1 to 30:1 or higher</td></tr>
<tr><td>Round-trip safe</td><td>Yes &mdash; decompress and get identical pixels</td><td>No &mdash; each lossy re-encode further degrades quality</td></tr>
<tr><td>Use cases</td><td>Archival, diagnostics, legal records</td><td>Preliminary review, telemedicine, network transmission</td></tr>
<tr><td>Supported codecs</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>Important:</strong> Transcoding from a lossy-compressed file to a lossless syntax does not restore lost data. The quality degradation from the original lossy compression is permanent.</p>

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
