---
title: تحويل XML إلى DICOM في C# .NET | Aspose.Medical
weight: 5000

description: إنشاء ملفات DICOM من XML نموذج DICOM الأصلي لـ PS3.19 في C# .NET. قراءة XML من سلسلة، أو تدفق، أو قناة، بث المستندات المتتالية، وحل مراجع البيانات الضخمة باستخدام Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تحويل XML إلى DICOM في .NET C#" h2="قراءة XML نموذج DICOM الأصلي لـ PS3.19 وإرجاعه إلى مجموعات البيانات وملفات DICOM. العمل من سلسلة، أو تدفق أو قناة، بث المستندات المتتالية، وحل مراجع البيانات الضخمة." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="XML نموذج DICOM الأصلي القياسي">}}

<p><strong>Aspose.Medical for .NET</strong> يقرأ <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">نموذج DICOM الأصلي</a> المحدد في DICOM PS3.19. هذا هو تمثيل XML المكتوب داخل المعيار نفسه، وليس تنسيقًا اخترعه Aspose، وهو ما يجعلها مفيدة للتكامل: النظام الذي يتبادل DICOM كـ XML ينتج مستندات تقبلها هذه المكتبة.</p>

<p>جذر المستند هو <code>NativeDicomModel</code>، وكل سمة هي عنصر <code>DicomAttribute</code> يحمل العلامة، تمثيل القيمة والكلمة المفتاحية:</p>

<div class="codeblock" id="code">
 <h3>تنسيق نموذج DICOM الأصلي</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>هذه الصفحة هي الاتجاه العكسي لـ <a href="/medical/net/dicom-to-xml/">DICOM إلى XML</a>، وكلاهما يستخدمان نفس الفئة، <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="إنشاء ملف DICOM من XML في C#">}}

<p><code>Deserialize</code> يحول المستند إلى <code>Dataset</code>، ويتم كتابة مجموعة البيانات إلى القرص كملف DICOM.</p>

<div class="codeblock" id="code">
 <h3>إنشاء ملف DICOM من XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>نموذج DICOM الأصلي لا يحتوي على مجموعة معلومات ميتا للملف، لذا فإن صيغة النقل ليست جزءًا من المستند. مجموعة البيانات المغلفة في <code>DicomFile</code> تُكتب باستخدام صيغة النقل الافتراضية، Implicit VR Little Endian. لتخزين الملف بصيغة أخرى، قم بتحويله، كما تُظهر صفحة <a href="/medical/net/dicom-transfer-syntax-conversion/">تحويل صيغة النقل</a>.</p>

<p>قراءة DICOM XML هي ميزة مرخصة. بدون تطبيق ترخيص محلي، يُطلق القارئ استثناءً <code>MedicalApiException</code>، لذا قم بتطبيق الترخيص أولاً، كما يصف <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">دليل الترخيص</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="التدفقات، القنوات واللامتزامن">}}

<p>لكل نقطة دخول يوجد تحميل زائد لتدفق وتحميل زائد غير متزامن، كما أن غير المتزامنين يقبلان أيضًا <code>PipeReader</code>. يتم تحليل المستند الوارد من استجابة ويب أثناء قراءته، دون تحويله إلى سلسلة أولاً.</p>

<div class="codeblock" id="code">
 <h3>قراءة XML من تدفق - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="مستندات متتالية في تدفق واحد">}}

<p>غالبًا ما يحتوي تصدير من نظام آخر على عنصر <code>NativeDicomModel</code> بعد آخر في تدفق واحد. <code>DeserializeAsyncEnumerable</code> يُنتج مجموعة بيانات واحدة لكل عنصر، وفق ترتيب الإدخال، لذا يتم معالجة التدفق دون احتجازه في الذاكرة. تتبع العناصر بعضها البعض مباشرة: يُسمح بتصريح XML فقط في البداية تمامًا، كما هو الحال في أي إدخال XML.</p>

<div class="codeblock" id="code">
 <h3>بث مستندات متتالية - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="مراجع البيانات الضخمة">}}

<p>القيم الكبيرة مثل بيانات البكسل لا تُكتب مدمجة. تظهر كعنصر <code>BulkData</code> يحتوي على URI يشير إلى البايتات، مما يحافظ على صغر حجم المستند. لحل تلك المراجع أثناء القراءة، قدم للمعالج محمّل بيانات ضخمة. <code>DefaultBulkDataLoader</code> يجلب عناوين URI من نوع <code>file</code> و <code>http</code> و <code>https</code> دون مصادقة؛ بالنسبة لأرشيف يحتاج إلى بيانات اعتماد، قم بتنفيذ <code>IBulkDataLoader</code> أو <code>IAsyncBulkDataLoader</code> بنفسك.</p>

<div class="codeblock" id="code">
 <h3>حل البيانات الضخمة أثناء القراءة - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="رحلة عكسية مع DICOM إلى XML">}}

<p>الوجهان مُصممان للاستخدام معًا: يخرج دراسة كـ XML، تمر عبر نظام يتعامل مع XML، وتعود كملف DICOM. كل ذلك مُدار بواسطة .NET، لذا تعمل الرحلة العكسية نفسها على Windows و Linux و macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM إلى XML والعودة - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>للخيارات التي تتحكم في شكل XML، راجع صفحة <a href="/medical/net/dicom-to-xml/">DICOM إلى XML</a>. نفس الزوج موجود لـ JSON: <a href="/medical/net/dicom-to-json/">DICOM إلى JSON</a> و <a href="/medical/net/json-to-dicom/">JSON إلى DICOM</a>. يغطي <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">دليل التسلسل</a> كامل API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="الوثائق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="دليل المطور" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="مرجع API" href="https://reference.aspose.com/medical/net/" >}}
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