---
title: C# .NET'te DICOMweb Sunucusu - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Kendi DICOMweb sunucunuzu veya PACS'nizi C#'ta oluşturun. Aspose.Medical, ASP.NET Core üzerinde QIDO-RS, WADO-RS, STOW-RS ve iş listesi servislerini uygular; depolamayı bağlarsınız.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# için DICOMweb Sunucusu" h2="Bir ASP.NET Core servisine QIDO-RS, WADO-RS ve STOW-RS uç noktalarını ekleyin. Kütüphane protokolü konuşur; kodunuz çalışmaları nerede saklayacağını belirler." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="C#'ta kendi DICOMweb sunucunuz">}}

<p>Bir PACS, VNA, bulut arşivi veya araştırma platformu DICOMweb isteklerine yanıt vermelidir: QIDO-RS aramaları, WADO-RS indirmeleri ve STOW-RS yüklemeleri, ayrıca sık sık iş listesi servisleri de. Her biri kendi yollarına, medya tiplerine, multipart gövdelerine, eşleştirme kurallarına ve durum kodlarına sahiptir.</p>

<p><strong>Aspose.Medical for .NET</strong> bu protokol tarafını 26.10 sürümünden itibaren uygular. Her isteği okur, standarda karşı kontrol eder, DICOM kurallarına göre arama anahtarlarını eşleştirir, sonuçları sayfalara ayırır, transfer syntax'lerini dönüştürür ve yanıtı yazar. Kodunuz yalnızca sizin yapabileceğiniz şeyi yapar: bir örneği depolar, bir arama için adayları bulur ve depolanan dosyaları döndürür. Görüntüleyiciler, modaliteler ve diğer arşivler daha sonra sunucunuzla diğer DICOMweb sunucuları gibi çalışır ve aynı kütüphanenin <a href="/medical/net/dicomweb-client/">DICOMweb istemcisi</a> de aynı şekilde çalışır.</p>

<p>Sunucu yönetilen C#'tır. ASP.NET Core'un çalıştığı her yerde çalışır: Windows'ta, Linux'ta ve bir konteyner içinde.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ASP.NET Core'da tam bir sunucu">}}

<p><code>Aspose.Medical.Dicom.Web.AspNetCore</code> paketini yükleyin. Aspose.Medical'i beraberinde getirir ve DICOMweb servislerini bağımlılık enjeksiyonu ve uç nokta yönlendirmesine bağlar. Bu <code>Program.cs</code> çalışan bir Studies sunucusudur; kaydettiği iki sınıf aşağıda gösterilmiştir.</p>

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

<p>Her Studies yolu artık <code>/dicomweb</code> altında yanıt verir: arama ve yükleme için <code>/dicomweb/studies</code>, getirme için <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code>, ayrıca metadata, çerçeveler, toplu veri ve oluşturulmuş görüntüler için yollar. Servis kökündeki yetenekler belgesi, sunucunun desteklediği işlemleri tam olarak listeler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Depolamanızı bağlayın">}}

<p>Studies servisi, bir örneği depolama, depolanan dosyaları döndürme ve arama adaylarını döndürme olmak üzere üç metoda sahip bir arayüz gerektirir. Bu uygulama, her örneği çalışmaya ve seriye göre bir klasörde dosya olarak tutar.</p>

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

<p>Kütüphane, sağlayıcıyı çağırmadan önce her istek yolundaki ve yüklenen her örnekteki UID'leri kontrol eder. Geçerli UID'leri olmayan bir örnek başarısız olarak raporlanır ve <code>StoreAsync</code>'e asla ulaşmaz.</p>

<p>Sunucuyu denemek için bir klasör yeterlidir. Üretim arşivi aynı üç metodu tutar ve <code>StoreAsync</code> ve <code>RetrieveAsync</code> arkasına bir nesne deposu, <code>FindCandidatesAsync</code> arkasına bir veritabanı indeksi koyar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Arama: kütüphane DICOM eşleştirme kurallarını uygular">}}

