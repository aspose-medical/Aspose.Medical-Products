---
title: JPEG XL for DICOM in C# .NET | Aspose.Medical
weight: 10500

description: Store DICOM images in JPEG XL from C#. Lossless JPEG XL that returns the pixels bit for bit, in a single managed assembly with no native codec to deploy.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL for DICOM in .NET C#" h2="The newest compression in the DICOM standard, with the smallest lossless files we measured, implemented in managed C# and shipped inside one assembly." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Why JPEG XL reached DICOM">}}

<p>Medical archives grow and never shrink. JPEG XL is the codec the imaging world designed after two decades of JPEG and JPEG 2000 experience, and DICOM added it as a transfer syntax for the reason storage teams care about: for the same pixels, the file is smaller.</p>

<p><strong>Aspose.Medical for .NET</strong> writes and reads JPEG XL through a C# port of libjxl that lives inside the library. The package ships one assembly, <code>Aspose.Medical.dll</code>, and no native binary beside it, so a codec this new does not turn into a deployment project: the same assembly runs on Windows, on Linux, on a build agent and in a container.</p>

<p>Two transfer syntaxes carry the pixels:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), for diagnostic data that has to come back unchanged.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), for the cases where a smaller file matters more than an exact copy.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Compress a study, keep every pixel">}}

<p>Transcoding is one call, and the dataset around the pixels travels with it.</p>

<div class="codeblock" id="code">
 <h3>Transcode a DICOM file to JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>We measured it on a 1714 by 1933 16-bit image from our own test set: 6.3 MB uncompressed becomes 2.7 MB in JPEG XL lossless, which is smaller than the same image in HTJ2K lossless. Your own numbers depend on the modality, so run the comparison over a folder of your files before choosing.</p>

<p>Lossless is the word to take literally here. Transcode to JPEG XL and back, and the pixel data equals the bytes you started with, so an archive can be recompressed without a discussion about diagnostic quality.</p>

<div class="codeblock" id="code">
 <h3>Back to an uncompressed syntax - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Read what is already stored as JPEG XL">}}

<p>A file that arrives in JPEG XL opens like any other. The transfer syntax says what it is, and the pixel data is available once the frame is decoded.</p>

<div class="codeblock" id="code">
 <h3>Open a JPEG XL file - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL or HTJ2K">}}

<p>Both are recent, both are lossless when you ask for lossless, and the library writes and reads both. They answer different questions.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Question</th>
<th>Answer</th>
</tr>
</thead>
<tbody>
<tr><td>Which produced the smaller file in our test</td><td>JPEG XL lossless, by a few percent</td></tr>
<tr><td>Which is built for progressive viewing over a network</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, especially the RPCL variant</td></tr>
<tr><td>Which entered the DICOM standard first</td><td>HTJ2K, so more archives accept it today</td></tr>
<tr><td>Which costs a native dependency here</td><td>Neither, both are managed code in one assembly</td></tr>
</tbody>
</table>

<p>The choice usually comes from the other side of the link: transcode to the syntax the archive accepts, and keep the rest of the pipeline the same.</p>

<div class="codeblock" id="code">
 <h3>Let the target archive decide - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Where it pays off">}}

<ul>
<li>Long-term archives: the same studies, fewer terabytes, and no loss to justify to a radiologist.</li>
<li>Cloud storage bills: the saving repeats every month, while the transcoding runs once.</li>
<li>Data sets for research and AI: smaller copies move faster between storage and training.</li>
<li>Deployment: a codec this new normally means a native build per platform; here it is part of the assembly you already reference.</li>
</ul>

<p>The library also writes the codecs an existing archive is full of: JPEG, JPEG-LS, JPEG 2000, HTJ2K and RLE. The <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> page covers the whole set, <a href="/medical/net/htj2k/">HTJ2K</a> has its own page, and <a href="/medical/net/jpeg2000/">JPEG 2000</a> is where both of the new codecs come from.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Learning Resources" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Developer Guide" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
