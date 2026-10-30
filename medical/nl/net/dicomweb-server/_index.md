---
title: DICOMweb-server in C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Bouw uw eigen DICOMweb-server of PACS in C#. Aspose.Medical implementeert QIDO-RS, WADO-RS, STOW-RS en de worklist-services op ASP.NET Core; u sluit de opslag aan.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb-server voor .NET C#" h2="Voeg QIDO-RS, WADO-RS en STOW-RS eindpunten toe aan een ASP.NET Core-service. De bibliotheek spreekt het protocol; uw code bepaalt waar de studies worden opgeslagen." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Uw eigen DICOMweb-server in C#">}}

<p>Een PACS, een VNA, een cloudarchief of een onderzoeksplatform moet DICOMweb‑verzoeken beantwoorden: QIDO-RS‑zoekopdrachten, WADO-RS‑downloads en STOW-RS‑uploads, en vaak ook de worklist‑services. Elk van hen heeft zijn eigen routes, mediatypen, multipart‑lichamen, overeenkomstregels en statuscodes.</p>

<p><strong>Aspose.Medical voor .NET</strong> implementeert die protocolkant sinds versie 26.10. Het leest elk verzoek, controleert het volgens de norm, matcht zoek‑sleutels volgens de DICOM‑regels, pagineert de resultaten, zet transfer‑syntaxis om en schrijft de respons. Uw code doet wat alleen u kunt: een instantie opslaan, kandidaten voor een zoekopdracht vinden en de opgeslagen bestanden retourneren. Viewers, modaliteiten en andere archieven werken vervolgens met uw server zoals met elke andere DICOMweb‑server, en hetzelfde geldt voor de <a href="/medical/net/dicomweb-client/">DICOMweb-client</a> van dezelfde bibliotheek.</p>

<p>De server wordt beheerd in C#. Hij draait waar ASP.NET Core ook draait: op Windows, op Linux en in een container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Een volledige server in ASP.NET Core">}}

<p>Installeer het <code>Aspose.Medical.Dicom.Web.AspNetCore</code>-pakket. Het brengt Aspose.Medical mee en koppelt de DICOMweb‑services aan dependency injection en endpoint‑routing. Deze <code>Program.cs</code> is een werkende Studies‑server; de twee klassen die hij registreert worden hieronder getoond.</p>

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

<p>Elke Studies‑route beantwoordt nu onder <code>/dicomweb</code>: <code>/dicomweb/studies</code> voor zoeken en uploaden, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> voor ophalen, en de routes voor metadata, frames, bulkdata en gerenderde afbeeldingen. Het capaciteiten‑document bij de service‑root somt precies de transacties op die de server ondersteunt.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sluit uw opslag aan">}}

<p>De Studies‑service vereist één interface met drie methoden: een instantie opslaan, de opgeslagen bestanden retourneren en zoek‑kandidaten retourneren. Deze implementatie bewaart elke instantie als een bestand in een map per studie en serie.</p>

<div class="codeblock" id="code">
 <h3>Een map‑archief achter QIDO-RS, WADO-RS en STOW-RS - C#</h3>
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

<p>De bibliotheek controleert de UID’s in elk aanvraagpad en in elke geüploade instantie voordat zij de provider aanroept. Een instantie zonder geldige UID’s wordt gemeld als mislukt en bereikt nooit <code>StoreAsync</code>.</p>

<p>Een map volstaat om de server te testen. Een productie‑archief behoudt dezelfde drie methoden en plaatst een object‑store achter <code>StoreAsync</code> en <code>RetrieveAsync</code> en een database‑index achter <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zoeken: de bibliotheek past de DICOM‑overeenstemmingsregels toe">}}

<p>QIDO-RS‑matching is meer dan tekenreeks‑vergelijking: wildcards, datum‑ en tijdsintervallen, lijsten van UID’s, fuzzy persoonsnamen, lege waarden en sleutels binnen sequensen. De provider implementeert geen van deze functionaliteit. Hij retourneert kandidaten in een stabiele volgorde, en kan meer retourneren dan overeenkomt. De bibliotheek past de exacte DICOM‑match toe op elke kandidaat, behoudt de gevraagde velden, en past <code>offset</code> en <code>limit</code> toe. Eén respons bevat standaard maximaal 1000 resultaten, en <code>DicomWebStandardServiceOptions</code> verandert die limiet.</p>

<p>Om zoekopdrachten snel te houden op een groot archief, geef de selectieve sleutels door aan de database. <code>search.Plan.Filter</code> bevat de gecompileerde zoek‑sleutels, met het attribuut, het type match en de waarden van elk:</p>

<div class="codeblock" id="code">
 <h3>Lees een exacte Patient ID uit een zoekopdracht - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Een database‑query die op deze manier is opgebouwd kan extra rijen retourneren, bijvoorbeeld wanneer hij een sleutel negeert die hij niet kan vertalen. De definitieve match in de bibliotheek verwijdert deze. Het mag geen echte matches weggooien, omdat de bibliotheek ze niet kan terug toevoegen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opslaan: een resultaat voor elke instantie">}}

<p>Een STOW-RS‑verzoek kan honderden instanties bevatten. De bibliotheek parseert het multipart‑lichaam en roept <code>StoreAsync</code> aan voor elke instantie op zijn beurt, zodat de volledige upload nooit in één keer in het geheugen wordt gehouden. Voor elke instantie retourneert de provider <code>Stored</code>, <code>Warning</code> voor een instantie die met wijzigingen is opgeslagen, of <code>Failed</code> met een optionele DICOM‑reden‑code.</p>

