---
title: DICOMweb kliens C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Keresés, letöltés és feltöltés DICOM vizsgálatok HTTP-n keresztül .NET-ből. Egy tiszta C#‑ban írt DICOMweb kliens QIDO-RS, WADO-RS és STOW-RS számára, amely együttműködik PACS, VNA és felhő alapú képalkotó szolgáltatásokkal.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb kliens .NET C# számára" h2="Keresés, letöltés és feltöltés vizsgálatok bármely DICOMweb szerveren QIDO-RS, WADO-RS és STOW-RS segítségével, teljesen felügyelt C# környezetben, semmilyen telepítés nélkül a gépen." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM HTTP-n keresztül, saját kódból">}}

<p>A DICOMweb a DICOM HTTP-alapú változata. A 104-es porton történő DIMSE kapcsolat helyett az alkalmazás egyszerű HTTPS kéréseket küld: QIDO-RS a kereséshez, WADO-RS a letöltéshez, STOW-RS a feltöltéshez. Az olyan felhő alapú képalkotó szolgáltatások, mint az Azure Health Data Services DICOM szolgáltatás és a Google Cloud Healthcare API is így működnek, akárcsak az Orthanc és a dcm4chee archívumok.</p>

<p><strong>Aspose.Medical for .NET</strong> már a 26.10-es verziótól tartalmaz DICOMweb klienst. A <code>DicomWebClient</code> minden standard tranzakciót egy típusos metódusba burkol, és az eredmények ugyanazok a <code>Dataset</code> és <code>DicomFile</code> objektumok, amelyeket a könyvtár többi része olvas és ír. A másik oldalon lévő szerver lehet bármilyen DICOMweb implementáció; nem szükséges, hogy Aspose.Medical‑lel készült legyen.</p>

<p>A kliens felügyelt C#-on, a <code>HttpClient</code> tetején alapul. Nincs natív eszközkészlet és proxy szolgáltatás, így ugyanaz a kód fut Windows, Linux és egy konténer környezetben is.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kapcsolódás DICOMweb szerverhez">}}

<p>Kliens létrehozása egy <code>HttpClient</code> objektumból és a szerver szolgáltatás alap URL-jéből. A hitelesítés, TLS beállítások, proxyk és a kapcsolatkezelés a <code>HttpClient</code>-en marad, ahol az alkalmazás már konfigurálja őket. A kliens nem birtokolja a <code>HttpClient</code>-et, és nem is zárja le azt.</p>

<div class="codeblock" id="code">
 <h3>DICOMweb kliens létrehozása - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Ha egy telepítés WADO-URI‑t vagy más szolgáltatást külön URL‑ről szolgáltat, a <code>ServiceBaseUris</code> felülírja az alap URL-t csak az adott szolgáltatásra.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vizsgálatok keresése QIDO-RS használatával">}}

<p>A keresés egyező paramétereket vár a DICOM kulcsszóval megnevezve, a visszaadandó mezőket és az oldalak vezérlését. A találatok adathalmazok áramlata formájában érkeznek, egyesével olvasva, amíg a válasz még letöltődik.</p>

<div class="codeblock" id="code">
 <h3>Beteg vizsgálatainak megtalálása - C#</h3>
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

<p>A <code>SearchSeriesAsync</code> és a <code>SearchInstancesAsync</code> egy szinttel lejjebb keres, akár az egész archívumban, akár egy vizsgálaton vagy sorozaton belül. Ezek az opciók lehetővé teszik a fuzzy (homályos) egyezést, az üres értékek egyezését és az eredményeltolást az oldalak kezeléséhez.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vizsgálat letöltése WADO-RS használatával">}}

<p>Egy vizsgálat több ezer példányt is tartalmazhat. A <code>RetrieveStudyAsync</code> áramlásként adja vissza őket, így minden fájl feldolgozásra kerül, amint a válasz része megérkezik, és menthető a következő beolvasása előtt. A memóriahasználat nem nő a vizsgálat méretével.</p>

<div class="codeblock" id="code">
 <h3>A vizsgálat minden példányának mentése - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Miniatűr vagy renderelt kép lekérése">}}

<p>Egy néző vagy egy munkalista képernyő gyakran képet igényel, nem DICOM fájlt. A szerver rendereli azt, és JPEG‑et vagy PNG‑t küld, így a kliens nem tölti le és nem dekódolja az eredeti pixel adatokat.</p>

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

