---
title: C# .NET'te DICOMweb Sunucusu - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Kendi DICOMweb sunucunuzu ya da PACS'ınızı C# ile oluşturun. Aspose.Medical, ASP.NET Core üzerinde QIDO-RS, WADO-RS, STOW-RS ve iş listesi hizmetlerini uygular; depolamayı bağlarsınız.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb Sunucusu .NET C# için" h2="Bir ASP.NET Core hizmetine QIDO-RS, WADO-RS ve STOW-RS uç noktalarını ekleyin. Kütüphane protokolü uygular; kodunuz çalışmaların nerede saklanacağını belirler." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="C# ile kendi DICOMweb sunucunuz">}}

<p>Bir PACS, bir VNA, bir bulut arşivi ya da bir araştırma platformu DICOMweb isteklerine yanıt vermelidir: QIDO-RS aramaları, WADO-RS indirmeleri ve STOW-RS yüklemeleri, ayrıca çoğu zaman iş listesi hizmetleri de. Bunların her birinin kendi rotaları, medya tipleri, çok parçalı gövdeleri, eşleştirme kuralları ve durum kodları vardır.</p>

<p><strong>Aspose.Medical for .NET</strong> bu protokol yan tarafını 26.10 sürümünden beri uygulamaktadır. Her isteği okur, standarda karşı kontrol eder, arama anahtarlarını DICOM kurallarına göre eşleştirir, sonuçları sayfalar, transfer sözdizimlerini dönüştürür ve yanıtı yazar. Kodunuz yalnızca sizin yapabileceğiniz şeyi yapar: bir örneği depolar, bir arama için adayları bulur ve depolanan dosyaları döndürür. Görüntüleyiciler, modallar ve diğer arşivler, sunucunuzla diğer DICOMweb sunucuları gibi çalışır ve aynı kütüphanenin <a href="/medical/net/dicomweb-client/">DICOMweb client</a>ı da öyle çalışır.</p>

<p>Sunucu yönetilen C#'tır. ASP.NET Core'un çalıştığı her yerde çalışır: Windows'ta, Linux'ta ve bir konteyner içinde.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ASP.NET Core'da tam bir sunucu">}}

<p><code>Aspose.Medical.Dicom.Web.AspNetCore</code> paketini kurun. Bu paket Aspose.Medical'i beraberinde getirir ve DICOMweb hizmetlerini bağımlılık enjeksiyonu ve uç nokta yönlendirmesine bağlar. Bu <code>Program.cs</code> çalışan bir Studies sunucusudur; kaydettiği iki sınıf aşağıda gösterilmiştir.</p>

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

<p>Her Studies rotası artık <code>/dicomweb</code> altında yanıt verir: arama ve yükleme için <code>/dicomweb/studies</code>, alım için <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code>, ayrıca metadata, kareler, toplu veri ve oluşturulmuş görüntüler için rotalar. Servis kökündeki yetenekler belgesi, sunucunun desteklediği işlemleri tam olarak listeler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Depolamanızı bağlayın">}}

<p>Studies hizmeti üç metoda sahip bir arayüze ihtiyaç duyar: bir örneği depolamak, depolanan dosyaları döndürmek ve arama adaylarını döndürmek. Bu uygulama her örneği çalışma ve seri başına bir klasörde dosya olarak tutar.</p>

<div class="codeblock" id="code">
 <h3>QIDO-RS, WADO-RS ve STOW-RS arkasında klasör arşivi - C#</h3>
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

<p>Sunucuyu denemek için bir klasör yeterlidir. Üretim arşivi aynı üç metodu korur ve <code>StoreAsync</code> ve <code>RetrieveAsync</code> arkasına bir nesne deposu, <code>FindCandidatesAsync</code> arkasına bir veritabanı indeksi yerleştirir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Arama: kütüphane DICOM eşleştirme kurallarını uygular">}}

