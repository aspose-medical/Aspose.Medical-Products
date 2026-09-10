---
title: C# .NET'te DICOM JPEG 2000 Sıkıştırması | Aspose.Medical
weight: 2000
description: C# .NET'te JPEG 2000 sıkıştırmasıyla DICOM dosyalarını okuyun, yazın ve dönüştürün. 8-bit renk ve 16-bit monokrom görüntüler, kayıpsız ve kayıplı modlar ve Aspose.Medical API ile HTJ2K desteği.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'de DICOM JPEG 2000 Desteği" h2="JPEG 2000 sıkıştırmasıyla DICOM dosyalarını okuyun, yazın ve dönüştürün. Kayıpsız ve kayıplı modlar, 8-bit renk ve 16-bit monokrom piksel verisi, HTJ2K dahildir - tamamen saf .NET ile." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Tıbbi Görüntülemede JPEG 2000">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444), tıbbi görüntülemede en yaygın kullanılan dalga boyuna dayalı sıkıştırma standardıdır. Geleneksel JPEG'den farklı olarak, tek bir codec içinde hem kayıpsız hem kayıplı sıkıştırma, ilgi alanı erişimi için ilerlemeli kod çözme ve üstün sıkıştırma oranları &mdash; büyük çalışmaları arşivlemek ve sınırlı ağlarda görüntüleri iletmek için ideal kılar.</p>

<p><strong>Aspose.Medical for .NET</strong>, herhangi bir yerel bağımlılık olmadan JPEG 2000 codec'inin saf C# uygulamasını sunar. Kütüphane, dört standart JPEG 2000 transfer sözdiziminden herhangi biriyle sıkıştırılmış DICOM dosyalarını okuyabilir, işleyebilir ve dönüştürebilir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Desteklenen JPEG 2000 Transfer Sözdizimleri">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Sözdizimi</th>
<th>UID</th>
<th>Mod</th>
<th>Okuma</th>
<th>Yazma</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 Yalnızca Kayıpsız</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Kayıpsız</td><td>8-bit RGB, 16-bit monokrom</td><td>16-bit monokrom, 8-bit RGB</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Kayıplı veya kayıpsız</td><td>8-bit RGB, 16-bit monokrom</td><td>16-bit monokrom, 8-bit RGB</td></tr>
<tr><td>JPEG 2000 Bölüm 2 Çok Bileşenli Yalnızca Kayıpsız</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Kayıpsız</td><td>Desteklenmiyor</td><td>Desteklenmiyor</td></tr>
<tr><td>JPEG 2000 Bölüm 2 Çok Bileşenli</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Kayıplı veya kayıpsız</td><td>Desteklenmiyor</td><td>Desteklenmiyor</td></tr>
<tr><td>HTJ2K Yalnızca Kayıpsız</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Kayıpsız</td><td>Monokrom ve renk</td><td>Monokrom ve renk</td></tr>
<tr><td>HTJ2K RPCL Seçenekli Yalnızca Kayıpsız</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Kayıpsız</td><td>Monokrom ve renk</td><td>Monokrom ve renk</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Kayıplı veya kayıpsız</td><td>Monokrom ve renk</td><td>Monokrom ve renk</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8-Bit ve 16-Bit Piksel Verisi">}}

<p>Tıbbi görüntüler, CT (genellikle 12-bit, 16-bit içinde depolanan) ve MRI gibi modalitelerin tam dinamik aralığını yakalamak için genellikle örnek başına 16 bit kullanır. Aspose.Medical, JPEG 2000 için her iki bit derinliğini de destekler:</p>

<ul>
<li><strong>Okuma (kodu çözme)</strong>: 16-bit monokrom dosyalar (CT, MRI, X-ray) ve 8-bit üç bileşenli renk dosyaları (RGB, YBR_RCT, YBR_ICT). Palet, CMYK, ICC-profili ve alt örneklenmiş renk kod akışları, sessizce hatalı bir görüntü üretmek yerine açık bir istisna ile reddedilir.</li>
<li><strong>Yazma (sıkıştırma)</strong>: 16-bit monokrom ve 8-bit RGB görüntüler. 8-bit monokrom ve 16-bit renk kodlaması mevcut değildir; bunlar için HTJ2K veya JPEG XL kullanın, ikisi de monokrom ve renk için her iki bit derinliğini kabul eder.</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 sıkıştırılmış DICOM'u okuyun ve inceleyin - C#</h3>
 <pre><code class="cs">// Open a JPEG 2000 compressed DICOM file (8-bit or 16-bit)
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Check transfer syntax
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax
Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");

