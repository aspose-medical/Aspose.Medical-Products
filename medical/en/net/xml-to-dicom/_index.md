---
title: Convert XML to DICOM in C# .NET | Aspose.Medical
weight: 5000

description: Build DICOM files from the Native DICOM Model XML of PS3.19 in C# .NET. Read XML from a string, a stream or a pipe, stream consecutive documents, and resolve bulk data references with Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convert XML to DICOM in .NET C#" h2="Read the Native DICOM Model XML of PS3.19 back into datasets and DICOM files. Work from a string, a stream or a pipe, stream consecutive documents, and resolve bulk data references." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standard Native DICOM Model XML">}}

<p><strong>Aspose.Medical for .NET</strong> reads the <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a> defined in DICOM PS3.19. This is the XML representation written into the standard itself, not a format Aspose invented, which is what makes it useful for integration: a system that already exchanges DICOM as XML produces documents this library accepts.</p>

<p>The document root is <code>NativeDicomModel</code>, and each attribute is a <code>DicomAttribute</code> element carrying its tag, value representation and keyword:</p>

<div class="codeblock" id="code">
 <h3>Native DICOM Model format</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>This page is the reverse direction of <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>, and both use the same class, <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Create a DICOM file from XML in C#">}}

<p><code>Deserialize</code> turns a document into a <code>Dataset</code>, and a dataset is written to disk as a DICOM file.</p>

<div class="codeblock" id="code">
 <h3>Create a DICOM file from XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>The Native DICOM Model has no File Meta Information group, so the transfer syntax is not part of the document. A dataset wrapped in a <code>DicomFile</code> is written with the default transfer syntax, Implicit VR Little Endian. To store the file with another one, transcode it, as the <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> page shows.</p>

<p>Reading DICOM XML is a licensed feature. Without an on-premise license applied the reader throws a <code>MedicalApiException</code>, so apply the license first, as the <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">licensing guide</a> describes.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streams, pipes and async">}}

<p>Every entry point has a stream overload and an asynchronous overload, and the asynchronous ones also accept a <code>PipeReader</code>. A document that arrives from a web response is parsed as it is read, without being turned into a string first.</p>

<div class="codeblock" id="code">
 <h3>Read XML from a stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Consecutive documents in one stream">}}

<p>An export from another system often holds one <code>NativeDicomModel</code> element after another in a single stream. <code>DeserializeAsyncEnumerable</code> yields one dataset per element, in input order, so the stream is processed without being held in memory. The elements follow each other directly: an XML declaration is allowed only at the very start, as in any XML input.</p>

<div class="codeblock" id="code">
 <h3>Stream consecutive documents - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk data references">}}

<p>Large values such as pixel data are not written inline. They appear as a <code>BulkData</code> element with a URI that points at the bytes, which keeps the document small. To resolve those references while reading, give the serializer a bulk data loader. <code>DefaultBulkDataLoader</code> fetches <code>file</code>, <code>http</code> and <code>https</code> URIs without authentication; for an archive that needs credentials, implement <code>IBulkDataLoader</code> or <code>IAsyncBulkDataLoader</code> yourself.</p>

<div class="codeblock" id="code">
 <h3>Resolve bulk data while reading - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Round trip with DICOM to XML">}}

<p>The two directions are meant to be used together: a study leaves as XML, passes through a system that speaks XML, and comes back as a DICOM file. Everything is managed .NET, so the same round trip runs on Windows, Linux and macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM to XML and back - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>For the options that control what the XML looks like, see the <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> page. The same pair exists for JSON: <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> and <a href="/medical/net/json-to-dicom/">JSON to DICOM</a>. The <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">serialization guide</a> covers the whole API.</p>

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