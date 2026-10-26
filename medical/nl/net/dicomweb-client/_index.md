---
title: DICOMweb-client in C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Zoek, download en upload DICOM-studies via HTTP vanuit .NET. Een DICOMweb-client in pure C# voor QIDO-RS, WADO-RS en STOW-RS die werkt met PACS, VNA en cloud‑beeldvormingsservices.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb-client voor .NET C#" h2="Zoek, download en upload studies op elke DICOMweb-server met QIDO-RS, WADO-RS en STOW-RS, in beheerde C# zonder iets te installeren op de machine." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM via HTTP, vanuit uw eigen code">}}

<p>DICOMweb is de HTTP‑variant van DICOM. In plaats van een DIMSE‑associatie op poort 104 stuurt uw applicatie gewone HTTPS‑verzoeken: QIDO-RS om te zoeken, WADO-RS om te downloaden, STOW-RS om te uploaden. Cloud‑beeldvormingsdiensten zoals de Azure Health Data Services DICOM‑service en de Google Cloud Healthcare API werken op deze manier, evenals archieven zoals Orthanc en dcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> bevat een DICOMweb-client sinds versie 26.10. <code>DicomWebClient</code> verpakt elke standaardtransactie in een getypeerde methode, en de resultaten komen terug als dezelfde <code>Dataset</code>- en <code>DicomFile</code>-objecten die de rest van de bibliotheek leest en schrijft. De server aan de andere kant kan elke DICOMweb-implementatie zijn; hij hoeft niet met Aspose.Medical gebouwd te zijn.</p>

<p>De client is beheerde C# bovenop <code>HttpClient</code>. Er is geen native toolkit en geen proxy‑service, zodat dezelfde code werkt op Windows, Linux en in een container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Verbinden met een DICOMweb‑server">}}

<p>Maak de client aan vanuit een <code>HttpClient</code> en de service‑root‑URL van de server. Authenticatie, TLS‑instellingen, proxy’s en connection‑pooling blijven op de <code>HttpClient</code>, waar uw applicatie ze al configureert. De client bezit de <code>HttpClient</code> niet en vernietigt hem niet.</p>

<div class="codeblock" id="code">
 <h3>Maak een DICOMweb-client - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Als één implementatie WADO-URI of een andere service vanaf een andere URL bedient, overschrijft <code>ServiceBaseUris</code> de root alleen voor die service.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zoek studies met QIDO-RS">}}

<p>Een zoekopdracht neemt overeenkomende parameters met het DICOM‑sleutelwoord als naam, de terug te geven velden en paging‑besturingen. De overeenkomsten komen binnen als een stroom van datasets, één voor één gelezen terwijl de respons nog wordt gedownload.</p>

<div class="codeblock" id="code">
 <h3>Vind de studies van een patiënt - C#</h3>
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

<p><code>SearchSeriesAsync</code> en <code>SearchInstancesAsync</code> zoeken één niveau lager, over het hele archief of binnen een specifieke study of serie. Dezelfde opties stellen ook fuzzy‑matching, matching van lege waarden en de result‑offset voor paging in.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Download een study met WADO-RS">}}

<p>Een study kan duizenden instanties bevatten. <code>RetrieveStudyAsync</code> retourneert ze als een stroom, zodat elk bestand wordt geparseerd zodra het deel van de respons arriveert en kan worden opgeslagen voordat het volgende wordt gelezen. Het geheugenverbruik groeit niet mee met de grootte van de study.</p>

<div class="codeblock" id="code">
 <h3>Sla elke instantie van een study op - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> en <code>RetrieveInstanceAsync</code> downloaden één serie of één instantie op dezelfde manier.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Haal een thumbnail of een gerenderde afbeelding op">}}

<p>Een viewer of een werklijst‑scherm heeft vaak een afbeelding nodig, niet een DICOM‑bestand. De server rendert deze en stuurt een JPEG of PNG, zodat de client de originele pixeldata niet downloadt of decodeert.</p>

