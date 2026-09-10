---
title: Сжатие DICOM JPEG 2000 в C# .NET | Aspose.Medical
weight: 2000
description: Чтение, запись и транскодинг DICOM‑файлов с сжатием JPEG 2000 в C# .NET. Поддержка 8‑битных цветных и 16‑битных монохромных изображений, режимов lossless и lossy, а также HTJ2K через API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Поддержка DICOM JPEG 2000 в .NET C#" h2="Чтение, запись и транскодинг DICOM‑файлов с сжатием JPEG 2000. Режимы lossless и lossy, 8‑битные цветные и 16‑битные монохромные пиксельные данные, включён HTJ2K — всё это в чистом .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 в медицинской визуализации">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) является самым широко используемым стандартом сжатия на основе вейвлетов в медицинской визуализации. В отличие от традиционного JPEG, он предоставляет как lossless, так и lossy сжатие в одном кодеке, прогрессивную декодировку для доступа к интересующей области и превосходные коэффициенты сжатия &mdash; что делает его идеальным для архивирования больших исследований и передачи изображений через ограниченные сети.</p>

<p><strong>Aspose.Medical for .NET</strong> предоставляет чистую реализацию JPEG 2000 codec на C# без нативных зависимостей. Библиотека может читать, визуализировать и транскодировать DICOM‑файлы, сжатые любой из четырёх стандартных JPEG 2000 transfer syntax.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Поддерживаемые JPEG 2000 Transfer Syntax">}}

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
<tr><td>JPEG 2000 (только Lossless)</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>8‑битный RGB, 16‑битный монохром</td><td>16‑битный монохром, 8‑битный RGB</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy или lossless</td><td>8‑битный RGB, 16‑битный монохром</td><td>16‑битный монохром, 8‑битный RGB</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component (только Lossless)</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Не поддерживается</td><td>Не поддерживается</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy или lossless</td><td>Не поддерживается</td><td>Не поддерживается</td></tr>
<tr><td>HTJ2K (только Lossless)</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Монохром и цвет</td><td>Монохром и цвет</td></tr>
<tr><td>HTJ2K с RPCL‑опциями (только Lossless)</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Монохром и цвет</td><td>Монохром и цвет</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy или lossless</td><td>Монохром и цвет</td><td>Монохром и цвет</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8‑битные и 16‑битные пиксельные данные">}}

<p>Медицинские изображения часто используют 16 бит на образец, чтобы захватить полный динамический диапазон таких модальностей, как КТ (обычно 12 бит, хранящиеся в 16‑битных) и МРТ. Aspose.Medical обрабатывает обе глубины цвета для JPEG 2000:</p>

<ul>
<li><strong>Чтение (декомпрессия)</strong>: 16‑битные монохромные файлы (CT, MRI, рентген) и 8‑битные трехкомпонентные цветные файлы (RGB, YBR_RCT, YBR_ICT). Паллета, CMYK, ICC‑профиль и потоки субдискретизованного цвета отклоняются с явным исключением вместо того, чтобы тихо выдавать неверное изображение.</li>
<li><strong>Запись (сжатие)</strong>: 16‑битные монохромные и 8‑битные RGB‑изображения. 8‑битный монохром и 16‑битный цвет недоступны; используйте HTJ2K или JPEG XL для этих случаев — оба поддерживают монохром и цвет любой глубины.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Чтение и проверка DICOM, сжатого JPEG 2000 — C#</h3>
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

<p>Используйте метод <code>Transcode</code> для сжатия любого DICOM‑файла в JPEG 2000 или преобразования между режимами JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Сжать DICOM в JPEG 2000 Lossless — C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Сжать DICOM в JPEG 2000 Lossy — C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Декомпрессия DICOM‑файлов JPEG 2000">}}

<p>Декомпрессировать файлы JPEG 2000 в несжатый transfer syntax для обработки, анализа или совместимости с системами, не поддерживающими JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Декомпрессировать JPEG 2000 в несжатый формат — C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Вы также можете одновременно декомпрессировать и транскодировать в другие форматы сжатия за один шаг:</p>

<div class="codeblock" id="code">
 <h3>Транскодировать между форматами сжатия — C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Визуализация DICOM‑изображений JPEG 2000">}}

<p>DICOM‑файлы, сжатые JPEG 2000, могут быть визуализированы в пиксельные данные для отображения или экспорта, так же как любые другие transfer syntax:</p>

<div class="codeblock" id="code">
 <h3>Визуализировать сжатый кадр JPEG 2000 — C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Lossless vs Lossy JPEG 2000">}}

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
<tr><td>Качество изображения</td><td>Pixel-perfect &mdash; идентично оригиналу</td><td>Визуально похожее, часть данных безвозвратно утрачена</td></tr>
<tr><td>Коэффициент сжатия</td><td>Обычно 2:1–3:1</td><td>Обычно 10:1–30:1 и выше</td></tr>
<tr><td>Оптимально для</td><td>Диагностическое архивирование, юридические документы, первичное чтение</td><td>Предварительный просмотр, телемедицина, передача по сети</td></tr>
<tr><td>Безопасно при круговом прохождении</td><td>Да</td><td>Нет &mdash; повторное перекодирование дополнительно ухудшает качество</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) заменяет медленный арифметический кодер JPEG 2000 более быстрым блочным кодером. Он сохраняет тот же вейвлет‑преобразование, порядок прогрессии и качество, а декодирует и кодирует в несколько раз быстрее. Aspose.Medical реализует все три DICOM HTJ2K transfer syntax в чистом .NET, для монохромных и цветных изображений, и выполняет транскодинг между HTJ2K и любым другим поддерживаемым синтаксом:</p>

<ul>
<li><code>HTJ2KLossful</code> (1.2.840.10008.1.2.4.201) &mdash; только lossless</li>
<li><code>HTJ2KLossfulRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; lossless с порядком прогрессии RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; lossy или lossless</li>
</ul>

<div class="codeblock" id="code">
 <h3>Транскодировать JPEG 2000 в HTJ2K и обратно — C#</h3>
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
{{< blocks/products/pf/slr-tab tabTitle="Обучающие ресурсы" tabId="resources" >}}
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
