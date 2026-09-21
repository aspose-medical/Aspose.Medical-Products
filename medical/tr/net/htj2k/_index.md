---
title: HTJ2K in C# .NET - DICOM için High-Throughput JPEG 2000 | Aspose.Medical
weight: 10000

description: C#'tan High-Throughput JPEG 2000 içinde DICOM görüntülerini sıkıştırın ve okuyun. Kayıpsız HTJ2K, RPCL varyantı ve kayıplı HTJ2K, yerel codec gerektirmeden yönetilen .NET içinde uygulanmıştır.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K in .NET C#" h2="DICOM için High-Throughput JPEG 2000: standardın hızlı arşivler ve bulut görüntüleme için eklediği sıkıştırma, yönetilen C# içinde yerel bir şey kurmadan uygulanmıştır." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="HTJ2K neler değiştiriyor">}}

<p>High-Throughput JPEG 2000, JPEG 2000’ın dalga paketini ve görüntü kalitesini korur ve yavaş olmasına sebep olan bölümü değiştirir. Blok kodlayıcı yenidir ve kod çözme bir mertebe daha hızlıdır; bu nedenle DICOM standardı onu üç transfer syntax'lerde benimsemiştir ve bulut görüntüleme platformları ona geçmiştir.</p>

<p>.NET ekibi için pratik soru farklıdır: bu dosyaları gerçekten kim üretebilir. Çoğu kütüphane HTJ2K’yı yerel bir OpenJPH derlemesi üzerinden sağlar, bu da platform başına ikili, konteyner içinde bir derleme adımı ve güvenlik incelemesinin soracağı bir bağımlılık anlamına gelir. <strong>Aspose.Medical for .NET</strong> kod çözücüyü aynı paket içinde, dosyaları okuyan ve yazan yönetilen kodda uygular, böylece HTJ2K Windows, Linux ve konteyner içinde aynı şekilde çalışır, kurulacak bir şey yoktur.</p>

<p>Üç transfer syntaxı desteklenir ve üçü de okuma ve yazma yapabilir:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), High-Throughput JPEG 2000 kayıpsız.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), RPCL ilerleme sırasına sahip kayıpsız varyant.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bir çalışmayı HTJ2K ile sıkıştırın">}}

<p>Tek bir çağrı dosyayı yeni syntax’a taşır. Veri kümesi, özel etiketler ve dosya meta bilgileri onunla birlikte gider.</p>

<div class="codeblock" id="code">
 <h3>DICOM dosyasını HTJ2K'ye kod dönüştürme - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>Kendi test setimizden 1714 x 1933 16-bit bir görüntüde, dosya 6,3 MB'den 2,9 MB'ye düşer ve pikseller bit seviyesinde geri gelir. Sayılar modalite ve görüntüye göre değişir, bu yüzden kendi verinizde ölçüm yapın; bu, halihazırda sahip olduğunuz dosyalar üzerinde bir döngüdür.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kayıpsız demek kayıpsız demektir">}}

<p>Tanısal veriler neredeyse doğru bir codec'i tolerans göstermez. HTJ2K kayıpsız olarak kod dönüştürülüp geri alındığında piksel verisi, başladığınız baytlarla tamamen aynı olur; bu, arşiv yeniden sıkıştırılmadan önce kendi test süitinize ekleyebileceğiniz bir özelliktir.</p>

<div class="codeblock" id="code">
 <h3>Sıkıştırılmamış bir syntax'a geri dön - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, ağ üzerinden izleme için yapılmış varyant">}}

<p>1.2.840.10008.1.2.4.202 syntaxı aynı kayıpsız kod akışını RPCL ilerleme sırasıyla depolar: önce çözünürlük, ardından konum, ardından bileşen, ardından katman. Akışın yalnızca başlangıcını okuyan bir okuyucu, düşük çözünürlüklü tam bir görüntü elde eder; bu da bir görüntüleyicinin kontrolü dışında bir bağlantı üzerinden büyük bir çalışma açtığında ihtiyaç duyduğu şeydir.</p>

<div class="codeblock" id="code">
 <h3>RPCL ilerleme sırası ile sıkıştırma - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bir arşivin size gönderdiği şeyi okuyun">}}

<p>İşin diğer yarısı, zaten HTJ2K üreten sistemlerden gelen HTJ2K’yı kabul etmektir. Dosyayı açın, nasıl depolandığını kontrol edin ve piksel verisiyle çalışın.</p>

<div class="codeblock" id="code">
 <h3>HTJ2K dosyasını okuyun - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Çok-çerçeveli görüntüler çerçeve çerçeve işlenir, bu yüzden uzun bir seri bellek tüketimini çalışma başına değil çerçeve başına yapar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTJ2K'nın yerini bulduğu yerler">}}

<ul>
<li>Arşiv migrasyonu: depolanmış bir çalışmayı HTJ2K kayıpsız olarak yeniden sıkıştırın, depolama alanını azaltın, tanısal verileri bozulmadan tutun.</li>
<li>Bulut ve DICOMweb: kod çözme hızı, tarayıcı tarafı ya da sunucu tarafı görüntüleyicinin büyük görüntülerde anlık hissettirmesini sağlar.</li>
<li>AI veri hatları: eğitim setleri yazılmaktan çok daha sık okunur ve kod çözme süresi tekrarlanan maliyettir.</li>
<li>Konteynerler ve serverless: codec derlemenin bir parçasıdır, bu yüzden bir görüntünün derlemede yerel bir kütüphane ya da derleyiciye ihtiyacı yoktur.</li>
</ul>

<p>Kütüphane ayrıca JPEG XL'yi, standardın diğer yeni ekini ve bir arşivin barındırması muhtemel eski codec'leri de içerir: JPEG, JPEG-LS, JPEG 2000 ve RLE. <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> sayfası tüm seti kapsar, ve <a href="/medical/net/jpeg2000/">JPEG 2000</a> sayfası HTJ2K'nın türediği codec'i kapsar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokümantasyon" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Geliştirici Kılavuzu" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
