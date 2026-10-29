---
title: DICOMweb szerver C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Építsd meg a saját DICOMweb szervered vagy PACS-odat C#-ban. Az Aspose.Medical megvalósítja a QIDO-RS, WADO-RS, STOW-RS és a munkalistákkal kapcsolatos szolgáltatásokat az ASP.NET Core-on; te csatlakoztatod a tárolót.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb szerver .NET C# számára" h2="Adj QIDO-RS, WADO-RS és STOW-RS végpontokat egy ASP.NET Core szolgáltatáshoz. A könyvtár beszéli a protokollt; a kódod dönt a tanulmányok tárolásáról." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="A saját DICOMweb szervered C#-ban">}}

<p>Egy PACS, VNA, felhőarchívum vagy kutatási platformnak válaszolnia kell a DICOMweb kérésekre: QIDO-RS keresések, WADO-RS letöltések és STOW-RS feltöltések, valamint gyakran a munkalista szolgáltatások is. Mindegyiknek megvannak a saját útvonalai, média típusa, multipart testje, egyezési szabályai és státuszkódjai.</p>

<p><strong>Aspose.Medical for .NET</strong> ezt a protokolloldalt valósítja meg a 26.10-es verzió óta. Olvassa minden kérést, ellenőrzi a szabvány szerint, a DICOM szabályok szerint egyezteti a keresési kulcsokat, oldalazza az eredményeket, átalakítja a transfer syntaxeket és írja a választ. A kódod teszi azt, amit csak te tudsz: egy példány tárolása, keresési jelöltek megtalálása és a tárolt fájlok visszaadása. A megjelenítők, modalitások és egyéb archívumok ezután a te szervereddel úgy működnek, mint bármely más DICOMweb szerverrel, és ugyanígy a <a href="/medical/net/dicomweb-client/">DICOMweb kliens</a> is, amely ugyanabból a könyvtárból származik.</p>

<p>A szerver C#-ban van menedzselve. Bárhol futtatható, ahol az ASP.NET Core fut: Windows-on, Linux-on és konténerben.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Teljes körű szerver ASP.NET Core-ban">}}

<p>Telepítsd a <code>Aspose.Medical.Dicom.Web.AspNetCore</code> csomagot. Ez magával hozza az Aspose.Medical-t, és összekapcsolja a DICOMweb szolgáltatásokat a függőséginjekcióval és az endpoint útválasztással. Ez a <code>Program.cs</code> egy működő Studies szerver; az általa regisztrált két osztály alább látható.</p>

<div class="codeblock" id="code">
 <h3>Program.cs egy DICOMweb szerverhez - C#</h3>
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

<p>Most minden Studies útvonal a <code>/dicomweb</code> alatt válaszol: <code>/dicomweb/studies</code> kereséshez és feltöltéshez, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> lekérdezéshez, valamint a metaadatok, keretek, bulk adatok és renderelt képek útvonalai. A szolgáltatás gyökerénél található képesítési dokumentum pontosan felsorolja a szerver által támogatott tranzakciókat.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Csatlakoztasd a tárolót">}}

<p>A Studies szolgáltatásnak egy interfészre van szüksége három metódussal: egy példány tárolása, a tárolt fájlok visszaadása, és a keresési jelöltek visszaadása. Ez a megvalósítás minden példányt egy fájlként tárol egy mappában tanulmányonként és sorozatonként.</p>

<div class="codeblock" id="code">
 <h3>Mappaarchívum QIDO-RS, WADO-RS és STOW-RS mögött - C#</h3>
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

<p>A könyvtár ellenőrzi az UID-ket minden kéréspath-ban és minden feltöltött példányban, mielőtt meghívná a szolgáltatót. Egy érvényes UID-kkel nem rendelkező példányt hibaként jelzi, és soha nem éri el a <code>StoreAsync</code> metódust.</p>

