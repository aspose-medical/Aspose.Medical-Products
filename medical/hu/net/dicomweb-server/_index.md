---
title: DICOMweb szerver C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Építsd saját DICOMweb szervered vagy PACS-od C#-ban. Az Aspose.Medical megvalósítja a QIDO-RS, WADO-RS, STOW-RS és a munkalistákkal kapcsolatos szolgáltatásokat az ASP.NET Core-on; te csatlakoztatod a tárolót.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb szerver .NET C#-hoz" h2="Adj hozzá QIDO-RS, WADO-RS és STOW-RS végpontokat egy ASP.NET Core szolgáltatáshoz. A könyvtár beszéli a protokollt; a te kódod dönt a tanulmányok tárolási helyéről." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Saját DICOMweb szerver C#-ban">}}

<p>A PACS, egy VNA, egy felhőarchívum vagy egy kutatási platformnak meg kell válaszolnia a DICOMweb kéréseket: QIDO-RS keresések, WADO-RS letöltések és STOW-RS feltöltések, valamint gyakran a munkalistákkal kapcsolatos szolgáltatásokat is. Mindegyiknek saját útvonala, média típusa, multipart testje, egyezési szabályai és állapotkódjai vannak.</p>

<p><strong>Aspose.Medical for .NET</strong> már a 26.10-es verzió óta megvalósítja ezt a protokolloldalt. Minden kérést beolvas, ellenőrzi a szabvány alapján, a DICOM szabályok szerint egyezteti a keresési kulcsokat, lapozza az eredményeket, átalakítja az átvitel szintaxist és megírja a választ. A te kódod teszi azt, amit csak te tudsz: egy példányt tárol, keresési jelölteket talál, és visszaadja a tárolt fájlokat. A megtekintők, modalitások és egyéb archívumok ezután a szervereddel úgy működnek, mint bármely más DICOMweb szerverrel, és ugyanígy a <a href="/medical/net/dicomweb-client/">DICOMweb kliens</a> is ugyanabból a könyvtárból.</p>

<p>A szerver C#-ban kezelt. Ahol az ASP.NET Core fut, ott fut: Windows-on, Linuxon és egy konténerben.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Teljes körű szerver ASP.NET Core-ban">}}

<p>Telepítsd a <code>Aspose.Medical.Dicom.Web.AspNetCore</code> csomagot. Ez magával hozza az Aspose.Medical-t, és összekapcsolja a DICOMweb szolgáltatásokat a függőséginjektálással és az útvonalkezeléssel. Ez a <code>Program.cs</code> egy működő Studies szerver; az általa regisztrált két osztály lent látható.</p>

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

<p>Most minden Studies útvonal a <code>/dicomweb</code> alatt válaszol: kereséshez és feltöltéshez a <code>/dicomweb/studies</code>, lekéréshez a <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code>, valamint a metaadat, keret, nagy mennyiségű adat és renderelt képek útvonalai. A szolgáltatás gyökerénél lévő képességek dokumentuma pontosan felsorolja a szerver által támogatott tranzakciókat.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Csatlakoztasd a tárolót">}}

<p>A Studies szolgáltatásnak egy interfészre van szüksége három metódussal: egy példány tárolása, a tárolt fájlok visszaadása és a keresési jelöltek visszaadása. Ez a megvalósítás minden példányt fájlként tárol egy mappában tanulmányonként és sorozatonként.</p>

<div class="codeblock" id="code">
 <h3>Mappaarchívum a QIDO-RS, WADO-RS és STOW-RS mögött - C#</h3>
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

<p>A könyvtár minden kérés útvonalában és minden feltöltött példányban ellenőrzi az UID-ket, mielőtt meghívná a szolgáltatót. Egy érvényes UID-kkel nem rendelkező példány hibaként jelentkezik, és soha nem ér el a <code>StoreAsync</code>-hoz.</p>

<p>Egy mappa elegendő a szerver kipróbálásához. Egy éles archívum ugyanazt a három metódust használja, és egy objektumtárolót helyez a <code>StoreAsync</code> és <code>RetrieveAsync</code> mögé, valamint egy adatbázis-indexet a <code>FindCandidatesAsync</code> mögé.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Keresés: a könyvtár alkalmazza a DICOM egyezési szabályokat">}}

