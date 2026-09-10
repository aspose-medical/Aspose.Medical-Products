---
title: فشرده‌سازی DICOM JPEG 2000 در C# .NET | Aspose.Medical
weight: 2000
description: خواندن، نوشتن و تبدیل فرمت (ترانس‌کد) فایل‌های DICOM با فشرده‌سازی JPEG 2000 در C# .NET. پشتیبانی از تصاویر 8‑bit رنگی و 16‑bit تک‌رنگ، حالت‌های lossless و lossy، به‌علاوه HTJ2K با API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="پشتیبانی DICOM JPEG 2000 در .NET C#" h2="خواندن، نوشتن و تبدیل فرمت فایل‌های DICOM با فشرده‌سازی JPEG 2000. حالت‌های lossless و lossy، داده‌های پیکسل 8‑bit رنگی و 16‑bit تک‌رنگ، HTJ2K گنجانده‌شده – همه در .NET خالص." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 در تصویربرداری پزشکی">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) پرکاربردترین استاندارد فشرده‌سازی مبتنی بر موجک در تصویربرداری پزشکی است. برخلاف JPEG سنتی، هر دو حالت lossless و lossy را در یک کدک فراهم می‌کند، رمزگشایی تدریجی برای دسترسی به ناحیه مورد علاقه (region‑of‑interest) و نسبت‌های فشرده‌سازی برتر &mdash; که آن را برای بایگانی مطالعات بزرگ و انتقال تصاویر بر روی شبکه‌های محدود ایده‌آل می‌سازد.</p>

<p><strong>Aspose.Medical for .NET</strong> یک پیاده‌سازی خالص C# از کدک JPEG 2000 بدون وابستگی به کتابخانه‌های بومی فراهم می‌کند. این کتابخانه قادر به خواندن، رندر و ترانسکد فایل‌های DICOM فشرده‌شده با هر یک از چهار سینتکس انتقال استاندارد JPEG 2000 است.</p>

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
<tr><td>JPEG 2000 فقط lossless</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>8‑bit RGB، 16‑bit تک‌رنگ</td><td>16‑bit تک‌رنگ، 8‑bit RGB</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy یا lossless</td><td>8‑bit RGB، 16‑bit تک‌رنگ</td><td>16‑bit تک‌رنگ، 8‑bit RGB</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component فقط lossless</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>پشتیبانی نمی‌شود</td><td>پشتیبانی نمی‌شود</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy یا lossless</td><td>پشتیبانی نمی‌شود</td><td>پشتیبانی نمی‌شود</td></tr>
<tr><td>HTJ2K فقط lossless</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>تک‌رنگ و رنگی</td><td>تک‌رنگ و رنگی</td></tr>
<tr><td>HTJ2K با گزینه‌های RPCL فقط lossless</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>تک‌رنگ و رنگی</td><td>تک‌رنگ و رنگی</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy یا lossless</td><td>تک‌رنگ و رنگی</td><td>تک‌رنگ و رنگی</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="داده‌های پیکسل 8‑Bit و 16‑Bit">}}

<p>تصاویر پزشکی اغلب از 16 بیت برای هر نمونه استفاده می‌کنند تا دامنه دینامیکی کامل مدالیته‌هایی مانند CT (معمولاً 12‑bit ذخیره‌شده در 16‑bit) و MRI را به‌دست آورند. Aspose.Medical هر دو عمق بیت را برای JPEG 2000 پشتیبانی می‌کند:</p>

<ul>
<li><strong>خواندن (decompression)</strong>: فایل‌های تک‌رنگ 16‑bit (CT، MRI، X‑ray) و فایل‌های رنگی سه‑کامپوننت 8‑bit (RGB، YBR_RCT، YBR_ICT). پالت، CMYK، ICC‑profile و جریان‌های کد رنگ زیرنمونه‌گیری‌شده با یک استثنا واضح رد می‌شوند تا تصویر نادرست به‌صورت ساکت تولید نشود.</li>
<li><strong>نوشتن (compression)</strong>: تصاویر تک‌رنگ 16‑bit و RGB 8‑bit. کدگذاری تک‌رنگ 8‑bit و رنگی 16‑bit در دسترس نیست؛ برای این موارد از HTJ2K یا JPEG XL استفاده کنید، هر دو تک‌رنگ و رنگی را در هر عمق بیتی می‌پذیرند.</li>
</ul>

