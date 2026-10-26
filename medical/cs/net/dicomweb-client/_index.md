---
title: DICOMweb klient v C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Vyhledávejte, stahujte a nahrávejte DICOM studie přes HTTP z .NET. DICOMweb klient v čistém C# pro QIDO-RS, WADO-RS a STOW-RS, který funguje s PACS, VNA a cloudovými zobrazovacími službami.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb klient pro .NET C#" h2="Vyhledávejte, stahujte a nahrávejte studie na libovolném DICOMweb serveru pomocí QIDO-RS, WADO-RS a STOW-RS, v řízeném C# bez nutnosti instalace na počítači." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM přes HTTP, z vašeho vlastního kódu">}}

<p>DICOMweb je HTTP forma DICOM. Místo DIMSE asociace na portu 104 vaše aplikace posílá běžné HTTPS požadavky: QIDO-RS pro vyhledávání, WADO-RS pro stahování, STOW-RS pro nahrávání. Cloudové zobrazovací služby jako Azure Health Data Services DICOM služba a Google Cloud Healthcare API fungují tímto způsobem, a tak také archivy jako Orthanc a dcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> obsahuje DICOMweb klienta od verze 26.10. <code>DicomWebClient</code> zapouzdřuje každou standardní transakci do typizované metody a výsledky přicházejí jako stejné objekty <code>Dataset</code> a <code>DicomFile</code>, které zbytek knihovny čte a zapisuje. Server na druhé straně může být jakákoliv implementace DICOMweb; nemusí být postaven pomocí Aspose.Medical.</p>

<p>Klient je řízený C# postavený na <code>HttpClient</code>. Neexistuje žádná nativní sady nástrojů ani proxy služba, takže stejný kód běží na Windows, Linuxu i v kontejneru.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Připojení k DICOMweb serveru">}}

<p>Vytvořte klienta z <code>HttpClient</code> a kořenové URL služby serveru. Ověřování, nastavení TLS, proxy a sdílení připojení zůstávají na <code>HttpClient</code>, kde je vaše aplikace již konfiguruje. Klient nevlastní <code>HttpClient</code> a nevolá jeho uvolnění.</p>

<div class="codeblock" id="code">
 <h3>Vytvoření DICOMweb klienta - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Pokud jedno nasazení poskytuje WADO-URI nebo jinou službu z jiné URL, <code>ServiceBaseUris</code> přepíše kořen jen pro tuto službu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vyhledávání studií pomocí QIDO-RS">}}

<p>Vyhledávání přijímá parametry shody s DICOM klíčovým slovem jako názvem, pole, která se mají vrátit, a ovládání stránkování. Výsledky přicházejí jako proud datasetů, čtou se po jednom, dokud odpověď stále stahuje.</p>

<div class="codeblock" id="code">
 <h3>Vyhledání studií pacienta - C#</h3>
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

<p><code>SearchSeriesAsync</code> a <code>SearchInstancesAsync</code> vyhledávají o úroveň níž, napříč celým archivem nebo uvnitř studie či série. Stejné možnosti také nastavení fuzzy shody, shody prázdných hodnot a offsetu výsledků pro stránkování.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stažení studie pomocí WADO-RS">}}

<p>Studie může obsahovat tisíce instancí. <code>RetrieveStudyAsync</code> je vrací jako proud, takže každý soubor je zpracován, jakmile dorazí jeho část odpovědi, a může být uložen před načtením dalšího. Spotřeba paměti neroste s velikostí studie.</p>

<div class="codeblock" id="code">
 <h3>Uložení každé instance studie - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Získání náhledu nebo renderovaného obrazu">}}

<p>Prohlížeč nebo obrazovka pracovního seznamu často potřebuje obrázek, ne DICOM soubor. Server jej renderuje a pošle JPEG nebo PNG, takže klient nestahuje ani ne dekóduje původní pixelová data.</p>

