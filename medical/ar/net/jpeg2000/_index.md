---
title: ضغط DICOM JPEG 2000 في C# .NET | Aspose.Medical
weight: 2000
description: اقرأ واكتب وحوّل ملفات DICOM باستخدام ضغط JPEG 2000 في C# .NET. يدعم الصور أحادية اللون والملونة من 1 إلى 16 بت، وضعَي lossless و lossy، بالإضافة إلى HTJ2K مع Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="دعم DICOM JPEG 2000 في .NET C#" h2="اقرأ واكتب وحوّل ملفات DICOM باستخدام ضغط JPEG 2000. وضعَي lossless و lossy، بيانات بكسل أحادية اللون وملونة من 1 إلى 16 بت، HTJ2K مدرج - كل ذلك في .NET النقي." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 في التصوير الطبي">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) هو المعيار الأكثر استخدامًا لضغط الموجات في التصوير الطبي. على عكس JPEG التقليدي، يوفر كلًا من ضغط lossless و lossy في مُشفّر واحد، فك ترميز تدريجي للوصول إلى مناطق الاهتمام، ونسب ضغط متفوقة &mdash; مما يجعله مثاليًا لأرشفة الدراسات الضخمة ونقل الصور عبر شبكات ذات سعات محدودة.</p>

<p><strong>Aspose.Medical for .NET</strong> يوفر تنفيذًا نقيًا بلغة C# لمرمز JPEG 2000 دون أي تبعيات أصلية. يمكن للمكتبة قراءة، عرض، وتحويل ملفات DICOM المضغوطة باستخدام صيغ نقل JPEG 2000 الجزء 1، lossless و lossy.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="صيغ نقل JPEG 2000 المدعومة">}}

<table class="table table-bordered">
<thead>
<tr>
<th>صيغة النقل</th>
<th>المعرّف الفريد</th>
<th>الوضع</th>
<th>قراءة</th>
<th>كتابة</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 بدون فقدان فقط</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>بدون فقدان</td><td>أحادي اللون وملون، من 1 إلى 16 بت</td><td>أحادي اللون، ألوان لوحة وRGB، من 1 إلى 16 بت</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>فقدان جزئي أو بدون فقدان</td><td>أحادي اللون وملون، من 1 إلى 16 بت</td><td>أحادي اللون وRGB، من 1 إلى 16 بت</td></tr>
<tr><td>JPEG 2000 الجزء 2 متعدد المكوّنات بدون فقدان فقط</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>بدون فقدان</td><td>غير مدعوم</td><td>غير مدعوم</td></tr>
<tr><td>JPEG 2000 الجزء 2 متعدد المكوّنات</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>فقدان جزئي أو بدون فقدان</td><td>غير مدعوم</td><td>غير مدعوم</td></tr>
<tr><td>HTJ2K بدون فقدان فقط</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>بدون فقدان</td><td>أحادي اللون وملون</td><td>أحادي اللون وملون</td></tr>
<tr><td>HTJ2K مع خيارات RPCL بدون فقدان فقط</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>بدون فقدان</td><td>أحادي اللون وملون</td><td>أحادي اللون وملون</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>فقدان جزئي أو بدون فقدان</td><td>أحادي اللون وملون</td><td>أحادي اللون وملون</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="بيانات البكسل من 1 إلى 16 بت">}}

<p>غالبًا ما تستخدم الصور الطبية 16 بت لكل عينة لالتقاط النطاق الديناميكي الكامل للأنماط مثل CT (عادةً 12‑بت مخزَّن في 16‑بت) وMRI. يتعامل Aspose.Medical مع جميع الأعماق من 1 إلى 16 بت لتقنية JPEG 2000، في عينات 8‑بت أو 16‑بت:</p>

