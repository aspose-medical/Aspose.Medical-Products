---
title: Конвертация DICOM в PNG на C# .NET | Aspose.Medical
weight: 11000
description: Отображение DICOM‑изображений в пиксельные данные на C# .NET с сохранением без потерь, управлением уровнем окна/уровня и поддержкой многокадровых изображений. Используйте полный конвейер DICOM LUT с API Aspose.Medical и сохраняйте в PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Конвертация DICOM в PNG в .NET C#" h2="Отображение DICOM‑изображений в необработанные пиксельные данные с поддержкой полного градационного конвейера. Сохраняйте качество изображения с помощью управления уровнем окна/уровня, рендеринга наложений и обработки многокадровых данных. Сохраняйте в безсжимающий PNG с помощью любой .NET библиотеки обработки изображений." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Конвертация DICOM в PNG">}}

<p><strong>Aspose.Medical for .NET</strong> выводит DICOM‑изображения через стандартный градационный конвейер DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT), создавая корректно откорректированные BGRA32 пиксельные данные. PNG — формат без потерь, что делает его предпочтительным выбором, когда необходимо сохранять качество изображения без артефактов сжатия &mdash; что критично для диагностического просмотра, архивирования и исследовательских задач.</p>

<p>Отрисованный результат — это <code>IRawImage</code> &mdash; буфер пикселей BGRA32 с параметрами <code>Width</code>, <code>Height</code> и массивом <code>Pixels</code>. Вы можете сохранить эти пиксельные данные в PNG, используя любую .NET библиотеку обработки изображений, такую как <code>System.Drawing</code>, <code>SkiaSharp</code> или <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Отображение DICOM в пиксельные данные на C#">}}

<p>Загрузите DICOM‑файл, отобразите нужный кадр и получите доступ к пиксельным данным:</p>

<div class="codeblock" id="code">
 <h3>Отображение DICOM‑изображения - C#</h3>
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

<p>Метод <code>RenderImage</code> автоматически применяет значения окна/уровня и параметры LUT, хранящиеся в наборе данных DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Сохранить отрисованное изображение в PNG">}}

<p><code>IRawImage</code> предоставляет необработанные пиксельные данные BGRA32, которые можно сохранить в безсжимающий PNG с помощью любой .NET библиотеки обработки изображений. Ниже пример с использованием <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Сохранить отрисованный кадр DICOM в PNG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Пользовательское окно/уровень">}}

<p>Управляйте диапазоном отображаемых пиксельных значений, задавая центр окна (Window Center) и ширину окна (Window Width). Разные настройки окна раскрывают различные анатомические структуры из одних и тех же DICOM‑данных:</p>

<div class="codeblock" id="code">
 <h3>Отрисовка с пользовательским окном/уровнем - C#</h3>
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

<p>Класс <code>GrayscaleRenderOptions</code> также предоставляет свойства <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> и <code>BitDepth</code> для полного контроля над конвейером рендеринга.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Отображение многокадрового DICOM">}}

<p>DICOM‑файлы из КТ, МРТ и ультразвука часто содержат несколько кадров (срезов). Отобразите все кадры в пиксельные данные:</p>

<div class="codeblock" id="code">
 <h3>Отрисовать все кадры из многокадрового DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Контроль наложений">}}

<p>DICOM‑изображения могут содержать наложения с аннотациями, измерениями или областями интереса. Управляйте видимостью и цветом наложений при рендеринге:</p>

<div class="codeblock" id="code">
 <h3>Отрисовка с наложениями и без них - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG для медицинских изображений">}}

<p>Выбор между PNG и JPG зависит от сценария использования:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Характеристика</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Сжатие</strong></td><td>Без потерь</td><td>С потерями</td></tr>
<tr><td><strong>Качество изображения</strong></td><td>Идеальное по пикселям — без артефактов</td><td>Возможны небольшие артефакты сжатия</td></tr>
<tr><td><strong>Размер файла</strong></td><td>Больше (обычно 2&ndash;10 раз больше, чем JPG)</td><td>Меньше</td></tr>
<tr><td><strong>Прозрачность</strong></td><td>Поддерживается (альфа‑канал)</td><td>Не поддерживается</td></tr>
<tr><td><strong>Оптимально для</strong></td><td>Диагностический просмотр, архивирование, исследования, наложения</td><td>Веб‑распространение, миниатюры, превью</td></tr>
<tr><td><strong>Регулятивные требования</strong></td><td>Предпочтительно для критически важных к качеству процессов</td><td>Допустимо для недиагностического использования</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET использует один и тот же конвейер рендеринга для обоих целевых форматов. Пиксельные данные <code>IRawImage</code> идентичны независимо от выбранного формата — выбор формата влияет лишь на финальный шаг сохранения, реализуемый вашей библиотекой обработки изображений.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Обучающие ресурсы" tabId="resources" >}}
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
