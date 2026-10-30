---
title: Сжатие DICOM JPEG 2000 на C# .NET | Aspose.Medical
weight: 2000
description: Чтение, запись и транскодирование DICOM‑файлов с сжатием JPEG 2000 на C# .NET. Поддержка монохромных и цветных изображений от 1 до 16 бит, без потерь и со сжатием, а также HTJ2K с API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Поддержка DICOM JPEG 2000 в .NET C#" h2="Чтение, запись и транскодирование DICOM‑файлов с сжатием JPEG 2000. Режимы без потерь и со сжатием, монохромные и цветные пиксельные данные от 1 до 16 бит, включён HTJ2K — всё в чистом .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 в медицинской визуализации">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) — наиболее широко используемый стандарт сжатия на основе вейвлетов в медицинской визуализации. В отличие от традиционного JPEG, он предоставляет как безпотерянное, так и сжатое с потерями, в одном кодеке, прогрессивную декодировку для доступа к интересующей области и более высокие коэффициенты сжатия &mdash; что делает его идеальным для архивации больших исследований и передачи изображений по ограниченным сетям.</p>

<p><strong>Aspose.Medical for .NET</strong> предоставляет чистую C# реализацию кодека JPEG 2000 без нативных зависимостей. Библиотека может читать, визуализировать и транскодировать DICOM‑файлы, сжатые обеими синтаксисами передачи JPEG 2000 Part 1, без потерь и с потерями.</p>

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
<tr><td>JPEG 2000 только без потерь</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Без потерь</td><td>Монохромные и цветные, 1‑16 бит</td><td>Монохромные, палитровый цвет и RGB, 1‑16 бит</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>С потерями или без потерь</td><td>Монохромные и цветные, 1‑16 бит</td><td>Монохромные и RGB, 1‑16 бит</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component только без потерь</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Без потерь</td><td>Не поддерживается</td><td>Не поддерживается</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>С потерями или без потерь</td><td>Не поддерживается</td><td>Не поддерживается</td></tr>
<tr><td>HTJ2K только без потерь</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Без потерь</td><td>Монохромные и цветные</td><td>Монохромные и цветные</td></tr>
<tr><td>HTJ2K с параметрами RPCL только без потерь</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Без потерь</td><td>Монохромные и цветные</td><td>Монохромные и цветные</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>С потерями или без потерь</td><td>Монохромные и цветные</td><td>Монохромные и цветные</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Пиксельные данные от 1 до 16 бит">}}

<p>В медицинских изображениях часто используется 16 бит на образец, чтобы захватить полный динамический диапазон таких модальностей, как КТ (обычно 12‑бит, хранящиеся в 16‑бит) и МРТ. Aspose.Medical обрабатывает любую глубину от 1 до 16 бит для JPEG 2000, в 8‑битных или 16‑битных образцах:</p>

<ul>
<li><strong>Чтение (декомпрессия)</strong>: монохромные файлы (КТ, МРТ, рентген), файлы с палитровым цветом и трехкомпонентные цветные файлы (RGB, YBR_RCT, YBR_ICT и другие цветовые метки, такие как YBR_FULL, декодируемые как кодированные) с 1‑16 битами на образец, знаковые или беззнаковые. CMYK, ICC‑профиль, палитра JP2 и субдискретные потоки цветового кода отклоняются с явным исключением вместо тихо искажённого изображения.</li>
<li><strong>Запись (компрессия)</strong>: монохромные и RGB‑изображения с 1‑16 битами на образец, знаковые или беззнаковые, а также палитровые цветные изображения с JPEG 2000 без потерь. YBR‑изображения не кодируются, если только они не декодируются в RGB, как это делают файлы JPEG YBR с потерями.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Чтение и просмотр DICOM с сжатием JPEG 2000 — C#</h3>
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

<p>Используйте метод <code>Transcode</code> для сжатия любого DICOM‑файла в JPEG 2000 или для преобразования между режимами JPEG 2000:</p>

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

{{< blocks/products/pf/feature-page-section h2="Декомпрессировать DICOM‑файлы JPEG 2000">}}

<p>Декомпрессировать файлы JPEG 2000 в несжатый синтаксис передачи для обработки, анализа или совместимости с системами, которые не поддерживают JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Декомпрессировать JPEG 2000 в несжатый формат — C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Вы также можете декомпрессировать и транскодировать в другие форматы сжатия за один шаг:</p>

<div class="codeblock" id="code">
 <h3>Транскодировать между форматами сжатия — C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Визуализировать DICOM‑изображения JPEG 2000">}}

<p>DICOM‑файлы, сжатые JPEG 2000, могут быть визуализированы в пиксельные данные для отображения или экспорта, как и любой другой синтаксис передачи:</p>

<div class="codeblock" id="code">
 <h3>Визуализировать кадр, сжатый JPEG 2000 — C#</h3>
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
<tr><td>Качество изображения</td><td>Пиксельно‑точное &mdash; идентично оригиналу</td><td>Визуально схоже, часть данных безвозвратно утрачена</td></tr>
<tr><td>Коэффициент сжатия</td><td>Обычно 2:1–3:1</td><td>Обычно 10:1–30:1 и выше</td></tr>
<tr><td>Оптимально для</td><td>Диагностическое архивирование, юридическая документация, первичное чтение</td><td>Предварительный просмотр, телемедицина, передача по сети</td></tr>
<tr><td>Безопасно при обратном преобразовании</td><td>Да</td><td>Нет &mdash; повторное перекодирование дополнительно ухудшает качество</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Высокопроизводительный JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) заменяет медленный арифметический кодировщик JPEG 2000 более быстрым блочным кодером. Он сохраняет тот же вейвлет‑преобразователь, порядок прогрессии и качество, а декодирование и кодирование происходят в несколько раз быстрее. Aspose.Medical реализует все три DICOM‑синтаксиса передачи HTJ2K в чистом .NET, для монохромных и цветных изображений, и транскодирует между HTJ2K и всеми другими поддерживаемыми синтаксисами:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; только без потерь</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; без потерь с порядком прогрессии RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; с потерями или без потерь</li>
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
{{< blocks/products/pf/slr-tab tabTitle="Обучающие материалы" tabId="resources" >}}
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
