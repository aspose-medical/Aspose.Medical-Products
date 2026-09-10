---
title: تبدیل سینتکس انتقال DICOM در C# .NET | Aspose.Medical
weight: 16000
description: تبدیل (Transcode) فایل‌های DICOM بین سینتکس‌های انتقال در C# .NET. پشتیبانی از JPEG، JPEG 2000، HTJ2K، JPEG XL، JPEG‑LS، RLE و فرمت‌های بدون فشرده‌سازی با Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تبدیل سینتکس انتقال DICOM در .NET C#" h2="تبدیل فایل‌های DICOM بین سینتکس‌های انتقال بدون فشرده‌سازی، JPEG، JPEG 2000، HTJ2K، JPEG XL، JPEG‑LS و RLE. کتابخانهٔ خالص .NET بدون وابستگی‌های بومی." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="سینتکس انتقال چیست؟">}}

<p>یک <strong>سینتکس انتقال</strong> نحوهٔ رمزگذاری داده‌های DICOM برای ذخیره‌سازی و انتقال را تعریف می‌کند. این استاندارد سه جنبهٔ کلیدی را مشخص می‌کند: ترتیب بایت (اندیانی)، این که نمایش مقادیر (Value Representations) بصورت صریح یا ضمنی باشد، و الگوریتم فشرده‌سازی اعمال شده بر داده‌های پیکسل. هر فایل DICOM سینتکس انتقال خود را در سرعنوان اطلاعات متا (File Meta Information) اعلام می‌کند.</p>

<p>دستگاه‌های پزشکی، سرورهای PACS و برنامه‌های نمایش مختلف، مجموعه‌های متفاوتی از سینتکس‌های انتقال را پشتیبانی می‌کنند. <strong>Aspose.Medical for .NET</strong> روش <code>Transcode</code> را برای تبدیل بین سینتکس‌های انتقال فراهم می‌کند تا قابلیت همکاری، بهینه‌سازی ذخیره‌سازی و سازگاری با ابزارهای پردازشی را فعال سازد &mdash; همه این‌ها در یک کتابخانهٔ خالص .NET بدون وابستگی‌های بومی.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تبدیل یک فایل DICOM در C#">}}

<p>متد <code>DicomFile.Transcode</code> یک فایل DICOM را از سینتکس انتقال فعلی به هر سینتکس هدف پشتیبانی‌شده‌ای تبدیل می‌کند. این متد یک نمونهٔ جدید از <code>DicomFile</code> برمی‌گرداند — فایل اصلی بدون تغییر می‌ماند:</p>

<div class="codeblock" id="code">
 <h3>تبدیل پایه‌ای DICOM - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>همچنین می‌توانید مستقیماً در سطح <code>Dataset</code> تبدیل انجام دهید:</p>

<div class="codeblock" id="code">
 <h3>تبدیل یک Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="سینتکس‌های انتقال پشتیبانی‌شده">}}

<p>جدول زیر تمام سینتکس‌های استاندارد انتقال داده‌های تصویر DICOM و وضعیت پشتیبانی فعلی آن‌ها در Aspose.Medical for .NET را فهرست می‌کند. تمامی کدک‌های پشتیبانی‌شده به صورت خالص C# پیاده‌سازی شده‌اند و به‌صورت کامل مستقل از بستر هستند.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>سینتکس انتقال</th>
<th>UID</th>
<th>نوع</th>
<th>وضعیت</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>بدون فشرده‌سازی</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی‌شده</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی‌شده</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>بدون فشرده‌سازی (منسوخ)</td><td>پشتیبانی‌شده</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی نشده</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>فشرده (Deflated)</td><td>پشتیبانی‌شده</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>ضرری، ۸ بیتی</td><td>پشتیبانی‌شده</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>ضرری، ۱۲ بیتی</td><td>پشتیبانی نشده</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی‌شده (فقط ۸ بیتی)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی‌شده (فقط ۸ بیتی)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی‌شده</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>نزدیک به بدون فشرده‌سازی</td><td>پشتیبانی‌شده</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی‌شده (خواندن رنگ ۸ بیتی و تک‌رنگ ۱۶ بیتی؛ نوشتن تک‌رنگ ۱۶ بیتی یا RGB ۸ بیتی)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>ضرری یا بدون فشرده‌سازی</td><td>پشتیبانی‌شده (خواندن رنگ ۸ بیتی و تک‌رنگ ۱۶ بیتی؛ نوشتن تک‌رنگ ۱۶ بیتی یا RGB ۸ بیتی)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی نشده</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>ضرری یا بدون فشرده‌سازی</td><td>پشتیبانی نشده</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی‌شده</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی‌شده</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی‌شده</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>ضرری یا بدون فشرده‌سازی</td><td>پشتیبانی‌شده</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی‌شده</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>بدون فشرده‌سازی</td><td>فقط رمزگشایی (رمزگذاری به جریان منبع JPEG نیاز دارد، نه داده‌های پیکسل)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>ضرری یا بدون فشرده‌سازی</td><td>پشتیبانی‌شده (حالت ضرری)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="سناریوهای رایج تبدیل">}}

