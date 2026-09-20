---
title: Convert JSON to DICOM in C# .NET | Aspose.Medical
weight: 6000

description: Build DICOM files from the standard DICOM JSON Model (PS3.18) in C# .NET. Read JSON from a string, a stream or a pipe, stream a sequence of datasets, and resolve bulk data references with Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convert JSON to DICOM in .NET C#" h2="Read the standard DICOM JSON Model (PS3.18) back into datasets and DICOM files. Work from a string, a stream or a pipe, stream a sequence of studies, and resolve bulk data references." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="From DICOM JSON to a DICOM file">}}

<p><strong>Aspose.Medical for .NET</strong> reads the <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>, the representation used by DICOMweb services and by systems that exchange studies over HTTP. What arrives as JSON becomes a <code>Dataset</code>, and a <code>Dataset</code> is written to disk as a DICOM file.</p>

<p>This is the reverse direction of the <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> page, and the two use the same class, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Create a DICOM file from JSON - C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>A dataset that carries no File Meta Information is written with the default transfer syntax, Implicit VR Little Endian, when it is wrapped in a <code>DicomFile</code>.</p>

<p>Reading DICOM JSON is a licensed feature. Without an on-premise license applied the reader throws a <code>MedicalApiException</code>, so apply the license first, as the <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">licensing guide</a> describes.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Keep the File Meta Information">}}

<p><code>Deserialize</code> returns the dataset alone. When the JSON document also carries the File Meta Information group, for example because it was produced from a complete DICOM file, <code>DeserializeFile</code> returns a <code>DicomFile</code> with that group intact, including the transfer syntax the file declares.</p>

<div class="codeblock" id="code">
 <h3>Read a complete DICOM file from JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streams, pipes and async">}}

<p>Every entry point has a stream overload and an asynchronous overload, and the asynchronous ones also accept a <code>PipeReader</code>. A document that arrives from a web response or from disk is read without being turned into a string first, which matters as soon as the JSON carries pixel data.</p>

<div class="codeblock" id="code">
 <h3>Read JSON from a stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="A sequence of datasets, one at a time">}}

<p>A DICOMweb query answers with an array of datasets, and such a document can be large. <code>DeserializeList</code> reads the whole array into memory; <code>DeserializeAsyncEnumerable</code> yields one dataset at a time, so the document is never held in full.</p>

<div class="codeblock" id="code">
 <h3>Stream an array of datasets - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk data references">}}

<p>The DICOM JSON Model does not carry pixel data inline. Large values are replaced by a <code>BulkDataURI</code> that points at the bytes, which keeps the JSON document small. To resolve those references while reading, give the serializer a bulk data loader. <code>DefaultBulkDataLoader</code> fetches <code>file</code>, <code>http</code> and <code>https</code> URIs without authentication; for an archive that needs credentials, implement <code>IBulkDataLoader</code> or <code>IAsyncBulkDataLoader</code> yourself.</p>

<div class="codeblock" id="code">
 <h3>Resolve BulkDataURI while reading - C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Round trip with DICOM to JSON">}}

<p>The two directions are meant to be used together: a study leaves as JSON, travels through a web service, and comes back as a DICOM file. Nothing in the process depends on native code, so the same round trip runs on Windows, Linux and macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM to JSON and back - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>For the options that control what the JSON looks like, see the <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> page. The same pair exists for XML: <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> and <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>. The <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">JSON serialization guide</a> covers the whole API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Learning Resources" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Developer Guide" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
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