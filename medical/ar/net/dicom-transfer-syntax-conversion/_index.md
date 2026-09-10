---
title: تحويل صيغ نقل DICOM في C# .NET | Aspose.Medical
weight: 16000
description: تحويل ترميز ملفات DICOM بين صيغ النقل في C# .NET. دعم لـ JPEG، JPEG 2000، HTJ2K، JPEG XL، JPEG-LS، RLE، والصيغ غير المضغوطة باستخدام Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تحويل صيغ نقل DICOM في .NET C#" h2="تحويل ملفات DICOM بين صيغ النقل غير المضغوطة، JPEG، JPEG 2000، HTJ2K، JPEG XL، JPEG-LS، وRLE. مكتبة .NET خالصة دون أي تبعيات أصلية." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="ما هي صيغ النقل؟">}}

<p>تُعرّف <strong>صيغ النقل</strong> طريقة ترميز بيانات DICOM للتخزين والنقل. تحدد ثلاثة جوانب رئيسية: ترتيب البايتات (الحدية)، ما إذا كانت تمثيلات القيم صريحة أم ضمنية، وخalgorithm الضغط المُطبق على بيانات البكسل. كل ملف DICOM يُعلن عن صيغته النقلية في رأس معلومات البيانات الوصفية للملف.</p>

<p>تدعم أجهزة الطب المختلفة، وخوادم PACS، وتطبيقات العرض مجموعات مختلفة من صيغ النقل. <strong>Aspose.Medical for .NET</strong> توفر طريقة <code>Transcode</code> للتحويل بين صيغ النقل، مما يتيح التوافقية، تحسين التخزين، والملاءمة مع أدوات المعالجة &mdash; كل ذلك في مكتبة .NET خالصة دون أي تبعيات أصلية.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تحويل ملف DICOM في C#">}}

<p>طريقة <code>DicomFile.Transcode</code> تحول ملف DICOM من صيغته النقلية الحالية إلى أي صيغ هدف مدعومة. تُعيد الطريقة كائن <code>DicomFile</code> جديد &mdash; يبقى الأصلي دون تغيير:</p>

<div class="codeblock" id="code">
 <h3>تحويل DICOM الأساسي - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>يمكنك أيضًا تحويل الترميز على مستوى <code>Dataset</code> مباشرةً:</p>

<div class="codeblock" id="code">
 <h3>تحويل مجموعة البيانات - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="صيغ النقل المدعومة">}}

<p>الجدول التالي يدرج جميع صيغ نقل بيانات صور DICOM القياسية وحالة دعمها الحالية في Aspose.Medical for .NET. جميع البرامج الترميزية المدعومة مُنفذة بلغة C# الخالصة وتعمل على جميع المنصات بشكل مستقل.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>صيغة النقل</th>
<th>UID</th>
<th>النوع</th>
<th>الحالة</th>
</tr>
</thead>
<tbody>
<tr><td colspan=\"4\"><strong>غير مضغوط</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>غير مضغوط</td><td>مدعوم</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>غير مضغوط</td><td>مدعوم</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>غير مضغوط (متقاعد)</td><td>مدعوم</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>غير مضغوط</td><td>غير مدعوم</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>مدعوم</td></tr>
<tr><td colspan=\"4\"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>فقدان جودة، 8‑بت</td><td>مدعوم</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>فقدان جودة، 12‑بت</td><td>غير مدعوم</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>بدون فقدان</td><td>مدعوم (8‑بت فقط)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>بدون فقدان</td><td>مدعوم (8‑بت فقط)</td></tr>
<tr><td colspan=\"4\"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>بدون فقدان</td><td>مدعوم</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>قريب من عدم الفقدان</td><td>مدعوم</td></tr>
<tr><td colspan=\"4\"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>بدون فقدان</td><td>مدعوم (قراءة ألوان 8‑بت وتفاحل أحادي 16‑بت؛ كتابة أحادي 16‑بت أو RGB 8‑بت)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>فقدان أو بدون فقدان</td><td>مدعوم (قراءة ألوان 8‑بت وتفاحل أحادي 16‑بت؛ كتابة أحادي 16‑بت أو RGB 8‑بت)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>بدون فقدان</td><td>غير مدعوم</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>فقدان أو بدون فقدان</td><td>غير مدعوم</td></tr>
<tr><td colspan=\"4\"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>بدون فقدان</td><td>مدعوم</td></tr>
<tr><td colspan=\"4\"><strong>JPEG 2000 عالي السرعة (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>بدون فقدان</td><td>مدعوم</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>بدون فقدان</td><td>مدعوم</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>فقدان أو بدون فقدان</td><td>مدعوم</td></tr>
<tr><td colspan=\"4\"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>بدون فقدان</td><td>مدعوم</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>بدون فقدان</td><td>فقط فك تشفير (الترميز يتطلب تيار مصدر JPEG، ليس بيانات البكسل)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>فقدان أو بدون فقدان</td><td>مدعوم (وضع فقدان)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="سيناريوهات التحويل الشائعة">}}

