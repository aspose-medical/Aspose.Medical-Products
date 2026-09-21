---
title: Преобразование XML в DICOM на C# .NET | Aspose.Medical
weight: 5000

description: Создавайте DICOM‑файлы из XML модели Native DICOM Model PS3.19 на C# .NET. Читайте XML из строки, потока или канала, передавайте последовательные документы и разрешайте ссылки на bulk data с помощью API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Преобразование XML в DICOM в .NET C#" h2="Чтение XML модели Native DICOM Model PS3.19 обратно в наборы данных и DICOM‑файлы. Работайте со строкой, потоком или каналом, передавайте последовательные документы и разрешайте ссылки на bulk data." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Стандартный XML модели Native DICOM Model">}}

<p><strong>Aspose.Medical for .NET</strong> читает <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a>, определённую в DICOM PS3.19. Это XML‑представление, записанное непосредственно в стандарт, а не формат, придуманный Aspose, что делает его полезным для интеграции: система, уже обменивающаяся DICOM в виде XML, генерирует документы, которые принимает эта библиотека.</p>

<p>Корневой элемент документа — <code>NativeDicomModel</code>, а каждый атрибут представлен элементом <code>DicomAttribute</code>, содержащим тег, представление значения (VR) и ключевое слово:</p>

<div class="codeblock" id="code">
 <h3>Формат Native DICOM Model</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>Эта страница реализует обратное направление к <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>, и обе используют один и тот же класс <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Создание DICOM‑файла из XML на C#">}}

<p>Метод <code>Deserialize</code> преобразует документ в <code>Dataset</code>, а набор данных записывается на диск как DICOM‑файл.</p>

<div class="codeblock" id="code">
 <h3>Создание DICOM‑файла из XML — C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>В Native DICOM Model отсутствует группа File Meta Information, поэтому синтаксис передачи не включён в документ. Набор данных, упакованный в <code>DicomFile</code>, записывается с синтаксисом передачи по умолчанию — Implicit VR Little Endian. Чтобы сохранить файл с другим синтаксисом, выполните транскодирование, как показано на странице <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a>.</p>

<p>Чтение DICOM XML является лицензируемой функцией. Без применённой локальной лицензии читатель генерирует исключение <code>MedicalApiException</code>, поэтому сначала необходимо применить лицензию, как описано в <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">руководстве по лицензированию</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Потоки, каналы и асинхронность">}}

<p>Каждая точка входа имеет перегрузку для потока и асинхронную перегрузку, причём асинхронные варианты также принимают <code>PipeReader</code>. Документ, полученный из веб‑ответа, разбирается по мере чтения, без предварительного преобразования в строку.</p>

<div class="codeblock" id="code">
 <h3>Чтение XML из потока — C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Последовательные документы в одном потоке">}}

<p>Экспорт из другой системы часто содержит один элемент <code>NativeDicomModel</code> за другим в едином потоке. <code>DeserializeAsyncEnumerable</code> выдаёт по одному набору данных для каждого элемента в порядке поступления, поэтому поток обрабатывается без удерживания в памяти. Элементы следуют друг за другом напрямую: объявление XML допускается только в самом начале, как и в любом XML‑входе.</p>

<div class="codeblock" id="code">
 <h3>Передача последовательных документов — C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ссылки на bulk data">}}

<p>Большие значения, такие как данные пикселей, не записываются непосредственно. Они представлены элементом <code>BulkData</code> с URI, указывающим на байты, что сохраняет размер документа небольшим. Для разрешения этих ссылок во время чтения предоставьте сериализатору загрузчик bulk data. <code>DefaultBulkDataLoader</code> получает данные по URI <code>file</code>, <code>http</code> и <code>https</code> без аутентификации; если для архива требуются учётные данные, реализуйте свои <code>IBulkDataLoader</code> или <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Разрешение bulk data при чтении — C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Круговой процесс с DICOM to XML">}}

<p>Эти два направления предназначены для совместного использования: исследование экспортируется как XML, проходит через систему, работающую с XML, и возвращается в виде DICOM‑файла. Всё реализовано на .NET, поэтому одинаковый круговой процесс работает в Windows, Linux и macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM to XML и обратно — C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Для параметров, определяющих вид XML, см. страницу <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>. Тот же набор доступен для JSON: <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> и <a href="/medical/net/json-to-dicom/">JSON to DICOM</a>. Полное описание API содержится в <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">руководстве по сериализации</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Учебные ресурсы" tabId="resources" >}}
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