<p>A QIDO-RS egyezés több mint egyszerű karakterlánc összehasonlítás: helyettesítő karakterek, dátum- és időtartományok, UID-listák, fuzzy személynevek, üres értékek és kulcsok a szekvenciákon belül. A szolgáltató nem valósítja meg ezeket. Jelölteket stabil sorrendben ad vissza, és lehet, hogy több elemet ad, mint amennyi egyezik. A könyvtár minden jelöltre pontos DICOM egyezést alkalmaz, megtartja a kért mezőket, és alkalmazza a <code>offset</code> és <code>limit</code> paramétereket. Egy válasz alapértelmezés szerint legfeljebb 1000 eredményt tartalmaz, és a <code>DicomWebStandardServiceOptions</code> módosítja ezt a korlátot.</p>

<p>A keresések gyorsan tartásához nagy archívum esetén add át a szelektív kulcsokat az adatbázisnak. A <code>search.Plan.Filter</code> tartalmazza a lefordított keresési kulcsokat, az attribútummal, a egyezés típusával és az egyes értékekkel:</p>

<div class="codeblock" id="code">
 <h3>Pontos Patient ID lekérdezése a keresésből - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Ezzel a módon épített adatbázis lekérdezés extra sorokat adhat vissza, például ha egy nem lefordítható kulcsot figyelmen kívül hagy. A könyvtár végső egyezése ezeket eltávolítja. Nem szabad eldobni a valódi egyezéseket, mert a könyvtár nem tudja visszailleszteni őket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tárolás: eredmény minden példányhoz">}}

<p>Egy STOW-RS kérés több száz példányt is tartalmazhat. A könyvtár elemzi a multipart testet, és sorban meghívja a <code>StoreAsync</code> metódust minden egyes példányra, így a teljes feltöltés soha nem kerül egyszerre a memóriába. Minden példányra a szolgáltató visszaadja a <code>Stored</code>, <code>Warning</code> (változásokkal tárolt példány) vagy <code>Failed</code> állapotot egy opcionális DICOM hiba kóddal.</p>

<p>A könyvtár a fenti eredményekből építi fel a STOW-RS jelentést, és kiválasztja a státuszkódot: 200, ha minden példány sikeresen tárolva van; 202, ha egyeseknek figyelmeztetései vagy hibái vannak; 409, ha mindegyik hibás. Egy kérésben egy elnevezett tanulmányhoz tartozó, de másik tanulmányhoz tartozó példányok elutasításra kerülnek, mielőtt a szolgáltató meghívásra kerülne.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lekérés: átvitel szintaxisok, keretek és renderelt képek">}}

<p>A szolgáltató visszaadja a tárolt fájlokat, a könyvtár pedig a kliens által kért formátumban szolgáltatja őket. Egy WADO-RS kliens kérhet egy átviteli szintaxist a <code>Accept</code> fejléchez, és a könyvtár minden fájlt átkonvertál, mielőtt elküldené, saját kodekekkel, beleértve a HTJ2K és JPEG XL támogatást. A metaadatok DICOM JSON vagy XML formátumban, a nagy mennyiségű adat és az egyes keretek ugyanabból a szolgáltatóból származnak, extra kód nélkül.</p>

<p>Renderelt képekhez egy további osztály szükséges, mivel csak te döntöd el, mely képeket állíthatja elő a szerver. Ez JPEG-et kínál renderelt példányokhoz, renderelt keretekhez és bélyegképekhez, a könyvtár pedig ezeket rendereli:</p>

<div class="codeblock" id="code">
 <h3>Renderelt JPEG képek és bélyegképek ajánlása - C#</h3>
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

<p>A beépített renderelő JPEG-et állít elő egy egykeretes példányból vagy a kérés által kiválasztott keretekből. Egy renderelt többkeretes példány teljes egészében, egy 65500 pixelnél szélesebb vagy magasabb kép (bélyegképek átmennek ezeken a képeken), egyéb képfájlformátumok, valamint a nézetablak, ablak és ICC profil paraméterek egy saját <code>RetrieveAsync</code> metódust igényelnek ebben az osztályban.</p>

