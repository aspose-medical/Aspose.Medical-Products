---
title: کار با پرونده‌های بزرگ DICOM در C# .NET | Aspose.Medical
weight: 11500

description: باز کردن مطالعات چند‑قابلمه و تصاویر اسلاید کامل در C# بدون بارگذاری آن‌ها در حافظه. خواندن فراداده‌ها بدون داده‌های پیکسلی، به تعویق انداختن عناصر بزرگ، و انتقال پرونده‌ها از طریق استریم‌ها و لوله‌ها.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="پرونده‌های بزرگ DICOM در .NET C#" h2="خواندن فراداده‌های یک مطالعه چند‑قابلمه بدون پیکسل‌ها، به تعویق انداختن عناصر بزرگ تا زمانی که درخواست شوند، و انتقال کل پرونده‌ها از طریق استریم‌ها و لوله‌ها." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="پرونده بزرگ است، سؤال معمولاً کوچک است">}}

<p>یک تصویر اسلاید کامل، یک سری طولانی CT یا یک حجم OCT صدها مگابایت است و بیشتر آن داده‌های پیکسلی هستند. کاری که یک برنامه در واقع انجام می‌دهد اغلب بسیار کوچک‌تر است: فهرست کردن محتویات یک پوشه، بررسی شناسه بیمار، شمارش فریم‌ها، تصمیم‌گیری درباره محل قرارگیری مطالعه. بارگذاری هر بایت برای پاسخ به این موارد همان چیزی است که یک کار ساده را به یک مشکل حافظه تبدیل می‌کند.</p>

<p><strong>Aspose.Medical for .NET</strong> به فراخواننده اجازه می‌دهد تعیین کند چه مقدار از یک پرونده خوانده شود. این گزینه یک آرگومان در <code>DicomFile.Open</code> است و برای پرونده‌ها، استریم‌ها و لوله‌ها به طور یکسان اعمال می‌شود.</p>

<p>اندازه‌گیری بر روی یک مطالعه ۱۴ مگابایتی با ۱۲۸ فریم از مجموعه تست ما، بر روی همان ماشین و همان پرونده انجام شد:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>استراتژی خواندن</th>
<th>زمان باز کردن</th>
<th>حافظه تخصیص داده شده</th>
</tr>
</thead>
<tbody>
<tr><td>همه چیز، پیش‌فرض</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>عناصر بزرگ حذف شدند</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>عناصر بزرگ به تعویق افتادند</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>تفاوت با بزرگ‌تر شدن پرونده افزایش می‌یابد. یک پوشه شامل ۱۰,۰۰۰ مطالعه موردی است که دیگر به عنوان بهینه‌سازی کوچک محسوب نمی‌شود.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="خواندن فراداده، بدون دست زدن به پیکسل‌ها">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> تمام عناصری که بالاتر از آستانه اندازه هستند را از خواندن حذف می‌کند. دیتاست بازگردانده شده شامل برچسب‌هایی است که یک ایندکس یا مسیر‌دهنده به آن‌ها نیاز دارد.</p>

<div class="codeblock" id="code">
 <h3>خواندن یک مطالعه بدون داده‌های پیکسلی - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>آستانه به‌صورت پیش‌فرض ۶۴ کیلوبایت است و مقداری بر حسب کیلوبایت می‌گیرد، بنابراین یک فرآیند کاری که ۸ کیلوبایت را بزرگ می‌داند می‌تواند این مقدار را تنظیم کند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="به‌جای حذف، به‌تعویق انداختن">}}

<p>زمانی که ممکن است پیکسل‌ها مورد نیاز باشند، اما احتمالاً بعداً و احتمالاً همه آن‌ها نیستند، <code>ReadLargeOnDemand</code> نیمه دیگر این جفت است. باز کردن پرونده هزینه‌ای مشابه حذف دارد و یک عنصر بزرگ در همان لحظه‌ای که کد به آن دسترسی پیدا می‌کند خوانده می‌شود.</p>

<div class="codeblock" id="code">
 <h3>بارگذاری یک فریم فقط هنگام استفاده - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>خواندن به‌تعویق یک ویژگی دارای لایسنس است؛ سایر استراتژی‌ها نیز در حالت ارزیابی کار می‌کنند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ایندکس کردن یک پوشه بدون دست زدن به پیکسل‌ها">}}

<p>همین استراتژی بر روی یک استریم نیز اعمال می‌شود، که همان چیزی است که اسکن آرشیو یا ذخیره‌سازی اشیاء ابری از دید کد ظاهر می‌شود.</p>

<div class="codeblock" id="code">
 <h3>اسکن یک آرشیو - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="استریم‌ها و لوله‌ها، ورودی و خروجی">}}

<p>خواندن و نوشتن هر دو از استریم‌ها پشتیبانی می‌کنند و نقاط ورودی ناهمزمان نیز انواع <code>System.IO.Pipelines</code> را می‌پذیرند. یک مطالعه می‌تواند از پاسخ شبکه به ذخیره‌سازی حرکت کند بدون این‌که فرآیند کل پرونده را به‌صورت یک آرایه در حافظه نگه دارد.</p>

<div class="codeblock" id="code">
 <h3>خواندن و نوشتن از طریق استریم‌ها - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>همین ایده بر روی نمایش‌های متنی نیز اعمال می‌شود: یک سند شامل دیتاست‌های متعدد به‌صورت یک دیتاست در هر بار در صفحات <a href="/medical/net/json-to-dicom/">JSON to DICOM</a> و <a href="/medical/net/xml-to-dicom/">XML to DICOM</a> خوانده می‌شود.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="فریم به فریم">}}

<p>داده‌های چند‑قابلمه به صورت فریم به فریم مورد دسترس قرار می‌گیرند، بنابراین یک سری ۵۰۰ فریم هزینه خواندن یک فریم به‌صورت جداگانه را دارد نه کل عنصر داده‌های پیکسلی.</p>

<div class="codeblock" id="code">
 <h3>گشتن در فریم‌ها - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="جایی که این تصمیم‌گیری به طراحی می‌رسد">}}

<ul>
<li>ایندکس‌گذاری و مهاجرت آرشیو: میلیون‌ها پرونده، و تنها هدر تا زمانی که چیزی جابه‌جا شود اهمیت دارد.</li>
<li>مسیر‌دهنده‌ها و گره‌های ذخیره‌سازی: یک مطالعه را بپذیرند، آنچه برای مسیریابی لازم است بخوانند، بایت‌ها را منتقل کنند.</li>
<li>خطوط لوله AI: مانیفست را از فراداده‌ها بسازید، سپس فریم‌ها را برای زیرمجموعه‌ای که واقعاً برای آموزش استفاده می‌شود دریافت کنید.</li>
<li>محفظه‌هایی با محدودیت حافظه: مجموعه کاری بر اساس استراتژی تصمیم می‌گیرد، نه بر پایه حجم پرونده.</li>
<li>داده‌های اسلاید کامل و OCT: پرونده‌هایی که خواندن همه‌چیز به‌طور کامل امکان‌پذیر نیست.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">راهنمای مدیریت حافظه</a> استراتژی‌ها را به‌تفصیل توضیح می‌دهد و <a href="/medical/net/dicom-networking/">شبکه‌سازی DICOM</a> همان داده‌ها را که از طریق DIMSE می‌رسند نشان می‌دهد.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="منابع یادگیری" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="مستندات" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="راهنمای توسعه‌دهندگان" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
{{< blocks/products/pf/slr-element name="مرجع API" href="https://reference.aspose.com/medical/net/" >}}
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
