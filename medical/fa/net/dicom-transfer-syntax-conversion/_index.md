---
title: تبدیل نحوه انتقال DICOM در C# .NET | Aspose.Medical
weight: 16000
description: تبدیل کدگذاری (Transcode) فایل‌های DICOM بین نحوه‌های انتقال در C# .NET. پشتیبانی از JPEG، JPEG 2000، JPEG-LS، RLE و فرمت‌های بدون فشرده‌سازی با Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تبدیل نحوه انتقال DICOM در .NET C#" h2="تبدیل کدگذاری فایل‌های DICOM بین نحوه‌های انتقال بدون فشرده‌سازی، JPEG، JPEG 2000، JPEG-LS و RLE. کتابخانه خالص .NET بدون وابستگی بومی." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="نحوه انتقال چیست؟">}}

<p>A <strong>Transfer Syntax</strong> تعریف می‌کند که داده‌های DICOM چگونه برای ذخیره‌سازی و انتقال رمزگذاری می‌شوند. این ویژگی سه جنبه کلیدی را مشخص می‌کند: ترتیب بایت‌ها (اندینس)، اینکه نمایش مقادیر (Value Representations) صریح یا ضمنی هستند، و الگوریتم فشرده‌سازی اعمال‌شده بر داده‌های پیکسل. هر فایل DICOM نحوه انتقال خود را در بخش سرآیند File Meta Information اعلام می‌کند.</p>

<p>دستگاه‌های پزشکی مختلف، سرورهای PACS و برنامه‌های مشاهده از مجموعه‌های متفاوتی از نحوه‌های انتقال پشتیبانی می‌کنند. <strong>Aspose.Medical for .NET</strong> متد <code>Transcode</code> را برای تبدیل بین نحوه‌های انتقال فراهم می‌کند، که امکان تعامل‌پذیری، بهینه‌سازی ذخیره‌سازی و سازگاری با ابزارهای پردازش را فراهم می‌سازد &mdash; همه این‌ها در یک کتابخانه خالص .NET بدون وابستگی بومی.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تبدیل (Transcode) یک فایل DICOM در C#">}}

<p>متد <code>DicomFile.Transcode</code> یک فایل DICOM را از نحوه انتقال فعلی به هر نحوه هدف پشتیبانی‌شده‌ای تبدیل می‌کند. این متد یک نمونه جدید از <code>DicomFile</code> برمی‌گرداند — فایل اصلی بدون تغییر می‌ماند:</p>

<div class="codeblock" id="code">
 <h3>تبدیل پایه DICOM - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>همچنین می‌توانید مستقیماً در سطح <code>Dataset</code> تبدیل (transcode) کنید:</p>

<div class="codeblock" id="code">
 <h3>تبدیل یک Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="نحوه‌های انتقال پشتیبانی‌شده">}}

<p>جدول زیر تمام نحوه‌های انتقال استاندارد داده‌های تصویر DICOM و وضعیت پشتیبانی فعلی آن‌ها در Aspose.Medical برای .NET را فهرست می‌کند. تمام کدک‌های پشتیبانی‌شده به‌صورت خالص C# پیاده‌سازی شده‌اند و کاملاً مستقل از پلتفرم هستند.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>نحوه انتقال</th>
<th>UID</th>
<th>نوع</th>
<th>وضعیت</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>بدون فشرده‌سازی</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی‌شده</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی‌شده</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>بدون فشرده‌سازی</td><td>پشتیبانی‌شده</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>پشتیبانی‌شده</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>فشرده‌سازی با افت کیفیت، 8‑bit</td><td>پشتیبانی‌شده</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>فشرده‌سازی با افت کیفیت، 12‑bit</td><td>پشتیبانی‌نشده</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>بدون افت کیفیت</td><td>پشتیبانی‌شده (فقط 8‑bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>بدون افت کیفیت</td><td>پشتیبانی‌شده (فقط 8‑bit)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>بدون افت کیفیت</td><td>پشتیبانی‌شده</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>نزدیک به عدم افت کیفیت</td><td>پشتیبانی‌شده</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>بدون افت کیفیت</td><td>پشتیبانی‌شده (خواندن 8/16‑bit، نوشتن 8‑bit)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>فشرده‌سازی با افت یا بدون افت</td><td>پشتیبانی‌شده (خواندن 8/16‑bit، نوشتن 8‑bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>بدون افت کیفیت</td><td>پشتیبانی‌شده (خواندن 8/16‑bit، نوشتن 8‑bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>فشرده‌سازی با افت یا بدون افت</td><td>پشتیبانی‌شده (خواندن 8/16‑bit، نوشتن 8‑bit)</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>بدون افت کیفیت</td><td>پشتیبانی‌شده</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>بدون افت کیفیت</td><td>به‌زودی</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>بدون افت کیفیت</td><td>به‌زودی</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>فشرده‌سازی با افت یا بدون افت</td><td>به‌زودی</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>بدون افت کیفیت</td><td>به‌زودی</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>بدون افت کیفیت</td><td>به‌زودی</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>فشرده‌سازی با افت یا بدون افت</td><td>به‌زودی</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="سناریوهای رایج تبدیل کدگذاری">}}

<p>جریان‌های کاری مختلف به استراتژی‌های متفاوتی برای تبدیل نیاز دارند. در اینجا رایج‌ترین سناریوها آورده شده است:</p>

<div class="codeblock" id="code">
 <h3>دیکامپرس برای پردازش - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>فشرده‌سازی برای ذخیره‌سازی بایگانی - C#</h3>
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
 <h3>فشرده‌سازی برای انتقال شبکه‌ای - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="بررسی ویژگی‌های نحوه انتقال">}}