<div class="codeblock" id="code">
 <h3>خواندن و بازرسی DICOM فشرده‌شده با JPEG 2000 - C#</h3>
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
 <h3>فشرده‌سازی DICOM به JPEG 2000 lossless - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>فشرده‌سازی DICOM به JPEG 2000 lossy - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="بازگشایی فایل‌های DICOM JPEG 2000">}}

<p>فایل‌های JPEG 2000 را به یک سینتکس انتقال بدون فشرده‌سازی برای پردازش، تجزیه و تحلیل، یا سازگاری با سیستم‌هایی که از JPEG 2000 پشتیبانی نمی‌کنند، بازگشایی کنید:</p>

<div class="codeblock" id="code">
 <h3>بازگشایی JPEG 2000 به حالت بدون فشرده‌سازی - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>همچنین می‌توانید در یک مرحله، بازگشایی و تبدیل فرمت به سایر فرمت‌های فشرده‌سازی را انجام دهید:</p>

<div class="codeblock" id="code">
 <h3>تبدیل فرمت بین فرمت‌های فشرده‌سازی - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="رندر کردن تصاویر DICOM JPEG 2000">}}

<p>فایل‌های DICOM فشرده‌شده با JPEG 2000 می‌توانند به داده‌های پیکسل برای نمایش یا استخراج رندر شوند، همانند هر سینتکس انتقال دیگری:</p>

<div class="codeblock" id="code">
 <h3>رندر یک فریم فشرده‌شده JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Lossless در مقابل Lossy JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>جنبه</th>
<th>JPEG 2000 lossless</th>
<th>JPEG 2000 lossy</th>
</tr>
</thead>
<tbody>
<tr><td>سینتکس انتقال</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>کیفیت تصویر</td><td>Pixel-perfect &mdash; مشابه اصل</td><td>به‌صورت بصری مشابه، برخی داده‌ها به‌صورت دائم از دست رفته‌اند</td></tr>
<tr><td>نسبت فشرده‌سازی</td><td>معمولاً ۲:۱ تا ۳:۱</td><td>معمولاً ۱۰:۱ تا ۳۰:۱ یا بیشتر</td></tr>
<tr><td>بهترین برای</td><td>آرشیو تشخیصی، سوابق قانونی، خوانش اولیه</td><td>مرور مقدماتی، تله‌مدیسین، انتقال شبکه‌ای</td></tr>
<tr><td>امن برای دورگرد</td><td>بله</td><td>خیر &mdash; رمزنگاری مجدد کیفیت را بیشتر کاهش می‌دهد</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 با توان پردازشی بالا (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) کدگذار حسابی کند JPEG 2000 را با یک کدگذار بلوکی سریع‌تر جایگزین می‌کند. تبدیل موجک، ترتیب پیشرفت و کیفیت یکسان را حفظ می‌کند و چندین برابر سریع‌تر رمزگشایی و رمزگذاری می‌شود. Aspose.Medical تمام سه سینتکس انتقال DICOM HTJ2K را در .NET خالص پیاده‌سازی می‌کند، برای تصاویر تک‌رنگ و رنگی، و بین HTJ2K و هر سینتکس پشتیبانی‌شده دیگر تبدیل می‌گردد:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; فقط lossless</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; lossless با ترتیب پیشرفت RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; lossy یا lossless</li>
</ul>

<div class="codeblock" id="code">
 <h3>تبدیل JPEG 2000 به HTJ2K و برعکس - C#</h3>
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
{{< blocks/products/pf/slr-tab tabTitle="منابع آموزشی" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="مستندات" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="کد منبع" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="مراجعه‌های API" href="https://reference.aspose.com/medical/net/" >}}
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
