---
title: DICOMweb kliens C# .NET-ben - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Keresse, töltse le és töltse fel a DICOM vizsgálatokat HTTP-n keresztül .NET-ből. Egy tiszta C#-ban írt DICOMweb kliens a QIDO-RS, WADO-RS és STOW-RS számára, amely együttműködik PACS, VNA és felhő alapú képalkotó szolgáltatásokkal.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb kliens .NET C#-hez" h2="Keressen, töltsön le és töltse fel a vizsgálatokat bármely DICOMweb szerveren QIDO-RS, WADO-RS és STOW-RS segítségével, menedzselt C#-ban, anélkül, hogy bármit telepíteni kellene a gépre." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM HTTP-n keresztül, saját kódból">}}

<p>A DICOMweb a DICOM HTTP-alakja. A 104-es porton lévő DIMSE kapcsolat helyett az alkalmazása hagyományos HTTPS kéréseket küld: QIDO-RS a kereséshez, WADO-RS a letöltéshez, STOW-RS a feltöltéshez. Ilyen módon működnek a felhő alapú képalkotó szolgáltatások, mint az Azure Health Data Services DICOM szolgáltatása és a Google Cloud Healthcare API, valamint az olyan archívumok, mint az Orthanc és a dcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> DICOMweb klienset tartalmaz a 26.10-es verziótól kezdve. A <code>DicomWebClient</code> minden standard tranzakciót egy típusos metódusba csomagol, és az eredmények ugyanazok a <code>Dataset</code> és <code>DicomFile</code> objektumok, amelyekkel a könyvtár többi része olvas és ír. A másik oldalon lévő szerver lehet bármely DICOMweb implementáció; nem szükséges, hogy az Aspose.Medical-lel készült legyen.</p>

<p>A kliens menedzselt C#-on fut a <code>HttpClient</code> tetején. Nincs natív eszközkészlet és proxy szolgáltatás, így ugyanaz a kód fut Windows-on, Linux-on és konténerben is.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Csatlakozás DICOMweb szerverhez">}}

<p>A kliens létrehozása egy <code>HttpClient</code> és a szerver szolgáltatás gyökér URL-je alapján történik. A hitelesítés, TLS beállítások, proxyk és a kapcsolat-gyűjtés a <code>HttpClient</code>-en marad, ahol az alkalmazás már konfigurálja őket. A kliens nem birtokolja a <code>HttpClient</code>-et, és nem zárja le azt.</p>

<div class="codeblock" id="code">
 <h3>Hozzon létre DICOMweb klienst - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Ha egy telepítés WADO-URI-t vagy egy másik szolgáltatást egy eltérő URL-ről szolgál ki, a <code>ServiceBaseUris</code> csak arra a szolgáltatásra felülírja a gyökeret.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vizsgálatok keresése QIDO-RS-sel">}}

<p>A keresés DICOM kulcsszavakkal ellátott egyező paramétereket, a visszaadandó mezőket és a lapozási vezérléseket használja. A találatok adatkészletek folyamként érkeznek, egyesével olvasva, amíg a válasz még letöltődik.</p>

<div class="codeblock" id="code">
 <h3>Páciens vizsgálatainak megtalálása - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> és <code>SearchInstancesAsync</code> egy szinttel lejjebb keresnek, az egész archívumon vagy egy vizsgálaton vagy sorozaton belül. Ugyanezek a beállítások lehetővé teszik a homályos egyezést, az üres érték egyezést és az eredmény eltolását a lapozáshoz.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vizsgálat letöltése WADO-RS-sel">}}

<p>Egy vizsgálat több ezer példányt is tartalmazhat. A <code>RetrieveStudyAsync</code> folyamként adja vissza őket, így minden fájl a válasz érkezésének részeként kerül feldolgozásra, és a következő olvasása előtt menthető. A memóriahasználat nem növekszik a vizsgálat méretével.</p>

<div class="codeblock" id="code">
 <h3>Minden vizsgálati példány mentése - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p>A <code>RetrieveSeriesAsync</code> és a <code>RetrieveInstanceAsync</code> ugyanígy töltenek le egy sorozatot vagy egy példányt.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Miniatűr vagy megjelenített kép lekérése">}}

<p>Egy megjelenítő vagy egy munkalista képernyő gyakran egy képet igényel, nem DICOM fájlt. A szerver rendereli, és JPEG vagy PNG formátumban küldi, így a kliens nem tölti le vagy dekódolja az eredeti pixel adatokat.</p>