<p>De bibliotheek bouwt het STOW-RS‑rapport op basis van deze resultaten en selecteert de status: 200 wanneer elke instantie is opgeslagen, 202 wanneer sommige waarschuwingen hebben of mislukt zijn, 409 wanneer alle mislukt zijn. Instanties in een verzoek voor één genoemde studie die tot een andere studie behoren, worden geweigerd voordat de provider wordt aangeroepen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ophalen: transfer‑syntaxis, frames en gerenderde afbeeldingen">}}

<p>De provider retourneert opgeslagen bestanden, en de bibliotheek levert ze in de vorm die de client vraagt. Een WADO-RS‑client kan een transfer‑syntax opvragen in de <code>Accept</code>-header, en de bibliotheek zet elk bestand om voordat het wordt verzonden, met eigen codecs, inclusief HTJ2K en JPEG XL. Metadata in DICOM JSON of XML, bulk‑data en enkel‑frames komen van dezelfde provider, zonder extra code.</p>

<p>Gerenderde afbeeldingen hebben nog een klasse nodig, omdat alleen u beslist welke afbeeldingen de server mag produceren. Deze biedt JPEG voor gerenderde instanties, gerenderde frames en thumbnails, en de bibliotheek rendert ze:</p>

<div class="codeblock" id="code">
 <h3>Bied gerenderde JPEG‑afbeeldingen en thumbnails aan - C#</h3>
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

<p>De ingebouwde renderer produceert JPEG van een single‑frame‑instantie of van de frames die een verzoek selecteert. Een gerenderde multi‑frame‑instantie als geheel, een afbeelding breder of hoger dan 65500 pixels (thumbnails overslaan dergelijke afbeeldingen), andere afbeeldingstypen, en de viewport-, window- en ICC‑profiel‑parameters hebben uw eigen <code>RetrieveAsync</code> in deze klasse nodig.</p>

<p>Thumbnails worden zorgvuldig behandeld met het oog op patiëntprivacy. De ingebouwde renderer maakt een thumbnail alleen van een instantie waarvan de attributen Burned In Annotation en Recognizable Visual Features beide <code>NO</code> zijn, wat betekent dat de afbeelding zelf verklaart geen patiëntgegevens weer te geven. Veel afbeeldingen hebben deze attributen niet, en voor hen geeft de server 406 terug. Als uw archief thumbnails van dergelijke afbeeldingen nodig heeft, produceer ze dan in uw eigen <code>RetrieveAsync</code> en zorg ervoor dat ze alle patiëntgegevens verbergen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklists, procedure‑stappen en de andere services">}}

<p>De andere DICOMweb‑services volgen hetzelfde model: registreer de service, implementeer de opslag‑interface.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a>-worklists met <code>AddStandardWorklistService</code>: maak, zoek en werk workitems bij en wijzig hun status, waarbij de statusregels door de bibliotheek worden gecontroleerd en gebeurtenis‑meldingen via een WebSocket worden verzonden.</li>
<li>De <a href="/medical/net/modality-worklist/">modality worklist</a> (MWL‑RS) met <code>AddStandardModalityWorklistService</code>, zodat modaliteiten de geplande procedure‑stappen van uw RIS kunnen opvragen.</li>
<li>Modality performed procedure steps (MPPS‑RS), storage commitment en niet‑patiënt‑instanties zoals kleurpaletten en hanging‑protocols.</li>
<li>WADO-URI‑ophalen voor oudere viewers, geleverd door de Studies‑service.</li>
</ul>

<p>Wanneer het standaardgedrag van één transactie niet past, registreer uw eigen handler hiervoor met <code>AddHandler</code>. De bibliotheek routeert en controleert nog steeds het verzoek, en de andere transacties behouden hun standaardhandlers.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Beveiliging en hosting blijven in uw handen">}}

<p>De DICOMweb‑eindpunten zijn gewone ASP.NET Core‑eindpunten. Authenticatie, autorisatie, CORS, TLS, doorgestuurde headers en limieten voor verzoekgrootte worden op dezelfde manier geconfigureerd als voor de rest van uw service:</p>

<div class="codeblock" id="code">
 <h3>Vereis een geauthenticeerde gebruiker - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Het protocolgedeelte hangt niet af van ASP.NET Core. <code>DicomWebApplication</code> zet een <code>DicomWebRequest</code> om in een respons, zodat dezelfde services achter een andere HTTP‑server kunnen draaien of in een test zonder netwerk:</p>

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

<p>De <a href="/medical/net/dicomweb-client/">DICOMweb-client</a> van Aspose.Medical test uw server vanuit een unit‑test of een console‑programma, en elk bestand dat de server ontvangt is een regulier <code>DicomFile</code> dat geanonimiseerd, getranscodeerd of gerenderd kan worden in hetzelfde proces.</p>

<p>De <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb-ontwikkelaarsgids</a> behandelt elke service en opslag‑interface, en de <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core‑gids</a> behandelt endpoint‑mapping en WebSockets.</p>

<p>Gerelateerde pagina's: <a href="/medical/net/dicom-networking/">DICOM‑netwerken</a> voor systemen die studies verzenden via DIMSE in plaats van HTTP, <a href="/medical/net/anonymization/">anonimisering</a> van binnenkomende studies, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer‑syntax‑conversie</a>, en <a href="/medical/net/dicom-to-json/">DICOM naar JSON</a> voor het formaat dat QIDO-RS‑resultaten gebruiken.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leerbronnen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Ontwikkelaarsgids" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="API‑referenties" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Productondersteuning" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis ondersteuning" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betaalde ondersteuning" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Waarom Aspose.Medical voor .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Klantenlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
