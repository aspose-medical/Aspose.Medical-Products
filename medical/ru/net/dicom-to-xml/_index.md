---
title: Конвертировать DICOM в XML на C# .NET | Aspose.Medical
weight: 3000
description: Сериализовать наборы данных DICOM в стандартный формат DICOM XML на C# .NET. Настройте обработку bulk‑data, потоковую обработку и async операции с помощью API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Конвертировать DICOM в XML на .NET C#" h2="Сериализовать наборы данных DICOM в стандартное представление DICOM XML (PS3.19). Настройте ссылки на bulk‑data, вывод в потоковом режиме и async обработку с помощью чистой библиотеки .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Стандартизованная сериализация DICOM XML">}}

<p><strong>Aspose.Medical for .NET</strong> сериализует данные DICOM в XML в соответствии с <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a>. Это официальный стандарт представления наборов данных DICOM в XML, используемый сервисами DICOMweb, платформами интеграции и системами, которым требуется человекочитаемое представление метаданных медицинских изображений, проверяемое схемой.</p>

<p>Класс <code>DicomXmlSerializer</code> предоставляет статические методы как для сериализации, так и для десериализации. В отличие от простых подходов «dump‑tag», вывод соответствует схеме DICOM XML, где каждый элемент представлен со своим тегом, VR и правильно отформатированными значениями &mdash; обеспечивая без потерь обратимое преобразование между двоичным DICOM и XML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Сериализовать DICOM в XML на C#">}}

<p>Используйте класс <code>DicomXmlSerializer</code> для преобразования набора данных DICOM в строку XML. Самый простой подход генерирует XML‑документ, соответствующий стандарту:</p>

<div class="codeblock" id="code">
 <h3>Конвертировать DICOM в XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Потоковая и асинхронная сериализация">}}

<p>Для больших файлов DICOM или сценариев с высокой пропускной способностью сериализуйте непосредственно в поток, чтобы избежать выделения больших строк в памяти. Доступны как синхронные, так и асинхронные методы:</p>

<div class="codeblock" id="code">
 <h3>Синхронная сериализация в поток - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Асинхронная сериализация в поток - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Конвейерная передача потоков для больших исследований">}}

<p>Полные исследования не обязаны помещаться в памяти. <code>DicomXmlSerializer</code> пишет в <code>PipeWriter</code> и читает из <code>PipeReader</code>, поэтому XML может генерироваться и потребляться по мере передачи, а последовательность наборов данных может читаться один за другим через <code>DeserializeAsyncEnumerable</code>. Каждый метод принимает <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Сериализация и десериализация через pipe - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Чтение последовательности наборов данных по одному - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Параметры сериализации">}}

<p>Класс <code>DicomXmlSerializerOptions</code> управляет тем, как данные DICOM представляются в XML. Основная настройка касается обработки bulk‑data для больших двоичных значений:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Свойство</th>
<th>Тип</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Пользовательский конвертер для записи больших данных (например, pixel data) в виде BulkData URI‑ссылок вместо их встраивания</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Пользовательский загрузчик для разрешения BulkData URI во время десериализации</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Экземпляр параметров по умолчанию, используемый когда пользовательские параметры не предоставлены</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Сериализация с пользовательскими параметрами - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Обработка Bulk Data">}}

<p>Большие двоичные значения (pixel data, waveforms, инкапсулированные документы) могут быть вынесены во внешние ссылки BulkData URI вместо встраивания в вывод XML. Это соответствует спецификации <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">элемента BulkData DICOM PS3.19</a>.</p>

<p>Реализуйте <code>IBulkDataConverter</code> для внешней записи больших данных при сериализации и <code>IBulkDataLoader</code> для разрешения URI при десериализации. Для типовых случаев нет необходимости писать собственный загрузчик: <code>DefaultBulkDataLoader.Instance</code> разрешает URI <code>file</code>, <code>http</code> и <code>https</code>, а также реализует <code>IAsyncBulkDataLoader</code>, поэтому bulk‑data загружается асинхронно в потоковых сценариях.</p>

<div class="codeblock" id="code">
 <h3>Пользовательская обработка bulk data - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
public class FileBulkDataConverter : IBulkDataConverter
{
    public string? GetBulkDataUri(IElement element)
    {
        // Externalize pixel data to a separate file
        if (element.Tag == Tag.PixelData)
            return "file:///bulk/pixeldata.raw";
        return null; // inline all other elements
    }
}

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Десериализовать XML в DICOM">}}

<p>Разберите DICOM XML обратно в объекты Dataset. Поддерживается ввод в виде строки, потока и async операции:</p>

<div class="codeblock" id="code">
 <h3>Десериализовать XML в DICOM - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Сериализация XML vs JSON">}}

<p>Aspose.Medical поддерживает сериализацию как DICOM XML (PS3.19), так и DICOM JSON (PS3.18). Оба формата обеспечивают без потерь обратимое преобразование, но подходят для разных сценариев интеграции:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Возможность</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Стандарт</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>Проверка схемы</td><td>XML Schema (XSD) доступна</td><td>Нет формальной схемы</td></tr>
<tr><td>Наиболее подходит для</td><td>Enterprise integration, HL7 CDA, audit logs, XDS registries</td><td>DICOMweb, REST APIs, FHIR ImagingStudy</td></tr>
<tr><td>Человеческая читаемость</td><td>Verbose but self-describing</td><td>Compact and widely supported</td></tr>
<tr><td>Bulk data</td><td>BulkData element with URI</td><td>BulkDataURI property</td></tr>
<tr><td>Класс сериализатора</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ресурсы для обучения" tabId="resources" >}}
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
