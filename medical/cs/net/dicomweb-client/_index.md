---
title: DICOMweb klient v C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Vyhledávejte, stahujte a nahrávejte DICOM studie přes HTTP z .NET. Čistý C# DICOMweb klient pro QIDO-RS, WADO-RS a STOW-RS, který funguje s PACS, VNA a cloudovými zobrazovacími službami.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb klient pro .NET C#" h2="Vyhledávejte, stahujte a nahrávejte studie na libovolném DICOMweb serveru pomocí QIDO-RS, WADO-RS a STOW-RS, v řízeném C# bez nutnosti instalace na počítači." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM přes HTTP, z vašeho vlastního kódu">}}

<p>DICOMweb je HTTP forma DICOM. Místo DIMSE asociace na portu 104 vaše aplikace odesílá běžné HTTPS požadavky: QIDO-RS pro vyhledávání, WADO-RS pro stahování, STOW-RS pro nahrávání. Cloudové zobrazovací služby jako Azure Health Data Services DICOM služba a Google Cloud Healthcare API fungují tímto způsobem, stejně jako archivy jako Orthanc a dcm4chee.</p>

<p><strong>Aspose.Medical pro .NET</strong> obsahuje DICOMweb klienta od verze 26.10. <code>DicomWebClient</code> zapouzdřuje každou standardní transakci v typové metodě a výsledky přicházejí jako stejné objekty <code>Dataset</code> a <code>DicomFile</code>, které zbytek knihovny čte a zapisuje. Server na druhé straně může být jakákoli implementace DICOMweb; nemusí být vytvořen s Aspose.Medical.</p>

<p>Klient je řízený C# postavený nad <code>HttpClient</code>. Neexistuje žádná nativní sada nástrojů ani proxy služba, takže stejný kód běží na Windows, Linuxu i v kontejneru.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Připojte se k DICOMweb serveru">}}

<p>Vytvořte klienta z <code>HttpClient</code> a kořenové URL služby serveru. Autentizace, nastavení TLS, proxy a sdílení spojení zůstávají na <code>HttpClient</code>, kde je vaše aplikace již konfiguruje. Klient nevlastní <code>HttpClient</code> a neukončuje jeho životnost.</p>

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

<p>Pokud jedna nasazení poskytuje WADO-URI nebo jinou službu z jiné URL, <code>ServiceBaseUris</code> přepíše kořen jen pro tuto službu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vyhledávejte studie pomocí QIDO-RS">}}

<p>Vyhledávání přijímá parametry shody s DICOM klíčovým slovem jako názvem, pole k vrácení a řízení stránkování. Výsledky přicházejí jako proud datasetů, čtou se po jednom, dokud odpověď stále stahuje.</p>

<div class="codeblock" id="code">
 <h3>Najděte studie pacienta - C#</h3>
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

<p><code>SearchSeriesAsync</code> a <code>SearchInstancesAsync</code> vyhledávají o úroveň níž, napříč celým archivem nebo v rámci jedné studie či série. Stejné možnosti také nastavují fuzzy shodu, shodu prázdných hodnot a offset výsledků pro stránkování.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stáhněte studii pomocí WADO-RS">}}

<p>Studie může obsahovat tisíce instancí. <code>RetrieveStudyAsync</code> je vrací jako proud, takže každý soubor je zpracován, jakmile dorazí jeho část odpovědi, a může být uložen před načtením dalšího. Spotřeba paměti neroste s velikostí studie.</p>

<div class="codeblock" id="code">
 <h3>Uložte každou instanci studie - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Získejte miniaturu nebo vykreslený obrázek">}}

<p>Prohlížeč nebo obrazovka pracovního seznamu často potřebuje obrázek, nikoli DICOM soubor. Server jej vykreslí a pošle JPEG nebo PNG, takže klient nestahuje ani nedekóduje původní pixelová data.</p>

