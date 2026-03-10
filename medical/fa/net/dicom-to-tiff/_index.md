---
title: تبدیل DICOM به TIFF در C# .NET | Aspose.Medical
weight: 12000
description: تصویرهای DICOM را به داده‌های پیکسل در C# .NET رندر کنید و با پشتیبانی از چند صفحه‌ای برای فایل‌های DICOM چند‑قابلی به TIFF ذخیره کنید. از خط لوله کامل DICOM LUT با API Aspose.Medical استفاده کنید و با هر کتابخانه تصویربرداری .NET به TIFF ذخیره کنید.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تبدیل DICOM به TIFF در .NET C#" h2="تصویرهای DICOM را به داده‌های پیکسل خام با پشتیبانی کامل از خط لوله خاکستری رندر کنید. کیفیت تصویر را با کنترل window/level، رندر overlay و پردازش چند‑قاب حفظ کنید. با هر کتابخانه تصویربرداری .NET به TIFF ذخیره کنید &mdash; از جمله TIFF چند‑صفحه‌ای &mdash;." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="تبدیل DICOM به TIFF">}}

<p><strong>Aspose.Medical for .NET</strong> تصویرهای DICOM را از طریق خط لوله استاندارد خاکستری DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT) رندر می‌کند تا داده‌های پیکسل BGRA32 با پنجره‌بندی صحیح تولید شوند. TIFF فرمت چندکاره‌ای است که به‌طور گسترده در تصویربرداری پزشکی، تحقیقات علمی و انتشارات به‌کار می‌رود زیرا فشرده‌سازی بدون ضرر، عمق بیت بالا و &mdash; مهم‌ترین ویژگی &mdash; <strong>اسناد چند‑صفحه‌ای</strong> را پشتیبانی می‌کند. این ویژگی TIFF را به گزینه طبیعی برای تبدیل فایل‌های DICOM چند‑قاب (CT، MRI، سونوگرافی) به یک فایل خروجی واحد که تمام برش‌ها را حفظ می‌کند، تبدیل می‌نماید.</p>

<p>خروجی رندر شده یک <code>IRawImage</code> &mdash; یک بافر پیکسل BGRA32 با <code>Width</code>، <code>Height</code>، و آرایه <code>Pixels</code> است. می‌توانید این داده‌های پیکسل را با هر کتابخانه تصویربرداری .NET مانند <code>System.Drawing</code>، <code>SkiaSharp</code> یا <code>ImageSharp</code> به TIFF ذخیره کنید.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="رندر DICOM به داده‌های پیکسل در C#">}}

<p>یک فایل DICOM را بارگذاری کنید، فریم دلخواه را رندر کنید و به داده‌های پیکسل دسترسی پیدا کنید:</p>

<div class="codeblock" id="code">
 <h3>رندر تصویر DICOM - C#</h3>
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

<p>متد <code>RenderImage</code> به‌صورت خودکار مقادیر window/level و پارامترهای LUT ذخیره‌شده در دیتاست DICOM را اعمال می‌کند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ذخیره تصویر رندر شده به عنوان TIFF">}}

<p><code>IRawImage</code> داده‌های خام پیکسل BGRA32 را فراهم می‌کند که می‌توان با هر کتابخانه تصویربرداری .NET به TIFF ذخیره کرد. در اینجا یک مثال با استفاده از <code>System.Drawing</code> آمده است:</p>

<div class="codeblock" id="code">
 <h3>ذخیره فریم رندر شده DICOM به عنوان TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF چند‑صفحه‌ای از DICOM چند‑قاب">}}

<p>فایل‌های DICOM چند‑قاب (مانند سری‌های CT یا MRI) شامل چندین برش تصویری در یک فایل هستند. قابلیت چند‑صفحه‌ای TIFF به شما امکان می‌دهد تمام فریم‌ها را در یک فایل خروجی ذخیره کنید و مطالعه کامل را در قالبی که به‌صورت جهانی قابل خواندن است، حفظ نمایید. از کدگذار TIFF در <code>System.Drawing</code> با پارامترهای چند‑قاب استفاده کنید:</p>

<div class="codeblock" id="code">
 <h3>تبدیل DICOM چند‑قاب به TIFF چند‑صفحه‌ای - C#</h3>
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

