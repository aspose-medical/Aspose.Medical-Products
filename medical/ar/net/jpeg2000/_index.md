---
title: ضغط DICOM JPEG 2000 في C# .NET | Aspose.Medical
weight: 2000
description: قراءة، كتابة، وتحويل ملفات DICOM مع ضغط JPEG 2000 في C# .NET. دعم للصور بدقة 8 بت و 16 بت، أوضاع غير فقدانية وفقدانية، بيانات متعددة المكونات مع Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="دعم DICOM JPEG 2000 في .NET C#" h2="قراءة، كتابة، وتحويل ملفات DICOM مع ضغط JPEG 2000. أوضاع غير فقدانية وفقدانية، بيانات بكسل 8‑بت و 16‑بت، صور متعددة المكونات — كل ذلك في .NET نقي." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 في التصوير الطبي">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) هو المعيار الأكثر انتشارًا لضغط الموجات في التصوير الطبي. على عكس JPEG التقليدي، يوفر ضغطًا غير فقدانيًا وفقدانيًا في برنامج ترميز واحد، وفك تشفير تدريجي للوصول إلى مناطق الاهتمام، ونسب ضغط متفوقة &mdash; مما يجعله مثاليًا لأرشفة الدراسات الكبيرة ونقل الصور عبر شبكات ذات نطاق محدود.</p>

<p><strong>Aspose.Medical for .NET</strong> توفر تنفيذًا نقيًا بلغة C# لبرنامج ترميز JPEG 2000 بدون تبعيات أصلية. يمكن للمكتبة قراءة، عرض، وتحويل ملفات DICOM المضغوطة بأي من صيغ النقل الأربعة القياسية للـ JPEG 2000.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="صيغ نقل JPEG 2000 المدعومة">}}

<table class="table table-bordered">
<thead>
<tr>
<th>صيغة النقل</th>
<th>UID</th>
<th>الوضع</th>
<th>قراءة</th>
<th>كتابة</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 غير فقداني فقط</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>غير فقداني</td><td>8 بت و 16 بت</td><td>8 بت</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>فقداني أو غير فقداني</td><td>8 بت و 16 بت</td><td>8 بت</td></tr>
<tr><td>JPEG 2000 الجزء 2 متعدد المكونات غير فقداني فقط</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>غير فقداني</td><td>8 بت و 16 بت</td><td>8 بت</td></tr>
<tr><td>JPEG 2000 الجزء 2 متعدد المكونات</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>فقداني أو غير فقداني</td><td>8 بت و 16 بت</td><td>8 بت</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="بيانات بكسل 8-بت و 16-بت">}}

<p>غالبًا ما تستخدم الصور الطبية 16 بت لكل عينة لالتقاط النطاق الديناميكي الكامل للأنواع مثل التصوير المقطعي (عادةً 12‑بت مخزن في 16‑بت) والرنين المغناطيسي. تتعامل Aspose.Medical مع كلا عمقي البت لضغط JPEG 2000:</p>

<ul>
<li><strong>القراءة (فك الضغط)</strong>: دعم كامل لكل من ملفات DICOM المضغوطة بـ JPEG 2000 بدقة 8‑بت و 16‑بت. المكتبة تقوم بفك ترميز بيانات البكسل بشكل صحيح بغض النظر عن قيم Bits Allocated و Bits Stored و High Bit الأصلية.</li>
<li><strong>الكتابة (الضغط)</strong>: يدعم حاليًا صور 8‑بت. دعم كتابة 16‑بت مخطط للإصدار المستقبلي.</li>
</ul>

<div class="codeblock" id="code">
 <h3>قراءة وفحص DICOM المضغوط بـ JPEG 2000 - C#</h3>
 <pre><code class="cs">// Open a JPEG 2000 compressed DICOM file (8-bit or 16-bit)
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Check transfer syntax
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax
Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");

// Inspect pixel data bit depth
PixelData pixelData = PixelData.Create(dicomFile.Dataset);
Console.WriteLine($"Bits Allocated: {pixelData.BitsAllocated}");
Console.WriteLine($"Bits Stored: {pixelData.BitsStored}");
Console.WriteLine($"High Bit: {pixelData.HighBit}");
Console.WriteLine($"Samples Per Pixel: {pixelData.SamplesPerPixel}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تحويل إلى JPEG 2000">}}

