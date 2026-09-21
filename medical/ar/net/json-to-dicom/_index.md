---
title: تحويل JSON إلى DICOM في C# .NET | Aspose.Medical
weight: 6000

description: إنشاء ملفات DICOM من نموذج DICOM JSON القياسي (PS3.18) في C# .NET. قراءة JSON من سلسلة نصية أو تدفق أو أنبوب، بث سلسلة من مجموعات البيانات، وحل مراجع البيانات الضخمة باستخدام Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تحويل JSON إلى DICOM في .NET C#" h2="قراءة نموذج DICOM JSON القياسي (PS3.18) مرة أخرى إلى مجموعات البيانات وملفات DICOM. العمل من سلسلة نصية أو تدفق أو أنبوب، بث تسلسل من الدراسات، وحل مراجع البيانات الضخمة." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="من DICOM JSON إلى ملف DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> يقرأ <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">نموذج DICOM PS3.18 JSON</a>، التمثيل المستخدم من قبل خدمات DICOMweb والأنظمة التي تتبادل الدراسات عبر HTTP. ما يصل كـ JSON يتحول إلى <code>Dataset</code>، ويتم كتابة <code>Dataset</code> إلى القرص كملف DICOM.</p>

<p>هذه هي الاتجاه المعكوس لصفحة <a href="/medical/net/dicom-to-json/">DICOM إلى JSON</a>، وتستخدم كلاهما نفس الفئة، <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>إنشاء ملف DICOM من JSON - C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>مجموعة البيانات التي لا تحمل معلومات ملف التعريف (File Meta Information) تُكتب باستخدام صيغة النقل الافتراضية، Implicit VR Little Endian، عند تغليفها في <code>DicomFile</code>.</p>

<p>قراءة DICOM JSON هي ميزة مرخصة. بدون تطبيق ترخيص محلي، يطرح القارئ استثناءً <code>MedicalApiException</code>، لذا يجب تطبيق الترخيص أولاً، كما يوضح <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">دليل الترخيص</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الاحتفاظ بمعلومات ملف التعريف (File Meta Information)">}}

<p>تُعيد <code>Deserialize</code> مجموعة البيانات فقط. عندما يحمل مستند JSON أيضًا مجموعة معلومات ملف التعريف (File Meta Information)، على سبيل المثال لأنه تم إنشاؤه من ملف DICOM كامل، تُعيد <code>DeserializeFile</code> <code>DicomFile</code> مع تلك المجموعة محفوظة، بما في ذلك صيغة النقل التي يعلن عنها الملف.</p>

<div class="codeblock" id="code">
 <h3>قراءة ملف DICOM كامل من JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="التدفقات، الأنابيب والبرمجة غير المتزامنة">}}

<p>كل نقطة دخول لديها نسخة معالجة تدفق (stream overload) ونسخة معالجة غير متزامنة (asynchronous overload)، والإصدارات غير المتزامنة تقبل أيضًا <code>PipeReader</code>. يتم قراءة مستند يأتي من استجابة ويب أو من القرص دون تحويله إلى سلسلة نصية أولاً، وهو ما يصبح مهمًا عندما يحمل JSON بيانات بكسل.</p>

<div class="codeblock" id="code">
 <h3>قراءة JSON من تدفق - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تسلسل من مجموعات البيانات، واحدة في كل مرة">}}

<p>استعلام DICOMweb يرد بمصفوفة من مجموعات البيانات، ويمكن أن يكون هذا المستند كبيرًا. <code>DeserializeList</code> يقرأ المصفوفة بالكامل إلى الذاكرة؛ <code>DeserializeAsyncEnumerable</code> تُعيد مجموعة بيانات واحدة في كل مرة، لذلك لا يتم الاحتفاظ بالمستند بالكامل في الذاكرة.</p>

<div class="codeblock" id="code">
 <h3>بث مصفوفة من مجموعات البيانات - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="مراجع البيانات الضخمة">}}

<p>نموذج DICOM JSON لا يحمل بيانات البكسل مضمّنًا. القيم الكبيرة تُستبدل بـ <code>BulkDataURI</code> التي تشير إلى البايتات، مما يحافظ على صغر حجم مستند JSON. لحل تلك المراجع أثناء القراءة، زوّد المُسلسِل (serializer) بتحميل بيانات ضخم. <code>DefaultBulkDataLoader</code> يجلب عناوين URI من نوع <code>file</code> و <code>http</code> و <code>https</code> دون مصادقة؛ بالنسبة لأرشيف يحتاج إلى بيانات اعتماد، قم بتنفيذ <code>IBulkDataLoader</code> أو <code>IAsyncBulkDataLoader</code> بنفسك.</p>

<div class="codeblock" id="code">
 <h3>حل BulkDataURI أثناء القراءة - C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الرحلة العكسية مع DICOM إلى JSON">}}

<p>من المفترض استخدام الاتجاهين معًا: تُرسل الدراسة كـ JSON، تمر عبر خدمة ويب، وتعود كملف DICOM. لا يعتمد أي شيء في العملية على شفرة أصلية، لذا يمكن تنفيذ نفس الرحلة العكسية على Windows وLinux وmacOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM إلى JSON والعودة - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>للاطلاع على الخيارات التي تتحكم في شكل JSON، راجع صفحة <a href="/medical/net/dicom-to-json/">DICOM إلى JSON</a>. يوجد نفس الزوج للـ XML: <a href="/medical/net/dicom-to-xml/">DICOM إلى XML</a> و<a href="/medical/net/xml-to-dicom/">XML إلى DICOM</a>. يغطي <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">دليل تسلسل JSON</a> كامل الـ API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="الوثائق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="دليل المطور" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="مراجع API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="دعم المنتج" tabId="support" >}}
{{< blocks/products/pf/slr-element name="دعم مجاني" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="دعم مدفوع" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="مدونة" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="لماذا Aspose.Medical لـ .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="قائمة العملاء" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="قصص نجاح" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}