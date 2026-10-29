---
title: DICOMweb klient v C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Vyhledávejte, stahujte a nahrávejte DICOM studie přes HTTP z .NET. Čistý C# DICOMweb klient pro QIDO-RS, WADO-RS a STOW-RS, který funguje s PACS, VNA a cloudovými zobrazovacími službami.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb klient pro .NET C#" h2="Vyhledávejte, stahujte a nahrávejte studie na libovolném DICOMweb serveru pomocí QIDO-RS, WADO-RS a STOW-RS, v řízeném C# bez nutnosti instalace čehokoliv na stroj." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM přes HTTP, z vaší vlastní kódu">}}

<p>DICOMweb je HTTP forma DICOM. Místo DIMSE asociace na portu 104 vaše aplikace odesílá běžné HTTPS požadavky: QIDO-RS pro vyhledávání, WADO-RS pro stahování, STOW-RS pro nahrávání. Cloudové zobrazovací služby jako Azure Health Data Services DICOM služba a Google Cloud Healthcare API fungují tímto způsobem, stejně jako archivy jako Orthanc a dcm4chee.</p>

<p><strong>Aspose.Medical pro .NET</strong> obsahuje DICOMweb klienta od verze 26.10. <code>DicomWebClient</code> zapouzdřuje každou standardní transakci v typované metodě a výsledky přicházejí jako stejné objekty <code>Dataset</code> a <code>DicomFile</code>, které zbytek knihovny čte a zapisuje. Server na druhé straně může být jakákoliv implementace DICOMweb; nemusí být postaven pomocí Aspose.Medical.</p>

<p>Klient je řízený C# postavený nad <code>HttpClient</code>. Neexistuje žádný nativní nástroj ani proxy služba, takže stejný kód běží na Windows, Linuxu i v kontejneru.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Připojit se k DICOMweb serveru">}}

<p>Vytvořte klienta z <code>HttpClient</code> a kořenové URL služby serveru. Autentizace, nastavení TLS, proxy a poolování připojení zůstávají na <code>HttpClient</code>, kde je vaše aplikace již konfiguruje. Klient nevlastní <code>HttpClient</code> a nevolá jeho uvolnění.</p>

<div class="codeblock" id="code">
 <h3>Vytvořit DICOMweb klienta - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Pokud jedno nasazení poskytuje WADO-URI nebo jinou službu z odlišné URL, <code>ServiceBaseUris</code> přepíše kořen jen pro tuto službu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vyhledávat studie pomocí QIDO-RS">}}

<p>Vyhledávání přijímá parametry shody s DICOM klíčovým slovem jako názvem, pole k vrácení a ovládání stránkování. Shody přicházejí jako proud datasetů, čtou se po jednom, zatímco odpověď se stále stahuje.</p>

<div class="codeblock" id="code">
 <h3>Najít studie pacienta - C#</h3>
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

<p><code>SearchSeriesAsync</code> a <code>SearchInstancesAsync</code> vyhledávají o úroveň níž, napříč celým archivem či uvnitř jedné studie nebo série. Stejné možnosti také nastavují fuzzy shodu, shodu prázdných hodnot a offset výsledků pro stránkování.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stáhnout studii pomocí WADO-RS">}}

<p>Studie může obsahovat tisíce instancí. <code>RetrieveStudyAsync</code> je vrací jako proud, takže každý soubor je parsován, jakmile dorazí jeho část odpovědi, a může být uložen před načtením dalšího. Spotřeba paměti neroste s velikostí studie.</p>

<div class="codeblock" id="code">
 <h3>Uložit každou instanci studie - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> a <code>RetrieveInstanceAsync</code> stahují jednu sérii nebo jednu instanci stejným způsobem.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Získat miniaturu nebo vykreslený obrázek">}}

<p>Prohlížeč nebo obrazovka pracovního seznamu často potřebuje obrázek, ne DICOM soubor. Server jej vykreslí a pošle JPEG nebo PNG, takže klient nestahuje ani nerozkládá původní pixelová data.</p>