<div class="codeblock" id="code">
 <h3>Uložení JPEG náhledu instance - C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> a <code>RetrieveRenderedFramesAsync</code> místo toho vrací obrázek v plné velikosti. <code>DicomWebRenderOptions</code> předává serveru standardní nastavení renderování: kvalita obrazu, velikost viewportu, okno a ICC profil.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nahrání instancí pomocí STOW-RS">}}

<p>STOW-RS odesílá libovolný počet instancí v jednom požadavku. Server odpovídá reportem, který uvádí instance po instanci, co uložené a co odmítnuté.</p>

<div class="codeblock" id="code">
 <h3>Nahrání složky DICOM souborů - C#</h3>
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

<p>Některé servery odpovídají HTTP chybou, například 409, když selže jedna nebo více instancí. Pak volání vyhodí <code>DicomWebClientException</code>, jak ukazuje následující sekce, a stejný report je v těle odpovědi výjimky.</p>

<p><code>StoreStudyAsync</code> nahrává do jedné pojmenované studie. Obě metody také přijímají <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, takže soubory mohou být generovány během odesílání požadavku.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zpracování chyb serveru">}}

<p>Každá typová operace kontroluje HTTP status před návratem. Když server odmítne požadavek, volání vyhodí <code>DicomWebClientException</code>, která obsahuje stavový kód, hlavičky a začátek těla odpovědi pro log.</p>

<div class="codeblock" id="code">
 <h3>Reportování neúspěšného požadavku - C#</h3>
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

<p>Metody, které vrací surovou odpověď, jako např. výše uvedený náhled, nechávají kontrolu statusu na vás pomocí <code>IsSuccessStatusCode</code> nebo <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Co dalšího klient pokrývá">}}

<p>Vyhledávání, stahování a nahrávání jsou běžné volání. Klient má také skupinu operací pro každou z ostatních DICOMweb služeb:</p>

<ul>
<li>Hromadná data, pixelová data a vybrané snímky instance podle číslování od jedné, tak jak je server posílá.</li>
<li>WADO-URI, starší získání jedné URL, které stále používá mnoho prohlížečů a archivů, pro DICOM soubory a renderované obrazy.</li>
<li>UPS-RS pracovní seznamy: vytvořit, vyhledat, aktualizovat a přidělit pracovní položky, změnit jejich stav, požádat o zrušení a přihlásit se k událostem přicházejícím přes WebSocket.</li>
<li>Modální pracovní seznam (MWL-RS), modální provedené kroky procedur (MPPS-RS) a závazek úložiště.</li>
<li>Instanace nesouvisející s pacientem, jako jsou barevné palety a hanging protokoly, a dokument schopností serveru.</li>
<li><code>SendAsync</code> pro jakoukoliv standardní transakci s vašimi vlastními hodnotami trasy, parametry dotazu, hlavičkami a tělem, když typizovaná metoda nevyhovuje.</li>
</ul>

<p>Průvodce <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb client guide</a> dokumentuje každou operaci a volbu a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">client operation reference</a> uvádí všechny metody.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Jedna knihovna od HTTP odpovědi po pixelová data">}}

<p>Soubor stažený pomocí klienta je běžný <code>DicomFile</code>. Může být anonymizován, transkódován, renderován nebo serializován ve stejném procesu, bez konverzního kroku a bez druhé knihovny v řetězci.</p>

<p>Související stránky: <a href="/medical/net/dicom-networking/">DICOM networking</a> pro archivy, které komunikují pomocí DIMSE místo HTTP, <a href="/medical/net/anonymization/">anonymizace</a> před tím, než studie opustí nemocnici, <a href="/medical/net/dicom-transfer-syntax-conversion/">převod syntaxe přenosu</a> pro to, co server ukládá, a <a href="/medical/net/dicom-to-json/">DICOM do JSON</a> pro formát, který používají výsledky vyhledávání QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Vzdělávací zdroje" tabId="resources" >}}
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