<p>استخدم طريقة <code>Transcode</code> لضغط أي ملف DICOM إلى JPEG 2000 أو للتحويل بين أوضاع JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>ضغط DICOM إلى JPEG 2000 غير فقداني - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>ضغط DICOM إلى JPEG 2000 فقداني - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="فك ضغط ملفات DICOM بتنسيق JPEG 2000">}}

<p>فك ضغط ملفات JPEG 2000 إلى صيغة نقل غير مضغوطة للمعالجة أو التحليل أو التوافق مع الأنظمة التي لا تدعم JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>فك ضغط JPEG 2000 إلى صيغة غير مضغوطة - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>يمكنك أيضًا فك الضغط والتحويل إلى صيغ ضغط أخرى في خطوة واحدة:</p>

<div class="codeblock" id="code">
 <h3>تحويل بين صيغ الضغط - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="عرض صور DICOM المضغوطة بـ JPEG 2000">}}

<p>يمكن عرض ملفات DICOM المضغوطة بـ JPEG 2000 إلى بيانات بكسل للعرض أو التصدير، مثل أي صيغة نقل أخرى:</p>

<div class="codeblock" id="code">
 <h3>عرض إطار مضغوط بـ JPEG 2000 - C#</h3>
 <pre><code class="cs">// Open JPEG 2000 DICOM file
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Render the first frame — decompression is handled automatically
using PixelImage&lt;Bgra32&gt; image = dicomFile.RenderImage(0);

// Copy the BGRA32 pixel data for display or export
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = new Bgra32[width * height];
image.CopyPixelsTo(pixels);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="غير فقداني مقابل فقداني JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>الجانب</th>
<th>JPEG 2000 غير فقداني</th>
<th>JPEG 2000 فقداني</th>
</tr>
</thead>
<tbody>
<tr><td>صيغة النقل</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>جودة الصورة</td><td>بدقة بكسلية &mdash; مطابقة للأصل</td><td>متشابه بصريًا، بعض البيانات فقدت نهائيًا</td></tr>
<tr><td>نسبة الضغط</td><td>عادةً من 2:1 إلى 3:1</td><td>عادةً من 10:1 إلى 30:1 أو أكثر</td></tr>
<tr><td>الأمثل لـ</td><td>أرشفة تشخيصية، سجلات قانونية، قراءة أولية</td><td>مراجعة أولية، الطب عن بُعد، نقل عبر الشبكة</td></tr>
<tr><td>آمن للعودة</td><td>نعم</td><td>لا &mdash; إعادة الترميز تضعف الجودة أكثر</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 الجزء 2 متعدد المكونات">}}

<p>JPEG 2000 الجزء 2 (ISO/IEC 15444-2) يوسّع برنامج الترميز القياسي بقدرات تحويل متعددة المكونات. يُستخدم لهذا في الصور الطبية الملونة وأنواع تصوير تنتج بيانات متعددة القنوات. Aspose.Medical يدعم كلا صيغ نقل الجزء 2:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; ضغط غير فقداني مع إزالة التفاعل بين المكونات للحصول على ضغط أمثل للبيانات متعددة القنوات.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; ضغط فقداني أو غير فقداني مع تحويلات متعددة المكونات.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 عالي السرعة (HTJ2K) — قادم قريبًا">}}

<p>HTJ2K (ISO/IEC 15444-15) هو امتداد الجيل التالي لـ JPEG 2000 صُمّم لتوفير سرعات تشفير وفك تشفير أسرع بصورة ملحوظة مع الحفاظ على نفس كفاءة الضغط. من المتوقع أن يصبح برنامج الترميز المفضل لتدفقات العمل في التصوير الطبي الفوري.</p>

<p>ستضيف Aspose.Medical دعم HTJ2K في إصدار مستقبلي، يغطي ثلاث صيغ نقل:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; غير فقداني فقط</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; غير فقداني مع ترتيب تقدم RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; فقداني أو غير فقداني</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="الوثائق" href="https://docs.aspose.com/medical/net/" >}}
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
{{< blocks/products/pf/slr-element name="قصص النجاح" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
