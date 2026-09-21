---
title: HTJ2K في C# .NET - JPEG 2000 عالي الإنتاجية للـ DICOM | Aspose.Medical
weight: 10000

description: ضغط وقراءة صور DICOM باستخدام JPEG 2000 عالي الإنتاجية من C#. HTJ2K غير فقداني، النسخة RPCL و HTJ2K فقداني، مُنفذة في .NET المُدارة دون الحاجة إلى أي برنامج ترميز أصلي للنشر.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K في .NET C#" h2="JPEG 2000 عالي الإنتاجية للـ DICOM: الضغط الذي أضافه المعيار لأرشفة سريعة وعرض سحابي، مُنفذ بلغة C# المُدارة دون الحاجة إلى أي مكوّن أصلي للتثبيت." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="ما الذي يغيّره HTJ2K">}}

<p>JPEG 2000 عالي الإنتاجية يحتفظ بالموجة وجودة الصورة الخاصة بـ JPEG 2000 ويستبدل الجزء الذي كان يجعل العملية بطيئة. ترميز الكتلة جديد، وفك الترميز أسرع بأكثر من مرتبة من حيث السرعة، وهذا هو السبب في اعتماد معيار DICOM له في ثلاث صيغ تحويل ولماذا انتقلت منصات التصوير السحابي إليه.</p>

<p>بالنسبة لفريق .NET السؤال العملي يختلف: من يستطيع فعليًا إنتاج تلك الملفات. معظم المكتبات تصل إلى HTJ2K عبر بناء أصلي OpenJPH، مما يعني وجود ملف ثنائي لكل منصة، خطوة بناء في الحاوية وإحدى الاعتماديات التي سيطرح عنها مراجعة الأمان. <strong>Aspose.Medical for .NET</strong> ينفّذ برنامج الترميز في الشيفرة المُدارة داخل نفس الحزمة التي تقرأ وتكتب الملفات، لذا يعمل HTJ2K بنفس الطريقة على Windows وLinux وفي الحاوية، دون الحاجة إلى أي تثبيت.</p>

<p>يتم دعم ثلاث صيغ نقل، وجميعها يمكن قراءتها وكتابتها:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201)، JPEG 2000 عالي الإنتاجية غير فقداني.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202)، النسخة غير الفادانية مع ترتيب تقدم RPCL.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203)، JPEG 2000 عالي الإنتاجية.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ضغط دراسة إلى HTJ2K">}}

<p>ناديّة واحدة تنقل ملفًا إلى الصيغة الجديدة. مجموعة البيانات، الوسوم الخاصة ومعلومات التعريف الخاصة بالملف تنتقل معها.</p>

<div class="codeblock" id="code">
 <h3>تحويل ملف DICOM إلى HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>على صورة بدقة 1714 × 1933 16‑بت من مجموعة الاختبار الخاصة بنا، يقل حجم الملف من 6.3 ميغابايت إلى 2.9 ميغابايت، وتعود البكسلات بنفس الشكل. تختلف الأرقام حسب النوع والصورة، لذا قم بالقياس على بياناتك الخاصة، وذلك عبر حلقة واحدة على الملفات التي لديك بالفعل.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossless يعني غير فقداني">}}

<p>البيانات التشخيصية لا تتحمل برنامج ترميز غير دقيق. قم بتحويل إلى HTJ2K غير فقداني ثم العودة، وستكون بيانات البكسل مطابقة تمامًا للبايتات الأصلية، وهي الخاصية التي يمكنك التحقق منها في مجموعة الاختبار الخاصة بك قبل الموافقة على إعادة ضغط الأرشيف.</p>

<div class="codeblock" id="code">
 <h3>العودة إلى صيغة غير مضغوطة - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL، النسخة المخصصة للعرض عبر الشبكة">}}

<p>صيغة 1.2.840.10008.1.2.4.202 تخزن نفس تدفق الشيفرة غير الفاداني بترتيب تقدم RPCL: الدقة أولاً، ثم الموقع، ثم المكوّن، ثم الطبقة. القارئ الذي يقرأ فقط بداية التدفق يحصل على صورة منخفضة الدقة كاملة، وهو ما يحتاجه المشاهد عند فتح دراسة كبيرة عبر رابط لا يتحكم فيه.</p>

<div class="codeblock" id="code">
 <h3>ضغط باستخدام ترتيب تقدم RPCL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="قراءة ما يرسله الأرشيف لك">}}

<p>الجزء الآخر من المهمة هو قبول HTJ2K من الأنظمة التي تنتجه بالفعل. افتح الملف، تحقق من الصيغة التي يُخزن بها، وتعامل مع بيانات البكسل.</p>

<div class="codeblock" id="code">
 <h3>قراءة ملف HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>يتم معالجة الصور متعددة الإطارات إطارًا بإطار، لذا فإن سلسلة طويلة تستهلك الذاكرة لكل إطار بدلاً من كل دراسة.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="أين يثبت HTJ2K قيمته">}}

<ul>
<li>ترحيل الأرشيف: إعادة ضغط دراسة مخزنة إلى HTJ2K غير فقداني، تقليل الحجم، مع الحفاظ على البيانات التشخيصية دون تغيير.</li>
<li>السحابة و DICOMweb: سرعة فك الترميز هي ما يجعل المشاهد على الجانب العميل أو الخادم يشعر بالاستجابة الفورية للصور الكبيرة.</li>
<li>خطوط أنابيب الذكاء الاصطناعي: مجموعات التدريب تُقرأ كثيرًا مقارنة بكتابتها، ووقت فك الترميز هو التكلفة المتكررة.</li>
<li>الحاويات والخوادم غير التقليدية: برنامج الترميز جزء من التجميع، لذا لا تحتاج الصورة إلى مكتبة أصيلة أو مترجم أثناء عملية البناء.</li>
</ul>

<p>تضمّن المكتبة أيضًا JPEG XL، الإضافة الحديثة الأخرى إلى المعيار، بالإضافة إلى المشفرات القديمة التي قد يحتويها الأرشيف: JPEG، JPEG‑LS، JPEG 2000 وRLE. تغطي صفحة <a href="/medical/net/dicom-transfer-syntax-conversion/">تحويل صيغ النقل</a> المجموعة بالكامل، وتغطي صفحة <a href="/medical/net/jpeg2000/">JPEG 2000</a> المشفر الذي نشأ منه HTJ2K.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="الوثائق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="دليل المطور" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="مراجع API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="دعم المنتج" tabId="support" >}}
{{< blocks/products/pf/slr-element name="دعم مجاني" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="دعم مدفوع" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="المدونة" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="لماذا Aspose.Medical للـ .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="قائمة العملاء" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="قصص نجاح" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
