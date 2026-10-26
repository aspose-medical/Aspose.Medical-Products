---
title: C# .NET'te DICOMweb İstemcisi - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: DICOM çalışmalarını HTTP üzerinden .NET ile arayın, indirin ve yükleyin. PACS, VNA ve bulut görüntüleme hizmetleriyle çalışan, QIDO-RS, WADO-RS ve STOW-RS için saf C# ile bir DICOMweb istemcisi.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# için DICOMweb İstemcisi" h2="Herhangi bir DICOMweb sunucusunda QIDO-RS, WADO-RS ve STOW-RS ile çalıştırılan, yönetilen C# içinde, makinede hiçbir şey kurmadan çalışmalarını arayın, indirin ve yükleyin." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Kendi kodunuzdan HTTP üzerinden DICOM">}}

<p>DICOMweb, DICOM'un HTTP biçimidir. 104 portunda bir DIMSE ilişkisi yerine, uygulamanız normal HTTPS istekleri gönderir: arama için QIDO-RS, indirme için WADO-RS, yükleme için STOW-RS. Azure Health Data Services DICOM hizmeti ve Google Cloud Healthcare API gibi bulut görüntüleme hizmetleri bu şekilde çalışır ve Orthanc ve dcm4chee gibi arşivler de aynı şekilde çalışır.</p>

<p><strong>Aspose.Medical for .NET</strong>, 26.10 sürümünden itibaren bir DICOMweb istemcisi içerir. <code>DicomWebClient</code>, her standart işlemi tiplenmiş bir yöntemle kapsar ve sonuçlar, kütüphanenin geri kalanı tarafından okunan ve yazılan aynı <code>Dataset</code> ve <code>DicomFile</code> nesneleri olarak gelir. Diğer taraftaki sunucu herhangi bir DICOMweb uygulaması olabilir; Aspose.Medical ile inşa edilmiş olması gerekmez.</p>

<p>İstemci, <code>HttpClient</code> üzerine inşa edilmiş yönetilen C#'dır. Yerel bir araç kiti ve proxy servisi yoktur, bu yüzden aynı kod Windows, Linux ve bir konteyner içinde çalışır.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bir DICOMweb sunucusuna bağlanın">}}

<p>İstemciyi bir <code>HttpClient</code> ve sunucunun hizmet kök URL'si ile oluşturun. Kimlik doğrulama, TLS ayarları, proxy'ler ve bağlantı havuzu <code>HttpClient</code> üzerinde kalır; uygulamanız zaten bunları burada yapılandırır. İstemci, <code>HttpClient</code>'i sahiplenmez ve onu kapatmaz.</p>

<div class="codeblock" id="code">
 <h3>DICOMweb istemcisi oluşturma - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Bir dağıtım WADO-URI veya başka bir hizmeti farklı bir URL'den sunuyorsa, <code>ServiceBaseUris</code> yalnızca o hizmet için kökü geçersiz kılar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="QIDO-RS ile çalışmaların aranması">}}

<p>Arama, DICOM anahtar kelimesini isim olarak kullanan eşleşen parametreleri, döndürülecek alanları ve sayfalama kontrollerini alır. Eşleşmeler, yanıt hâlâ indiriliyor iken birer birer okunabilen dataset akışı olarak gelir.</p>

<div class="codeblock" id="code">
 <h3>Bir hastanın çalışmalarını bulma - C#</h3>
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

<p><code>SearchSeriesAsync</code> ve <code>SearchInstancesAsync</code>, bir seviye aşağı, tüm arşivde veya bir çalışma ya da seri içinde arama yapar. Aynı seçenekler aynı zamanda bulanık eşleşme, boş değer eşleşmesi ve sayfalama için sonuç ofsetini ayarlar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="WADO-RS ile bir çalışma indirme">}}

<p>Bir çalışma binlerce örnek içerebilir. <code>RetrieveStudyAsync</code> bunları bir akış olarak döndürür, böylece her dosya yanıtın bir bölümü geldiğinde ayrıştırılır ve bir sonrakini okumadan önce kaydedilebilir. Bellek kullanımı çalışmanın boyutu ile artmaz.</p>

<div class="codeblock" id="code">
 <h3>Bir çalışmanın tüm örneklerini kaydet - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> ve <code>RetrieveInstanceAsync</code>, bir seriyi veya bir örneği aynı şekilde indirir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Küçük resim veya renderlanmış görüntü alın">}}

<p>Bir görüntüleyici veya iş listesi ekranı genellikle bir DICOM dosyası yerine bir resim ister. Sunucu bunu renderlar ve JPEG ya da PNG olarak gönderir, böylece istemci orijinal piksel verisini indirmez veya çözümlemez.</p>

