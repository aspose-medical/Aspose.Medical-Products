---
title: تبدیل DICOM به JPG در C# .NET | Aspose.Medical
weight: 7000
description: تصویرهای DICOM را به داده‌های پیکسل در C# .NET رندر کنید با کنترل window/level، رندر پوشش (overlay) و پشتیبانی از چند فریم. از لوله‌کشی کامل DICOM LUT با Aspose.Medical API استفاده کنید و به JPG ذخیره نمایید.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تبدیل DICOM به JPG در .NET C#" h2="تصویرهای DICOM را به داده‌های پیکسل خام با پشتیبانی کامل از لوله‌کشی grayscale شامل window/level، modality LUT، VOI LUT و رندر پوشش (overlay) رندر کنید. با استفاده از هر کتابخانه تصویری .NET به JPG ذخیره نمایید." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="لوله‌کشی رندر تصویر DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> تصاویر DICOM را از طریق لوله‌کشی استاندارد DICOM grayscale که در مشخصات DICOM تعریف شده است، رندر می‌کند. فرآیند رندر زنجیره‌ای از جداول Look-Up Tables (LUTs) را برای تبدیل مقادیر پیکسل ذخیره‌شده خام به تصاویر قابل نمایش اعمال می‌کند:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; مقادیر پیکسل ذخیره‌شده را به مقادیر مستقل از سازنده تبدیل می‌کند با استفاده از Rescale Slope/Intercept یا یک LUT Sequence.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; با اعمال window/level (Window Center و Window Width) بازه مقادیری که باید نمایش داده شوند را انتخاب می‌کند و از تبدیلات Linear، Linear Exact و Sigmoid پشتیبانی می‌کند.</li>
<li><strong>Presentation LUT</strong> &mdash; مقادیر نهایی را به P-Values قابل نمایش نگاشت می‌کند، به‌هم‌راه پشتیبانی از INVERSE (وارون کردن تصویر).</li>
<li><strong>Output LUT</strong> &mdash; مقادیر grayscale را به RGB برای نمایش تبدیل می‌کند، با امکان رنگ‌آمیزی اختیاری با استفاده از نقشه‌های رنگ سفارشی یا جداول Palette Color.</li>
</ol>

<p>خروجی رندر شده یک <code>IRawImage</code> &mdash; بافر پیکسل BGRA32 (۸ بیت برای هر کانال) با <code>Width</code>، <code>Height</code> و آرایه <code>Pixels</code> است. سپس می‌توانید این داده‌های پیکسل را به JPG با استفاده از هر کتابخانه تصویری .NET مانند <code>System.Drawing</code>، <code>SkiaSharp</code> یا <code>ImageSharp</code> ذخیره کنید.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="رندر DICOM به داده‌های پیکسل در C#">}}

<p>یک فایل DICOM را بارگذاری کنید و یک فریم را به یک <code>IRawImage</code> حاوی داده‌های پیکسل BGRA32 رندر کنید:</p>

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

<p>متد <code>RenderImage</code> به‌صورت خودکار لوله‌کشی کامل grayscale را با استفاده از مقادیر window/level و پارامترهای LUT ذخیره‌شده در فایل DICOM اعمال می‌کند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ذخیره تصویر رندر شده به صورت JPG">}}

<p><code>IRawImage</code> داده‌های پیکسل خام BGRA32 را فراهم می‌کند که می‌توان با استفاده از هر کتابخانه تصویری .NET به JPG ذخیره کرد. در ادامه یک مثال با استفاده از <code>System.Drawing</code> آورده شده است:</p>

<div class="codeblock" id="code">
 <h3>ذخیره فریم رندر شده DICOM به JPG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Window/Level سفارشی (Windowing)">}}

<p>Window/level (که به‌عنوان windowing یا W/L نیز شناخته می‌شود) مهم‌ترین پارامتر برای رندر تصویر DICOM است. این پارامتر کنترل می‌کند که کدام بازه از مقادیر پیکسل به بازه grayscale قابل مشاهده نگاشته شود. <strong>Window Center</strong> نقطه میانی را تعریف می‌کند و <strong>Window Width</strong> بازه مقادیری که نمایش داده می‌شوند را تعیین می‌کند:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; کنتراست بالا، سطوح خاکستری کمتر نمایش داده می‌شوند (مناسب برای بافت نرم).</li>
<li><strong>Wide window</strong> &mdash; کنتراست پایین‌تر، سطوح خاکستری بیشتری نمایش داده می‌شوند (مناسب برای استخوان یا ریه).</li>
</ul>

<div class="codeblock" id="code">
 <h3>رندر با window/level سفارشی - C#</h3>
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

<p>پیش‌تنظیمات رایج window در CT:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>نوع بافت</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>بافت نرم</td><td>40</td><td>400</td></tr>
<tr><td>ریه</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>استخوان</td><td>400</td><td>1800</td></tr>
<tr><td>مغز</td><td>40</td><td>80</td></tr>
<tr><td>کبد</td><td>60</td><td>150</td></tr>
<tr><td>میان‌قفسه سینه</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="گزینه‌های رندر Grayscale">}}

