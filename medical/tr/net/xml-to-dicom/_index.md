---
title: C# .NET'te XML'yi DICOM'e Dönüştür | Aspose.Medical
weight: 5000

description: C# .NET ile PS3.19'un Native DICOM Model XML'inden DICOM dosyaları oluşturun. XML'yi bir dizeden, bir akıştan veya bir borudan okuyun, ardışık belgeleri akışlayın ve Aspose.Medical API ile bulk veri referanslarını çözün.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'ta XML'yi DICOM'e Dönüştür" h2="PS3.19'un Native DICOM Model XML'sini veri kümelerine ve DICOM dosyalarına geri okuyun. Bir dizeden, bir akıştan veya bir borudan çalışın, ardışık belgeleri akışlayın ve bulk veri referanslarını çözün." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standart Native DICOM Model XML">}}

<p><strong>Aspose.Medical for .NET</strong>, DICOM PS3.19'da tanımlanan <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a>i okur. Bu, standart içine yazılmış XML temsili olup, Aspose tarafından icat edilen bir format değildir; bu özelliği entegrasyon için faydalı kılar: DICOM'u XML olarak zaten değişen bir sistem, bu kütüphanenin kabul ettiği belgeleri üretir.</p>

<p>Belgenin kökü <code>NativeDicomModel</code>’dir ve her öznitelik, etiketini, değer temsili ve anahtar kelimesini taşıyan bir <code>DicomAttribute</code> öğesidir:</p>

<div class="codeblock" id="code">
 <h3>Native DICOM Model biçimi</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>Bu sayfa <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> işleminin ters yönüdür ve her ikisi de aynı sınıfı, <code>DicomXmlSerializer</code>, kullanır.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="XML'den C# ile DICOM dosyası oluşturma">}}

<p><code>Deserialize</code>, bir belgeyi <code>Dataset</code>e dönüştürür ve bir dataset, DICOM dosyası olarak diske yazılır.</p>

<div class="codeblock" id="code">
 <h3>XML'den DICOM dosyası oluşturma - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Native DICOM Model, File Meta Information grubuna sahip değildir, bu yüzden transfer sözdizimi belgenin bir parçası değildir. <code>DicomFile</code> içinde paketlenmiş bir dataset, varsayılan transfer sözdizimi olan Implicit VR Little Endian ile yazılır. Dosyayı başka bir sözdizimiyle depolamak için, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer sözdizimi dönüşümü</a> sayfasında gösterildiği gibi, onu yeniden kodlayın.</p>

<p>DICOM XML okuma, lisanslı bir özelliktir. Yerinde bir lisans uygulanmadığında okuyucu <code>MedicalApiException</code> hatası verir; bu yüzden önce lisansı uygulayın, <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">lisans kılavuzu</a> bunu açıklar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Akışlar, borular ve async">}}

<p>Her giriş noktasının bir stream aşırı yüklemesi ve bir asynchronous aşırı yüklemesi vardır; asynchronous olanlar ayrıca bir <code>PipeReader</code> kabul eder. Web yanıtından gelen bir belge, önce bir dizeye dönüştürülmeden okunurken ayrıştırılır.</p>

<div class="codeblock" id="code">
 <h3>Bir stream'den XML oku - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tek bir stream'de ardışık belgeler">}}

<p>Başka bir sistemden yapılan bir aktarım genellikle tek bir stream içinde bir <code>NativeDicomModel</code> öğesini diğerinin ardından tutar. <code>DeserializeAsyncEnumerable</code>, her öğe başına bir dataset döndürür, giriş sırasına göre, böylece stream bellek içinde tutulmadan işlenir. Öğeler doğrudan birbirini izler: bir XML bildirimi yalnızca en başta izin verilir, tıpkı diğer XML girdilerinde olduğu gibi.</p>

<div class="codeblock" id="code">
 <h3>Ardışık belgeleri streamle - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk veri referansları">}}

<p>Piksel verisi gibi büyük değerler satır içinde yazılmaz. Bunlar, baytlara işaret eden bir URI içeren bir <code>BulkData</code> öğesi olarak görünür; bu belgeyi küçültür. Okuma sırasında bu referansları çözmek için serileştiriciye bir bulk veri yükleyicisi sağlayın. <code>DefaultBulkDataLoader</code>, kimlik doğrulama gerektirmeden <code>file</code>, <code>http</code> ve <code>https</code> URI'larını alır; kimlik bilgileri gereken bir arşiv için, <code>IBulkDataLoader</code> veya <code>IAsyncBulkDataLoader</code> arayüzlerini kendiniz uygulayın.</p>

<div class="codeblock" id="code">
 <h3>Okurken bulk veri çöz - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM'dan XML'e tam döngü">}}

<p>İki yön birlikte kullanılmak üzere tasarlanmıştır: bir çalışma XML olarak çıkar, XML konuşan bir sistemden geçer ve DICOM dosyası olarak geri döner. Her şey .NET ile yönetilir, bu yüzden aynı tam döngü Windows, Linux ve macOS'ta çalışır.</p>

<div class="codeblock" id="code">
 <h3>DICOM'dan XML'e ve geri - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>XML'in görünümünü kontrol eden seçenekler için <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> sayfasına bakın. Aynı çift JSON için de mevcuttur: <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> ve <a href="/medical/net/json-to-dicom/">JSON to DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">Serileştirme kılavuzu</a> tüm API'yi kapsar.</p>

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

{{< blocks/products/pf/slr-tab tabTitle="Neden Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Müşteri Listesi" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Başarı Hikayeleri" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}