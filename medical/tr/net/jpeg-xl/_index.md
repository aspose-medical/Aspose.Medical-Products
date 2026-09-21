---
title: C# .NET için DICOM'da JPEG XL | Aspose.Medical
weight: 10500

description: DICOM görüntülerini C# ile JPEG XL formatında depolayın. Bit‑bit aynı pikselleri geri döndüren kayıpsız JPEG XL, tek bir yönetilen derlemede, dağıtılacak yerel codec olmadan.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="C# .NET içinde DICOM için JPEG XL" h2="DICOM standardındaki en yeni sıkıştırma, ölçtüğümüz en küçük kayıpsız dosyalar, yönetilen C# ile uygulanmış ve tek bir derleme içinde sunulmuştur." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Neden JPEG XL DICOM'a eklendi">}}

<p>Tıbbi arşivler büyür ve asla küçülmez. JPEG XL, görüntüleme dünyasının iki on yıl süren JPEG ve JPEG 2000 deneyiminin ardından tasarladığı codec'tir ve DICOM, depolama ekiplerinin önem verdiği bir nedenle onu bir transfer sözdizimi olarak ekledi: aynı pikseller için dosya daha küçüktür.</p>

<p><strong>Aspose.Medical for .NET</strong> JPEG XL'yi kütüphane içinde yaşayan libjxl'nin C# portu aracılığıyla yazar ve okur. Paket tek bir derleme, <code>Aspose.Medical.dll</code>, gönderir ve yanında yerel ikili dosya bulunmaz, bu yüzden bu kadar yeni bir codec dağıtım projesine dönüşmez: aynı derleme Windows, Linux, bir build ajanı ve bir konteyner içinde çalışır.</p>

<p>İki transfer sözdizimi pikselleri taşır:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), değişmeden geri dönmesi gereken teşhis verileri için.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), daha küçük bir dosyanın tam kopyadan daha önemli olduğu durumlar için.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bir çalışmayı sıkıştırın, her pikseli koruyun">}}

<p>Dönüştürme tek bir çağrı ile yapılır ve piksel etrafındaki veri setiyle birlikte taşınır.</p>

<div class="codeblock" id="code">
 <h3>DICOM dosyasını JPEG XL'ye dönüştür - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Kendi test setimizden 1714 × 1933 boyutunda 16‑bit bir görüntü üzerinde ölçtük: 6,3 MB sıkıştırılmamış, JPEG XL kayıpsızda 2,7 MB olur, ki bu aynı görüntünün HTJ2K kayıpsızından daha küçüktür. Kendi sayılarınız modaliteye bağlıdır, bu yüzden seçim yapmadan önce dosyalarınızın bulunduğu bir klasör üzerinde karşılaştırma yapın.</p>

<p>Kayıpsız kelimesi burada tam anlamıyla alınmalıdır. JPEG XL'ye ve geri dönüştürün, piksel verileri başlangıçtaki baytlarla eşittir, bu yüzden bir arşiv tanısal kalite konusunda tartışma olmadan yeniden sıkıştırılabilir.</p>

<div class="codeblock" id="code">
 <h3>Sıkıştırılmamış bir sözdizimine geri dön - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL olarak zaten depolanmış olanı okuyun">}}

<p>JPEG XL formatında gelen bir dosya diğerleri gibi açılır. Transfer sözdizimi ne olduğunu belirtir ve çerçeve çözüldükten sonra piksel verileri kullanılabilir.</p>

<div class="codeblock" id="code">
 <h3>JPEG XL dosyasını aç - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL veya HTJ2K">}}

<p>Her ikisi de yenidir, kayıpsız istendiğinde her ikisi de kayıpsızdır ve kütüphane ikisini de yazar ve okur. Farklı sorulara yanıt verirler.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Question</th>
<th>Answer</th>
</tr>
</thead>
<tbody>
<tr><td>Testimizde daha küçük dosyayı hangisi üretti</td><td>JPEG XL kayıpsız, birkaç yüzde daha az</td></tr>
<tr><td>Ağ üzerinden ilerleyici görüntüleme için hangisi tasarlandı</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, özellikle RPCL varyantı</td></tr>
<tr><td>İlk olarak hangi codec DICOM standardına eklendi</td><td>HTJ2K, bu yüzden daha fazla arşiv bugün onu kabul ediyor</td></tr>
<tr><td>Burada hangi codec yerel bağımlılık gerektirir</td><td>Hiçbiri, ikisi de tek bir derlemede yönetilen koddur</td></tr>
</tbody>
</table>

<p>Seçim genellikle bağlantının diğer ucundan gelir: arşivin kabul ettiği sözdizimine dönüştürün ve pipeline'ın geri kalanını aynı tutun.</p>

<div class="codeblock" id="code">
 <h3>Hedef arşivin karar vermesine izin ver - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nerede kazanç sağlanır">}}

<ul>
<li>Uzun vadeli arşivler: aynı çalışmalar, daha az terabayt, ve radyologa gerekçe sunulacak bir kayıp yok.</li>
<li>Bulut depolama faturaları: tasarruf her ay tekrarlanır, dönüştürme ise bir kez çalışır.</li>
<li>Araştırma ve AI için veri setleri: daha küçük kopyalar depolama ve eğitim arasında daha hızlı hareket eder.</li>
<li>Dağıtım: bu kadar yeni bir codec genellikle platform başına native derleme anlamına gelir; burada zaten referans verdiğiniz derlemenin bir parçasıdır.</li>
</ul>

<p>Kütüphane ayrıca mevcut bir arşivin içinde bulunan codec'leri de yazar: JPEG, JPEG-LS, JPEG 2000, HTJ2K ve RLE. <a href="/medical/net/dicom-transfer-syntax-conversion/">Transfer sözdizimi dönüştürme</a> sayfası tüm seti kapsar, <a href="/medical/net/htj2k/">HTJ2K</a> kendi sayfasına sahiptir ve <a href="/medical/net/jpeg2000/">JPEG 2000</a> yeni codec'lerin geldiği yerdir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokümantasyon" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Geliştirici Rehberi" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
