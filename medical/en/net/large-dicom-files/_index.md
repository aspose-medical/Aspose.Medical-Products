---
title: Work with Large DICOM Files in C# .NET | Aspose.Medical
weight: 11500

description: Open multi-frame studies and whole slide images in C# without loading them into memory. Read metadata without pixel data, defer large elements, and move files through streams and pipes.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Large DICOM Files in .NET C#" h2="Read the metadata of a multi-frame study without the pixels, defer large elements until something asks for them, and move whole files through streams and pipes." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="The file is large, the question is usually small">}}

<p>A whole slide image, a long CT series or an OCT volume is hundreds of megabytes, and most of it is pixel data. The work an application actually does is often much smaller: list what is in a folder, check a patient identifier, count the frames, decide where a study should go. Loading every byte to answer that is what turns a simple job into a memory problem.</p>

<p><strong>Aspose.Medical for .NET</strong> lets the caller decide how much of a file is read. The choice is one argument on <code>DicomFile.Open</code>, and it applies to files, streams and pipes alike.</p>

<p>Measured on a 14 MB study with 128 frames from our test set, on the same machine and the same file:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Reading strategy</th>
<th>Time to open</th>
<th>Memory allocated</th>
</tr>
</thead>
<tbody>
<tr><td>Everything, the default</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Large elements skipped</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Large elements deferred</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>The gap grows with the file. A folder of 10,000 studies is the case where it stops being a micro-optimization.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Read the metadata, leave the pixels alone">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> leaves every element above a size threshold out of the read. The dataset that comes back has the tags an index or a router needs.</p>

<div class="codeblock" id="code">
 <h3>Read a study without its pixel data - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>The threshold defaults to 64 kB and takes a value in kilobytes, so a workflow that treats 8 kB as large can say so.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Defer instead of skip">}}

<p>When the pixels may be needed, but probably later and probably not all of them, <code>ReadLargeOnDemand</code> is the other half of the pair. Opening the file costs the same as skipping, and a large element is read at the moment the code touches it.</p>

<div class="codeblock" id="code">
 <h3>Load a frame only when it is used - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>Deferred reading is a licensed feature; the other strategies work in evaluation as well.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Index a folder without touching the pixels">}}

<p>The same strategy applies to a stream, which is what an archive scan or a cloud object store looks like from the code.</p>

<div class="codeblock" id="code">
 <h3>Scan an archive - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streams and pipes, in and out">}}

<p>Reading and writing both accept streams, and the asynchronous entry points also accept <code>System.IO.Pipelines</code> types. A study can travel from a network response to storage without the process ever holding the whole file as one array.</p>

<div class="codeblock" id="code">
 <h3>Read and write through streams - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>The same idea covers the text representations: a document with many datasets is read one dataset at a time on the <a href="/medical/net/json-to-dicom/">JSON to DICOM</a> and <a href="/medical/net/xml-to-dicom/">XML to DICOM</a> pages.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Frame by frame">}}

<p>Multi-frame data is addressed per frame, so a 500 frame series costs one frame at a time rather than the whole pixel data element.</p>

<div class="codeblock" id="code">
 <h3>Walk the frames - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Where this decides the design">}}

<ul>
<li>Archive indexing and migration: millions of files, and only the header matters until something is moved.</li>
<li>Routers and store nodes: accept a study, read what is needed to route it, pass the bytes on.</li>
<li>AI pipelines: build the manifest from metadata, then pull frames for the subset that is actually trained on.</li>
<li>Containers with a memory limit: the working set follows the strategy, not the file size.</li>
<li>Whole slide and OCT data: files where reading everything is not an option at all.</li>
</ul>

<p>The <a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">memory management guide</a> explains the strategies in detail, and <a href="/medical/net/dicom-networking/">DICOM networking</a> shows the same data arriving over DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Learning Resources" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Developer Guide" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
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
