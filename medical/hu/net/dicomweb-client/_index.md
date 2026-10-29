---
title: DICOMweb kliens C# .NET-ben - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Keresés, letöltés és feltöltés DICOM tanulmányok HTTP-n keresztül .NET-ből. Egy tiszta C# DICOMweb kliens QIDO-RS, WADO-RS és STOW-RS támogatással, amely működik PACS, VNA és felhő alapú képalkotó szolgáltatásokkal.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb kliens .NET C#-hoz" h2="Keresés, letöltés és feltöltés tanulmányok bármely DICOMweb szerveren QIDO-RS, WADO-RS és STOW-RS használatával, managed C#-ban, semmi telepítendő a gépre." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM HTTP-n keresztül, saját kódból">}}

<p>DICOMweb a DICOM HTTP változata. A 104-es porton lévő DIMSE asszociáció helyett az alkalmazás egyszerű HTTPS kéréseket küld: QIDO-RS kereséshez, WADO-RS letöltéshez, STOW-RS feltöltéshez. Ilyen módon működnek a felhő alapú képalkotó szolgáltatások, például az Azure Health Data Services DICOM szolgáltatás és a Google Cloud Healthcare API, valamint az Orthanc és a dcm4chee archivák.</p>

<p><strong>Aspose.Medical for .NET</strong> DICOMweb klienst tartalmaz a 26.10-es verzió óta. A <code>DicomWebClient</code> minden standard tranzakciót egy típusos metódusba csomagol, és az eredmények ugyanazokként érkeznek <code>Dataset</code> és <code>DicomFile</code> objektumokként, amelyeket a könyvtár többi része olvas és ír. A másik oldalon lévő szerver lehet bármely DICOMweb implementáció; nem kell, hogy Aspose.Medical‑lel készüljön.</p>

<p>A kliens managed C# a <code>HttpClient</code> felett. Nincs natív eszközkészlet és proxy szolgáltatás, így ugyanaz a kód fut Windowson, Linuxon és egy konténerben.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kapcsolódás DICOMweb szerverhez">}}

<p>Hozza létre a klienset egy <code>HttpClient</code> és a szerver szolgáltatásgyökér URL-jének felhasználásával. A hitelesítés, TLS beállítások, proxyk és a kapcsolatkezelés a <code>HttpClient</code>-en marad, ahol az alkalmazás már konfigurálja őket. A kliens nem birtokolja a <code>HttpClient</code>-et, és nem zárja le azt.</p>

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

<p>Ha egy telepítés WADO-URI‑t vagy egy másik szolgáltatást más URL‑ről szolgál ki, a <code>ServiceBaseUris</code> csak az adott szolgáltatás gyökerét írja felül.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tanulmányok keresése QIDO-RS-sel">}}

<p>A keresés a DICOM kulcsszóval azonosított egyező paramétereket, a visszaadandó mezőket és az oldalak vezérlését tartalmazza. A találatok adathalmazok áramlataként érkeznek, egyesével olvasva, amíg a válasz még letöltődik.</p>

<div class="codeblock" id="code">
 <h3>Beteg tanulmányainak megtalálása - C#</h3>
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

<p>A <code>SearchSeriesAsync</code> és <code>SearchInstancesAsync</code> egy szinttel lejjebb keres, az egész archívumon vagy egy tanulmányon vagy sorozaton belül. Ugyanezek az opciók állítják be a fuzzy egyezést, az üres érték egyezést és az oldalak eltolását.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tanulmány letöltése WADO-RS-sel">}}

<p>Egy tanulmány több ezer példányt tartalmazhat. A <code>RetrieveStudyAsync</code> áramlásként adja vissza őket, így minden fájl feldolgozásra kerül, amint a válasz része megérkezik, és menthető, mielőtt a következő beolvasásra kerül. A memóriahasználat nem nő a tanulmány méretével.</p>

<div class="codeblock" id="code">
 <h3>Tanulmány minden példányának mentése - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p>A <code>RetrieveSeriesAsync</code> és <code>RetrieveInstanceAsync</code> egy sorozatot vagy egy példányt ugyanígy töltenek le.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Miniatűr vagy renderelt kép lekérése">}}

<p>Egy megjelenítő vagy munkalista képernyő gyakran képet igényel, nem DICOM fájlt. A szerver rendereli azt, és JPEG vagy PNG formátumban küldi, így a kliens nem tölt le vagy dekódolja az eredeti pixeleket.</p>

