---
title: تحويل بنية نقل DICOM في C# .NET | Aspose.Medical
weight: 16000
description: تحويل ملفات DICOM بين بنى النقل في C# .NET. دعم تنسيقات JPEG، JPEG 2000، JPEG-LS، RLE، وغير المضغوطة باستخدام Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تحويل بنية نقل DICOM في .NET C#" h2="تحويل ملفات DICOM بين بنى النقل غير المضغوطة، JPEG، JPEG 2000، JPEG-LS، وRLE. مكتبة .NET صافية دون أي تبعيات أصلية." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="ما هي بنية النقل؟">}}

<p>تحدد <strong>بنية النقل</strong> طريقة ترميز بيانات DICOM للتخزين والنقل. تُحدد ثلاثة جوانب رئيسية: ترتيب البايتات (endianness)، ما إذا كانت Value Representations صريحة أو ضمنية، وخ algoritmo الضغط المطبق على بيانات البكسل. كل ملف DICOM يعلن عن بنية النقل الخاصة به في عنوان معلومات التعريف (File Meta Information).</p>

<p>تدعم أجهزة طبية مختلفة، وخوادم PACS، وتطبيقات العرض مجموعات مختلفة من بنى النقل. <strong>Aspose.Medical for .NET</strong> يقدم طريقة <code>Transcode</code> للتحويل بين بنى النقل، مما يتيح التشغيل البيني، تحسين التخزين، والتوافق مع أدوات المعالجة &mdash; كل ذلك في مكتبة .NET صافية دون أي تبعيات أصلية.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تحويل ملف DICOM في C#">}}

<p>طريقة <code>DicomFile.Transcode</code> تحوِّل ملف DICOM من بنية النقل الحالية إلى أي بنية هدف مدعومة. تُعيد الطريقة كائن <code>DicomFile</code> جديد — يبقى الأصل دون تغيير:</p>

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

<p>يمكنك أيضًا تحويل على مستوى <code>Dataset</code> مباشرةً:</p>

<div class="codeblock" id="code">
 <h3>تحويل مجموعة البيانات - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="بنَى النقل المدعومة">}}

<p>الجدول التالي يُظهر جميع بنى نقل بيانات صور DICOM القياسية وحالة الدعم الحالية لها في Aspose.Medical for .NET. جميع المُرمِّزات المدعومة مُنفّذة بلغة C# صافية وتعمل بشكل مستقل عن النظام الأساسي.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>بنية النقل</th>
<th>UID</th>
<th>النوع</th>
<th>الحالة</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>غير مضغوط</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>غير مضغوط</td><td>مدعوم</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>غير مضغوط</td><td>مدعوم</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>غير مضغوط</td><td>مدعوم</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>مدعوم</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>فقدان، 8‑بت</td><td>مدعوم</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>فقدان، 12‑بت</td><td>غير مدعوم</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>بدون فقدان</td><td>مدعوم (8‑بت فقط)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>بدون فقدان</td><td>مدعوم (8‑بت فقط)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>بدون فقدان</td><td>مدعوم</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>قريب من عدم الفقدان</td><td>مدعوم</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>بدون فقدان</td><td>مدعوم (قراءة 8/16‑بت، كتابة 8‑بت)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>فقدان أو بدون فقدان</td><td>مدعوم (قراءة 8/16‑بت، كتابة 8‑بت)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>بدون فقدان</td><td>مدعوم (قراءة 8/16‑بت، كتابة 8‑بت)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>فقدان أو بدون فقدان</td><td>مدعوم (قراءة 8/16‑بت، كتابة 8‑بت)</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>بدون فقدان</td><td>مدعوم</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>بدون فقدان</td><td>قريبًا</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>بدون فقدان</td><td>قريبًا</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>فقدان أو بدون فقدان</td><td>قريبًا</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>بدون فقدان</td><td>قريبًا</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>بدون فقدان</td><td>قريبًا</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>فقدان أو بدون فقدان</td><td>قريبًا</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="سيناريوهات التحويل الشائعة">}}

<p>تتطلب سير العمل المختلفة استراتيجيات تحويل مختلفة. إليكم أكثر السيناريوهات شيوعًا:</p>

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
 <h3>ضغط للإرسال عبر الشبكة - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="استعراض خصائص بنية النقل">}}

<p>تُعرِض فئة <code>TransferSyntax</code> خصائص تصف خصائص الترميز. استخدمها لاستعراض بنية النقل الحالية للملف أو لاختيار بنية هدف مناسبة:</p>

<div class="codeblock" id="code">
 <h3>قراءة خصائص بنية النقل - C#</h3>
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
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>معرّف فريد لبنية النقل</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>ما إذا كانت Value Representations مشفرة صراحةً</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>ما إذا كان ترتيب البايتات little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>ما إذا كانت بيانات البكسل مغلّفة (مضغوطة)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>ما إذا كانت طريقة الضغط ذات فقدان</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>ما إذا كانت بنية النقل تستخدم ضغط deflate</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>ما إذا كانت بنية النقل متقادمة وفق معيار DICOM</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>معرّف معيار ISO لطريقة الضغط ذات الفقدان</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الضغط الفاقد مقابل الضغط غير الفاقد">}}

<p>فهم الفرق بين الضغط الفاقد والضغط غير الفاقد أمر حاسم عند تحويل ملفات DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>الجانب</th>
<th>بدون فقدان</th>
<th>فاقد</th>
</tr>
</thead>
<tbody>
<tr><td>Image quality</td><td>دقة بكسلية — البيانات الأصلية محفوظة بالكامل</td><td>بعض البيانات تُفقد بشكل دائم لتحقيق حجم أصغر</td></tr>
<tr><td>Compression ratio</td><td>عادةً 2:1 إلى 3:1</td><td>عادةً 10:1 إلى 30:1 أو أعلى</td></tr>
<tr><td>Round-trip safe</td><td>نعم — فك الضغط والحصول على بكسلات مطابقة</td><td>لا — كل إعادة ترميز فاقد تقلل الجودة أكثر</td></tr>
<tr><td>Use cases</td><td>أرشفة، تشخيص، سجلات قانونية</td><td>مراجعة أولية، طب عن بُعد، إرسال عبر الشبكة</td></tr>
<tr><td>Supported codecs</td><td>JPEG Lossless، JPEG-LS، JPEG 2000 Lossless، RLE</td><td>JPEG Baseline، JPEG-LS Near-Lossless، JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>مهم:</strong> تحويل ملف مضغوط بفقدان إلى بنية نقل غير فائضة لا يعيد البيانات المفقودة. تدهور الجودة الناتج عن الضغط الفاقد الأصلي دائم.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="مصادر التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="الوثائق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="الكود المصدر" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="مراجع API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="دعم المنتج" tabId="support" >}}
{{< blocks/products/pf/slr-element name="دعم مجاني" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="دعم مدفوع" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="المدونة" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="لماذا Aspose.Medical for .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="قائمة العملاء" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="قصص النجاح" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
