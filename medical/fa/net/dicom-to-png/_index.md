---
title: تبدیل DICOM به PNG در C# .NET | Aspose.Medical
weight: 11000
description: نمایش تصاویر DICOM به داده پیکسل در C# .NET با کیفیت بدون افت، کنترل window/level و پشتیبانی از چند فریم. از خط لوله کامل DICOM LUT با API Aspose.Medical استفاده کنید و به PNG ذخیره نمایید.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تبدیل DICOM به PNG در .NET C#" h2="نمایش تصاویر DICOM به داده پیکسل خام با پشتیبانی کامل از خط لوله grayscale. حفظ کیفیت تصویر با کنترل window/level، رندر overlay و پردازش چند فریم. ذخیره به PNG بدون فشرده‌سازی با استفاده از هر کتابخانه تصویری .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="تبدیل DICOM به PNG">}}

<p><strong>Aspose.Medical for .NET</strong> تصاویر DICOM را از طریق خط لوله استاندارد DICOM grayscale (Modality LUT، VOI LUT، Presentation LUT، Output LUT) رندر می‌کند تا داده پیکسل BGRA32 با تنظیمات صحیح windowed تولید شود. PNG یک قالب بدون فشرده‌سازی است که آن را به گزینهٔ برتر تبدیل می‌کند وقتی که باید کیفیت تصویر بدون ایجاد artefacts فشرده‌سازی حفظ شود &mdash; که برای مرور تشخیصی، بایگانی و موارد پژوهشی ضروری است.</p>

<p>خروجی رندر شده یک <code>IRawImage</code> &mdash; بافر پیکسل BGRA32 با <code>Width</code>، <code>Height</code> و آرایهٔ <code>Pixels</code> است. می‌توانید این داده پیکسل را با استفاده از هر کتابخانه تصویری .NET مانند <code>System.Drawing</code>، <code>SkiaSharp</code> یا <code>ImageSharp</code> به PNG ذخیره کنید.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="رندر DICOM به داده پیکسل در C#">}}

<p>یک فایل DICOM را بارگذاری کنید، فریم موردنظر را رندر کنید و به داده پیکسل دسترسی پیدا کنید:</p>

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

<p>متد <code>RenderImage</code> به‌طور خودکار مقادیر window/level و پارامترهای LUT ذخیره شده در مجموعه داده DICOM را اعمال می‌کند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ذخیره تصویر رندر شده به PNG">}}

<p><code>IRawImage</code> داده پیکسل خام BGRA32 را فراهم می‌کند که می‌توان با استفاده از هر کتابخانه تصویری .NET به PNG بدون فشرده‌سازی ذخیره کرد. در اینجا یک مثال با استفاده از <code>System.Drawing</code> آورده شده است:</p>

<div class="codeblock" id="code">
 <h3>ذخیره فریم رندر شده DICOM به PNG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Window/Level سفارشی">}}

<p>با تنظیم Window Center و Window Width می‌توانید محدودهٔ مقادیر پیکسلی که رندر می‌شود را کنترل کنید. تنظیمات مختلف window بخش‌های مختلف آناتومیک را از همان داده‌های DICOM نشان می‌دهد:</p>

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
IRawImage boneImage = dicomFile.RenderImage(bone, 0);</code></pre>
</div>

<p>کلاس <code>GrayscaleRenderOptions</code> همچنین ویژگی‌های <code>RescaleSlope</code>، <code>RescaleIntercept</code>، <code>Invert</code> و <code>BitDepth</code> را برای کنترل کامل روی خط لوله رندر ارائه می‌دهد.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="رندر DICOM چند فریم">}}

<p>فایل‌های DICOM از سی‌تی، ام‌آرآی و سونوگرافی اغلب شامل فریم‌های متعدد (برش‌ها) هستند. همه فریم‌ها را به داده پیکسل رندر کنید:</p>

<div class="codeblock" id="code">
 <h3>رندر تمام فریم‌ها از DICOM چند فریم - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="کنترل Overlay">}}

<p>تصاویر DICOM ممکن است شامل لایه‌های overlay با حاشیه‌نویسی، اندازه‌گیری یا نواحی مورد علاقه باشند. هنگام رندر، قابل مشاهده بودن و رنگ overlay را کنترل کنید:</p>

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
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="PNG در مقابل JPG برای تصاویر پزشکی">}}

<p>انتخاب بین PNG و JPG به مورد استفاده بستگی دارد:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>ویژگی</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>فشرده‌سازی</strong></td><td>بدون فقدان</td><td>با فقدان</td></tr>
<tr><td><strong>کیفیت تصویر</strong></td><td>Pixel-perfect &mdash; بدون artefacts</td><td>امکان وجود artefacts فشرده‌سازی جزئی</td></tr>
<tr><td><strong>حجم فایل</strong></td><td>بزرگ‌تر (معمولاً ۲&ndash;۱۰ برابر نسبت به JPG)</td><td>کوچک‌تر</td></tr>
<tr><td><strong>شفافیت</strong></td><td>پشتیبانی شده (alpha channel)</td><td>پشتیبانی نشده</td></tr>
<tr><td><strong>بهترین برای</strong></td><td>بررسی تشخیصی، بایگانی، تحقیق، overlays</td><td>به اشتراک‌گذاری وب، تصاویر بندانگشتی، پیش‌نمایش‌ها</td></tr>
<tr><td><strong>قانونی</strong></td><td>ترجیح داده شده برای جریان‌های کاری حساس به کیفیت</td><td>قابل قبول برای استفاده غیرتشخیصی</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET از همان خط لوله رندر برای هر دو هدف خروجی استفاده می‌کند. داده پیکسل <code>IRawImage</code> بدون توجه به قالب هدف یکسان است &mdash; انتخاب قالب فقط بر مرحلهٔ نهایی ذخیره‌سازی که توسط کتابخانه تصویری شما انجام می‌شود، تأثیر می‌گذارد.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="منابع آموزشی" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="مستندات" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="کد منبع" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="مراجع API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="پشتیبانی محصول" tabId="support" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی رایگان" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی پولی" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="وبلاگ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="چرا Aspose.Medical برای .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="فهرست مشتریان" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="داستان‌های موفقیت" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
