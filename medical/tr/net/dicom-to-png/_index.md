---
title: C# .NET'te DICOM'i PNG'ye Dönüştür | Aspose.Medical
weight: 11000
description: C# .NET'te DICOM görüntülerini piksel verisine, kayıpsız kalite, pencere/seviye kontrolü ve çoklu çerçeve desteğiyle işleyin. Aspose.Medical API ile tam DICOM LUT boru hattını kullanın ve PNG olarak kaydedin.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'ta DICOM'i PNG'ye Dönüştür" h2="DICOM görüntülerini tam gri ton boru hattı desteğiyle ham piksel verisine işleyin. Pencere/seviye kontrolü, overlay işleme ve çoklu çerçeve işleme ile görüntü kalitesini koruyun. Herhangi bir .NET görüntüleme kütüphanesi kullanarak kayıpsız PNG olarak kaydedin." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM'dan PNG'ye Dönüştürme">}}

<p><strong>Aspose.Medical for .NET</strong> DICOM görüntülerini standart DICOM gri ton boru hattı (Modality LUT, VOI LUT, Presentation LUT, Output LUT) aracılığıyla işleyerek doğru pencereleme yapılmış BGRA32 piksel verisi üretir. PNG kayıpsız bir format olduğu için sıkıştırma artefaktları olmadan görüntü kalitesinin korunması gereken durumlarda tercih edilen seçenektir — tanısal inceleme, arşivleme ve araştırma kullanım senaryoları için esastır.</p>

<p>Oluşturulan çıktı bir <code>IRawImage</code> &mdash; <code>Width</code>, <code>Height</code> ve bir <code>Pixels</code> dizisi içeren BGRA32 piksel tamponudur. Bu piksel verisini <code>System.Drawing</code>, <code>SkiaSharp</code> veya <code>ImageSharp</code> gibi herhangi bir .NET görüntüleme kütüphanesi kullanarak PNG olarak kaydedebilirsiniz.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#'ta DICOM'i Piksel Verisine İşleyin">}}

<p>Bir DICOM dosyasını yükleyin, istenen çerçeveyi işleyin ve piksel verisine erişin:</p>

<div class="codeblock" id="code">
 <h3>DICOM Görüntüsünü İşle - C#</h3>
 <pre><code class="cs">using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;

// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Render the first frame (index 0) through the full LUT pipeline
IRawImage image = dicomFile.RenderImage(0);

// Access the rendered pixel data
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = image.Pixels; // BGRA32 pixel buffer</code></pre>
</div>

<p><code>RenderImage</code> yöntemi, DICOM veri kümesinde saklanan pencere/seviye değerlerini ve LUT parametrelerini otomatik olarak uygular.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="İşlenmiş Görüntüyü PNG Olarak Kaydet">}}

<p><code>IRawImage</code>, herhangi bir .NET görüntüleme kütüphanesi kullanılarak kayıpsız PNG olarak kaydedilebilen ham BGRA32 piksel verisi sağlar. İşte <code>System.Drawing</code> kullanılarak bir örnek:</p>

<div class="codeblock" id="code">
 <h3>İşlenmiş DICOM çerçevesini PNG olarak kaydet - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");
IRawImage image = dicomFile.RenderImage(0);

// Create a Bitmap from the BGRA32 pixel data
using var bitmap = new Bitmap(image.Width, image.Height, PixelFormat.Format32bppArgb);
var bitmapData = bitmap.LockBits(
    new Rectangle(0, 0, image.Width, image.Height),
    ImageLockMode.WriteOnly,
    PixelFormat.Format32bppArgb);

// Copy BGRA32 pixels to the bitmap
MemoryMarshal.AsBytes(image.Pixels.AsSpan())
    .CopyTo(new Span&lt;byte&gt;(
        (void*)bitmapData.Scan0,
        bitmapData.Stride * bitmapData.Height));

bitmap.UnlockBits(bitmapData);

// Save as lossless PNG
bitmap.Save("ct_scan.png", ImageFormat.Png);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Özel Pencere/Seviye">}}

<p>Pencere Merkezi ve Pencere Genişliği ayarlanarak hangi piksel değer aralığının işleneceği kontrol edilir. Farklı pencere ayarları, aynı DICOM verisinden farklı anatomik yapıların ortaya çıkmasını sağlar:</p>

<div class="codeblock" id="code">
 <h3>Özel pencere/seviye ile işleyin - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);</code></pre>
</div>

<p><code>GrayscaleRenderOptions</code> sınıfı, render boru hattı üzerinde tam kontrol sağlamak için <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> ve <code>BitDepth</code> özelliklerini de sunar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Çoklu Çerçeve DICOM İşleme">}}

<p>CT, MRI ve ultrason DICOM dosyaları genellikle birden çok çerçeve (dilim) içerir. Tüm çerçeveleri piksel verisine işleyin:</p>

<div class="codeblock" id="code">
 <h3>Çoklu çerçeveli DICOM'dan tüm çerçeveleri işleyin - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as PNG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Üst Bilgi Kontrolü">}}

<p>DICOM görüntüleri, açıklamalar, ölçümler veya ilgi bölgeleri içeren overlay (üst bilgi) düzlemlerine sahip olabilir. İşleme sırasında overlay görünürlüğünü ve rengini kontrol edin:</p>

<div class="codeblock" id="code">
 <h3>Overlay'li ve overlay'siz işleyin - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible (default)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions { ShowOverlays = false };
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tıbbi Görüntülerde PNG vs JPG">}}

<p>PNG ile JPG arasında seçim, kullanım senaryosuna bağlıdır:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Özellik</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Sıkıştırma</strong></td><td>Kayıpsız</td><td>Kayıplı</td></tr>
<tr><td><strong>Görüntü Kalitesi</strong></td><td>Piksel mükemmelliği &mdash; artefakt yok</td><td>Küçük sıkıştırma artefaktları olabilir</td></tr>
<tr><td><strong>Dosya Boyutu</strong></td><td>Daha büyük (genellikle JPG'ye göre 2&ndash;10x)</td><td>Daha küçük</td></tr>
<tr><td><strong>Şeffaflık</strong></td><td>Desteklenir (alfa kanalı)</td><td>Desteklenmez</td></tr>
<tr><td><strong>En Uygun</strong></td><td>Tanısal inceleme, arşivleme, araştırma, overlay'ler</td><td>Web paylaşımı, küçük resimler, ön izlemeler</td></tr>
<tr><td><strong>Düzenleyici</strong></td><td>Kalite kritik iş akışları için tercih edilir</td><td>Tanısal olmayan kullanım için kabul edilebilir</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET, her iki çıktı hedefi için aynı render boru hattını kullanır. <code>IRawImage</code> piksel verisi hedef formatından bağımsız olarak aynı kalır &mdash; format seçimi yalnızca görüntüleme kütüphaneniz tarafından gerçekleştirilen son kaydetme adımını etkiler.</p>

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
