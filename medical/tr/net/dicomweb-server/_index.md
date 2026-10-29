---
title: C# .NET'te DICOMweb Sunucusu - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Kendi DICOMweb sunucunuzu veya PACS'ınızı C# ile oluşturun. Aspose.Medical, ASP.NET Core üzerinde QIDO-RS, WADO-RS, STOW-RS ve iş listesi hizmetlerini uygular; depolamayı bağlarsınız.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# için DICOMweb Sunucusu" h2="Bir ASP.NET Core servisine QIDO-RS, WADO-RS ve STOW-RS uç noktalarını ekleyin. Kütüphane protokolü konuşur; kodunuz çalışmaların nerede saklanacağını belirler." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="C# ile kendi DICOMweb sunucunuz">}}

<p>Bir PACS, VNA, bulut arşivi veya araştırma platformu DICOMweb isteklerine yanıt vermelidir: QIDO-RS aramaları, WADO-RS indirmeleri ve STOW-RS yüklemeleri, ayrıca sık sık iş listesi hizmetleri. Her birinin kendi rotaları, medya tipleri, çok parçalı gövdeleri, eşleştirme kuralları ve durum kodları vardır.</p>

<p><strong>Aspose.Medical for .NET</strong> bu protokol tarafını sürüm 26.10'dan beri uygular. Her isteği okur, standarda karşı kontrol eder, DICOM kurallarına göre arama anahtarlarını eşleştirir, sonuçları sayfalara ayırır, transfer syntax'larını dönüştürür ve yanıtı yazar. Sizin kodunuz sadece sizin yapabileceğiniz şeyi yapar: bir örneği depolar, bir arama için adayları bulur ve depolanan dosyaları geri döndürür. Görüntüleyiciler, modaliteler ve diğer arşivler, sunucunuzla diğer DICOMweb sunucuları gibi çalışır ve aynı kütüphanenin <a href="/medical/net/dicomweb-client/">DICOMweb istemcisi</a> de aynı şekilde çalışır.</p>

<p>Sunucu yönetilen C#'tır. ASP.NET Core'un çalıştığı her yerde çalışır: Windows'ta, Linux'ta ve bir konteyner içinde.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ASP.NET Core'da Tam Bir Sunucu">}}

<p><code>Aspose.Medical.Dicom.Web.AspNetCore</code> paketini kurun. Bu paket Aspose.Medical'i getirir ve DICOMweb hizmetlerini bağımlılık enjeksiyonu ve uç nokta yönlendirmesine bağlar. Bu <code>Program.cs</code> çalışan bir Studies sunucusudur; kaydettiği iki sınıf aşağıda gösterilmiştir.</p>

<div class="codeblock" id="code">
 <h3>DICOMweb sunucusunun Program.cs - C#</h3>
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

<p>Artık her Studies yolu <code>/dicomweb</code> altında yanıt verir: arama ve yükleme için <code>/dicomweb/studies</code>, geri alma için <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code>, ayrıca metadata, kareler, toplu veri ve oluşturulan görüntüler için yollar. Servis kökündeki yetenekler belgesi, sunucunun desteklediği işlemleri tam olarak listeler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Depolamanızı Bağlayın">}}

<p>Studies hizmeti, bir örneği depolamak, depolanan dosyaları döndürmek ve arama adaylarını döndürmek için üç yönteme sahip bir arayüz gerekir. Bu uygulama, her örneği çalışma ve seri başına bir klasörde dosya olarak tutar.</p>

<div class="codeblock" id="code">
 <h3>QIDO-RS, WADO-RS ve STOW-RS arkasındaki klasör arşivi - C#</h3>
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

<p>Kütüphane, sağlayıcıyı çağırmadan önce her istek yolundaki ve her yüklenen örnekteki UID'leri kontrol eder. Geçerli UID'leri olmayan bir örnek başarısız olarak raporlanır ve <code>StoreAsync</code>'e asla ulaşmaz.</p>

