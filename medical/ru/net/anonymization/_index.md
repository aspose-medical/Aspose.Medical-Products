---
title: Анонимизация DICOM файлов в C# .NET | Aspose.Medical
weight: 1000
description: Анонимизируйте DICOM файлы в C# .NET с использованием настраиваемых профилей конфиденциальности. Удаляйте персональные данные пациента (PII), обеспечивая соответствие требованиям HIPAA и GDPR с помощью API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Анонимизация DICOM файлов в .NET C#" h2="Удалите идентифицирующую информацию пациента из DICOM файлов, используя профили конфиденциальности DICOM PS 3.15. Обеспечьте соответствие HIPAA и GDPR с помощью чистой .NET библиотеки." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Анонимизация DICOM на основе стандартов">}}

<p><strong>Aspose.Medical for .NET</strong> предоставляет комплексный API для анонимизации DICOM, основанный на <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">профилях конфиденциальности DICOM PS 3.15</a>. API позволяет разработчикам медицинского ПО удалять или изменять идентифицирующую информацию пациента (PII) в DICOM файлах, сохраняя клиническую ценность данных изображений. В отличие от простых подходов удаления тегов, Aspose.Medical следует официальному стандарту DICOM, обеспечивая корректную деперсонализацию.</p>

<p>Класс <code>Anonymizer</code> работает с объектами <code>ConfidentialityProfile</code>, которые точно определяют, как следует обрабатывать каждый атрибут DICOM — удалять, заменять фиктивным значением, очищать или оставлять без изменений. Этот подход обеспечивает последовательную, поддающуюся аудиту анонимизацию, соответствующую нормативным требованиям.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Анонимизировать DICOM файл в C#">}}

<p>Самый простой способ анонимизировать DICOM файл — использовать профиль конфиденциальности по умолчанию. Он применяет базовый профиль DICOM PS 3.15, который обрабатывает все стандартные атрибуты, идентифицирующие пациента:</p>

<div class="codeblock" id="code">
 <h3>Базовая анонимизация DICOM - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>Метод <code>Anonymize</code> является <strong>незаписывающим</strong> &mdash; он клонирует исходный набор данных и возвращает новую анонимизированную копию. Исходный DICOM файл остаётся неизменным, что важно для аудиторских журналов и процессов обеспечения целостности данных.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Замена идентичности пациента на пользовательскую">}}

<p>Во многих рабочих процессах требуется заменить информацию о пациенте конкретными псевдонимами, а не просто удалить её. Класс <code>ConfidentialityProfile</code> позволяет задать значения замены для имени пациента и его идентификатора:</p>

<div class="codeblock" id="code">
 <h3>Анонимизация с пользовательской идентичностью пациента - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create a confidentiality profile with custom replacement values
ConfidentialityProfile profile = new()
{
    PatientName = "ANONYMOUS PATIENT",
    PatientId = "00000000"
};

// Create an anonymizer with this profile
Anonymizer anonymizer = new(profile);

// Anonymize the file
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("custom_anonymized.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Анонимизация «на месте»">}}

<p>Для сценариев, где важна эффективность использования памяти или нет необходимости сохранять оригинальные данные, API предоставляет анонимизацию «на месте», непосредственно модифицирующую набор данных:</p>

<div class="codeblock" id="code">
 <h3>Анонимизация DICOM «на месте» - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Оба метода <code>Anonymize</code> и <code>AnonymizeInPlace</code> также принимают объект <code>Dataset</code> напрямую, предоставляя полный контроль над тем, какой набор данных будет обработан.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Настраиваемые параметры профиля конфиденциальности">}}

<p>Стандарт DICOM PS 3.15 определяет <strong>Базовый профиль</strong> и несколько дополнительных профилей, контролирующих обработку определённых категорий данных при анонимизации. Вы можете комбинировать эти параметры с помощью перечисления <code>ConfidentialityProfileOptions</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Опция</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Стандартный базовый профиль конфиденциальности уровня приложения DICOM PS 3.15</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Сохранять безопасные приватные атрибуты при анонимизации</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Сохранять оригинальные DICOM UID (Study, Series, Instance) без изменений</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Сохранять атрибуты идентификации устройства (производитель, модель, название станции)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Сохранять атрибуты идентификации учреждения (название, адрес, отделение)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Сохранять характеристики пациента (возраст, пол, рост, вес) для исследовательских целей</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Сохранять полные даты и время для лонгитюдных исследований</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Сохранять изменённые даты для лонгитюдных исследований</td></tr>
<tr><td><code>CleanDesc</code></td><td>Очищать текстовые описания, которые могут содержать идентифицирующую информацию</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Очищать структурированное содержание, которое может содержать идентифицирующую информацию</td></tr>
<tr><td><code>CleanGraph</code></td><td>Очищать графические данные, которые могут содержать вшитую информацию о пациенте</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Анонимизация с конкретными параметрами профиля - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Коды действий для обработки атрибутов">}}

