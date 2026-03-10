---
title: Converteer DICOM naar JSON in C# .NET | Aspose.Medical
weight: 4000
description: Serialiseer DICOM‑bestanden naar het standaard DICOM JSON‑formaat in C# .NET. Configureer getalafhandeling, bulk‑data‑URI’s, sleutelwoorduitvoer en asynchrone streamverwerking met de Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converteer DICOM naar JSON in .NET C#" h2="Serialiseer DICOM‑datasets en -bestanden naar het standaard DICOM JSON‑model (PS3.18). Configureer getalafhandeling, bulk‑data‑referenties, sleutelwoorduitvoer en op stream gebaseerde asynchrone verwerking." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standaard DICOM JSON‑model">}}

<p><strong>Aspose.Medical for .NET</strong> serialiseert DICOM‑data naar JSON volgens het <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON‑model</a>. Dit is de officiële standaard voor het weergeven van DICOM‑datasets in JSON, gebruikt door DICOMweb‑services, FHIR ImagingStudy‑resources en moderne zorg‑API’s.</p>

<p>In het DICOM JSON‑model wordt elk attribuut weergegeven door zijn tag als JSON‑sleutel (bijv. <code>"00100010"</code> voor Patient Name), met de Value Representation en waardelijst als geneste eigenschappen:</p>

<div class="codeblock" id="code">
 <h3>DICOM JSON‑modelindeling</h3>
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

{{< blocks/products/pf/feature-page-section h2="Serialiseer DICOM naar JSON in C#">}}

<p>Gebruik de <code>DicomJsonSerializer</code>-klasse om een DICOM‑bestand of -dataset naar JSON te converteren. De eenvoudigste aanpak levert compacte, standaarden‑conforme output op:</p>

<div class="codeblock" id="code">
 <h3>Converteer DICOM naar JSON - C#</h3>
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

<p>U kunt een <code>Dataset</code>, een volledig <code>DicomFile</code> (inclusief File Meta Information), of een array van datasets serialiseren:</p>

<div class="codeblock" id="code">
 <h3>Serialiseer DicomFile- en dataset‑arrays - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Op stream gebaseerde en asynchrone serialisatie">}}

<p>Voor grote DICOM‑bestanden of scenario’s met hoge doorvoersnelheid kunt u direct naar een UTF‑8‑stream serialiseren om het toewijzen van grote strings in het geheugen te vermijden. Zowel synchroon als asynchroon methoden zijn beschikbaar:</p>

<div class="codeblock" id="code">
 <h3>Stream‑ en async‑serialisatie - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Serialisatie‑opties">}}

<p>De <code>DicomJsonSerializerOptions</code>-klasse bepaalt hoe DICOM‑data in JSON wordt weergegeven:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Eigenschap</th>
<th>Type</th>
<th>Beschrijving</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Regelt of numerieke VR’s (IS, DS, SV, UV) als JSON‑getallen of als strings worden geschreven</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Gebruik DICOM‑sleutelwoorden (bijv. <code>"PatientName"</code>) in plaats van tags (<code>"00100010"</code>) als JSON‑sleutels. Opmerking: niet‑standaard</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Voeg het DICOM‑sleutelwoord toe als extra JSON‑attribuut</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Voeg de DICOM‑tagnaam toe als extra JSON‑attribuut</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Aangepaste converter om grote gegevens (bijv. pixeldata) als URI‑referenties te schrijven</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Aangepaste loader om bulk‑data‑URI’s op te lossen tijdens deserialisatie</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Configureer serialisatie‑opties - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Getalafhandeling">}}

<p>DICOM‑numerieke Value Representations (IS, DS, SV, UV) kunnen worden geserialiseerd als JSON‑getallen of JSON‑strings. Dit is belangrijk voor interoperabiliteit, omdat sommige DICOM‑implementaties getallen opslaan met voorloopspaties of niet‑standaard opmaak:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modus</th>
<th>JSON‑output</th>
<th>Toepassing</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>Standaard‑conform, ideaal voor berekeningen. Geeft een fout als de waarde niet kan worden geparst.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>Behoudt de oorspronkelijke stringrepresentatie. Veiliger voor round‑tripping van niet‑standaard data.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk‑data‑afhandeling">}}

<p>Grote binaire waarden (pixeldata, waveforms, ingesloten documenten) kunnen worden behandeld als bulk‑data‑referenties in plaats van inline in de JSON‑output. Dit volgt de specificatie van de <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>.</p>

<p>Implementeer <code>IBulkDataConverter</code> om grote data extern beschikbaar te maken tijdens serialisatie, en <code>IBulkDataLoader</code> om URI’s te resolven tijdens deserialisatie:</p>

<div class="codeblock" id="code">
 <h3>Aangepaste bulk‑data‑afhandeling - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Deserialiseer JSON naar DICOM">}}

<p>Parse DICOM JSON terug naar Dataset‑ of DicomFile‑objecten. Ondersteunt string‑invoer, stream‑invoer en asynchrone stream‑operaties:</p>

<div class="codeblock" id="code">
 <h3>Deserialiseer JSON naar DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="System.Text.Json‑integratie">}}

<p>Aspose.Medical biedt <code>DatasetJsonConverter</code> en <code>DicomFileJsonConverter</code> voor integratie met de standaard <code>System.Text.Json</code>-serialisatie‑pipeline. Hierdoor kunnen DICOM‑objecten worden geserialiseerd naast andere .NET‑types in uw bestaande JSON‑verwerkingscode:</p>

<div class="codeblock" id="code">
 <h3>System.Text.Json‑integratie - C#</h3>
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
{{< blocks/products/pf/slr-tab tabTitle="Leermaterialen" tabId="resources" >}}
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
{{< blocks/products/pf/slr-element name="Klantenlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
