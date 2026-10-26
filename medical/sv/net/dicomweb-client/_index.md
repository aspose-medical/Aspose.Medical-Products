---
title: DICOMweb‑klient i C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Sök, ladda ner och ladda upp DICOM‑studier över HTTP från .NET. En DICOMweb‑klient i ren C# för QIDO-RS, WADO-RS och STOW-RS som fungerar med PACS, VNA och molnbaserade bildtjänster.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb‑klient för .NET C#" h2="Sök, ladda ner och ladda upp studier på vilken DICOMweb‑server som helst med QIDO-RS, WADO-RS och STOW-RS, i hanterad C# utan att behöva installera något på maskinen." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM över HTTP, från din egen kod">}}

<p>DICOMweb är DICOM:s HTTP‑form. Istället för en DIMSE‑association på port 104 skickar din applikation vanliga HTTPS‑förfrågningar: QIDO-RS för sökning, WADO-RS för nedladdning, STOW-RS för uppladdning. Molnbaserade bildtjänster som Azure Health Data Services DICOM‑tjänst och Google Cloud Healthcare API fungerar på detta sätt, likaså arkiv som Orthanc och dcm4chee.</p>

<p><strong>Aspose.Medical för .NET</strong> innehåller en DICOMweb‑klient sedan version 26.10. <code>DicomWebClient</code> omsluter varje standardtransaktion i en typad metod, och resultaten levereras som samma <code>Dataset</code>- och <code>DicomFile</code>-objekt som resten av biblioteket läser och skriver. Servern på andra sidan kan vara vilken DICOMweb‑implementation som helst; den behöver inte vara byggd med Aspose.Medical.</p>

<p>Klienten är hanterad C# ovanpå <code>HttpClient</code>. Det finns inget inbyggt verktygssats och ingen proxytjänst, så samma kod körs på Windows, Linux och i en container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anslut till en DICOMweb‑server">}}

<p>Skapa klienten från ett <code>HttpClient</code> och serverns tjänstrot‑URL. Autentisering, TLS‑inställningar, proxys och anslutningspoolering förblir på <code>HttpClient</code>, där din applikation redan konfigurerar dem. Klienten äger inte <code>HttpClient</code> och disponerar den inte.</p>

<div class="codeblock" id="code">
 <h3>Skapa en DICOMweb‑klient - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Om en distribution betjänar WADO-URI eller en annan tjänst från en annan URL, överskriver <code>ServiceBaseUris</code> roten endast för den tjänsten.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sök studier med QIDO-RS">}}

<p>En sökning tar matchningsparametrar med DICOM‑nyckelordet som namn, fälten som ska returneras och sidindelningskontroller. Matchningarna levereras som en ström av dataset, läses ett i taget medan svaret fortfarande laddas ner.</p>

<div class="codeblock" id="code">
 <h3>Hitta patientens studier - C#</h3>
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

<p><code>SearchSeriesAsync</code> och <code>SearchInstancesAsync</code> söker en nivå ner, över hela arkivet eller inom en studie eller serie. Samma alternativ ställer även in fuzzy‑matchning, matchning av tomma värden och resultatoffset för sidindelning.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ladda ner en studie med WADO-RS">}}

<p>En studie kan innehålla tusentals instanser. <code>RetrieveStudyAsync</code> returnerar dem som en ström, så varje fil parsas när dess del av svaret anländer och kan sparas innan nästa läses. Minnesanvändningen växer inte med studiens storlek.</p>

<div class="codeblock" id="code">
 <h3>Spara varje instans i en studie - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> och <code>RetrieveInstanceAsync</code> laddar ner en serie eller en instans på samma sätt.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Hämta en miniatyrbild eller en renderad bild">}}

<p>En visare eller en arbetslisteskärm behöver ofta en bild, inte en DICOM‑fil. Servern renderar den och skickar en JPEG eller PNG, så klienten laddar inte ner eller avkodar de ursprungliga pixeldatan.</p>