<p>QIDO-RS eşleştirme yalnızca dize karşılaştırmasından fazlasıdır: joker karakterler, tarih ve zaman aralıkları, UID listeleri, bulanık kişi adları, boş değerler ve diziler içindeki anahtarlar. Sağlayıcı bunların hiç birini uygulamaz. Adayları sabit bir sırada döndürür ve eşleşmeden daha fazla döndürebilir. Kütüphane her adaya kesin DICOM eşleşmesini uygular, istenen alanları korur ve <code>offset</code> ve <code>limit</code> uygular. Bir yanıt varsayılan olarak en fazla 1000 sonuç tutar ve <code>DicomWebStandardServiceOptions</code> bu limiti değiştirir.</p>

<p>Büyük bir arşivde aramaların hızlı kalmasını sağlamak için seçici anahtarları veritabanına iletin. <code>search.Plan.Filter</code>, derlenmiş arama anahtarlarını, özniteliği, eşleşme türünü ve her birinin değerlerini tutar:</p>

<div class="codeblock" id="code">
 <h3>Bir aramadan kesin Patient ID okuma - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Bu şekilde oluşturulan bir veritabanı sorgusu ek satırlar döndürebilir, örneğin çeviremediği bir anahtarı yok saydığında. Kütüphanedeki son eşleşme bunları kaldırır. Gerçek eşleşmeleri düşürmemelidir, çünkü kütüphane onları geri ekleyemez.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Depolama: her örnek için bir sonuç">}}

<p>Bir STOW-RS isteği yüzlerce örnek taşıyabilir. Kütüphane çok parçalı gövdeyi ayrıştırır ve her bir örnek için sırayla <code>StoreAsync</code>'i çağırır, böylece tüm yükleme bir kerede bellekte tutulmaz. Her örnek için sağlayıcı <code>Stored</code>, değişiklikle depolanmış bir örnek için <code>Warning</code> veya isteğe bağlı bir DICOM neden kodu ile <code>Failed</code> döndürür.</p>

<p>Kütüphane bu sonuçlardan STOW-RS raporunu oluşturur ve durumu seçer: her örnek depolandığında 200, bazıları uyarı alır ya da başarısız olursa 202, tümü başarısız olursa 409. Başka bir çalışma ait olan ve tek bir isimli çalışma için gelen istekteki örnekler sağlayıcı çağırılmadan reddedilir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Alma: transfer sözdizimleri, kareler ve oluşturulmuş görüntüler">}}

<p>Sağlayıcı depolanmış dosyaları döndürür ve kütüphane bunları istemcinin talep ettiği biçimde sunar. Bir WADO-RS istemcisi <code>Accept</code> başlığında bir transfer sözdizimi isteyebilir, ve kütüphane her dosyayı göndermeden önce kendi codec'leri, HTJ2K ve JPEG XL dahil, dönüştürür. DICOM JSON veya XML metadata'sı, toplu veri ve tek kareler aynı sağlayıcıdan gelir, ek kod gerekmez.</p>

<p>Oluşturulmuş görüntüler bir sınıf daha gerektirir, çünkü yalnızca siz sunucunun hangi görüntüleri üretebileceğine karar verirsiniz. Bu sınıf oluşturulmuş örnekler, oluşturulmuş kareler ve küçük görseller için JPEG sunar ve kütüphane bunları oluşturur:</p>

<div class="codeblock" id="code">
 <h3>Oluşturulmuş JPEG görüntüleri ve küçük görselleri sun - C#</h3>
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

<p>Yerleşik renderleyici tek kareli bir örnekten ya da isteğin seçtiği karelerden JPEG üretir. Tüm bir oluşturulmuş çok kareli örnek, diğer görüntü formatları ve görüntü alanı, pencere ve ICC profil parametreleri bu sınıfta kendi <code>RetrieveAsync</code>'inizi gerektirir.</p>

