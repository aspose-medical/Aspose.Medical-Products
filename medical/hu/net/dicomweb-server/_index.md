---
title: DICOMweb szerver C# .NET-ben - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Készítsd el saját DICOMweb szervered vagy PACS-odat C#-ban. Az Aspose.Medical megvalósítja a QIDO-RS, WADO-RS, STOW-RS és a munkalista szolgáltatásokat az ASP.NET Core-on; te csatlakoztatod a tárolót.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb szerver .NET C#-hoz" h2="Adj hozzá QIDO-RS, WADO-RS és STOW-RS végpontokat egy ASP.NET Core szolgáltatáshoz. A könyvtár beszéli a protokollt; a kódod dönt arról, hogy hol tárolják a vizsgálatokat." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Saját DICOMweb szerver C#-ban">}}

<p>Egy PACS, VNA, felhőarchívum vagy kutatási platformnak válaszolnia kell a DICOMweb kérésekre: QIDO-RS keresések, WADO-RS letöltések és STOW-RS feltöltések, valamint gyakran a munkalista szolgáltatások is. Mindegyiknek saját útvonala, média típusa, multipart törzse, egyezési szabályai és állapotkódjai vannak.</p>

<p><strong>Aspose.Medical for .NET</strong> már a 26.10-es verzió óta megvalósítja ezt a protokolloldalt. Beolvassa minden kérést, ellenőrzi a szabványnak megfelelően, a DICOM szabályok szerint párosítja a keresési kulcsokat, oldalazza az eredményeket, konvertálja az átvitel szintaxisait és írja a választ. A te kódod teszi azt, amit csak te tudsz: egy példány tárolása, keresési jelöltek megtalálása és a tárolt fájlok visszaadása. A megjelenítők, modalitások és egyéb archívumok ezután a szervereddel úgy működnek, mint bármely más DICOMweb szerverrel, és ugyanígy működik a <a href="/medical/net/dicomweb-client/">DICOMweb kliens</a> is ugyanabból a könyvtárból.</p>

<p>A szerver menedzselt C#-ban készült. Bárhol fut, ahol az ASP.NET Core fut: Windows-on, Linux-on és egy konténerben.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Teljes szerver ASP.NET Core-ban">}}

<p>Telepítsd a <code>Aspose.Medical.Dicom.Web.AspNetCore</code> csomagot. Ez magával hozza az Aspose.Medical-t, és összekapcsolja a DICOMweb szolgáltatásokat a függőséginjekcióval és az endpoint routinggal. Ez a <code>Program.cs</code> egy működő Studies szerver; az általa regisztrált két osztály alább látható.</p>

<div class="codeblock" id="code">
 <h3>Program.cs egy DICOMweb szerverből - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>Most már minden Studies útvonal a <code>/dicomweb</code> alatt válaszol: a <code>/dicomweb/studies</code> a kereséshez és feltöltéshez, a <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> a lekéréshez, valamint a metaadatok, keretek, tömeges adatok és renderelt képek útvonalai. A szolgáltatás gyökérénél található képességek dokumentuma pontosan felsorolja azokat a tranzakciókat, amelyeket a szerver támogat.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Csatlakoztasd a tárolódat">}}

<p>A Studies szolgáltatásnak egy interfészre van szüksége három módszerrel: egy példány tárolása, a tárolt fájlok visszaadása és a keresési jelöltek visszaadása. Ez a megvalósítás minden példányt fájlként tárol egy mappában vizsgálatonként és sorozatonként.</p>

<div class="codeblock" id="code">
 <h3>Egy mappaalapú archívum a QIDO-RS, WADO-RS és STOW-RS mögött - C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>A könyvtár ellenőrzi az UID-ket minden kérés útvonalában és minden feltöltött példányban, mielőtt meghívná a szolgáltatót. Egy érvényes UID-kkel nem rendelkező példány hibaként jelentkezik, és sosem ér el a <code>StoreAsync</code>-hez.</p>

<p>Egy mappa elegendő a szerver kipróbálásához. Egy éles archívum ugyanazt a három módszert használja, és egy objektumtárolót helyez a <code>StoreAsync</code> és <code>RetrieveAsync</code> mögé, valamint egy adatbázis-indexet a <code>FindCandidatesAsync</code> mögé.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Keresés: a könyvtár alkalmazza a DICOM egyezési szabályokat">}}

