---
title: JPEG XL لتقنية DICOM في C# .NET | Aspose.Medical
weight: 10500

description: تخزين صور DICOM بصيغة JPEG XL من خلال C#. JPEG XL غير مفقود يعيد البكسلات بتساوٍ كامل، في تجميع مُدارة واحد دون الحاجة إلى ترميز أصلي للنشر.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL لتقنية DICOM في .NET C#" h2="أحدث ضغط في معيار DICOM، مع أصغر ملفات غير مفقودة قمنا بقياسها، تم تنفيذها بلغة C# المُدارة وشُحّنت داخل تجميع واحد." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="لماذا وصل JPEG XL إلى DICOM">}}

<p>تزداد أرشيفات الطب ولا تنكمش أبداً. JPEG XL هو الترميز الذي صممته عالم التصوير بعد عقدين من الخبرة مع JPEG و JPEG 2000، وقد أضافه DICOM كصيغة نقل للسبب الذي يهم فرق التخزين: بالنسبة لنفس البكسلات، يكون الملف أصغر.</p>

<p><strong>Aspose.Medical لـ .NET</strong> يكتب ويقرأ JPEG XL عبر منفذ C# لمكتبة libjxl الموجود داخل المكتبة. الحزمة تُوزّع تجميعًا واحدًا، <code>Aspose.Medical.dll</code>، ولا يوجد ملف ثنائي أصلي بجانبه، لذا لا يتحول هذا الترميز الجديد إلى مشروع نشر: يعمل التجميع نفسه على Windows، على Linux، على وكيل البناء وفي حاوية.</p>

<p>صيغتا نقل تحملان البكسلات:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110)، للبيانات التشخيصية التي يجب أن تعود دون تغيير.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112)، للحالات التي يكون فيها حجم الملف الأصغر أهم من نسخة مطابقة.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ضغط دراسة، مع الحفاظ على كل بكسل">}}

<p>التحويل الترميزي يتم عبر استدعاء واحد، ومجموعة البيانات المحيطة بالبكسلات تنتقل معه.</p>

<div class="codeblock" id="code">
 <h3>تحويل ملف DICOM إلى JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>قمنا بقياسه على صورة بحجم 1714 × 1933 ببت 16 من مجموعة الاختبار الخاصة بنا: 6.3 ميجابايت غير مضغوطة تتحول إلى 2.7 ميجابايت في JPEG XL غير مفقود، وهو أصغر من نفس الصورة في HTJ2K غير مفقود. أرقامك الخاصة تعتمد على النوع، لذا قم بإجراء المقارنة على مجلد ملفاتك قبل الاختيار.</p>

<p>كلمة غير مفقود تُؤخذ حرفيًا هنا. قم بالتحويل إلى JPEG XL ثم العودة، وتكون بيانات البكسل مساوية للبايتات التي بدأت بها، لذا يمكن إعادة ضغط الأرشيف دون نقاش حول جودة التشخيص.</p>

<div class="codeblock" id="code">
 <h3>العودة إلى صيغة غير مضغطة - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="قراءة ما تم تخزينه بالفعل كـ JPEG XL">}}

<p>الملف الذي يأتي بصيغة JPEG XL يُفتح مثل أي ملف آخر. صيانة النقل تحدد ما هو، وتتوفر بيانات البكسل بمجرد فك تشفير الإطار.</p>

<div class="codeblock" id="code">
 <h3>فتح ملف JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL أو HTJ2K">}}

<p>كلاهما حديث، كلاهما غير مفقود عندما تطلب عدم فقدان، والمكتبة تكتب وتقرأ كليهما. يجيبون على أسئلة مختلفة.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>السؤال</th>
<th>الإجابة</th>
</tr>
</thead>
<tbody>
<tr><td>أيهما أنتج الملف الأصغر في اختبارنا</td><td>JPEG XL غير مفقود، بنسبة قليلة</td></tr>
<tr><td>أيها مُصمم للعرض التقدمي عبر الشبكة</td><td><a href="/medical/net/htj2k/">HTJ2K</a>، خاصةً المتغير RPCL</td></tr>
<tr><td>أيها دخل معيار DICOM أولاً</td><td>HTJ2K، لذا أكثر الأرشيفات تدعمه اليوم</td></tr>
<tr><td>أيها يتطلب تبعية أصلية هنا</td><td>لا أحد، كلاهما كود مُدار في تجميع واحد</td></tr>
</tbody>
</table>

<p>عادةً ما يأتي الاختيار من الجانب الآخر للربط: تحويل الترميز إلى الصيغة التي يقبلها الأرشيف، والحفاظ على باقي خط الأنابيب كما هو.</p>

<div class="codeblock" id="code">
 <h3>دع الأرشيف المستهدف يقرر - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="حيث يحقق الفائدة">}}

<ul>
<li>الأرشيفات طويلة الأجل: نفس الدراسات، تيرابايتات أقل، ولا فقدان لتبريره أمام أخصائي الأشعة.</li>
<li>فواتير التخزين السحابي: التوفير يتكرر كل شهر، بينما يتم التحويل مرة واحدة.</li>
<li>مجموعات البيانات للبحث والذكاء الاصطناعي: النسخ الأصغر تنتقل أسرع بين التخزين والتدريب.</li>
<li>النشر: عادةً ما يعني ترميز جديد كهذا بناءً أصليًا لكل منصة؛ هنا هو جزء من التجميع الذي تشير إليه بالفعل.</li>
</ul>

<p>المكتبة تكتب أيضًا الترميزات التي يحتويها الأرشيف الحالي: JPEG, JPEG-LS, JPEG 2000, HTJ2K و RLE. تغطي صفحة <a href="/medical/net/dicom-transfer-syntax-conversion/">تحويل صيغ النقل</a> المجموعة كاملة، <a href="/medical/net/htj2k/">HTJ2K</a> لها صفحتها الخاصة، و<a href="/medical/net/jpeg2000/">JPEG 2000</a> هو المصدر الذي يأتي منه كلا الترميزين الجديدين.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="التوثيق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="دليل المطور" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
