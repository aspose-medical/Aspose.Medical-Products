---
title: Конвертировать JSON в DICOM на C# .NET | Aspose.Medical
weight: 6000

description: Создавайте файлы DICOM из стандартной модели DICOM JSON (PS3.18) на C# .NET. Читайте JSON из строки, потока или канала, передавайте последовательность наборов данных и разрешайте ссылки на bulk‑данные с помощью API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Конвертация JSON в DICOM в .NET C#" h2="Чтение стандартной модели DICOM JSON (PS3.18) обратно в наборы данных и файлы DICOM. Работайте со строкой, потоком или каналом, передавайте последовательность исследований и разрешайте ссылки на bulk‑данные." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Из DICOM JSON в файл DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> читает <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">модель DICOM PS3.18 JSON</a>, представление, используемое сервисами DICOMweb и системами, обменивающимися исследованиями по HTTP. То, что приходит в виде JSON, превращается в <code>Dataset</code>, а <code>Dataset</code> записывается на диск как файл DICOM.</p>

<p>Это обратное направление страницы <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>, и оба используют один и тот же класс, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Создать файл DICOM из JSON — C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Набор данных без File Meta Information записывается с синтаксисом передачи по умолчанию, Implicit VR Little Endian, когда он обернут в <code>DicomFile</code>.</p>

<p>Чтение DICOM JSON является лицензируемой функцией. Без применения on-premise лицензии чтение бросает <code>MedicalApiException</code>, поэтому сначала примените лицензию, как описано в <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">руководстве по лицензированию</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Сохранить File Meta Information">}}

<p><code>Deserialize</code> возвращает только набор данных. Когда JSON‑документ также содержит группу File Meta Information, например потому, что он был создан из полного файла DICOM, <code>DeserializeFile</code> возвращает <code>DicomFile</code> с этой группой в неизменном виде, включая объявленный в файле синтаксис передачи.</p>

<div class="codeblock" id="code">
 <h3>Прочитать полный файл DICOM из JSON — C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Потоки, каналы и асинхронность">}}

<p>Каждая точка входа имеет перегрузку для потока и асинхронную перегрузку, причём асинхронные версии также принимают <code>PipeReader</code>. Документ, полученный из веб‑ответа или с диска, читается без предварительного преобразования в строку, что важно, когда JSON содержит пиксельные данные.</p>

<div class="codeblock" id="code">
 <h3>Считать JSON из потока — C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Последовательность наборов данных, по одному">}}

<p>Запрос DICOMweb возвращает массив наборов данных, и такой документ может быть большим. <code>DeserializeList</code> считывает весь массив в память; <code>DeserializeAsyncEnumerable</code> выдаёт один набор данных за раз, поэтому документ никогда не загружается полностью.</p>

<div class="codeblock" id="code">
 <h3>Передавать массив наборов данных — C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ссылки на bulk‑данные">}}

<p>Модель DICOM JSON не содержит пиксельные данные inline. Большие значения заменяются <code>BulkDataURI</code>, указывающим на байты, что сохраняет небольшой размер JSON‑документа. Чтобы разрешить такие ссылки при чтении, предоставьте сериализатору загрузчик bulk‑данных. <code>DefaultBulkDataLoader</code> получает URI <code>file</code>, <code>http</code> и <code>https</code> без аутентификации; для архива, требующего учётных данных, реализуйте собственные <code>IBulkDataLoader</code> или <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Разрешить BulkDataURI при чтении — C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Круговой процесс с DICOM to JSON">}}

<p>Оба направления предназначены для совместного использования: исследование экспортируется как JSON, проходит через веб‑сервис и возвращается как файл DICOM. В процессе ничего не зависит от нативного кода, поэтому один и тот же круговой процесс работает на Windows, Linux и macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM to JSON и обратно — C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Для параметров, определяющих внешний вид JSON, смотрите страницу <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>. Такая же пара существует и для XML: <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> и <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">Руководство по сериализации JSON</a> охватывает весь API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Обучающие ресурсы" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Руководство разработчика" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
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