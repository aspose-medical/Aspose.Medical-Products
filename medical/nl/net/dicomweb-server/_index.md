---
title: DICOMweb Server in C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Bouw uw eigen DICOMweb-server of PACS in C#. Aspose.Medical implementeert QIDO-RS, WADO-RS, STOW-RS en de worklist‑services op ASP.NET Core; u verbindt de opslag.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb Server voor .NET C#" h2="Voeg QIDO-RS-, WADO-RS- en STOW-RS‑eindpunten toe aan een ASP.NET Core‑service. De bibliotheek spreekt het protocol; uw code beslist waar de studies worden opgeslagen." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Uw eigen DICOMweb‑server in C#">}}

<p>Een PACS, een VNA, een cloudarchief of een onderzoeksplatform moet DICOMweb‑verzoeken afhandelen: QIDO‑RS‑searches, WADO‑RS‑downloads en STOW‑RS‑uploads, en vaak ook de worklist‑services. Elk van hen heeft eigen routes, mediatypes, multipart‑bodies, matching‑regels en statuscodes.</p>

<p><strong>Aspose.Medical for .NET</strong> implementeert dat protocol sinds versie 26.10. Het leest elk verzoek, controleert het tegen de standaard, matcht zoek‑sleutels volgens de DICOM‑regels, pagina‑t de resultaten, converteert transfer‑syntaxis en schrijft de respons. Uw code doet wat alleen u kunt: een instantie opslaan, kandidaten voor een zoekopdracht vinden en de opgeslagen bestanden retourneren. Viewers, modaliteiten en andere archieven werken vervolgens met uw server net als met elke andere DICOMweb‑server, en hetzelfde geldt voor de <a href="/medical/net/dicomweb-client/">DICOMweb‑client</a> van dezelfde bibliotheek.</p>

<p>De server is beheerd in C#. Hij draait waar ASP.NET Core ook draait: op Windows, op Linux en in een container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Een volledige server in ASP.NET Core">}}

<p>Installeer het <code>Aspose.Medical.Dicom.Web.AspNetCore</code>-pakket. Het brengt Aspose.Medical mee en koppelt de DICOMweb‑services aan dependency injection en endpoint‑routing. Deze <code>Program.cs</code> is een werkende Studies‑server; de twee klassen die het registreert staan hieronder weergegeven.</p>

<div class="codeblock" id="code">
 <h3>Program.cs van een DICOMweb‑server - C#</h3>
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

<p>Elke Studies‑route antwoordt nu onder <code>/dicomweb</code>: <code>/dicomweb/studies</code> voor zoeken en uploaden, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> voor ophalen, en de routes voor metadata, frames, bulk‑data en gerenderde beelden. Het capabilities‑document op de service‑root geeft precies de transacties weer die de server ondersteunt.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Verbind uw opslag">}}

<p>De Studies‑service vereist één interface met drie methoden: een instantie opslaan, de opgeslagen bestanden retourneren en zoek‑kandidaten retourneren. Deze implementatie bewaart elke instantie als een bestand in een map per studie en serie.</p>

<div class="codeblock" id="code">
 <h3>Een map‑archief achter QIDO‑RS, WADO‑RS en STOW‑RS - C#</h3>
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

<p>De bibliotheek controleert de UID's in elk request‑pad en in elke geüploade instantie voordat zij de provider aanroept. Een instantie zonder geldige UID's wordt gemeld als mislukt en bereikt nooit <code>StoreAsync</code>.</p>

<p>Een map is voldoende om de server uit te proberen. Een productie‑archief behoudt dezelfde drie methoden en plaatst een objectstore achter <code>StoreAsync</code> en <code>RetrieveAsync</code> en een database‑index achter <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zoeken: de bibliotheek past de DICOM‑matching‑regels toe">}}

<p>QIDO‑RS‑matching is meer dan tekenreeks‑vergelijking: wildcards, datum‑ en tijds‑bereiken, lijsten van UID's, fuzzy persoonsnamen, lege waarden en sleutels binnen sequenties. De provider implementeert geen van deze. Hij retourneert kandidaten in een stabiele volgorde, en kan meer retourneren dan overeenkomt. De bibliotheek past de exacte DICOM‑match toe op elke kandidaat, behoudt de gevraagde velden, en past <code>offset</code> en <code>limit</code> toe. Eén respons bevat standaard maximaal 1000 resultaten, en <code>DicomWebStandardServiceOptions</code> wijzigt die limiet.</p>

<p>Om zoekopdrachten snel te houden op een groot archief, geeft u de selectieve sleutels door aan de database. <code>search.Plan.Filter</code> bevat de gecompileerde zoek‑sleutels, met het attribuut, het type match en de waarden van elk:</p>

<div class="codeblock" id="code">
 <h3>Lees een exacte Patient ID uit een zoekopdracht - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Een database‑query opgebouwd op deze manier kan extra rijen retourneren, bijvoorbeeld wanneer een sleutel die niet vertaald kan worden wordt genegeerd. De uiteindelijke match in de bibliotheek verwijdert deze. Het mag geen ware matches verwijderen, omdat de bibliotheek ze niet kan terugvoegen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opslaan: een resultaat voor elke instantie">}}

<p>Een STOW‑RS‑verzoek kan honderden instanties bevatten. De bibliotheek parseert de multipart‑body en roept <code>StoreAsync</code> aan voor elke instantie op beurt, zodat de volledige upload nooit in één keer in het geheugen duurt. Voor elke instantie retourneert de provider <code>Stored</code>, <code>Warning</code> voor een instantie die met wijzigingen is opgeslagen, of <code>Failed</code> met een optionele DICOM‑redencode.</p>

