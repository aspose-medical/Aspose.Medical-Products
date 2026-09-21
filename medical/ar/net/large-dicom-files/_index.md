---
title: العمل مع ملفات DICOM الكبيرة في C# .NET | Aspose.Medical
weight: 11500

description: افتح الدراسات متعددة الإطارات وصور الشرائح الكاملة في C# دون تحميلها إلى الذاكرة. اقرأ البيانات الوصفية بدون بيانات البكسل، وجدل العناصر الكبيرة، وانقل الملفات عبر التدفقات والأنابيب.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="ملفات DICOM الكبيرة في .NET C#" h2="اقرأ البيانات الوصفية لدراسة متعددة الإطارات دون البكسلات، وجدل العناصر الكبيرة حتى يتم طلبها، وانقل الملفات بالكامل عبر التدفقات والأنابيب." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="الملف كبير، والسؤال عادةً ما يكون صغيرًا">}}

<p>صورة شريحة كاملة، أو سلسلة CT طويلة أو حجم OCT قد يصل إلى مئات الميجابايت، ومعظمها بيانات بكسل. العمل الذي تقوم به التطبيق فعليًا يكون غالبًا أصغر بكثير: سرد ما هو موجود في مجلد، التحقق من معرف المريض، عد الإطارات، تحديد مكان إرسال الدراسة. تحميل كل بايت للإجابة على ذلك هو ما يحول مهمة بسيطة إلى مشكلة ذاكرة.</p>

<p><strong>Aspose.Medical for .NET</strong> يتيح للمتصل أن يقرر مقدار ما يُقرأ من الملف. الاختيار هو وسيط واحد في <code>DicomFile.Open</code>، وينطبق على الملفات، والتدفقات، والأنابيب على حد سواء.</p>

<p>تم القياس على دراسة بحجم 14 ميجابايت تحتوي على 128 إطارًا من مجموعة الاختبار الخاصة بنا، على نفس الجهاز ونفس الملف:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>استراتيجية القراءة</th>
<th>وقت الفتح</th>
<th>الذاكرة المخصصة</th>
</tr>
</thead>
<tbody>
<tr><td>الكل، الافتراضي</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>تخطي العناصر الكبيرة</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>تأجيل العناصر الكبيرة</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>الفجوة تزداد مع حجم الملف. مجلد يحتوي على 10,000 دراسة هو الحالة التي تتوقف فيها العملية عن كونها تحسينًا جزئيًا.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="اقرأ البيانات الوصفية، واترك البكسلات دون تغيير">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> يستبعد كل عنصر يزيد عن حد الحجم من القراءة. مجموعة البيانات التي تعود تحتوي على العلامات التي يحتاجها الفهرس أو الموجه.</p>

<div class="codeblock" id="code">
 <h3>قراءة دراسة دون بيانات البكسل - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>الحد الافتراضي هو 64 كيلوبايت ويأخذ قيمة بالكيلوبايت، لذا يمكن لمسار عمل يعتبر 8 كيلوبايت كبيرًا أن يحدد ذلك.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تأجيل بدلًا من التخطي">}}

<p>عندما قد تكون البكسلات مطلوبة، ولكن ربما لاحقًا وربما ليس كلها، فإن <code>ReadLargeOnDemand</code> هو النصف الآخر من الزوج. فتح الملف يكلف نفس تكلفة التخطي، ويتم قراءة العنصر الكبير في اللحظة التي يتفاعل معها الكود.</p>

<div class="codeblock" id="code">
 <h3>تحميل إطار فقط عند الاستخدام - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>القراءة المؤجلة هي ميزة مرخصة؛ وتعمل الاستراتيجيات الأخرى أيضًا في وضع التقييم.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="فهرسة مجلد دون التعامل مع البكسلات">}}

<p>تنطبق نفس الاستراتيجية على التدفق، وهو ما يشبه عملية فحص أرشيف أو تخزين كائن سحابي من منظور الكود.</p>

<div class="codeblock" id="code">
 <h3>فحص أرشيف - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="التدفقات والأنابيب، داخلًا وخارجًا">}}

<p>كلا من القراءة والكتابة يقبلان التدفقات، ونقاط الدخول غير المتزامنة تقبل أيضًا أنواع <code>System.IO.Pipelines</code>. يمكن للدراسة الانتقال من استجابة الشبكة إلى التخزين دون أن يحتفظ العملية بالملف كاملًا كمصفوفة واحدة.</p>

<div class="codeblock" id="code">
 <h3>القراءة والكتابة عبر التدفقات - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>تنطبق الفكرة نفسها على تمثيلات النص: يتم قراءة مستند يحتوي على العديد من مجموعات البيانات مجموعة بيانات واحدة في كل مرة على صفحتي <a href="/medical/net/json-to-dicom/">JSON إلى DICOM</a> و <a href="/medical/net/xml-to-dicom/">XML إلى DICOM</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="إطار بإطار">}}

<p>يتم معالجة بيانات متعدد الإطارات إطارًا بإطار، لذا سلسلة مكوّنة من 500 إطار تُعالج إطارًا واحدًا في كل مرة بدلاً من معالجة عنصر بيانات البكسل بالكامل.</p>

<div class="codeblock" id="code">
 <h3>التجول عبر الإطارات - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="حيث يحدد هذا التصميم">}}

<ul>
<li>فهرسة الأرشيف والهجرة: ملايين الملفات، ويهم فقط الرأس حتى يتم نقل شيء ما.</li>
<li>الموجهات وعقد التخزين: تقبل دراسة، تقرأ ما يلزم لتوجيهها، وتنقل البايتات.</li>
<li>خطوط تجميع الذكاء الاصطناعي: بناء القائمة من البيانات الوصفية، ثم سحب الإطارات للجزء الفرعي الذي يتم التدريب عليه فعليًا.</li>
<li>حاويات بحدود ذاكرة: مجموعة العمل تتبع الاستراتيجية، لا حجم الملف.</li>
<li>بيانات الشريحة الكاملة وOCT: ملفات لا تكون قراءة كل شيء فيها خيارًا على الإطلاق.</li>
</ul>

<p>دليل <a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">إدارة الذاكرة</a> يشرح الاستراتيجيات بالتفصيل، و<a href="/medical/net/dicom-networking/">شبكات DICOM</a> تعرض نفس البيانات القادمة عبر DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="الوثائق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="دليل المطور" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
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
