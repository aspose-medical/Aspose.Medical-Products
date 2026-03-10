---
title: Konvertera DICOM till JSON i C# .NET | Aspose.Medical
weight: 4000
description: Serialisera DICOM-filer till standard DICOM JSON-format i C# .NET. Konfigurera nummerhantering, bulkdata-URI:er, nyckelordsutmatning och asynkron strömbehandling med Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konvertera DICOM till JSON i .NET C#" h2="Serialisera DICOM-dataset och -filer till den standard DICOM JSON-modellen (PS3.18). Konfigurera nummerhantering, bulkdatareferenser, nyckelordsutmatning och strömbaserad asynkron behandling." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standard DICOM JSON-modell">}}

<p><strong>Aspose.Medical för .NET</strong> serialiserar DICOM-data till JSON enligt <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>. Detta är den officiella standarden för att representera DICOM-dataset i JSON, som används av DICOMweb-tjänster, FHIR ImagingStudy-resurser och moderna vård-API:er.</p>

<p>I DICOM JSON-modellen representeras varje attribut av sin tagg som JSON-nyckel (t.ex. <code>"00100010"</code> för Patient Name), med Value Representation och värdearray som nästlade egenskaper:</p>

<div class="codeblock" id="code">
 <h3>DICOM JSON-modellformat</h3>
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

{{< blocks/products/pf/feature-page-section h2="Serialisera DICOM till JSON i C#">}}

<p>Använd klassen <code>DicomJsonSerializer</code> för att konvertera en DICOM-fil eller ett dataset till JSON. Det enklaste tillvägagångssättet producerar kompakt, standardkompatibel utdata:</p>

<div class="codeblock" id="code">
 <h3>Konvertera DICOM till JSON - C#</h3>
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

<p>Du kan serialisera ett <code>Dataset</code>, en komplett <code>DicomFile</code> (inklusive File Meta Information), eller en array av dataset:</p>

<div class="codeblock" id="code">
 <h3>Serialisera DicomFile och dataset-arrayer - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Strömbaserad och asynkron serialisering">}}

<p>För stora DICOM-filer eller höggenomströmningsscenarier, serialisera direkt till en UTF-8-ström för att undvika att allokera stora strängar i minnet. Både synkrona och asynkrona metoder finns tillgängliga:</p>

<div class="codeblock" id="code">
 <h3>Ström- och async-serialisering - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Serialiseringsalternativ">}}

<p>Klassen <code>DicomJsonSerializerOptions</code> styr hur DICOM-data representeras i JSON:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Egenskap</th>
<th>Typ</th>
<th>Beskrivning</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Styr om numeriska VR:er (IS, DS, SV, UV) skrivs som JSON-nummer eller strängar</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Använd DICOM-nyckelord (t.ex. <code>"PatientName"</code>) istället för taggar (<code>"00100010"</code>) som JSON-nycklar. Obs: icke-standard</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Inkludera DICOM-nyckelordet som ett ytterligare JSON-attribut</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Inkludera DICOM-taggens namn som ett ytterligare JSON-attribut</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Anpassad konverterare för att skriva stora data (t.ex. pixeldata) som URI-referenser</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Anpassad laddare för att lösa bulkdata-URI:er under deserialisering</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Konfigurera serialiseringsalternativ - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Nummerhantering">}}

<p>DICOM numeriska Value Representations (IS, DS, SV, UV) kan serialiseras som JSON-nummer eller JSON-strängar. Detta är viktigt för interoperabilitet, eftersom vissa DICOM-implementationer lagrar tal med inledande mellanslag eller icke-standardformat:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Läge</th>
<th>JSON-utdata</th>
<th>Användningsfall</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>Standardkonform, ideal för beräkningar. Kastar undantag om värdet inte kan parsas.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>Bevarar original strängrepresentation. Säkerare för rundresning av icke-standarddata.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulkdata-hantering">}}

<p>Stora binära värden (pixeldata, vågformer, inkapslade dokument) kan hanteras som bulkdata-referenser istället för att infogas i JSON-utdata. Detta följer <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>-specifikationen.</p>

<p>Implementera <code>IBulkDataConverter</code> för att externalisera stora data under serialisering, och <code>IBulkDataLoader</code> för att lösa URI:er under deserialisering:</p>

<div class="codeblock" id="code">
 <h3>Anpassad bulkdata-hantering - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Deserialisera JSON till DICOM">}}

<p>Parsa DICOM JSON tillbaka till Dataset- eller DicomFile-objekt. Stöder stränginmatning, ströminmatning och asynkrona strömoperationer:</p>

<div class="codeblock" id="code">
 <h3>Deserialisera JSON till DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="System.Text.Json-integration">}}

<p>Aspose.Medical tillhandahåller <code>DatasetJsonConverter</code> och <code>DicomFileJsonConverter</code> för integration med den standard <code>System.Text.Json</code>-serialiseringspipeline. Detta gör att DICOM-objekt kan serialiseras tillsammans med andra .NET-typer i din befintliga JSON‑behandlingskod:</p>

<div class="codeblock" id="code">
 <h3>System.Text.Json-integration - C#</h3>
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
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Källkod" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API-referenser" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktstöd" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betald support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blogg" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Varför Aspose.Medical för .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundlista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Framgångshistorier" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
