---
title: Převod DICOM do JSON v C# .NET | Aspose.Medical
weight: 4000
description: Serializujte soubory DICOM do standardního formátu DICOM JSON v C# .NET. Nakonfigurujte zpracování čísel, URI bulk dat, výstup klíčových slov a asynchronní zpracování streamu pomocí Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Převod DICOM do JSON v .NET C#" h2="Serializujte datové sady a soubory DICOM do standardního modelu DICOM JSON (PS3.18). Nakonfigurujte zpracování čísel, odkazy na bulk data, výstup klíčových slov a asynchronní zpracování založené na streamu." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standardní model DICOM JSON">}}

<p><strong>Aspose.Medical for .NET</strong> serializuje data DICOM do JSON podle <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">modelu DICOM PS3.18 JSON</a>. Jedná se o oficiální standard pro reprezentaci datových sad DICOM v JSON, který používají služby DICOMweb, zdroje FHIR ImagingStudy a moderní API ve zdravotnictví.</p>

<p>V modelu DICOM JSON je každý atribut reprezentován svým tagem jako klíčem JSON (např. <code>"00100010"</code> pro Patient Name), přičemž Value Representation a pole hodnot jsou vnořené vlastnosti:</p>

<div class="codeblock" id="code">
 <h3>Formát modelu DICOM JSON</h3>
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

{{< blocks/products/pf/feature-page-section h2="Serializujte DICOM do JSON v C#">}}

<p>Použijte třídu <code>DicomJsonSerializer</code> pro konverzi souboru DICOM nebo datové sady do JSON. Nejjednodušší přístup vytváří kompaktní výstup kompatibilní se standardem:</p>

<div class="codeblock" id="code">
 <h3>Převod DICOM do JSON - C#</h3>
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

<p>Můžete serializovat <code>Dataset</code>, kompletní <code>DicomFile</code> (včetně File Meta Information), nebo pole datových sad:</p>

<div class="codeblock" id="code">
 <h3>Serializace DicomFile a polí datových sad - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serializace založená na streamu a asynchronní">}}

<p>Pro velké soubory DICOM nebo scénáře s vysokou propustností serializujte přímo do UTF-8 streamu, abyste se vyhnuli alokaci velkých řetězců v paměti. K dispozici jsou jak synchronní, tak asynchronní metody:</p>

<div class="codeblock" id="code">
 <h3>Streamová a asynchronní serializace - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Možnosti serializace">}}

<p>Třída <code>DicomJsonSerializerOptions</code> řídí, jak jsou data DICOM reprezentována v JSON:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Vlastnost</th>
<th>Typ</th>
<th>Popis</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Řídí, zda jsou číselné VR (IS, DS, SV, UV) zapisovány jako JSON čísla nebo řetězce</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Použít DICOM klíčová slova (např. <code>"PatientName"</code>) místo tagů (<code>"00100010"</code>) jako klíče JSON. Pozn.: nestandardní</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Vložit DICOM klíčové slovo jako další atribut JSON</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Vložit název DICOM tagu jako další atribut JSON</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Vlastní konvertor pro zápis velkých dat (např. pixelových dat) jako URI odkazy</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Vlastní načítač pro řešení URI bulk dat během deserializace</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Nastavení možností serializace - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Zpracování čísel">}}

<p>Číselné Value Representations (IS, DS, SV, UV) v DICOM mohou být serializovány jako JSON čísla nebo JSON řetězce. To je důležité pro interoperabilitu, protože některé implementace DICOM ukládají čísla s úvodními mezerami nebo v nestandardním formátu:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Režim</th>
<th>Výstup JSON</th>
<th>Případ použití</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>Kompatibilní se standardem, ideální pro výpočty. Vyvolá výjimku, pokud hodnotu nelze parsovat.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>Uchovává původní řetězcovou reprezentaci. Bezpečnější pro round‑tripování nestandardních dat.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zpracování bulk dat">}}

<p>Velké binární hodnoty (pixelová data, vlnové formy, enkapsulované dokumenty) mohou být zpracovány jako odkazy na bulk data místo vkládání přímo do výstupu JSON. Toto odpovídá specifikaci <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>.</p>

<p>Implementujte <code>IBulkDataConverter</code> pro externí ukládání velkých dat během serializace a <code>IBulkDataLoader</code> pro řešení URI během deserializace:</p>

<div class="codeblock" id="code">
 <h3>Vlastní zpracování bulk dat - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Deserializace JSON do DICOM">}}

<p>Parsujte DICOM JSON zpět do objektů Dataset nebo DicomFile. Podporuje vstup jako řetězec, stream a asynchronní operace se streamem:</p>

<div class="codeblock" id="code">
 <h3>Deserializace JSON do DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Integrace System.Text.Json">}}

<p>Aspose.Medical poskytuje <code>DatasetJsonConverter</code> a <code>DicomFileJsonConverter</code> pro integraci se standardní pipelineserializací <code>System.Text.Json</code>. To umožňuje serializovat objekty DICOM společně s ostatními .NET typy ve vašem stávajícím kódu pro zpracování JSON:</p>

<div class="codeblock" id="code">
 <h3>Integrace System.Text.Json – C#</h3>
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
{{< blocks/products/pf/slr-tab tabTitle="Výukové materiály" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Zdrojový kód" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Reference API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Podpora produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Bezplatná podpora" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Placená podpora" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Proč Aspose.Medical pro .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Seznam zákazníků" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Úspěšné příběhy" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
