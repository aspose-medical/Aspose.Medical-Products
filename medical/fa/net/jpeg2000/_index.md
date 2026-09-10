---
title: فشرده‌سازی DICOM JPEG 2000 در C# .NET | Aspose.Medical
weight: 2000
description: خواندن، نوشتن و تبدیل فرمت فایل‌های DICOM با فشرده‌سازی JPEG 2000 در C# .NET. پشتیبانی از تصاویر ۸ بیتی و ۱۶ بیتی، حالت‌های بدون‌از‑دست‑رفتنی و با‌از‑دست‑رفتنی، داده‌های چند‑مولفه‌ای با Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="پشتیبانی DICOM JPEG 2000 در .NET C#" h2="خواندن، نوشتن و تبدیل فرمت فایل‌های DICOM با فشرده‌سازی JPEG 2000. حالت‌های بدون‌از‑دست‑رفتنی و با‌از‑دست‑رفتنی، داده‌های پیکسل ۸ بیتی و ۱۶ بیتی، تصاویر چند‑مولفه‌ای — همه در .NET خالص." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 در تصویربرداری پزشکی">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) استاندارد فشرده‌سازی مبتنی بر موجک است که به‌طور گسترده‌ای در تصویربرداری پزشکی استفاده می‌شود. برخلاف JPEG سنتی، این استاندارد هم فشرده‌سازی بدون‌از‑دست‑رفتنی و هم با‑از‑دست‑رفتنی را در یک کدک فراهم می‌کند، رمزگشایی پیش‌رونده برای دسترسی به ناحیه مورد علاقه، و نسبت‌های فشرده‌سازی برتر &mdash; که آن را برای بایگانی مطالعات بزرگ و انتقال تصاویر در شبکه‌های محدود ایده‌آل می‌سازد.</p>

<p><strong>Aspose.Medical for .NET</strong> یک پیاده‌سازی خالص C# از کدک JPEG 2000 را بدون وابستگی‌های بومی ارائه می‌دهد. این کتابخانه می‌تواند فایل‌های DICOM فشرده‌شده با هر یک از چهار سینتکس انتقال استاندارد JPEG 2000 را بخواند، رندر کند و تبدیل فرمت (transcode) نماید.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="سینتکس‌های انتقال JPEG 2000 پشتیبانی‌شده">}}

<table class="table table-bordered">
<thead>
<tr>
<th>سینتکس انتقال</th>
<th>UID</th>
<th>حالت</th>
<th>خواندن</th>
<th>نوشتن</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 فقط بدون‌از‑دست‑رفتنی</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>بدون‌از‑دست‑رفتنی</td><td>۸ بیتی و ۱۶ بیتی</td><td>۸ بیتی</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>با‌از‑دست‑رفتنی یا بدون‌از‑دست‑رفتنی</td><td>۸ بیتی و ۱۶ بیتی</td><td>۸ بیتی</td></tr>
<tr><td>JPEG 2000 Part 2 چند‑مولفه‌ای فقط بدون‌از‑دست‑رفتنی</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>بدون‌از‑دست‑رفتنی</td><td>۸ بیتی و ۱۶ بیتی</td><td>۸ بیتی</td></tr>
<tr><td>JPEG 2000 Part 2 چند‑مولفه‌ای</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>با‌از‑دست‑رفتنی یا بدون‌از‑دست‑رفتنی</td><td>۸ بیتی و ۱۶ بیتی</td><td>۸ بیتی</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="داده‌های پیکسل ۸ بیتی و ۱۶ بیتی">}}

<p>تصاویر پزشکی اغلب از ۱۶ بیت در هر نمونه استفاده می‌کنند تا دامنه دینامیکی کامل مدولاسیون‌های مانند CT (معمولاً ۱۲‑بیتی که در ۱۶‑بیت ذخیره می‌شود) و MRI را ضبط کنند. Aspose.Medical هر دو عمق بیت را برای JPEG 2000 پشتیبانی می‌کند:</p>

<ul>
<li><strong>خواندن (باز کردن فشرده‌سازی)</strong>: پشتیبانی کامل از هر دو فایل DICOM فشرده‌شده با JPEG 2000 به صورت ۸ بیتی و ۱۶ بیتی. کتابخانه به‌درستی داده‌های پیکسل را صرف‌نظر از مقادیر Bits Allocated، Bits Stored و High Bit اصلی رمزگشایی می‌کند.</li>
<li><strong>نوشتن (فشرده‌سازی)</strong>: در حال حاضر از تصاویر ۸ بیتی پشتیبانی می‌کند. پشتیبانی از نوشتن ۱۶ بیتی برای نسخه آتی برنامه‌ریزی شده است.</li>
</ul>

<div class="codeblock" id="code">
 <h3>خواندن و بررسی DICOM فشرده‌شده با JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="تبدیل فرمت به JPEG 2000">}}

