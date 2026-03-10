---
title: تحويل DICOM إلى PNG في C# .NET | Aspose.Medical
weight: 11000
description: عرض صور DICOM كبيانات بكسل في C# .NET بجودة غير مضغوطة، مع التحكم في نافذة/مستوى، ودعم متعدد الإطارات. استخدم خط أنابيب DICOM LUT الكامل عبر Aspose.Medical API واحفظ إلى PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تحويل DICOM إلى PNG في .NET C#" h2="عرض صور DICOM إلى بيانات بكسل خام مع دعم كامل لخط أنابيب التدرج الرمادي. الحفاظ على جودة الصورة مع التحكم في نافذة/مستوى، وعرض الطبقات الإضافية، ومعالجة متعددة الإطارات. احفظ إلى PNG غير مضغوط باستخدام أي مكتبة تصوير .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="تحويل DICOM إلى PNG">}}

<p><strong>Aspose.Medical for .NET</strong> يعرض صور DICOM عبر خط أنابيب التدرج الرمادي القياسي لـ DICOM (Modality LUT، VOI LUT، Presentation LUT، Output LUT) لإنتاج بيانات بكسل BGRA32 ذات نافذة صحيحة. PNG هو تنسيق غير مضغوط، مما يجعله الاختيار المفضل عندما يجب الحفاظ على جودة الصورة دون عيوب ضغط &mdash; وهو أمر أساسي للمراجعة التشخيصية، والأرشفة، وحالات البحث.</p>

<p>المخرج المعروض هو <code>IRawImage</code> &mdash; مخزن بكسلات BGRA32 يحتوي على <code>Width</code>، <code>Height</code>، ومصفوفة <code>Pixels</code>. يمكنك حفظ بيانات البكسل هذه إلى PNG باستخدام أي مكتبة تصوير .NET مثل <code>System.Drawing</code> أو <code>SkiaSharp</code> أو <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="عرض DICOM إلى بيانات البكسل في C#">}}

<p>حمّل ملف DICOM، واعرض الإطار المطلوب، وابدأ بالوصول إلى بيانات البكسل:</p>

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

<p>طريقة <code>RenderImage</code> تطبق تلقائيًا قيم النافذة/المستوى ومعلمات LUT المخزنة في مجموعة بيانات DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="حفظ الصورة المعروضة كـ PNG">}}

<p>توفر <code>IRawImage</code> بيانات بكسل BGRA32 خام يمكن حفظها كـ PNG غير مضغوط باستخدام أي مكتبة تصوير .NET. إليك مثالًا باستخدام <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>حفظ إطار DICOM المعروض كـ PNG - C#</h3>
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

// Save as lossless PNG
bitmap.Save("ct_scan.png", ImageFormat.Png);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="نافذة/مستوى مخصص">}}

<p>تحكم في نطاق قيم البكسل التي يتم عرضها عن طريق ضبط مركز النافذة وعرض النافذة. إعدادات النافذة المختلفة تكشف عن هياكل تشريحية مختلفة من نفس بيانات DICOM:</p>

<div class="codeblock" id="code">
 <h3>عرض باستخدام نافذة/مستوى مخصص - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);</code></pre>
</div>

<p>فئة <code>GrayscaleRenderOptions</code> تكشف أيضًا عن خصائص <code>RescaleSlope</code>، <code>RescaleIntercept</code>، <code>Invert</code>، و<code>BitDepth</code> للتحكم الكامل في خط أنابيب العرض.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="عرض DICOM متعدد الإطارات">}}

<p>غالبًا ما تحتوي ملفات DICOM من CT وMRI والموجات فوق الصوتية على إطارات متعددة (شرائح). عرض جميع الإطارات إلى بيانات بكسل:</p>

<div class="codeblock" id="code">
 <h3>عرض جميع الإطارات من DICOM متعدد الإطارات - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as PNG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="التحكم في الطبقة الإضافية">}}

<p>قد تحتوي صور DICOM على طبقات إضافية تحتوي على تعليقات توضيحية أو قياسات أو مناطق اهتمام. تحكم في ظهور الطبقة الإضافية ولونها عند العرض:</p>

<div class="codeblock" id="code">
 <h3>عرض مع وبدون طبقات إضافية - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible (default)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions { ShowOverlays = false };
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="PNG مقابل JPG للصور الطبية">}}

<p>اختيار بين PNG و JPG يعتمد على حالة الاستخدام:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>الخاصية</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>الضغط</strong></td><td>غير مضغوط</td><td>مضغوط</td></tr>
<tr><td><strong>جودة الصورة</strong></td><td>دقيقة للبيكسل &mdash; لا توجد عيوب</td><td>هناك احتمال لوجود عيوب ضغط طفيفة</td></tr>
<tr><td><strong>حجم الملف</strong></td><td>أكبر (عادةً 2&ndash;10 أضعاف مقارنة بـ JPG)</td><td>أصغر</td></tr>
<tr><td><strong>الشفافية</strong></td><td>مدعومة (قناة ألفا)</td><td>غير مدعومة</td></tr>
<tr><td><strong>الأفضل لـ</strong></td><td>المراجعة التشخيصية، الأرشفة، البحث، الطبقات الإضافية</td><td>المشاركة على الويب، الصور المصغرة، المعاينات</td></tr>
<tr><td><strong>التنظيمية</strong></td><td>مفضلة للعمليات الحرجة من حيث الجودة</td><td>مقبولة للاستخدام غير التشخيصي</td></tr>
</tbody>
</table>

<p>يستخدم Aspose.Medical for .NET نفس خط أنابيب العرض لكلا هدفَي الإخراج. بيانات بكسل <code>IRawImage</code> هي نفسها بغض النظر عن تنسيق الهدف &mdash; اختيار التنسيق يؤثر فقط على خطوة الحفظ النهائية التي تقوم بها مكتبة التصوير الخاصة بك.</p>

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
