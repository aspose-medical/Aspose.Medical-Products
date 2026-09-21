---
title: JSON konvertálása DICOM-ra C# .NET környezetben | Aspose.Medical
weight: 6000

description: Készítsen DICOM fájlokat a szabványos DICOM JSON Model (PS3.18) alapján C# .NET környezetben. Olvassa be a JSON-t karakterláncból, adatfolyamból vagy csővezetékből, adatfolyamon küldjön egy sor Dataset-et, és oldja fel a bulk adat hivatkozásokat az Aspose.Medical API-val.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JSON konvertálása DICOM-ra .NET C#-ban" h2="Olvassa be a szabványos DICOM JSON Model (PS3.18) adatokat vissza Dataset-ekbe és DICOM fájlokba. Működjön karakterláncból, adatfolyamból vagy csővezetékből, adatfolyamban küldjön egy sor tanulmányt, és oldja fel a bulk adat hivatkozásokat." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM JSON-ról DICOM fájlra">}}

<p><strong>Aspose.Medical for .NET</strong> beolvassa a <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>-t, ami a DICOMweb szolgáltatások és az HTTP-n keresztül tanulmányokat cserélő rendszerek által használt ábrázolás. Ami JSON-ként érkezik, <code>Dataset</code>-dé alakul, és a <code>Dataset</code>-et DICOM fájlként írja a lemezre.</p>

<p>Ez a <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> oldal ellentétes iránya, és mindkettő ugyanazt az osztályt használja, a <code>DicomJsonSerializer</code>-t.</p>

<div class="codeblock" id="code">
 <h3>DICOM fájl létrehozása JSON-ból - C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Egy olyan Dataset, amely nem tartalmaz File Meta Information-t, az alapértelmezett átvitel szintaxissal, Implicit VR Little Endian, kerül kiírásra, amikor egy <code>DicomFile</code>-ba van becsomagolva.</p>

<p>A DICOM JSON olvasása licencelt funkció. Ha helyi licenc nincs alkalmazva, az olvasó <code>MedicalApiException</code>-t dob, ezért először alkalmazza a licencet, ahogyan a <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">licencelési útmutató</a> leírja.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="A File Meta Information megőrzése">}}

<p>A <code>Deserialize</code> csak a Dataset-et adja vissza. Amikor a JSON dokumentum a File Meta Information csoportot is tartalmazza, például egy teljes DICOM fájlból készült, a <code>DeserializeFile</code> egy <code>DicomFile</code>-t ad vissza ezzel a csoporttal érintetlenül, beleértve a fájl által deklarált átviteli szintaxist.</p>

<div class="codeblock" id="code">
 <h3>Teljes DICOM fájl beolvasása JSON-ból - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Adatfolyamok, csövek és aszinkron">}}

<p>Minden belépési pont rendelkezik stream túlterheléssel és aszinkron túlterheléssel, az aszinkron változatok emellett elfogadják a <code>PipeReader</code>-t. Egy webes válaszból vagy lemezről érkező dokumentumot közvetlenül olvassák be, anélkül hogy előbb karakterlánccá alakítanák, ami akkor fontos, ha a JSON pixel adatot tartalmaz.</p>

<div class="codeblock" id="code">
 <h3>JSON beolvasása adatfolyamból - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dataset sorozat, egyesével">}}

<p>Egy DICOMweb lekérdezés egy Dataset-tömböt ad vissza, és egy ilyen dokumentum nagy lehet. A <code>DeserializeList</code> az egész tömböt a memóriába olvassa; a <code>DeserializeAsyncEnumerable</code> egyesével adja vissza a Dataset-eket, így a dokumentum soha nem kerül betöltésre teljes egészében.</p>

<div class="codeblock" id="code">
 <h3>Dataset-tömb adatfolyamba küldése - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk adat hivatkozások">}}

<p>A DICOM JSON Model nem tartalmaz pixel adatot inline. A nagy értékeket egy <code>BulkDataURI</code>-ra cserélik, amely a bájtokra mutat, ezáltal a JSON dokumentum kicsi marad. A hivatkozások feloldásához beolvasás közben adjon a sorosítónak egy bulk adat betöltőt. A <code>DefaultBulkDataLoader</code> <code>file</code>, <code>http</code> és <code>https</code> URI-kat autentikáció nélkül tölt le; ha egy archívumnak hitelesítésre van szüksége, implementálja saját maga a <code>IBulkDataLoader</code> vagy <code>IAsyncBulkDataLoader</code> interfészt.</p>

<div class="codeblock" id="code">
 <h3>BulkDataURI feloldása beolvasás közben - C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Körkörös átalakítás DICOM és JSON között">}}

<p>A két irányt együtt kell használni: egy tanulmány JSON-ként indul, egy webszolgáltatáson keresztül utazik, majd DICOM fájlként tér vissza. A folyamatban semmi sem függ natív kódtól, így ugyanaz a körkörös átalakítás futtatható Windows, Linux és macOS rendszereken.</p>

<div class="codeblock" id="code">
 <h3>DICOM to JSON és vissza - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>A JSON megjelenését befolyásoló beállításokért tekintse meg a <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> oldalt. Ugyanez a páros XML-re is létezik: <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> és <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>. A <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">JSON sorosítási útmutató</a> lefedi az egész API-t.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Fejlesztői útmutató" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
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