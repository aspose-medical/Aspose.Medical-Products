---
title: DICOMweb server v C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Vytvořte si vlastní DICOMweb server nebo PACS v C#. Aspose.Medical implementuje QIDO-RS, WADO-RS, STOW-RS a služby pracovního seznamu na ASP.NET Core; vy připojíte úložiště.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb server pro .NET C#" h2="Přidejte koncové body QIDO-RS, WADO-RS a STOW-RS do služby ASP.NET Core. Knihovna mluví protokolem; váš kód rozhoduje, kde jsou studie uloženy." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Váš vlastní DICOMweb server v C#">}}

<p>PACS, VNA, cloudové archiv nebo výzkumná platforma musí odpovídat na DICOMweb požadavky: vyhledávání QIDO-RS, stahování WADO-RS a nahrávání STOW-RS, a často také služby pracovních seznamů. Každá z nich má své vlastní cesty, typy médií, multipart těla, pravidla shody a stavové kódy.</p>

<p><strong>Aspose.Medical pro .NET</strong> implementuje tuto stranu protokolu od verze 26.10. Čte každý požadavek, kontroluje jej vůči standardu, porovnává klíče vyhledávání podle DICOM pravidel, stránkuje výsledky, převádí přenosové syntaxe a zapisuje odpověď. Váš kód dělá to, co můžete jen vy: uložit instanci, najít kandidáty pro vyhledávání a vrátit uložené soubory. Prohlížeče, modality a další archivy pak spolupracují s vaším serverem jako s jakýmkoli jiným DICOMweb serverem, a taktéž <a href="/medical/net/dicomweb-client/">klient DICOMweb</a> stejné knihovny.</p>

<p>Server je spravován v C#. Běží kdekoliv, kde běží ASP.NET Core: na Windows, na Linuxu a v kontejneru.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kompletní server v ASP.NET Core">}}

<p>Nainstalujte balíček <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Ten přináší Aspose.Medical a propojuje služby DICOMweb s injekcí závislostí a směrováním koncových bodů. Tento <code>Program.cs</code> je funkční server studií; níže jsou zobrazeny dvě třídy, které registruje.</p>

<div class="codeblock" id="code">
 <h3>Program.cs DICOMweb serveru - C#</h3>
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

<p>Každá cesta Studies nyní odpovídá pod <code>/dicomweb</code>: <code>/dicomweb/studies</code> pro vyhledávání a nahrávání, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> pro načítání a cesty pro metadata, snímky, hromadná data a renderované obrázky. Dokument schopností u kořene služby přesně uvádí transakce, které server podporuje.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Připojte své úložiště">}}

<p>Služba Studies potřebuje jedno rozhraní se třemi metodami: uložit instanci, vrátit uložené soubory a vrátit kandidáty vyhledávání. Tato implementace uchovává každou instanci jako soubor ve složce pro každou studii a sérii.</p>

<div class="codeblock" id="code">
 <h3>Archiv ve složkách za QIDO-RS, WADO-RS a STOW-RS - C#</h3>
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

<p>Knihovna kontroluje UID v každé cestě požadavku i v každé nahrané instanci, než zavolá poskytovatele. Instance bez platných UID je označena jako selhaná a nikdy nedosáhne <code>StoreAsync</code>.</p>

<p>Složka stačí pro vyzkoušení serveru. Produkční archiv zachovává stejné tři metody a umisťuje objektové úložiště za <code>StoreAsync</code> a <code>RetrieveAsync</code> a databázový index za <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vyhledávání: knihovna aplikuje DICOM pravidla shody">}}

<p>Shoda QIDO-RS je víc než porovnání řetězců: zástupné znaky, intervaly dat a časů, seznamy UID, fuzzy jména osob, prázdné hodnoty a klíče uvnitř sekvencí. Poskytovatel to neimplementuje. Vrací kandidáty ve stabilním pořadí a může vrátit více než odpovídá. Knihovna aplikuje přesnou DICOM shodu na každého kandidáta, zachovává požadovaná pole a aplikuje <code>offset</code> a <code>limit</code>. Jedna odpověď obsahuje ve výchozím nastavení nejvýše 1000 výsledků a <code>DicomWebStandardServiceOptions</code> tento limit mění.</p>

<p>Aby byly vyhledávání rychlá v rozsáhlém archivu, předejte selektivní klíče databázi. <code>search.Plan.Filter</code> obsahuje zkompilované klíče vyhledávání, s atributem, typem shody a hodnotami každého:</p>

<div class="codeblock" id="code">
 <h3>Načíst přesné Patient ID z vyhledávání - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Databázový dotaz vytvořený tímto způsobem může vrátit nadbytečné řádky, například když ignoruje klíč, který nedokáže přeložit. Konečná shoda v knihovně je odstraňuje. Nesmí ztratit skutečné shody, protože knihovna je nemůže zpětně přidat.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Uložení: výsledek pro každou instanci">}}

<p>Požadavek STOW-RS může nést stovky instancí. Knihovna parsuje multipart tělo a volá <code>StoreAsync</code> pro každou instanci postupně, takže celý upload není najednou držen v paměti. Pro každou instanci poskytovatel vrátí <code>Stored</code>, <code>Warning</code> pro instanci uloženou se změnami, nebo <code>Failed</code> s volitelným DICOM kódem důvodu.</p>