<p>Sunucuyu denemek için bir klasör yeterlidir. Üretim arşivi aynı üç yöntemi tutar ve <code>StoreAsync</code> ve <code>RetrieveAsync</code> arkasına bir nesne deposu, <code>FindCandidatesAsync</code> arkasına bir veritabanı indeksi koyar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Arama: kütüphane DICOM eşleştirme kurallarını uygular">}}

<p>QIDO-RS eşleştirme, sadece dize karşılaştırmasından daha fazlasıdır: joker karakterler, tarih ve saat aralıkları, UID listeleri, bulanık kişi adları, boş değerler ve diziler içindeki anahtarlar. Sağlayıcı bunların hiçbirini uygulamaz. Adayları sabit bir sırada döndürür ve eşleşmeden daha fazlasını döndürebilir. Kütüphane her adaya tam DICOM eşleşmesini uygular, istenen alanları tutar ve <code>offset</code> ve <code>limit</code> uygular. Bir yanıt varsayılan olarak en fazla 1000 sonuç içerir ve <code>DicomWebStandardServiceOptions</code> bu limiti değiştirir.</p>

<p>Geniş bir arşivde aramaları hızlı tutmak için seçici anahtarları veritabanına aktarın. <code>search.Plan.Filter</code> derlenmiş arama anahtarlarını, özniteliği, eşleştirme türünü ve her birinin değerlerini tutar:</p>

<div class="codeblock" id="code">
 <h3>Aramadan kesin bir Hasta Kimliği oku - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Bu şekilde oluşturulan bir veritabanı sorgusu, örneğin çeviremeyeceği bir anahtarı görmezden geldiğinde ekstra satırlar döndürebilir. Kütüphanedeki son eşleşme bunları kaldırır. Gerçek eşleşmeleri düşürmemelidir, çünkü kütüphane onları geri ekleyemez.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Depolama: her örnek için bir sonuç">}}

<p>Bir STOW-RS isteği yüzlerce örnek taşıyabilir. Kütüphane çok parçalı gövdeyi ayrıştırır ve her örnek için sırayla <code>StoreAsync</code>'i çağırır, böylece tüm yükleme aynı anda bellekte tutulmaz. Her örnek için sağlayıcı <code>Stored</code>, değişiklikle depolanmış bir örnek için <code>Warning</code> veya isteğe bağlı bir DICOM neden kodu ile <code>Failed</code> döndürür.</p>

<p>Kütüphane bu sonuçlardan STOTR-RS raporunu oluşturur ve durumu seçer: tüm örnekler depolandığında 200, bazılarında uyarı veya başarısız olduğunda 202, tümü başarısız olduğunda 409. Başka bir çalışmaya ait olduğu bir isimli çalışma isteğindeki örnekler, sağlayıcı çağrılmadan önce reddedilir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Alma: transfer syntax'lar, kareler ve oluşturulan görüntüler">}}

<p>Sağlayıcı depolanan dosyaları döndürür ve kütüphane bunları istemcinin istediği biçimde sunar. Bir WADO-RS istemcisi <code>Accept</code> başlığında bir transfer syntax'ı isteyebilir ve kütüphane her dosyayı göndermeden önce kendi codec'leri, HTJ2K ve JPEG XL dahil olmak üzere dönüştürür. DICOM JSON veya XML biçimindeki metadata, toplu veri ve tek kareler aynı sağlayıcıdan gelir, ekstra kod gerektirmez.</p>

<p>Oluşturulan görüntüler bir sınıf daha gerektirir, çünkü sadece siz sunucunun hangi görüntüleri üretebileceğine karar verirsiniz. Bu sınıf, oluşturulan örnekler, oluşturulan kareler ve küçük resimler için JPEG sunar ve kütüphane bunları oluşturur:</p>

<div class="codeblock" id="code">
 <h3>Oluşturulan JPEG görüntüler ve küçük resimler sun - C#</h3>
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

<p>Yerleşik renderleyici, tek kareli bir örnekten veya bir isteğin seçtiği karelerden JPEG üretir. Tüm olarak oluşturulan çok kareli bir örnek, diğer görüntü formatları ve görüntüleme alanı, pencere ve ICC profili parametreleri bu sınıfta kendi <code>RetrieveAsync</code> metodunuzu gerektirir.</p>

