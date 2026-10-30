---
title: DICOMweb Server i C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Bygg din egen DICOMweb-server eller PACS i C#. Aspose.Medical implementerar QIDO-RS, WADO-RS, STOW-RS och worklist-tjänsterna på ASP.NET Core; du ansluter lagringen.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb Server för .NET C#" h2="Lägg till QIDO-RS-, WADO-RS- och STOW-RS-endpunkter i en ASP.NET Core-tjänst. Biblioteket talar protokollet; din kod bestämmer var studierna lagras." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Din egen DICOMweb-server i C#">}}

<p>Ett PACS, ett VNA, ett molnarkiv eller en forskningsplattform måste svara på DICOMweb-förfrågningar: QIDO-RS-sökningar, WADO-RS-nedladdningar och STOW-RS-uppladdningar, och ofta även worklist-tjänsterna. Var och en av dem har sina egna rutter, mediatyper, multipart‑kroppar, matchningsregler och statuskoder.</p>

<p><strong>Aspose.Medical för .NET</strong> implementerar den protocollsidan sedan version 26.10. Den läser varje förfrågan, kontrollerar den mot standarden, matchar söknycklar enligt DICOM-reglerna, paginerar resultaten, konverterar överföringssyntaxer och skriver svaret. Din kod gör det som bara du kan: lagra en instans, hitta kandidater för en sökning och returnera de lagrade filerna. Visare, modaliteter och andra arkiv arbetar sedan med din server som med vilken annan DICOMweb-server som helst, och det gör även <a href="/medical/net/dicomweb-client/">DICOMweb-klienten</a> i samma bibliotek.</p>

<p>Servern är skriven i C#. Den körs där ASP.NET Core körs: på Windows, på Linux och i en container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="En komplett server i ASP.NET Core">}}

<p>Installera paketet <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Det medför Aspose.Medical och ansluter DICOMweb-tjänsterna till dependency injection och endpoint‑routing. Denna <code>Program.cs</code> är en fungerande Studies‑server; de två klasser den registrerar visas nedan.</p>

<div class="codeblock" id="code">
 <h3>Program.cs för en DICOMweb-server - C#</h3>
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

<p>Varje Studies‑ruta svarar nu under <code>/dicomweb</code>: <code>/dicomweb/studies</code> för sökning och uppladdning, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> för hämtning, samt rutorna för metadata, ramar, bulkdata och renderade bilder. Kapacitetsdokumentet vid service‑roten listar exakt de transaktioner som servern stödjer.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anslut din lagring">}}

<p>Studies‑tjänsten kräver ett gränssnitt med tre metoder: lagra en instans, returnera de lagrade filerna och returnera sökkandidater. Denna implementation behåller varje instans som en fil i en mapp per studie och serie.</p>

<div class="codeblock" id="code">
 <h3>Ett mapparkiv bakom QIDO-RS, WADO-RS och STOW-RS - C#</h3>
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

<p>Biblioteket kontrollerar UIDs i varje förfrågningsväg och i varje uppladdad instans innan det anropar leverantören. En instans utan giltiga UIDs rapporteras som misslyckad och når aldrig <code>StoreAsync</code>.</p>

<p>En mapp räcker för att prova servern. Ett produktionsarkiv behåller samma tre metoder och placerar ett objektslagringssystem bakom <code>StoreAsync</code> och <code>RetrieveAsync</code> samt ett databasindex bakom <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sökning: biblioteket tillämpar DICOM-matchningsreglerna">}}

<p>QIDO-RS‑matchning är mer än strängjämförelse: jokertecken, datum- och tidsintervall, listor med UIDs, oskarpa personnamn, tomma värden och nycklar i sekvenser. Leverantören implementerar inte någon av detta. Den returnerar kandidater i en stabil ordning och kan returnera mer än en matchning. Biblioteket tillämpar exakt DICOM‑matchning på varje kandidat, behåller de begärda fälten och tillämpar <code>offset</code> och <code>limit</code>. Ett svar innehåller som standard högst 1000 resultat, och <code>DicomWebStandardServiceOptions</code> ändrar den gränsen.</p>

<p>För att hålla sökningar snabba i ett stort arkiv, skicka de selektiva nycklarna till databasen. <code>search.Plan.Filter</code> innehåller de kompilerade söknycklarna, med attributet, matchningstypen och värdena för varje:</p>

<div class="codeblock" id="code">
 <h3>Läs ett exakt Patient-ID från en sökning - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>En databasfråga byggd på detta sätt kan returnera extra rader, till exempel när den ignorerar en nyckel den inte kan översätta. Slutmatchen i biblioteket tar bort dem. Den får inte släppa verkliga matchningar, eftersom biblioteket inte kan lägga till dem igen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lagra: ett resultat för varje instans">}}

<p>En STOW-RS‑förfrågan kan innehålla hundratals instanser. Biblioteket parsar multipart‑kroppen och anropar <code>StoreAsync</code> för varje instans i tur och ordning, så hela uppladdningen hålls aldrig i minnet samtidigt. För varje instans returnerar leverantören <code>Stored</code>, <code>Warning</code> för en instans lagrad med ändringar, eller <code>Failed</code> med en valfri DICOM‑felkod.</p>

