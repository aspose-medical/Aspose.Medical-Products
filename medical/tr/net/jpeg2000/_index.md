---
title: C# .NET'te DICOM JPEG 2000 Sıkıştırması | Aspose.Medical
weight: 2000
description: C# .NET'te JPEG 2000 sıkıştırmasıyla DICOM dosyalarını okuyun, yazın ve kod çevirisi yapın. 1 ila 16 bit arasında tek renk ve renkli görüntüler, kayıpsız ve kayıplı modlar, ayrıca Aspose.Medical API ile HTJ2K desteği.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'ta DICOM JPEG 2000 Desteği" h2="JPEG 2000 sıkıştırmasıyla DICOM dosyalarını okuyun, yazın ve kod çevirisi yapın. Kayıpsız ve kayıplı modlar, 1 ila 16 bit arasında tek renk ve renkli piksel verileri, HTJ2K dahil – hepsi saf .NET'te." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Tıbbi Görüntülemede JPEG 2000">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444), tıbbi görüntülemede en yaygın kullanılan dalgalet tabanlı sıkıştırma standardıdır. Geleneksel JPEG’den farklı olarak, tek bir codec içinde hem kayıpsız hem kayıplı sıkıştırma, ilgi bölgesi erişimi için aşamalı çözümleme ve üstün sıkıştırma oranları &mdash; büyük çalışmaların arşivlenmesi ve sınırlı ağlarda görüntülerin aktarılması için idealdir.</p>

<p><strong>Aspose.Medical for .NET</strong>, JPEG 2000 codec'inin yerel bağımlılıkları olmayan saf C# implementasyonunu sağlar. Kütüphane, JPEG 2000 Part 1 transfer sözdizimleriyle sıkıştırılmış DICOM dosyalarını okuyabilir, işleyebilir ve kod çevirebilir; hem kayıpsız hem kayıplı.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Desteklenen JPEG 2000 Transfer Sözdizimleri">}}

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
<tr><td>JPEG 2000 Sadece Kayıpsız</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Kayıpsız</td><td>Monokrom ve renkli, 1 ila 16 bit</td><td>Monokrom, palet renkli ve RGB, 1 ila 16 bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Kayıplı veya kayıpsız</td><td>Monokrom ve renkli, 1 ila 16 bit</td><td>Monokrom ve RGB, 1 ila 16 bit</td></tr>
<tr><td>JPEG 2000 Part 2 Çok Bileşenli Sadece Kayıpsız</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Kayıpsız</td><td>Desteklenmiyor</td><td>Desteklenmiyor</td></tr>
<tr><td>JPEG 2000 Part 2 Çok Bileşenli</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Kayıplı veya kayıpsız</td><td>Desteklenmiyor</td><td>Desteklenmiyor</td></tr>
<tr><td>HTJ2K Sadece Kayıpsız</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Kayıpsız</td><td>Monokrom ve renkli</td><td>Monokrom ve renkli</td></tr>
<tr><td>RPCL Seçenekli HTJ2K Sadece Kayıpsız</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Kayıpsız</td><td>Monokrom ve renkli</td><td>Monokrom ve renkli</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Kayıplı veya kayıpsız</td><td>Monokrom ve renkli</td><td>Monokrom ve renkli</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="1 ila 16 Bit Arasındaki Piksel Verisi">}}

<p>Tıbbi görüntüler, CT (genellikle 12-bit, 16-bit içinde saklanır) ve MRI gibi modalitylerin tam dinamik aralığını yakalamak için sıklıkla örnek başına 16 bit kullanır. Aspose.Medical, JPEG 2000 için 1 ila 16 bit arasındaki tüm derinlikleri, 8-bit veya 16-bit örneklerde işler:</p>