<p>QIDO-RS eşleştirmesi yalnızca dize karşılaştırmasından daha fazlasıdır: joker karakterler, tarih ve zaman aralıkları, UID listeleri, belirsiz kişi adları, boş değerler ve diziler içindeki anahtarlar. Sağlayıcı bunların hiç birini uygulamaz. Adayları istikrarlı bir sırada döndürür ve eşleşmeden daha fazla döndürebilir. Kütüphane, her adaya tam DICOM eşlemesini uygular, istenen alanları korur ve <code>offset</code> ve <code>limit</code> uygular. Bir yanıt varsayılan olarak en fazla 1000 sonuç tutar ve <code>DicomWebStandardServiceOptions</code> bu sınırlamayı değiştirir.</p>

<p>Büyük bir arşivde aramaları hızlı tutmak için seçici anahtarları veritabanına gönderin. <code>search.Plan.Filter</code>, derlenmiş arama anahtarlarını, özniteliği, eşleştirme türünü ve her birinin değerlerini tutar:</p>

<div class="codeblock" id="code">
 <h3>Aramadan kesin bir Hasta ID'si okuyun - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Bu şekilde oluşturulan bir veritabanı sorgusu ek satırlar döndürebilir, örneğin çeviremediği bir anahtarı yoksaydığı zaman. Kütüphanedeki son eşleşme bunları kaldırır. Gerçek eşleşmeleri düşürmemelidir, çünkü kütüphane bunları geri ekleyemez.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Depolama: her örnek için bir sonuç">}}

<p>Bir STOW-RS isteği yüzlerce örnek taşıyabilir. Kütüphane multipart gövdesini ayrıştırır ve sırayla her örnek için <code>StoreAsync</code>'i çağırır, böylece tüm yükleme bir kerede bellekte tutulmaz. Her örnek için sağlayıcı <code>Stored</code>, değişiklikle depolanmış bir örnek için <code>Warning</code> ya da isteğe bağlı bir DICOM neden kodu ile <code>Failed</code> döndürür.</p>

<p>Kütüphane bu sonuçlardan STOW-RS raporunu oluşturur ve durumu seçer: tüm örnekler depolandığında 200, bazıları uyarı alır veya başarısız olduğunda 202, hepsi başarısız olduğunda 409. Başka bir çalışmaya ait olan, adlandırılmış bir çalışma isteğinde bulunan örnekler sağlayıcı çağrılmadan reddedilir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Getirme: transfer syntax'leri, çerçeveler ve oluşturulmuş görüntüler">}}

<p>Sağlayıcı depolanan dosyaları döndürür ve kütüphane bunları istemcinin talep ettiği biçimde sunar. Bir WADO-RS istemcisi <code>Accept</code> başlığında bir transfer syntax'i isteyebilir ve kütüphane her dosyayı göndermeden önce, kendi codec'leri, HTJ2K ve JPEG XL dahil olmak üzere, dönüştürür. DICOM JSON veya XML şeklinde metadata, toplu veri ve tek çerçeveler aynı sağlayıcıdan gelir, ekstra bir kod gerektirmez.</p>

<p>Oluşturulmuş görüntüler bir sınıf daha gerektirir, çünkü sadece siz sunucunun hangi görüntüleri üretebileceğine karar verirsiniz. Bu sınıf, oluşturulmuş örnekler, çerçeveler ve küçük resimler için JPEG sunar ve kütüphane bunları oluşturur:</p>

<div class="codeblock" id="code">
 <h3>Oluşturulmuş JPEG görüntüleri ve küçük resimler sunun - C#</h3>
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

<p>Yerleşik renderleyici, tek çerçeve örneğinden veya bir isteğin seçtiği çerçevelerden JPEG üretir. Tam bir çok-çerçeveli örnek, 65500 pikselden daha geniş veya yüksek bir görüntü (küçük resimler bu tür görüntülerin üzerinden geçer), diğer görüntü formatları ve görünüm penceresi, pencere ve ICC profili parametreleri bu sınıfta kendi <code>RetrieveAsync</code> kodunuzu gerektirir.</p>

