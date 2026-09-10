---
title: DICOM konvertálása XML-re C# .NET | Aspose.Medical
weight: 3000
description: Serializálja a DICOM adathalmazokat a szabványos DICOM XML formátumba C# .NET-ben. Konfigurálja a bulk data kezelését, a stream-alapú feldolgozást és az aszinkron műveleteket az Aspose.Medical API-val.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM konvertálása XML-be .NET C#" h2="Serializálja a DICOM adathalmazokat a szabványos DICOM XML reprezentációba (PS3.19). Állítsa be a bulk data hivatkozásokat, a stream-alapú kimenetet és az aszinkron feldolgozást egy tiszta .NET könyvtárral." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standard-alapú DICOM XML sorosítás">}}

<p><strong>Aspose.Medical for .NET</strong> serializálja a DICOM adatokat XML-be a <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Natív DICOM Modell</a> szerint. Ez a hivatalos szabvány a DICOM adathalmazok XML-ben való ábrázolására, amelyet a DICOMweb szolgáltatások, integrációs platformok és azok a rendszerek használnak, amelyeknek ember által olvasható, sémával validált ábrázolásra van szükségük a orvosi képalkotási metaadatokról.</p>

<p>A <code>DicomXmlSerializer</code> osztály statikus metódusokat biztosít mind a sorosításhoz, mind a deszerializációhoz. Az egyszerű címkel dumpolású megközelítésekkel ellentétben a kimenet megfelel a DICOM XML sémának, ahol minden elem a címkéjével, VR-ével és helyesen formázott értékekkel jelenik meg &mdash; lehetővé téve a veszteségmentes körkörös átalakítást a bináris DICOM és az XML között.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM sorosítása XML-be C#-ban">}}

<p>Használja a <code>DicomXmlSerializer</code> osztályt egy DICOM adathalmaz XML stringgé konvertálásához. A legegyszerűbb megközelítés egy szabványnak megfelelő XML dokumentumot eredményez:</p>

<div class="codeblock" id="code">
 <h3>DICOM konvertálása XML-re - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stream-alapú és aszinkron sorosítás">}}

<p>Nagy DICOM fájlok vagy nagy áteresztőképességű forgatókönyvek esetén sorosítsa közvetlenül egy streambe, hogy elkerülje a nagy méretű stringek memóriában történő lefoglalását. Szinkron és aszinkron metódusok egyaránt elérhetők:</p>

<div class="codeblock" id="code">
 <h3>Szinkron stream sorosítás - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Aszinkron stream sorosítás - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Pipeline streaming nagy tanulmányokhoz">}}

<p>Az egész tanulmányokat nem szükséges memóriában tartani. A <code>DicomXmlSerializer</code> egy <code>PipeWriter</code>-be ír és egy <code>PipeReader</code>-ről olvas, így az XML folyamatosan előállítható és felhasználható, miközben egy adathalmaz-sorozat egyes elemei egyenként olvashatók a <code>DeserializeAsyncEnumerable</code> segítségével. Minden metódus egy <code>CancellationToken</code>-t fogad.</p>

<div class="codeblock" id="code">
 <h3>Sorosítás és deszerializáció csővezetéken keresztül - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Adathalmaz-sorozat egyes elemeinek olvasása egyenként - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sorosítási beállítások">}}

<p>A <code>DicomXmlSerializerOptions</code> osztály szabályozza, hogyan jelennek meg a DICOM adatok XML-ben. Az elsődleges beállítás a nagy bináris értékek (bulk data) kezelését érinti:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tulajdonság</th>
<th>Típus</th>
<th>Leírás</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Egyedi konverter nagy adatok (pl. pixel adatok) BulkData URI hivatkozásként történő írásához a beágyazás helyett</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Egyedi betöltő BulkData URI-k feloldásához a deszerializáció során</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Alapértelmezett opciós példány, amely akkor használatos, ha nincs egyéni beállítás megadva</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Sorosítás egyéni beállításokkal - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk Data kezelése">}}

<p>Nagy bináris értékek (pixel adatok, hullámformák, enkapszulált dokumentumok) externalizálhatók BulkData URI hivatkozásokként ahelyett, hogy beágyazottak lennének az XML kimenetben. Ez a <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">DICOM PS3.19 BulkData elem</a> specifikációját követi.</p>

<p>Valósítsa meg a <code>IBulkDataConverter</code> interfészt a nagy adatok serializáció alatti externalizálásához, és a <code>IBulkDataLoader</code> interfészt az URI-k deszerializáció alatti feloldásához. A gyakori esetekben nem szükséges saját betöltőt írni: a <code>DefaultBulkDataLoader.Instance</code> feloldja a <code>file</code>, <code>http</code> és <code>https</code> URI-kat, valamint implementálja a <code>IAsyncBulkDataLoader</code> interfészt, így a bulk adatok aszinkron módon kerülnek letöltésre a streaming útvonalakon.</p>

<div class="codeblock" id="code">
 <h3>Egyéni bulk data kezelés - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="XML deszerializálása DICOM-ra">}}

<p>Feldolgozza a DICOM XML-t vissza Dataset objektumokká. Támogatja a string bemenetet, stream bemenetet és az aszinkron műveleteket:</p>

<div class="codeblock" id="code">
 <h3>XML deszerializálása DICOM-ra - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="XML vs JSON sorosítás">}}

<p>Az Aspose.Medical támogatja a DICOM XML (PS3.19) és a DICOM JSON (PS3.18) sorosítást is. Mindkét formátum veszteségmentes körkörös konverziót biztosít, de különböző integrációs forgatókönyvekhez alkalmas.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Funkció</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Standard</td><td>PS3.19 (Natív DICOM Modell)</td><td>PS3.18 (DICOM JSON Modell)</td></tr>
<tr><td>Séma validáció</td><td>XML séma (XSD) elérhető</td><td>Nincs formális séma</td></tr>
<tr><td>Legalkalmasabb</td><td>Vállalati integráció, HL7 CDA, audit naplók, XDS regisztrációk</td><td>DICOMweb, REST API-k, FHIR ImagingStudy</td></tr>
<tr><td>Emberi olvashatóság</td><td>Bőbeszédű, de önleíró</td><td>Kompakt és széles körben támogatott</td></tr>
<tr><td>Bulk data</td><td>BulkData elem URI-val</td><td>BulkDataURI tulajdonság</td></tr>
<tr><td>Sorosító osztály</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Forráskód" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetős támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért az Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikeres esetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
