---
title: Konwertuj DICOM do XML w C# .NET | Aspose.Medical
weight: 3000
description: Serializuj zestawy danych DICOM do standardowego formatu DICOM XML w C# .NET. Skonfiguruj obsługę dużych danych, przetwarzanie oparte na strumieniach oraz operacje asynchroniczne przy użyciu API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konwertuj DICOM do XML w .NET C#" h2="Serializuj zestawy danych DICOM do standardowej reprezentacji DICOM XML (PS3.19). Skonfiguruj odniesienia do dużych danych, wyjście oparte na strumieniach i przetwarzanie asynchroniczne przy użyciu czystej biblioteki .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Serializacja DICOM XML oparta na standardach">}}

<p><strong>Aspose.Medical for .NET</strong> serializuje dane DICOM do XML zgodnie z <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Modelem natywnym DICOM PS3.19</a>. Jest to oficjalny standard reprezentacji zestawów danych DICOM w XML, używany przez usługi DICOMweb, platformy integracyjne i systemy wymagające czytelnej dla człowieka, walidowanej schematem reprezentacji metadanych obrazowania medycznego.</p>

<p>Klasa <code>DicomXmlSerializer</code> udostępnia statyczne metody zarówno do serializacji, jak i deserializacji. W odróżnieniu od prostych podejść polegających na zrzucie tagów, wynik jest zgodny ze schematem DICOM XML, gdzie każdy element jest przedstawiony ze swoim tagiem, VR i prawidłowo sformatowanymi wartościami &mdash; umożliwiając bezstratną konwersję w obie strony między binarnym DICOM a XML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serializuj DICOM do XML w C#">}}

<p>Użyj klasy <code>DicomXmlSerializer</code> do konwersji zestawu danych DICOM na ciąg XML. Najprostsze podejście generuje dokument XML zgodny ze standardem:</p>

<div class="codeblock" id="code">
 <h3>Konwertuj DICOM do XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serializacja oparta na strumieniach i asynchroniczna">}}

<p>Dla dużych plików DICOM lub scenariuszy o wysokiej przepustowości serializuj bezpośrednio do strumienia, aby uniknąć alokacji dużych ciągów w pamięci. Dostępne są zarówno metody synchroniczne, jak i asynchroniczne:</p>

<div class="codeblock" id="code">
 <h3>Synchroniczna serializacja strumienia - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Asynchroniczna serializacja strumienia - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Strumieniowanie potokowe dla dużych badań">}}

<p>Całe badania nie muszą być trzymane w pamięci. <code>DicomXmlSerializer</code> zapisuje do <code>PipeWriter</code> i odczytuje z <code>PipeReader</code>, dzięki czemu XML może być generowany i konsumowany w czasie przepływu, a sekwencja zestawów danych może być odczytywana pojedynczo za pomocą <code>DeserializeAsyncEnumerable</code>. Każda metoda przyjmuje <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Serializuj i deserializuj przy użyciu potoku - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Odczytaj sekwencję zestawów danych po jednym - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opcje serializacji">}}

<p>Klasa <code>DicomXmlSerializerOptions</code> kontroluje sposób reprezentacji danych DICOM w XML. Podstawowa konfiguracja dotyczy obsługi dużych danych (Bulk Data) dla dużych wartości binarnych:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Właściwość</th>
<th>Typ</th>
<th>Opis</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Niestandardowy konwerter do zapisu dużych danych (np. danych pikseli) jako odwołania URI BulkData zamiast wstawiania ich bezpośrednio</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Niestandardowy loader do rozwiązywania URI BulkData podczas deserializacji</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Domyślny obiekt opcji używany, gdy nie zostaną podane niestandardowe opcje</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Serializuj z niestandardowymi opcjami - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Obsługa dużych danych">}}

<p>Duże wartości binarne (dane pikseli, przebiegi, dokumenty enkapsulowane) mogą być zewnętrznie przechowywane jako odwołania URI BulkData zamiast być wstawiane w wynikowy XML. Zgodnie jest to ze specyfikacją <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">elementu BulkData DICOM PS3.19</a>.</p>

<p>Zaimplementuj <code>IBulkDataConverter</code>, aby zewnętrznie przechowywać duże dane podczas serializacji, oraz <code>IBulkDataLoader</code>, aby rozwiązywać URI podczas deserializacji. W typowych przypadkach nie ma potrzeby tworzenia własnego loadera: <code>DefaultBulkDataLoader.Instance</code> rozwiązuje URI <code>file</code>, <code>http</code> i <code>https</code>, a także implementuje <code>IAsyncBulkDataLoader</code>, dzięki czemu duże dane są pobierane asynchronicznie podczas strumieniowania.</p>

<div class="codeblock" id="code">
 <h3>Niestandardowa obsługa dużych danych - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Deserializuj XML do DICOM">}}

<p>Parsuj XML DICOM z powrotem do obiektów Dataset. Obsługuje wejście jako ciąg znaków, strumień oraz operacje asynchroniczne:</p>

<div class="codeblock" id="code">
 <h3>Deserializuj XML do DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Serializacja XML vs JSON">}}

<p>Aspose.Medical obsługuje zarówno serializację DICOM XML (PS3.19), jak i DICOM JSON (PS3.18). Oba formaty zapewniają bezstratną konwersję w obie strony, ale służą różnym scenariuszom integracji:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Funkcja</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Standard</td><td>PS3.19 (Model natywny DICOM)</td><td>PS3.18 (Model DICOM JSON)</td></tr>
<tr><td>Walidacja schematu</td><td>Dostępny schemat XML (XSD)</td><td>Brak formalnego schematu</td></tr>
<tr><td>Najlepsze dla</td><td>Integracji korporacyjnej, HL7 CDA, logów audytu, rejestrów XDS</td><td>DICOMweb, REST API, FHIR ImagingStudy</td></tr>
<tr><td>Czytelność dla człowieka</td><td>Rozbudowane, ale samowyjaśniające</td><td>Kompaktowe i szeroko wspierane</td></tr>
<tr><td>Duże dane</td><td>Element BulkData z URI</td><td>Własność BulkDataURI</td></tr>
<tr><td>Klasa serializatora</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zasoby edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Kod źródłowy" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referencje API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Wsparcie produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Wsparcie darmowe" href="https://forum.aspose.com/c/medical" >}}
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
