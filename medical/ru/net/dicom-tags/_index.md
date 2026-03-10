---
title: Чтение и изменение тегов DICOM в C# .NET | Aspose.Medical
weight: 15000
description: Чтение, добавление, обновление и удаление тегов DICOM в C# .NET. Доступ к метаданным тегов, работа с частными тегами, перечисление по группам и управление последовательностями с использованием Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Чтение и изменение тегов DICOM в .NET C#" h2="Доступ, добавление, обновление и удаление элементов данных DICOM с типобезопасным API. Работа со стандартными тегами, частными тегами, последовательностями и полным словарём тегов DICOM." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Типобезопасный доступ к тегам DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> предоставляет всесторонний типобезопасный API для работы с тегами DICOM через класс <code>Dataset</code>. Каждый элемент данных DICOM представлен объектом <code>Tag</code> &mdash; парой номеров группы и элемента, однозначно идентифицирующей атрибут. API предлагает строго типизированный доступ с обобщёнными методами, безопасные схемы получения данных, избегающие исключений, и поддержку одиночных значений, массивов и индекcного доступа.</p>

<p>Общие теги DICOM доступны как статические поля класса <code>Tag</code> (например, <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), что избавляет от необходимости запоминать числовые коды тегов. Каждый тег содержит <code>TagMetadata</code> с ключевым словом, описанием, представлением значения (Value Representation, VR), кратностью значений и статусом устаревания.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Чтение тегов DICOM в C#">}}

<p>Класс <code>Dataset</code> предоставляет несколько методов для получения значений тегов в зависимости от ваших потребностей &mdash; одиночное значение, все значения, индекcный доступ или безопасное получение с значениями по умолчанию:</p>

<div class="codeblock" id="code">
 <h3>Чтение значений тегов DICOM — C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Get a single value (throws if tag is missing or multi-valued)
string patientName = dataset.GetSingleValue&lt;string&gt;(Tag.PatientName);

// Safe retrieval with a default value
string institution = dataset.GetSingleValueOrDefault&lt;string&gt;(
    Tag.InstitutionName, "Unknown");

// Get all values from a multi-valued tag
double[] pixelSpacing = dataset.GetValues&lt;double&gt;(Tag.PixelSpacing);

// Get a value by index (supports negative indexing with ^)
double firstPosition = dataset.GetValue&lt;double&gt;(Tag.ImagePositionPatient, 0);

// Check if a tag exists before accessing it
if (dataset.Contains(Tag.PatientBirthDate))
{
    var birthDate = dataset.GetSingleValue&lt;DateOnly&gt;(Tag.PatientBirthDate);
}

// TryGet pattern for safe access without exceptions
if (dataset.TryGetSingleValue&lt;string&gt;(Tag.PatientID, out var patientId))
{
    // Use patientId
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Добавление и обновление тегов DICOM">}}

<p>Используйте <code>AddOrUpdate</code> для установки значений тегов. Метод создаёт элемент, если тег отсутствует, или заменяет его значение, если он уже существует. Вы можете задавать одиночные значения, массивы или добавлять полностью типизированные элементы:</p>

<div class="codeblock" id="code">
 <h3>Добавление и обновление тегов DICOM — C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Set a single value
dataset.AddOrUpdate(Tag.PatientName, "John Doe");
dataset.AddOrUpdate(Tag.PatientAge, new Age { Number = 42, Units = Age.Unit.Years });

// Set multiple values on a multi-valued tag
dataset.AddOrUpdate&lt;double&gt;(Tag.PixelSpacing, [0.5, 0.5]);

// Add multiple elements at once using typed element classes
dataset.AddOrUpdateRange(new IElement[]
{
    new PersonName(Tag.PatientName, ["John Doe"]),
    new LongString(Tag.PatientID, ["64AD6A3F"]),
    new Date(Tag.PatientBirthDate, [new DateOnly(2002, 10, 10)])
});

// Save the modified file
dicomFile.Save("updated.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Удаление тегов DICOM">}}

<p>Метод <code>Remove</code> поддерживает удаление одного тега, нескольких тегов одновременно или тегов, соответствующих предикату &mdash; что удобно для удаления целых групп элементов:</p>

<div class="codeblock" id="code">
 <h3>Удаление тегов DICOM — C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Remove a single tag
dataset.Remove(Tag.PatientAddress);

// Remove multiple tags at once
dataset.Remove(Tag.PatientName, Tag.PatientID, Tag.PatientBirthDate);

// Remove all tags in a specific group (e.g., group 0x0002 - File Meta Information)
dataset.Remove(element => element.Tag.Group == 0x0002);

dicomFile.Save("cleaned.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Перечисление тегов по группам">}}

<p>Теги DICOM организованы в группы &mdash; например, группа <code>0x0010</code> содержит информацию о пациенте, группа <code>0x0008</code> содержит идентификацию исследования, а группа <code>0x0028</code> содержит атрибуты пикселей изображения. Используйте <code>EnumerateGroup</code> для перебора всех элементов в конкретной группе:</p>

<div class="codeblock" id="code">
 <h3>Перебор элементов по группе — C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Enumerate all patient-related tags (group 0x0010)
foreach (IElement element in dataset.EnumerateGroup(0x0010))
{
    Tag tag = element.Tag;
    string keyword = tag.Metadata.Keyword;
    string vr = element.ValueRepresentation.ToString();

    Console.WriteLine($"({tag.Group:X4},{tag.Element:X4}) {keyword} [{vr}]");
}

