---
title: HTJ2K in C# .NET - High-Throughput JPEG 2000 for DICOM | Aspose.Medical
weight: 10000

description: Compress and read DICOM images in High-Throughput JPEG 2000 from C#. Lossless HTJ2K, the RPCL variant and lossy HTJ2K, implemented in managed .NET with no native codec to deploy.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K in .NET C#" h2="High-Throughput JPEG 2000 for DICOM: the compression the standard added for fast archives and cloud viewing, implemented in managed C# with nothing native to install." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="What HTJ2K changes">}}

<p>High-Throughput JPEG 2000 keeps the wavelet and the image quality of JPEG 2000 and replaces the part that made it slow. The block coder is new, and decoding is an order of magnitude faster, which is why the DICOM standard adopted it in three transfer syntaxes and why cloud imaging platforms moved to it.</p>

<p>For a .NET team the practical question is different: who can actually produce those files. Most libraries reach HTJ2K through a native OpenJPH build, which means a binary per platform, a build step in the container and a dependency the security review will ask about. <strong>Aspose.Medical for .NET</strong> implements the codec in managed code inside the same package that reads and writes the files, so HTJ2K works the same on Windows, on Linux and in a container, with nothing to install.</p>

<p>Three transfer syntaxes are supported, and all three both read and write:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), High-Throughput JPEG 2000 lossless.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), the lossless variant with the RPCL progression order.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Compress a study to HTJ2K">}}

<p>One call moves a file into the new syntax. The dataset, the private tags and the file meta information travel with it.</p>

<div class="codeblock" id="code">
 <h3>Transcode a DICOM file to HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>On a 1714 by 1933 16-bit image from our own test set, the file goes from 6.3 MB to 2.9 MB, and the pixels come back bit for bit. Numbers differ per modality and per image, so measure on your own data, which is one loop over the files you already have.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossless means lossless">}}

<p>Diagnostic data does not tolerate a codec that is nearly right. Transcode to HTJ2K lossless and back, and the pixel data is identical to the bytes you started with, which is a property you can assert in your own test suite before you agree to recompress an archive.</p>

<div class="codeblock" id="code">
 <h3>Back to an uncompressed syntax - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, the variant made for viewing over a network">}}

<p>The 1.2.840.10008.1.2.4.202 syntax stores the same lossless codestream in the RPCL progression order: resolution first, then position, then component, then layer. A reader that takes only the beginning of the stream gets a complete low resolution image, which is what a viewer needs when it opens a large study over a link it does not control.</p>

<div class="codeblock" id="code">
 <h3>Compress with the RPCL progression order - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Read what an archive sends you">}}

<p>The other half of the job is accepting HTJ2K from systems that already produce it. Open the file, check what it is stored as, and work with the pixel data.</p>

<div class="codeblock" id="code">
 <h3>Read an HTJ2K file - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Multi-frame images are handled frame by frame, so a long series costs memory per frame rather than per study.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Where HTJ2K earns its place">}}

<ul>
<li>Archive migration: recompress a stored study to HTJ2K lossless, cut the footprint, keep the diagnostic data intact.</li>
<li>Cloud and DICOMweb: the decode speed is what makes a browser-side or server-side viewer feel immediate on large images.</li>
<li>AI pipelines: training sets are read far more often than they are written, and decode time is the cost that repeats.</li>
<li>Containers and serverless: the codec is part of the assembly, so an image does not need a native library or a compiler in the build.</li>
</ul>

<p>The library also ships JPEG XL, the other recent addition to the standard, and the older codecs an archive is likely to hold: JPEG, JPEG-LS, JPEG 2000 and RLE. The <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> page covers the whole set, and the <a href="/medical/net/jpeg2000/">JPEG 2000</a> page covers the codec HTJ2K grew out of.</p>

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