<div class="codeblock" id="code">
 <h3>Példány JPEG miniatűrjének mentése - C#</h3>
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

<p>A <code>RetrieveRenderedInstanceAsync</code> és <code>RetrieveRenderedFramesAsync</code> helyette egy teljes méretű képet ad vissza. A <code>DicomWebRenderOptions</code> átadja a standard renderelési vezérlőket a szervernek: képpontminőség, nézőablak mérete, ablak és ICC profil.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Példányok feltöltése STOW-RS-sel">}}

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

<p>Néhány szerver HTTP hibával válaszol, például 409‑gyel, ha egy vagy több példány hibát okoz. Ebben az esetben a hívás <code>DicomWebClientException</code>-t dob, ahogy a következő rész mutatja, és ugyanaz a jelentés az exception választestében található.</p>

<p>A <code>StoreStudyAsync</code> egy elnevezett tanulmányba tölt fel. Mindkét metódus elfogad egy <code>IAsyncEnumerable&lt;DicomFile&gt;</code>-t is, így a fájlok a kérés küldése közben is előállíthatók.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Szerverhibák kezelése">}}

<p>Minden típusos művelet ellenőrzi az HTTP állapotot, mielőtt visszatér. Ha a szerver elutasít egy kérést, a hívás <code>DicomWebClientException</code>-t dob, amely a státuszkódot, a fejléceket és a választest elejét tartalmazza a naplóhoz.</p>

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

<p>Az olyan metódusok, amelyek nyers választ adnak vissza, például a fenti miniatűr, a státuszkontrollt a <code>IsSuccessStatusCode</code> vagy <code>EnsureSuccessStatusCode</code> segítségével a felhasználóra bízzák.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="A kliens további funkciói">}}

<p>A keresés, letöltés és feltöltés a gyakori hívások. A kliensnek minden egyes további DICOMweb szolgáltatásra is van egy műveletcsoportja:</p>

<ul>
<li>Tömeges adat, pixel adat és kiválasztott képkockák egy példányból, egy‑alapú képkockaszám szerint, ahogy a szerver küldi őket.</li>
<li>WADO-URI, a régebbi egy‑URL‑es lekérés, amelyet még sok megjelenítő és archívum használ, DICOM fájlok és renderelt képekhez.</li>
<li>UPS‑RS munkalisták: létrehozás, keresés, frissítés és munkaelemek lefoglalása, állapotuk módosítása, lemondás kérése, és feliratkozás a WebSocket‑en érkező eseményekre.</li>
<li>A modalitás munkalista (MWL‑RS), a modalitás által végrehajtott eljáráslépések (MPPS‑RS) és a tárolási kötelezettség.</li>
<li>Nem‑páciens példányok, mint például színpaletták és függes protokollok, valamint a szerver képességdokumentuma.</li>
<li><code>SendAsync</code> bármely standard tranzakcióhoz, saját útváltozókkal, lekérdezési paraméterekkel, fejlécekkel és törzzsel, ha egy típusos metódus nem megfelelő.</li>
</ul>

<p>A <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb kliens útmutató</a> dokumentál minden műveletet és opciót, a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">kliens műveleti referencia</a> pedig felsorolja az összes metódust.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Egy könyvtár a HTTP válaszról a pixel adatokig">}}

<p>A klienssel letöltött fájl egy szokásos <code>DicomFile</code>. Anonimizálható, átkódolható, renderelhető vagy sorosítható ugyanabban a folyamatban, konverziós lépés vagy második könyvtár nélkül a láncban.</p>

<p>Kapcsolódó oldalak: <a href="/medical/net/dicomweb-server/">DICOMweb szerver</a> a protokoll másik oldalához saját ASP.NET Core szolgáltatásában, <a href="/medical/net/dicom-networking/">DICOM hálózat</a> archivumokhoz, amelyek DIMSE‑t használnak HTTP helyett, <a href="/medical/net/anonymization/">anonimizálás</a> mielőtt egy tanulmány elhagyja a kórházat, <a href="/medical/net/dicom-transfer-syntax-conversion/">átviteli szintaxis konverzió</a> a szerver által tárolt adatokhoz, illetve <a href="/medical/net/dicom-to-json/">DICOM JSON‑ra</a> a QIDO‑RS keresési eredmények formátumához.</p>

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

{{< blocks/products/pf/slr-tab tabTitle="Miért Aspose.Medical .NET‑hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyféllista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
