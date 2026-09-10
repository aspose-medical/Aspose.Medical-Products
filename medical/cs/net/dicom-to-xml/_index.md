---
title: Převod DICOM na XML v C# .NET | Aspose.Medical
weight: 3000
description: Serializujte DICOM datasety do standardního formátu DICOM XML v C# .NET. Nakonfigurujte zpracování bulk dat, streamové zpracování a asynchronní operace pomocí Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Převod DICOM na XML v .NET C#" h2="Serializujte DICOM datasety do standardní reprezentace DICOM XML (PS3.19). Nakonfigurujte odkazy na bulk data, výstup založený na streamu a asynchronní zpracování pomocí čisté .NET knihovny." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standardní serializace DICOM XML">}}

<p><strong>Aspose.Medical for .NET</strong> serializuje DICOM data do XML podle <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a>. Jedná se o oficiální standard pro reprezentaci DICOM datasetů v XML, který používají služby DICOMweb, integrační platformy a systémy vyžadující lidsky čitelnou, schématem validovanou reprezentaci metadat lékařského zobrazování.</p>

<p>Třída <code>DicomXmlSerializer</code> poskytuje statické metody pro serializaci i deserializaci. Na rozdíl od jednoduchých metod výpisu tagů výstup odpovídá schématu DICOM XML, kde je každý prvek reprezentován svým tagem, VR a správně formátovanými hodnotami &mdash; což umožňuje bezeztrátovou obousměrnou konverzi mezi binárním DICOM a XML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serializujte DICOM na XML v C#">}}

<p>Použijte třídu <code>DicomXmlSerializer</code> pro převod DICOM datasetu na XML řetězec. Nejjednodušší přístup vytvoří XML dokument vyhovující standardu:</p>

<div class="codeblock" id="code">
 <h3>Převod DICOM na XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serializace na bázi streamu a asynchronní">}}

<p>U velkých DICOM souborů nebo scénářů s vysokou propustností serializujte přímo do streamu, abyste se vyhnuli alokaci velkých řetězců v paměti. K dispozici jsou jak synchronní, tak asynchronní metody:</p>

<div class="codeblock" id="code">
 <h3>Synchronní streamová serializace - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Asynchronní streamová serializace - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Pipelínové streamování pro velké studie">}}

<p>Celé studie nemusí být drženy v paměti. <code>DicomXmlSerializer</code> zapisuje do <code>PipeWriter</code> a čte z <code>PipeReader</code>, takže XML může být vytvářeno a spotřebováváno během toku a sekvence datasetů může být načítána po jednom pomocí <code>DeserializeAsyncEnumerable</code>. Každá metoda přijímá <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Serializace a deserializace přes pipe - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Čtení sekvence datasetů po jednom - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Možnosti serializace">}}

<p>Třída <code>DicomXmlSerializerOptions</code> řídí, jak jsou DICOM data reprezentována v XML. Hlavní nastavení se týká zpracování bulk dat pro velké binární hodnoty:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Vlastnost</th>
<th>Typ</th>
<th>Popis</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Vlastní převodník pro zápis velkých dat (např. pixelových dat) jako odkazů BulkData URI místo vkládání</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Vlastní načítač pro řešení BulkData URI během deserializace</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Výchozí instance možností používaná, když nejsou poskytnuty vlastní možnosti</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Serializace s vlastními možnostmi - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zpracování bulk dat">}}

<p>Velké binární hodnoty (pixelová data, vlnové formy, zapouzdřené dokumenty) lze externalizovat jako odkazy BulkData URI místo vložení do výstupu XML. Toto vychází ze specifikace <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">DICOM PS3.19 BulkData element</a>.</p>

<p>Implementujte <code>IBulkDataConverter</code> pro externalizaci velkých dat během serializace a <code>IBulkDataLoader</code> pro řešení URI během deserializace. Pro běžné případy není nutné vůbec psát načítač: <code>DefaultBulkDataLoader.Instance</code> řeší URI <code>file</code>, <code>http</code> a <code>https</code> a také implementuje <code>IAsyncBulkDataLoader</code>, takže bulk data jsou načítána asynchronně v rámci streamovacích cest.</p>

<div class="codeblock" id="code">
 <h3>Vlastní zpracování bulk dat - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Deserializace XML do DICOM">}}

<p>Analyzujte DICOM XML zpět do objektů Dataset. Podporuje vstup jako řetězec, vstup jako stream a asynchronní operace:</p>

<div class="codeblock" id="code">
 <h3>Deserializace XML do DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Serializace XML vs JSON">}}

<p>Aspose.Medical podporuje serializaci jak DICOM XML (PS3.19), tak DICOM JSON (PS3.18). Oba formáty nabízejí bezeztrátovou obousměrnou konverzi, ale slouží různým integračním scénářům:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Funkce</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Standard</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>Ověření schématu</td><td>XML Schema (XSD) k dispozici</td><td>Žádné formální schéma</td></tr>
<tr><td>Nejvhodnější pro</td><td>Enterprise integration, HL7 CDA, audit logs, XDS registries</td><td>DICOMweb, REST APIs, FHIR ImagingStudy</td></tr>
<tr><td>Lidská čitelnost</td><td>Rozsáhlé, ale samovysvětlující</td><td>Komprimované a široce podporované</td></tr>
<tr><td>Bulk data</td><td>BulkData element with URI</td><td>BulkDataURI property</td></tr>
<tr><td>Třída serializeru</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Výukové zdroje" tabId="resources" >}}
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
{{< blocks/products/pf/slr-element name="Příběhy úspěchů" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
