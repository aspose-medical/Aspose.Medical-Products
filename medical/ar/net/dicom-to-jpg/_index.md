---
title: تحويل DICOM إلى JPG في C# .NET | Aspose.Medical
weight: 7000
description: عرض صور DICOM إلى بيانات البكسل في C# .NET مع التحكم في النافذة/المستوى، وعرض الطبقة المتراكبة، ودعم الإطارات المتعددة. استخدم خط أنابيب LUT الكامل لـ DICOM مع واجهة برمجة تطبيقات Aspose.Medical واحفظه كـ JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تحويل DICOM إلى JPG في .NET C#" h2="عرض صور DICOM إلى بيانات بكسل خام مع دعم كامل لخط أنابيب التدرج الرمادي بما يشمل النافذة/المستوى، LUT النمط، LUT القيمة ذات الاهتمام (VOI)، وعرض الطبقة المتراكبة. احفظ كـ JPG باستخدام أي مكتبة تصوير .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="خط أنابيب عرض صور DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> يعرض صور DICOM عبر خط أنابيب التدرج الرمادي القياسي لـ DICOM كما هو معرف في مواصفة DICOM. عملية العرض تطبق سلسلة من جداول البحث (Look-Up Tables - LUTs) لتحويل قيم البكسل المخزنة الخام إلى صور قابلة للعرض:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; يحول قيم البكسل المخزنة إلى قيم مستقلة عن الشركة المصنعة باستخدام Rescale Slope/Intercept أو تسلسل LUT.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; يطبق النافذة/المستوى (Window Center و Window Width) لاختيار نطاق القيم المراد عرضها، مع دعم التحولات Linear و Linear Exact و Sigmoid.</li>
<li><strong>Presentation LUT</strong> &mdash; يطابق القيم النهائية إلى قيم P-Values قابلة للعرض، بما في ذلك دعم INVERSE (عكس الصورة).</li>
<li><strong>Output LUT</strong> &mdash; يحول قيم التدرج الرمادي إلى RGB للعرض، مع إمكانية التلوين باستخدام خرائط ألوان مخصصة أو جداول Palette Color.</li>
</ol>

<p>الناتج المعروض هو <code>IRawImage</code> &mdash; مخزن بكسلات BGRA32 (8 بت لكل قناة) يحتوي على <code>Width</code>، <code>Height</code>، ومصفوفة <code>Pixels</code>. يمكنك بعد ذلك حفظ بيانات البكسل هذه إلى JPG باستخدام أي مكتبة تصوير .NET مثل <code>System.Drawing</code> أو <code>SkiaSharp</code> أو <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="عرض DICOM إلى بيانات البكسل في C#">}}

<p>حمّل ملف DICOM وعرض إطار إلى <code>IRawImage</code> يحتوي على بيانات بكسل BGRA32:</p>

<div class="codeblock" id="code">
 <h3>عرض صورة DICOM - C#</h3>
 <pre><code class="cs">using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;

// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Render the first frame (index 0) through the full LUT pipeline
IRawImage image = dicomFile.RenderImage(0);

// Access the rendered pixel data
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = image.Pixels; // BGRA32 pixel buffer</code></pre>
</div>

<p>طريقة <code>RenderImage</code> تطبق تلقائيًا خط أنابيب التدرج الرمادي الكامل باستخدام قيم النافذة/المستوى ومعلمات LUT المخزنة في ملف DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="حفظ الصورة المعروضة كـ JPG">}}

<p>تقدم <code>IRawImage</code> بيانات بكسل BGRA32 خام يمكن حفظها كـ JPG باستخدام أي مكتبة تصوير .NET. إليك مثال باستخدام <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>حفظ إطار DICOM المعروض كـ JPG - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");
IRawImage image = dicomFile.RenderImage(0);

// Create a Bitmap from the BGRA32 pixel data
using var bitmap = new Bitmap(image.Width, image.Height, PixelFormat.Format32bppArgb);
var bitmapData = bitmap.LockBits(
    new Rectangle(0, 0, image.Width, image.Height),
    ImageLockMode.WriteOnly,
    PixelFormat.Format32bppArgb);