// Iterate over all elements in the dataset
foreach (IElement element in dataset)
{
    // Process each element
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Манипуляция данными на уровне элементов">}}

<p>Каждый элемент DICOM предоставляет методы для чтения, добавления, вставки, удаления и замены отдельных значений внутри элемента. Это полезно для многозначных тегов, где требуется детальный контроль над списком значений:</p>

<div class="codeblock" id="code">
 <h3>Манипуляция значениями элементов — C#</h3>
 <pre><code class="cs">// Create an element with multiple values
var element = new FloatingPointDouble(
    Tag.TableOfYBreakPoints, [50.1, 100.2, 150.3]);

// Access values by index (supports ^ for indexing from end)
double first = element.Get&lt;double&gt;(0);
double last = element.Get&lt;double&gt;(^1);

// Safe access with default
double safe = element.GetOrDefault&lt;double&gt;(10); // returns 0.0 if out of range

// Get all values or a range
double[] all = element.GetValues&lt;double&gt;();
double[] subset = element.GetValues&lt;double&gt;(1..3);

// Modify element values
element.Add(200.4);
element.AddRange([300.5, 400.6]);
element.Insert(2, 75.0);
element.RemoveAt(0);
element.Replace([1.0, 2.0, 3.0]);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Словарь тегов и метаданные">}}

<p>Класс <code>TagDictionary</code> предоставляет реестр известных тегов DICOM вместе с их метаданными &mdash; ключевое слово, описание, представление значения (Value Representation), кратность значений и статус устаревания. Вы можете искать теги по ключевому слову или программно просматривать их метаданные:</p>

<div class="codeblock" id="code">
 <h3>Запрос к словарю тегов — C#</h3>
 <pre><code class="cs">// Look up a tag by its keyword
Tag? tag = TagDictionary.Default.FindByKeyword("PatientName");

// Access tag metadata from the dictionary
TagMetadata meta = TagDictionary.Default[Tag.PatientName];
Console.WriteLine($"Keyword: {meta.Keyword}");
Console.WriteLine($"Name: {meta.Name}");
Console.WriteLine($"VR: {string.Join("/", meta.ValueRepresentations)}");
Console.WriteLine($"VM: {meta.Multiplicity}");
Console.WriteLine($"Retired: {meta.Retired}");

// Access metadata directly from a tag instance
TagMetadata info = Tag.Rows.Metadata;

// Parse a tag from its string representation
Tag parsed = Tag.Parse("0010,0010");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Поддержка частных тегов">}}

<p>DICOM позволяет поставщикам и организациям определять частные теги для проприетарных данных. Aspose.Medical for .NET полностью поддерживает чтение, запись и регистрацию частных тегов. Вы можете загружать пользовательские словари тегов из XML‑файлов, чтобы обеспечить корректную обработку частных элементов.</p>

<div class="codeblock" id="code">
 <h3>Работа с частными тегами — C#</h3>
 <pre><code class="cs">// Load a custom private tag dictionary from XML
// XML format:
// &lt;dictionaries&gt;
//   &lt;dictionary creator="MY ORGANIZATION"&gt;
//     &lt;tag group="3011" element="xx00" vr="SL" vm="1"&gt;Custom Value&lt;/tag&gt;
//   &lt;/dictionary&gt;
// &lt;/dictionaries&gt;
TagDictionary.Default.Load("custom-tag-dictionary.xml");

// Check if a tag is private
Tag tag = Tag.GetOrCreate(0x3011, 0x0010);
bool isPrivate = tag.IsPrivate;

// Get a valid private tag (creates Private Creator element if needed)
Dataset dataset = dicomFile.Dataset;
Tag privateTag = dataset.GetPrivateTag(tag);

// Register a private creator programmatically
PrivateCreator creator = new("MY ORGANIZATION");
TagDictionary privateDictionary =
    TagDictionary.Default.GetPrivateDictionary(creator);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Работа с последовательностями DICOM">}}

<p>Последовательности DICOM (тип VR SQ) содержат вложенные наборы данных, образуя древовидную структуру внутри файла DICOM. Класс <code>Sequence</code> предоставляет типобезопасный доступ к элементам последовательности.</p>

<div class="codeblock" id="code">
 <h3>Чтение и создание последовательностей DICOM — C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Read a sequence element
IElement seqElement = dataset.GetOrDefault(Tag.ReferencedStudySequence);
if (seqElement is Sequence sequence)
{
    // Iterate over sequence items (each item is a Dataset)
    foreach (Dataset item in sequence.Items)
    {
        string uid = item.GetSingleValueOrDefault&lt;string&gt;(
            Tag.ReferencedSOPInstanceUID, "");
    }

    // Access an item by index
    Dataset firstItem = sequence.Get(0);
}

// Create a new sequence
var items = new List&lt;Dataset&gt;
{
    new(new IElement[]
    {
        new UniqueIdentifier(Tag.ReferencedSOPClassUID, ["1.2.840.10008.5.1.4.1.1.2"]),
        new UniqueIdentifier(Tag.ReferencedSOPInstanceUID, ["1.2.3.4.5.6.7.8.9"])
    })
};
dataset.Add(new Sequence(Tag.ReferencedStudySequence, items));</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Распространённые группы тегов DICOM">}}

<p>В следующей таблице перечислены часто используемые группы тегов DICOM и примеры тегов, доступных через класс <code>Tag</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Группа</th>
<th>Категория</th>
<th>Пример тегов</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Метаданные файла</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Идентификация исследования</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Информация о пациенте</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Параметры получения</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Позиционирование изображения</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Атрибуты пикселей изображения</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Пиксельные данные</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Обучающие ресурсы" tabId="resources" >}}
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
