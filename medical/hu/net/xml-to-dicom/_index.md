---
title: XML konvertálása DICOM-ra C# .NET-ben | Aspose.Medical
weight: 5000

description: DICOM fájlok létrehozása a PS3.19 Native DICOM Model XML-ből C# .NET-ben. XML beolvasása karakterláncból, adatfolyamból vagy csőből, egymást követő dokumentumok adatfolyamba olvasása, és a nagyméretű adatreferenciák feloldása az Aspose.Medical API-val.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="XML konvertálása DICOM-ra .NET C#-ban" h2="A PS3.19 Native DICOM Model XML visszaolvasása adatkészletekbe és DICOM fájlokba. Működés karakterláncból, adatfolyamból vagy csőből, egymást követő dokumentumok adatfolyamba olvasása, és a nagyméretű adatreferenciák feloldása." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Szabványos Native DICOM Model XML">}}

<p><strong>Aspose.Medical for .NET</strong> beolvassa a DICOM PS3.19-ben definiált <a href=\"https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html\">Native DICOM Model</a>-t. Ez a szabványba beépített XML ábrázolás, nem egy Aspose által kitalált formátum, ami az integrációban hasznossá teszi: egy már DICOM-ot XML-ként cserélő rendszer olyan dokumentumokat állít elő, amelyeket ez a könyvtár elfogad.</p>

<p>A dokumentum gyökere a <code>NativeDicomModel</code>, és minden attribútum egy <code>DicomAttribute</code> elem, amely a tagejét, értékábrázolását és kulcsszavát tartalmazza:</p>

<div class="codeblock" id="code">
 <h3>Native DICOM Model formátum</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>Ez az oldal a <a href=\"/medical/net/dicom-to-xml/\">DICOM to XML</a> fordított irányát mutatja, és mindkettő ugyanazt a osztályt használja, a <code>DicomXmlSerializer</code>-t.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM fájl létrehozása XML-ből C#-ban">}}

<p>A <code>Deserialize</code> egy dokumentumot <code>Dataset</code>-té alakít, és az adatkészletet DICOM fájlként írja a lemezre.</p>

<div class="codeblock" id="code">
 <h3>DICOM fájl létrehozása XML-ből - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>A Native DICOM Model nem tartalmaz File Meta Information csoportot, így az átvitel szintaxis nem része a dokumentumnak. Egy <code>DicomFile</code>-ba csomagolt adatkészlet az alapértelmezett átvitel szintaxissal, Implicit VR Little Endian-nel kerül írásra. A fájl más szintaxissal való tárolásához konvertálja át, ahogy a <a href=\"/medical/net/dicom-transfer-syntax-conversion/\">transfer syntax conversion</a> oldal mutatja.</p>

<p>A DICOM XML olvasása licencelt funkció. Ha nincs alkalmazva helyi licenc, az olvasó <code>MedicalApiException</code>-t dob, ezért először alkalmazza a licencet, ahogy a <a href=\"https://docs.aspose.com/medical/net/getting-started/licensing/\">licencelési útmutató</a> leírja.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Adatfolyamok, csövek és aszinkron">}}

<p>Minden belépési pont rendelkezik stream és aszinkron túlterheléssel, és az aszinkron változatok egy <code>PipeReader</code>-t is elfogadnak. Egy webes válaszból érkező dokumentumot a beolvasás közben elemzi, anélkül, hogy először karakterlánccá alakítaná.</p>

<div class="codeblock" id="code">
 <h3>XML olvasása adatfolyamból - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Egymást követő dokumentumok egy adatfolyamban">}}

<p>Egy másik rendszer exportja gyakran egyetlen adatfolyamban tartalmaz egymás után több <code>NativeDicomModel</code> elemet. A <code>DeserializeAsyncEnumerable</code> minden elemhez egy adatkészletet ad vissza, a bemeneti sorrendben, így az adatfolyamot memóriába való betöltés nélkül dolgozza fel. Az elemek közvetlenül egymás után következnek: az XML deklaráció csak a legelső pozíción engedélyezett, ahogy bármely XML bemenetnél.</p>

<div class="codeblock" id="code">
 <h3>Egymást követő dokumentumok adatfolyamba olvasása - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nagyadat-referenciák">}}

<p>A nagy értékek, például a pixeltadatok, nincsenek beágyazva. Egy <code>BulkData</code> elemként jelennek meg, amely egy URI-ra mutat, amely a bájtokra hivatkozik, ezáltal a dokumentum kicsi marad. A referenciák feloldásához olvasás közben adjon a sorosítónak egy bulk adat betöltőt. A <code>DefaultBulkDataLoader</code> <code>file</code>, <code>http</code> és <code>https</code> URI-kat autentikáció nélkül tölti le; ha egy archivumnak hitelesítésre van szüksége, saját maga implementálja a <code>IBulkDataLoader</code> vagy <code>IAsyncBulkDataLoader</code> interfészt.</p>

<div class="codeblock" id="code">
 <h3>Bulk adatok feloldása olvasás közben - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Körkörös átalakítás DICOM és XML között">}}

<p>A két irányt együtt kell használni: egy vizsgálat XML-ként indul, egy XML-et kezelő rendszeren keresztül megy, és DICOM fájlként tér vissza. Mindezt .NET kezeli, így ugyanaz a körkörös folyamat fut Windows, Linux és macOS rendszereken is.</p>

<div class="codeblock" id="code">
 <h3>DICOM XML-re és vissza - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Az XML kinézetét szabályozó opciókért tekintse meg a <a href=\"/medical/net/dicom-to-xml/\">DICOM to XML</a> oldalt. Ugyanez a páros létezik JSON-hez is: <a href=\"/medical/net/dicom-to-json/\">DICOM to JSON</a> és <a href=\"/medical/net/json-to-dicom/\">JSON to DICOM</a>. A <a href=\"https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/\">sorozás útmutató</a> lefedi a teljes API-t.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási anyagok" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Fejlesztői útmutató" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetett támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}