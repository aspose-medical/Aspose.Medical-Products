---
title: C# .NET'te DICOM'u TIFF'e Dönüştür | Aspose.Medical
weight: 12000
description: C# .NET'te DICOM görüntülerini piksel verisine render edin ve çok çerçeveli DICOM dosyaları için çok sayfalı destekle TIFF olarak kaydedin. Aspose.Medical API ile tam DICOM LUT işlem hattını kullanın ve .NET görüntüleme kütüphanesini kullanarak TIFF olarak kaydedin.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'te DICOM'u TIFF'e Dönüştür" h2="DICOM görüntülerini tam gri ton işleme hattı desteğiyle ham piksel verisine render edin. Pencere/level kontrolü, overlay renderleme ve çok çerçeveli işleme ile görüntü kalitesini koruyun. Herhangi bir .NET görüntüleme kütüphanesini kullanarak TIFF olarak kaydedin &mdash; çok sayfalı TIFF dahil &mdash;." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM'tan TIFF'e Dönüştürme">}}

<p><strong>Aspose.Medical for .NET</strong>, standart DICOM gri ton işleme hattı (Modality LUT, VOI LUT, Presentation LUT, Output LUT) üzerinden DICOM görüntülerini render ederek doğru pencerelemeli BGRA32 piksel verisi üretir. TIFF, kayıpsız sıkıştırma, yüksek bit derinlikleri ve &mdash; en önemlisi &mdash; <strong>çok sayfalı belgeler</strong> desteği sayesinde tıbbi görüntüleme, bilimsel araştırma ve yayıncılıkta yaygın kullanılan çok yönlü bir formattır. Bu, çok çerçeveli DICOM dosyalarını (CT, MRI, ultrason) tüm dilimlerini koruyan tek bir çıkış dosyasına dönüştürmek için TIFF'i doğal bir seçenek yapar.</p>

<p>Render edilen çıktı bir <code>IRawImage</code> &mdash; <code>Width</code>, <code>Height</code> ve bir <code>Pixels</code> dizisine sahip BGRA32 piksel tamponudur. Bu piksel verisini <code>System.Drawing</code>, <code>SkiaSharp</code> veya <code>ImageSharp</code> gibi herhangi bir .NET görüntüleme kütüphanesini kullanarak TIFF olarak kaydedebilirsiniz.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#'ta DICOM'u Piksel Verisine Render Et">}}

<p>Bir DICOM dosyasını yükleyin, istenen çerçeveyi render edin ve piksel verisine erişin:</p>

<div class="codeblock" id="code">
 <h3>Render DICOM görüntüsü - C#</h3>
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

<p><code>RenderImage</code> metodu, DICOM veri kümesinde saklanan pencere/level değerlerini ve LUT parametrelerini otomatik olarak uygular.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render Edilen Görüntüyü TIFF Olarak Kaydet">}}

<p><code>IRawImage</code>, herhangi bir .NET görüntüleme kütüphanesi kullanılarak TIFF olarak kaydedilebilen ham BGRA32 piksel verisi sağlar. İşte <code>System.Drawing</code> kullanılarak bir örnek:</p>

<div class="codeblock" id="code">
 <h3>Render edilen DICOM çerçevesini TIFF olarak kaydet - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("xray.dcm");
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

// Save as TIFF
bitmap.Save("xray.tiff", ImageFormat.Tiff);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Çok Çerçeveli DICOM'dan Çok Sayfalı TIFF">}}

<p>Çok çerçeveli DICOM dosyaları (ör. CT veya MRI serileri) tek bir dosyada birden fazla görüntü dilimi içerir. TIFF'in çok sayfalı yeteneği, tüm çerçeveleri tek bir çıkış dosyasında saklamanıza izin vererek çalışmanın tamamını evrensel olarak okunabilir bir formatta korur. <code>System.Drawing</code> TIFF kodlayıcısını çok çerçeveli parametrelerle kullanın:</p>

<div class="codeblock" id="code">
 <h3>Çok çerçeveli DICOM'u çok sayfalı TIFF'e dönüştür - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
int totalFrames = dicomFile.NumberOfFrames;

// Get the TIFF codec
var tiffCodec = ImageCodecInfo.GetImageEncoders()
    .First(c =&gt; c.FormatID == ImageFormat.Tiff.Guid);

// Multi-frame parameters
var multiFrameParams = new EncoderParameters(2);
multiFrameParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.MultiFrame);
multiFrameParams.Param[1] = new EncoderParameter(
    Encoder.Compression, (long)EncoderValue.CompressionLZW);

