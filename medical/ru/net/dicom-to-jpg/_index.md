---
title: Конвертировать DICOM в JPG на C# .NET | Aspose.Medical
weight: 7000
description: Отрисовывать DICOM‑изображения в пиксельные данные на C# .NET с управлением window/level, рендерингом наложений и поддержкой мульти‑кадров. Использовать полный конвейер DICOM LUT через API Aspose.Medical и сохранять в JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Конвертировать DICOM в JPG в .NET C#" h2="Отрисовывать DICOM‑изображения в необработанные пиксельные данные с полной поддержкой градационного конвейера, включая window/level, Modality LUT, VOI LUT и рендеринг наложений. Сохранять в JPG с помощью любой .NET библиотеки обработки изображений." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Конвейер отрисовки DICOM‑изображений">}}

<p><strong>Aspose.Medical for .NET</strong> отрисовывает DICOM‑изображения через стандартный градационный конвейер DICOM, определённый в спецификации DICOM. Процесс отрисовки применяет цепочку таблиц поиска (LUT) для преобразования необработанных сохранённых пиксельных значений в отображаемые изображения:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; преобразует сохранённые пиксельные значения в независимые от производителя значения с помощью Rescale Slope/Intercept или последовательности LUT.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; применяет window/level (Window Center и Window Width) для выбора диапазона отображаемых значений, поддерживая трансформации Linear, Linear Exact и Sigmoid.</li>
<li><strong>Presentation LUT</strong> &mdash; отображает финальные значения в выводимые P‑Values, включая поддержку INVERSE (инверсия изображения).</li>
<li><strong>Output LUT</strong> &mdash; преобразует градационные значения в RGB для отображения, с опциональной колоризацией с использованием пользовательских цветовых карт или таблиц Palette Color.</li>
</ol>

<p>Отрисованный результат — это <code>IRawImage</code> &mdash; буфер пикселей BGRA32 (8‑бит на канал) с полями <code>Width</code>, <code>Height</code> и массивом <code>Pixels</code>. Затем можно сохранить эти пиксельные данные в JPG с помощью любой .NET библиотеки обработки изображений, такой как <code>System.Drawing</code>, <code>SkiaSharp</code> или <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Отрисовать DICOM в пиксельные данные на C#">}}

<p>Загрузите DICOM‑файл и отрисуйте кадр в <code>IRawImage</code>, содержащий пиксельные данные BGRA32:</p>

<div class="codeblock" id="code">
 <h3>Отрисовка DICOM‑изображения - C#</h3>
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

<p>Метод <code>RenderImage</code> автоматически применяет полный градационный конвейер, используя значения window/level и параметры LUT, хранящиеся в DICOM‑файле.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Сохранить отрисованное изображение в JPG">}}

<p>Объект <code>IRawImage</code> предоставляет необработанные пиксельные данные BGRA32, которые можно сохранить в JPG с помощью любой .NET библиотеки обработки изображений. Пример с использованием <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Сохранить отрисованный кадр DICOM в JPG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Пользовательское Window/Level (окно/уровень)">}}

<p>Window/level (также называемый windowing или W/L) — самый важный параметр при отрисовке DICOM‑изображений. Он определяет, какой диапазон пиксельных значений будет отображён в видимом градационном диапазоне. <strong>Window Center</strong> задаёт центральную точку, а <strong>Window Width</strong> определяет диапазон отображаемых значений:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; высокий контраст, отображается меньше уровней серого (полезно для мягких тканей).</li>
<li><strong>Wide window</strong> &mdash; низкий контраст, отображается больше уровней серого (полезно для кости или лёгких).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Отрисовка с пользовательским window/level - C#</h3>
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

<p>Распространённые предустановки CT‑окна:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Тип ткани</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Мягкие ткани</td><td>40</td><td>400</td></tr>
<tr><td>Лёгкие</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Кость</td><td>400</td><td>1800</td></tr>
<tr><td>Мозг</td><td>40</td><td>80</td></tr>
<tr><td>Печень</td><td>60</td><td>150</td></tr>
<tr><td>Средостение</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Опции градационной отрисовки">}}