<ul>
<li><strong>القراءة (فك الضغط)</strong>: ملفات أحادية اللون (CT, MRI, X-ray)، ملفات ألوان لوحة وملفات ألوان ثلاثية المكوّنات (RGB, YBR_RCT, YBR_ICT، وغيرها من تسميات اللون مثل YBR_FULL، مُفسَّرة ككود) مع 1 إلى 16 بت لكل عينة، موقعة أو غير موقعة. يتم رفض CMYK، ملفات التعريف ICC، لوحة JP2 وتيارات اللون المُنمَّطة فرعيًا مع استثناء واضح بدلًا من صورة خاطئة صامتة.</li>
<li><strong>الكتابة (الضغط)</strong>: صور أحادية اللون وRGB مع 1 إلى 16 بت لكل عينة، موقعة أو غير موقعة، وصور ألوان لوحة باستخدام JPEG 2000 بدون فقدان. لا يتم ترميز صور YBR إلا إذا تم فك ترميزها إلى RGB، كما هو الحال في ملفات JPEG YBR ذات الفقدان.</li>
</ul>

<div class="codeblock" id="code">
 <h3>قراءة وفحص ملفات DICOM المضغوطة بـ JPEG 2000 - C#</h3>
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
 <h3>ضغط DICOM إلى JPEG 2000 بدون فقدان - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>ضغط DICOM إلى JPEG 2000 بفقدان - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="فك ضغط ملفات DICOM بصيغة JPEG 2000">}}

<p>افك ضغط ملفات JPEG 2000 إلى صيغة نقل غير مضغوطة للمعالجة أو التحليل أو التوافق مع الأنظمة التي لا تدعم JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>فك ضغط JPEG 2000 إلى غير مضغوط - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="عرض صور DICOM بصيغة JPEG 2000">}}

<p>يمكن عرض ملفات DICOM المضغوطة بـ JPEG 2000 إلى بيانات بكسل للعرض أو التصدير، كما هو الحال مع أي صيغة نقل أخرى:</p>

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

{{< blocks/products/pf/feature-page-section h2="بدون فقدان مقابل فقدان JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>الجانب</th>
<th>JPEG 2000 بدون فقدان</th>
<th>JPEG 2000 بفقدان</th>
</tr>
</thead>
<tbody>
<tr><td>صيغة النقل</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>جودة الصورة</td><td>دقة بكسل كامل &mdash; مطابقة للأصل</td><td>متشابه بصريًا، بعض البيانات مفقودة بشكل دائم</td></tr>
<tr><td>نسبة الضغط</td><td>عادةً 2:1 إلى 3:1</td><td>عادةً 10:1 إلى 30:1 أو أعلى</td></tr>
<tr><td>الأفضل لـ</td><td>أرشفة تشخيصية، سجلات قانونية، القراءة الأولية</td><td>مراجعة أولية، طب عن بُعد، نقل عبر الشبكات</td></tr>
<tr><td>آمن للرحلة المستديرة</td><td>نعم</td><td>لا &mdash; إعادة الترميز تدهور الجودة أكثر</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 عالي الإنتاجية (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) يستبدل المشفر الحسابي البطيء في JPEG 2000 بمشفّر كتل أسرع. يحافظ على نفس تحويل المويجات، أوامر التقدم والجودة، ويقوم بالتحويل وفك التحويل أسرع بضع مرات. يتم تنفيذ جميع صيغ نقل DICOM HTJ2K الثلاث في Aspose.Medical باستخدام .NET النقي، للصور أحادية اللون والملونة، ويتم التحويل بين HTJ2K وكل صيغة أخرى مدعومة:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; بدون فقدان فقط</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; بدون فقدان مع ترتيب تقدم RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; فقدان جزئي أو بدون فقدان</li>
</ul>

<div class="codeblock" id="code">
 <h3>تحويل JPEG 2000 إلى HTJ2K والعودة - C#</h3>
 <pre><code class="cs">// Transcode a JPEG 2000 file to HTJ2K, and back to classic JPEG 2000
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile htj2kFile = j2kFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2kFile.Save("htj2k_lossless.dcm");

// HTJ2K with RPCL progression order, lossless
DicomFile rpclFile = j2kFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
rpclFile.Save("htj2k_rpcl.dcm");

// HTJ2K lossy
DicomFile htj2kLossy = j2kFile.Transcode(TransferSyntax.HTJ2K);
htj2kLossy.Save("htj2k_lossy.dcm");

// Any HTJ2K file decodes back to an uncompressed transfer syntax
DicomFile uncompressed = htj2kFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decoded.dcm");</code></pre>
</div>

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
{{< blocks/products/pf/slr-element name="مدونة" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="لماذا Aspose.Medical لـ .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="قائمة العملاء" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="قصص النجاح" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
