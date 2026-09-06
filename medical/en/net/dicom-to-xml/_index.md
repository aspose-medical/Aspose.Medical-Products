---
title: Convert DICOM to XML in C# .NET | Aspose.Medical
weight: 3000
description: Serialize DICOM datasets to standard DICOM XML format in C# .NET. Configure bulk data handling, stream-based processing, and async operations with Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convert DICOM to XML in .NET C#" h2="Serialize DICOM datasets to the standard DICOM XML representation (PS3.19). Configure bulk data references, stream-based output, and async processing with a pure .NET library." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standards-Based DICOM XML Serialization">}}

<p><strong>Aspose.Medical for .NET</strong> serializes DICOM data to XML following the <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a>. This is the official standard for representing DICOM datasets in XML, used by DICOMweb services, integration platforms, and systems that require a human-readable, schema-validated representation of medical imaging metadata.</p>

<p>The <code>DicomXmlSerializer</code> class provides static methods for both serialization and deserialization. Unlike simple tag-dump approaches, the output conforms to the DICOM XML schema where each element is represented with its tag, VR, and properly formatted values &mdash; enabling lossless round-trip conversion between binary DICOM and XML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialize DICOM to XML in C#">}}

<p>Use the <code>DicomXmlSerializer</code> class to convert a DICOM dataset to an XML string. The simplest approach produces a standards-compliant XML document:</p>

<div class="codeblock" id="code">
 <h3>Convert DICOM to XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stream-Based and Async Serialization">}}

<p>For large DICOM files or high-throughput scenarios, serialize directly to a stream to avoid allocating large strings in memory. Both synchronous and async methods are available:</p>

<div class="codeblock" id="code">
 <h3>Synchronous stream serialization - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Async stream serialization - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pipeline Streaming for Large Studies">}}

<p>Whole studies do not have to be held in memory. <code>DicomXmlSerializer</code> writes to a <code>PipeWriter</code> and reads from a <code>PipeReader</code>, so the XML can be produced and consumed as it flows, and a sequence of datasets can be read one at a time through <code>DeserializeAsyncEnumerable</code>. Every method takes a <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Serialize and deserialize through a pipe - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Read a sequence of datasets one at a time - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialization Options">}}

<p>The <code>DicomXmlSerializerOptions</code> class controls how DICOM data is represented in XML. The primary configuration involves bulk data handling for large binary values:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Property</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Custom converter for writing large data (e.g., pixel data) as BulkData URI references instead of inlining</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Custom loader for resolving BulkData URIs during deserialization</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Default options instance used when no custom options are provided</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Serialize with custom options - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk Data Handling">}}

<p>Large binary values (pixel data, waveforms, encapsulated documents) can be externalized as BulkData URI references instead of being inlined in the XML output. This follows the <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">DICOM PS3.19 BulkData element</a> specification.</p>

<p>Implement <code>IBulkDataConverter</code> to externalize large data during serialization, and <code>IBulkDataLoader</code> to resolve URIs during deserialization. For the common cases there is no need to write a loader at all: <code>DefaultBulkDataLoader.Instance</code> resolves <code>file</code>, <code>http</code> and <code>https</code> URIs, and it also implements <code>IAsyncBulkDataLoader</code>, so bulk data is fetched asynchronously on the streaming paths.</p>

<div class="codeblock" id="code">
 <h3>Custom bulk data handling - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
public class FileBulkDataConverter : IBulkDataConverter
{
    public string? GetBulkDataUri(IElement element)
    {
        // Externalize pixel data to a separate file
        if (element.Tag == Tag.PixelData)
            return "file:///bulk/pixeldata.raw";
        return null; // inline all other elements
    }
}

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Deserialize XML to DICOM">}}

<p>Parse DICOM XML back into Dataset objects. Supports string input, stream input, and async operations:</p>

<div class="codeblock" id="code">
 <h3>Deserialize XML to DICOM - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="XML vs JSON Serialization">}}

<p>Aspose.Medical supports both DICOM XML (PS3.19) and DICOM JSON (PS3.18) serialization. Both formats offer lossless round-trip conversion, but serve different integration scenarios:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Feature</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Standard</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>Schema validation</td><td>XML Schema (XSD) available</td><td>No formal schema</td></tr>
<tr><td>Best for</td><td>Enterprise integration, HL7 CDA, audit logs, XDS registries</td><td>DICOMweb, REST APIs, FHIR ImagingStudy</td></tr>
<tr><td>Human readability</td><td>Verbose but self-describing</td><td>Compact and widely supported</td></tr>
<tr><td>Bulk data</td><td>BulkData element with URI</td><td>BulkDataURI property</td></tr>
<tr><td>Serializer class</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

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