<p>Egy mappa elegendő a szerver kipróbálásához. Egy éles archívum ugyanazt a három metódust használja, és objektumtárat helyez a <code>StoreAsync</code> és <code>RetrieveAsync</code> mögé, valamint adatbázis indexet a <code>FindCandidatesAsync</code> mögé.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Keresés: a könyvtár alkalmazza a DICOM egyezési szabályokat">}}

<p>A QIDO-RS egyezés több, mint egyszerű karakterlánc-összehasonlítás: helyettesítő karakterek, dátum- és időintervallumok, UID-listák, homályos személynevek, üres értékek és kulcsok sorozatokon belül. A szolgáltató nem valósítja meg ezeket. Jelölteket stabil sorrendben ad vissza, és visszaadhat több elemet, mint ami egyezik. A könyvtár pontos DICOM egyezést alkalmaz minden jelöltre, megtartja a kért mezőket, és alkalmazza a <code>offset</code> és <code>limit</code> paramétereket. Egy válasz alapértelmezés szerint legfeljebb 1000 eredményt tartalmaz, és a <code>DicomWebStandardServiceOptions</code> módosíthatja ezt a korlátot.</p>

<p>A keresések gyorsaságának megőrzése érdekében egy nagy archívumban, add át a szelektív kulcsokat az adatbázisnak. A <code>search.Plan.Filter</code> tartalmazza a lefordított keresési kulcsokat, az attribútumot, a egyezés típusát és az egyes értékeket:</p>

<div class="codeblock" id="code">
 <h3>Pontos Patient ID kiolvasása keresésből - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Egy ilyen módon felépített adatbázis-lekérdezés extra sorokat is visszaadhat, például ha figyelmen kívül hagy egy olyan kulcsot, amelyet nem tud lefordítani. A könyvtár végső egyezése eltávolítja ezeket. Nem szabad elveszítenie a valódi egyezéseket, mert a könyvtár nem tudja visszaadni őket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tárolás: eredmény minden példányra">}}

<p>Egy STOW-RS kérés akár több száz példányt is tartalmazhat. A könyvtár elemzi a multipart testet és sorban meghívja a <code>StoreAsync</code> metódust minden példányra, így a teljes feltöltés sosem kerül egyszerre a memóriába. Minden példányra a szolgáltató <code>Stored</code>, <code>Warning</code> (változtatással tárolt példány) vagy <code>Failed</code> értéket ad vissza egy opcionális DICOM hiba kóddal.</p>

<p>A könyvtár a fenti eredményekből építi fel a STOW-RS jelentést, és a státuszkódot választja: 200, ha minden példány tárolva van, 202, ha néhány figyelmeztetést vagy hibát kap, 409, ha minden sikertelen. Egy név alapján megnevezett tanulmányra vonatkozó kérésben, de egy másik tanulmányhoz tartozó példányok visszautasításra kerülnek, mielőtt a szolgáltató meghívásra kerülne.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lekérdezés: transfer syntaxek, keretek és renderelt képek">}}

<p>A szolgáltató visszaadja a tárolt fájlokat, és a könyvtár a kliens által kért formátumban szolgáltatja őket. Egy WADO-RS kliens kérhet transfer syntaxet a <code>Accept</code> fejlécben, és a könyvtár minden fájlt átalakít a küldés előtt, saját kodekekkel, többek között HTJ2K és JPEG XL támogatással. A metaadatok DICOM JSON vagy XML formátumban, bulk adatok és egyedi keretek ugyanabból a szolgáltatóból származnak, extra kód nélkül.</p>

<p>A renderelt képekhez egy további osztály szükséges, mivel csak te döntöd el, mely képeket hozhatja létre a szerver. Ez az osztály JPEG-et kínál renderelt példányokhoz, renderelt keretekhez és bélyegképekhez, és a könyvtár ezeket rendereli:</p>

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

<p>A beépített renderelő JPEG-et állít elő egy egykeretes példányból vagy a kérés által kiválasztott keretekből. Egy teljes, többkeretes renderelt példány, egyéb képformátumok, valamint a viewport, window és ICC profil paraméterek saját <code>RetrieveAsync</code> implementációját igénylik ebben az osztályban.</p>

