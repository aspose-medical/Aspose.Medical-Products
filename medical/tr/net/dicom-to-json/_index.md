---
title: C# .NET'te DICOM'u JSON'a Dönüştür | Aspose.Medical
weight: 4000
description: DICOM dosyalarını C# .NET'te standart DICOM JSON formatına serileştir. Numara işleme, toplu veri URI'leri, anahtar kelime çıktısı ve async akış işleme seçeneklerini Aspose.Medical API ile yapılandır.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'ta DICOM'u JSON'a Dönüştür" h2="DICOM veri setlerini ve dosyalarını standart DICOM JSON Modeli (PS3.18)'e serileştir. Numara işleme, toplu veri referansları, anahtar kelime çıktısı ve akış tabanlı async işleme seçeneklerini yapılandır." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standart DICOM JSON Modeli">}}

<p><strong>Aspose.Medical for .NET</strong> DICOM verilerini <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Modeli</a>'ni izleyerek JSON'a serileştirir. Bu, DICOM veri setlerini JSON'da temsil etmek için resmi standarttır; DICOMweb hizmetleri, FHIR ImagingStudy kaynakları ve modern sağlık API'leri tarafından kullanılır.</p>

<p>DICOM JSON Modelinde, her öznitelik JSON anahtarı olarak etiketine (<code>"00100010"</code> gibi, Hasta Adı için) karşılık gelir ve Value Representation ile değer dizisi iç içe özellikler olarak yer alır:</p>

<div class="codeblock" id="code">
 <h3>DICOM JSON Model formatı</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#'ta DICOM'u JSON'a Serileştir">}}

<p><code>DicomJsonSerializer</code> sınıfını kullanarak bir DICOM dosyasını veya veri setini JSON'a dönüştürün. En basit yaklaşım, kompakt ve standartlara uygun çıktı üretir:</p>

<div class="codeblock" id="code">
 <h3>DICOM'u JSON'a Dönüştür - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p>Bir <code>Dataset</code>, tam bir <code>DicomFile</code> (File Meta Information dahil) ya da veri seti dizisini serileştirebilirsiniz:</p>

<div class="codeblock" id="code">
 <h3>DicomFile ve veri seti dizilerini serileştir - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Akış Tabanlı ve Async Serileştirme">}}

<p>Büyük DICOM dosyaları veya yüksek verim senaryoları için, bellekte büyük string'ler tahsis etmekten kaçınmak amacıyla doğrudan bir UTF-8 akışına serileştirin. Senkron ve async yöntemler mevcuttur:</p>

<div class="codeblock" id="code">
 <h3>Akış ve async serileştirme - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serileştirme Seçenekleri">}}

<p><code>DicomJsonSerializerOptions</code> sınıfı, DICOM verilerinin JSON'da nasıl temsil edileceğini kontrol eder:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Özellik</th>
<th>Tür</th>
<th>Açıklama</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Sayısal VR'ların (IS, DS, SV, UV) JSON sayısı mı yoksa dize olarak mı yazılacağını kontrol eder</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>DICOM anahtar kelimelerini (örn. <code>"PatientName"</code>) etiketler (<code>"00100010"</code>) yerine JSON anahtarı olarak kullan. Not: standart dışı</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>DICOM anahtar kelimesini ek bir JSON özniteliği olarak ekle</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>DICOM etiket adını ek bir JSON özniteliği olarak ekle</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Büyük verileri (örn. piksel verisi) URI referansları olarak yazmak için özel dönüştürücü</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Deseralizasyon sırasında toplu veri URI'lerini çözmek için özel yükleyici</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Serileştirme seçeneklerini yapılandır - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Numara İşleme">}}

<p>DICOM sayısal Value Representation'ları (IS, DS, SV, UV) JSON sayıları ya da JSON dizeleri olarak serileştirilebilir. Bu, bazı DICOM uygulamalarının sayıları önde boşluklarla veya standart dışı biçimlerde saklaması nedeniyle birlikte çalışabilirlik için önemlidir:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Mod</th>
<th>JSON Çıktısı</th>
<th>Kullanım Durumu</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>Standart uyumlu, hesaplamalar için ideal. Değer ayrıştırılamazsa hata fırlatır.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>Orijinal dize temsili korunur. Standart dışı verilerin geri dönüşümünde daha güvenlidir.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Toplu Veri İşleme">}}

<p>Büyük ikili değerler (piksel verisi, dalga biçimleri, kapsüllenmiş belgeler) JSON çıktısına gömülmek yerine toplu veri referansları olarak işlenebilir. Bu, <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a> spesifikasyonuna uyar.</p>

<p>Serileştirme sırasında büyük verileri dışa aktarmak için <code>IBulkDataConverter</code> uygulayın ve deseralizasyon sırasında URI'leri çözmek için <code>IBulkDataLoader</code> kullanın:</p>

<div class="codeblock" id="code">
 <h3>Özel toplu veri işleme - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JSON'ı DICOM'a Dönüştür">}}

<p>DICOM JSON'ı tekrar Dataset veya DicomFile nesnelerine ayrıştırın. Dize girişi, akış girişi ve async akış işlemlerini destekler:</p>

<div class="codeblock" id="code">
 <h3>JSON'ı DICOM'a Dönüştür - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="System.Text.Json Entegrasyonu">}}

<p>Aspose.Medical, standart <code>System.Text.Json</code> serileştirme hattı ile entegrasyon için <code>DatasetJsonConverter</code> ve <code>DicomFileJsonConverter</code> sağlar. Bu sayede DICOM nesneleri mevcut JSON işleme kodunuzda diğer .NET türleriyle birlikte serileştirilebilir:</p>

<div class="codeblock" id="code">
 <h3>System.Text.Json entegrasyonu - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Belgeler" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Kaynak Kodu" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
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
