---
title: C# .NET'te DICOM'u JPG'ye dönüştür | Aspose.Medical
weight: 7000
description: C# .NET'te DICOM görüntülerini piksel verisine işleyin, pencere/seviye kontrolü, kaplama renderleme ve çok çerçeve desteğiyle. Aspose.Medical API ile tam DICOM LUT hattını kullanın ve JPG olarak kaydedin.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'ta DICOM'u JPG'ye dönüştür" h2="DICOM görüntülerini ham piksel verisine, tam gri ton hattı desteğiyle, pencere/seviye, modalite LUT, VOI LUT ve kaplama renderlemesi dahil olmak üzere işleyin. Herhangi bir .NET görüntüleme kütüphanesi kullanarak JPG olarak kaydedin." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM Görüntü İşleme Boru Hattı">}}

<p><strong>Aspose.Medical for .NET</strong>, DICOM spesifikasyonunda tanımlanan standart DICOM gri ton hattı aracılığıyla DICOM görüntülerini işler. Renderleme süreci, ham saklanan piksel değerlerini görüntülenebilir görüntülere dönüştürmek için bir Look-Up Table (LUT) zinciri uygular:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; saklanan piksel değerlerini Rescale Slope/Intercept ya da bir LUT Sequence kullanarak üretici bağımsız değerlere dönüştürür.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; görüntülenecek değer aralığını seçmek için pencere/seviye (Window Center ve Window Width) uygular; Linear, Linear Exact ve Sigmoid dönüşümlerini destekler.</li>
<li><strong>Presentation LUT</strong> &mdash; son değerleri görüntülenebilir P-Values'e eşler; INVERSE (görsel tersleme) desteğini içerir.</li>
<li><strong>Output LUT</strong> &mdash; gri ton değerlerini ekranda göstermek için RGB'ye dönüştürür; isteğe bağlı olarak özel renk haritaları veya Palette Color tablolarıyla renklendirme sağlar.</li>
</ol>

<p>Renderlenen çıktı bir <code>IRawImage</code> &mdash; <code>Width</code>, <code>Height</code> ve bir <code>Pixels</code> dizisine sahip BGRA32 piksel tamponu (kanal başına 8-bit) dir. Bu piksel verisini, <code>System.Drawing</code>, <code>SkiaSharp</code> veya <code>ImageSharp</code> gibi herhangi bir .NET görüntüleme kütüphanesi kullanarak JPG olarak kaydedebilirsiniz.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#'ta DICOM'u Piksel Verisine İşleyin">}}

<p>Bir DICOM dosyasını yükleyin ve bir çerçeveyi BGRA32 piksel verisi içeren bir <code>IRawImage</code>'e renderleyin:</p>

<div class="codeblock" id="code">
 <h3>DICOM görüntüsünü renderle - C#</h3>
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

<p><code>RenderImage</code> metodu, DICOM dosyasında saklanan pencere/seviye değerleri ve LUT parametrelerini kullanarak tam gri ton hattını otomatik olarak uygular.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderlenmiş Görüntüyü JPG Olarak Kaydet">}}

<p><code>IRawImage</code>, herhangi bir .NET görüntüleme kütüphanesi kullanarak JPG olarak kaydedilebilen ham BGRA32 piksel verisi sağlar. İşte <code>System.Drawing</code> kullanarak bir örnek:</p>

<div class="codeblock" id="code">
 <h3>Renderlenen DICOM çerçevesini JPG olarak kaydet - C#</h3>
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

// Save as JPG
bitmap.Save("ct_scan.jpg", ImageFormat.Jpeg);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Özel Pencere/Seviye (Windowing)">}}

<p>Pencere/seviye (windowing ya da W/L olarak da adlandırılır), DICOM görüntü işleme için en önemli parametredir. Görünür gri ton aralığına hangi piksel değerlerinin eşleneceğini kontrol eder. <strong>Window Center</strong> orta noktayı, <strong>Window Width</strong> ise görüntülenen değer aralığını tanımlar:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; yüksek kontrast, daha az gri seviye gösterilir (yumuşak doku için faydalıdır).</li>
<li><strong>Wide window</strong> &mdash; düşük kontrast, daha fazla gri seviye gösterilir (kemik ya da akciğer için faydalıdır).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Özel pencere/seviye ile renderle - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window: center=40, width=400
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window: center=400, width=1800
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Lung window: center=-600, width=1500
var lung = new GrayscaleRenderOptions
{
    WindowCenter = -600,
    WindowWidth = 1500
};
IRawImage lungImage = dicomFile.RenderImage(lung, 0);</code></pre>
</div>

<p>Yaygın CT pencere ön ayarları:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Doku Tipi</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Yumuşak Doku</td><td>40</td><td>400</td></tr>
<tr><td>Akciğer</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Kemik</td><td>400</td><td>1800</td></tr>
<tr><td>Beyin</td><td>40</td><td>80</td></tr>
<tr><td>Karaciğer</td><td>60</td><td>150</td></tr>
<tr><td>Mediastinum</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gri Ton Render Seçenekleri">}}