<p>کلاس <code>GrayscaleRenderOptions</code> کنترل کامل بر لوله‌کشی رندر grayscale را فراهم می‌کند:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>ویژگی</th>
<th>توضیح</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>مرکز بازه مقادیر نمایش داده‌شده (نقطه میانی افقی VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>عرض بازه مقادیر نمایش داده‌شده (کنترل کنتراست)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>شیب مقیاس‌گذاری Modality LUT &mdash; در مقادیر پیکسل ذخیره‌شده ضرب می‌شود</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>قطعه مقیاس‌گذاری Modality LUT &mdash; پس از ضرب شیب به مقدار افزوده می‌شود</td></tr>
<tr><td><code>Invert</code></td><td>تصویر grayscale را وارون می‌کند (شکل INVERSE Presentation LUT را اعمال می‌کند)</td></tr>
<tr><td><code>BitDepth</code></td><td>اطلاعات عمق بیت: BitsAllocated، BitsStored، HighBit، IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>رندر grayscale وارونه - C#</h3>
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

<p>از <code>RenderOptionsFactory.Create(pixelData)</code> استفاده کنید تا گزینه‌های رندر را به‌صورت خودکار از ویژگی‌های داده‌های پیکسل مجموعه‌داده DICOM پر کند، شامل عمق بیت، پارامترهای مقیاس‌گذاری، window/level و تنظیمات وارونگی.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="رندر پوشش (Overlay)">}}

<p>تصاویر DICOM می‌توانند حاوی لایه‌های overlay — گرافیک یا حاشیه‌نویسی‌های متنی که در تصویر سوزانده شده‌اند — باشند. کلاس <code>RenderOptions</code> تعیین می‌کند که آیا overlayها رندر شوند و با چه رنگی نمایش داده شوند:</p>

<div class="codeblock" id="code">
 <h3>رندر با کنترل overlay - C#</h3>
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

<p>DICOM دو نوع overlay را پشتیبانی می‌کند: overlayهای <strong>Graphics</strong> (حاشیه‌نویسی‌ها، اندازه‌گیری‌ها) و overlayهای <strong>Region of Interest</strong> (ROI). هر دو نوع توسط تنظیم <code>ShowOverlays</code> کنترل می‌شوند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="رندر DICOM چند‑فریم">}}

<p>بسیاری از فایل‌های DICOM از CT، MRI و سونوگرافی حاوی فریم‌های متعدد (برش‌ها) هستند. می‌توانید تعداد فریم‌ها را بررسی کنید و هر کدام را به‌صورت جداگانه رندر کنید:</p>

<div class="codeblock" id="code">
 <h3>رندر تمام فریم‌های DICOM چند‑فریم - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="انواع تبدیل VOI LUT">}}

<p>استاندارد DICOM چندین تابع تبدیل VOI LUT را تعریف می‌کند که کنترل می‌کند نحوه اعمال نگاشت window/level چگونه باشد. Aspose.Medical for .NET تمام انواع استاندارد را پشتیبانی می‌کند:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>نوع</th>
<th>توضیح</th>
<th>محیط استفاده</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>نگاشت خطی استاندارد با انحراف از مرکز</td><td>متداول‌ترین، برای تصویربرداری عمومی CT/MR استفاده می‌شود</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>نگاشت خطی بدون انحراف از مرکز</td><td>زمانی که نیاز به نگاشت دقیق مقادیر پیکسل است</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>تبدیل S‑curve برای انتقال‌های کنتراست نرم‌تر</td><td>تصویر‌برداری بافت نرم، ماموگرافی</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>LUT سفارشی تعریف‌شده به‌عنوان جدول lookup در مجموعه‌داده DICOM</td><td>منحنی‌های نمایش مخصوص فروشنده یا مخصوص مودالیتی</td></tr>
</tbody>
</table>

<p>موتور رندر به‌صورت خودکار تبدیل VOI LUT مناسب را بر پایه ویژگی‌های مجموعه‌داده DICOM انتخاب می‌کند. هنگام استفاده از <code>GrayscaleRenderOptions</code> سفارشی، تبدیل Linear به‌طور پیش‌فرض اعمال می‌شود.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="مودالیت‌های پشتیبانی‌شده">}}

<p>Aspose.Medical برای .NET رندر تصویر DICOM را از تمام مودالیت‌های رایج تصویربرداری پزشکی پشتیبانی می‌کند:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>مودالیت</th>
<th>توضیح</th>
<th>فرمت معمولی</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>توموگرافی کامپیوتری</td><td>Grayscale، چند‑فریم، 12–16 بیت</td></tr>
<tr><td><strong>MR</strong></td><td>رزونانس مغناطیسی</td><td>Grayscale، چند‑فریم، 12–16 بیت</td></tr>
<tr><td><strong>CR / DX</strong></td><td>رادیولوژی کامپیوتری / دیجیتال</td><td>Grayscale، تک‑فریم، 10–16 بیت</td></tr>
<tr><td><strong>US</strong></td><td>سونوگرافی</td><td>Grayscale یا RGB، چند‑فریم</td></tr>
<tr><td><strong>MG</strong></td><td>ماموگرافی</td><td>Grayscale، وضوح بالا، 12–16 بیت</td></tr>
<tr><td><strong>XA</strong></td><td>آنجیوگرافی رادیولوژی</td><td>Grayscale، چند‑فریم</td></tr>
<tr><td><strong>NM</strong></td><td>پزشکی هسته‌ای</td><td>Grayscale، چند‑فریم</td></tr>
<tr><td><strong>PT</strong></td><td>PET (پوزیترون ایمیشن توموگرافی)</td><td>Grayscale، چند‑فریم</td></tr>
</tbody>
</table>

<p>هر دو تفسیر فوتومتریک grayscale (MONOCHROME1/MONOCHROME2) و رنگی (RGB، YBR_FULL، PALETTE COLOR) پشتیبانی می‌شوند. موتور رندر به‌صورت خودکار تبدیل فضای رنگی مناسب را انجام می‌دهد.</p>

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
{{< blocks/products/pf/slr-element name="پشتیبانی پولی" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="وبلاگ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="چرا Aspose.Medical برای .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="لیست مشتریان" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="داستان‌های موفقیت" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
