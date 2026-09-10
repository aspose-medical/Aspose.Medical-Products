---
title: Конвертация синтаксиса передачи DICOM в C# .NET | Aspose.Medical
weight: 16000
description: Преобразование DICOM‑файлов между синтаксисами передачи в C# .NET. Поддержка JPEG, JPEG 2000, JPEG‑LS, RLE и несжатых форматов с помощью API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Конвертация синтаксиса передачи DICOM в .NET C#" h2="Преобразование DICOM‑файлов между несжатыми, JPEG, JPEG 2000, JPEG‑LS и RLE синтаксисами передачи. Чистая .NET‑библиотека без нативных зависимостей." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Что такое синтаксис передачи?">}}

<p><strong>Transfer Syntax</strong> определяет, как данные DICOM кодируются для хранения и передачи. Он задает три ключевых аспекта: порядок байтов (endianness), явное или неявное представление Value Representations, а также алгоритм сжатия, применяемый к пиксельным данным. Каждый DICOM‑файл указывает свой синтаксис передачи в заголовке File Meta Information.</p>

<p>Разные медицинские устройства, PACS‑серверы и приложения для просмотра поддерживают различные наборы синтаксисов передачи. <strong>Aspose.Medical for .NET</strong> предоставляет метод <code>Transcode</code> для преобразования между синтаксисами передачи, обеспечивая интероперабельность, оптимизацию хранения и совместимость с инструментами обработки &mdash; всё в чистой .NET‑библиотеке без нативных зависимостей.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Преобразовать DICOM‑файл в C#">}}

<p>Метод <code>DicomFile.Transcode</code> преобразует DICOM‑файл из текущего синтаксиса передачи в любой поддерживаемый целевой синтаксис. Метод возвращает новый экземпляр <code>DicomFile</code> &mdash; оригинал остаётся неизменным:</p>

<div class="codeblock" id="code">
 <h3>Базовое транскодирование DICOM - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Также можно выполнять транскодирование непосредственно на уровне <code>Dataset</code>:</p>

<div class="codeblock" id="code">
 <h3>Транскодировать Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Поддерживаемые синтаксисы передачи">}}

<p>В следующей таблице перечислены все стандартные синтаксисы передачи изображений DICOM и их текущий статус поддержки в Aspose.Medical for .NET. Все поддерживаемые кодеки реализованы на чистом C# и полностью независимы от платформы.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Синтаксис передачи</th>
<th>UID</th>
<th>Тип</th>
<th>Статус</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Несжатый</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Несжатый</td><td>Поддерживается</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Несжатый</td><td>Поддерживается</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Несжатый</td><td>Поддерживается</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Поддерживается</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Сжатый, 8‑бит</td><td>Поддерживается</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Сжатый, 12‑бит</td><td>Не поддерживается</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Без сжатия</td><td>Поддерживается (только 8‑бит)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Без сжатия</td><td>Поддерживается (только 8‑бит)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Без сжатия</td><td>Поддерживается</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Почти без потерь</td><td>Поддерживается</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Без сжатия</td><td>Поддерживается (чтение 8/16‑бит, запись 8‑бит)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Сжатый или без сжатия</td><td>Поддерживается (чтение 8/16‑бит, запись 8‑бит)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Без сжатия</td><td>Поддерживается (чтение 8/16‑бит, запись 8‑бит)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Сжатый или без сжатия</td><td>Поддерживается (чтение 8/16‑бит, запись 8‑бит)</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Без сжатия</td><td>Поддерживается</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Без сжатия</td><td>Скоро будет</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Без сжатия</td><td>Скоро будет</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Сжатый или без сжатия</td><td>Скоро будет</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Без сжатия</td><td>Скоро будет</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Без сжатия</td><td>Скоро будет</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Сжатый или без сжатия</td><td>Скоро будет</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Типовые сценарии транскодирования">}}

<p>Различные рабочие процессы требуют разных стратегий транскодирования. Ниже представлены самые типичные сценарии:</p>

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
 <h3>Сжатие для сетевой передачи - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Просмотр свойств синтаксиса передачи">}}

<p>Класс <code>TransferSyntax</code> предоставляет свойства, описывающие характеристики кодирования. Используйте их для проверки текущего синтаксиса передачи файла или выбора подходящего целевого синтаксиса:</p>

<div class="codeblock" id="code">
 <h3>Чтение свойств синтаксиса передачи - C#</h3>
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
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>Уникальный идентификатор синтаксиса передачи</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Явно ли кодируются Value Representations</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Является ли порядок байтов little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Являются ли пиксельные данные инкапсулированными (сжатыми)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Является ли метод сжатия с потерями</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Использует ли синтаксис сжатие deflate</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Устарел ли синтаксис передачи согласно стандарту DICOM</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>Идентификатор метода сжатия с потерями по ISO</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Сжатие с потерями vs без потерь">}}

<p>Понимание различий между сжатием с потерями и без потерь имеет решающее значение при транскодировании DICOM‑файлов:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Аспект</th>
<th>Без потерь</th>
<th>С потерями</th>
</tr>
</thead>
<tbody>
<tr><td>Качество изображения</td><td>Пиксельное соответствие &mdash; оригинальные данные полностью сохранены</td><td>Некоторые данные безвозвратно теряются для уменьшения размера</td></tr>
<tr><td>Коэффициент сжатия</td><td>Обычно 2:1–3:1</td><td>Обычно 10:1–30:1 или выше</td></tr>
<tr><td>Безопасно при обратном преобразовании</td><td>Да &mdash; декомпрессировать и получить идентичные пиксели</td><td>Нет &mdash; каждое повторное сжатие с потерями ухудшает качество</td></tr>
<tr><td>Сценарии использования</td><td>Архивирование, диагностика, юридические записи</td><td>Предварительный просмотр, телемедицина, сетевые передачи</td></tr>
<tr><td>Поддерживаемые кодеки</td><td>JPEG Lossless, JPEG‑LS, JPEG 2000 Lossless, RLE</td><td>JPEG Baseline, JPEG‑LS Near-Lossless, JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>Важно:</strong> Транскодирование из файла со сжатием с потерями в синтаксис без потерь не восстанавливает потерянные данные. Деградация качества, возникшая при исходном сжатии с потерями, является постоянной.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ресурсы для обучения" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Исходный код" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Ссылки API" href="https://reference.aspose.com/medical/net/" >}}
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