// Copy BGRA32 pixels to the bitmap
MemoryMarshal.AsBytes(image.Pixels.AsSpan())
    .CopyTo(new Span&lt;byte&gt;(
        (void*)bitmapData.Scan0,
        bitmapData.Stride * bitmapData.Height));

bitmap.UnlockBits(bitmapData);

// Save as JPG
bitmap.Save("ct_scan.jpg", ImageFormat.Jpeg);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="نافذة/مستوى مخصص (Windowing)">}}

<p>النافذة/المستوى (المعروفة أيضًا باسم windowing أو W/L) هي أهم معلمة لعرض صور DICOM. تتحكم في أي نطاق من قيم البكسل يتم تحويله إلى نطاق التدرج الرمادي المرئي. يحدد <strong>Window Center</strong> النقطة الوسطى، ويحدد <strong>Window Width</strong> نطاق القيم المعروضة:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; تباين عالٍ، عدد أقل من مستويات الرمادي المعروضة (مفيد للأنسجة الناعمة).</li>
<li><strong>Wide window</strong> &mdash; تباين منخفض، عدد أكبر من مستويات الرمادي المعروضة (مفيد للعظام أو الرئتين).</li>
</ul>

<div class="codeblock" id="code">
 <h3>العرض باستخدام نافذة/مستوى مخصص - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window: center=40, width=400
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window: center=400, width=1800
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Lung window: center=-600, width=1500
var lung = new GrayscaleRenderOptions
{
    WindowCenter = -600,
    WindowWidth = 1500
};
IRawImage lungImage = dicomFile.RenderImage(lung, 0);</code></pre>
</div>

<p>إعدادات نافذة CT الشائعة:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>نوع النسيج</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>الأنسجة الناعمة</td><td>40</td><td>400</td></tr>
<tr><td>رئة</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>عظم</td><td>400</td><td>1800</td></tr>
<tr><td>دماغ</td><td>40</td><td>80</td></tr>
<tr><td>كبد</td><td>60</td><td>150</td></tr>
<tr><td>المنصف</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="خيارات عرض التدرج الرمادي">}}

<p>فئة <code>GrayscaleRenderOptions</code> توفر تحكمًا كاملاً في خط أنابيب عرض التدرج الرمادي:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>الخاصية</th>
<th>الوصف</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>مركز نطاق القيم المعروضة (متوسط أفقى من VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>عرض نطاق القيم المعروضة (يتحكم في التباين)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>منحدر إعادة التحجيم لـ Modality LUT — يُضرب في قيم البكسل المخزنة</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>اعتراض إعادة التحجيم لـ Modality LUT — يُضاف بعد ضرب المنحدر</td></tr>
<tr><td><code>Invert</code></td><td>يعكس صورة التدرج الرمادي (يطبق شكل INVERSE Presentation LUT)</td></tr>
<tr><td><code>BitDepth</code></td><td>معلومات عمق البت: BitsAllocated، BitsStored، HighBit، IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>عرض التدرج الرمادي المعكوس - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray.dcm");

// Render with inverted grayscale (white-on-black becomes black-on-white)
var options = new GrayscaleRenderOptions
{
    WindowCenter = 2048,
    WindowWidth = 4096,
    Invert = true
};

IRawImage image = dicomFile.RenderImage(options, 0);</code></pre>
</div>

<p>استخدم <code>RenderOptionsFactory.Create(pixelData)</code> لتعبئة خيارات العرض تلقائيًا من سمات بيانات بكسل مجموعة بيانات DICOM، بما في ذلك عمق البت، معلمات إعادة التحجيم، النافذة/المستوى، وإعدادات العكس.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="عرض الطبقة المتراكبة">}}

<p>يمكن أن تحتوي صور DICOM على طبقات متراكبة — رسومات أو تعليقات نصية مدمجة في الصورة. فئة <code>RenderOptions</code> تتحكم فيما إذا كانت الطبقات المتراكبة تُعرض وأي لون ستظهر به:</p>

<div class="codeblock" id="code">
 <h3>العرض مع التحكم في الطبقة المتراكبة - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible in white (default behavior)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions
{
    ShowOverlays = false
};
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

