---
title: Работа с большими файлами DICOM на C# .NET | Aspose.Medical
weight: 11500

description: Открывайте многокадровые исследования и изображения цельного среза на C# без загрузки их в память. Читайте метаданные без пиксельных данных, откладывайте чтение больших элементов и перемещайте файлы через потоки и каналы.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Большие файлы DICOM в .NET C#" h2="Читайте метаданные многокадрового исследования без пикселей, откладывайте чтение больших элементов до момента, когда они потребуются, и перемещайте целые файлы через потоки и каналы." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Файл большой, а вопрос обычно небольшой">}}

<p>Изображение цельного среза, длинный ряд КТ или том OCT занимают сотни мегабайт, и большая часть их – пиксельные данные. Операции, которые реально выполняет приложение, часто гораздо меньше: перечисление содержимого папки, проверка идентификатора пациента, подсчёт кадров, определение места хранения исследования. Загрузка каждого байта для выполнения этих задач превращает простую задачу в проблему памяти.</p>

<p><strong>Aspose.Medical for .NET</strong> позволяет вызывающему решить, сколько файла читать. Выбор задаётся одним аргументом метода <code>DicomFile.Open</code> и применяется как к файлам, так и к потокам и каналам.</p>

<p>Измерено на исследовании размером 14 МБ с 128 кадрами из нашего тестового набора, на той же машине и том же файле:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Стратегия чтения</th>
<th>Время открытия</th>
<th>Выделенная память</th>
</tr>
</thead>
<tbody>
<tr><td>Все, по умолчанию</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Большие элементы пропущены</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Большие элементы отложены</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>Разрыв растёт с размером файла. Папка из 10 000 исследований – случай, когда это перестаёт быть микрооптимизацией.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Читать метаданные, оставляя пиксели нетронутыми">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> исключает из чтения каждый элемент, превышающий пороговый размер. Возвращаемый набор данных содержит только те теги, которые нужны индексу или маршрутизатору.</p>

<div class="codeblock" id="code">
 <h3>Чтение исследования без его пиксельных данных - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>Порог по умолчанию — 64 КБ и задаётся в килобайтах, поэтому процесс, считающий 8 КБ большими, может указать это значение.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Отложить вместо пропуска">}}

<p>Когда пиксели могут понадобиться, но, вероятно, позже и не все из них, <code>ReadLargeOnDemand</code> — вторая часть этой пары. Открытие файла стоит столько же, сколько пропуск, а большой элемент читается в момент, когда код к нему обращается.</p>

<div class="codeblock" id="code">
 <h3>Загружать кадр только при его использовании - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>Отложенное чтение — функция, доступная только по лицензии; остальные стратегии работают в оценочном режиме.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Индексировать папку, не затрагивая пиксели">}}

<p>Та же стратегия применяется к потоку, который выглядит для кода как сканирование архива или облачное хранилище объектов.</p>

<div class="codeblock" id="code">
 <h3>Сканировать архив - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Потоки и каналы, вход и выход">}}

<p>Чтение и запись принимают потоки, а асинхронные точки входа также поддерживают типы <code>System.IO.Pipelines</code>. Исследование может перемещаться от сетевого ответа к хранилищу без того, чтобы процесс держал весь файл в виде единственного массива.</p>

<div class="codeblock" id="code">
 <h3>Чтение и запись через потоки - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>Та же идея применяется к текстовым представлениям: документ с множеством наборов данных читается по одному набору за раз на страницах <a href="/medical/net/json-to-dicom/">JSON в DICOM</a> и <a href="/medical/net/xml-to-dicom/">XML в DICOM</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Кадр за кадром">}}

<p>Многокадровые данные адресуются по отдельным кадрам, поэтому серия из 500 кадров обрабатывается по одному кадру за раз, а не целым элементом пиксельных данных.</p>

<div class="codeblock" id="code">
 <h3>Перебирайте кадры - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Где это определяет дизайн">}}

<ul>
<li>Индексация и миграция архивов: миллионы файлов, и важен только заголовок, пока что‑то не будет перемещено.</li>
<li>Маршрутизаторы и узлы хранилища: принимают исследование, читают необходимое для маршрутизации, передают байты дальше.</li>
<li>AI‑конвейеры: формируют манифест из метаданных, затем извлекают кадры для подмножества, которое действительно используется для обучения.</li>
<li>Контейнеры с ограничением памяти: рабочий набор следует стратегии, а не размеру файла.</li>
<li>Данные цельных срезов и OCT: файлы, в которых чтение всего содержимого невозможно.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">Руководство по управлению памятью</a> подробно объясняет стратегии, а <a href="/medical/net/dicom-networking/">DICOM‑сетевое взаимодействие</a> показывает те же данные, поступающие по протоколу DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Обучающие материалы" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Руководство разработчика" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
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