<p>A bélyegképek kezelése során a beteg adatvédelmét szem előtt tartjuk. A beépített renderelő csak olyan példányból készít bélyegképet, amelynek a Burned In Annotation és a Recognizable Visual Features attribútumai egyaránt <code>NO</code>, ami azt jelenti, hogy a kép maga jelzi, hogy nem tartalmaz betegadatot. Sok kép nem rendelkezik ezekkel az attribútumokkal, és ilyen esetben a szerver 406‑os választ ad. Ha az archivumnak ilyen képek bélyegképeire van szüksége, állítsd elő őket a saját <code>RetrieveAsync</code> metódusoddal, és biztosítsd, hogy elrejtse a betegadatokat.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Munkalisták, eljáráslépések és a többi szolgáltatás">}}

<p>A többi DICOMweb szolgáltatás ugyanazt a modellt követi: regisztráld a szolgáltatást, valósítsd meg a tárolási interfészt.</p>

<ul>
<li>UPS-RS munkalisták a <code>AddStandardWorklistService</code> segítségével: munkatárgyak létrehozása, keresése és frissítése, állapotuk módosítása, a könyvtár által ellenőrzött állapotszabályokkal és WebSocket-en keresztül küldött eseményértesítésekkel.</li>
<li>A modalitás munkalista (MWL-RS) a <code>AddStandardModalityWorklistService</code> segítségével, így a modalitások lekérdezhetik a RIS-edben ütemezett eljáráslépéseket.</li>
<li>Modality performed procedure steps (MPPS-RS), storage commitment és nem-beteg példányok, például színpaletták és hanging protocol-ok.</li>
<li>WADO-URI lekérés régebbi megjelenítőknek, a Studies szolgáltatás által kiszolgált.</li>
</ul>

<p>Ha egy tranzakció szabványos viselkedése nem megfelelő, regisztrálj saját kezelőt hozzá a <code>AddHandler</code> segítségével. A könyvtár továbbra is útvonalaz és ellenőrzi a kérést, és a többi tranzakció megtartja a standard kezelőit.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="A biztonság és a hosztolás a te kezedben marad">}}

<p>A DICOMweb végpontok egyszerű ASP.NET Core végpontok. A hitelesítés, jogosultságkezelés, CORS, TLS, továbbított fejlécek és a kérés méretkorlátok ugyanúgy vannak beállítva, mint a szolgáltatásod többi része esetén:</p>

<div class="codeblock" id="code">
 <h3>Hitelesített felhasználó megkövetelése - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>A protokoll rész nem függ az ASP.NET Core-tól. A <code>DicomWebApplication</code> egy <code>DicomWebRequest</code>-et válaszra alakít, így ugyanazok a szolgáltatások futtathatók másik HTTP szerver mögött vagy hálózat nélküli tesztben:</p>

<div class="codeblock" id="code">
 <h3>Ugyanez a szerver ASP.NET Core nélkül - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kliens és szerver egy könyvtárból">}}

<p>Az Aspose.Medical <a href="/medical/net/dicomweb-client/">DICOMweb kliens</a> unit tesztből vagy konzol programból teszteli a szervert, és minden bejövő fájl egy szabványos <code>DicomFile</code>, amely anonimizálható, transzkódolható vagy renderelhető ugyanabban a folyamatban.</p>

<p>A <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb fejlesztői útmutató</a> minden szolgáltatást és tárolási interfészt bemutat, a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core útmutató</a> pedig az endpoint leképezést és a WebSocket-eket tárgyalja.</p>

<p>Kapcsolódó oldalak: <a href="/medical/net/dicom-networking/">DICOM hálózat</a> rendszerekhez, amelyek tanulmányokat DIMSE-n keresztül, nem HTTP-n küldenek, <a href="/medical/net/anonymization/">anonymizálás</a> a bejövő tanulmányok esetén, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax átalakítás</a>, valamint <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> a QIDO-RS eredmények formátumához.</p>

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

{{< blocks/products/pf/slr-tab tabTitle="Miért Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyféllista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