<p>يدعم DICOM نوعين من الطبقات المتراكبة: طبقات <strong>Graphics</strong> (تعليقات، قياسات) وطبقات <strong>Region of Interest</strong> (ROI). يتم التحكم في كلا النوعين عبر إعداد <code>ShowOverlays</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="عرض DICOM متعدد الإطارات">}}

<p>العديد من ملفات DICOM من CT، MRI، والموجات فوق الصوتية تحتوي على إطارات متعددة (شرائح). يمكنك فحص عدد الإطارات وعرض كل إطار على حدة:</p>

<div class="codeblock" id="code">
 <h3>عرض جميع الإطارات من DICOM متعدد الإطارات - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;
Console.WriteLine($"Total frames: {totalFrames}");

// Render all frames to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as JPG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="أنواع تحويل VOI LUT">}}

<p>معيار DICOM يحدد عدة وظائف تحويل VOI LUT التي تتحكم في كيفية تطبيق تحويل النافذة/المستوى. Aspose.Medical for .NET يدعم جميع الأنواع القياسية:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>النوع</th>
<th>الوصف</th>
<th>حالة الاستخدام</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>تحويل خطي قياسي مع إزاحة من المركز</td><td>الأكثر شيوعًا، يستخدم لتصوير CT/MR العام</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>تحويل خطي دون إزاحة من المركز</td><td>عند الحاجة إلى تحويل قيمة بكسل دقيقة</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>تحويل على شكل S لتدرجات تباين أكثر سلاسة</td><td>تصور الأنسجة الناعمة، الماموجرافيا</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>LUT مخصص معرف كجدول بحث في مجموعة بيانات DICOM</td><td>منحنيات عرض خاصة بالمصنع أو بالنمط</td></tr>
</tbody>
</table>

<p>يختار محرك العرض تلقائيًا التحويل المناسب لـ VOI LUT بناءً على سمات مجموعة بيانات DICOM. عند استخدام <code>GrayscaleRenderOptions</code> مخصص، يتم تطبيق التحويل Linear بشكل افتراضي.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الأنماط المدعومة">}}

<p>Aspose.Medical for .NET يدعم عرض صور DICOM من جميع الأنماط الشائعة للتصوير الطبي:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>النمط</th>
<th>الوصف</th>
<th>الصيغة النموذجية</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>التصوير المقطعي المحوسب</td><td>تدرج رمادي، متعدد الإطارات، 12–16 بت</td></tr>
<tr><td><strong>MR</strong></td><td>التصوير بالرنين المغناطيسي</td><td>تدرج رمادي، متعدد الإطارات، 12–16 بت</td></tr>
<tr><td><strong>CR / DX</strong></td><td>التصوير الشعاعي المحوسب / الرقمي</td><td>تدرج رمادي، إطار واحد، 10–16 بت</td></tr>
<tr><td><strong>US</strong></td><td>الموجات فوق الصوتية</td><td>تدرج رمادي أو RGB، متعدد الإطارات</td></tr>
<tr><td><strong>MG</strong></td><td>الماموجرافيا</td><td>تدرج رمادي، دقة عالية، 12–16 بت</td></tr>
<tr><td><strong>XA</strong></td><td>تصوير الأوعية بالأشعة السينية</td><td>تدرج رمادي، متعدد الإطارات</td></tr>
<tr><td><strong>NM</strong></td><td>الطب النووي</td><td>تدرج رمادي، متعدد الإطارات</td></tr>
<tr><td><strong>PT</strong></td><td>PET (التصوير المقطعي البوزيتروني)</td><td>تدرج رمادي، متعدد الإطارات</td></tr>
</tbody>
</table>

<p>كلا التفسيرات الضوئية التدرج الرمادي (MONOCHROME1/MONOCHROME2) واللون (RGB, YBR_FULL, PALETTE COLOR) مدعومة. محرك العرض يتعامل تلقائيًا مع التحويل المناسب لمجال اللون.</p>

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
{{< blocks/products/pf/slr-element name="قصص النجاح" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
