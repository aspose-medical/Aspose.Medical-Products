---
title: C# .NET'te DICOM JPEG 2000 Sıkıştırma | Aspose.Medical
weight: 2000
description: C# .NET'te JPEG 2000 sıkıştırmalı DICOM dosyalarını okuyun, yazın ve dönüştürün. 8-bit ve 16-bit görüntüler, kayıpsız ve kayıplı modlar, çok bileşenli veri desteği Aspose.Medical API ile.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'de DICOM JPEG 2000 Desteği" h2="JPEG 2000 sıkıştırmalı DICOM dosyalarını okuyun, yazın ve dönüştürün. Kayıpsız ve kayıplı modlar, 8-bit ve 16-bit piksel verisi, çok bileşenli görüntüler — tamamen saf .NET içinde." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Tıbbi Görüntülemede JPEG 2000">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444), tıbbi görüntülemede en yaygın kullanılan dalga tabanlı sıkıştırma standardıdır. Geleneksel JPEG'in aksine, tek bir codec içinde hem kayıpsız hem kayıplı sıkıştırma, bölge‑ilgi erişimi için ilerleyici kod çözme ve üstün sıkıştırma oranları sunar &mdash; bu da büyük çalışmaları arşivlemek ve kısıtlı ağlarda görüntüleri iletmek için idealdir.</p>

<p><strong>Aspose.Medical for .NET</strong>, native bağımlılıkları olmayan saf C# JPEG 2000 codec uygulamasını sağlar. Kütüphane, JPEG 2000 ile sıkıştırılmış DICOM dosyalarını okuyabilir, render edebilir ve dönüştürebilir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Desteklenen JPEG 2000 Transfer Syntaxes">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Mode</th>
<th>Read</th>
<th>Write</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 Sadece Kayıpsız</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Kayıpsız</td><td>8-bit ve 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Kayıplı ya da kayıpsız</td><td>8-bit ve 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 Bölüm 2 Çok Bileşenli Sadece Kayıpsız</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Kayıpsız</td><td>8-bit ve 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 Bölüm 2 Çok Bileşenli</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Kayıplı ya da kayıpsız</td><td>8-bit ve 16-bit</td><td>8-bit</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8-Bit ve 16-Bit Piksel Verisi">}}

<p>Tıbbi görüntüler genellikle CT (genellikle 12-bit olarak 16-bit içinde depolanır) ve MRI gibi modalitelerin tam dinamik aralığını yakalamak için örnek başına 16 bit kullanır. Aspose.Medical, JPEG 2000 için her iki bit derinliğini de yönetir:</p>

<ul>
<li><strong>Okuma (çözme)</strong>: 8-bit ve 16-bit JPEG 2000 sıkıştırmalı DICOM dosyaları için tam destek. Kütüphane, orijinal Bits Allocated, Bits Stored ve High Bit değerlerinden bağımsız olarak piksel verisini doğru bir şekilde çözer.</li>
<li><strong>Yazma (sıkıştırma)</strong>: Şu anda 8-bit görüntüleri desteklemektedir. 16-bit yazma desteği gelecekteki bir sürümde planlanmaktadır.</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 sıkıştırmalı DICOM'ı oku ve incele - C#</h3>
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

<p><code>Transcode</code> metodunu kullanarak herhangi bir DICOM dosyasını JPEG 2000'e sıkıştırabilir veya JPEG 2000 modları arasında dönüştürebilirsiniz:</p>

<div class="codeblock" id="code">
 <h3>DICOM'ı JPEG 2000 Kayıpsız Sıkıştır - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>DICOM'ı JPEG 2000 Kayıplı Sıkıştır - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM Dosyalarını Çöz">}}

<p>İşleme, analiz veya JPEG 2000'ı desteklemeyen sistemlerle uyumluluk için JPEG 2000 dosyalarını sıkıştırılmamış bir transfer syntax'ına çözer:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000'ı sıkıştırılmamış formata çözü - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Ayrıca tek bir adımda diğer sıkıştırma formatlarına da çözümleyebilir ve dönüştürebilirsiniz:</p>

<div class="codeblock" id="code">
 <h3>Sıkıştırma formatları arasında dönüştür - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM Görüntülerini Render Et">}}

<p>JPEG 2000 sıkıştırmalı DICOM dosyaları, diğer transfer syntax'ları gibi piksel verisine render edilerek görüntülenebilir veya dışa aktarılabilir:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 sıkıştırmalı bir çerçeveyi render et - C#</h3>
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
<th>Aspect</th>
<th>JPEG 2000 Lossless</th>
<th>JPEG 2000 Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Transfer Syntax</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Görüntü kalitesi</td><td>Piksel‑kusursuz &mdash; orijinaliyle tamamen aynı</td><td>Görsel olarak benzer, bazı veriler kalıcı olarak kaybedilir</td></tr>
<tr><td>Sıkıştırma oranı</td><td>Tipik olarak 2:1 ila 3:1</td><td>Tipik olarak 10:1 ila 30:1 veya daha yüksek</td></tr>
<tr><td>En uygun</td><td>Tanısal arşivleme, yasal kayıtlar, birincil okuma</td><td>İlk inceleme, telemedisin, ağ iletimi</td></tr>
<tr><td>Gidiş-dönüş güvenli</td><td>Evet</td><td>Hayır &mdash; yeniden kodlama kaliteyi daha da düşürür</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Bölüm 2 Çok Bileşenli">}}

<p>JPEG 2000 Bölüm 2 (ISO/IEC 15444-2), standart codec'i çok bileşenli dönüşüm yetenekleriyle genişletir. Bu, renkli tıbbi görüntüler ve çok kanallı veri üreten modaliteler için kullanılır. Aspose.Medical, her iki Bölüm 2 transfer syntax'ını da destekler:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; çok kanallı verinin optimum sıkıştırması için bileşenler arası dekorrelasyon ile kayıpsız sıkıştırma.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; çok bileşenli dönüşümlerle kayıplı ya da kayıpsız sıkıştırma.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Yüksek Hızlı JPEG 2000 (HTJ2K) — Yakında">}}

<p>HTJ2K (ISO/IEC 15444-15), JPEG 2000'ın bir sonraki nesil uzantısı olarak tasarlanmıştır; aynı sıkıştırma verimliliğini korurken kodlama ve kod çözme hızlarını büyük ölçüde artırır. Gerçek zamanlı tıbbi görüntüleme iş akışları için tercih edilen codec olacağı öngörülmektedir.</p>

<p>Aspose.Medical, gelecekteki bir sürümde üç transfer syntax'ını kapsayan HTJ2K desteği ekleyecek:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; Sadece kayıpsız</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; RPCL ilerleme sırası ile kayıpsız</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; Kayıplı ya da kayıpsız</li>
</ul>

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

{{< blocks/products/pf/slr-tab tabTitle="Neden Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Müşteri Listesi" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Başarı Hikayeleri" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
