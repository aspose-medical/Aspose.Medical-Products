---
title: C# .NET'te DICOMweb İstemcisi - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: HTTP üzerinden .NET ile DICOM çalışmaları ara, indir ve yükle. QIDO-RS, WADO-RS ve STOW-RS için saf C#'ta bir DICOMweb istemcisi; PACS, VNA ve bulut görüntüleme hizmetleriyle çalışır.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb İstemcisi .NET C# için" h2="QIDO-RS, WADO-RS ve STOW-RS kullanarak herhangi bir DICOMweb sunucusunda çalışmaları ara, indir ve yükle; yönetilen C# ile makinede hiçbir şey kurmaya gerek yok." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Kendi kodunuzdan HTTP üzerinden DICOM">}}

<p>DICOMweb, DICOM'un HTTP biçimidir. 104 portunda bir DIMSE ilişkisi yerine uygulamanız normal HTTPS istekleri gönderir: arama için QIDO-RS, indirme için WADO-RS, yükleme için STOW-RS. Azure Health Data Services DICOM servisi ve Google Cloud Healthcare API gibi bulut görüntüleme hizmetleri bu şekilde çalışır; Orthanc ve dcm4chee gibi arşivler de aynı şekilde çalışır.</p>

<p><strong>Aspose.Medical for .NET</strong>, 26.10 sürümünden bu yana bir DICOMweb istemcisi içerir. <code>DicomWebClient</code> her standart işlemi tiplenmiş bir metodda kapsar ve sonuçlar kütüphanenin geri kalanının okuduğu ve yazdığı aynı <code>Dataset</code> ve <code>DicomFile</code> nesneleri olarak gelir. Diğer taraftaki sunucu herhangi bir DICOMweb uygulaması olabilir; Aspose.Medical ile oluşturulmak zorunda değildir.</p>

<p>İstemci, <code>HttpClient</code> üzerine kurulmuş yönetilen C#'dır. Yerel bir araç takımı ve proxy servisi yoktur, bu yüzden aynı kod Windows, Linux ve bir konteyner içinde çalışır.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bir DICOMweb sunucusuna bağlan">}}

<p>İstemciyi bir <code>HttpClient</code> ve sunucunun hizmet kök URL'si ile oluşturun. Kimlik doğrulama, TLS ayarları, proxy'ler ve bağlantı havuzu <code>HttpClient</code>'da kalır; uygulamanız zaten onları yapılandırır. İstemci <code>HttpClient</code>'i sahiplenmez ve onu kapatmaz.</p>

<div class="codeblock" id="code">
 <h3>Bir DICOMweb istemcisi oluştur - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="QIDO-RS ile çalışmaları ara">}}

<p>Bir arama, DICOM anahtar sözcüğü isim olarak kullanılan eşleşen parametreleri, döndürülecek alanları ve sayfalama kontrollerini alır. Eşleşmeler, yanıt hâlâ indirilirken bir defada bir okunabilecek veri seti akışı olarak gelir.</p>

<div class="codeblock" id="code">
 <h3>Bir hastanın çalışmalarını bul - C#</h3>
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

<p><code>SearchSeriesAsync</code> ve <code>SearchInstancesAsync</code>, bir seviye aşağıya, tüm arşivde ya da bir çalışma ya da seri içinde arama yapar. Aynı seçenekler bulanık eşleşme, boş değer eşleşmesi ve sayfalama için sonuç ofsetini de ayarlar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="WADO-RS ile bir çalışma indir">}}

<p>Bir çalışma binlerce örnek içerebilir. <code>RetrieveStudyAsync</code> bunları bir akış olarak döndürür; böylece her dosya, yanıtın bir parçası geldiğinde ayrıştırılır ve bir sonrakini okumadan önce kaydedilebilir. Bellek kullanımı çalışmanın boyutu ile artmaz.</p>

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

<p><code>RetrieveSeriesAsync</code> ve <code>RetrieveInstanceAsync</code>, bir seriyi ya da bir örneği aynı şekilde indirir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Küçük resim veya oluşturulmuş bir görüntü al">}}

<p>Bir görüntüleyici veya iş listesi ekranı genellikle bir resim ister, DICOM dosyası değil. Sunucu bunu oluşturur ve JPEG ya da PNG olarak gönderir; böylece istemci orijinal piksel verisini indirmez veya çözmez.</p>

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