<p>Küçük resimler hasta gizliliği için özenle işlenir. Yerleşik renderleyici, Yanmış Anotasyon ve Tanınabilir Görsel Özellikler öznitelikleri <code>NO</code> olan bir örnekten sadece küçük resim oluşturur; bu, görüntünün hasta verisi göstermediğini bildirir. Birçok görüntü bu özniteliklere sahip değildir ve bu durumda sunucu 406 yanıt verir. Arşivinizin bu tür görüntüler için küçük resimlere ihtiyacı varsa, bunları kendi <code>RetrieveAsync</code> metodunuzda üretin ve hasta verilerini gizlediğinden emin olun.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="İş Listeleri, Prosedür Adımları ve Diğer Hizmetler">}}

<p>Diğer DICOMweb hizmetleri aynı modeli izler: hizmeti kaydedin, depolama arayüzünü uygulayın.</p>

<ul>
<li>UPS-RS iş listeleri <code>AddStandardWorklistService</code> ile: iş öğelerini oluşturun, arayın ve güncelleyin ve durumlarını değiştirin; durum kuralları kütüphane tarafından kontrol edilir ve olay bildirimleri bir WebSocket üzerinden gönderilir.</li>
<li>Modality iş listesi (MWL-RS) <code>AddStandardModalityWorklistService</code> ile, böylece modaliteler RIS'inizde planlanan prosedür adımlarını sorgulayabilir.</li>
<li>Gerçekleştirilen modality prosedür adımları (MPPS-RS), depolama taahhüdü ve renk paletleri ile asılı protokoller gibi hasta olmayan örnekler.</li>
<li>Eski görüntüleyiciler için WADO-URI alma, Studies hizmeti tarafından sunulur.</li>
</ul>

<p>Bir işlemin standart davranışı uymadığında, <code>AddHandler</code> ile kendi işleyicinizi kaydedin. Kütüphane hâlâ isteği yönlendirir ve kontrol eder, diğer işlemler standart işleyicilerini korur.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Güvenlik ve barındırma sizin kontrolünüzde">}}

<p>DICOMweb uç noktaları sıradan ASP.NET Core uç noktalarıdır. Kimlik doğrulama, yetkilendirme, CORS, TLS, yönlendirilmiş başlıklar ve istek boyut sınırlamaları, hizmetinizin geri kalanıyla aynı şekilde yapılandırılır:</p>

<div class="codeblock" id="code">
 <h3>Kimliği doğrulanmış bir kullanıcı gerektir - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Protokol kısmı ASP.NET Core'a bağlı değildir. <code>DicomWebApplication</code>, bir <code>DicomWebRequest</code>'i yanıt haline getirir, böylece aynı hizmetler başka bir HTTP sunucusunun arkasında veya ağ olmadan bir testte çalışabilir.</p>

<div class="codeblock" id="code">
 <h3>Aynı sunucu ASP.NET Core olmadan - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tek bir kütüphaneden istemci ve sunucu">}}

<p>Aspose.Medical'in <a href="/medical/net/dicomweb-client/">DICOMweb istemcisi</a>, sunucunuzu bir birim testi ya da konsol programı üzerinden test eder ve sunucunun aldığı her dosya aynı işlem içinde anonimleştirilebilen, kodu dönüştürülebilen veya render edilebilen normal bir <code>DicomFile</code>'dır.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb geliştirici rehberi</a> her hizmet ve depolama arayüzünü kapsar ve <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core rehberi</a> uç nokta eşlemesini ve WebSocket'leri kapsar.</p>

<p>İlgili sayfalar: <a href="/medical/net/dicom-networking/">DICOM ağ iletişimi</a> HTTP yerine DIMSE üzerinden çalışma gönderen sistemler için, gelen çalışmalarda <a href="/medical/net/anonymization/">anonimleştirme</a>, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax dönüşümü</a> ve QIDO-RS sonuçlarının kullandığı biçim için <a href="/medical/net/dicom-to-json/">DICOM'dan JSON'a</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokümantasyon" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Geliştirici Rehberi" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
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
