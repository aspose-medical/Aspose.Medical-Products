---
title: HTJ2K در C# .NET - High-Throughput JPEG 2000 برای DICOM | Aspose.Medical
weight: 10000

description: فشرده‌سازی و خواندن تصاویر DICOM در High-Throughput JPEG 2000 از C#. Lossless HTJ2K، نسخه RPCL و lossy HTJ2K، پیاده‌سازی‌شده در .NET مدیریت‌شده بدون نیاز به کدک بومی برای استقرار.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K در .NET C#" h2="High-Throughput JPEG 2000 برای DICOM: فشرده‌سازی‌ای که استاندارد برای آرشیوهای سریع و مشاهده در ابر اضافه کرده است، پیاده‌سازی‌شده در C# مدیریت‌شده بدون نیاز به نصب کدک بومی." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="تغییرات HTJ2K">}}

<p>High-Throughput JPEG 2000 موج‌کشی و کیفیت تصویر JPEG 2000 را حفظ می‌کند و بخشی که باعث کندی آن می‌شده را جایگزین می‌نماید. کدگذار بلوکی جدید است و رمزگشایی به‌مراتب سریع‌تر است، به همین دلیل استاندارد DICOM آن را در سه سینتکس انتقال پذیرفته و پلتفرم‌های تصویربرداری ابری به آن مهاجرت کرده‌اند.</p>

<p>برای یک تیم .NET سؤال عملی متفاوت است: چه کسی می‌تواند واقعاً این فایل‌ها را تولید کند. اکثر کتابخانه‌ها به HTJ2K از طریق یک بیلد بومی OpenJPH می‌رسند که به معنی یک باینری برای هر پلتفرم، یک مرحله ساخت در کانتینر و وابستگی‌ای است که بررسی امنیتی درباره آن سؤال می‌کند. <strong>Aspose.Medical for .NET</strong> کدک را در کد مدیریت‌شده داخل همان بسته‌ای که فایل‌ها را می‌خواند و می‌نویسد پیاده‌سازی می‌کند، بنابراین HTJ2K به‌صورت یکسان بر ویندوز، لینوکس و در یک کانتینر کار می‌کند، بدون نیاز به نصب چیزی.</p>

<p>سه سینتکس انتقال پشتیبانی می‌شوند و هر سه قابلیت خواندن و نوشتن را دارند:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201)، High-Throughput JPEG 2000 lossless.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202)، نسخه lossless با ترتیب پیشرفت RPCL.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203)، High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="فشرده‌سازی یک مطالعه به HTJ2K">}}

<p>یک فراخوانی فایل را به سینتکس جدید منتقل می‌کند. دیتاست، تگ‌های خصوصی و اطلاعات متادیتای فایل همراه آن حرکت می‌کنند.</p>

<div class="codeblock" id="code">
 <h3>تبدیل فرمت یک فایل DICOM به HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>در یک تصویر 16 بیتی با ابعاد 1714×1933 از مجموعه تست خودمان، حجم فایل از 6.3 مگابایت به 2.9 مگابایت کاهش می‌یابد و پیکسل‌ها دقیقاً همانند قبل باز می‌گردند. مقادیر بسته به نوع مدالیته و تصویر متفاوت است، بنابراین بر روی داده‌های خودتان اندازه‌گیری کنید؛ این کار یک دور بر روی فایل‌های موجود شماست.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossless به معنای بدون افت است">}}

<p>داده‌های تشخیصی تحمل کدکی که تقریباً درست باشد را ندارند. تبدیل به HTJ2K lossless و بازگرداندن آن باعث می‌شود داده‌های پیکسل دقیقاً همان بایت‌هایی باشند که در ابتدا داشتید، که این ویژگی را می‌توانید در مجموعه تست خود قبل از توافق برای فشرده‌سازی مجدد یک آرشیو تأیید کنید.</p>

<div class="codeblock" id="code">
 <h3>بازگشت به یک سینتکس غیر فشرده - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL، نسخه‌ای ساخته شده برای مشاهده از طریق شبکه">}}

<p>سینتکس 1.2.840.10008.1.2.4.202 همان جریان کدگذاری lossless را در ترتیب پیشرفت RPCL ذخیره می‌کند: ابتدا وضوح، سپس موقعیت، سپس مؤلفه، سپس لایه. خواننده‌ای که فقط ابتدای جریان را می‌گیرد، تصویر با وضوح پایین کامل را دریافت می‌کند، که دقیقاً همان چیزی است که یک نمایشگر هنگام باز کردن یک مطالعه بزرگ از طریق لینکی که تحت کنترل آن نیست، نیاز دارد.</p>

<div class="codeblock" id="code">
 <h3>فشرده‌سازی با ترتیب پیشرفت RPCL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="خواندن آنچه یک آرشیو برای شما می‌فرستد">}}

<p>نیمه دیگر کار پذیرش HTJ2K از سیستم‌هایی است که قبلاً آن را تولید می‌کنند. فایل را باز کنید، بررسی کنید به چه شکلی ذخیره شده است و با داده‌های پیکسل کار کنید.</p>

<div class="codeblock" id="code">
 <h3>خواندن یک فایل HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>تصاویر چند فریمی به صورت فریم به فریم پردازش می‌شوند، بنابراین یک سری طولانی حافظه را به ازای هر فریم مصرف می‌کند نه به ازای کل مطالعه.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="محل‌ای که HTJ2K در آن ارزش خود را به دست می‌آورد">}}

<ul>
<li>مهاجرت آرشیو: فشرده‌سازی مجدد یک مطالعه ذخیره‌شده به HTJ2K lossless، کاهش حجم، حفظ داده‌های تشخیصی به صورت دست‌نخورده.</li>
<li>ابر و DICOMweb: سرعت رمزگشایی همان چیزی است که باعث می‌شود یک نمایشگر سمت مرورگر یا سمت سرور بر روی تصاویر بزرگ به‌صورت آنی حس شود.</li>
<li>خطوط لوله AI: مجموعه‌های آموزشی بیشتر خوانده می‌شوند تا نوشته شوند، و زمان رمزگشایی هزینه‌ای است که مکرراً تکرار می‌شود.</li>
<li>کانتینرها و سرورلس: کدک بخشی از اسمبلی است، بنابراین یک تصویر نیازی به کتابخانه بومی یا کامپایلر در فرآیند ساخت ندارد.</li>
</ul>

<p>این کتابخانه همچنین JPEG XL را که افزودنی دیگری به استاندارد است، همراه با کدک‌های قدیمی‌تری که احتمالاً یک آرشیو داشته باشد: JPEG، JPEG‑LS، JPEG 2000 و RLE، ارائه می‌دهد. صفحه <a href="/medical/net/dicom-transfer-syntax-conversion/">تبدیل سینتکس انتقال</a> تمام مجموعه را پوشش می‌دهد و صفحه <a href="/medical/net/jpeg2000/">JPEG 2000</a> به کدکی که HTJ2K از آن توسعه یافته است، می‌پردازد.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="منابع آموزشی" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="مستندات" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="راهنمای توسعه‌دهنده" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="مرجع‌های API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="پشتیبانی محصول" tabId="support" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی رایگان" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی پرداختی" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="وبلاگ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="چرا Aspose.Medical برای .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="فهرست مشتریان" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="داستان‌های موفقیت" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