<div class="codeblock" id="code">
 <h3>Uložit JPEG miniaturu instance - C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> a <code>RetrieveRenderedFramesAsync</code> místo toho vrací obrázek v plné velikosti. <code>DicomWebRenderOptions</code> předává standardní ovládací prvky renderování serveru: kvalitu obrázku, velikost viewportu, okno a ICC profil.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nahrát instance pomocí STOW-RS">}}

<p>STOW-RS odesílá libovolný počet instancí v jednom požadavku. Server odpovídá zprávou, která uvádí, instance po instanci, co bylo uloženo a co odmítnuto.</p>

<div class="codeblock" id="code">
 <h3>Nahrát složku DICOM souborů - C#</h3>
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

<p>Některé servery odpovídají HTTP chybou, například 409, když jedna nebo více instancí selže. Volání pak vyvolá <code>DicomWebClientException</code>, jak ukazuje následující sekce, a stejná zpráva je v těle odpovědi výjimky.</p>

<p><code>StoreStudyAsync</code> nahrává do jedné pojmenované studie. Obě metody také akceptují <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, takže soubory mohou být generovány během odesílání požadavku.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zpracovat chyby serveru">}}

<p>Každá typovaná operace kontroluje stav HTTP před návratem. Když server požadavek odmítne, volání vyvolá <code>DicomWebClientException</code>, která nese stavový kód, hlavičky a začátek těla odpovědi pro log.</p>

<div class="codeblock" id="code">
 <h3>Hlásit neúspěšný požadavek - C#</h3>
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

<p>Metody, které vracejí surovou odpověď, jako výše uvedená miniatura, nechávají kontrolu stavu na vás pomocí <code>IsSuccessStatusCode</code> nebo <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Co dalšího klient pokrývá">}}

<p>Vyhledávání, stahování a nahrávání jsou běžné volání. Klient má také skupinu operací pro každou z ostatních DICOMweb služeb:</p>

<ul>
<li>Hromadná data, pixelová data a vybrané snímky instance podle číslování snímků od jedné, tak jak je server posílá.</li>
<li>WADO-URI, starší získávání jedné URL, které mnoho prohlížečů a archivů stále používá, pro DICOM soubory a vykreslené obrázky.</li>
<li>UPS-RS pracovní seznamy: vytvořit, vyhledat, aktualizovat a přebrat pracovní položky, změnit jejich stav, požádat o zrušení a přihlásit se k událostem přicházejícím přes WebSocket.</li>
<li>Modální pracovní seznam (MWL-RS), provedené procedurální kroky modality (MPPS-RS) a úschova závazku.</li>
<li>Instance nesouvisející s pacientem, jako jsou barevné palety a hanging protokoly, a dokument o schopnostech serveru.</li>
<li><code>SendAsync</code> pro jakoukoli standardní transakci s vlastními hodnotami trasy, parametry dotazu, hlavičkami a tělem, když typová metoda nevyhovuje.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">Průvodce DICOMweb klientem</a> dokumentuje každou operaci a možnost a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">reference operací klienta</a> uvádí všechny metody.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Jedna knihovna od HTTP odpovědi po pixelová data">}}

<p>Soubor stažený klientem je běžný <code>DicomFile</code>. Může být anonymizován, transkódován, vykreslen nebo serializován ve stejném procesu, bez kroku konverze a bez druhé knihovny v řetězci.</p>

<p>Související stránky: <a href="/medical/net/dicomweb-server/">DICOMweb server</a> pro druhou stranu protokolu ve vaší vlastní ASP.NET Core službě, <a href="/medical/net/dicom-networking/">DICOM síťování</a> pro archivy, které používají DIMSE místo HTTP, <a href="/medical/net/anonymization/">anonymizaci</a> před tím, než studie opustí nemocnici, <a href="/medical/net/dicom-transfer-syntax-conversion/">převod přenosové syntaxe</a> pro to, co server ukládá, a <a href="/medical/net/dicom-to-json/">DICOM na JSON</a> pro formát, který používají výsledky vyhledávání QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Výukové zdroje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Průvodce vývojáře" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
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