<p>جریان‌های کاری مختلف به استراتژی‌های متفاوتی برای تبدیل نیاز دارند. در اینجا رایج‌ترین سناریوها آورده شده است:</p>

<div class="codeblock" id="code">
 <h3>قالب‌زدایی برای پردازش - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>فشرده‌سازی برای ذخیره‌سازی آرشیوی - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>فشرده‌سازی برای انتقال شبکه - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>استفاده از جدیدترین کدک‌ها: HTJ2K و JPEG XL - C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="بررسی خصوصیات سینتکس انتقال">}}

<p>کلاس <code>TransferSyntax</code> ویژگی‌هایی را ارائه می‌دهد که خصوصیات رمزگذاری را توصیف می‌کنند. از این ویژگی‌ها برای بررسی سینتکس انتقال فعلی یک فایل یا انتخاب یک سینتکس هدف مناسب استفاده کنید:</p>

<div class="codeblock" id="code">
 <h3>خواندن خصوصیات سینتکس انتقال - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>خصوصیت</th>
<th>نوع</th>
<th>توضیح</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>شناسهٔ یکتا برای سینتکس انتقال</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>اینکه Value Representations به‌صورت صریح رمزگذاری شده‌اند یا نه</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>اینکه ترتیب بایت به‌صورت little endian باشد</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>اینکه داده‌های پیکسل محاط (فشرده) باشند یا نه</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>اینکه روش فشرده‌سازی ضرری باشد</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>اینکه سینتکس از فشرده‌سازی deflate استفاده می‌کند یا نه</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>اینکه سینتکس انتقال توسط استاندارد DICOM منسوخ شده باشد</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>شناسهٔ استاندارد ISO برای روش فشرده‌سازی ضرری</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="فشرده‌سازی ضرری در مقابل بدون فشرده‌سازی">}}

<p>درک تفاوت بین فشرده‌سازی ضرری و بدون فشرده‌سازی هنگام تبدیل فایل‌های DICOM حیاتی است:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>جنبه</th>
<th>بدون فشرده‌سازی</th>
<th>ضرری</th>
</tr>
</thead>
<tbody>
<tr><td>کیفیت تصویر</td><td>دقیقاً پیکسل به پیکسل — دادهٔ اصلی به‌صورت کامل حفظ می‌شود</td><td>بعضی داده‌ها به‌صورت دائم از دست می‌روند تا حجم کمتر به‌دست آید</td></tr>
<tr><td>نسبت فشرده‌سازی</td><td>معمولاً ۲:۱ تا ۳:۱</td><td>معمولاً ۱۰:۱ تا ۳۰:۱ یا بیشتر</td></tr>
<tr><td>ایمن برای رفت‑آمد</td><td>بله — بازگشایی و دریافت پیکسل‌های یکسان</td><td>خیر — هر بار باز‑رمزگذاری ضرری کیفیت را بیشتر کاهش می‌دهد</td></tr>
<tr><td>موارد استفاده</td><td>آرشیوی، تشخیصی، سوابق قانونی</td><td>بررسی مقدماتی، تل‌پزشکی، انتقال شبکه‌ای</td></tr>
<tr><td>کدک‌های پشتیبانی‌شده</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>Important:</strong> تبدیل یک فایل فشرده‌سازی ضرری به سینتکس بدون فشرده‌سازی، داده‌های از دست رفته را بازنشانی نمی‌کند. کاهش کیفیت ناشی از فشرده‌سازی ضرری اولیه دائمی است.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="منابع آموزشی" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="مستندات" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="کد منبع" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="مرجع API" href="https://reference.aspose.com/medical/net/" >}}
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