<p>از متد <code>Transcode</code> برای فشرده‌سازی هر فایل DICOM به JPEG 2000 یا تبدیل بین حالت‌های JPEG 2000 استفاده کنید:</p>

<div class="codeblock" id="code">
 <h3>فشرده‌سازی DICOM به JPEG 2000 بدون‌از‑دست‑رفتنی - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>فشرده‌سازی DICOM به JPEG 2000 با‌از‑دست‑رفتنی - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="بازگشایی فایل‌های DICOM JPEG 2000">}}

<p>فایل‌های JPEG 2000 را به یک سینتکس انتقال بدون فشرده‌سازی برای پردازش، تجزیه و تحلیل یا سازگاری با سیستم‌هایی که از JPEG 2000 پشتیبانی نمی‌کنند، بازگشایی کنید:</p>

<div class="codeblock" id="code">
 <h3>بازگشایی JPEG 2000 به بدون فشرده‌سازی - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>همچنین می‌توانید در یک قدم بازگشایی کرده و به دیگر فرمت‌های فشرده‌سازی تبدیل (transcode) کنید:</p>

<div class="codeblock" id="code">
 <h3>تبدیل بین فرمت‌های فشرده‌سازی - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="رندر کردن تصاویر DICOM JPEG 2000">}}

<p>فایل‌های DICOM فشرده‌شده با JPEG 2000 می‌توانند به داده‌های پیکسل برای نمایش یا استخراج رندر شوند، همانند هر سینتکس انتقال دیگر:</p>

<div class="codeblock" id="code">
 <h3>رندر فریم فشرده‌شده JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 بدون‌از‑دست‑رفتنی در مقابل با‌از‑دست‑رفتنی">}}

<table class="table table-bordered">
<thead>
<tr>
<th>جنبه</th>
<th>JPEG 2000 بدون‌از‑دست‑رفتنی</th>
<th>JPEG 2000 با‌از‑دست‑رفتنی</th>
</tr>
</thead>
<tbody>
<tr><td>سینتکس انتقال</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>کیفیت تصویر</td><td>پیکسل‑پرفکت &mdash; کاملاً مشابه اصل</td><td>از نظر بصری مشابه، برخی داده‌ها دائماً از دست رفته‌اند</td></tr>
<tr><td>نسبت فشرده‌سازی</td><td>معمولاً ۲:۱ تا ۳:۱</td><td>معمولاً ۱۰:۱ تا ۳۰:۱ یا بالاتر</td></tr>
<tr><td>مناسب برای</td><td>آرشیو تشخیصی، اسناد قانونی، خوانش اولیه</td><td>بررسی اولیه، تلمدیسین، انتقال شبکه‌ای</td></tr>
<tr><td>امن برای دور‌گردی</td><td>بله</td><td>خیر &mdash; رمزگذاری مجدد کیفیت را بیشتر کاهش می‌دهد</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 چند‑مولفه‌ای">}}

<p>JPEG 2000 Part 2 (ISO/IEC 15444-2) قابلیت‌های تبدیل چند‑مولفه‌ای را به کدک استاندارد اضافه می‌کند. این ویژگی برای تصاویر رنگی پزشکی و مدولاسیون‌هایی که داده‌های چند‑کاناله تولید می‌کنند، استفاده می‌شود. Aspose.Medical هر دو سینتکس انتقال Part 2 را پشتیبانی می‌کند:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; فشرده‌سازی بدون‌از‑دست‑رفتنی با حذف همبستگی بین اجزاء برای فشرده‌سازی بهینه داده‌های چند‑کاناله.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; فشرده‌سازی با‌از‑دست‑رفتنی یا بدون‌از‑دست‑رفتنی با تبدیل‌های چند‑مولفه‌ای.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 با توان بالا (HTJ2K) —به‌زودی">}}

<p>HTJ2K (ISO/IEC 15444-15) یک توسعه‌نسل بعدی از JPEG 2000 است که برای سرعت رمزگذاری و رمزگشایی به‌طرز چشمگیری سریع‌تر طراحی شده، در حالی که کارایی فشرده‌سازی مشابهی را حفظ می‌کند. انتظار می‌رود این کدک تبدیل به کدک ترجیحی برای جریان‌های کاری تصویربرداری پزشکی زمان واقعی شود.</p>

<p>Aspose.Medical در یک نسخه آینده، پشتیبانی از HTJ2K را اضافه خواهد کرد که شامل سه سینتکس انتقال می‌شود:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; فقط بدون‌از‑دست‑رفتنی</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; بدون‌از‑دست‑رفتنی با ترتیب پیشرفت RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; با‌از‑دست‑رفتنی یا بدون‌از‑دست‑رفتنی</li>
</ul>

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