<p>De bibliotheek bouwt het STOW‑RS‑rapport op uit deze resultaten en selecteert de status: 200 wanneer elke instantie is opgeslagen, 202 wanneer sommige waarschuwingen of fouten hebben, 409 wanneer alle mislukt zijn. Instanties in een verzoek voor één genoemde studie die tot een andere studie behoren, worden geweigerd voordat de provider wordt aangeroepen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ophalen: transfer‑syntaxis, frames en gerenderde beelden">}}

<p>De provider retourneert opgeslagen bestanden, en de bibliotheek levert ze in de vorm die de client heeft aangevraagd. Een WADO‑RS‑client kan een transfer‑syntax opvragen in de <code>Accept</code>-header, en de bibliotheek converteert elk bestand voordat het wordt verzonden, met eigen codecs, HTJ2K en JPEG XL inbegrepen. Metadata in DICOM JSON of XML, bulk‑data en enkele frames komen van dezelfde provider, zonder extra code.</p>

<p>Gerenderde beelden hebben een extra klasse nodig, omdat alleen u beslist welke beelden de server mag produceren. Deze biedt JPEG voor gerenderde instanties, gerenderde frames en thumbnails, en de bibliotheek rendert ze:</p>

<div class="codeblock" id="code">
 <h3>Lever gerenderde JPEG‑beelden en thumbnails - C#</h3>
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

<p>De ingebouwde renderer produceert JPEG van een single‑frame‑instantie of van de frames die een verzoek selecteert. Een gerenderde multi‑frame‑instantie als geheel, andere beeldformaten, en de viewport-, window- en ICC‑profiel‑parameters vereisen uw eigen <code>RetrieveAsync</code> in deze klasse.</p>

<p>Thumbnails worden zorgvuldig behandeld vanwege patiëntprivacy. De ingebouwde renderer maakt een thumbnail alleen van een instantie waarvan de attributen Burned In Annotation en Recognizable Visual Features beide <code>NO</code> zijn, wat betekent dat het beeld zelf aangeeft geen patiëntgegevens te tonen. Veel beelden hebben deze attributen niet, en voor hen antwoordt de server met 406. Als uw archief thumbnails van dergelijke beelden nodig heeft, produceer ze dan in uw eigen <code>RetrieveAsync</code> en zorg ervoor dat ze alle patiëntgegevens verbergen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklists, procedurestappen en de andere services">}}

<p>De andere DICOMweb‑services volgen hetzelfde model: registreer de service, implementeer de opslag‑interface.</p>

<ul>
<li>UPS‑RS‑worklists met <code>AddStandardWorklistService</code>: maak, zoek en werk workitems bij en wijzig hun status, met de status‑regels gecontroleerd door de bibliotheek en gebeurtenis‑notificaties verzonden via een WebSocket.</li>
<li>De modality‑worklist (MWL‑RS) met <code>AddStandardModalityWorklistService</code>, zodat modaliteiten de geplande procedurestappen van uw RIS kunnen opvragen.</li>
<li>Modality performed procedure steps (MPPS‑RS), storage commitment en niet‑patiënt instanties zoals kleurschema's en hanging‑protocols.</li>
<li>WADO‑URI‑ophaling voor oudere viewers, aangeboden door de Studies‑service.</li>
</ul>

<p>Wanneer het standaardgedrag van een transactie niet past, registreer uw eigen handler daarvoor met <code>AddHandler</code>. De bibliotheek routeert en controleert nog steeds het verzoek, en de andere transacties behouden hun standaardhandlers.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Beveiliging en hosting blijven in uw handen">}}

<p>De DICOMweb‑eindpunten zijn gewone ASP.NET Core‑eindpunten. Authenticatie, autorisatie, CORS, TLS, doorgestuurde headers en limieten voor request‑grootte worden op dezelfde manier geconfigureerd als voor de rest van uw service:</p>

<div class="codeblock" id="code">
 <h3>Vereis een geauthenticeerde gebruiker - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Het protocolgedeelte hangt niet af van ASP.NET Core. <code>DicomWebApplication</code> zet een <code>DicomWebRequest</code> om in een respons, zodat dezelfde services kunnen draaien achter een andere HTTP‑server of in een test zonder netwerk:</p>

<div class="codeblock" id="code">
 <h3>Dezelfde server zonder ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Client en server uit één bibliotheek">}}

<p>De <a href="/medical/net/dicomweb-client/">DICOMweb‑client</a> van Aspose.Medical test uw server vanuit een unit‑test of een console‑programma, en elk bestand dat de server ontvangt is een reguliere <code>DicomFile</code> die geanonimiseerd, getranscodeerd of gerenderd kan worden in hetzelfde proces.</p>

<p>De <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb‑developer‑gids</a> behandelt elke service en opslag‑interface, en de <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core‑gids</a> behandelt endpoint‑mapping en WebSockets.</p>

<p>Gerelateerde pagina's: <a href="/medical/net/dicom-networking/">DICOM‑networking</a> voor systemen die studies versturen via DIMSE in plaats van HTTP, <a href="/medical/net/anonymization/">anonimisering</a> van binnenkomende studies, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer‑syntax‑conversie</a>, en <a href="/medical/net/dicom-to-json/">DICOM naar JSON</a> voor het formaat dat QIDO‑RS‑resultaten gebruiken.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leerbronnen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Developergids" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="API‑referenties" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Productondersteuning" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis ondersteuning" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betaalde ondersteuning" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Waarom Aspose.Medical voor .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Klantlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
