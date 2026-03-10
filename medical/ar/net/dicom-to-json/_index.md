---
title: تحويل DICOM إلى JSON في C# .NET | Aspose.Medical
weight: 4000
description: تسلسل ملفات DICOM إلى تنسيق DICOM JSON القياسي في C# .NET. قم بتكوين معالجة الأعداد، عناوين URI للبيانات الضخمة، إخراج الكلمات المفتاحية، ومعالجة التدفق غير المتزامن باستخدام واجهة برمجة التطبيقات Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تحويل DICOM إلى JSON في .NET C#" h2="تسلسل مجموعات بيانات DICOM والملفات إلى نموذج DICOM JSON القياسي (PS3.18). قم بتكوين معالجة الأعداد، مراجع البيانات الضخمة، إخراج الكلمات المفتاحية، والمعالجة غير المتزامنة المعتمدة على التدفق." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="نموذج DICOM JSON القياسي">}}

<p><strong>Aspose.Medical for .NET</strong> يسلسل بيانات DICOM إلى JSON وفقًا لـ <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">نموذج DICOM PS3.18 JSON</a>. هذا هو المعيار الرسمي لتمثيل مجموعات بيانات DICOM في JSON، ويُستخدم من قبل خدمات DICOMweb، موارد FHIR ImagingStudy، وواجهات برمجة التطبيقات الحديثة في الرعاية الصحية.</p>

<p>في نموذج DICOM JSON، يتم تمثيل كل خاصية بواسطة العلامة الخاصة بها كمفتاح JSON (مثال: <code>\"00100010\"</code> للاسم المريض)، مع تمثيل القيمة (Value Representation) ومصفوفة القيم كخصائص متداخلة:</p>

<div class="codeblock" id="code">
 <h3>تنسيق نموذج DICOM JSON</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تحويل DICOM إلى JSON في C#">}}

<p>استخدم الفئة <code>DicomJsonSerializer</code> لتحويل ملف DICOM أو مجموعة بيانات إلى JSON. ينتج النهج الأبسط إخراجًا مضغوطًا ومتوافقًا مع المعايير:</p>

<div class="codeblock" id="code">
 <h3>تحويل DICOM إلى JSON - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p>يمكنك تسلسل <code>Dataset</code>، أو <code>DicomFile</code> كامل (بما في ذلك معلومات التعريف File Meta Information)، أو مصفوفة من مجموعات البيانات:</p>

<div class="codeblock" id="code">
 <h3>تسلسل DicomFile ومصفوفات مجموعة البيانات - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="التسلسل المعتمد على التدفق وغير المتزامن">}}

<p>بالنسبة لملفات DICOM الكبيرة أو السيناريوهات عالية الإنتاجية، يتم التسلسل مباشرة إلى تدفق UTF-8 لتجنب تخصيص سلاسل نصية كبيرة في الذاكرة. تتوفر كل من الطرق المتزامنة وغير المتزامنة:</p>

<div class="codeblock" id="code">
 <h3>التسلسل عبر التدفق وغير المتزامن - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="خيارات التسلسل">}}

<p>تتحكم الفئة <code>DicomJsonSerializerOptions</code> في طريقة تمثيل بيانات DICOM في JSON:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>الخاصية</th>
<th>النوع</th>
<th>الوصف</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>يتحكم في ما إذا كانت تمثيلات القيمة الرقمية (VR) (IS, DS, SV, UV) تُكتب كأعداد JSON أو كسلاسل نصية</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>استخدام كلمات DICOM المفتاحية (مثال: <code>\"PatientName\"</code>) بدلاً من العلامات (<code>\"00100010\"</code>) كمفاتيح JSON. ملاحظة: غير معيارية</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>تضمين كلمة DICOM المفتاحية كخاصية JSON إضافية</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>تضمين اسم علامة DICOM كخاصية JSON إضافية</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>محول مخصص لكتابة البيانات الكبيرة (مثل بيانات البكسل) كمرجع URI</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>محمل مخصص لحل عناوين URI للبيانات الضخمة أثناء فك التسلسل</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>تكوين خيارات التسلسل - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="معالجة الأعداد">}}

<p>يمكن تسلسل تمثيلات القيمة الرقمية في DICOM (IS, DS, SV, UV) كأعدد JSON أو كسلاسل JSON. هذا مهم للتوافقية، لأن بعض تطبيقات DICOM تخزن الأعداد مع مسافات بادئة أو تنسيق غير قياسي:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>الوضع</th>
<th>إخراج JSON</th>
<th>حالة الاستخدام</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>\"00180086\":{\"vr\":\"IS\",\"Value\":[42]}</code></td><td>متوافق مع المعيار، مثالي للعمليات الحسابية. يرمي استثناءً إذا تعذر تحليل القيمة.</td></tr>
<tr><td><code>AsString</code></td><td><code>\"00180086\":{\"vr\":\"IS\",\"Value\":[\"42\"]}</code></td><td>يحافظ على تمثيل السلسلة الأصلي. أكثر أمانًا لإعادة التحويل للبيانات غير القياسية.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="معالجة البيانات الضخمة">}}

<p>يمكن التعامل مع القيم الثنائية الكبيرة (بيانات البكسل، الموجات، المستندات المغلفة) كمرجع للبيانات الضخمة بدلاً من تضمينها في إخراج JSON. يتبع هذا مواصفة <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>.</p>

<p>نفّذ <code>IBulkDataConverter</code> لإخراج البيانات الكبيرة خارجيًا أثناء التسلسل، و<code>IBulkDataLoader</code> لحل عناوين URI أثناء فك التسلسل:</p>

<div class="codeblock" id="code">
 <h3>معالجة مخصصة للبيانات الضخمة - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="فك تسلسل JSON إلى DICOM">}}

<p>تحليل DICOM JSON مرة أخرى إلى كائنات Dataset أو DicomFile. يدعم إدخال السلسلة النصية، إدخال التدفق، وعمليات التدفق غير المتزامنة:</p>

<div class="codeblock" id="code">
 <h3>فك تسلسل JSON إلى DICOM - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تكامل System.Text.Json">}}

<p>توفر Aspose.Medical <code>DatasetJsonConverter</code> و <code>DicomFileJsonConverter</code> للتكامل مع خط أنابيب التسلسل القياسي <code>System.Text.Json</code>. يتيح ذلك تسلسل كائنات DICOM جنبًا إلى جنب مع الأنواع الأخرى في .NET ضمن شفرة معالجة JSON الحالية لديك:</p>

<div class="codeblock" id="code">
 <h3>تكامل System.Text.Json - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="التوثيق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="الكود المصدري" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
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
