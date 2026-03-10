---
title: DICOM in JSON konvertieren in C# .NET | Aspose.Medical
weight: 4000
description: DICOM-Dateien in das standardmäßige DICOM-JSON-Format in C# .NET serialisieren. Nummernbehandlung, Bulk‑Data‑URIs, Schlüsselwortausgabe und asynchrone Stream‑Verarbeitung mit der Aspose.Medical API konfigurieren.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM in JSON konvertieren in .NET C#" h2="DICOM-Datensätze und -Dateien in das standardmäßige DICOM JSON Model (PS3.18) serialisieren. Nummernbehandlung, Bulk‑Data‑Referenzen, Schlüsselwortausgabe und streambasierte asynchrone Verarbeitung konfigurieren." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standard‑DICOM‑JSON‑Modell">}}

<p><strong>Aspose.Medical for .NET</strong> serialisiert DICOM-Daten nach JSON gemäß dem <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>. Dies ist der offizielle Standard zur Darstellung von DICOM‑Datensätzen in JSON, verwendet von DICOMweb‑Diensten, FHIR ImagingStudy‑Ressourcen und modernen Healthcare‑APIs.</p>

<p>Im DICOM JSON Model wird jedes Attribut durch sein Tag als JSON‑Schlüssel dargestellt (z. B. <code>"00100010"</code> für Patient Name), wobei die Value Representation und das Werte‑Array als verschachtelte Eigenschaften erscheinen:</p>

<div class="codeblock" id="code">
 <h3>DICOM JSON Model-Format</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM nach JSON serialisieren in C#">}}

<p>Verwenden Sie die Klasse <code>DicomJsonSerializer</code>, um eine DICOM‑Datei oder einen Datensatz in JSON zu konvertieren. Der einfachste Ansatz erzeugt kompakte, normkonforme Ausgabe:</p>

<div class="codeblock" id="code">
 <h3>DICOM in JSON konvertieren – C#</h3>
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

<p>Sie können ein <code>Dataset</code>, ein komplettes <code>DicomFile</code> (einschließlich File Meta Information) oder ein Array von Datasets serialisieren:</p>

<div class="codeblock" id="code">
 <h3>DicomFile‑ und Dataset‑Arrays serialisieren – C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stream‑basierte und asynchrone Serialisierung">}}

<p>Für große DICOM‑Dateien oder Hochdurchsatz‑Szenarien serialisieren Sie direkt in einen UTF‑8‑Stream, um das Anlegen großer Strings im Speicher zu vermeiden. Sowohl synchrone als auch asynchrone Methoden stehen zur Verfügung:</p>

<div class="codeblock" id="code">
 <h3>Stream‑ und Async‑Serialisierung – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Serialisierungsoptionen">}}

<p>Die Klasse <code>DicomJsonSerializerOptions</code> steuert, wie DICOM‑Daten in JSON dargestellt werden:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Eigenschaft</th>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Steuert, ob numerische VRs (IS, DS, SV, UV) als JSON‑Zahlen oder als Strings geschrieben werden</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Verwendet DICOM‑Schlüsselwörter (z. B. <code>"PatientName"</code>) anstelle von Tags (<code>"00100010"</code>) als JSON‑Schlüssel. Hinweis: nicht standardkonform</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Fügt das DICOM‑Schlüsselwort als zusätzliches JSON‑Attribut ein</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Fügt den DICOM‑Tagnamen als zusätzliches JSON‑Attribut ein</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Benutzerdefinierter Konverter zum Schreiben großer Daten (z. B. Pixel‑Daten) als URI‑Referenzen</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Benutzerdefinierter Loader zum Auflösen von Bulk‑Data‑URIs während der Deserialisierung</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Serialisierungsoptionen konfigurieren – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Nummernbehandlung">}}

<p>DICOM‑numerische Value Representations (IS, DS, SV, UV) können als JSON‑Zahlen oder JSON‑Strings serialisiert werden. Das ist für die Interoperabilität wichtig, da einige DICOM‑Implementierungen Zahlen mit führenden Leerzeichen oder nicht standardmäßiger Formatierung speichern:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modus</th>
<th>JSON‑Ausgabe</th>
<th>Anwendungsfall</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>Standard‑konform, ideal für Berechnungen. Wirft eine Ausnahme, wenn der Wert nicht geparst werden kann.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>Bewahrt die ursprüngliche String‑Darstellung. Sicherer für das Round‑Trip‑Verarbeiten nicht‑standardisierter Daten.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk‑Data‑Verarbeitung">}}

<p>Große binäre Werte (Pixel‑Daten, Waveforms, gekapselte Dokumente) können als Bulk‑Data‑Referenzen behandelt werden, anstatt sie in die JSON‑Ausgabe einzubetten. Dies entspricht der <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>-Spezifikation.</p>

<p>Implementieren Sie <code>IBulkDataConverter</code>, um große Daten während der Serialisierung auszulagern, und <code>IBulkDataLoader</code>, um URIs während der Deserialisierung aufzulösen:</p>

<div class="codeblock" id="code">
 <h3>Benutzerdefinierte Bulk‑Data‑Verarbeitung – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="JSON in DICOM deserialisieren">}}

<p>DICOM‑JSON zurück in Dataset‑ oder DicomFile‑Objekte parsen. Unterstützt String‑Eingabe, Stream‑Eingabe und asynchrone Stream‑Operationen:</p>

<div class="codeblock" id="code">
 <h3>JSON in DICOM deserialisieren – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="System.Text.Json‑Integration">}}

<p>Aspose.Medical stellt <code>DatasetJsonConverter</code> und <code>DicomFileJsonConverter</code> für die Integration in die standardmäßige <code>System.Text.Json</code>-Serialisierungspipeline bereit. Damit können DICOM‑Objekte zusammen mit anderen .NET‑Typen in Ihrem bestehenden JSON‑Verarbeitungscode serialisiert werden:</p>

<div class="codeblock" id="code">
 <h3>System.Text.Json‑Integration – C#</h3>
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
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Quellcode" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑Referenzen" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktunterstützung" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Kostenlose Unterstützung" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Kostenpflichtige Unterstützung" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Warum Aspose.Medical für .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundenliste" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Erfolgsgeschichten" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