<div class="codeblock" id="code">
 <h3>Sla een JPEG‑thumbnail van een instantie op - C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> en <code>RetrieveRenderedFramesAsync</code> retourneren in plaats daarvan een afbeelding in volledige grootte. <code>DicomWebRenderOptions</code> geeft de standaard weergave‑controles door aan de server: beeldkwaliteit, viewport‑grootte, window en ICC‑profiel.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Upload instanties met STOW-RS">}}

<p>STOW-RS stuurt een willekeurig aantal instanties in één verzoek. De server beantwoordt met een rapport dat, instantie per instantie, aangeeft wat hij heeft opgeslagen en wat hij heeft geweigerd.</p>

<div class="codeblock" id="code">
 <h3>Upload een map met DICOM‑bestanden - C#</h3>
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

<p>Sommige servers antwoorden met een HTTP‑fout, zoals 409, wanneer een of meer instanties falen. De oproep gooit dan een <code>DicomWebClientException</code>, zoals de volgende sectie laat zien, en hetzelfde rapport staat in de respons‑body van de exceptie.</p>

<p><code>StoreStudyAsync</code> uploadt naar één benoemde study. Beide methoden accepteren ook een <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, zodat bestanden gegenereerd kunnen worden terwijl het verzoek wordt verzonden.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Afhandelen van server‑fouten">}}

<p>Elke getypeerde bewerking controleert de HTTP‑status voordat ze terugkeert. Wanneer de server een verzoek weigert, gooit de oproep een <code>DicomWebClientException</code>, die de statuscode, de headers en het begin van de respons‑body voor de log bevat.</p>

<div class="codeblock" id="code">
 <h3>Rapporteer een mislukte aanvraag - C#</h3>
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

<p>Methoden die een ruwe respons teruggeven, zoals de thumbnail hierboven, laten de statuscontrole aan u over via <code>IsSuccessStatusCode</code> of <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Wat de client verder dekt">}}

<p>Zoeken, downloaden en uploaden zijn de gebruikelijke oproepen. De client heeft ook een operatierij voor elk van de andere DICOMweb‑services:</p>

<ul>
<li>Bulk‑data, pixeldata en geselecteerde frames van een instantie, per één‑gebaseerd framenummer, zoals de server ze heeft verzonden.</li>
<li>WADO-URI, de oudere enkel‑URL‑ophaling die veel viewers en archieven nog gebruiken, voor DICOM‑bestanden en gerenderde afbeeldingen.</li>
<li>UPS-RS‑werk lijsten: maak, zoek, update en claim werkitems, wijzig hun status, vraag annulering aan, en abonneer op events die via een WebSocket binnenkomen.</li>
<li>De modaliteitswerklijst (MWL-RS), modaliteit uitgevoerde procedurestappen (MPPS-RS) en opslag‑commitment.</li>
<li>Niet‑patiënt‑instanties zoals kleurbereiken en hanging‑protocols, en het capabilities‑document van de server.</li>
<li><code>SendAsync</code> voor elke standaardtransactie met uw eigen route‑waarden, query‑parameters, headers en body, wanneer een getypeerde methode niet past.</li>
</ul>

<p>De <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb-clientgids</a> documenteert elke bewerking en optie, en de <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">client‑operatierefentie</a> somt elke methode op.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Eén bibliotheek van de HTTP‑respons tot de pixeldata">}}

<p>Een bestand dat met de client wordt gedownload is een regulier <code>DicomFile</code>. Het kan geanonimiseerd, getranscodeerd, gerenderd of geserialiseerd worden in hetzelfde proces, zonder conversiestap en zonder een tweede bibliotheek in de keten.</p>

<p>Gerelateerde pagina's: <a href="/medical/net/dicom-networking/">DICOM‑netwerken</a> voor archieven die DIMSE in plaats van HTTP spreken, <a href="/medical/net/anonymization/">anonimisering</a> voordat een study het ziekenhuis verlaat, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer‑syntax‑conversie</a> voor wat de server opslaat, en <a href="/medical/net/dicom-to-json/">DICOM naar JSON</a> voor het formaat dat QIDO-RS‑zoekresultaten gebruiken.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leerbronnen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Ontwikkelaarshandleiding" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
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
