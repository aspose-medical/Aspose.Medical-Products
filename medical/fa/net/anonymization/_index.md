---
title: مجهول‌سازی فایل‌های DICOM در C# .NET | Aspose.Medical
weight: 1000
description: فایل‌های DICOM را در C# .NET با استفاده از پروفایل‌های محرمانگی قابل تنظیم، مجهول‌سازی کنید. اطلاعات شناسایی شخص (PII) بیمار را حذف کنید و با استفاده از API Aspose.Medical، سازگاری با HIPAA و GDPR را تضمین کنید.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="مجهول‌سازی فایل‌های DICOM در .NET C#" h2="اطلاعات شناسایی بیمار را از فایل‌های DICOM با استفاده از پروفایل‌های محرمانگی DICOM PS 3.15 حذف کنید. با یک کتابخانه خالص .NET، سازگاری با HIPAA و GDPR را تضمین کنید." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="مجهول‌سازی DICOM مبتنی بر استانداردها">}}

<p><strong>Aspose.Medical for .NET</strong> یک API جامع برای مجهول‌سازی DICOM بر پایه <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Confidentiality Profiles</a> ارائه می‌دهد. این API به توسعه‌دهندگان حوزه بهداشت و درمان امکان حذف یا تغییر اطلاعات شناسایی بیمار (PII) از فایل‌های DICOM را می‌دهد، در حالی که ارزش بالینی داده‌های تصویربرداری حفظ می‌شود. متفاوت از روش‌های ساده حذف تگ‌ها، Aspose.Medical از استاندارد رسمی DICOM پیروی می‌کند تا اطمینان از حذف صحیح هویت حاصل شود.</p>

<p>کلاس <code>Anonymizer</code> با اشیاء <code>ConfidentialityProfile</code> کار می‌کند که دقیقاً نحوه‌ی پردازش هر ویژگی DICOM را تعریف می‌کنند — چه باید حذف شود، با مقدار ساختگی جایگزین شود، پاکسازی شود یا بدون تغییر نگه داشته شود. این رویکرد، مجهول‌سازی سازگار و قابل حسابرسی را که با الزامات قانونی مطابقت دارد، تضمین می‌کند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="مجهول‌سازی یک فایل DICOM در C#">}}

<p>ساده‌ترین روش برای مجهول‌سازی یک فایل DICOM استفاده از پروفایل محرمانگی پیش‌فرض است. این پروفایل، پروفایل پایه DICOM PS 3.15 را اعمال می‌کند که تمام ویژگی‌های استاندارد شناسایی بیمار را مدیریت می‌کند:</p>

<div class="codeblock" id="code">
 <h3>مجهول‌سازی پایه DICOM - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>روش <code>Anonymize</code> <strong>غیر مخرب</strong> است — مجموعه داده اصلی را کپی می‌کند و یک نسخه جدید مجهول‌سازی شده بازمی‌گرداند. فایل DICOM اصلی بدون تغییر می‌ماند که برای مسیرهای حسابرسی و جریان‌های کاری یکپارچگی داده‌ها حیاتی است.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="جایگزینی سفارشی هویت بیمار">}}

<p>در بسیاری از جریان‌های کاری، لازم است اطلاعات بیمار را با مقادیر مستعار خاصی جایگزین کنید به جای حذف ساده آن. کلاس <code>ConfidentialityProfile</code> امکان تعیین مقادیر جایگزین برای نام بیمار و شناسه بیمار (Patient ID) را فراهم می‌کند:</p>

<div class="codeblock" id="code">
 <h3>مجهول‌سازی با هویت سفارشی بیمار - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="مجهول‌سازی در محل">}}

<p>برای سناریوهایی که کارایی حافظه مهم است یا نیازی به حفظ داده‌های اصلی ندارید، API امکان مجهول‌سازی در محل را فراهم می‌کند که مستقیماً مجموعه داده را تغییر می‌دهد:</p>

<div class="codeblock" id="code">
 <h3>مجهول‌سازی DICOM در محل - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>هر دو روش <code>Anonymize</code> و <code>AnonymizeInPlace</code> همچنین می‌توانند یک شیء <code>Dataset</code> را به‌صورت مستقیم دریافت کنند و کنترل کامل بر مجموعه داده پردازش‌شده را به شما می‌دهند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="گزینه‌های قابل تنظیم پروفایل محرمانگی">}}

<p>استاندارد DICOM PS 3.15 یک <strong>پروفایل پایه</strong> به‌همراه چندین پروفایل اختیاری تعریف می‌کند که نحوه‌ی پردازش دسته‌های خاصی از داده‌ها را در هنگام مجهول‌سازی کنترل می‌نمایند. می‌توانید این گزینه‌ها را با استفاده از enum <code>ConfidentialityProfileOptions</code> ترکیب کنید:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>گزینه</th>
<th>توضیح</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>پروفایل پیش‌فرض DICOM PS 3.15 سطح کاربردی پایه برای محرمانگی</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>نگهداری ویژگی‌های خصوصی ایمن در طول مجهول‌سازی</td></tr>
<tr><td><code>RetainUIDs</code></td><td>حفظ UIDهای اصلی DICOM (Study، Series، Instance) بدون تغییر</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>نگهداری ویژگی‌های شناسایی دستگاه (سازنده، مدل، نام ایستگاه)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>نگهداری ویژگی‌های شناسایی موسسه (نام، آدرس، بخش)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>نگهداری ویژگی‌های بیمار (سن، جنس، اندازه، وزن) برای مقاصد پژوهشی</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>نگهداری مقادیر کامل تاریخ و زمان برای مطالعات طولی</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>نگهداری تاریخ‌های اصلاح‌شده برای مطالعات طولی</td></tr>
<tr><td><code>CleanDesc</code></td><td>پاک‌سازی توصیفات متنی که ممکن است حاوی اطلاعات شناسایی باشد</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>پاک‌سازی محتوای ساختاری که ممکن است حاوی اطلاعات شناسایی باشد</td></tr>
<tr><td><code>CleanGraph</code></td><td>پاک‌سازی داده‌های گرافیکی که ممکن است شامل اطلاعات بیمار به‌صورت سوزانده‌شده باشد</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>مجهول‌سازی با گزینه‌های پروفایل خاص - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="کدهای عملی برای پردازش ویژگی‌ها">}}

