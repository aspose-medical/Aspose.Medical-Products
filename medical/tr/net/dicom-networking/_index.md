---
title: C# .NET'te DICOM Ağ Bağlantısı - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: .NET uygulamanızı bir PACS'e bağlayın. C-ECHO ile doğrulayın, C-STORE ile görüntü gönderin, C-FIND ile sorgulayın ve kendi SCP'nizle görüntü alın. Saf C#'ta bir DIMSE istemcisi ve sunucusu.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="C# .NET'te DICOM Ağ Bağlantısı" h2="Kendi uygulamanızdan bir PACS ile iletişim kurun: C-ECHO, C-STORE, C-FIND, C-MOVE ve C-GET, istemci ve sunucu olarak, yönetilen C# ile ve makineye hiçbir şey kurmanıza gerek olmadan." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Kendi kodunuzdan bir PACS'e bağlanın">}}

<p>DICOM dosyalarını okumak tıbbi görüntünün kolay yarısıdır. Uygulamanızın gerçek bir hastane sistemiyle çalışması gerektiği anda DIMSE konuşmalıdır: bir PACS ile ilişki (association) açın, görüntüleri gönderin, hangi çalışmaların olduğunu sorun ve başka bir sistem bir şey gönderdiğinde yanıt verin.</p>

<p><strong>Aspose.Medical for .NET</strong> bu protokolü kütüphanenin bir parçası olarak sunar. <code>Aspose.Medical.Dicom.Network</code> size bir DIMSE istemcisi ve bir DIMSE sunucusu sağlar, her ikisi de yönetilen C# ile yazılmıştır. Kurulacak yerel bir araç takımı, yapılandırılacak bir hizmet veya platforma özel bir şey yoktur, bu yüzden aynı kod Windows, Linux ve bir kapsayıcı içinde çalışır.</p>

<p>Çoğu entegrasyonu kapsayan üç şey vardır ve her biri birkaç satır kod içerir: bağlantıyı C-ECHO ile kontrol edin, görüntüleri C-STORE ile gönderin ve diğer tarafta ne olduğunu C-FIND ile bulun.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-ECHO ile Başlayın">}}

<p>C-ECHO, DICOM ping'idir. Başka bir şeyden önce host, port ve iki AE başlığının doğru olduğunu kanıtlar. Bir istemciyi bir kez oluşturun, yanıtı gözlemleyen bir işleyici (handler) atayın ve isteği gönderin.</p>

<div class="codeblock" id="code">
 <h3>C-ECHO ile Bağlantıyı Doğrulama - C#</h3>
 <pre><code class="cs">AssociationNegotiationOptions negotiation = new AssociationNegotiationOptions()
    .WithPresentationContext(new PresentationContext
    {
        AbstractSyntax = Uid.Verification,
        Role = null,
        TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
    });

DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = negotiation
    })
    .AddCEchoHandler((request, response, cancellationToken) =>
    {
        Console.WriteLine($"C-ECHO answered with status 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(new CEchoRequest());

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Durum <code>0x0000</code> başarı anlamına gelir. İstekler kuyruğa alınır ve tek bir ilişki üzerinden gönderilir, bu sayede bir iş paketi her öğe için ayrı bir bağlantı açmaz.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-STORE ile Görüntü Gönderme">}}

<p>C-STORE, bir uygulamanın bir görüntü ürettikten veya aldıktan sonra yaptığı işlemdir: örneği arşive gönderir. Her örnek için bir istek kuyruğuna ekleyin ve birlikte gönderin.</p>

<div class="codeblock" id="code">
 <h3>Bir DICOM dosyasını PACS'e gönder - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Önerdiğiniz sunum bağlamları (presentation contexts), arşivin neyi kabul edeceğini belirler. Eğer sıkıştırılmış bir sözdizimi (syntax) istiyorsa, gönderimden önce dönüştürün, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> sayfasının gösterdiği gibi, ya da alternatifleri <code>AdditionalTransferSyntaxes</code> içinde listeleyerek müzakerenin birini seçmesine izin verin.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-FIND ile Çalışmaları Bulma">}}

<p>C-FIND, "arşivde ne var" sorusunu yanıtlar. Eşleşmeler tek tek gelir, her biri kendi tanımlayıcı veri setine (identifier dataset) sahiptir ve son yanıt sorguyu kapatır.</p>

<div class="codeblock" id="code">
 <h3>Hasta bazında çalışmalar sorgulama - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.StudyRootQueryRetrieveInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddCFindHandler((request, response, cancellationToken) =>
    {
        // A match arrives with an identifier; the final response carries the status only
        if (response.Identifier is not null)
            Console.WriteLine(response.Identifier.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty));

        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(CFindRequest.CreateStudyQuery(
    patientId: "PATIENT-001",
    patientName: null,
    studyDateTime: null,
    accession: null,
    studyId: null,
    modalitiesInStudy: null,
    studyInstanceUid: null,
    priority: DimsePriority.Medium));

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Aynı fabrika diğer sorgu seviyelerini oluşturur: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> ve <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code>, bir modalitenin tarama öncesinde sorduğu çalışma listesi sorgusunu oluşturur.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Görüntü Alımı: Kendi Store SCP'niz">}}

<p>Kütüphane aynı zamanda bir sunucudur. Sunmak istediğiniz hizmet için bir işleyici (handler) kaydedin, dinlemeyi başlatın ve uygulamanız bir modalite ya da başka bir PACS'in gönderim yapabileceği bir DICOM düğümü haline gelir.</p>

<div class="codeblock" id="code">
 <h3>Gelen Görüntüleri Kabul Et - C#</h3>
 <pre><code class="cs">DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Any, 11112)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.SecondaryCaptureImageStorage,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddSingletonCStoreHandler(new StoreHandler())
    .Build();

await server.StartAsync(CancellationToken.None);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Gelenleri depolayan işleyici - C#</h3>
 <pre><code class="cs">public sealed class StoreHandler : ICStoreRequestHandler
{
    public ValueTask&lt;CStoreResponse&gt; Handle(CStoreRequest request, CancellationToken cancellationToken)
    {
        // Write what arrived, then answer Success
        new DicomFile(request.Dataset).Save($"{request.AffectedSopInstanceUid}.dcm");

        CStoreResponse response = new();
        response.Command.AddOrUpdate(Tag.Status, (ushort)0x0000);
        return ValueTask.FromResult(response);
    }
}</code></pre>
</div>

<p>İşleyicinin veri setiyle ne yaptığı sizin kararınızdır: diske yazmak, kuyruğa koymak, önce <a href="/medical/net/anonymization/">anonymization API</a> ile anonimleştirmek ya da arşivin sözdizimine dönüştürmek.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ağ API'sinin başkaca kapsadığı konular">}}

<p>Yukarıdaki üç hizmet yaygın olanlardır. DIMSE'nin geri kalanı da mevcuttur:</p>

<ul>
<li>Getirme: C-MOVE ve C-GET, yanıtların içinde alt işlem sayacı raporlanır.</li>
<li>N-servisleri: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE ve N-EVENT-REPORT; bunlar depolama taahhüdü (storage commitment) ve MPPS'in temelini oluşturur.</li>
<li>İlişki (association) kontrolü: sunum bağlamları, hizmet sınıfı rolleri, genişletilmiş müzakere, eşzamanlı olmayan işlemler penceresi, kullanıcı kimliği müzakeresi ve AE başlığı çağrısıyla bir ilişkiyi reddedebilen bir politika kancası.</li>
<li>Her iki tarafta TLS, <code>TlsInitiatorAuthenticator</code> ve <code>TlsAcceptorAuthenticator</code> üzerinden, ihtiyacınız olursa kendi sertifika doğrulamanızla.</li>
<li>TCP bağlanmadan serbest bırakmaya kadar her aşama için zaman aşımı ayarları ve ilişki yaşam döngüsü bildirimleri, böylece uzun süren bir düğüm ne olduğunu kaydedebilir.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">DICOM ağ kılavuzu</a> her seçeneği ve her işleyiciyi belgeler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Saf .NET, soketten piksel verisine kadar">}}

<p>Bu sayfadaki her şey dosyaları okuyan ve yazan aynı paketten gelen yönetilen koddur. İlişki (association), codec'ler ve ayrıştırıcı (parser) aynı kütüphaneden gelir, böylece ağ üzerinden alınan bir çalışma, süreçten çıkmadan ve zincirde hiçbir yerel bağımlılık olmadan anonimleştirilebilir, dönüştürülebilir veya serileştirilebilir.</p>

<p>İlgili sayfalar: gönderilecek şeyler için <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a>, önce neyin kaldırılacağını belirlemek için <a href="/medical/net/anonymization/">anonymization</a> ve geleni okumak için <a href="/medical/net/dicom-tags/">DICOM tags</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dökümantasyon" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Geliştirici Kılavuzu" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="API Referansları" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Ürün Desteği" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ücretsiz Destek" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Ücretli Destek" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Neden Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Müşteri Listesi" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Başarı Hikâyeleri" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