<p><code>GrayscaleRenderOptions</code> sınıfı, gri ton renderleme hattı üzerinde tam kontrol sağlar:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Özellik</th>
<th>Açıklama</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Görüntülenen değer aralığının merkezi (VOI LUT'un yatay orta noktası)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Görüntülenen değer aralığının genişliği (kontrastı kontrol eder)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Modality LUT yeniden ölçekleme eğimi &mdash; saklanan piksel değerleriyle çarpılır</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Modality LUT yeniden ölçekleme kesişimi &mdash; eğim çarpımından sonra eklenir</td></tr>
<tr><td><code>Invert</code></td><td>Gri ton görüntüyü ters çevirir (INVERSE Presentation LUT şekli uygulanır)</td></tr>
<tr><td><code>BitDepth</code></td><td>Bit derinliği bilgileri: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Ters gri ton renderleme - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray.dcm");

// Render with inverted grayscale (white-on-black becomes black-on-white)
var options = new GrayscaleRenderOptions
{
    WindowCenter = 2048,
    WindowWidth = 4096,
    Invert = true
};

IRawImage image = dicomFile.RenderImage(options, 0);</code></pre>
</div>

<p>DICOM veri setinin piksel veri özniteliklerinden, bit derinliği, yeniden ölçekleme parametreleri, pencere/seviye ve tersleme ayarları dahil olmak üzere render seçeneklerini otomatik olarak doldurmak için <code>RenderOptionsFactory.Create(pixelData)</code> kullanın.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kaplama Renderleme">}}

<p>DICOM görüntüleri, görüntüye yerleştirilmiş grafik veya metin açıklamaları gibi kaplama düzlemleri içerebilir. <code>RenderOptions</code> sınıfı, kaplamaların renderlenip renderlenmeyeceğini ve hangi renkte görüneceklerini kontrol eder:</p>

<div class="codeblock" id="code">
 <h3>Kaplama kontrolüyle renderle - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible in white (default behavior)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions
{
    ShowOverlays = false
};
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

<p>DICOM iki kaplama türünü destekler: <strong>Graphics</strong> kaplamalar (açıklamalar, ölçümler) ve <strong>Region of Interest</strong> (ROI) kaplamaları. Her iki tür de <code>ShowOverlays</code> ayarıyla kontrol edilir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Çok Çerçeveli DICOM Renderleme">}}

<p>CT, MR ve ultrason gibi birçok DICOM dosyası birden fazla çerçeve (dilim) içerir. Çerçeve sayısını kontrol edebilir ve her birini ayrı ayrı renderleyebilirsiniz:</p>

<div class="codeblock" id="code">
 <h3>Çok-çerçeveli DICOM'dan tüm çerçeveleri renderle - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;
Console.WriteLine($"Total frames: {totalFrames}");

// Render all frames to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as JPG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="VOI LUT Dönüşüm Türleri">}}

<p>DICOM standardı, pencere/seviye eşlemesinin nasıl uygulanacağını kontrol eden çeşitli VOI LUT dönüşüm fonksiyonları tanımlar. Aspose.Medical for .NET, tüm standart türleri destekler:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tür</th>
<th>Açıklama</th>
<th>Kullanım Durumu</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Merkezden ofsetli standart lineer eşleme</td><td>En yaygın, genel CT/MR görüntülemede kullanılır</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Merkezden ofsetsiz lineer eşleme</td><td>Kesin piksel değeri eşlemesi gerektiğinde</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>Daha yumuşak kontrast geçişleri için S-eğrisi dönüşümü</td><td>Yumuşak doku görselleştirme, mamografi</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>DICOM veri setinde bir lookup tablosu olarak tanımlanan özel LUT</td><td>Satıcıya özgü veya modaliteye özgü görüntü eğrileri</td></tr>
</tbody>
</table>

<p>Renderleme motoru, DICOM veri seti özniteliklerine göre uygun VOI LUT dönüşümünü otomatik olarak seçer. Özel <code>GrayscaleRenderOptions</code> kullanıldığında, Linear dönüşüm varsayılan olarak uygulanır.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Desteklenen Modaliteler">}}

<p>Aspose.Medical for .NET, tüm yaygın tıbbi görüntüleme modalitelerinden DICOM görüntülerinin renderlenmesini destekler:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modalite</th>
<th>Açıklama</th>
<th>Tipik Biçim</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Bilgisayarlı Tomografi</td><td>Gri ton, çok çerçeve, 12&ndash;16 bit</td></tr>
<tr><td><strong>MR</strong></td><td>Manyetik Resonans</td><td>Gri ton, çok çerçeve, 12&ndash;16 bit</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Bilgisayarlı / Dijital Radiografi</td><td>Gri ton, tek çerçeve, 10&ndash;16 bit</td></tr>
<tr><td><strong>US</strong></td><td>Ultrason</td><td>Gri ton veya RGB, çok çerçeve</td></tr>
<tr><td><strong>MG</strong></td><td>Mamografi</td><td>Gri ton, yüksek çözünürlük, 12&ndash;16 bit</td></tr>
<tr><td><strong>XA</strong></td><td>X-Ray Anjiyografi</td><td>Gri ton, çok çerçeve</td></tr>
<tr><td><strong>NM</strong></td><td>Nükleer Tıp</td><td>Gri ton, çok çerçeve</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Pozitron Emisyon Tomografisi)</td><td>Gri ton, çok çerçeve</td></tr>
</tbody>
</table>

<p>Hem gri ton (MONOCHROME1/MONOCHROME2) hem de renk (RGB, YBR_FULL, PALETTE COLOR) fotometrik yorumlamalar desteklenir. Renderleme motoru uygun renk uzayı dönüşümünü otomatik olarak gerçekleştirir.</p>

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
