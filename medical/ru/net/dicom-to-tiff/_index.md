---
title: Конвертировать DICOM в TIFF на C# .NET | Aspose.Medical
weight: 12000
description: Отобразить изображения DICOM в пиксельные данные в C# .NET и сохранить в TIFF с поддержкой многостраничных файлов для многокадровых DICOM‑файлов. Используйте полноценный конвейер DICOM LUT через API Aspose.Medical и сохраняйте в TIFF любой .NET библиотекой обработки изображений.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Конвертировать DICOM в TIFF в .NET C#" h2="Отобразить изображения DICOM в необработанные пиксельные данные с полной поддержкой серого конвейера. Сохранить качество изображения с помощью управления окна/уровня, рендеринга наложений и обработки многокадровых изображений. Сохранить в TIFF &mdash; включая многостраничный TIFF &mdash; любой .NET библиотекой обработки изображений." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Преобразование DICOM в TIFF">}}

<p><strong>Aspose.Medical for .NET</strong> отображает изображения DICOM через стандартный серый конвейер DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT), создавая правильно откалиброванные пиксельные данные BGRA32. TIFF — универсальный формат, широко используемый в медицинской визуализации, научных исследованиях и публикациях, поскольку он поддерживает без потерь сжатие, большие разрёмы и &mdash; самое важное &mdash; <strong>многостраничные документы</strong>. Поэтому TIFF является естественным выбором для преобразования многокадровых DICOM‑файлов (КТ, МРТ, ультразвук) в единый файл, сохраняющий все срезы.</p>

<p>Отрендеренный вывод представляет собой <code>IRawImage</code> &mdash; буфер пикселей BGRA32 с параметрами <code>Width</code>, <code>Height</code> и массивом <code>Pixels</code>. Вы можете сохранить эти пиксельные данные в TIFF, используя любую .NET библиотеку обработки изображений, такую как <code>System.Drawing</code>, <code>SkiaSharp</code> или <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Отобразить DICOM в пиксельные данные на C#">}}

<p>Загрузите DICOM‑файл, отобразите нужный кадр и получите доступ к пиксельным данным:</p>

<div class="codeblock" id="code">
 <h3>Отобразить изображение DICOM — C#</h3>
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

<p>Метод <code>RenderImage</code> автоматически применяет значения окна/уровня и параметры LUT, сохранённые в наборе данных DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Сохранить отрендеренное изображение в TIFF">}}

<p><code>IRawImage</code> предоставляет необработанные пиксельные данные BGRA32, которые можно сохранить в TIFF любой .NET библиотекой обработки изображений. Ниже пример с использованием <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Сохранить отрендеренный кадр DICOM в TIFF — C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Многостраничный TIFF из многокадрового DICOM">}}

<p>Многокадровые файлы DICOM (например, серии КТ или МРТ) содержат несколько изображений‑срезов в одном файле. Возможность TIFF хранить несколько страниц позволяет сохранить все кадры в одном выходном файле, сохранив полное исследование в универсальном читаемом формате. Используйте TIFF‑кодировщик <code>System.Drawing</code> с параметрами многокадровости:</p>

<div class="codeblock" id="code">
 <h3>Преобразовать многокадровый DICOM в многостраничный TIFF — C#</h3>
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

<p>Вы также можете сохранять каждый кадр как отдельный TIFF‑файл, если нужны отдельные срезы:</p>

<div class="codeblock" id="code">
 <h3>Преобразовать каждый кадр в отдельный TIFF — C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Пользовательское окно/уровень">}}

<p>Контролируйте диапазон отображаемых пиксельных значений, задавая центр окна (Window Center) и ширину окна (Window Width). Разные настройки окна раскрывают различные анатомические структуры из одних и тех же данных DICOM:</p>

<div class="codeblock" id="code">
 <h3>Отобразить с пользовательским окном/уровнем — C#</h3>
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

<p>Класс <code>GrayscaleRenderOptions</code> также предоставляет свойства <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> и <code>BitDepth</code> для полного контроля над конвейером рендеринга.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Рендеринг многокадрового DICOM">}}

<p>Файлы DICOM из КТ, МРТ и ультразвука часто содержат несколько кадров (срезов). Отобразите все кадры в пиксельные данные:</p>

<div class="codeblock" id="code">
 <h3>Отобразить все кадры из многокадрового DICOM — C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Управление наложениями">}}

<p>Изображения DICOM могут содержать наложения с аннотациями, измерениями или областями интереса. Управляйте видимостью и цветом наложений при рендеринге:</p>

<div class="codeblock" id="code">
 <h3>Отобразить с наложениями и без них — C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG для медицинских изображений">}}

<p>Каждый формат вывода предназначен для разных сценариев использования:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Характеристика</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Compression</strong></td><td>Без потерь (LZW, ZIP) или без сжатия</td><td>Без потерь</td><td>С потерями</td></tr>
<tr><td><strong>Multi-Page</strong></td><td>Поддерживается &mdash; все кадры в одном файле</td><td>Не поддерживается</td><td>Не поддерживается</td></tr>
<tr><td><strong>Image Quality</strong></td><td>Pixel-perfect</td><td>Pixel-perfect</td><td>Возможны артефакты сжатия</td></tr>
<tr><td><strong>File Size</strong></td><td>Самый большой</td><td>Средний</td><td>Самый маленький</td></tr>
<tr><td><strong>Transparency</strong></td><td>Поддерживается</td><td>Поддерживается</td><td>Не поддерживается</td></tr>
<tr><td><strong>Best For</strong></td><td>Архивирование, многокадровые серии, печать, исследования</td><td>Диагностический просмотр, веб с высоким качеством</td><td>Веб‑распространение, миниатюры</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET использует один и тот же конвейер рендеринга для всех целевых форматов вывода. Пиксельные данные <code>IRawImage</code> идентичны независимо от выбранного формата — выбор формата влияет только на конечный шаг сохранения, выполняемый вашей библиотекой обработки изображений. TIFF является предпочтительным форматом, когда необходимо сохранить полное многокадровое исследование DICOM в одном файле или когда вывод предназначен для архивирования, печати или дальнейшей обработки изображений.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Учебные материалы" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Исходный код" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Справочники API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Поддержка продукта" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Бесплатная поддержка" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Платная поддержка" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Блог" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Почему Aspose.Medical для .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Список клиентов" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Истории успеха" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
