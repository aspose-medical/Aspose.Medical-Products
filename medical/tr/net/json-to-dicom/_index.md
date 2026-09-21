---
title: C# .NET'te JSON'i DICOM'a Dönüştür | Aspose.Medical
weight: 6000

description: C# .NET'te standart DICOM JSON Model (PS3.18)'den DICOM dosyaları oluşturun. JSON'u bir dizeden, bir akıştan veya bir borudan okuyun, veri seti dizisini akış olarak gönderin ve Aspose.Medical API ile bulk veri referanslarını çözün.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'ta JSON'i DICOM'a Dönüştür" h2="Standart DICOM JSON Model (PS3.18)'i veri setlerine ve DICOM dosyalarına geri okuyun. Bir dizeden, bir akıştan veya bir borudan çalışın, bir çalışma dizisini akış olarak gönderin ve bulk veri referanslarını çözün." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM JSON'dan DICOM dosyasına">}}

<p><strong>Aspose.Medical for .NET</strong>, <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>'i okur; bu model DICOMweb hizmetleri ve HTTP üzerinden çalışmaları değiştiren sistemler tarafından kullanılır. JSON olarak gelen veri bir <code>Dataset</code> haline gelir ve bir <code>Dataset</code> disk üzerine DICOM dosyası olarak yazılır.</p>

<p>Bu, <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> sayfasının ters yönüdür ve ikisi aynı sınıfı, <code>DicomJsonSerializer</code>, kullanır.</p>

<div class="codeblock" id="code">
 <h3>JSON'dan DICOM dosyası oluştur - C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Dosya Meta Bilgisi bulunmayan bir veri seti, <code>DicomFile</code> içinde paketlendiğinde, varsayılan transfer sözdizimi olan Implicit VR Little Endian ile yazılır.</p>

<p>DICOM JSON okuma lisanslı bir özelliktir. Yerinde bir lisans uygulanmadığında okuyucu bir <code>MedicalApiException</code> fırlatır, bu yüzden önce lisansı uygulayın, <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">lisanslama kılavuzu</a>nda açıklandığı gibi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dosya Meta Bilgisini Koruyun">}}

<p><code>Deserialize</code> yalnızca veri setini döndürür. JSON belgesi aynı zamanda Dosya Meta Bilgi grubunu da içerdiğinde, örneğin tam bir DICOM dosyasından üretildiyse, <code>DeserializeFile</code> bu grubu bozulmadan içeren bir <code>DicomFile</code> döndürür; dosyanın belirttiği transfer sözdizimini de kapsar.</p>

<div class="codeblock" id="code">
 <h3>JSON'dan tam bir DICOM dosyasını oku - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Akışlar, borular ve async">}}

<p>Her giriş noktasının bir akış aşırı yüklemesi ve bir asenkron aşırı yüklemesi vardır; asenkron olanlar ayrıca bir <code>PipeReader</code> kabul eder. Web yanıtından veya diskten gelen bir belge, önce bir dizeye dönüştürülmeden okunur; bu, JSON piksel verisi içerdiğinde önemlidir.</p>

<div class="codeblock" id="code">
 <h3>Akıştan JSON oku - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Veri seti dizisi, tek tek">}}

<p>Bir DICOMweb sorgusu, veri seti dizisiyle yanıt verir ve bu belge büyük olabilir. <code>DeserializeList</code> tüm diziyi belleğe okur; <code>DeserializeAsyncEnumerable</code> ise bir seferde bir veri seti verir, böylece belge hiçbir zaman tamamen bellekte tutulmaz.</p>

<div class="codeblock" id="code">
 <h3>Veri seti dizisini akış olarak gönder - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk veri referansları">}}

<p>DICOM JSON Model, piksel verisini satır içi taşımaz. Büyük değerler, baytlara işaret eden bir <code>BulkDataURI</code> ile değiştirilir, bu da JSON belgesinin küçük kalmasını sağlar. Okuma sırasında bu referansları çözmek için serileştiriciye bir bulk veri yükleyicisi verin. <code>DefaultBulkDataLoader</code>, kimlik doğrulama olmadan <code>file</code>, <code>http</code> ve <code>https</code> URI'larını getirir; kimlik bilgisi gerektiren bir arşiv için, <code>IBulkDataLoader</code> veya <code>IAsyncBulkDataLoader</code> arayüzlerini kendiniz uygulayın.</p>

<div class="codeblock" id="code">
 <h3>Okuma sırasında BulkDataURI'yi çöz - C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM'dan JSON'a çift yönlü dönüşüm">}}

<p>İki yön birlikte kullanılmak üzere tasarlanmıştır: bir çalışma JSON olarak çıkar, bir web servisi üzerinden geçer ve DICOM dosyası olarak geri döner. İşlemde yerel koda bağımlılık yoktur, bu nedenle aynı çift yönlü dönüşüm Windows, Linux ve macOS'ta çalışır.</p>

<div class="codeblock" id="code">
 <h3>DICOM'dan JSON'a ve geri - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>JSON'un nasıl görüneceğini kontrol eden seçenekler için <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> sayfasına bakın. Aynı çift XML için de mevcuttur: <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> ve <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">JSON serileştirme kılavuzu</a> tüm API'yı kapsar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokümantasyon" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Geliştirici Kılavuzu" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="API Referansları" href="https://reference.aspose.com/medical/net/" >}}
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