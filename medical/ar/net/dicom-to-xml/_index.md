---
title: تحويل DICOM إلى XML في C# .NET | Aspose.Medical
weight: 3000
description: تسلسل مجموعات بيانات DICOM إلى تنسيق XML القياسي لـ DICOM في C# .NET. إعداد معالجة البيانات الضخمة، المعالجة المستندة إلى التدفق، والعمليات غير المتزامنة باستخدام Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تحويل DICOM إلى XML في .NET C#" h2="تسلسل مجموعات بيانات DICOM إلى تمثيل XML القياسي لـ DICOM (PS3.19). إعداد مراجع البيانات الضخمة، الإخراج المستند إلى التدفق، والمعالجة غير المتزامنة باستخدام مكتبة .NET النقية." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="تسلسل DICOM XML المبني على المعايير">}}

<p><strong>Aspose.Medical for .NET</strong> يقوم بتسلسل بيانات DICOM إلى XML وفقًا <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">نموذج DICOM PS3.19 Native DICOM</a>. هذا هو المعيار الرسمي لتمثيل مجموعات بيانات DICOM في XML، يُستخدم من قبل خدمات DICOMweb، منصات التكامل، والأنظمة التي تحتاج إلى تمثيل قابل للقراءة البشرية ومُتحقق من المخطط لميتا بيانات التصوير الطبي.</p>

<p>توفر فئة <code>DicomXmlSerializer</code> طرقًا ثابتة لكل من التسلسل العكسي والتسلسل. على عكس أساليب تفريغ الوسوم البسيطة، يتطابق الناتج مع مخطط DICOM XML حيث يتم تمثيل كل عنصر بالوسم الخاص به، نوع القيمة (VR)، والقيم المُنسقة بشكل صحيح &mdash; مما يتيح تحويلًا بدون فقد بين DICOM الثنائي وXML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تسلسل DICOM إلى XML في C#">}}

<p>استخدم فئة <code>DicomXmlSerializer</code> لتحويل مجموعة بيانات DICOM إلى سلسلة XML. ينتج النهج الأبسط مستند XML متوافق مع المعايير:</p>

<div class="codeblock" id="code">
 <h3>تحويل DICOM إلى XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="التسلسل المستند إلى التدفق وغير المتزامن">}}

<p>بالنسبة لملفات DICOM الكبيرة أو سيناريوهات الإنتاجية العالية، يتم التسلسل مباشرة إلى تدفق لتجنب تخصيص سلاسل نصية ضخمة في الذاكرة. تتوفر كل من الطرق المتزامنة وغير المتزامنة:</p>

<div class="codeblock" id="code">
 <h3>تسلسل تدفق متزامن - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>تسلسل تدفق غير متزامن - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تدفق الأنابيب للدراسات الكبيرة">}}

<p>لا يلزم الاحتفاظ بالدراسات الكاملة في الذاكرة. تقوم <code>DicomXmlSerializer</code> بالكتابة إلى <code>PipeWriter</code> والقراءة من <code>PipeReader</code>، وبالتالي يمكن إنتاج XML واستهلاكه أثناء تدفقه، ويمكن قراءة سلسلة من مجموعات البيانات واحدة تلو الأخرى عبر <code>DeserializeAsyncEnumerable</code>. كل طريقة تستقبل <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>تسلسل وفك تسلسل عبر الأنبوب - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>قراءة سلسلة من مجموعات البيانات واحدة تلو الأخرى - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="خيارات التسلسل">}}

<p>تُتحكم فئة <code>DicomXmlSerializerOptions</code> في كيفية تمثيل بيانات DICOM في XML. التكوين الأساسي يتعلق بمعالجة البيانات الضخمة للقيم الثنائية الكبيرة:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>الخاصية</th>
<th>النوع</th>
<th>الوصف</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>محول مخصص لكتابة البيانات الكبيرة (مثل بيانات البكسل) كمرجع URI للـ BulkData بدلاً من تضمينها</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>محمل مخصص لحل عناوين URI للـ BulkData أثناء فك التسلسل</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>كائن الخيارات الافتراضي يستخدم عندما لا يتم توفير خيارات مخصصة</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>تسلسل باستخدام خيارات مخصصة - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="معالجة البيانات الضخمة">}}

<p>يمكن إسناد القيم الثنائية الكبيرة (بيانات البكسل، الموجات، المستندات المدمجة) كمرجع URI للـ BulkData بدلاً من تضمينها في ناتج XML. يتبع ذلك مواصفة <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">عنصر BulkData في DICOM PS3.19</a>.</p>

<p>قم بتنفيذ <code>IBulkDataConverter</code> لإسناد البيانات الكبيرة خارجياً أثناء التسلسل، و<code>IBulkDataLoader</code> لحل عناوين URI أثناء فك التسلسل. في الحالات الشائعة لا توجد حاجة لكتابة محمل على الإطلاق: <code>DefaultBulkDataLoader.Instance</code> يحل عناوين URI من النوع <code>file</code> و<code>http</code> و<code>https</code>, كما ينفذ <code>IAsyncBulkDataLoader</code>, وبالتالي تُجلب البيانات الضخمة بشكل غير متزامن في مسارات التدفق.</p>

<div class="codeblock" id="code">
 <h3>معالجة مخصصة للبيانات الضخمة - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
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

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="فك تسلسل XML إلى DICOM">}}

<p>تحليل XML لـ DICOM وإعادته إلى كائنات Dataset. يدعم الإدخال كسلسلة نصية، إدخال تدفق، والعمليات غير المتزامنة:</p>

<div class="codeblock" id="code">
 <h3>فك تسلسل XML إلى DICOM - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="التسلسل XML مقابل JSON">}}

<p>يدعم Aspose.Medical كلًا من تسلسل DICOM XML (PS3.19) و DICOM JSON (PS3.18). كلا الصيغتين توفران تحويلًا بدون فقد للذهاب والإياب، لكنهما تخدمان سيناريوهات تكامل مختلفة:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>الميزة</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>المعيار</td><td>PS3.19 (النموذج الأصلي لـ DICOM)</td><td>PS3.18 (نموذج DICOM JSON)</td></tr>
<tr><td>التحقق من المخطط</td><td>مخطط XML (XSD) متاح</td><td>لا مخطط رسمي</td></tr>
<tr><td>الأفضل لـ</td><td>تكامل المؤسسات، HL7 CDA، سجلات التدقيق، سجلات XDS</td><td>DICOMweb، REST APIs، FHIR ImagingStudy</td></tr>
<tr><td>قابلية القراءة البشرية</td><td>مفصل ولكنه يصف نفسه</td><td>مدمج وذو دعم واسع</td></tr>
<tr><td>البيانات الضخمة</td><td>عنصر BulkData مع URI</td><td>خاصية BulkDataURI</td></tr>
<tr><td>فئة المُسلسلة</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

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