<p>Knihovna sestaví zprávu STOW-RS z těchto výsledků a vybere stav: 200 když je každá instance uložena, 202 když některé mají varování nebo selhaly, 409 když všechny selhaly. Instance v požadavku pro jednu pojmenovanou studii, které patří do jiné studie, jsou odmítnuty před voláním poskytovatele.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Načítání: přenosové syntaxe, snímky a renderované obrázky">}}

<p>Poskytovatel vrací uložené soubory a knihovna je doručuje ve formě, o kterou klient požádal. WADO-RS klient může požadovat přenosovou syntaxi v hlavičce <code>Accept</code> a knihovna každou soubor před odesláním převede, včetně vlastních kodeků, HTJ2K a JPEG XL. Metadata v DICOM JSON nebo XML, hromadná data a jednotlivé snímky pocházejí od stejného poskytovatele, bez dalšího kódu.</p>

<p>Renderované obrázky potřebují další třídu, protože jen vy rozhodujete, které obrázky server může vytvořit. Tato nabízí JPEG pro renderované instance, renderované snímky a náhledy, a knihovna je vykresluje:</p>

<div class="codeblock" id="code">
 <h3>Nabídnout renderované JPEG obrázky a náhledy - C#</h3>
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

<p>Vestavěný renderér vytváří JPEG z jednosnímkové instance nebo ze snímků, které požadavek vybere. Renderovaná multi-snímková instance jako celek, jiné formáty obrázků a parametry viewportu, okna a ICC profilu vyžadují váš vlastní <code>RetrieveAsync</code> v této třídě.</p>

<p>Náhledy jsou zpracovávány s ohledem na soukromí pacienta. Vestavěný renderér vytvoří náhled pouze z instance, jejíž atributy Burned In Annotation a Recognizable Visual Features jsou obě <code>NO</code>, což znamená, že obrázek sám deklaruje, že neobsahuje údaje o pacientovi. Mnoho obrázků tyto atributy nemá, a pro ně server vrací 406. Pokud váš archiv potřebuje náhledy takových obrázků, vytvořte je ve svém vlastním <code>RetrieveAsync</code> a zajistěte, aby skrývaly jakékoli údaje o pacientovi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pracovní seznamy, kroky procedur a další služby">}}

<p>Další DICOMweb služby následují stejný model: zaregistrovat službu, implementovat její rozhraní úložiště.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> pracovní seznamy s <code>AddStandardWorklistService</code>: vytváření, vyhledávání a aktualizace pracovních položek a změna jejich stavu, přičemž pravidla stavu kontroluje knihovna a notifikace událostí jsou odesílány přes WebSocket.</li>
<li><a href="/medical/net/modality-worklist/">Modality worklist</a> (MWL-RS) s <code>AddStandardModalityWorklistService</code>, aby modality mohly dotazovat naplánované kroky procedur vašeho RIS.</li>
<li>Modality performed procedure steps (MPPS-RS), závazek úložiště a ne-pacientské instance jako barevné palety a hanging protokoly.</li>
<li>WADO-URI načítání pro starší prohlížeče, poskytované službou Studies.</li>
</ul>

<p>Když standardní chování jedné transakce nevyhovuje, zaregistrujte si vlastní obslužnou rutinu pro ni pomocí <code>AddHandler</code>. Knihovna i nadále směruje a kontroluje požadavek a ostatní transakce si zachovají své standardní obslužné rutiny.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bezpečnost a hostování zůstávají ve vašich rukou">}}

<p>Koncové body DICOMweb jsou běžné ASP.NET Core koncové body. Autentizace, autorizace, CORS, TLS, přeposlané hlavičky a limity velikosti požadavků jsou konfigurovány stejným způsobem jako pro zbytek vaší služby:</p>

<div class="codeblock" id="code">
 <h3>Vyžadovat ověřeného uživatele - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Protokolová část nezávisí na ASP.NET Core. <code>DicomWebApplication</code> převádí <code>DicomWebRequest</code> na odpověď, takže stejné služby mohou běžet za jiným HTTP serverem nebo v testu bez sítě:</p>

<div class="codeblock" id="code">
 <h3>Stejný server bez ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Klient a server z jedné knihovny">}}

<p><a href="/medical/net/dicomweb-client/">Klient DICOMweb</a> v Aspose.Medical testuje váš server z jednotkového testu nebo konzolového programu a každý soubor, který server přijme, je běžný <code>DicomFile</code>, který lze anonymizovat, transkódovat nebo renderovat ve stejném procesu.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">Průvodce vývojem DICOMweb</a> pokrývá každou službu a rozhraní úložiště a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">průvodce ASP.NET Core</a> pokrývá mapování koncových bodů a WebSockets.</p>

<p>Související stránky: <a href="/medical/net/dicom-networking/">DICOM networking</a> pro systémy, které odesílají studie přes DIMSE místo HTTP, <a href="/medical/net/anonymization/">anonymizace</a> příchozích studií, <a href="/medical/net/dicom-transfer-syntax-conversion/">převod přenosových syntaxí</a> a <a href="/medical/net/dicom-to-json/">DICOM do JSON</a> pro formát výsledků QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Vzdělávací zdroje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Průvodce vývojáře" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="Reference API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Podpora produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Bezplatná podpora" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Placená podpora" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Proč Aspose.Medical pro .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Seznam zákazníků" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Úspěšné příběhy" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
