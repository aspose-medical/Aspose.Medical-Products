---
title: Konvertera DICOM till XML i C# .NET | Aspose.Medical
weight: 3000
description: Serialisera DICOM-datasets till standard DICOM XML-format i C# .NET. Konfigurera hantering av bulkdata, strömbaserad bearbetning och asynkrona operationer med Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konvertera DICOM till XML i .NET C#" h2="Serialisera DICOM-datasets till den standard DICOM XML-representationen (PS3.19). Konfigurera bulkdatreferenser, strömbaserad utmatning och asynkron behandling med ett rent .NET-bibliotek." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standardbaserad DICOM XML-serialisering">}}

<p><strong>Aspose.Medical för .NET</strong> serialiserar DICOM-data till XML enligt <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a>. Detta är den officiella standarden för att representera DICOM-datasets i XML, som används av DICOMweb-tjänster, integrationsplattformar och system som kräver en människoläst, schemavaliderad representation av metadata för medicinsk bilddiagnostik.</p>

<p>Klassen <code>DicomXmlSerializer</code> erbjuder statiska metoder för både serialisering och deserialisering. Till skillnad från enkla tagdump‑metoder följer resultatet DICOM XML‑schemat där varje element representeras med sin tagg, VR och korrekt formaterade värden &mdash; vilket möjliggör förlustfri rundresa mellan binär DICOM och XML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialisera DICOM till XML i C#">}}

<p>Använd klassen <code>DicomXmlSerializer</code> för att konvertera ett DICOM-dataset till en XML-sträng. Det enklaste tillvägagångssättet skapar ett standardkompatibelt XML-dokument:</p>

<div class="codeblock" id="code">
 <h3>Konvertera DICOM till XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ström‑baserad och asynkron serialisering">}}

<p>För stora DICOM‑filer eller scenarier med hög genomströmning, serialisera direkt till en ström för att undvika att allokera stora Strängar i minnet. Både synkrona och asynkrona metoder är tillgängliga:</p>

<div class="codeblock" id="code">
 <h3>Synkron strömserialisering - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Asynkron strömserialisering - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Pipeline‑strömning för stora studier">}}

<p>Hela studier behöver inte hållas i minnet. <code>DicomXmlSerializer</code> skriver till en <code>PipeWriter</code> och läser från en <code>PipeReader</code>, så XML kan produceras och konsumeras medan den flödar, och en sekvens av datasets kan läsas ett i taget via <code>DeserializeAsyncEnumerable</code>. Varje metod tar en <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Serialisera och deserialisera via en pipe - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Läs en sekvens av datasets ett i taget - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialiseringsalternativ">}}

<p>Klassen <code>DicomXmlSerializerOptions</code> styr hur DICOM-data representeras i XML. Den primära konfigurationen involverar bulkdatabehandling för stora binära värden:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Egenskap</th>
<th>Typ</th>
<th>Beskrivning</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Anpassad konverterare för att skriva stora data (t.ex. pixeldata) som BulkData-URI‑referenser istället för inbäddning</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Anpassad laddare för att lösa BulkData‑URI:er under deserialisering</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Standardalternativinstans som används när inga anpassade alternativ har angetts</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Serialisera med anpassade alternativ - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulkdatahantering">}}

<p>Stora binära värden (pixeldata, vågformer, kapslade dokument) kan externaliseras som BulkData‑URI‑referenser istället för att inbäddas i XML‑utdata. Detta följer <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">DICOM PS3.19 BulkData element</a>-specifikationen.</p>

<p>Implementera <code>IBulkDataConverter</code> för att externalisera stora data under serialisering, och <code>IBulkDataLoader</code> för att lösa URI:er under deserialisering. För vanliga fall behövs ingen egen laddare: <code>DefaultBulkDataLoader.Instance</code> löser <code>file</code>-, <code>http</code>- och <code>https</code>-URI:er, och den implementerar även <code>IAsyncBulkDataLoader</code>, så bulkdata hämtas asynkront i strömningsvägarna.</p>

<div class="codeblock" id="code">
 <h3>Anpassad bulkdatabehandling - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Deserialisera XML till DICOM">}}

<p>Analysera DICOM XML tillbaka till Dataset‑objekt. Stöder strängindata, strömningsindata och asynkrona operationer:</p>

<div class="codeblock" id="code">
 <h3>Deserialisera XML till DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="XML vs JSON‑serialisering">}}

<p>Aspose.Medical stödjer både DICOM XML (PS3.19) och DICOM JSON (PS3.18) serialisering. Båda formaten möjliggör förlustfri rundresa, men används i olika integrationsscenario:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Egenskap</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Standard</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>Schemainvalidering</td><td>XML‑schema (XSD) tillgängligt</td><td>Ingen formell schema</td></tr>
<tr><td>Bäst för</td><td>Enterprise‑integration, HL7 CDA, auditloggar, XDS‑register</td><td>DICOMweb, REST‑API:er, FHIR ImagingStudy</td></tr>
<tr><td>Läsbarhet för människa</td><td>Utförlig men självbeskrivande</td><td>Kompakt och brett stödjande</td></tr>
<tr><td>Bulkdata</td><td>BulkData‑element med URI</td><td>BulkDataURI‑egenskap</td></tr>
<tr><td>Serialiseringsklass</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Källkod" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑referenser" href="https://reference.aspose.com/medical/net/" >}}
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
