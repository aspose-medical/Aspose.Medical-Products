---
title: C# .NET'te DICOM Transfer Syntax Dönüştürme | Aspose.Medical
weight: 16000
description: C# .NET'te DICOM dosyalarını transfer syntax'ları arasında dönüştürün. JPEG, JPEG 2000, JPEG-LS, RLE ve sıkıştırılmamış formatlar için Aspose.Medical API desteği.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'te DICOM Transfer Syntax Dönüştürme" h2="Sıkıştırılmamış, JPEG, JPEG 2000, JPEG-LS ve RLE transfer syntax'ları arasında DICOM dosyalarını dönüştürün. Yerel bağımlılıkları olmayan saf .NET kütüphanesi." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Transfer Syntax Nedir?">}}

<p>A <strong>Transfer Syntax</strong> DICOM verilerinin depolama ve iletim için nasıl kodlandığını tanımlar. Üç temel yönü belirtir: bayt sırası (endianness), Value Representation'ların açık mı yoksa örtülü mü olduğu ve piksel verisine uygulanan sıkıştırma algoritması. Her DICOM dosyası, Transfer Syntax'ını File Meta Information başlığında bildirir.</p>

<p>Farklı tıbbi cihazlar, PACS sunucuları ve görüntüleme uygulamaları farklı transfer syntax setlerini destekler. <strong>Aspose.Medical for .NET</strong>, transfer syntax'ları arasında dönüştürme sağlayan <code>Transcode</code> metodunu sunar; bu sayede birlikte çalışabilirlik, depolama optimizasyonu ve işleme araçlarıyla uyumluluk sağlanır &mdash; tümü yerel bağımlılıkları olmayan saf bir .NET kütüphanesinde.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#'te bir DICOM Dosyasını Dönüştürün">}}

<p><code>DicomFile.Transcode</code> metodu, bir DICOM dosyasını mevcut transfer syntax'ından desteklenen hedef syntax'lardan birine dönüştürür. Metod, yeni bir <code>DicomFile</code> örneği döndürür &mdash; orijinali değişmeden kalır:</p>

<div class="codeblock" id="code">
 <h3>Temel DICOM transcoding - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Ayrıca <code>Dataset</code> seviyesinde doğrudan dönüştürme yapabilirsiniz:</p>

<div class="codeblock" id="code">
 <h3>Bir Dataset'i Dönüştürün - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Desteklenen Transfer Syntax'lar">}}

<p>Aşağıdaki tablo, tüm standart DICOM görüntü verisi transfer syntax'larını ve Aspose.Medical for .NET'te mevcut destek durumlarını listeler. Desteklenen tüm codec'ler saf C# ile uygulanmış olup tamamen platform bağımsızdır.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Tür</th>
<th>Durum</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Sıkıştırılmamış</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Sıkıştırılmamış</td><td>Destekleniyor</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Sıkıştırılmamış</td><td>Destekleniyor</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Sıkıştırılmamış</td><td>Destekleniyor</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Destekleniyor</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Kayıplı, 8-bit</td><td>Destekleniyor</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Kayıplı, 12-bit</td><td>Desteklenmiyor</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Kayıpsız</td><td>Destekleniyor (yalnızca 8-bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Kayıpsız</td><td>Destekleniyor (yalnızca 8-bit)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Kayıpsız</td><td>Destekleniyor</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Neredeyse kayıpsız</td><td>Destekleniyor</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Kayıpsız</td><td>Destekleniyor (okuma 8/16-bit, yazma 8-bit)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Kayıplı veya kayıpsız</td><td>Destekleniyor (okuma 8/16-bit, yazma 8-bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Kayıpsız</td><td>Destekleniyor (okuma 8/16-bit, yazma 8-bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Kayıplı veya kayıpsız</td><td>Destekleniyor (okuma 8/16-bit, yazma 8-bit)</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Kayıpsız</td><td>Destekleniyor</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Kayıpsız</td><td>Yakında</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Kayıpsız</td><td>Yakında</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Kayıplı veya kayıpsız</td><td>Yakında</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Kayıpsız</td><td>Yakında</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Kayıpsız</td><td>Yakında</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Kayıplı veya kayıpsız</td><td>Yakında</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Yaygın Dönüştürme Senaryoları">}}

<p>Farklı iş akışları farklı dönüştürme stratejileri gerektirir. İşte en yaygın senaryolar:</p>

<div class="codeblock" id="code">
 <h3>İşleme için sıkıştırmayı kaldır - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Arşivleme depolama için sıkıştır - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Ağ iletimi için sıkıştır - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transfer Syntax Özelliklerini İnceleyin">}}

<p><code>TransferSyntax</code> sınıfı, kodlama özelliklerini tanımlayan özellikleri açığa çıkarır. Bunları bir dosyanın mevcut transfer syntax'ını incelemek veya uygun bir hedef syntax seçmek için kullanın:</p>

<div class="codeblock" id="code">
 <h3>Transfer syntax özelliklerini okuyun - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>Özellik</th>
<th>Tip</th>
<th>Açıklama</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>Transfer syntax'ın benzersiz tanımlayıcısı</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Value Representation'ların açıkça kodlanıp kodlanmadığı</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Bayt sıralamasının little endian olup olmadığı</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Piksel verisinin kapsüllenip (sıkıştırılmış) olup olmadığı</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Sıkıştırma yönteminin kayıplı olup olmadığı</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Syntax'ın deflate sıkıştırması kullanıp kullanmadığı</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Transfer syntax'ın DICOM standardı tarafından kullanımdan kaldırılıp kaldırılmadığı</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>Kayıplı sıkıştırma yönteminin ISO standart kimliği</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kayıplı vs Kayıpsız Sıkıştırma">}}

<p>Kayıplı ve kayıpsız sıkıştırma arasındaki farkı anlamak, DICOM dosyalarını dönüştürürken kritiktir:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Alan</th>
<th>Kayıpsız</th>
<th>Kayıplı</th>
</tr>
</thead>
<tbody>
<tr><td>Görüntü kalitesi</td><td>Piksel kusursuz &mdash; orijinal veri tamamen korunur</td><td>Daha küçük boyut için bazı veriler kalıcı olarak kaybedilir</td></tr>
<tr><td>Sıkıştırma oranı</td><td>Genellikle 2:1 ila 3:1</td><td>Genellikle 10:1 ila 30:1 veya daha yüksek</td></tr>
<tr><td>Geri dönüşüm güvenli</td><td>Evet &mdash; sıkıştırmayı kaldır ve aynı pikselleri al</td><td>Hayır &mdash; her kayıplı yeniden kodlama kaliteyi daha da düşürür</td></tr>
<tr><td>Kullanım durumları</td><td>Arşivleme, tanı, yasal kayıtlar</td><td>Ön inceleme, tele-tıp, ağ iletimi</td></tr>
<tr><td>Desteklenen codec'ler</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>Önemli:</strong> Kayıplı sıkıştırılmış bir dosyayı kayıpsız bir syntax'a dönüştürmek, kaybolan verileri geri getirmez. Orijinal kayıplı sıkıştırmadan kaynaklanan kalite kaybı kalıcıdır.</p>

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
