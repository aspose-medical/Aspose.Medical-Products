---
title: خواندن و اصلاح برچسب‌های DICOM در C# .NET | Aspose.Medical
weight: 15000
description: خواندن، افزودن، به‌روزرسانی و حذف برچسب‌های DICOM در C# .NET. دسترسی به متادیتای برچسب، کار با برچسب‌های خصوصی، پیمایش بر اساس گروه و دستکاری توالی‌ها با استفاده از Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="خواندن و اصلاح برچسب‌های DICOM در .NET C#" h2="دسترسی، افزودن، به‌روزرسانی و حذف عناصر داده DICOM با یک API ایمن از نوع. کار با برچسب‌های استاندارد، برچسب‌های خصوصی، توالی‌ها و فرهنگ‌نامه کامل برچسب‌های DICOM." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="دسترسی ایمن از نوع به برچسب‌های DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> یک API جامع و ایمن از نوع برای کار با برچسب‌های DICOM از طریق کلاس <code>Dataset</code> ارائه می‌دهد. هر عنصر داده DICOM توسط یک <code>Tag</code> &mdash; جفتی از شماره‌های گروه و عنصر که ویژگی را به‌صورت یکتا شناسایی می‌کند - نمایش داده می‌شود. این API دسترسی با نوع قوی را با متدهای عمومی، الگوهای بازیابی ایمن که استثناها را جلوگیری می‌کند و پشتیبانی از مقادیر منفرد، آرایه‌ها و دسترسی ایندکسی فراهم می‌کند.</p>

<p>برچسب‌های متداول DICOM به‌صورت فیلدهای ایستا در کلاس <code>Tag</code> در دسترس هستند (مثلاً <code>Tag.PatientName</code>، <code>Tag.StudyInstanceUID</code>) که نیاز به حفظ کدهای عددی برچسب را از بین می‌برد. هر برچسب حاوی <code>TagMetadata</code> شامل کلیدواژه، توضیح، نمایان‌سازی مقدار (VR)، چندگانگی مقدار و وضعیت منقضی شدن است.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="خواندن برچسب‌های DICOM در C#">}}

<p>کلاس <code>Dataset</code> روش‌های متعددی برای بازیابی مقادیر برچسب بر اساس نیازهای شما ارائه می‌دهد &mdash; مقدار منفرد، تمام مقادیر، دسترسی ایندکسی یا بازیابی ایمن با پیش‌فرض‌ها:</p>

<div class="codeblock" id="code">
 <h3>خواندن مقادیر برچسب DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="افزودن و به‌روزرسانی برچسب‌های DICOM">}}

<p>از <code>AddOrUpdate</code> برای تنظیم مقادیر برچسب استفاده کنید. این متد در صورتی که برچسب موجود نباشد، عنصر را ایجاد می‌کند و در صورتی که از پیش وجود داشته باشد، مقدار آن را جایگزین می‌نماید. می‌توانید مقادیر منفرد، آرایه‌ها یا عناصر کاملاً تایپ‌شده را تنظیم کنید:</p>

<div class="codeblock" id="code">
 <h3>افزودن و به‌روزرسانی برچسب‌های DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="حذف برچسب‌های DICOM">}}

<p>متد <code>Remove</code> امکان حذف یک برچسب، چندین برچسب به‌صورت همزمان، یا برچسب‌های منطبق با یک پیش‌شرط را فراهم می‌کند &mdash; که برای حذف کل گروه‌های عناصر مفید است:</p>

<div class="codeblock" id="code">
 <h3>حذف برچسب‌های DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="پیمایش برچسب‌ها بر حسب گروه">}}

<p>برچسب‌های DICOM به‌صورت گروه‌ها سازماندهی شده‌اند — به عنوان مثال، گروه <code>0x0010</code> شامل اطلاعات بیمار، گروه <code>0x0008</code> شامل شناسایی مطالعه، و گروه <code>0x0028</code> شامل ویژگی‌های پیکسل تصویر است. از <code>EnumerateGroup</code> برای مرور تمام عناصر در یک گروه خاص استفاده کنید:</p>

<div class="codeblock" id="code">
 <h3>پیمایش عناصر بر حسب گروه - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="دستکاری داده‌ها در سطح عنصر">}}

<p>هر عنصر DICOM متدهایی برای خواندن، افزودن، درج، حذف و جایگزینی مقادیر تک‌تکی داخل عنصر فراهم می‌کند. این برای برچسب‌های چند‌مقداری که نیاز به کنترل دقیق بر لیست مقادیر دارند، مفید است:</p>

<div class="codeblock" id="code">
 <h3>دستکاری مقادیر عنصر - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="فرهنگ‌نامه برچسب‌ها و متادیتا">}}

<p>کلاس <code>TagDictionary</code> یک رجیستری از برچسب‌های شناخته‌شده DICOM به همراه متادیتای آن‌ها — کلیدواژه، توضیح، نمایان‌سازی مقدار، چندگانگی مقدار و وضعیت منقضی شدن — فراهم می‌کند. می‌توانید برچسب‌ها را بر اساس کلیدواژه جستجو کنید یا متادیتای آن‌ها را به‌صورت برنامه‌نویسی بررسی نمایید:</p>

<div class="codeblock" id="code">
 <h3>استعلام از فرهنگ‌نامه برچسب‌ها - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="پشتیبانی از برچسب‌های خصوصی">}}

<p>DICOM به فروشندگان و سازمان‌ها اجازه می‌دهد تا برچسب‌های خصوصی برای داده‌های انحصاری تعریف کنند. Aspose.Medical for .NET به‌صورت کامل از خواندن، نوشتن و رجیستر کردن برچسب‌های خصوصی پشتیبانی می‌کند. می‌توانید فرهنگ‌نامه‌های برچسب سفارشی را از فایل‌های XML بارگذاری کنید تا به‌درستی عناصر خصوصی را مدیریت نمایید:</p>

<div class="codeblock" id="code">
 <h3>کار با برچسب‌های خصوصی - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="کار با توالی‌های DICOM">}}

<p>توابع DICOM (نوع VR: SQ) شامل دیتاست‌های تو در تو هستند که ساختار درختی داخل فایل DICOM را تشکیل می‌دهند. کلاس <code>Sequence</code> دسترسی با نوع به آیتم‌های توالی را فراهم می‌کند:</p>

<div class="codeblock" id="code">
 <h3>خواندن و ایجاد توالی‌های DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="گروه‌های متداول برچسب‌های DICOM">}}

<p>جدول زیر گروه‌های پرکاربرد برچسب‌های DICOM و نمونه برچسب‌های دسترسی‌پذیر از طریق کلاس <code>Tag</code> را فهرست می‌کند:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>گروه</th>
<th>دسته‌بندی</th>
<th>نمونه برچسب‌ها</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>اطلاعات متاداده فایل</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>شناسایی مطالعه</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>اطلاعات بیمار</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>پارامترهای اکتساب</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>موقعیت‌گیری تصویر</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>ویژگی‌های پیکسل تصویر</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>داده‌های پیکسل</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="منابع آموزشی" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="مستندات" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="کد منبع" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="مرجع API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="پشتیبانی محصول" tabId="support" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی رایگان" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی پرداختی" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="وبلاگ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="چرا Aspose.Medical برای .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="فهرست مشتریان" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="موفقیت‌ها" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
