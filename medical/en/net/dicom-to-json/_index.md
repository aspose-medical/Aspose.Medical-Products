---
title: Convert DICOM to JSON in C# .NET | Aspose.Medical
weight: 4000
description: Serialize DICOM files to standard DICOM JSON format in C# .NET. Configure number handling, bulk data URIs, keyword output, and async stream processing with Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convert DICOM to JSON in .NET C#" h2="Serialize DICOM datasets and files to the standard DICOM JSON Model (PS3.18). Configure number handling, bulk data references, keyword output, and stream-based async processing." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standard DICOM JSON Model">}}

<p><strong>Aspose.Medical for .NET</strong> serializes DICOM data to JSON following the <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>. This is the official standard for representing DICOM datasets in JSON, used by DICOMweb services, FHIR ImagingStudy resources, and modern healthcare APIs.</p>

<p>In the DICOM JSON Model, each attribute is represented by its tag as the JSON key (e.g., <code>"00100010"</code> for Patient Name), with the Value Representation and value array as nested properties:</p>

<div class="codeblock" id="code">
 <h3>DICOM JSON Model format</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialize DICOM to JSON in C#">}}

<p>Use the <code>DicomJsonSerializer</code> class to convert a DICOM file or dataset to JSON. The simplest approach produces compact, standards-compliant output:</p>

<div class="codeblock" id="code">
 <h3>Convert DICOM to JSON - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p>You can serialize a <code>Dataset</code>, a complete <code>DicomFile</code> (including File Meta Information), or an array of datasets:</p>

<div class="codeblock" id="code">
 <h3>Serialize DicomFile and dataset arrays - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stream-Based and Async Serialization">}}

<p>For large DICOM files or high-throughput scenarios, serialize directly to a UTF-8 stream to avoid allocating large strings in memory. Both synchronous and async methods are available:</p>

<div class="codeblock" id="code">
 <h3>Stream and async serialization - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialization Options">}}

<p>The <code>DicomJsonSerializerOptions</code> class controls how DICOM data is represented in JSON:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Property</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Controls whether numeric VRs (IS, DS, SV, UV) are written as JSON numbers or strings</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Use DICOM keywords (e.g., <code>"PatientName"</code>) instead of tags (<code>"00100010"</code>) as JSON keys. Note: non-standard</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Include the DICOM keyword as an additional JSON attribute</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Include the DICOM tag name as an additional JSON attribute</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Custom converter for writing large data (e.g., pixel data) as URI references</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Custom loader for resolving bulk data URIs during deserialization</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Configure serialization options - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Number Handling">}}

<p>DICOM numeric Value Representations (IS, DS, SV, UV) can be serialized as JSON numbers or JSON strings. This is important for interoperability, since some DICOM implementations store numbers with leading spaces or non-standard formatting:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Mode</th>
<th>JSON Output</th>
<th>Use Case</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>Standard-compliant, ideal for computation. Throws if value can't be parsed.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>Preserves original string representation. Safer for round-tripping non-standard data.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk Data Handling">}}

<p>Large binary values (pixel data, waveforms, encapsulated documents) can be handled as bulk data references instead of being inlined in the JSON output. This follows the <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a> specification.</p>

<p>Implement <code>IBulkDataConverter</code> to externalize large data during serialization, and <code>IBulkDataLoader</code> to resolve URIs during deserialization:</p>

<div class="codeblock" id="code">
 <h3>Custom bulk data handling - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Deserialize JSON to DICOM">}}

<p>Parse DICOM JSON back into Dataset or DicomFile objects. Supports string input, stream input, and async stream operations:</p>

<div class="codeblock" id="code">
 <h3>Deserialize JSON to DICOM - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="System.Text.Json Integration">}}

<p>Aspose.Medical provides <code>DatasetJsonConverter</code> and <code>DicomFileJsonConverter</code> for integration with the standard <code>System.Text.Json</code> serialization pipeline. This allows DICOM objects to be serialized alongside other .NET types in your existing JSON processing code:</p>

<div class="codeblock" id="code">
 <h3>System.Text.Json integration - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
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
