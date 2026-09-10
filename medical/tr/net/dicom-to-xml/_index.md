---
title: C# .NET'te DICOM'i XML'e Dönüştür | Aspose.Medical
weight: 3000
description: DICOM veri setlerini C# .NET'te standart DICOM XML formatına serileştirin. Bulk veri işleme, akış tabanlı işleme ve asenkron işlemleri Aspose.Medical API ile yapılandırın.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'ta DICOM'i XML'e Dönüştür" h2="DICOM veri setlerini standart DICOM XML temsiline (PS3.19) serileştirin. Bulk veri referanslarını, akış tabanlı çıktıyı ve asenkron işleme saf .NET kütüphanesiyle yapılandırın." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standart Tabanlı DICOM XML Serileştirme">}}

<p><strong>Aspose.Medical for .NET</strong>, DICOM verilerini XML'e, <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a>ine uygun olarak serileştirir. Bu, DICOM veri setlerini XML olarak temsil eden resmi standarttır ve DICOMweb hizmetleri, entegrasyon platformları ve tıbbi görüntüleme meta verilerinin insan tarafından okunabilir, şema doğrulamalı temsiline ihtiyaç duyan sistemler tarafından kullanılır.</p>

<p><code>DicomXmlSerializer</code> sınıfı, serileştirme ve serileştirme kaldırma için statik metodlar sağlar. Basit etiket dökümüne kıyasla, çıktı her öğenin etiketi, VR'si ve uygun biçimlendirilmiş değerleri ile DICOM XML şemasına uyar &mdash; ikili DICOM ile XML arasında kayıpsız çift yönlü dönüşüm sağlar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM'i XML'e C#'ta Serileştir">}}

<p><code>DicomXmlSerializer</code> sınıfını kullanarak bir DICOM veri setini XML dizesine dönüştürün. En basit yaklaşım, standartlara uygun bir XML belgesi üretir:</p>

<div class="codeblock" id="code">
 <h3>DICOM'i XML'e Dönüştür - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Akış Tabanlı ve Asenkron Serileştirme">}}

<p>Büyük DICOM dosyaları veya yüksek verim senaryoları için, bellekte büyük dizeler tahsis etmeyi önlemek amacıyla doğrudan bir akışa serileştirin. Hem senkron hem de asenkron metodlar mevcuttur:</p>

<div class="codeblock" id="code">
 <h3>Senkron akış serileştirme - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Asenkron akış serileştirme - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Büyük Çalışmalar İçin Boru Hattı Akışı">}}

<p>Tüm çalışmaların bellekte tutulması gerekmez. <code>DicomXmlSerializer</code>, bir <code>PipeWriter</code>a yazar ve bir <code>PipeReader</code>dan okur, böylece XML akış sırasında üretilir ve tüketilir; veri seti dizisi ise <code>DeserializeAsyncEnumerable</code> aracılığıyla tek tek okunabilir. Her metod bir <code>CancellationToken</code> alır.</p>

<div class="codeblock" id="code">
 <h3>Bir boru aracılığıyla serileştirme ve serileştirme kaldırma - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Dizi halinde veri setlerini tek tek okuyun - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serileştirme Seçenekleri">}}

<p><code>DicomXmlSerializerOptions</code> sınıfı, DICOM verilerinin XML içinde nasıl temsil edildiğini kontrol eder. Birincil yapılandırma, büyük ikili değerler için bulk veri işleme içerir:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Özellik</th>
<th>Tür</th>
<th>Açıklama</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Büyük verileri (ör. piksel verisi) satır içi eklemek yerine BulkData URI referansları olarak yazmak için özel dönüştürücü</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Serileştirme kaldırma sırasında BulkData URI'larını çözmek için özel yükleyici</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Özel seçenekler sağlanmadığında kullanılan varsayılan seçenek örneği</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Özel seçeneklerle serileştir - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk Veri İşleme">}}

<p>Büyük ikili değerler (piksel verileri, dalga formları, kapsüllenmiş belgeler) XML çıktısına satır içi eklenmek yerine BulkData URI referansları olarak dışa aktarılabilir. Bu, <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">DICOM PS3.19 BulkData öğesi</a> spesifikasyonuna uygundur.</p>

<p>Serileştirme sırasında büyük verileri dışa aktarmak için <code>IBulkDataConverter</code> uygulayın ve serileştirme kaldırma sırasında URI'ları çözmek için <code>IBulkDataLoader</code> uygulayın. Yaygın durumlarda bir yükleyici yazmaya gerek yoktur: <code>DefaultBulkDataLoader.Instance</code>, <code>file</code>, <code>http</code> ve <code>https</code> URI'larını çözer ve ayrıca <code>IAsyncBulkDataLoader</code>'ı uygular, böylece bulk veri akış yollarında asenkron olarak alınır.</p>

<div class="codeblock" id="code">
 <h3>Özel bulk veri işleme - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
public class FileBulkDataConverter : IBulkDataConverter
{
    public string? GetBulkDataUri(IElement element)
    {
        // Externalize pixel data to a separate file
        if (element.Tag == Tag.PixelData)
            return "file:///bulk/pixeldata.raw";
        return null; // inline all other elements
    }
}

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="XML'i DICOM'e Serileştirme Kaldır">}}

<p>DICOM XML'i tekrar Dataset nesnelerine ayrıştırın. Dize girişi, akış girişi ve asenkron işlemleri destekler:</p>

<div class="codeblock" id="code">
 <h3>XML'i DICOM'e Serileştirme Kaldır - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="XML ve JSON Serileştirme">}}

<p>Aspose.Medical, DICOM XML (PS3.19) ve DICOM JSON (PS3.18) serileştirmesini destekler. Her iki format da kayıpsız çift yönlü dönüşüm sağlar, ancak farklı entegrasyon senaryolarına hizmet eder:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Özellik</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Standart</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>Şema doğrulama</td><td>XML Şeması (XSD) mevcut</td><td>Resmi şema yok</td></tr>
<tr><td>En İyi Kullanım</td><td>Kurumsal entegrasyon, HL7 CDA, denetim logları, XDS kayıtları</td><td>DICOMweb, REST API'ler, FHIR ImagingStudy</td></tr>
<tr><td>İnsan okunabilirliği</td><td>Ayrıntılı ancak kendini tanımlayan</td><td>Kısa ve geniş destekli</td></tr>
<tr><td>Bulk veri</td><td>URI'lu BulkData öğesi</td><td>BulkDataURI özelliği</td></tr>
<tr><td>Serileştirici sınıfı</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokümantasyon" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Kaynak Kodu" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
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
