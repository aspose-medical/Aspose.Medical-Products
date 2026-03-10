---
title: إزالة هوية ملفات DICOM في C# .NET | Aspose.Medical
weight: 1000
description: إزالة هوية ملفات DICOM في C# .NET باستخدام ملفات تعريف السرية القابلة للتكوين. حذف معلومات التعريف الشخصية للمريض (PII)، وضمان الامتثال لـ HIPAA و GDPR باستخدام API الخاص بـ Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="إزالة هوية ملفات DICOM في .NET C#" h2="إزالة معلومات تعريف المريض من ملفات DICOM باستخدام ملفات تعريف السرية DICOM PS 3.15. ضمان الامتثال لـ HIPAA و GDPR باستخدام مكتبة .NET خالصة." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="إزالة هوية DICOM المستندة إلى المعايير">}}

<p><strong>Aspose.Medical for .NET</strong> يوفر واجهة برمجة تطبيقات شاملة لإزالة هوية DICOM تستند إلى <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">ملفات تعريف السرية DICOM PS 3.15</a>. تمكّن الواجهة مطوري الرعاية الصحية من حذف أو تعديل معلومات تعريف المريض (PII) من ملفات DICOM مع الحفاظ على القيمة السريرية لبيانات التصوير. على عكس أساليب إزالة العلامات البسيطة، يتبع Aspose.Medical المعيار الرسمي لـ DICOM لضمان إلغاء التعريف بشكل صحيح.</p>

<p>الفئة <code>Anonymizer</code> تعمل مع كائنات <code>ConfidentialityProfile</code> التي تحدد بدقة كيفية معالجة كل سمة DICOM — سواء يجب إزالتها، استبدالها بقيمة وهمية، تنظيفها، أو إبقاءها دون تغيير. يضمن هذا النهج إزالة هوية متسقة وقابلة للتدقيق تلبي المتطلبات التنظيمية.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="إزالة هوية ملف DICOM في C#">}}

<p>أسهل طريقة لإزالة هوية ملف DICOM هي استخدام ملف تعريف السرية الافتراضي. يطبق هذا ملف التعريف الأساسي DICOM PS 3.15، والذي يتعامل مع جميع السمات المعيارية التي تحدد هوية المريض:</p>

<div class="codeblock" id="code">
 <h3>إزالة هوية DICOM الأساسية - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>طريقة <code>Anonymize</code> هي <strong>غير مدمرة</strong> — تقوم بنسخ مجموعة البيانات الأصلية وتعيد نسخة جديدة مُزيل هوية. يظل ملف DICOM الأصلي دون تغيير، وهو أمر أساسي لسجلات التدقيق وتدفقات عمل سلامة البيانات.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="استبدال هوية المريض المخصص">}}

<p>في العديد من سير العمل، تحتاج إلى استبدال معلومات المريض بقيم مستعارة محددة بدلاً من حذفها فقط. تسمح لك فئة <code>ConfidentialityProfile</code> بتعيين قيم استبدال لاسم المريض ومعرف المريض:</p>

<div class="codeblock" id="code">
 <h3>إزالة هوية باستخدام هوية مريض مخصصة - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="إزالة هوية في الموقع">}}

<p>للحالات التي تكون فيها كفاءة الذاكرة مهمة أو لا تحتاج إلى الاحتفاظ بالبيانات الأصلية، توفر الواجهة إزالة هوية في الموقع تقوم بتعديل مجموعة البيانات مباشرةً:</p>

<div class="codeblock" id="code">
 <h3>إزالة هوية DICOM في الموقع - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>كلا طريقتي <code>Anonymize</code> و <code>AnonymizeInPlace</code> تقبلان أيضًا كائن <code>Dataset</code> مباشرة، مما يمنحك التحكم الكامل في مجموعة البيانات التي يتم معالجتها.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="خيارات ملف تعريف السرية القابلة للتكوين">}}

<p>يحدد معيار DICOM PS 3.15 <strong>ملف تعريف السرية الأساسي</strong> إلى جانب عدة ملفات تعريف اختيارية تتحكم في كيفية معالجة فئات معينة من البيانات أثناء إزالة الهوية. يمكنك دمج هذه الخيارات باستخدام تعداد <code>ConfidentialityProfileOptions</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>الخيار</th>
<th>الوصف</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>ملف تعريف السرية الأساسي لتطبيق DICOM PS 3.15 الافتراضي</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>الاحتفاظ بالسمات الخاصة الآمنة أثناء إزالة الهوية</td></tr>
<tr><td><code>RetainUIDs</code></td><td>الإبقاء على معرّفات DICOM الأصلية (Study, Series, Instance) دون تغيير</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>الاحتفاظ بسمات تعريف الجهاز (المصنّع، الموديل، اسم المحطة)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>الاحتفاظ بسمات تعريف المؤسسة (الاسم، العنوان، القسم)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>الاحتفاظ بخصائص المريض (العمر، الجنس، الحجم، الوزن) لأغراض البحث</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>الاحتفاظ بالقيم الكاملة للتاريخ والوقت للدراسات الطولية</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>الاحتفاظ بتواريخ التعديل للدراسات الطولية</td></tr>
<tr><td><code>CleanDesc</code></td><td>تنظيف أوصاف النص التي قد تحتوي على معلومات تعريفية</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>تنظيف المحتوى المُنظم الذي قد يحتوي على معلومات تعريفية</td></tr>
<tr><td><code>CleanGraph</code></td><td>تنظيف البيانات الرسومية التي قد تحتوي على معلومات مدمجة للمريض</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>إزالة هوية باستخدام خيارات ملف تعريف محددة - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="رموز الإجراء لمعالجة السمات">}}