<div class="codeblock" id="code">
 <h3>Spara en JPEG‑miniatyr av en instans - C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> och <code>RetrieveRenderedFramesAsync</code> returnerar en fullstor bild istället. <code>DicomWebRenderOptions</code> överför de standardrenderingskontrollerna till servern: bildkvalitet, viewport‑storlek, fönster och ICC‑profil.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ladda upp instanser med STOW-RS">}}

<p>STOW-RS skickar valfritt antal instanser i en enda begäran. Servern svarar med en rapport som listor, instans för instans, vad den har lagrat och vad den avvisade.</p>

<div class="codeblock" id="code">
 <h3>Ladda upp en mapp med DICOM‑filer - C#</h3>
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

<p>Vissa servrar svarar med ett HTTP‑fel, såsom 409, när en eller flera instanser misslyckas. Anropet kastar då <code>DicomWebClientException</code>, som nästa avsnitt visar, och samma rapport finns i undantagets svarskropp.</p>

<p><code>StoreStudyAsync</code> laddar upp till en namngiven studie. Båda metoderna accepterar också ett <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, så filer kan genereras medan begäran skickas.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Hantera serverfel">}}

<p>Varje typad operation kontrollerar HTTP‑status innan den returneras. När servern avvisar en begäran kastar anropet <code>DicomWebClientException</code>, som innehåller statuskoden, rubrikerna och början av svarskroppen för loggning.</p>

<div class="codeblock" id="code">
 <h3>Rapportera en misslyckad begäran - C#</h3>
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

<p>Metoder som returnerar ett rått svar, såsom miniatyrbilden ovan, lämnar statuskontrollen till dig via <code>IsSuccessStatusCode</code> eller <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vad mer täcker klienten">}}

<p>Sök, ladda ner och ladda upp är de vanliga anropen. Klienten har också en operationsgrupp för var och en av de övriga DICOMweb‑tjänsterna:</p>

<ul>
<li>Bulk‑data, pixeldata och valda ramar av en instans, enligt ett‑baserat ramnummer, som servern skickade.</li>
<li>WADO-URI, den äldre enkla‑URL‑hämtningen som många visare och arkiv fortfarande använder, för DICOM‑filer och renderade bilder.</li>
<li>UPS‑RS‑arbetslistor: skapa, söka, uppdatera och ta anspråk på arbetsobjekt, ändra deras tillstånd, begära avbokning och prenumerera på händelser som anländer via en WebSocket.</li>
<li>Modalitetsarbetslistan (MWL‑RS), modala utförda procedursteg (MPPS‑RS) och lagringsåtagande.</li>
<li>Icke‑patient‑instanser såsom färgpaletter och hängande protokoll, samt serverns capabilities‑dokument.</li>
<li><code>SendAsync</code> för vilken standardtransaktion som helst med egna route‑värden, query‑parametrar, rubriker och kropp, när en typad metod inte räcker.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb‑klientguiden</a> dokumenterar varje operation och alternativ, och <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">klientens operationsreferens</a> listar alla metoder.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ett bibliotek från HTTP‑svaret till pixeldata">}}

<p>En fil som laddas ner med klienten är en vanlig <code>DicomFile</code>. Den kan anonymiseras, transkodas, renderas eller serialiseras i samma process, utan omvandlingssteg och utan ett andra bibliotek i kedjan.</p>

<p>Relaterade sidor: <a href="/medical/net/dicom-networking/">DICOM‑nätverk</a> för arkiv som talar DIMSE istället för HTTP, <a href="/medical/net/anonymization/">anonymisering</a> innan en studie lämnar sjukhuset, <a href="/medical/net/dicom-transfer-syntax-conversion/">överföringssyntax‑konvertering</a> för vad servern lagrar, och <a href="/medical/net/dicom-to-json/">DICOM till JSON</a> för formatet som QIDO-RS‑sökresultat använder.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Utvecklarguide" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="API‑referenser" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktsupport" tabId="support" >}}
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