<p>A bélyegképek kezelése a beteg adatvédelmét szem előtt tartva történik. A beépített renderelő csak olyan példányból készít bélyegképet, amelynek a Burned In Annotation és a Recognizable Visual Features attribútumai mind <code>NO</code>, ami azt jelenti, hogy a kép maga kijelenti, hogy nem tartalmaz betegadatot. Sok kép nem rendelkezik ezekkel a tulajdonságokkal, és ilyen esetben a szerver 406 választ ad. Ha az archívumodnak ilyen képek bélyegképeire van szüksége, állítsd elő őket a saját <code>RetrieveAsync</code> metódusoddal, és győződj meg arról, hogy elrejtik a betegadatokat.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Munkalisták, eljáráslépések és a többi szolgáltatás">}}

<p>A többi DICOMweb szolgáltatás ugyanazt a modellt követi: regisztrálja a szolgáltatást, megvalósítja annak tároló interfészét.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> munkalisták a <code>AddStandardWorklistService</code> segítségével: munkatárgyak létrehozása, keresése és frissítése, valamint állapotuk módosítása, a könyvtár által ellenőrzött állapotszabályokkal és WebSocketen keresztül küldött eseményértesítésekkel.</li>
<li>A <a href="/medical/net/modality-worklist/">modality worklist</a> (MWL-RS) a <code>AddStandardModalityWorklistService</code> használatával, így a modalitások lekérdezhetik a RIS-edben ütemezett eljáráslépéseket.</li>
<li>Modality performed procedure steps (MPPS-RS), storage commitment és nem-páciens példányok, mint például színpaletták és függeszkedési protokollok.</li>
<li>WADO-URI lekérés régebbi megjelenítők számára, a Studies szolgáltatás által kiszolgált.</li>
</ul>

<p>Ha egy tranzakció szabványos viselkedése nem megfelelő, regisztrálj saját kezelőt hozzá a <code>AddHandler</code> segítségével. A könyvtár továbbra is útvonalt és ellenőrzést végez a kérésen, és a többi tranzakció megtartja a szabványos kezelőit.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Biztonság és üzemeltetés a te kezedben marad">}}

<p>A DICOMweb végpontok egyszerű ASP.NET Core végpontok. A hitelesítés, jogosultságkezelés, CORS, TLS, továbbított fejléc és a kérés méretkorlátok ugyanúgy konfigurálhatók, mint a szolgáltatás többi része esetén:</p>

<div class="codeblock" id="code">
 <h3>Hitelesített felhasználó kötelező - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>A protokoll része nem függ az ASP.NET Core-tól. A <code>DicomWebApplication</code> egy <code>DicomWebRequest</code>-et válasszá alakít, így ugyanazok a szolgáltatások egy másik HTTP szerver mögött vagy hálózat nélküli tesztkörnyezetben is futtathatók.</p>

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

<p>Az Aspose.Medical <a href="/medical/net/dicomweb-client/">DICOMweb kliens</a> egységtesztből vagy konzolprogramból teszteli a szervert, és a szerver által fogadott minden fájl egy szabványos <code>DicomFile</code>, amely anonimizálható, átkódolható vagy renderelhető ugyanabban a folyamatban.</p>

<p>A <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb fejlesztői útmutató</a> minden szolgáltatást és tároló interfészt lefed, és a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core útmutató</a> az endpoint leképezést és a WebSocket-eket tárgyalja.</p>

<p>Kapcsolódó oldalak: <a href="/medical/net/dicom-networking/">DICOM hálózat</a> azoknak a rendszereknek, amelyek a tanulmányokat DIMSE-n keresztül, nem HTTP-n küldik; <a href="/medical/net/anonymization/">anonimizálás</a> a bejövő tanulmányoknál; <a href="/medical/net/dicom-transfer-syntax-conversion/">átviteli szintaxis konverzió</a>; és <a href="/medical/net/dicom-to-json/">DICOM JSON-re</a> a QIDO-RS eredmények formátumához.</p>

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
{{< blocks/products/pf/slr-element name="Fizetős támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért az Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Vevők listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