<div class="codeblock" id="code">
 <h3>Egy példány JPEG miniatűrjének mentése - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p>A <code>RetrieveRenderedInstanceAsync</code> és a <code>RetrieveRenderedFramesAsync</code> ehelyett egy teljes méretű képet ad vissza. A <code>DicomWebRenderOptions</code> a szabványos renderelési beállításokat továbbítja a szervernek: képminőség, nézetablak mérete, ablak és ICC profil.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Példányok feltöltése STOW-RS-sel">}}

<p>A STOW-RS tetszőleges számú példányt küld egy kérésben. A szerver jelentéssel válaszol, amely példányonként felsorolja, hogy mit tárolt és mit utasított el.</p>

<div class="codeblock" id="code">
 <h3>DICOM fájlok mappájának feltöltése - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>Egyes szerverek HTTP hibával (például 409) válaszolnak, ha egy vagy több példány sikertelen. Ebben az esetben a hívás <code>DicomWebClientException</code>-t dob, ahogyan a következő rész mutatja, és ugyanaz a jelentés az kivétel választestében található.</p>

<p>A <code>StoreStudyAsync</code> egy nevű vizsgálatba tölt fel. Mindkét metódus elfogad egy <code>IAsyncEnumerable&lt;DicomFile&gt;</code>-t is, így a fájlok a kérés küldése közben is előállíthatók.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Szerver hibák kezelése">}}

<p>Minden típusos művelet ellenőrzi a HTTP státuszkódot a visszatérés előtt. Ha a szerver elutasítja a kérést, a hívás <code>DicomWebClientException</code>-t dob, amely a státuszkódot, a fejléceket és a választest kezdetét tartalmazza a naplózáshoz.</p>

<div class="codeblock" id="code">
 <h3>Sikertelen kérés jelentése - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>Az olyan metódusok, amelyek nyers választ adnak vissza, mint például a fenti miniatűr, a státuszkód ellenőrzését Önre bízzák a <code>IsSuccessStatusCode</code> vagy <code>EnsureSuccessStatusCode</code> segítségével.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Mivel további funkciókat nyújt a kliens">}}

<p>A keresés, letöltés és feltöltés a gyakori hívások. A kliensnek van egy műveletcsoportja minden egyéb DICOMweb szolgáltatáshoz is:</p>

<ul>
<li>Tömeges adat, pixel adat és a példány kiválasztott képkockái, egy alapú képkockaszám alapján, ahogy a szerver küldi.</li>
<li>WADO-URI, a régebbi egyetlen URL-re épülő lekérdezés, amelyet sok nézegető és archívum még mindig használ DICOM fájlok és renderelt képek esetén.</li>
<li>UPS-RS munkalisták: munkatétel létrehozása, keresése, frissítése és lefoglalása, állapotuk módosítása, törlés kérése, és eseményekre való feliratkozás, amelyek WebSocket-en keresztül érkeznek.</li>
<li>A modalitás munkalista (MWL-RS), a modalitás által végrehajtott eljárási lépések (MPPS-RS) és a tárolási kötelezettség.</li>
<li>Nem-páciens példányok, mint például színpaletták és függes protokollok, valamint a szerver képesség-dokumentuma.</li>
<li><code>SendAsync</code> bármely standard tranzakcióhoz a saját útváltozóival, lekérdezési paramétereivel, fejléceivel és törzsével, ha egy típusos metódus nem felel meg.</li>
</ul>

<p>A <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb kliens útmutató</a> minden műveletet és lehetőséget dokumentál, és a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">kliens műveleti referencia</a> felsorolja az összes metódust.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Egy könyvtár a HTTP válaszból a pixel adatokig">}}

<p>A kliens segítségével letöltött fájl egy szokásos <code>DicomFile</code>. Anonimizálható, átkódolható, renderelhető vagy sorosítható ugyanabban a folyamatban, konverziós lépés vagy második könyvtár használata nélkül.</p>

<p>Kapcsolódó oldalak: <a href="/medical/net/dicom-networking/">DICOM hálózat</a> archívumok számára, amelyek a DIMSE-t használják HTTP helyett, <a href="/medical/net/anonymization/">anonimizálás</a> a vizsgálat kórház elhagyása előtt, <a href="/medical/net/dicom-transfer-syntax-conversion/">átviteli szintaxis konverzió</a> a szerver által tárolt adatokhoz, és <a href="/medical/net/dicom-to-json/">DICOM JSON-re</a> a QIDO-RS keresési eredmények formátumához.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Fejlesztői útmutató" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="API referenciák" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