<p>Biblioteket bygger STOW-RS‑rapporten från dessa resultat och väljer status: 200 när varje instans är lagrad, 202 när vissa har varningar eller misslyckats, 409 när alla misslyckats. Instanser i en förfrågan för en namngiven studie som tillhör en annan studie avvisas innan leverantören anropas.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Hämta: överföringssyntaxer, ramar och renderade bilder">}}

<p>Leverantören returnerar lagrade filer, och biblioteket levererar dem i det format som klienten begär. En WADO-RS‑klient kan begära en överföringssyntax i <code>Accept</code>-headern, och biblioteket konverterar varje fil innan den skickas, med egna codecs, inklusive HTJ2K och JPEG XL. Metadata i DICOM JSON eller XML, bulkdata och enskilda ramar kommer från samma leverantör, utan extra kod.</p>

<p>Renderade bilder kräver en extra klass, eftersom endast du bestämmer vilka bilder servern får producera. Denna erbjuder JPEG för renderade instanser, renderade ramar och miniatyrbilder, och biblioteket renderar dem:</p>

<div class="codeblock" id="code">
 <h3>Erbjud renderade JPEG‑bilder och miniatyrer - C#</h3>
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

<p>Den inbyggda renderaren producerar JPEG från en enkelramig instans eller från de ramar som en förfrågan väljer. En renderad multi‑frame‑instans som helhet, en bild bredare eller högre än 65500 pixlar (miniatyrer passerar över sådana bilder), andra bildformat och parametrarna för viewport, fönster och ICC‑profil kräver din egen <code>RetrieveAsync</code> i denna klass.</p>

<p>Miniatyrer hanteras med omsorg för patientsekretessen. Den inbyggda renderaren skapar en miniatyr endast från en instans vars attribut Burned In Annotation och Recognizable Visual Features båda är <code>NO</code>, vilket betyder att bilden själv deklarerar att den inte visar patientdata. Många bilder saknar dessa attribut, och för dem svarar servern med 406. Om ditt arkiv behöver miniatyrer av sådana bilder, producera dem i din egen <code>RetrieveAsync</code> och säkerställ att de döljer eventuell patientdata.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Arbetslistor, procedursteg och de andra tjänsterna">}}

<p>De andra DICOMweb‑tjänsterna följer samma modell: registrera tjänsten, implementera dess lagringsgränssnitt.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a>-arbetslistor med <code>AddStandardWorklistService</code>: skapa, söka och uppdatera arbetsobjekt och ändra deras tillstånd, med tillståndsregler kontrollerade av biblioteket och händelsenotifikationer skickade via en WebSocket.</li>
<li><a href="/medical/net/modality-worklist/">Modality worklist</a> (MWL-RS) med <code>AddStandardModalityWorklistService</code>, så modaliteter kan fråga de schemalagda procedurstegen i ditt RIS.</li>
<li>Modality performed procedure steps (MPPS-RS), lagringsåtagande och icke‑patientinstanser såsom färgpaletter och hanging‑protokoll.</li>
<li>WADO-URI‑hämtning för äldre visare, levererad av Studies‑tjänsten.</li>
</ul>

<p>När standardbeteendet för en transaktion inte passar, registrera din egen hanterare för den med <code>AddHandler</code>. Biblioteket routar och kontrollerar fortfarande förfrågan, och de andra transaktionerna behåller sina standardhanterare.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Säkerhet och hosting ligger i dina händer">}}

<p>DICOMweb‑endpunkterna är vanliga ASP.NET Core‑endpunkter. Autentisering, auktorisation, CORS, TLS, vidarebefordrade headers och gränser för begärans storlek konfigureras på samma sätt som för resten av din tjänst:</p>

<div class="codeblock" id="code">
 <h3>Kräv en autentiserad användare - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Protokolldelen är oberoende av ASP.NET Core. <code>DicomWebApplication</code> omvandlar en <code>DicomWebRequest</code> till ett svar, så samma tjänster kan köras bakom en annan HTTP‑server eller i ett test utan nätverk:</p>

<div class="codeblock" id="code">
 <h3>Samma server utan ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Klient och server från samma bibliotek">}}

<p><a href="/medical/net/dicomweb-client/">DICOMweb-klienten</a> i Aspose.Medical testar din server från ett enhetstest eller ett konsolprogram, och varje fil som servern tar emot är en vanlig <code>DicomFile</code> som kan anonymiseras, transkodas eller renderas i samma process.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb‑utvecklarguiden</a> täcker alla tjänster och lagringsgränssnitt, och <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core‑guiden</a> täcker endpoint‑mappning och WebSockets.</p>

<p>Relaterade sidor: <a href="/medical/net/dicom-networking/">DICOM‑nätverk</a> för system som skickar studier via DIMSE istället för HTTP, <a href="/medical/net/anonymization/">anonymisering</a> av inkommande studier, <a href="/medical/net/dicom-transfer-syntax-conversion/">överföringssyntaxkonvertering</a> och <a href="/medical/net/dicom-to-json/">DICOM till JSON</a> för formatet som QIDO-RS‑resultat använder.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Utvecklarguide" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="API‑referenser" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktstöd" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betald support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blogg" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Varför Aspose.Medical för .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundlista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Framgångshistorier" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