<p>Каждому атрибуту в профиле конфиденциальности присваивается код действия, определяющий, как его обрабатывать при анонимизации. Эти коды соответствуют стандарту DICOM PS 3.15 Table E.1-1a:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Код</th>
<th>Действие</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Заменить фиктивным</td><td>Заменить значением ненулевой длины, соответствующим VR</td></tr>
<tr><td><strong>Z</strong></td><td>Нулевая длина или фиктивное</td><td>Заменить нулевой длиной или фиктивным значением, соответствующим VR</td></tr>
<tr><td><strong>X</strong></td><td>Удалить</td><td>Полностью удалить атрибут из набора данных</td></tr>
<tr><td><strong>K</strong></td><td>Сохранить</td><td>Оставить атрибут без изменений (для не‑последовательностей) или очистить (для последовательностей)</td></tr>
<tr><td><strong>C</strong></td><td>Очистить</td><td>Заменить очищенным значением аналогичного смысла, соответствующим VR</td></tr>
<tr><td><strong>U</strong></td><td>Заменить UID</td><td>Заменить новым UID, который внутренне согласован в наборе экземпляров</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Пользовательские профили из CSV, JSON и XML">}}

<p>Для сложных сценариев вы можете определить пользовательские профили анонимизации, загружая правила из внешних файлов CSV, JSON или XML. Это позволяет адаптировать поведение анонимизации к конкретным корпоративным политикам или нормативным требованиям:</p>

<div class="codeblock" id="code">
 <h3>Загружать профиль анонимизации из CSV - C#</h3>
 <pre><code class="cs">// CSV format: TagPattern;Action
// 0010,0010;Z   (Anonymize PatientName)
// 0010,0020;D   (Replace PatientID with dummy)
// 0020,000D;U   (Replace StudyInstanceUID)

var profile = ConfidentialityProfile.LoadFromCsvFile(
    "anonymization_rules.csv",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Загружать профиль анонимизации из JSON - C#</h3>
 <pre><code class="cs">// JSON format:
// [
//     { "Tag": "0010,0010", "Action": "Z" },
//     { "Tag": "0010,0020", "Action": "D" },
//     { "Tag": "0020,000D", "Action": "U" }
// ]

var profile = ConfidentialityProfile.LoadFromJsonFile(
    "anonymization_rules.json",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<p>Пользовательские профили также могут быть загружены из XML‑файлов (<code>LoadFromXmlFile</code>), потоков (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) или напрямую из строк (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Соответствие HIPAA и GDPR">}}

<p>DICOM файлы обычно содержат защищённую медицинскую информацию (PHI) в соответствии с HIPAA и персональные данные согласно GDPR. Aspose.Medical for .NET помогает соответствовать регулятивным требованиям, предоставляя:</p>

<ul>
<li><strong>Соответствие DICOM PS 3.15</strong>: движок анонимизации следует официальному стандарту DICOM для деперсонализации, гарантируя применение признанных лучших практик ко всем соответствующим атрибутам.</li>
<li><strong>Полное удаление PII</strong>: имя пациента, идентификатор, дата рождения, адрес и другие идентификаторы обрабатываются в соответствии с настроенным профилем конфиденциальности.</li>
<li><strong>Замена UID</strong>: UID исследования, серии и экземпляра SOP могут быть заменены новыми, внутренне согласованными UID, чтобы предотвратить повторную идентификацию через перекрёстные ссылки.</li>
<li><strong>Настраиваемое сохранение</strong>: когда исследовательские или клинические потребности требуют, отдельные категории данных (характеристики пациента, даты, информация об устройстве) могут быть избирательно сохранены с помощью стандартных профилей опций.</li>
<li><strong>Аудируемый процесс</strong>: подход, основанный на стандартах и определённых кодах действий, обеспечивает чёткий документируемый процесс анонимизации для регулятивных аудитов.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Учебные ресурсы" tabId="resources" >}}
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