<p>Класс <code>GrayscaleRenderOptions</code> предоставляет полный контроль над градационным конвейером отрисовки:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Свойство</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Центр диапазона отображаемых значений (горизонтальная середина VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Ширина диапазона отображаемых значений (регулирует контраст)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Коэффициент пересчёта Modality LUT &mdash; умножается на сохранённые пиксельные значения</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Смещение пересчёта Modality LUT &mdash; добавляется после умножения на коэффициент</td></tr>
<tr><td><code>Invert</code></td><td>Инвертирует градационное изображение (применяет форму INVERSE Presentation LUT)</td></tr>
<tr><td><code>BitDepth</code></td><td>Информация о разрядности: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Инвертированная градационная отрисовка - C#</h3>
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

<p>Используйте <code>RenderOptionsFactory.Create(pixelData)</code> для автоматического заполнения опций отрисовки из атрибутов пиксельных данных набора DICOM, включая разрядность, параметры пересчёта, window/level и настройки инверсии.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Рендеринг наложений">}}

<p>DICOM‑изображения могут содержать наложения &mdash; графику или текстовые аннотации, вписанные в изображение. Класс <code>RenderOptions</code> управляет тем, будут ли отображаться наложения и какого они будут цвета:</p>

<div class="codeblock" id="code">
 <h3>Отрисовка с управлением наложениями - C#</h3>
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

<p>DICOM поддерживает два типа наложений: наложения <strong>Graphics</strong> (аннотации, измерения) и наложения <strong>Region of Interest</strong> (ROI). Оба типа управляются параметром <code>ShowOverlays</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Отрисовка мульти‑кадрового DICOM">}}

<p>Многие DICOM‑файлы из КТ, МРТ и ультразвука содержат несколько кадров (слойов). Можно проверить количество кадров и отрисовать каждый отдельно:</p>

<div class="codeblock" id="code">
 <h3>Отрисовать все кадры из мульти‑кадрового DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Типы трансформаций VOI LUT">}}

<p>Стандарт DICOM определяет несколько функций трансформации VOI LUT, которые управляют применением сопоставления window/level. Aspose.Medical for .NET поддерживает все стандартные типы:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Тип</th>
<th>Описание</th>
<th>Сценарий применения</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Стандартное линейное отображение со смещением от центра</td><td>Самый распространённый, используется для общих CT/MR‑изображений</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Линейное отображение без смещения от центра</td><td>Когда требуется точное сопоставление пиксельных значений</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>Трансформация S‑кривой для более плавных переходов контрастности</td><td>Визуализация мягких тканей, маммография</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>Пользовательский LUT, определённый как таблица поиска в наборе DICOM</td><td>Кривые отображения, специфичные для производителя или модальности</td></tr>
</tbody>
</table>

<p>Движок отрисовки автоматически выбирает соответствующую трансформацию VOI LUT на основе атрибутов набора DICOM. При использовании пользовательских <code>GrayscaleRenderOptions</code> по умолчанию применяется линейная трансформация.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Поддерживаемые модальности">}}

<p>Aspose.Medical for .NET поддерживает отрисовку DICOM‑изображений со всех распространённых модальностей медицинской визуализации:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Модальность</th>
<th>Описание</th>
<th>Типичный формат</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Computed Tomography</td><td>Градационный, мульти‑кадровый, 12&ndash;16 бит</td></tr>
<tr><td><strong>MR</strong></td><td>Magnetic Resonance</td><td>Градационный, мульти‑кадровый, 12&ndash;16 бит</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Computed / Digital Radiography</td><td>Градационный, одиночный кадр, 10&ndash;16 бит</td></tr>
<tr><td><strong>US</strong></td><td>Ultrasound</td><td>Градационный или RGB, мульти‑кадровый</td></tr>
<tr><td><strong>MG</strong></td><td>Mammography</td><td>Градационный, высокое разрешение, 12&ndash;16 бит</td></tr>
<tr><td><strong>XA</strong></td><td>X-Ray Angiography</td><td>Градационный, мульти‑кадровый</td></tr>
<tr><td><strong>NM</strong></td><td>Nuclear Medicine</td><td>Градационный, мульти‑кадровый</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Positron Emission Tomography)</td><td>Градационный, мульти‑кадровый</td></tr>
</tbody>
</table>

<p>Поддерживаются как градационные (MONOCHROME1/MONOCHROME2), так и цветовые (RGB, YBR_FULL, PALETTE COLOR) фотометрические интерпретации. Движок отрисовки автоматически обрабатывает соответствующее преобразование цветового пространства.</p>

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

{{< blocks/products/pf/slr-tab tabTitle="Почему Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Список клиентов" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Истории успеха" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
