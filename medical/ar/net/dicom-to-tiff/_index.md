---
title: تحويل DICOM إلى TIFF في C# .NET | Aspose.Medical
weight: 12000
description: عرض صور DICOM إلى بيانات بكسل في C# .NET وحفظها كملف TIFF مع دعم الصفحات المتعددة لملفات DICOM متعددة الإطارات. استخدم خط أنابيب DICOM LUT الكامل مع Aspose.Medical API واحفظ إلى TIFF باستخدام أي مكتبة تصوير .NET.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تحويل DICOM إلى TIFF في .NET C#" h2="عرض صور DICOM إلى بيانات بكسل خام مع دعم خط أنابيب التدرج الرمادي الكامل. الحفاظ على جودة الصورة باستخدام التحكم في النافذة/المستوى، وعرض الطبقات الفوقية، ومعالجة متعددة الإطارات. حفظ إلى TIFF &mdash; بما في ذلك TIFF متعدد الصفحات &mdash; باستخدام أي مكتبة تصوير .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="تحويل DICOM إلى TIFF">}}

<p><strong>Aspose.Medical for .NET</strong> يعرض صور DICOM من خلال خط أنابيب التدرج الرمادي القياسي لـ DICOM (Modality LUT، VOI LUT، Presentation LUT، Output LUT) لإنتاج بيانات بكسل BGRA32 مُنَظَّمة بشكل صحيح. TIFF هو تنسيق متعدد الاستخدامات يُستخدم على نطاق واسع في التصوير الطبي، والبحوث العلمية، والنشر لأنه يدعم الضغط غير الفاقد، وأعماق البت العالية، و&mdash; الأهم من ذلك &mdash; <strong>المستندات متعددة الصفحات</strong>. هذا يجعل TIFF الخيار الطبيعي لتحويل ملفات DICOM متعددة الإطارات (CT، MRI، الموجات فوق الصوتية) إلى ملف خروج واحد يحافظ على جميع الشرائح.</p>

<p>الإخراج المعروض هو <code>IRawImage</code> &mdash; مخزن بكسل BGRA32 يحتوي على <code>Width</code>، <code>Height</code>، ومصفوفة <code>Pixels</code>. يمكنك حفظ بيانات البكسل هذه إلى TIFF باستخدام أي مكتبة تصوير .NET مثل <code>System.Drawing</code>، <code>SkiaSharp</code>، أو <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="عرض DICOM إلى بيانات البكسل في C#">}}

<p>تحميل ملف DICOM، عرض الإطار المطلوب، والوصول إلى بيانات البكسل:</p>

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

<p>طريقة <code>RenderImage</code> تُطبق تلقائيًا قيم النافذة/المستوى ومعلمات LUT المخزنة في مجموعة بيانات DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="حفظ الصورة المعروضة كملف TIFF">}}

<p>كائن <code>IRawImage</code> يوفر بيانات بكسل BGRA32 الخام التي يمكن حفظها كملف TIFF باستخدام أي مكتبة تصوير .NET. إليك مثال باستخدام <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>حفظ إطار DICOM المعروض كملف TIFF - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("xray.dcm");
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

// Save as TIFF
bitmap.Save("xray.tiff", ImageFormat.Tiff);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="TIFF متعدد الصفحات من DICOM متعدد الإطارات">}}

<p>ملفات DICOM متعددة الإطارات (مثلاً سلاسل CT أو MRI) تحتوي على عدة شرائح صور في ملف واحد. قدرة TIFF على تعدد الصفحات تسمح لك بتخزين جميع الإطارات في ملف خروج واحد، مع الحفاظ على الدراسة الكاملة بصيغة قابلة للقراءة عالميًا. استخدم مشفر TIFF في <code>System.Drawing</code> مع معلمات متعددة الإطارات:</p>

<div class="codeblock" id="code">
 <h3>تحويل DICOM متعدد الإطارات إلى TIFF متعدد الصفحات - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
int totalFrames = dicomFile.NumberOfFrames;

// Get the TIFF codec
var tiffCodec = ImageCodecInfo.GetImageEncoders()
    .First(c =&gt; c.FormatID == ImageFormat.Tiff.Guid);

// Multi-frame parameters
var multiFrameParams = new EncoderParameters(2);
multiFrameParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.MultiFrame);
multiFrameParams.Param[1] = new EncoderParameter(
    Encoder.Compression, (long)EncoderValue.CompressionLZW);

// Parameter for adding subsequent frames
var frameAddParams = new EncoderParameters(1);
frameAddParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.FrameDimensionPage);