<p>همچنین می‌توانید هر فریم را به‌عنوان یک فایل TIFF جداگانه ذخیره کنید هنگامی که برش‌های جداگانه مورد نیاز هستند:</p>

<div class="codeblock" id="code">
 <h3>تبدیل هر فریم به یک TIFF جداگانه - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Window/Level سفارشی">}}

<p>محدوده مقادیر پیکسل رندر شده را با تنظیم Window Center و Window Width کنترل کنید. تنظیمات مختلف window ساختارهای آناتومی متفاوتی را از همان داده‌های DICOM نشان می‌دهند:</p>

<div class="codeblock" id="code">
 <h3>رندر با Window/Level سفارشی - C#</h3>
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

<p>کلاس <code>GrayscaleRenderOptions</code> همچنین ویژگی‌های <code>RescaleSlope</code>، <code>RescaleIntercept</code>، <code>Invert</code> و <code>BitDepth</code> را برای کنترل کامل بر خط لوله رندر فراهم می‌کند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="رندر DICOM چند‑قاب">}}

<p>فایل‌های DICOM از CT، MRI و سونوگرافی اغلب شامل چندین فریم (برش) هستند. تمام فریم‌ها را به داده‌های پیکسل رندر کنید:</p>

<div class="codeblock" id="code">
 <h3>رندر تمام فریم‌ها از DICOM چند‑قاب - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="کنترل Overlay">}}

<p>تصویرهای DICOM ممکن است شامل لایه‌های overlay با حاشیه‌نویسی‌ها، اندازه‌گیری‌ها یا نواحی مورد علاقه باشند. هنگام رندر، قابلیت مشاهده و رنگ overlay را کنترل کنید:</p>

<div class="codeblock" id="code">
 <h3>رندر با و بدون overlayها - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF در مقایسه با PNG و JPG برای تصاویر پزشکی">}}

<p>هر فرمت خروجی برای موارد استفاده متفاوتی مناسب است:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>ویژگی</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>فشرده‌سازی</strong></td><td>بدون ضرر (LZW, ZIP) یا بدون فشرده‌سازی</td><td>بدون ضرر</td><td>با ضرر</td></tr>
<tr><td><strong>چند‑صفحه‌ای</strong></td><td>پشتیبانی می‌شود &mdash; تمام فریم‌ها در یک فایل</td><td>پشتیبانی نمی‌شود</td><td>پشتیبانی نمی‌شود</td></tr>
<tr><td><strong>کیفیت تصویر</strong></td><td>دقیق به پیکسل</td><td>دقیق به پیکسل</td><td>امکان وجود اشکالات فشرده‌سازی</td></tr>
<tr><td><strong>اندازه فایل</strong></td><td>بزرگ‌ترین</td><td>متوسط</td><td>کوچک‌ترین</td></tr>
<tr><td><strong>شفافیت</strong></td><td>پشتیبانی می‌شود</td><td>پشتیبانی می‌شود</td><td>پشتیبانی نمی‌شود</td></tr>
<tr><td><strong>بهترین استفاده برای</strong></td><td>آرشیو، سری‌های چند‑قاب، چاپ، تحقیق</td><td>مرور تشخیصی، وب با کیفیت</td><td>به اشتراک‌گذاری وب، تصاویر بندانگشتی</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET از همان خط لوله رندر برای تمام اهداف خروجی استفاده می‌کند. داده‌های پیکسل <code>IRawImage</code> بدون توجه به فرمت هدف یکسان هستند &mdash; انتخاب فرمت تنها بر مرحله نهایی ذخیره‌سازی که توسط کتابخانه تصویربرداری شما انجام می‌شود، تأثیر دارد. TIFF فرمت ترجیحی است هنگامی که نیاز به حفظ یک مطالعه DICOM چند‑قاب کامل در یک فایل دارید یا خروجی برای آرشیو، چاپ یا پردازش بیشتر تصویر منظور شده است.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="منابع آموزشی" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="مستندات" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="کد منبع" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="مستندات API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="پشتیبانی محصول" tabId="support" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی رایگان" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی تجاری" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="وبلاگ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="چرا Aspose.Medical برای .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="فهرست مشتریان" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="داستان‌های موفقیت" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