<p>تتطلب سير العمل المختلفة استراتيجيات تحويل مختلفة. إليك أكثر السيناريوهات شيوعًا:</p>

<div class="codeblock" id="code">
 <h3>فك الضغط للمعالجة - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>ضغط للتخزين الأرشيفي - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>ضغط للنقل عبر الشبكة - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>استخدام أحدث برامج الترميز: HTJ2K و JPEG XL - C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="فحص خصائص صيغ النقل">}}

<p>تُظهر الفئة <code>TransferSyntax</code> خصائص تصف ميزات الترميز. استخدم هذه الخصائص لفحص صيغة النقل الحالية للملف أو لاختيار صيغة هدف مناسبة:</p>

<div class="codeblock" id="code">
 <h3>قراءة خصائص صيغة النقل - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>الخاصية</th>
<th>النوع</th>
<th>الوصف</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>المعرف الفريد لصيغة النقل</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>ما إذا كانت تمثيلات القيم مُرمَّزة صراحةً</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>ما إذا كان ترتيب البايتات little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>ما إذا كانت بيانات البكسل مُغلَّفة (مُضغطَة)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>ما إذا كانت طريقة الضغط فقداناً للبيانات</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>ما إذا كانت الصيغة تستخدم ضغط deflate</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>ما إذا كانت صيغة النقل متقاعدّة وفقًا لمعيار DICOM</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>معرف معيار ISO لطريقة الضغط الفاقد</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الضغط الفاقد مقابل الضغط غير الفاقد">}}

<p>فهم الفرق بين الضغط الفاقد وغير الفاقد أمر حاسم عند تحويل ملفات DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>الجانب</th>
<th>بدون فقدان</th>
<th>فاقد</th>
</tr>
</thead>
<tbody>
<tr><td>جودة الصورة</td><td>دقة بكسل مثالية &mdash; البيانات الأصلية محفوظة بالكامل</td><td>بعض البيانات تُفقد بشكل دائم لتحقيق حجم أصغر</td></tr>
<tr><td>نسبة الضغط</td><td>عادةً 2:1 إلى 3:1</td><td>عادةً 10:1 إلى 30:1 أو أعلى</td></tr>
<tr><td>آمن للعودة إلى الأصل</td><td>نعم &mdash; فك الضغط والحصول على بكسلات مطابقة</td><td>لا &mdash; كل عملية إعادة ترميز فقدان تؤدي إلى تدهور إضافي في الجودة</td></tr>
<tr><td>حالات الاستخدام</td><td>الأرشفة، التشخيص، السجلات القانونية</td><td>المراجعة الأولية، التطبيب عن بعد، النقل عبر الشبكة</td></tr>
<tr><td>برامج الترميز المدعومة</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>مهم:</strong> التحويل من ملف مضغوط بفقدان إلى صيغة غير فائضة لا يعيد البيانات المفقودة. تدهور الجودة الناتج عن الضغط الفاقد الأصلي ثابت.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
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
