---
title: JPEG XL برای DICOM در C# .NET | Aspose.Medical
weight: 10500

description: تصویرهای DICOM را از C# به صورت JPEG XL ذخیره کنید. JPEG XL بدون‌افتراکی که پیکسل‌ها را بیت به بیت باز می‌گرداند، در یک اسمبلی مدیریت‌شدهٔ تک بدون نیاز به کدک بومی برای استقرار.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL برای DICOM در .NET C#" h2="جدیدترین فشرده‌سازی در استاندارد DICOM، با کوچکترین فایل‌های بدون‌افتراکی که اندازه‌گیری کردیم، پیاده‌سازی‌شده در C# مدیریت‌شده و درون یک اسمبلی توزیع می‌شود." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="چرا JPEG XL به DICOM رسید">}}

<p>آرشیوهای پزشکی رشد می‌کنند و هرگز کاهش نمی‌یابند. JPEG XL کدکی است که جهان تصویرسازی پس از دو دهه تجربه با JPEG و JPEG 2000 طراحی کرد، و DICOM آن را به عنوان یک سینتکس انتقال افزود به دلیلی که تیم‌های ذخیره‌سازی به آن اهمیت می‌دهند: برای یکسان بودن پیکسل‌ها، فایل کوچکتر است.</p>

<p><strong>Aspose.Medical برای .NET</strong> JPEG XL را از طریق پورت C# کتابخانه libjxl می‌نویسد و می‌خواند. این بسته یک اسمبلی، <code>Aspose.Medical.dll</code>، را ارائه می‌دهد و باینری بومی دیگری در کنار آن وجود ندارد، بنابراین یک کدک جدید تبدیل به پروژهٔ استقرار نمی‌شود: همان اسمبلی روی ویندوز، لینوکس، عامل ساخت و درون یک کانتینر اجرا می‌شود.</p>

<p>دو سینتکس انتقال پیکسل‌ها را حمل می‌کنند:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110)، برای داده‌های تشخیصی که باید بدون تغییر برگردند.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112)، برای مواردی که اندازهٔ کوچکتر فایل مهم‌تر از یک کپی دقیق است.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="یک مطالعه را فشرده کنید، هر پیکسل را حفظ کنید">}}

<p>تبدیل فرمت (Transcoding) با یک فراخوانی انجام می‌شود و مجموعه داده‌های پیرامون پیکسل‌ها همراه آن حرکت می‌کند.</p>

<div class="codeblock" id="code">
 <h3>تبدیل یک فایل DICOM به JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>ما این را روی تصویر 16‌بیتی با ابعاد 1714 × 1933 از مجموعه تست خود اندازه‌گیری کردیم: 6.3 MB بدون فشرده‌سازی به 2.7 MB در JPEG XL بدون‌افتراکی تبدیل شد، که کوچکتر از همان تصویر در HTJ2K بدون‌افتراکی است. اعداد شما به مدالیته وابسته است، بنابراین قبل از انتخاب، مقایسه را روی پوشه‌ای از فایل‌های خود اجرا کنید.</p>

<p>کلمهٔ «بدون‌افتراکی» در اینجا به معنای واقعی خود است. به JPEG XL تبدیل کنید و دوباره، و داده‌های پیکسل دقیقاً برابر با بایت‌های اولیه هستند، بنابراین یک آرشیو می‌تواند بدون هیچ‌گونه بحثی دربارهٔ کیفیت تشخیصی مجدداً فشرده شود.</p>

<div class="codeblock" id="code">
 <h3>بازگشت به یک سینتکس بدون‌فشرده‌سازی - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="آنچه به‌صورت JPEG XL ذخیره شده است را بخوانید">}}

<p>فایلی که به‌صورت JPEG XL می‌آید همانند سایر فایل‌ها باز می‌شود. سینتکس انتقال نشان می‌دهد که چه نوعی است، و داده‌های پیکسل پس از رمزگشایی فریم در دسترس می‌باشند.</p>

<div class="codeblock" id="code">
 <h3>باز کردن یک فایل JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL یا HTJ2K">}}

<p>هر دو جدید هستند، هر دو در حالت بدون‌افتراکی هستند وقتی شما درخواست بدون‌افتراکی می‌کنید، و کتابخانه هر دو را می‌نویسد و می‌خواند. آن‌ها به سؤالات متفاوتی پاسخ می‌دهند.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>سوال</th>
<th>پاسخ</th>
</tr>
</thead>
<tbody>
<tr><td>کدامیک در آزمایش ما فایل کوچکتری تولید کرد</td><td>JPEG XL بدون‌افتراکی، چند درصد کمتر</td></tr>
<tr><td>کدامیک برای نمایش پیش‌رونده روی شبکه ساخته شده است</td><td><a href="/medical/net/htj2k/">HTJ2K</a>، به‌ویژهٔ نوع RPCL</td></tr>
<tr><td>کدامیک ابتدا به استاندارد DICOM وارد شد</td><td>HTJ2K، بنابراین امروزه آرشیوهای بیشتری آن را می‌پذیرند</td></tr>
<tr><td>کدامیک وابستگی بومی دارد</td><td>هیچ‌کدام، هر دو کد مدیریت‌شده در یک اسمبلی هستند</td></tr>
</tbody>
</table>

<p>انتخاب معمولاً از سمت دیگر لینک می‌آید: به سینتکس مورد پذیرش آرشیو تبدیل کنید و بقیهٔ زنجیره پردازش را همان‌طور نگه دارید.</p>

<div class="codeblock" id="code">
 <h3>بگذارید آرشیوی هدف تصمیم بگیرد - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="جایی که سودآور است">}}

<ul>
<li>آرشیوهای طولانی‌مدت: همان مطالعات، ترابایت‌های کمتر، و بدون افت برای توجیه به رادیولوژیست.</li>
<li>صورتحساب‌های ذخیره‌سازی ابری: صرفه‌جویی هر ماه تکرار می‌شود، در حالی که تبدیل فرمت یک‌بار اجرا می‌شود.</li>
<li>مجموعه‌های داده برای تحقیقات و هوش مصنوعی: نسخه‌های کوچکتر سریع‌تر بین ذخیره‌سازی و آموزش جابجا می‌شوند.</li>
<li>استقرار: کدکی جدید معمولاً به معنای ساخت بومی برای هر پلتفرم است؛ در اینجا بخشی از اسمبلی است که پیش‌تر ارجاع داده‌اید.</li>
</ul>

<p>کتابخانه همچنین کدک‌هایی را می‌نویسد که یک آرشیو موجود پر از آن‌هاست: JPEG، JPEG‑LS، JPEG 2000، HTJ2K و RLE. صفحهٔ <a href="/medical/net/dicom-transfer-syntax-conversion/">تبدیل سینتکس انتقال</a> کل مجموعه را پوشش می‌دهد، <a href="/medical/net/htj2k/">HTJ2K</a> صفحهٔ اختصاصی خود را دارد، و <a href="/medical/net/jpeg2000/">JPEG 2000</a> جایی است که هر دو کدک جدید از آن می‌آیند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="منابع یادگیری" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="مستندات" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="راهنمای توسعه‌دهندگان" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="مستندات API" href="https://reference.aspose.com/medical/net/" >}}
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