<p>يُعيّن لكل سمة في ملف تعريف السرية رمز إجراء يحدد كيفية معالجتها أثناء إزالة الهوية. تتبع هذه الرموز معيار DICOM PS 3.15 الجدول E.1-1a:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>الرمز</th>
<th>الإجراء</th>
<th>الوصف</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>استبدال بقيمة وهمية</td><td>استبدال بقيمة بطول غير صفر تتوافق مع VR</td></tr>
<tr><td><strong>Z</strong></td><td>طول صفر أو وهمية</td><td>استبدال بقيمة بطول صفر أو قيمة وهمية تتوافق مع VR</td></tr>
<tr><td><strong>X</strong></td><td>إزالة</td><td>إزالة السمة تمامًا من مجموعة البيانات</td></tr>
<tr><td><strong>K</strong></td><td>الإبقاء</td><td>الإبقاء على السمة دون تعديل (لغير المتسلسلات) أو تنظيفها (للمتسلسلات)</td></tr>
<tr><td><strong>C</strong></td><td>تنظيف</td><td>استبدال بقيمة منقاة ذات معنى مشابه يتوافق مع VR</td></tr>
<tr><td><strong>U</strong></td><td>استبدال UID</td><td>استبدال بمعرف UID جديد يكون متسقًا داخليًا ضمن مجموعة الحالات</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ملفات تعريف مخصصة من CSV, JSON, و XML">}}

<p>لحالات الاستخدام المتقدمة، يمكنك تعريف ملفات تعريف مخصصة لإزالة الهوية بتحميل القواعد من ملفات CSV أو JSON أو XML الخارجية. يتيح لك ذلك تخصيص سلوك الإزالة وفقًا لسياستك التنظيمية أو المتطلبات التنظيمية المحددة.</p>

<div class="codeblock" id="code">
 <h3>تحميل ملف تعريف الإزالة من CSV - C#</h3>
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
 <h3>تحميل ملف تعريف الإزالة من JSON - C#</h3>
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

<p>يمكن أيضًا تحميل ملفات التعريف المخصصة من ملفات XML (<code>LoadFromXmlFile</code>)، أو من التدفقات (<code>LoadFromJsonStream</code>، <code>LoadFromXmlStream</code>)، أو مباشرةً من السلاسل (<code>LoadFromCsvString</code>، <code>LoadFromJsonString</code>، <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الامتثال لـ HIPAA و GDPR">}}

<p>تحتوي ملفات DICOM بشكل دوري على معلومات صحية محمية (PHI) كما تُعرّفها HIPAA وبيانات شخصية كما تُعرّفها GDPR. يساعدك Aspose.Medical for .NET على تلبية المتطلبات التنظيمية من خلال توفير:</p>

<ul>
<li><strong>الامتثال لـ DICOM PS 3.15</strong>: محرك الإزالة يتبع المعيار الرسمي لـ DICOM لإلغاء التعريف، مما يضمن تطبيق أفضل الممارسات المعترف بها على جميع السمات ذات الصلة.</li>
<li><strong>إزالة شاملة لمعلومات التعريف الشخصية (PII)</strong>: يتم معالجة اسم المريض، المعرف، تاريخ الميلاد، العنوان، وغيرها من المعرفات وفقًا لملف تعريف السرية المُكوّن.</li>
<li><strong>استبدال UID</strong>: يمكن استبدال معرّفات الدراسة، السلسلة، وحالة SOP بمعرفات UID جديدة متسقة داخليًا لمنع إعادة التعرف عبر الإشارة المتبادلة.</li>
<li><strong>الاحتفاظ القابل للتكوين</strong>: عندما تتطلب الأبحاث أو الاحتياجات السريرية ذلك، يمكن الاحتفاظ بشكل انتقائي بفئات محددة من البيانات (خصائص المريض، التواريخ، معلومات الجهاز) باستخدام ملفات تعريف الخيارات القياسية.</li>
<li><strong>عملية قابلة للتدقيق</strong>: النهج القائم على المعايير مع رموز الإجراء المحددة يوفر عملية إزالة هوية واضحة وقابلة للتوثيق لعمليات التدقيق التنظيمية.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="التوثيق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="الكود المصدر" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="مراجع API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="دعم المنتج" tabId="support" >}}
{{< blocks/products/pf/slr-element name="دعم مجاني" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="دعم مدفوع" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="المدونة" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="لماذا Aspose.Medical لـ .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="قائمة العملاء" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="قصص نجاح" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
