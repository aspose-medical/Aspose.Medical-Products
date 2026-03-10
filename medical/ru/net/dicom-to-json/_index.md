---
title: Преобразование DICOM в JSON на C# .NET | Aspose.Medical
weight: 4000
description: Сериализуйте файлы DICOM в стандартный формат DICOM JSON на C# .NET. Настройте обработку чисел, URI bulk‑данных, вывод ключевых слов и асинхронную потоковую обработку с помощью API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Преобразование DICOM в JSON в .NET C#" h2="Сериализуйте наборы данных и файлы DICOM в стандартную модель DICOM JSON (PS3.18). Настройте обработку чисел, ссылки на bulk‑данные, вывод ключевых слов и асинхронную обработку на основе потоков." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Стандартная модель DICOM JSON">}}

<p><strong>Aspose.Medical for .NET</strong> сериализует данные DICOM в JSON в соответствии с <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">моделью DICOM PS3.18 JSON</a>. Это официальный стандарт представления наборов данных DICOM в JSON, используемый сервисами DICOMweb, ресурсами FHIR ImagingStudy и современными API здравоохранения.</p>

<p>В модели DICOM JSON каждый атрибут представляется своим тегом в качестве ключа JSON (например, <code>"00100010"</code> для Patient Name), а его Value Representation и массив значений – вложенными свойствами:</p>

<div class="codeblock" id="code">
 <h3>Формат модели DICOM JSON</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Сериализация DICOM в JSON на C#">}}

<p>Используйте класс <code>DicomJsonSerializer</code> для преобразования файла DICOM или набора данных в JSON. Самый простой способ генерирует компактный вывод, соответствующий стандарту:</p>

<div class="codeblock" id="code">
 <h3>Преобразование DICOM в JSON - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p>Вы можете сериализовать <code>Dataset</code>, полностью <code>DicomFile</code> (включая File Meta Information) или массив наборов данных:</p>

<div class="codeblock" id="code">
 <h3>Сериализация DicomFile и массивов наборов данных - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Сериализация на основе потоков и асинхронная">}}

<p>Для больших файлов DICOM или сценариев с высоким пропускным способностям сериализуйте напрямую в поток UTF-8, чтобы избежать выделения больших строк в памяти. Доступны как синхронные, так и асинхронные методы:</p>

<div class="codeblock" id="code">
 <h3>Потоковая и асинхронная сериализация - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Параметры сериализации">}}

<p>Класс <code>DicomJsonSerializerOptions</code> определяет, как данные DICOM представляются в JSON:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Свойство</th>
<th>Тип</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Определяет, записываются ли числовые VR (IS, DS, SV, UV) как числа JSON или как строки</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Использовать ключевые слова DICOM (например, <code>"PatientName"</code>) вместо тегов (<code>"00100010"</code>) в качестве ключей JSON. Примечание: нестандартно</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Включать ключевое слово DICOM как дополнительный атрибут JSON</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Включать имя тега DICOM как дополнительный атрибут JSON</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Пользовательский конвертер для записи больших данных (например, пиксельных данных) в виде URI‑ссылок</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Пользовательский загрузчик для разрешения URI bulk‑данных при десериализации</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Настройка параметров сериализации - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Обработка чисел">}}

<p>Числовые Value Representations DICOM (IS, DS, SV, UV) могут сериализоваться как числа JSON или как строки JSON. Это важно для совместимости, поскольку некоторые реализации DICOM хранят числа с ведущими пробелами или в нестандартном формате:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Режим</th>
<th>Вывод JSON</th>
<th>Сценарий использования</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>Соответствует стандарту, идеально для вычислений. Вызывает исключение, если значение нельзя разобрать.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>Сохраняет оригинальное строковое представление. Надёжнее для обратного преобразования нестандартных данных.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Обработка bulk‑данных">}}

<p>Большие бинарные значения (пиксельные данные, волноформы, инкапсулированные документы) могут обрабатываться как ссылки на bulk‑данные, а не внедряться в вывод JSON. Это соответствует спецификации <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>.</p>

<p>Реализуйте <code>IBulkDataConverter</code> для внешней записи больших данных при сериализации и <code>IBulkDataLoader</code> для разрешения URI при десериализации:</p>

<div class="codeblock" id="code">
 <h3>Пользовательская обработка bulk‑данных - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Десериализация JSON в DICOM">}}

<p>Разберите JSON DICOM обратно в объекты Dataset или DicomFile. Поддерживаются ввод в виде строки, потока и асинхронные операции с потоками:</p>

<div class="codeblock" id="code">
 <h3>Десериализация JSON в DICOM - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Интеграция с System.Text.Json">}}

<p>Aspose.Medical предоставляет <code>DatasetJsonConverter</code> и <code>DicomFileJsonConverter</code> для интеграции со стандартным конвейером сериализации <code>System.Text.Json</code>. Это позволяет сериализовать объекты DICOM вместе с другими типами .NET в вашем существующем коде обработки JSON:</p>

<div class="codeblock" id="code">
 <h3>Интеграция System.Text.Json - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Учебные ресурсы" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Исходный код" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Справочники API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Поддержка продукта" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Бесплатная поддержка" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Платная поддержка" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Блог" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Почему выбирают Aspose.Medical для .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Список клиентов" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Истории успеха" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