<p>A QIDO-RS egyezés több, mint karakterlánc-összehasonlítás: helyettesítő karakterek, dátum- és időtartományok, UID-listák, fuzzy személynevek, üres értékek és szekvenciákon belüli kulcsok. A szolgáltató nem valósít meg semmit ebből. Jelölteket stabil sorrendben ad vissza, és lehet, hogy több, mint a tényleges egyezést adja. A könyvtár az egyes jelöltekre a pontos DICOM egyezést alkalmazza, megőrzi a kért mezőket, és alkalmazza a <code>offset</code> és <code>limit</code> paramétereket. Egy válasz alapértelmezés szerint legfeljebb 1000 eredményt tartalmaz, ezt a <code>DicomWebStandardServiceOptions</code> módosíthatja.</p>

<p>A keresések gyorsaságának megőrzése érdekében egy nagy archívumban, add át a szelektív kulcsokat az adatbázisnak. A <code>search.Plan.Filter</code> tartalmazza a lefordított keresési kulcsokat, az attribútummal, a egyezés típusával és az egyes értékekkel együtt:</p>

<div class="codeblock" id="code">
 <h3>Pontos Patient ID beolvasása keresésből - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Egy ilyen módon felépített adatbázis-lekérdezés extra sorokat adhat vissza, például ha egy olyan kulcsot figyelmen kívül hagy, amelyet nem tud lefordítani. A könyvtár végső egyezése eltávolítja ezeket. Nem szabad eldobnia a valódi egyezéseket, mert a könyvtár nem tudja majd visszaadni őket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tárolás: eredmény minden példányra">}}

<p>Egy STOW-RS kérés akár több száz példányt is tartalmazhat. A könyvtár elemzi a multipart törzset és sorban meghívja a <code>StoreAsync</code>-t minden egyes példányra, így a teljes feltöltés sosem kerül egyszerre a memóriába. Minden példányra a szolgáltató <code>Stored</code>, <code>Warning</code> (változtatásokkal tárolt példány) vagy <code>Failed</code> választ ad vissza, opcionális DICOM hibakóddal.</p>

<p>A könyvtár ezekből az eredményekből építi fel a STOW-RS jelentést és kiválasztja az állapotot: 200, ha minden példány tárolva van, 202, ha néhány figyelmeztetést kap vagy hibás, 409, ha mindegyik hibás. Egy kérésben egy adott tanulmányhoz tartozó példányok, amelyek egy másik tanulmányhoz tartoznak, a szolgáltató meghívása előtt el lesznek utasítva.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lekérés: átvitel szintaxisok, képkockák és renderelt képek">}}

<p>A szolgáltató visszaadja a tárolt fájlokat, és a könyvtár a kliens által kért formában szállítja őket. Egy WADO-RS kliens kérhet átviteli szintaxist az <code>Accept</code> fejlécben, a könyvtár pedig minden fájlt átkonvertál, mielőtt elküldi, saját kodekekkel, beleértve a HTJ2K és JPEG XL támogatást. A metaadatok DICOM JSON vagy XML formátumban, a tömeges adatok és az egyedi keretek ugyanabból a szolgáltatóból származnak, extra kód nélkül.</p>

<p>Renderelt képekhez egy további osztályra van szükség, mivel csak te döntöd el, hogy a szerver milyen képeket hozhat elő. Ez az osztály JPEG-et biztosít a renderelt példányokhoz, renderelt képkockákhoz és bélyegképekhez, a könyvtár pedig ezeket rendereli:</p>

<div class="codeblock" id="code">
 <h3>Renderelt JPEG képek és bélyegképek kínálása - C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>A beépített renderelő JPEG-et készít egy egykeretes példányból vagy a kérelem által kiválasztott képkockákból. Egy renderelt többkeretes példány egészben, más képkimenetek, valamint a viewport, window és ICC profil paraméterekhez saját <code>RetrieveAsync</code> szükséges ebben az osztályban.</p>

