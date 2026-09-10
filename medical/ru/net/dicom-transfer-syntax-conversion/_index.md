---
title: Конвертация синтаксиса передачи DICOM на C# .NET | Aspose.Medical
weight: 16000
description: Перекодировать файлы DICOM между синтаксами передачи на C# .NET. Поддержка JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, RLE и несжатых форматов с помощью API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Конвертация синтаксиса передачи DICOM в .NET C#" h2="Перекодировать файлы DICOM между несжатыми, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS и RLE синтаксами передачи. Чистая .NET библиотека без нативных зависимостей." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Что такое синтакс передачи?">}}

<p><strong>Transfer Syntax</strong> определяет, как данные DICOM кодируются для хранения и передачи. Он задает три ключевых аспекта: порядок байтов (endianness), явные или неявные Value Representations и алгоритм сжатия, применяемый к пиксельным данным. Каждый файл DICOM объявляет свой Transfer Syntax в заголовке File Meta Information.</p>

<p>Разные медицинские устройства, PACS‑серверы и приложения для просмотра поддерживают различные наборы Transfer Syntax. <strong>Aspose.Medical for .NET</strong> предоставляет метод <code>Transcode</code> для преобразования между Transfer Syntax, обеспечивая совместимость, оптимизацию хранения и совместимость с инструментами обработки &mdash; всё это в чистой .NET библиотеке без нативных зависимостей.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Перекодировать файл DICOM на C#">}}

<p>Метод <code>DicomFile.Transcode</code> преобразует файл DICOM из текущего Transfer Syntax в любой поддерживаемый целевой синтакс. Метод возвращает новый экземпляр <code>DicomFile</code> &mdash; оригинал остаётся неизменным:</p>

<div class="codeblock" id="code">
 <h3>Базовое перекодирование DICOM - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Вы также можете перекодировать напрямую на уровне <code>Dataset</code>:</p>

<div class="codeblock" id="code">
 <h3>Перекодировать Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Поддерживаемые Transfer Syntax">}}

<p>В следующей таблице перечислены все стандартные Transfer Syntax изображений DICOM и их текущий статус поддержки в Aspose.Medical for .NET. Все поддерживаемые кодеки реализованы на чистом C# и полностью независимы от платформы.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Тип</th>
<th>Статус</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Несжатый</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Несжатый</td><td>Поддерживается</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Несжатый</td><td>Поддерживается</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Несжатый (устаревший)</td><td>Поддерживается</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Несжатый</td><td>Не поддерживается</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Сжатый</td><td>Поддерживается</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>С потерями, 8‑бит</td><td>Поддерживается</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>С потерями, 12‑бит</td><td>Не поддерживается</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Без потерь</td><td>Поддерживается (только 8‑бит)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Без потерь</td><td>Поддерживается (только 8‑бит)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Без потерь</td><td>Поддерживается</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Почти без потерь</td><td>Поддерживается</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Без потерь</td><td>Поддерживается (чтение 8‑битного цвета и 16‑битного монохромного; запись 16‑битного монохромного или 8‑битного RGB)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>С потерями или без потерь</td><td>Поддерживается (чтение 8‑битного цвета и 16‑битного монохромного; запись 16‑битного монохромного или 8‑битного RGB)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Без потерь</td><td>Не поддерживается</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>С потерями или без потерь</td><td>Не поддерживается</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Без потерь</td><td>Поддерживается</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Без потерь</td><td>Поддерживается</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Без потерь</td><td>Поддерживается</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>С потерями или без потерь</td><td>Поддерживается</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Без потерь</td><td>Поддерживается</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Без потерь</td><td>Только декодирование (для кодирования требуется поток JPEG‑источника, а не пиксельные данные)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>С потерями или без потерь</td><td>Поддерживается (режим с потерями)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Распространённые сценарии перекодирования">}}

<p>Разные рабочие процессы требуют разных стратегий перекодирования. Ниже представлены наиболее распространённые сценарии:</p>

<div class="codeblock" id="code">
 <h3>Декомпрессия для обработки - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Сжатие для архивного хранения - C#</h3>
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
 <h3>Сжатие для передачи по сети - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Использовать новейшие кодеки: HTJ2K и JPEG XL - C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Просмотр свойств Transfer Syntax">}}

<p>Класс <code>TransferSyntax</code> раскрывает свойства, описывающие характеристики кодировки. Используйте их для проверки текущего Transfer Syntax файла или выбора подходящего целевого синтакса:</p>

<div class="codeblock" id="code">
 <h3>Чтение свойств Transfer Syntax - C#</h3>
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
<th>Свойство</th>
<th>Тип</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>Уникальный идентификатор Transfer Syntax</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Указывает, явно ли кодируются Value Representations</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Указывает, является ли порядок байтов little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Указывает, инкапсулированы ли пиксельные данные (сжаты)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Указывает, является ли метод сжатия с потерями</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Указывает, использует ли синтакс сжатие deflate</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Указывает, устарел ли Transfer Syntax согласно стандарту DICOM</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>Идентификатор метода сжатия с потерями согласно ISO‑стандарту</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Сжатие с потерями vs без потерь">}}

<p>Понимание различий между сжатием с потерями и без потерь критически важно при перекодировании файлов DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Аспект</th>
<th>Без потерь</th>
<th>С потерями</th>
</tr>
</thead>
<tbody>
<tr><td>Качество изображения</td><td>Pixel-perfect &mdash; оригинальные данные полностью сохранены</td><td>Некоторые данные безвозвратно теряются для уменьшения размера</td></tr>
<tr><td>Коэффициент сжатия</td><td>Обычно 2:1 до 3:1</td><td>Обычно 10:1 до 30:1 или выше</td></tr>
<tr><td>Безопасность обратного пути</td><td>Да &mdash; декодировать и получить идентичные пиксели</td><td>Нет &mdash; каждое повторное сжатие с потерями ухудшает качество</td></tr>
<tr><td>Сценарии использования</td><td>Архивирование, диагностика, юридические записи</td><td>Предварительный просмотр, телемедицина, передача по сети</td></tr>
<tr><td>Поддерживаемые кодеки</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>Важно:</strong> Перекодирование файла, сжатого с потерями, в синтакс без потерь не восстанавливает потерянные данные. Потеря качества от исходного сжатия с потерями является постоянной.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ресурсы для обучения" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Исходный код" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Ссылки на API" href="https://reference.aspose.com/medical/net/" >}}
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