<ul>
<li><strong>Okuma (çözme)</strong>: monokrom dosyalar (CT, MRI, X‑ray), palet renkli dosyalar ve üç bileşenli renk dosyaları (RGB, YBR_RCT, YBR_ICT ve YBR_FULL gibi diğer renk etiketleri, kodlandığı gibi çözülür) 1 ila 16 bit örnek başına, işaretli veya işaretsiz. CMYK, ICC‑profil, JP2 paleti ve alt örneklenmiş renk kod akışları, sessizce hatalı bir görüntü yerine net bir istisna ile reddedilir.</li>
<li><strong>Yazma (sıkıştırma)</strong>: 1 ila 16 bit örnek başına, işaretli veya işaretsiz monokrom ve RGB görüntüler, ayrıca JPEG 2000 Kayıpsız ile palet renkli görüntüler. YBR görüntüler, RGB'ye çözümlenmedikçe kodlanmaz; bu, kayıplı JPEG YBR dosyalarının yaptığı gibidir.</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 sıkıştırmalı DICOM'i Oku ve İncele - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="JPEG 2000'e Kod Çevir">}}

<p>Herhangi bir DICOM dosyasını JPEG 2000'e sıkıştırmak veya JPEG 2000 modları arasında dönüştürmek için <code>Transcode</code> metodunu kullanın:</p>

<div class="codeblock" id="code">
 <h3>DICOM'i JPEG 2000 Kayıpsız Sıkıştır - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>DICOM'i JPEG 2000 Kayıplı Sıkıştır - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM Dosyalarını Çöz">}}

<p>JPEG 2000 dosyalarını, işleme, analiz veya JPEG 2000'ı desteklemeyen sistemlerle uyumluluk için sıkıştırılmamış bir transfer sözdizimine çözmek:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000'ı Sıkıştırmasız Hale Çöz - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Ayrıca tek adımda diğer sıkıştırma formatlarına çözebilir ve kod çevirebilirsiniz:</p>

<div class="codeblock" id="code">
 <h3>Sıkıştırma formatları arasında kod çevirisi - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM Görüntülerini İşle">}}

<p>JPEG 2000 sıkıştırmalı DICOM dosyaları, diğer transfer sözdizimleri gibi, görüntüleme ya da dışa aktarma amacıyla piksel verisine işlenebilir:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 sıkıştırmalı bir kareyi işle - C#</h3>
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
<tr><td>Görüntü kalitesi</td><td>Piksel tam &mdash; orijinaliyle aynı</td><td>Görsel olarak benzer, bazı veriler kalıcı olarak kaybedilir</td></tr>
<tr><td>Sıkıştırma oranı</td><td>Genellikle 2:1 ile 3:1</td><td>Genellikle 10:1 ile 30:1 veya daha yüksek</td></tr>
<tr><td>En İyi Kullanım</td><td>Tanısal arşivleme, yasal kayıtlar, birincil okuma</td><td>Ön inceleme, tele tıp, ağ üzerinden aktarım</td></tr>
<tr><td>Geri dönüş güvenli</td><td>Evet</td><td>Hayır &mdash; yeniden kodlama kaliteyi daha da düşürür</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Yüksek Performanslı JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15), JPEG 2000'ın yavaş aritmetik kodlayıcısını daha hızlı bir blok kodlayıcıyla değiştirir. Aynı dalgalet dönüşümü, ilerleme sıraları ve kaliteyi korur ve çözümleme ve kodlamayı birkaç kat daha hızlı yapar. Aspose.Medical, saf .NET'te üç DICOM HTJ2K transfer sözdizimini, monokrom ve renkli görüntüler için uygular ve HTJ2K ile diğer tüm desteklenen sözdizimleri arasında kod çevirisi yapar:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; sadece kayıpsız</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; RPCL ilerleme sırasıyla kayıpsız</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; kayıplı veya kayıpsız</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000'ı HTJ2K'ya ve geri kod çevir - C#</h3>
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

{{< blocks/products/pf/slr-tab tabTitle="Neden Aspose.Medical .NET için?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Müşteri Listesi" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Başarı Hikayeleri" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
