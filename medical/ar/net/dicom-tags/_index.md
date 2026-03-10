---
title: قراءة وتعديل وسوم DICOM في C# .NET | Aspose.Medical
weight: 15000
description: قراءة وإضافة وتحديث وإزالة وسوم DICOM في C# .NET. الوصول إلى بيانات تعريف الوسم، التعامل مع الوسوم الخاصة، التعداد حسب المجموعة، ومعالجة السلاسل باستخدام Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="قراءة وتعديل وسوم DICOM في .NET C#" h2="الوصول وإضافة وتحديث وإزالة عناصر بيانات DICOM باستخدام API آمنة من حيث النوع. التعامل مع الوسوم القياسية، الوسوم الخاصة، السلاسل، والقاموس الكامل لوسوم DICOM." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="الوصول الآمن للنوع إلى وسوم DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> يوفر API شاملة وآمنة من حيث النوع للعمل مع وسوم DICOM عبر الفئة <code>Dataset</code>. كل عنصر بيانات DICOM يُمثَل بـ <code>Tag</code> &mdash; زوج من أرقام المجموعة والعنصر يحدد السمة بشكل فريد. تقدم API وصولًا قويًا للنوع باستخدام طرق عامة، نمط استرجاع آمن يتجنب الاستثناءات، ودعم القيم الفردية، المصفوفات، والوصول المفهرس.</p>

<p>تتوفر وسوم DICOM الشائعة كحقول ثابتة في الفئة <code>Tag</code> (مثل <code>Tag.PatientName</code>، <code>Tag.StudyInstanceUID</code>)، مما يلغي الحاجة لحفظ رموز الوسوم الرقمية. كل وسم يحمل <code>TagMetadata</code> يحتوي على الكلمة المفتاحية، الوصف، تمثيل القيمة (VR)، تكرار القيمة، وحالة الإلغاء.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="قراءة وسوم DICOM في C#">}}

<p>الفئة <code>Dataset</code> توفر عدة طرق لاسترجاع قيم الوسوم حسب احتياجاتك &mdash; قيمة واحدة، جميع القيم، الوصول المفهرس، أو استرجاع آمن مع القيم الافتراضية:</p>

<div class="codeblock" id="code">
 <h3>قراءة قيم وسوم DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="إضافة وتحديث وسوم DICOM">}}

<p>استخدم <code>AddOrUpdate</code> لتعيين قيم الوسوم. تنشئ الطريقة العنصر إذا كان الوسم مفقودًا، أو تستبدل قيمته إذا كان موجودًا بالفعل. يمكنك تعيين قيم فردية، مصفوفات، أو إضافة عناصر ذات نوع كامل:</p>

<div class="codeblock" id="code">
 <h3>إضافة وتحديث وسوم DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="إزالة وسوم DICOM">}}

<p>الطريقة <code>Remove</code> تدعم إزالة وسم واحد، أو عدة وسوم مرة واحدة، أو وسوم توافق شرطًا محددًا — مفيدة لإزالة مجموعات كاملة من العناصر:</p>

<div class="codeblock" id="code">
 <h3>إزالة وسوم DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="عد وسوم DICOM حسب المجموعة">}}

<p>يتم تنظيم وسوم DICOM في مجموعات — على سبيل المثال، المجموعة <code>0x0010</code> تحتوي على معلومات المريض، المجموعة <code>0x0008</code> تحتوي على تعريف الدراسة، والمجموعة <code>0x0028</code> تحتوي على خصائص بيكسل الصورة. استخدم <code>EnumerateGroup</code> للتكرار على جميع العناصر في مجموعة محددة:</p>

<div class="codeblock" id="code">
 <h3>عد العناصر حسب المجموعة - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="معالجة البيانات على مستوى العنصر">}}

<p>كل عنصر DICOM يوفر طرقًا للقراءة، الإضافة، الإدخال، الإزالة، واستبدال القيم الفردية داخل العنصر. هذا مفيد للوسوم ذات القيم المتعددة حيث تحتاج إلى تحكم دقيق في قائمة القيم:</p>

<div class="codeblock" id="code">
 <h3>معالجة قيم العنصر - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="قاموس الوسوم والبيانات الوصفية">}}

<p>الفئة <code>TagDictionary</code> توفر سجلًا للوسوم DICOM المعروفة مع بياناتها الوصفية — الكلمة المفتاحية، الوصف، تمثيل القيمة، تكرار القيمة، وحالة الإلغاء. يمكنك البحث عن الوسوم بالكلمة المفتاحية أو فحص بياناتها الوصفية برمجيًا:</p>

<div class="codeblock" id="code">
 <h3>استعلام عن قاموس الوسوم - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="دعم الوسوم الخاصة">}}

<p>يتيح DICOM للبائعين والمؤسسات تعريف وسوم خاصة للبيانات المملوكة. Aspose.Medical for .NET يدعم بالكامل قراءة وكتابة وتسجيل الوسوم الخاصة. يمكنك تحميل قواميس وسوم مخصصة من ملفات XML لتمكين المعالجة السليمة للعناصر الخاصة:</p>

<div class="codeblock" id="code">
 <h3>العمل مع الوسوم الخاصة - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="العمل مع سلاسل DICOM">}}

<p>سلاسل DICOM (نوع VR هو SQ) تحتوي على مجموعات بيانات متداخلة، مكوّنة بنية شجرية داخل ملف DICOM. الفئة <code>Sequence</code> توفر وصولًا محددًا إلى عناصر السلسلة:</p>

<div class="codeblock" id="code">
 <h3>قراءة وإنشاء سلاسل DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="مجموعات وسوم DICOM الشائعة">}}

<p>الجدول التالي يُظهر مجموعات وسوم DICOM المستخدمة بشكل شائع وبعض الأمثلة للوسوم المتاحة عبر الفئة <code>Tag</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>المجموعة</th>
<th>الفئة</th>
<th>وسوم مثال</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>معلومات ملف التعريف</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>تعريف الدراسة</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>معلومات المريض</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>معاملات الاكتساب</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>تموضع الصورة</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>خصائص بيكسل الصورة</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>بيانات البكسل</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="مصادر التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="الوثائق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="الكود المصدري" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="مراجع API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="دعم المنتج" tabId="support" >}}
{{< blocks/products/pf/slr-element name="دعم مجاني" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="دعم مدفوع" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="المدونة" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="لماذا Aspose.Medical for .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="قائمة العملاء" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="قصص نجاح" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