<p>A bélyegképeket a beteg adatvédelmének figyelembevételével kezelik. A beépített renderelő csak olyan példányból készít bélyegképet, amelynek a Burned In Annotation és a Recognizable Visual Features attribútumai egyaránt <code>NO</code>, ami azt jelenti, hogy a kép maga kijelenti, hogy nem tartalmaz betegadatokat. Sok kép nem rendelkezik ezekkel az attribútumokkal, ezért a szerver 406-os választ ad. Ha az archívumodnak szüksége van ilyen képek bélyegképeire, állítsd elő őket a saját <code>RetrieveAsync</code> metódusoddal, és ügyelj arra, hogy elrejtsék a betegadatokat.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Munkalista, eljáráslépések és a többi szolgáltatás">}}

<p>A többi DICOMweb szolgáltatás ugyanazt a modellt követi: regisztráld a szolgáltatást, implementáld a tárolási interfészét.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> munkalisták a <code>AddStandardWorklistService</code> segítségével: munkatétel létrehozása, keresése és frissítése, állapotuk módosítása, ahol az állapotszabályokat a könyvtár ellenőrzi és az eseményértesítéseket WebSocket-en keresztül küldi.</li>
<li>A <a href="/medical/net/modality-worklist/">modality munkalista</a> (MWL-RS) a <code>AddStandardModalityWorklistService</code>-val, hogy a modalitások lekérdezhessék a RIS-edben ütemezett eljáráslépéseket.</li>
<li>Modality által végrehajtott eljáráslépések (MPPS-RS), tárolási kötelezettség és nem beteg-példányok, például színpaletták és hanging protokollok.</li>
<li>WADO-URI lekérés régebbi megjelenítők számára, a Studies szolgáltatás által kiszolgálva.</li>
</ul>

<p>Ha egy tranzakció szabványos viselkedése nem felel meg, regisztráld saját kezelőjét a <code>AddHandler</code> segítségével. A könyvtár továbbra is útvonalakat állít fel és ellenőrzi a kérést, a többi tranzakció megőrzi a szabványos kezelőket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Biztonság és üzemeltetés a te kezedben marad">}}

<p>A DICOMweb végpontok egyszerű ASP.NET Core végpontok. A hitelesítés, jogosultságkezelés, CORS, TLS, továbbított fejlécek és a kérés méretkorlátok ugyanúgy vannak konfigurálva, mint a szolgáltatásod többi része:</p>

<div class="codeblock" id="code">
 <h3>Hitelesített felhasználó megkövetelése - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>A protokoll rész nem függ az ASP.NET Core-tól. A <code>DicomWebApplication</code> egy <code>DicomWebRequest</code>-et válasszá alakít, így ugyanazok a szolgáltatások futtathatók egy másik HTTP szerver mögött vagy hálózat nélküli tesztben:</p>

<div class="codeblock" id="code">
 <h3>Ugyanaz a szerver ASP.NET Core nélkül - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kliens és szerver egy könyvtárból">}}

<p>Az Aspose.Medical <a href="/medical/net/dicomweb-client/">DICOMweb kliens</a> egységtesztből vagy konzolprogramból teszteli a szervert, és a szerver által kapott minden fájl egy hagyományos <code>DicomFile</code>, amely anonimizálható, átkódolható vagy renderelhető ugyanabban a folyamatban.</p>

<p>A <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb fejlesztői útmutató</a> minden szolgáltatást és tárolási interfészt lefed, a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core útmutató</a> pedig az endpoint leképezést és a WebSocket-eket tárgyalja.</p>

<p>Kapcsolódó oldalak: <a href="/medical/net/dicom-networking/">DICOM hálózat</a> azoknak a rendszereknek, amelyek tanulmányokat küldenek DIMSE-n keresztül HTTP helyett, <a href="/medical/net/anonymization/">anonimizálás</a> a bejövő tanulmányokhoz, <a href="/medical/net/dicom-transfer-syntax-conversion/">átviteli szintaxis konverzió</a>, és <a href="/medical/net/dicom-to-json/">DICOM JSON-re</a> a QIDO-RS eredmények formátumához.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Fejlesztői útmutató" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetett támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyféllista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikeres esetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