<p>کلاس <code>TransferSyntax</code> ویژگی‌هایی را فراهم می‌کند که خصوصیات رمزگذاری را توصیف می‌کنند. از این ویژگی‌ها برای بررسی نحوه انتقال فعلی یک فایل یا انتخاب نحوه هدف مناسب استفاده کنید:</p>

<div class="codeblock" id="code">
 <h3>خواندن ویژگی‌های نحوه انتقال - C#</h3>
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
<th>ویژگی</th>
<th>نوع</th>
<th>توضیح</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>شناسه منحصر به فرد نحوه انتقال</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>اینکه نمایش مقادیر (Value Representations) به صورت صریح رمزگذاری شده‌اند یا خیر</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>اینکه ترتیب بایت‌ها به صورت little endian است یا خیر</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>اینکه داده‌های پیکسل محصور (فشرده) هستند یا خیر</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>اینکه روش فشرده‌سازی با افت کیفیت است یا خیر</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>اینکه نحوه از فشرده‌سازی Deflate استفاده می‌کند یا خیر</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>اینکه آیا این نحوه انتقال توسط استاندارد DICOM منقرض شده است یا خیر</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>شناسه استاندارد ISO روش فشرده‌سازی با افت کیفیت</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="فشرده‌سازی با افت کیفیت در مقابل بدون افت">}}

<p>درک تفاوت بین فشرده‌سازی با افت و بدون افت هنگام تبدیل فایل‌های DICOM اهمیت حیاتی دارد:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>جنبه</th>
<th>بدون افت کیفیت</th>
<th>با افت کیفیت</th>
</tr>
</thead>
<tbody>
<tr><td>کیفیت تصویر</td><td>پیکسل‑دقیق — داده اصلی به‌طور کامل حفظ می‌شود</td><td>برخی داده‌ها به‌صورت دائمی برای دستیابی به اندازهٔ کوچکتر از دست رفته‌اند</td></tr>
<tr><td>نسبت فشرده‌سازی</td><td>عموماً ۲:۱ تا ۳:۱</td><td>عموماً ۱۰:۱ تا ۳۰:۱ یا بیشتر</td></tr>
<tr><td>قابل استفاده در مسیر دوطرفه</td><td>بله — دیکامپرس کرده و پیکسل‌های یکسان به دست می‌آید</td><td>خیر — هر بار کدگذاری مجدد با افت، کیفیت بیشتر کاهش می‌یابد</td></tr>
<tr><td>موارد استفاده</td><td>بایگانی، تشخیص، سوابق قانونی</td><td>بررسی اولیه، تله‌پزشکی، انتقال شبکه‌ای</td></tr>
<tr><td>کدک‌های پشتیبانی‌شده</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>مهم:</strong> تبدیل یک فایل فشرده‌شده با افت کیفیت به یک نحوه بدون افت، داده‌های از دست رفته را بازیابی نمی‌کند. کاهش کیفیت ناشی از فشرده‌سازی اولیهٔ با افت کیفیت، دائمی است.</p>

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