// Parameter for flushing the final frame
var flushParams = new EncoderParameters(1);
flushParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.Flush);

Bitmap? tiffBitmap = null;

for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var frameBitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = frameBitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    frameBitmap.UnlockBits(bitmapData);

    if (i == 0)
    {
        // Save the first frame and start multi-page TIFF
        tiffBitmap = (Bitmap)frameBitmap.Clone();
        tiffBitmap.Save("ct_series.tiff", tiffCodec, multiFrameParams);
    }
    else
    {
        // Add subsequent frames
        tiffBitmap!.SaveAdd(frameBitmap, frameAddParams);
    }
}

// Flush and close the multi-page TIFF
tiffBitmap?.SaveAdd(flushParams);
tiffBitmap?.Dispose();</code></pre>
</div>

<p>يمكنك أيضًا حفظ كل إطار كملف TIFF منفصل عندما تحتاج إلى شرائح فردية:</p>

<div class="codeblock" id="code">
 <h3>تحويل كل إطار إلى ملف TIFF منفصل - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

for (int i = 0; i &lt; dicomFile.NumberOfFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var bitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = bitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    bitmap.UnlockBits(bitmapData);
    bitmap.Save($"frame_{i:D4}.tiff", ImageFormat.Tiff);
}</code></pre>
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
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

<p>فئة <code>GrayscaleRenderOptions</code> تكشف أيضًا عن خصائص <code>RescaleSlope</code>، <code>RescaleIntercept</code>، <code>Invert</code>، و<code>BitDepth</code> للتحكم الكامل في خط أنابيب العرض.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="عرض DICOM متعدد الإطارات">}}

<p>ملفات DICOM من CT، MRI، والموجات فوق الصوتية غالبًا ما تحتوي على عدة إطارات (شرائح). عرض جميع الإطارات إلى بيانات البكسل:</p>

<div class="codeblock" id="code">
 <h3>عرض جميع الإطارات من DICOM متعدد الإطارات - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as TIFF using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="التحكم في الطبقة الفوقية">}}

<p>قد تحتوي صور DICOM على طبقات فوقية تحتوي على تعليقات، قياسات، أو مناطق اهتمام. تحكم في ظهور اللون والطبقة الفوقية عند العرض:</p>

<div class="codeblock" id="code">
 <h3>عرض مع وبدون طبقات فوقية - C#</h3>
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
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="TIFF مقابل PNG مقابل JPG للصور الطبية">}}

<p>كل تنسيق إخراج يخدم حالات استخدام مختلفة:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>الخاصية</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>الضغط</strong></td><td>بدون فقدان (LZW, ZIP) أو غير مضغوط</td><td>بدون فقدان</td><td>مع فقدان</td></tr>
<tr><td><strong>متعدد الصفحات</strong></td><td>مدعوم &mdash; جميع الإطارات في ملف واحد</td><td>غير مدعوم</td><td>غير مدعوم</td></tr>
<tr><td><strong>جودة الصورة</strong></td><td>دقة بكسل مثالية</td><td>دقة بكسل مثالية</td><td>من الممكن حدوث تشوهات ضغط</td></tr>
<tr><td><strong>حجم الملف</strong></td><td>الأكبر</td><td>متوسط</td><td>الأصغر</td></tr>
<tr><td><strong>الشفافية</strong></td><td>مدعوم</td><td>مدعوم</td><td>غير مدعوم</td></tr>
<tr><td><strong>الأفضل لـ</strong></td><td>الأرشفة، سلاسل متعددة الإطارات، الطباعة، البحث</td><td>مراجعة تشخيصية، الويب بجودة</td><td>مشاركة على الويب، الصور المصغرة</td></tr>
</tbody>
</table>

<p>يستخدم Aspose.Medical for .NET نفس خط أنابيب العرض لجميع أهداف الإخراج. بيانات بكسل <code>IRawImage</code> هي نفسها بغض النظر عن تنسيق الهدف &mdash; اختيار التنسيق يؤثر فقط على خطوة الحفظ النهائية التي تقوم بها مكتبة التصوير الخاصة بك. TIFF هو التنسيق المفضل عندما تحتاج إلى الحفاظ على دراسة DICOM متعددة الإطارات بالكامل في ملف واحد أو عندما يكون الإخراج مخصصًا للأرشفة، الطباعة، أو المعالجة الإضافية للصور.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="التوثيق" href="https://docs.aspose.com/medical/net/" >}}
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