<div class="codeblock" id="code">
 <h3>Bir örneğin JPEG küçük resmini kaydet - C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> ve <code>RetrieveRenderedFramesAsync</code> bunun yerine tam boyutlu bir görüntü döndürür. <code>DicomWebRenderOptions</code>, standart render kontrol parametrelerini sunucuya iletir: görüntü kalitesi, görüntü alanı boyutu, pencere ve ICC profili.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="STOW-RS ile örnekleri yükleme">}}

<p>STOW-RS, tek bir istek içinde istediğiniz sayıda örnek gönderir. Sunucu, her bir örnek için ne depoladığını ve neyi reddettiğini listeleyen bir raporla yanıt verir.</p>

<div class="codeblock" id="code">
 <h3>DICOM dosyaları içeren bir klasörü yükle - C#</h3>
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

<p>Bazı sunucular, bir veya daha fazla örnek başarısız olduğunda 409 gibi bir HTTP hatası döndürür. Bu durumda çağrı, bir sonraki bölümde gösterildiği gibi <code>DicomWebClientException</code> fırlatır ve aynı rapor istisnanın yanıt gövdesinde bulunur.</p>

<p><code>StoreStudyAsync</code>, tek bir adlandırılmış çalışmaya yükleme yapar. Her iki yöntem de bir <code>IAsyncEnumerable&lt;DicomFile&gt;</code> kabul eder, böylece dosyalar istek gönderilirken üretilebilir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sunucu hatalarını ele alın">}}

<p>Her tiplenmiş işlem, dönüş yapmadan önce HTTP durumunu kontrol eder. Sunucu bir isteği reddettiğinde, çağrı <code>DicomWebClientException</code> fırlatır; bu istisna, günlüğe kaydedilecek durum kodunu, başlıkları ve yanıt gövdesinin başlangıcını içerir.</p>

<div class="codeblock" id="code">
 <h3>Başarısız bir isteği raporla - C#</h3>
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

<p>Yukarıdaki küçük resim gibi ham yanıt döndüren yöntemler, durum kontrolünü <code>IsSuccessStatusCode</code> veya <code>EnsureSuccessStatusCode</code> aracılığıyla size bırakır.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="İstemcinin kapsadığı diğer konular">}}

<p>Arama, indirme ve yükleme ortak çağrılardır. İstemcinin diğer DICOMweb hizmetleri için de bir işlem grubu vardır:</p>

<ul>
<li>Toplu veri, piksel verisi ve bir örneğin seçilen kareleri, sunucunun gönderdiği şekilde bir-tabanlı kare numarasıyla.</li>
<li>WADO-URI, birçok görüntüleyici ve arşivin hâlâ kullandığı eski tek-URL alımı, DICOM dosyaları ve renderlanmış görüntüler için.</li>
<li>UPS-RS iş listeleri: iş öğeleri oluşturma, arama, güncelleme ve talep etme, durumlarını değiştirme, iptal isteği ve WebSocket üzerinden gelen olaylara abone olma.</li>
<li>Modalite iş listesi (MWL-RS), modalite tarafından gerçekleştirilen prosedür adımları (MPPS-RS) ve depolama taahhüdü.</li>
<li>Renk paletleri ve asılı protokoller gibi hasta dışı örnekler ve sunucunun yetenekler belgesi.</li>
<li><code>SendAsync</code>, tiplenmiş bir yöntem uygun olmadığında, kendi yol değerleriniz, sorgu parametreleriniz, başlıklarınız ve gövdenizle herhangi bir standart işlem için.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb istemci kılavuzu</a>, her işlem ve seçeneği belgeler ve <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">istemci işlem referansı</a> tüm yöntemleri listeler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTTP yanıtından piksel verisine tek bir kütüphane">}}

<p>İstemci ile indirilen bir dosya normal bir <code>DicomFile</code>'dır. Aynı süreçte anonimleştirilebilir, kodlaması değiştirilebilir, renderlanabilir ya da serileştirilebilir; bir dönüşüm adımı ve zincirde ikinci bir kütüphane gerekmez.</p>

<p>İlgili sayfalar: HTTP yerine DIMSE kullanan arşivler için <a href="/medical/net/dicom-networking/">DICOM ağ iletişimi</a>, bir çalışmanın hastaneden çıkmadan önceki <a href="/medical/net/anonymization/">anonimleştirme</a>, sunucunun sakladığı şeyler için <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer sentaksı dönüşümü</a>, ve QIDO-RS arama sonuçları formatı için <a href="/medical/net/dicom-to-json/">DICOM'dan JSON'a</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Belgelendirme" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Geliştirici Kılavuzu" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="API Referansları" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Ürün Desteği" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ücretsiz Destek" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Ücretli Destek" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Neden .NET için Aspose.Medical?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Müşteri Listesi" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Başarı Hikayeleri" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