<p>به هر ویژگی در یک پروفایل محرمانگی یک کد عمل اختصاص داده می‌شود که تعیین می‌کند در طول مجهول‌سازی چگونه پردازش شود. این کدها مطابق با استاندارد DICOM PS 3.15 جدول E.1-1a هستند:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>کد</th>
<th>عمل</th>
<th>توضیح</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>جایگزینی با مقدار ساختگی</td><td>جایگزینی با مقدار غیر صفر طول که با VR سازگار باشد</td></tr>
<tr><td><strong>Z</strong></td><td>صفر-طول یا ساختگی</td><td>جایگزینی با مقدار صفر طول یا مقدار ساختگی که با VR سازگار باشد</td></tr>
<tr><td><strong>X</strong></td><td>حذف</td><td>ویژگی را به‌طور کامل از مجموعه داده حذف می‌کند</td></tr>
<tr><td><strong>K</strong></td><td>نگه‌داشتن</td><td>ویژگی را بدون تغییر نگه می‌دارد (برای غیر‑دنباله‌ها) یا پاک می‌سازد (برای دنباله‌ها)</td></tr>
<tr><td><strong>C</strong></td><td>پاک‌سازی</td><td>جایگزینی با مقدار تمیز شده با معنی مشابه که با VR سازگار باشد</td></tr>
<tr><td><strong>U</strong></td><td>جایگزینی UID</td><td>جایگزینی با UID جدیدی که به‌صورت داخلی در مجموعه نمونه‌ها سازگار باشد</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="پروفایل‌های سفارشی از CSV، JSON و XML">}}

<p>برای موارد پیشرفته، می‌توانید پروفایل‌های سفارشی مجهول‌سازی را با بارگذاری قواعد از فایل‌های CSV، JSON یا XML خارجی تعریف کنید. این امکان را می‌دهد که رفتار مجهول‌سازی را مطابق سیاست‌های سازمانی یا الزامات قانونی خاص خود تنظیم کنید:</p>

<div class="codeblock" id="code">
 <h3>بارگذاری پروفایل مجهول‌سازی از CSV - C#</h3>
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
 <h3>بارگذاری پروفایل مجهول‌سازی از JSON - C#</h3>
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

<p>پروفایل‌های سفارشی همچنین می‌توانند از فایل‌های XML (<code>LoadFromXmlFile</code>)، جریان‌ها (<code>LoadFromJsonStream</code>، <code>LoadFromXmlStream</code>)، یا به‌صورت مستقیم از رشته‌ها (<code>LoadFromCsvString</code>، <code>LoadFromJsonString</code>، <code>LoadFromXmlString</code>) بارگذاری شوند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="سازگاری با HIPAA و GDPR">}}

<p>فایل‌های DICOM به‌طور مداوم شامل اطلاعات محافظت‌شده سلامت (PHI) بر اساس تعریف HIPAA و داده‌های شخصی بر اساس تعریف GDPR هستند. Aspose.Medical برای .NET به شما کمک می‌کند تا با ارائه موارد زیر، الزامات قانونی را برآورده کنید:</p>

<ul>
<li><strong>پایبندی به DICOM PS 3.15</strong>: موتور مجهول‌سازی مطابق با استاندارد رسمی DICOM برای حذف هویت عمل می‌کند و اطمینان می‌دهد که بهترین روش‌های شناخته‌شده بر تمام ویژگی‌های مربوطه اعمال می‌شود.</li>
<li><strong>حذف جامع PII</strong>: نام بیمار، شناسه، تاریخ تولد، آدرس و سایر شناسه‌ها بر اساس پروفایل محرمانگی پیکربندی‌شده پردازش می‌شوند.</li>
<li><strong>جایگزینی UID</strong>: UIDهای Study، Series و SOP Instance می‌توانند با UIDهای جدیدی که به‌صورت داخلی سازگار باشند، جایگزین شوند تا از شناسایی مجدد از طریق ارجاع متقابل جلوگیری شود.</li>
<li><strong>نگه‌داری قابل تنظیم</strong>: هنگامی که نیازهای پژوهشی یا بالینی این را طلب می‌کند، دسته‌های خاصی از داده‌ها (ویژگی‌های بیمار، تاریخ‌ها، اطلاعات دستگاه) می‌توانند به‌صورت انتخابی با استفاده از پروفایل‌های گزینه استاندارد نگه داشته شوند.</li>
<li><strong>فرآیند قابل حسابرسی</strong>: رویکرد مبتنی بر استانداردها با کدهای عملی تعریف‌شده، فرآیند واضح و مستند قابل مجهول‌سازی را برای ممیزی‌های قانونی فراهم می‌کند.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="منابع آموزشی" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="مستندات" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="کد منبع" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="مرجع‌های API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="پشتیبانی محصول" tabId="support" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی رایگان" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی پرداختی" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="وبلاگ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="چرا Aspose.Medical برای .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="فهرست مشتریان" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="داستان‌های موفقیت" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
