---
title: Converteer DICOM naar XML in C# .NET | Aspose.Medical
weight: 3000
description: Serialiseer DICOM-datasets naar het standaard DICOM XML-formaat in C# .NET. Configureer bulk-gegevensverwerking, streamgebaseerde verwerking en asynchrone bewerkingen met de Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converteer DICOM naar XML in .NET C#" h2="Serialiseer DICOM-datasets naar de standaard DICOM XML-representatie (PS3.19). Configureer bulk-gegevensreferenties, streamgebaseerde output en asynchrone verwerking met een pure .NET-bibliotheek." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standaardgebaseerde DICOM XML-serialisatie">}}

<p><strong>Aspose.Medical for .NET</strong> serialiseert DICOM-gegevens naar XML volgens het <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a>. Dit is de officiële standaard voor het weergeven van DICOM-datasets in XML, gebruikt door DICOMweb‑services, integratieplatformen en systemen die een menselijk leesbare, schema‑gevalideerde representatie van medische beeldvormingsmetadata vereisen.</p>

<p>De <code>DicomXmlSerializer</code>-klasse biedt statische methoden voor zowel serialisatie als deserialisatie. In tegenstelling tot eenvoudige tag‑dump‑benaderingen, voldoet de output aan het DICOM XML‑schema waarbij elk element wordt weergegeven met zijn tag, VR en correct geformatteerde waarden &mdash; wat verliesloze round‑trip conversie tussen binaire DICOM en XML mogelijk maakt.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialiseer DICOM naar XML in C#">}}

<p>Gebruik de <code>DicomXmlSerializer</code>-klasse om een DICOM-dataset om te zetten naar een XML‑string. De eenvoudigste aanpak genereert een standaard‑conforme XML‑document:</p>

<div class="codeblock" id="code">
 <h3>Converteer DICOM naar XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stream‑gebaseerde en asynchrone serialisatie">}}

<p>Voor grote DICOM‑bestanden of scenario’s met hoge doorvoer, serialiseer rechtstreeks naar een stream om het toewijzen van grote strings in het geheugen te vermijden. Zowel synchrone als asynchrone methoden zijn beschikbaar:</p>

<div class="codeblock" id="code">
 <h3>Synchronische stream‑serialisatie - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Asynchrone stream‑serialisatie - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Pijplijn‑streaming voor grote studies">}}

<p>Complete studies hoeven niet volledig in het geheugen te worden geladen. <code>DicomXmlSerializer</code> schrijft naar een <code>PipeWriter</code> en leest van een <code>PipeReader</code>, zodat de XML kan worden geproduceerd en geconsumeerd terwijl deze stroomt, en een reeks datasets kan één voor één worden gelezen via <code>DeserializeAsyncEnumerable</code>. Elke methode accepteert een <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Serialiseren en deserialiseren via een pipe - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Lees een reeks datasets één voor één - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialisatie‑opties">}}

<p>De <code>DicomXmlSerializerOptions</code>-klasse bepaalt hoe DICOM-gegevens worden weergegeven in XML. De primaire configuratie betreft de bulk‑gegevensverwerking voor grote binaire waarden:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Eigenschap</th>
<th>Type</th>
<th>Beschrijving</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Aangepaste converter voor het schrijven van grote data (bijv. pixeldata) als BulkData‑URI‑referenties in plaats van inline</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Aangepaste loader voor het oplossen van BulkData‑URI’s tijdens deserialisatie</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Standaardoptie‑instantie die wordt gebruikt wanneer geen aangepaste opties zijn opgegeven</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Serialiseren met aangepaste opties - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk‑gegevensverwerking">}}

<p>Grote binaire waarden (pixeldata, golfformen, ingesloten documenten) kunnen worden geexternaliseerd als BulkData‑URI‑referenties in plaats van inline te worden opgenomen in de XML‑output. Dit volgt de <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">DICOM PS3.19 BulkData‑element</a>-specificatie.</p>

<p>Implementeer <code>IBulkDataConverter</code> om grote data tijdens serialisatie te externaliseren, en <code>IBulkDataLoader</code> om URI’s tijdens deserialisatie te resolven. Voor de meeste gevallen is het niet nodig een loader te schrijven: <code>DefaultBulkDataLoader.Instance</code> lost <code>file</code>, <code>http</code> en <code>https</code> URI’s op, en implementeert tevens <code>IAsyncBulkDataLoader</code>, zodat bulk‑data asynchroon wordt opgehaald op de streaming‑paden.</p>

<div class="codeblock" id="code">
 <h3>Aangepaste bulk‑gegevensverwerking - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Deserialiseer XML naar DICOM">}}

<p>Parse DICOM XML terug naar Dataset‑objecten. Ondersteunt string‑invoer, stream‑invoer en asynchrone bewerkingen:</p>

<div class="codeblock" id="code">
 <h3>Deserialiseer XML naar DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="XML vs JSON‑serialisatie">}}

<p>Aspose.Medical ondersteunt zowel DICOM XML (PS3.19) als DICOM JSON (PS3.18) serialisatie. Beide formaten bieden verliesloze round‑trip conversie, maar bedienen verschillende integratiescenario’s:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Functie</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Standaard</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>Schema‑validatie</td><td>XML‑schema (XSD) beschikbaar</td><td>Geen formeel schema</td></tr>
<tr><td>Het beste voor</td><td>Enterprise‑integratie, HL7 CDA, auditlogs, XDS‑registers</td><td>DICOMweb, REST‑API’s, FHIR ImagingStudy</td></tr>
<tr><td>Menselijke leesbaarheid</td><td>Uitgebreid maar zelfbeschrijvend</td><td>Compact en breed ondersteund</td></tr>
<tr><td>Bulk‑gegevens</td><td>BulkData‑element met URI</td><td>BulkDataURI‑eigenschap</td></tr>
<tr><td>Serializer‑klasse</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leerbronnen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Broncode" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑referenties" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Productondersteuning" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis ondersteuning" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betaalde ondersteuning" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Waarom Aspose.Medical voor .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Klantlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