<p>Küçük resimler, hasta gizliliği açısından özenle işlenir. Yerleşik renderleyici, yalnızca Burned In Annotation ve Recognizable Visual Features öznitelikleri <code>NO</code> olan bir örnekten küçük bir resim oluşturur; bu, görüntünün hasta verisi göstermediğini beyan ettiği anlamına gelir. Birçok görüntü bu özniteliklere sahip değildir ve bu durumda sunucu 406 yanıt verir. Arşivinizin bu tür görüntüler için küçük resimlere ihtiyacı varsa, bunları kendi <code>RetrieveAsync</code> kodunuzla üretin ve herhangi bir hasta verisini gizlediğinden emin olun.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="İş listeleri, prosedür adımları ve diğer servisler">}}

<p>Diğer DICOMweb servisleri aynı modeli takip eder: servisi kaydedin, depolama arayüzünü uygulayın.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> iş listeleri <code>AddStandardWorklistService</code> ile: iş öğelerini oluşturun, arayın ve güncelleyin ve durumlarını değiştirin; durum kuralları kütüphane tarafından kontrol edilir ve olay bildirimleri bir WebSocket üzerinden gönderilir.</li>
<li><a href="/medical/net/modality-worklist/">Modalite iş listesi</a> (MWL-RS) <code>AddStandardModalityWorklistService</code> ile, modaliteler RIS'inizin planlanmış prosedür adımlarını sorgulayabilir.</li>
<li>Modalite gerçekleştirilen prosedür adımları (MPPS-RS), depolama taahhüdü ve renk paletleri ve asılı protokoller gibi hasta dışı örnekler.</li>
<li>Eski görüntüleyiciler için WADO-URI getirme, Studies servisi tarafından sunulur.</li>
</ul>

<p>Bir işlemin standart davranışı uymadığında, <code>AddHandler</code> ile kendi işleyicinizi kaydedin. Kütüphane hâlâ isteği yönlendirir ve kontrol eder, diğer işlemler ise standart işleyicilerini korur.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Güvenlik ve barındırma sizin elinizde">}}

<p>DICOMweb uç noktaları sıradan ASP.NET Core uç noktalarıdır. Kimlik doğrulama, yetkilendirme, CORS, TLS, yönlendirilmiş başlıklar ve istek boyutu sınırlamaları, hizmetinizin geri kalanında olduğu gibi aynı şekilde yapılandırılır:</p>

<div class="codeblock" id="code">
 <h3>Kimlik doğrulamalı bir kullanıcı isteyin - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Protokol bölümü ASP.NET Core'a bağlı değildir. <code>DicomWebApplication</code>, bir <code>DicomWebRequest</code>'i yanıt haline getirir, böylece aynı servisler başka bir HTTP sunucusunun arkasında veya ağ olmadan bir testte çalışabilir.</p>

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

<p>Aspose.Medical'in <a href="/medical/net/dicomweb-client/">DICOMweb istemcisi</a>, sunucunuzu bir birim testi veya konsol programından test eder ve sunucunun aldığı her dosya, aynı süreçte anonimleştirilebilen, kodu değiştirilebilen veya renderlanabilen normal bir <code>DicomFile</code>'dır.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb geliştirici rehberi</a>, her hizmet ve depolama arayüzünü kapsar ve <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core rehberi</a> uç nokta haritalamasını ve WebSocket'leri kapsar.</p>

<p>İlgili sayfalar: HTTP yerine DIMSE üzerinden çalışma gönderimi yapan sistemler için <a href="/medical/net/dicom-networking/">DICOM ağ iletişimi</a>, gelen çalışmaların <a href="/medical/net/anonymization/">anonimleştirilmesi</a>, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax dönüşümü</a> ve QIDO-RS sonuçlarının kullandığı format için <a href="/medical/net/dicom-to-json/">DICOM'dan JSON'a</a>.</p>

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

{{< blocks/products/pf/slr-tab tabTitle="Neden Aspose.Medical .NET için?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Müşteri Listesi" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Başarı Hikayeleri" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
