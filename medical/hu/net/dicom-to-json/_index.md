---
title: DICOM átalakítása JSON formátumba C# .NET környezetben | Aspose.Medical
weight: 4000
description: DICOM fájlok sorosítása a szabványos DICOM JSON formátumba C# .NET-ben. Számkezelés, tömeges adat URI-k, kulcsszó kimenet és aszinkron adatfolyam‑feldolgozás beállítása az Aspose.Medical API-val.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM átalakítása JSON formátumba .NET C#-ban" h2="DICOM adatkészletek és fájlok sorosítása a szabványos DICOM JSON modellbe (PS3.18). Számkezelés, tömeges adat hivatkozások, kulcsszó kimenet és adatfolyam‑alapú aszinkron feldolgozás beállítása." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Szabványos DICOM JSON modell">}}

<p><strong>Aspose.Medical for .NET</strong> sorosítja a DICOM adatokat JSON-ba a <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON modell</a> szerint. Ez a hivatalos szabvány a DICOM adatkészletek JSON‑ban történő ábrázolására, amelyet a DICOMweb szolgáltatások, a FHIR ImagingStudy erőforrások és a modern egészségügyi API-k használnak.</p>

<p>A DICOM JSON modellben minden attribútum a címkéje alapján jelenik meg JSON kulcsként (például <code>"00100010"</code> a Patient Name esetén), a Value Representation és az érték tömb beágyazott tulajdonságokként.</p>

<div class="codeblock" id="code">
 <h3>DICOM JSON modell formátuma</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM sorosítása JSON-ba C#‑ban">}}

<p>Használja a <code>DicomJsonSerializer</code> osztályt egy DICOM fájl vagy adatkészlet JSON‑ba konvertálásához. A legegyszerűbb megközelítés tömör, szabványos kimenetet eredményez:</p>

<div class="codeblock" id="code">
 <h3>DICOM átalakítása JSON formátumba – C#</h3>
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

<p>Szériazhat egy <code>Dataset</code>-et, egy teljes <code>DicomFile</code>-t (beleértve a File Meta Information részt), vagy egy adatkészlet‑tömböt:</p>

<div class="codeblock" id="code">
 <h3>DicomFile és adatkészlet‑tömbök sorosítása – C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Adatfolyam‑alapú és aszinkron sorosítás">}}

<p>Nagy DICOM fájlok vagy nagy áteresztőképességű szcenáriók esetén sorosítsa közvetlenül UTF‑8 adatfolyamra a nagy memóriában lévő karakterláncok elkerülése érdekében. Szinkron és aszinkron módszerek egyaránt elérhetők:</p>

<div class="codeblock" id="code">
 <h3>Adatfolyam és aszinkron sorosítás – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Sorosítási beállítások">}}

<p>A <code>DicomJsonSerializerOptions</code> osztály határozza meg, hogyan jelennek meg a DICOM adatok a JSON‑ban:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tulajdonság</th>
<th>Típus</th>
<th>Leírás</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Ellenőrzi, hogy a numerikus VR‑ek (IS, DS, SV, UV) JSON számként vagy karakterláncként kerülnek-e kiírásra</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>A DICOM kulcsszavak (pl. <code>"PatientName"</code>) használata a címkék (<code>"00100010"</code>) helyett JSON kulcsként. Megjegyzés: nem szabványos</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>A DICOM kulcsszó belefoglalása kiegészítő JSON attribútumként</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>A DICOM címke nevének belefoglalása kiegészítő JSON attribútumként</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Egyedi konverter a nagy adatok (pl. pixel adat) URI hivatkozásként írásához</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Egyedi betöltő a tömeges adat URI‑k feloldásához deszerializálás során</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Sorosítási opciók konfigurálása – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Számkezelés">}}

<p>A DICOM numerikus Value Representation‑ek (IS, DS, SV, UV) sorosíthatók JSON számként vagy JSON karakterláncként. Ez az interoperabilitás szempontjából fontos, mivel egyes DICOM megvalósítások számokat tárolnak előre álló szóközökkel vagy nem szabványos formátummal:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Mód</th>
<th>JSON kimenet</th>
<th>Alkalmazási eset</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>Szabványos, számításra ideális. Hibát dob, ha az érték nem értelmezhető.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>Megőrzi az eredeti karakterlánc ábrázolást. Biztonságosabb a nem szabványos adatok körkörös átalakításához.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tömeges adatkezelés">}}

<p>Nagy bináris értékek (pixel adatok, hullámformák, beágyazott dokumentumok) kezelhetők tömeges adat hivatkozásokként ahelyett, hogy a JSON kimenetben beágyazottak lennének. Ez a <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a> specifikáción alapul.</p>

<p>Valósítsa meg a <code>IBulkDataConverter</code> interfészt a nagy adatok externalizálásához sorosítás során, és a <code>IBulkDataLoader</code> interfészt az URI‑k feloldásához deszerializáláskor:</p>

<div class="codeblock" id="code">
 <h3>Egyedi tömeges adatkezelés – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="JSON deszerializálása DICOM-ba">}}

<p>A DICOM JSON parse-olása vissza Dataset vagy DicomFile objektumokká. Támogatja a string bemenetet, adatfolyam bemenetet és aszinkron adatfolyam műveleteket:</p>

<div class="codeblock" id="code">
 <h3>JSON deszerializálása DICOM-ba – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="System.Text.Json integráció">}}

<p>Az Aspose.Medical a <code>DatasetJsonConverter</code> és <code>DicomFileJsonConverter</code> komponenseket biztosítja a szabványos <code>System.Text.Json</code> sorosítási pipeline-hoz való integrációhoz. Ez lehetővé teszi, hogy a DICOM objektumok más .NET típusokkal együtt legyenek sorosítva a meglévő JSON feldolgozó kódban:</p>

<div class="codeblock" id="code">
 <h3>System.Text.Json integráció – C#</h3>
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
{{< blocks/products/pf/slr-tab tabTitle="Tanulási anyagok" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Forráskód" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetett támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért az Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
