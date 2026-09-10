---
title: Сжатие DICOM JPEG 2000 на C# .NET | Aspose.Medical
weight: 2000
description: Чтение, запись и транскодирование DICOM‑файлов с сжатием JPEG 2000 на C# .NET. Поддержка 8‑битных и 16‑битных изображений, режимов без потерь и с потерями, многокомпонентных данных с помощью API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Поддержка DICOM JPEG 2000 в .NET C#" h2="Чтение, запись и транскодирование DICOM‑файлов с сжатием JPEG 2000. Режимы без потерь и с потерями, 8‑битные и 16‑битные пиксельные данные, многокомпонентные изображения — всё в чистом .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 в медицинской визуализации">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) является наиболее широко используемым стандартом вейвлет‑сжатия в медицинской визуализации. В отличие от традиционного JPEG, он предоставляет как безпотерьное, так и с‑потерями сжатие в одном кодеке, прогрессивную декодировку для доступа к интересующим областям и превосходные коэффициенты сжатия &mdash; что делает его идеальным для архивирования больших исследований и передачи изображений по ограниченным сетям.</p>

<p><strong>Aspose.Medical for .NET</strong> предоставляет чистую реализацию JPEG 2000 codec на C# без нативных зависимостей. Библиотека может читать, визуализировать и транскодировать DICOM‑файлы, сжатые любой из четырёх стандартных синтаксисов передачи JPEG 2000.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Поддерживаемые синтаксисы передачи JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Синтаксис передачи</th>
<th>UID</th>
<th>Режим</th>
<th>Чтение</th>
<th>Запись</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 только без потерь</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Без потерь</td><td>8‑битный и 16‑битный</td><td>8‑битный</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>С потерями или без потерь</td><td>8‑битный и 16‑битный</td><td>8‑битный</td></tr>
<tr><td>JPEG 2000 Part 2 многокомпонентный только без потерь</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Без потерь</td><td>8‑битный и 16‑битный</td><td>8‑битный</td></tr>
<tr><td>JPEG 2000 Part 2 многокомпонентный</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>С потерями или без потерь</td><td>8‑битный и 16‑битный</td><td>8‑битный</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8‑битные и 16‑битные пиксельные данные">}}

<p>Медицинские изображения часто используют 16 бит на образец, чтобы захватить полный динамический диапазон таких модальностей, как КТ (обычно 12‑бит хранится в 16‑битном формате) и МРТ. Aspose.Medical обрабатывает обе разрядности для JPEG 2000:</p>

<ul>
<li><strong>Чтение (декомпрессия)</strong>: Полная поддержка как 8‑битных, так и 16‑битных DICOM‑файлов, сжатых JPEG 2000. Библиотека правильно декодирует пиксельные данные независимо от исходных значений Bits Allocated, Bits Stored и High Bit.</li>
<li><strong>Запись (сжатие)</strong>: В текущей версии поддерживает только 8‑битные изображения. Поддержка записи 16‑битных планируется в будущих версиях.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Чтение и просмотр DICOM, сжатых JPEG 2000 — C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Транскодировать в JPEG 2000">}}

<p>Используйте метод <code>Transcode</code> для сжатия любого DICOM‑файла в JPEG 2000 или для конвертации между режимами JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Сжать DICOM в JPEG 2000 без потерь — C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Сжать DICOM в JPEG 2000 с потерями — C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Декодировать DICOM‑файлы JPEG 2000">}}

<p>Декодируйте файлы JPEG 2000 в несжатый синтаксис передачи для обработки, анализа или совместимости с системами, не поддерживающими JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Декодировать JPEG 2000 в несжатый формат — C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Вы также можете декодировать и транскодировать в другие форматы сжатия за один шаг:</p>

<div class="codeblock" id="code">
 <h3>Транскодировать между форматами сжатия — C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Отобразить DICOM‑изображения JPEG 2000">}}

<p>DICOM‑файлы, сжатые JPEG 2000, могут быть отрисованы в пиксельные данные для отображения или экспорта, так же как любые другие синтаксисы передачи:</p>

<div class="codeblock" id="code">
 <h3>Отрисовать кадр, сжатый JPEG 2000 — C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Без потерь vs с потерями JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Аспект</th>
<th>JPEG 2000 без потерь</th>
<th>JPEG 2000 с потерями</th>
</tr>
</thead>
<tbody>
<tr><td>Синтаксис передачи</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Качество изображения</td><td>Идеальная точность &mdash; идентично оригиналу</td><td>Визуально схоже, часть данных безвозвратно утеряна</td></tr>
<tr><td>Коэффициент сжатия</td><td>Обычно 2:1 — 3:1</td><td>Обычно 10:1 — 30:1 и выше</td></tr>
<tr><td>Оптимально для</td><td>Диагностическое архивирование, юридические записи, первичное чтение</td><td>Предварительный просмотр, телемедицина, передача по сети</td></tr>
<tr><td>Безопасно при обратном переходе</td><td>Да</td><td>Нет &mdash; повторное кодирование дополнительно ухудшает качество</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 многокомпонентный">}}

<p>JPEG 2000 Part 2 (ISO/IEC 15444-2) расширяет стандартный кодек возможностями многокомпонентного преобразования. Это используется для цветных медицинских изображений и модальностей, генерирующих многоканальные данные. Aspose.Medical поддерживает оба синтаксиса передачи Part 2:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; безпотерьное сжатие с декорреляцией между компонентами для оптимального сжатия многоканальных данных.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; сжатие с потерями или без потерь с многокомпонентными преобразованиями.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K) — Скоро">}}

<p>HTJ2K (ISO/IEC 15444-15) — это наследующее поколение расширения JPEG 2000, разработанное для существенно более быстрых процессов кодирования и декодирования при сохранении той же эффективности сжатия. Ожидается, что он станет предпочтительным кодеком для реального времени в рабочих процессах медицинской визуализации.</p>

<p>Aspose.Medical добавит поддержку HTJ2K в будущих версиях, охватывая три синтаксиса передачи:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; только без потерь</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; без потерь с порядком прогрессии RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; с потерями или без потерь</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Обучающие материалы" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Исходный код" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Справочник API" href="https://reference.aspose.com/medical/net/" >}}
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