<p><code>RetrieveRenderedInstanceAsync</code> ve <code>RetrieveRenderedFramesAsync</code>, bunun yerine tam boyutlu bir görüntü döndürür. <code>DicomWebRenderOptions</code>, standart render kontrol parametrelerini sunucuya iletir: görüntü kalitesi, görünüm alanı boyutu, pencere ve ICC profili.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="STOW-RS ile örnekleri yükle">}}

<p>STOW-RS, tek bir istek içinde istediğiniz sayıda örneği gönderir. Sunucu, her örnek için ne depoladığını ve neyi reddettiğini listeleyen bir raporla yanıt verir.</p>

<div class="codeblock" id="code">
 <h3>Bir DICOM dosyaları klasörünü yükle - C#</h3>
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

<p>Bazı sunucular, bir veya daha fazla örnek başarısız olduğunda 409 gibi bir HTTP hatası ile yanıt verir. Bu durumda çağrı <code>DicomWebClientException</code> fırlatır, sonraki bölümde gösterildiği gibi, ve aynı rapor istisna yanıt gövdesinde bulunur.</p>

<p><code>StoreStudyAsync</code>, tek bir adlandırılmış çalışmaya yükleme yapar. Her iki yöntem de bir <code>IAsyncEnumerable&lt;DicomFile&gt;</code> kabul eder; böylece dosyalar istek gönderilirken üretilebilir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sunucu hatalarını ele al">}}

<p>Her tiplenmiş işlem, dönüşünden önce HTTP durumunu kontrol eder. Sunucu bir isteği reddettiğinde, çağrı <code>DicomWebClientException</code> fırlatır; bu istisna durum kodunu, başlıkları ve yanıt gövdesinin başlangıcını günlük için taşır.</p>

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

<p>Yukarıdaki küçük resim gibi ham yanıt döndüren metodlar, durum kontrolünü <code>IsSuccessStatusCode</code> veya <code>EnsureSuccessStatusCode</code> üzerinden size bırakır.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="İstemcinin kapsadığı diğer özellikler">}}

<p>Arama, indirme ve yükleme ortak çağrılardır. İstemcinin diğer DICOMweb hizmetleri için de bir işlem grubu vardır:</p>

<ul>
<li>Bir örnek için toplu veri, piksel verisi ve seçili kareler, bir‑tabanlı kare numarası ile, sunucunun gönderdiği şekilde.</li>
<li>WADO-URI, birçok görüntüleyici ve arşivin hâlâ kullandığı eski tek‑URL alma yöntemi; DICOM dosyaları ve oluşturulmuş görüntüler için.</li>
<li>UPS-RS iş listeleri: iş öğeleri oluşturma, arama, güncelleme ve talep etme, durumlarını değiştirme, iptal talep etme ve WebSocket üzerinden gelen olaylara abone olma.</li>
<li>Modalite iş listesi (MWL-RS), gerçekleştirilen prosedür adımları (MPPS-RS) ve depolama taahhüdü.</li>
<li>Renk paletleri ve asılı protokoller gibi hasta dışı örnekler ve sunucunun yetenekler belgesi.</li>
<li>Tiplenmiş bir metod uygun olmadığında, kendi yol değerleriniz, sorgu parametreleriniz, başlıklarınız ve gövdenizle herhangi bir standart işlem için <code>SendAsync</code>.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb istemci kılavuzu</a> her işlem ve seçeneği belgeler, ve <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">istemci işlem referansı</a> tüm metodları listeler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTTP yanıtından piksel verisine tek bir kütüphane">}}

<p>İstemciyle indirilen bir dosya normal bir <code>DicomFile</code>'dır. Aynı süreçte anonimleştirilebilir, dönüştürülebilir, renderlanabilir veya serileştirilebilir; hiçbir dönüşüm adımı ve zincirde ikinci bir kütüphane gerekmez.</p>

<p>İlgili sayfalar: <a href="/medical/net/dicomweb-server/">DICOMweb sunucusu</a> protokolün diğer tarafı için, <a href="/medical/net/dicom-networking/">DICOM ağ iletişimi</a> HTTP yerine DIMSE kullanan arşivler için, <a href="/medical/net/anonymization/">anonimleştirme</a> bir çalışmanın hastaneden çıkmadan önce, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax dönüşümü</a> sunucunun ne depoladığını, ve <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> QIDO-RS arama sonuçlarının kullandığı format için.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokümantasyon" href="https://docs.aspose.com/medical/net/" >}}
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
