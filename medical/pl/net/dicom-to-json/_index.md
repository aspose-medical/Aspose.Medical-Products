---
title: Konwertuj DICOM na JSON w C# .NET | Aspose.Medical
weight: 4000
description: Serializuj pliki DICOM do standardowego formatu DICOM JSON w C# .NET. Konfiguruj obsługę liczb, URI danych masowych, wyjście słów kluczowych oraz asynchroniczne przetwarzanie strumieniowe przy użyciu API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konwertuj DICOM na JSON w .NET C#" h2="Serializuj zestawy danych i pliki DICOM do standardowego modelu DICOM JSON (PS3.18). Konfiguruj obsługę liczb, odwołania do danych masowych, wyjście słów kluczowych oraz asynchroniczne przetwarzanie oparte na strumieniach." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standardowy model DICOM JSON">}}

<p><strong>Aspose.Medical for .NET</strong> serializuje dane DICOM do JSON zgodnie z <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">Modelem DICOM PS3.18 JSON</a>. Jest to oficjalny standard przedstawiania zestawów danych DICOM w formacie JSON, wykorzystywany przez usługi DICOMweb, zasoby FHIR ImagingStudy oraz nowoczesne interfejsy API opieki zdrowotnej.</p>

<p>W modelu DICOM JSON każde atrybut jest reprezentowany przez jego tag jako klucz JSON (np. <code>"00100010"</code> dla Patient Name), a reprezentacja wartości (Value Representation) oraz tablica wartości są właściwościami zagnieżdżonymi:</p>

<div class="codeblock" id="code">
 <h3>Format modelu DICOM JSON</h3>
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

{{< blocks/products/pf/feature-page-section h2="Serializuj DICOM do JSON w C#">}}

<p>Użyj klasy <code>DicomJsonSerializer</code> do konwersji pliku DICOM lub zestawu danych na JSON. Najprostsze podejście generuje zwarty, zgodny ze standardem wynik:</p>

<div class="codeblock" id="code">
 <h3>Konwertuj DICOM na JSON - C#</h3>
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

<p>Możesz serializować <code>Dataset</code>, kompletny <code>DicomFile</code> (włącznie z informacjami meta pliku) lub tablicę zestawów danych:</p>

<div class="codeblock" id="code">
 <h3>Serializuj DicomFile i tablice zestawów danych - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serializacja oparta na strumieniach i asynchroniczna">}}

<p>Dla dużych plików DICOM lub scenariuszy o wysokiej przepustowości serializuj bezpośrednio do strumienia UTF-8, aby uniknąć alokacji dużych łańcuchów w pamięci. Dostępne są zarówno metody synchroniczne, jak i asynchroniczne:</p>

<div class="codeblock" id="code">
 <h3>Serializacja strumieniowa i asynchroniczna - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Opcje serializacji">}}

<p>Klasa <code>DicomJsonSerializerOptions</code> steruje sposobem reprezentacji danych DICOM w formacie JSON:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Właściwość</th>
<th>Typ</th>
<th>Opis</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Określa, czy numeryczne VR (IS, DS, SV, UV) są zapisywane jako liczby JSON lub jako ciągi znaków</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Używa słów kluczowych DICOM (np. <code>"PatientName"</code>) zamiast tagów (<code>"00100010"</code>) jako kluczy JSON. Uwaga: nie jest standardem</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Dołącza słowo kluczowe DICOM jako dodatkowy atrybut JSON</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Dołącza nazwę tagu DICOM jako dodatkowy atrybut JSON</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Niestandardowy konwerter do zapisywania dużych danych (np. danych pikseli) jako odwołań URI</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Niestandardowy loader do rozwiązywania odwołań URI danych masowych podczas deserializacji</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Konfiguruj opcje serializacji - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Obsługa liczb">}}

<p>Numeryczne reprezentacje wartości DICOM (IS, DS, SV, UV) mogą być serializowane jako liczby JSON lub ciągi znaków JSON. Jest to istotne dla interoperacyjności, ponieważ niektóre implementacje DICOM przechowują liczby z wiodącymi spacjami lub w niestandardowym formacie:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tryb</th>
<th>Wyjście JSON</th>
<th>Przypadek użycia</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>Zgodny ze standardem, idealny do obliczeń. Rzuca wyjątek, jeśli wartość nie może być sparsowana.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>Zachowuje oryginalną reprezentację jako ciąg znaków. Bezpieczniejsze przy dwukierunkowym przekazywaniu danych niezgodnych ze standardem.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Obsługa danych masowych">}}

<p>Duże wartości binarne (dane pikseli, przebiegi, dokumenty enkapsulowane) mogą być obsługiwane jako odwołania do danych masowych zamiast wstawiania ich bezpośrednio w wynik JSON. To jest zgodne ze specyfikacją <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>.</p>

<p>Zaimplementuj <code>IBulkDataConverter</code>, aby zewnętrznie przechowywać duże dane podczas serializacji, oraz <code>IBulkDataLoader</code>, aby rozwiązywać URI podczas deserializacji:</p>

<div class="codeblock" id="code">
 <h3>Niestandardowa obsługa danych masowych - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Deserializuj JSON do DICOM">}}

<p>Parsuj JSON DICOM z powrotem do obiektów Dataset lub DicomFile. Obsługuje wejście jako łańcuch znaków, strumień oraz asynchroniczne operacje na strumieniu:</p>

<div class="codeblock" id="code">
 <h3>Deserializuj JSON do DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Integracja z System.Text.Json">}}

<p>Aspose.Medical udostępnia <code>DatasetJsonConverter</code> i <code>DicomFileJsonConverter</code> do integracji ze standardowym potokiem serializacji <code>System.Text.Json</code>. Umożliwia to serializację obiektów DICOM razem z innymi typami .NET w istniejącym kodzie przetwarzania JSON:</p>

<div class="codeblock" id="code">
 <h3>Integracja System.Text.Json - C#</h3>
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
{{< blocks/products/pf/slr-tab tabTitle="Materiały edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Kod źródłowy" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referencje API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Wsparcie produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Wsparcie bezpłatne" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Wsparcie płatne" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Dlaczego Aspose.Medical dla .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista klientów" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Historie sukcesu" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
