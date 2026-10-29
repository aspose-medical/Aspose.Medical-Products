---
title: DICOM Ağ Bağlantısı C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Uygulamanızı .NET üzerinden bir PACS’e bağlayın. C-ECHO ile doğrulama yapın, C-STORE ile görüntü gönderin, C-FIND ile sorgulayın ve kendi SCP’nizle görüntüleri alın. Saf C# içinde bir DIMSE istemcisi ve sunucusu.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM Ağ Bağlantısı .NET C#" h2="Kendi uygulamanızdan bir PACS ile iletişim kurun: C-ECHO, C-STORE, C-FIND, C-MOVE ve C-GET, istemci ve sunucu olarak, yönetilen C# içinde ve makinede hiçbir şey kurmaya gerek olmadan." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Kendi kodunuzdan bir PACS’e bağlanın">}}

<p>DICOM dosyalarını okumak medikal görüntülemenin kolay yarısıdır. Uygulamanızın gerçek bir hastane sistemiyle çalışması gerektiğinde, DIMSE konuşması gerekir: bir PACS ile ilişki (association) açmak, görüntü göndermek, hangi çalışmaların olduğunu sormak ve başka bir sistem geri bir şey gönderdiğinde yanıt vermek.</p>

<p><strong>Aspose.Medical for .NET</strong> bu protokolü kütüphanenin bir parçası olarak sunar. <code>Aspose.Medical.Dicom.Network</code> size bir DIMSE istemcisi ve bir DIMSE sunucusu sağlar, her ikisi de yönetilen C# ile yazılmıştır. Kurulum gerektiren native bir araç takımı, yapılandırılacak bir hizmet ya da platforma özgü bir şey yoktur; bu yüzden aynı kod Windows, Linux ve bir konteyner içinde çalışır.</p>

<p>Entegrasyonların çoğunu üç şey kapsar ve her biri birkaç satır kodla yapılabilir: C-ECHO ile bağlantıyı kontrol edin, C-STORE ile görüntü gönderin ve C-FIND ile diğer tarafta ne olduğunu bulun.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-ECHO ile Başlayın">}}

<p>C-ECHO, DICOM ping’idir. Host, port ve iki AE başlığının doğru olduğunu, diğer her şeyden önce kanıtlar. Bir istemciyi bir kez oluşturun, cevabı gözlemleyecek bir işleyici verin ve isteği gönderin.</p>

<div class="codeblock" id="code">
 <h3>C-ECHO ile bağlantıyı doğrulayın - C#</h3>
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

<p>Durum <code>0x0000</code> başarı anlamına gelir. İstekler kuyruğa alınır ve tek bir ilişki üzerinden gönderilir, bu yüzden bir iş grubu her öğe için ayrı bir bağlantı açmaz.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-STORE ile görüntü gönderin">}}

<p>C-STORE, bir uygulamanın bir görüntü ürettikten ya da aldktan sonra yaptığı işlemdir: olayı arşive itmek. Her örnek için bir istek kuyruğa alın ve hepsini birlikte gönderin.</p>

<div class="codeblock" id="code">
 <h3>Bir DICOM dosyasını PACS’e gönderin - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Önerdiğiniz sunum bağlamları, arşivin neyi kabul edeceğini belirler. Sıkıştırılmış bir söz dizimi (syntax) istiyorsa, göndermeden önce dönüştürün; <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> sayfasının gösterdiği gibi, ya da alternatifleri <code>AdditionalTransferSyntaxes</code> içinde listeleyin ve müzakerenin birini seçmesine izin verin.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-FIND ile çalışmalar bulun">}}

<p>C-FIND, "arşivde ne var?" sorusuna yanıt verir. Eşleşmeler tek tek gelir, her biri kendi tanımlayıcı veri kümesiyle, ve son yanıt sorguyu kapatır.</p>

<div class="codeblock" id="code">
 <h3>Hasta bazlı çalışma sorgulama - C#</h3>
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

<p>Aynı fabrika diğer sorgu seviyelerini de oluşturur: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> ve <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> bir modalite iş listesi sorgusu oluşturur; bu, bir modalitenin tarama öncesi yaptığı sorgudur; <a href="/medical/net/modality-worklist/">modality worklist page</a> MPPS ve iş listesi sunucu tarafı ile birlikte gösterir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Görüntüleri alın: kendi store SCP’niz">}}

<p>Kütüphane aynı zamanda bir sunucudur. Sunmak istediğiniz hizmet için bir işleyici kaydedin, dinlemeye başlayın ve uygulamanız bir DICOM düğümü haline gelir; bir modalite ya da başka bir PACS ona veri gönderebilir.</p>

<div class="codeblock" id="code">
 <h3>Gelen görüntüleri kabul edin - C#</h3>
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
 <h3>Gelen veriyi saklayan işleyici - C#</h3>
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

<p>İşleyicinin veri kümesiyle ne yapacağı sizin kararınızdır: diske yazmak, kuyruğa koymak, önce <a href="/medical/net/anonymization/">anonymization API</a> ile anonimleştirmek, ya da arşivin söz dizimine dönüştürmek.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ağ API’sinin kapsadığı diğer konular">}}

<p>Yukarıdaki üç hizmet en yaygın olanlardır. DIMSE’nin geri kalan kısmı da mevcuttur:</p>

<ul>
<li>Retrieval: C-MOVE ve C-GET, yanıtların içinde alt‑operasyon sayıcıları raporlanır.</li>
<li>N‑services: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE ve N-EVENT-REPORT, bunlar storage commitment ve MPPS’in temelini oluşturur.</li>
<li>İlişki kontrolü: sunum bağlamları, servis sınıfı rolleri, genişletilmiş müzakere, asenkron operasyon penceresi, kullanıcı kimliği müzakeresi ve AE başlığıyla ilişkiyi reddedebilen politika kancası.</li>
<li>Her iki tarafta TLS, <code>TlsInitiatorAuthenticator</code> ve <code>TlsAcceptorAuthenticator</code> aracılığıyla, ihtiyacınız olursa kendi sertifika doğrulamanızla.</li>
<li>Bağlantının her aşaması için zaman aşımı, TCP bağlanmadan serbest bırakmaya kadar, ve ilişki yaşam döngüsü bildirimleri, böylece uzun süren bir düğüm ne olduğunu kaydedebilir.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">DICOM networking guide</a> her seçeneği ve her işleyiciyi belgeler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Saf .NET, soketten piksel verisine kadar">}}

<p>Bu sayfadaki her şey, dosyaları okuyan ve yazan aynı paketten gelen yönetilen koddur. İlişki, codec’ler ve ayrıştırıcı tek bir kütüphaneden gelir; böylece ağ üzerinden alınan bir çalışma, süreçten çıkmadan ve zincirin hiçbir yerinde native bağımlılık olmadan anonimleştirilebilir, dönüştürülebilir ya da serileştirilebilir.</p>

<p>İlgili sayfalar: <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> gönderilecek şey için, <a href="/medical/net/anonymization/">anonymization</a> önce neyin kaldırılacağını için ve <a href="/medical/net/dicom-tags/">DICOM tags</a> geleni okumak için.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokümantasyon" href="https://docs.aspose.com/medical/net/" >}}
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
{{< blocks/products/pf/slr-element name="Başarı Hikayeleri" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