<p>A <code>RetrieveRenderedInstanceAsync</code> és a <code>RetrieveRenderedFramesAsync</code> helyette egy teljes méretű képet ad vissza. A <code>DicomWebRenderOptions</code> a szabványos renderelési beállításokat továbbítja a szervernek: képminőség, nézőablak mérete, ablak és ICC profil.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Példányok feltöltése STOW-RS használatával">}}

<p>A STOW-RS egy kérésben tetszőleges számú példányt küld. A szerver egy jelentéssel válaszol, amely példányonként felsorolja, mit tárolt és mit utasított el.</p>

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

<p>Egyes szerverek HTTP hibával (például 409) válaszolnak, ha egy vagy több példány hibázik. Ekkor a hívás <code>DicomWebClientException</code>-t dob, ahogy a következő szakasz mutatja, és ugyanaz a jelentés megtalálható a kivétel választestében.</p>

<p>A <code>StoreStudyAsync</code> egy nevű vizsgálatba tölt fel. Mindkét metódus elfogad egy <code>IAsyncEnumerable&lt;DicomFile&gt;</code>-t is, így a fájlok a kérés küldése közben is előállíthatók.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Szerverhibák kezelése">}}

<p>Minden típusos művelet ellenőrzi a HTTP státuszkódot a visszatérés előtt. Ha a szerver elutasít egy kérést, a hívás <code>DicomWebClientException</code>-t dob, amely tartalmazza a státuszkódot, a fejléceket és a választest elejét a naplózáshoz.</p>

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

<p>A nyers választ visszaadó metódusok, mint például a fenti miniatűr, a státuszkód ellenőrzését a felhasználóra bízzák a <code>IsSuccessStatusCode</code> vagy <code>EnsureSuccessStatusCode</code> segítségével.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Milyen további funkciókat fed le a kliens">}}

<p>A keresés, letöltés és feltöltés a gyakori hívások. A kliens további műveletcsoportokkal rendelkezik a többi DICOMweb szolgáltatáshoz is:</p>

<ul>
<li>Bulk adat, pixel adat és egy példány kiválasztott képkockái, egy‑alapú képkockaszám szerint, ahogyan a szerver elküldte.</li>
<li>WADO-URI, a régebbi egy‑URL‑es lekérdezés, amelyet még sok néző és archívum használ DICOM fájlok és renderelt képek esetén.</li>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> munkalisták: létrehozás, keresés, frissítés és munkatétel lefoglalása, állapotuk módosítása, lemondás kérése, valamint a WebSocket‑en érkező eseményekre feliratkozás.</li>
<li>A <a href="/medical/net/modality-worklist/">modality munkalista</a> (MWL-RS), a modality által végrehajtott eljárási lépések (MPPS-RS) és a tárolási kötelezettség.</li>
<li>Nem‑páciensekhez tartozó példányok, például színpaletták és függő protokollok, valamint a szerver képesség-dokumentuma.</li>
<li><code>SendAsync</code> bármely standard tranzakcióhoz, saját útvonal‑értékekkel, lekérdezési paraméterekkel, fejlécekkel és törzzsel, ha a típusos metódus nem alkalmas.</li>
</ul>

<p>A <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb kliens útmutató</a> dokumentálja minden műveletet és beállítást, a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">kliens műveleti referencia</a> pedig felsorolja az összes metódust.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Egy könyvtár az HTTP válasstól a pixel adatokig">}}

<p>A klienssel letöltött fájl egy szabályos <code>DicomFile</code>. Anonimizálható, transzkódolható, renderelhető vagy sorosítható ugyanabban a folyamatban, átalakítási lépés vagy második könyvtár nélkül a láncban.</p>

<p>Kapcsolódó oldalak: <a href="/medical/net/dicomweb-server/">DICOMweb szerver</a> a protokoll másik oldalához az Ön saját ASP.NET Core szolgáltatásában, <a href="/medical/net/dicom-networking/">DICOM hálózat</a> archivumokhoz, amelyek DIMSE‑t használnak HTTP helyett, <a href="/medical/net/anonymization/">anonimizálás</a> a vizsgálat kórház elhagyása előtt, <a href="/medical/net/dicom-transfer-syntax-conversion/">átviteli szintaxis konverzió</a> a szerver által tárolt formátumhoz, és <a href="/medical/net/dicom-to-json/">DICOM JSON‑ná alakítás</a> a QIDO-RS keresési eredmények formátumához.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Fejlesztői útmutató" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetős támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért az Aspose.Medical .NET‑hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyféllista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