// Parameter for adding subsequent frames
var frameAddParams = new EncoderParameters(1);
frameAddParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.FrameDimensionPage);

// Parameter for flushing the final frame
var flushParams = new EncoderParameters(1);
flushParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.Flush);

Bitmap? tiffBitmap = null;

for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var frameBitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = frameBitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    frameBitmap.UnlockBits(bitmapData);

    if (i == 0)
    {
        // Save the first frame and start multi-page TIFF
        tiffBitmap = (Bitmap)frameBitmap.Clone();
        tiffBitmap.Save("ct_series.tiff", tiffCodec, multiFrameParams);
    }
    else
    {
        // Add subsequent frames
        tiffBitmap!.SaveAdd(frameBitmap, frameAddParams);
    }
}

// Flush and close the multi-page TIFF
tiffBitmap?.SaveAdd(flushParams);
tiffBitmap?.Dispose();</code></pre>
</div>

<p>Tek tek dilimlere ihtiyaç duyulduğunda her çerçeveyi ayrı bir TIFF dosyası olarak da kaydedebilirsiniz:</p>

<div class="codeblock" id="code">
 <h3>Her çerçeveyi ayrı bir TIFF dosyasına dönüştür - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

for (int i = 0; i &lt; dicomFile.NumberOfFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var bitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = bitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    bitmap.UnlockBits(bitmapData);
    bitmap.Save($"frame_{i:D4}.tiff", ImageFormat.Tiff);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Özel Pencere/Level">}}

<p>Pencere Merkezi ve Pencere Genişliği ayarlayarak hangi piksel değer aralığının render edileceğini kontrol edin. Farklı pencere ayarları, aynı DICOM verisinden farklı anatomik yapıları ortaya çıkarır:</p>

<div class="codeblock" id="code">
 <h3>Özel pencere/level ile render et - C#</h3>
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
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

<p><code>GrayscaleRenderOptions</code> sınıfı ayrıca renderleme işlem hattı üzerinde tam kontrol sağlamak için <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> ve <code>BitDepth</code> özelliklerini sunar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Çok Çerçeveli DICOM Renderleme">}}

<p>CT, MRI ve ultrason gibi DICOM dosyaları genellikle birden çok çerçeve (dilim) içerir. Tüm çerçeveleri piksel verisine render edin:</p>

<div class="codeblock" id="code">
 <h3>Çok çerçeveli DICOM'dan tüm çerçeveleri render et - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as TIFF using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Overlay Kontrolü">}}

<p>DICOM görüntüleri notlar, ölçümler veya ilgi bölgeleri içeren overlay katmanları barındırabilir. Render ederken overlay görünürlüğünü ve rengini kontrol edin:</p>

<div class="codeblock" id="code">
 <h3>Overlay'li ve overlay'siz render et - C#</h3>
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
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tıbbi Görüntüler için TIFF vs PNG vs JPG">}}

<p>Her çıktı formatı farklı kullanım senaryolarına hizmet eder:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Özellik</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Sıkıştırma</strong></td><td>Kayıpsız (LZW, ZIP) veya sıkıştırmasız</td><td>Kayıpsız</td><td>Kayıplı</td></tr>
<tr><td><strong>Çok Sayfalı</strong></td><td>Desteklenir &mdash; tüm çerçeveler tek dosyada</td><td>Desteklenmez</td><td>Desteklenmez</td></tr>
<tr><td><strong>Görüntü Kalitesi</strong></td><td>Piksel kusursuz</td><td>Piksel kusursuz</td><td>Sıkıştırma artefaktları olası</td></tr>
<tr><td><strong>Dosya Boyutu</strong></td><td>En büyük</td><td>Orta</td><td>En küçük</td></tr>
<tr><td><strong>Şeffaflık</strong></td><td>Desteklenir</td><td>Desteklenir</td><td>Desteklenmez</td></tr>
<tr><td><strong>En Uygun Kullanım</strong></td><td>Arşivleme, çok çerçeveli seriler, baskı, araştırma</td><td>Tanısal inceleme, kaliteyi koruyan web</td><td>Web paylaşımı, küçük resimler</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET, tüm çıktı hedefleri için aynı renderleme işlem hattını kullanır. <code>IRawImage</code> piksel verisi hedef format ne olursa olsun aynıdır &mdash; format seçimi yalnızca görüntüleme kütüphaneniz tarafından gerçekleştirilen son kaydetme adımını etkiler. TIFF, tek bir dosyada tam bir çok çerçeveli DICOM çalışmasını korumanız gerektiğinde ya da çıktı arşivleme, baskı veya ileri görüntü işleme amaçlı olduğunda tercih edilen formattır.</p>

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