<p>Küçük görseller, hasta gizliliği göz önünde bulundurularak işlenir. Yerleşik renderleyici, yalnızca Yanmış Anotasyon ve Tanımlanabilir Görsel Özellikler öznitelikleri <code>NO</code> olan bir örnekten küçük görsel oluşturur; bu, görüntünün hasta verisi göstermediğini beyan ettiği anlamına gelir. Birçok görüntü bu özniteliklere sahip değildir ve bunlar için sunucu 406 yanıtı verir. Arşivinizin bu tür görüntüler için küçük görsellere ihtiyacı varsa, kendi <code>RetrieveAsync</code>'inizde üretin ve hasta verilerini gizlediğinden emin olun.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="İş listeleri, prosedür adımları ve diğer hizmetler">}}

<p>Diğer DICOMweb hizmetleri aynı modeli izler: hizmeti kaydedin, depolama arayüzünü uygulayın.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> iş listeleri <code>AddStandardWorklistService</code> ile: iş öğelerini oluşturma, arama, güncelleme ve durumlarını değiştirme; durum kuralları kütüphane tarafından kontrol edilir ve olay bildirimleri WebSocket üzerinden gönderilir.</li>
<li><a href="/medical/net/modality-worklist/">Modality worklist</a> (MWL-RS) <code>AddStandardModalityWorklistService</code> ile, modallar RIS'inizin planlanmış prosedür adımlarını sorgulayabilir.</li>
<li>Modality performed procedure steps (MPPS-RS), depolama taahhüdü ve renk paletleri ve asma protokolleri gibi hasta dışı örnekler.</li>
<li>Eski görüntüleyiciler için WADO-URI alımı, Studies hizmeti tarafından sunulur.</li>
</ul>

<p>Bir işlemin standart davranışı uymadığında, onun için <code>AddHandler</code> ile kendi işleyicinizi kaydedin. Kütüphane hâlâ isteği yönlendirir ve kontrol eder, ve diğer işlemler standart işleyicilerini tutar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Güvenlik ve barındırma sizin elinizde kalır">}}

<p>DICOMweb uç noktaları normal ASP.NET Core uç noktalarıdır. Kimlik doğrulama, yetkilendirme, CORS, TLS, yönlendirilmiş başlıklar ve istek boyutu limitleri, hizmetinizin geri kalanında olduğu gibi yapılandırılır:</p>

<div class="codeblock" id="code">
 <h3>Kimliği doğrulanmış bir kullanıcı iste - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Protokol bölümü ASP.NET Core'a bağımlı değildir. <code>DicomWebApplication</code>, bir <code>DicomWebRequest</code>'i yanıt haline getirir, böylece aynı hizmetler başka bir HTTP sunucusunun arkasında ya da ağ olmadan bir testte çalışabilir:</p>

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

<p>Aspose.Medical'in <a href="/medical/net/dicomweb-client/">DICOMweb client</a>ı, sunucunuzu bir birim testi veya konsol programı üzerinden test eder ve sunucunun aldığı her dosya aynı süreçte anonimleştirilebilen, yeniden kodlanabilen veya oluşturulabilen normal bir <code>DicomFile</code>'dır.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb geliştirici rehberi</a> her hizmet ve depolama arayüzünü kapsar, ve <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core rehberi</a> uç nokta eşlemesini ve WebSocket'leri kapsar.</p>

<p>İlgili sayfalar: <a href="/medical/net/dicom-networking/">DICOM ağ iletişimi</a> HTTP yerine DIMSE üzerinden çalışma gönderen sistemler için, gelen çalışmalarda <a href="/medical/net/anonymization/">anonimleştirme</a>, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer sözdizimi dönüşümü</a> ve QIDO-RS sonuçlarının kullandığı format için <a href="/medical/net/dicom-to-json/">DICOM'dan JSON'a</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Belgelendirme" href="https://docs.aspose.com/medical/net/" >}}
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