// Inspect pixel data bit depth
PixelData pixelData = PixelData.Create(dicomFile.Dataset);
Console.WriteLine($"Bits Allocated: {pixelData.BitsAllocated}");
Console.WriteLine($"Bits Stored: {pixelData.BitsStored}");
Console.WriteLine($"High Bit: {pixelData.HighBit}");
Console.WriteLine($"Samples Per Pixel: {pixelData.SamplesPerPixel}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000'e Dönüştür">}}

<p>Herhangi bir DICOM dosyasını JPEG 2000'e sıkıştırmak veya JPEG 2000 modları arasında dönüştürmek için <code>Transcode</code> metodunu kullanın:</p>

<div class="codeblock" id="code">
 <h3>DICOM'u JPEG 2000 Kayıpsız Sıkıştır - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>DICOM'u JPEG 2000 Kayıplı Sıkıştır - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM Dosyalarını Aç">}}

<p>İşleme, analiz veya JPEG 2000'ı desteklemeyen sistemlerle uyumluluk için JPEG 2000 dosyalarını sıkıştırılmamış bir transfer sözdizimine açın:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000'i Sıkıştırılmamış Formata Aç - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Ayrıca tek adımda açıp diğer sıkıştırma formatlarına dönüştürebilirsiniz:</p>

<div class="codeblock" id="code">
 <h3>Sıkıştırma formatları arasında dönüştür - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM Görüntülerini Görüntüle">}}

<p>JPEG 2000 sıkıştırılmış DICOM dosyaları, diğer tüm transfer sözdizimleri gibi, görüntüleme veya dışa aktarma için piksel verisine işlenebilir:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 sıkıştırılmış bir çerçeveyi render et - C#</h3>
 <pre><code class="cs">// Open JPEG 2000 DICOM file
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Render the first frame — decompression is handled automatically
using PixelImage&lt;Bgra32&gt; image = dicomFile.RenderImage(0);

// Copy the BGRA32 pixel data for display or export
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = new Bgra32[width * height];
image.CopyPixelsTo(pixels);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kayıpsız vs Kayıplı JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Özellik</th>
<th>JPEG 2000 Kayıpsız</th>
<th>JPEG 2000 Kayıplı</th>
</tr>
</thead>
<tbody>
<tr><td>Transfer Sözdizimi</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Görüntü kalitesi</td><td>Piksel mükemmelliği &mdash; orijinale tamamen aynı</td><td>Görsel olarak benzer, bazı veriler kalıcı olarak kaybedilir</td></tr>
<tr><td>Sıkıştırma oranı</td><td>Genellikle 2:1 - 3:1</td><td>Genellikle 10:1 - 30:1 veya daha yüksek</td></tr>
<tr><td>En Uygun</td><td>Tanısal arşivleme, yasal kayıtlar, birincil okuma</td><td>İlk inceleme, tele-tıp, ağ üzerinden iletim</td></tr>
<tr><td>Geri dönüş güvenliği</td><td>Evet</td><td>Hayır &mdash; yeniden kodlama kaliteyi daha da düşürür</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Yüksek Hızlı JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15), JPEG 2000'ın yavaş aritmetik kodlayıcısını daha hızlı bir blok kodlayıcı ile değiştirir. Aynı dalga boyu dönüşümü, ilerleme sırası ve kaliteyi korur ve kod çözme ve kodlama işlemlerini birkaç kat daha hızlı gerçekleştirir. Aspose.Medical, saf .NET'te tüm üç DICOM HTJ2K transfer sözdizimini, monokrom ve renkli görüntüler için uygular ve HTJ2K ile diğer tüm desteklenen sözdizimleri arasında dönüştürme yapar:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; yalnızca kayıpsız</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; RPCL ilerleme sırası ile kayıpsız</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; kayıplı veya kayıpsız</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000'i HTJ2K'ye ve geri dönüştür - C#</h3>
 <pre><code class="cs">// Transcode a JPEG 2000 file to HTJ2K, and back to classic JPEG 2000
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile htj2kFile = j2kFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2kFile.Save("htj2k_lossless.dcm");

// HTJ2K with RPCL progression order, lossless
DicomFile rpclFile = j2kFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
rpclFile.Save("htj2k_rpcl.dcm");

// HTJ2K lossy
DicomFile htj2kLossy = j2kFile.Transcode(TransferSyntax.HTJ2K);
htj2kLossy.Save("htj2k_lossy.dcm");

// Any HTJ2K file decodes back to an uncompressed transfer syntax
DicomFile uncompressed = htj2kFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decoded.dcm");</code></pre>
</div>

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

{{< blocks/products/pf/slr-tab tabTitle="Aspose.Medical .NET için neden tercih edilmeli?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Müşteri Listesi" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Başarı Hikayeleri" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
