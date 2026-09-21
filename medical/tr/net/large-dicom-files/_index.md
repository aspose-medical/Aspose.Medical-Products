---
title: C# .NET'te Büyük DICOM Dosyalarıyla Çalışın | Aspose.Medical
weight: 11500

description: C#'ta çok çerçeveli çalışmalar ve bütün slayt görüntülerini belleğe yüklemeden açın. Piksel verisi olmadan meta verileri okuyun, büyük öğeleri erteleyin ve dosyaları akışlar ve borular aracılığıyla taşıyın.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Büyük DICOM Dosyaları .NET C#" h2="Bir çok çerçeveli çalışmanın meta verilerini pikseller olmadan okuyun, büyük öğeleri bir şey talep edene kadar erteleyin ve tüm dosyaları akışlar ve borular üzerinden taşıyın." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Dosya büyük, soru genellikle küçüktür">}}

<p>Bir bütün slayt görüntüsü, uzun bir CT serisi veya bir OCT hacmi yüzlerce megabayt olabilir ve çoğu piksel verisidir. Bir uygulamanın gerçekte yaptığı iş genellikle çok daha küçüktür: bir klasörde ne olduğunu listeleme, bir hasta kimliğini kontrol etme, çerçeveleri sayma, bir çalışmanın nereye gidileceğine karar verme. Buna yanıt vermek için her byte'ı yüklemek, basit bir görevi bellek sorununun başına getiren şeydir.</p>

<p><strong>Aspose.Medical for .NET</strong>, çağıranın bir dosyanın ne kadarının okunacağını belirlemesine olanak tanır. Seçim, <code>DicomFile.Open</code> üzerindeki bir argümandır ve dosyalar, akışlar ve borular için aynı şekilde geçerlidir.</p>

<p>Aynı makinede ve aynı dosyada, test setimizden 128 çerçeveli 14 MB'lık bir çalışma üzerinden ölçülmüştür:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Okuma stratejisi</th>
<th>Açma süresi</th>
<th>Ayrılan bellek</th>
</tr>
</thead>
<tbody>
<tr><td>Her şey, varsayılan</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Büyük öğeler atlandı</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Büyük öğeler ertelendi</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>Aradaki fark dosyayla birlikte artar. 10.000 çalışmadan oluşan bir klasör, bunun artık mikro-optimizasyon olmayacağı durumdur.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Meta verileri okuyun, pikselleri dokunmadan bırakın">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code>, boyut eşiğinin üzerindeki her öğeyi okuma dışı bırakır. Geri dönen veri kümesi, bir indeksin ya da yönlendiricinin ihtiyaç duyduğu etiketleri içerir.</p>

<div class="codeblock" id="code">
 <h3>Piksel verisi olmadan bir çalışmayı okuyun - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>Eşik varsayılan olarak 64 kB'dir ve kilobayt cinsinden bir değer alır, böylece 8 kB'yi büyük kabul eden bir iş akışı bunu belirtebilir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Atlamak yerine erteleyin">}}

<p>Piksellere ihtiyaç duyulabilir, ancak muhtemelen daha sonra ve muhtemelen hepsi değilse, <code>ReadLargeOnDemand</code> çiftin diğer yarısıdır. Dosyayı açmak, atlamaya eşdeğerdir ve büyük bir öğe, kod ona dokunduğu anda okunur.</p>

<div class="codeblock" id="code">
 <h3>Kullanıldığında yalnızca bir çerçeveyi yükleyin - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>Ertelenmiş okuma lisanslı bir özelliktir; diğer stratejiler de değerlendirme aşamasında çalışır.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Piksellere dokunmadan bir klasörü indeksleyin">}}

<p>Aynı strateji bir akışa da uygulanır; bu, koddan bakıldığında bir arşiv taraması ya da bulut nesne depolama gibi görünür.</p>

<div class="codeblock" id="code">
 <h3>Bir arşivi tarayın - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Akışlar ve borular, içe ve dışa">}}

<p>Okuma ve yazma her ikisi de akışları kabul eder ve eş zamanlı giriş noktaları ayrıca <code>System.IO.Pipelines</code> türlerini de kabul eder. Bir çalışma, ağ yanıtından depolamaya, süreç bütün dosyayı tek bir dizi olarak tutmadan geçebilir.</p>

<div class="codeblock" id="code">
 <h3>Akışlar üzerinden okuyun ve yazın - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>Aynı fikir metin temsillerini de kapsar: çok sayıda veri kümesi içeren bir belge, <a href="/medical/net/json-to-dicom/">JSON to DICOM</a> ve <a href="/medical/net/xml-to-dicom/">XML to DICOM</a> sayfalarında bir seferde bir veri kümesi olarak okunur.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Çerçeve çerçeve">}}

<p>Çok çerçeveli veri çerçeve bazında ele alınır, bu yüzden 500 çerçeveli bir seri, tüm piksel veri öğesini bir seferde değil, bir çerçeve başına maliyetle işlenir.</p>

<div class="codeblock" id="code">
 <h3>Çerçeveleri dolaşın - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bu kararın tasarımı belirlediği yer">}}

<ul>
<li>Arşiv indeksleme ve göç: milyonlarca dosya, ve bir şey taşınana kadar yalnızca başlık (header) önemlidir.</li>
<li>Yönlendiriciler ve depolama düğümleri: bir çalışmayı kabul eder, yönlendirme için gerekenleri okur, baytları iletir.</li>
<li>AI boru hatları: manifestoyu meta verilerden oluşturur, ardından gerçek eğitim yapılan alt küme için çerçeveleri çeker.</li>
<li>Bellek sınırı olan konteynerler: çalışma seti dosya boyutuna değil, stratejiye göre belirlenir.</li>
<li>Bütün slayt ve OCT verileri: her şeyin okunmasının kesinlikle bir seçenek olmadığı dosyalar.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">Bellek yönetimi rehberi</a> stratejileri ayrıntılı olarak açıklar ve <a href="/medical/net/dicom-networking/">DICOM networking</a> aynı verinin DIMSE üzerinden gelmesini gösterir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokümantasyon" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Geliştirici Kılavuzu" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
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
