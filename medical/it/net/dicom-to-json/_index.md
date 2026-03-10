---
title: Converti DICOM in JSON in C# .NET | Aspose.Medical
weight: 4000
description: Serializza i file DICOM nel formato standard DICOM JSON in C# .NET. Configura la gestione dei numeri, gli URI dei dati bulk, l'output delle parole chiave e l'elaborazione di flussi asincroni con l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converti DICOM in JSON in .NET C#" h2="Serializza set di dati e file DICOM nel modello standard DICOM JSON (PS3.18). Configura la gestione dei numeri, i riferimenti ai dati bulk, l'output delle parole chiave e l'elaborazione asincrona basata su stream." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Modello DICOM JSON standard">}}

<p><strong>Aspose.Medical for .NET</strong> serializza i dati DICOM in JSON secondo il <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">Modello DICOM PS3.18 JSON</a>. Questo è lo standard ufficiale per rappresentare set di dati DICOM in JSON, utilizzato dai servizi DICOMweb, dalle risorse FHIR ImagingStudy e dalle API sanitarie moderne.</p>

<p>Nel modello DICOM JSON, ogni attributo è rappresentato dal suo tag come chiave JSON (ad es., <code>"00100010"</code> per Patient Name), con la Value Representation e l'array di valori come proprietà annidate:</p>

<div class="codeblock" id="code">
 <h3>Formato del modello DICOM JSON</h3>
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

{{< blocks/products/pf/feature-page-section h2="Serializza DICOM in JSON in C#">}}

<p>Utilizza la classe <code>DicomJsonSerializer</code> per convertire un file DICOM o un dataset in JSON. L'approccio più semplice produce un output compatto e conforme agli standard:</p>

<div class="codeblock" id="code">
 <h3>Converti DICOM in JSON - C#</h3>
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

<p>Puoi serializzare un <code>Dataset</code>, un <code>DicomFile</code> completo (inclusa la File Meta Information) o un array di dataset:</p>

<div class="codeblock" id="code">
 <h3>Serializza DicomFile e array di dataset - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serializzazione basata su stream e asincrona">}}

<p>Per file DICOM di grandi dimensioni o scenari ad alto throughput, serializza direttamente su uno stream UTF-8 per evitare l'allocazione di grandi stringhe in memoria. Sono disponibili sia metodi sincroni che asincroni:</p>

<div class="codeblock" id="code">
 <h3>Serializzazione su stream e asincrona - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Opzioni di serializzazione">}}

<p>La classe <code>DicomJsonSerializerOptions</code> controlla come i dati DICOM sono rappresentati in JSON:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Proprietà</th>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Controlla se i VR numerici (IS, DS, SV, UV) vengono scritti come numeri JSON o come stringhe</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Utilizza le parole chiave DICOM (es., <code>"PatientName"</code>) anziché i tag (<code>"00100010"</code>) come chiavi JSON. Nota: non standard</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Includi la parola chiave DICOM come attributo JSON aggiuntivo</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Includi il nome del tag DICOM come attributo JSON aggiuntivo</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Convertitore personalizzato per scrivere dati di grandi dimensioni (es., pixel data) come riferimenti URI</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Loader personalizzato per risolvere gli URI dei dati bulk durante la deserializzazione</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Configura le opzioni di serializzazione - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Gestione dei numeri">}}

<p>Le Value Representations numeriche DICOM (IS, DS, SV, UV) possono essere serializzate come numeri JSON o come stringhe JSON. Questo è importante per l'interoperabilità, poiché alcune implementazioni DICOM memorizzano i numeri con spazi iniziali o formati non standard:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modalità</th>
<th>Output JSON</th>
<th>Caso d'uso</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>Conforme allo standard, ideale per i calcoli. Genera un'eccezione se il valore non può essere analizzato.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>Preserva la rappresentazione originale della stringa. Più sicuro per il round‑trip di dati non standard.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gestione dei dati bulk">}}

<p>I valori binari di grandi dimensioni (pixel data, forme d'onda, documenti incapsulati) possono essere gestiti come riferimenti a dati bulk invece di essere inseriti inline nell'output JSON. Questo segue la specifica <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>.</p>

<p>Implementa <code>IBulkDataConverter</code> per esternalizzare i dati di grandi dimensioni durante la serializzazione, e <code>IBulkDataLoader</code> per risolvere gli URI durante la deserializzazione:</p>

<div class="codeblock" id="code">
 <h3>Gestione personalizzata dei dati bulk - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Deserializza JSON in DICOM">}}

<p>Analizza il DICOM JSON e lo converte nuovamente in oggetti Dataset o DicomFile. Supporta input da stringa, da stream e operazioni su stream asincrone:</p>

<div class="codeblock" id="code">
 <h3>Deserializza JSON in DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Integrazione con System.Text.Json">}}

<p>Aspose.Medical fornisce <code>DatasetJsonConverter</code> e <code>DicomFileJsonConverter</code> per l'integrazione con la pipeline di serializzazione standard <code>System.Text.Json</code>. Ciò consente di serializzare oggetti DICOM insieme ad altri tipi .NET nel tuo codice di elaborazione JSON esistente:</p>

<div class="codeblock" id="code">
 <h3>Integrazione System.Text.Json - C#</h3>
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
{{< blocks/products/pf/slr-tab tabTitle="Risorse di apprendimento" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentazione" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Codice sorgente" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Riferimenti API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Supporto prodotto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Supporto gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Supporto a pagamento" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Perché Aspose.Medical per .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Elenco clienti" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Storie di successo" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