<div class="codeblock" id="code">
 <h3>Uložte JPEG miniaturu instance - C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> a <code>RetrieveRenderedFramesAsync</code> místo toho vrací obrázek v plné velikosti. <code>DicomWebRenderOptions</code> předává serveru standardní ovládací prvky vykreslování: kvalitu obrazu, velikost viewportu, okno a ICC profil.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nahrávejte instance pomocí STOW-RS">}}

<p>STOW-RS odesílá libovolný počet instancí v jednom požadavku. Server odpovídá zprávou, která uvádí, instance po instanci, co uložil a co odmítl.</p>

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

<p>Některé servery odpoví HTTP chybou, například 409, když selže jedna nebo více instancí. Volání pak vyvolá výjimku <code>DicomWebClientException</code>, jak ukazuje následující oddíl, a stejná zpráva je v těle odpovědi výjimky.</p>

<p><code>StoreStudyAsync</code> nahrává do jedné pojmenované studie. Obě metody také akceptují <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, takže soubory mohou být generovány během odesílání požadavku.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zpracování chyb serveru">}}

<p>Každá typová operace kontroluje HTTP stav před návratem. Když server odmítne požadavek, volání vyvolá <code>DicomWebClientException</code>, která obsahuje stavový kód, hlavičky a začátek těla odpovědi pro log.</p>

<div class="codeblock" id="code">
 <h3>Nahlásit neúspěšný požadavek - C#</h3>
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

<p>Metody, které vrací surovou odpověď, jako je výše uvedená miniatura, nechávají kontrolu stavu na vás pomocí <code>IsSuccessStatusCode</code> nebo <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Co dalšího klient pokrývá">}}

<p>Vyhledávání, stahování a nahrávání jsou běžné volání. Klient má také skupinu operací pro každou z ostatních DICOMweb služeb:</p>

<ul>
<li>Hromadná data, pixelová data a vybrané snímky instance podle jednorozměrného čísla snímku, jak je server poslal.</li>
<li>WADO-URI, starší jednorázové získání přes jedinou URL, které stále používá mnoho prohlížečů a archivů, pro DICOM soubory a vykreslené obrázky.</li>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> pracovní seznamy: vytvářet, hledat, aktualizovat a nárokovat pracovní položky, měnit jejich stav, žádat o zrušení a přihlásit se k událostem přicházejícím přes WebSocket.</li>
<li><a href="/medical/net/modality-worklist/">Modality worklist</a> (MWL-RS), provedené kroky procedury modality (MPPS-RS) a závazek úložiště.</li>
<li>Instance nesouvisející s pacientem, jako jsou barevné palety a zavěsné protokoly, a dokument o schopnostech serveru.</li>
<li><code>SendAsync</code> pro libovolnou standardní transakci s vlastními hodnotami trasy, parametry dotazu, hlavičkami a tělem, když typová metoda nevyhovuje.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">Průvodce DICOMweb klientem</a> dokumentuje každou operaci a možnost, a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">reference operací klienta</a> uvádí každou metodu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Jedna knihovna od HTTP odpovědi po pixelová data">}}

<p>Soubor stažený klientem je běžný <code>DicomFile</code>. Může být anonymizován, transkódován, vykreslen nebo serializován ve stejném procesu, bez kroků konverze a bez druhé knihovny v řetězci.</p>

<p>Související stránky: <a href="/medical/net/dicomweb-server/">DICOMweb server</a> pro druhou stranu protokolu ve vaší vlastní službě ASP.NET Core, <a href="/medical/net/dicom-networking/">DICOM networking</a> pro archivy, které komunikují pomocí DIMSE místo HTTP, <a href="/medical/net/anonymization/">anonymizaci</a> před tím, než studii opustí nemocnici, <a href="/medical/net/dicom-transfer-syntax-conversion/">konverzi přenosové syntaxe</a> pro to, co server ukládá, a <a href="/medical/net/dicom-to-json/">DICOM na JSON</a> pro formát, který používají výsledky vyhledávání QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Učební zdroje" tabId="resources" >}}
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
