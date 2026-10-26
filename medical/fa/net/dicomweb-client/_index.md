---
title: کلاینت DICOMweb در C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: جستجو، دانلود و آپلود مطالعات DICOM از طریق HTTP با .NET. یک کلاینت DICOMweb به صورت خالص C# برای QIDO-RS، WADO-RS و STOW-RS که با PACS، VNA و سرویس‌های تصویربرداری ابری کار می‌کند.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="کلاینت DICOMweb برای .NET C#" h2="جستجو، دانلود و آپلود مطالعات بر روی هر سرور DICOMweb با QIDO-RS، WADO-RS و STOW-RS، در C# مدیریت‌شده بدون نیاز به نصب هیچ‌چیزی بر روی ماشین." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM از طریق HTTP، از کد خودتان">}}

<p>DICOMweb شکل HTTP DICOM است. به‌جای ارتباط DIMSE در پورت ۱۰۴، برنامه شما درخواست‌های عادی HTTPS می‌فرستد: QIDO‑RS برای جستجو، WADO‑RS برای دانلود، STOW‑RS برای آپلود. سرویس‌های تصویربرداری ابری مانند سرویس DICOM در Azure Health Data Services و Google Cloud Healthcare API به همین صورت عمل می‌کنند و همین‌طور بایگانی‌هایی مانند Orthanc و dcm4chee.</p>

<p><strong>Aspose.Medical برای .NET</strong> از نسخه ۲۶.۱۰ شامل یک کلاینت DICOMweb می‌باشد. <code>DicomWebClient</code> هر تراکنش استاندارد را در یک متد تایپ‌شده می‌پیچد و نتایج به صورت همان اشیای <code>Dataset</code> و <code>DicomFile</code> که بقیه کتابخانه می‌خوانند و می‌نویسند، بازگردانده می‌شوند. سرور در سمت دیگر می‌تواند هر پیاده‌سازی DICOMweb باشد؛ نیازی به ساخت با Aspose.Medical نیست.</p>

<p>کلاینت C# مدیریت‌شده‌ای است که بر پایه <code>HttpClient</code> ساخته شده است. هیچ ابزارکی بومی یا سرویس پراکسی وجود ندارد، بنابراین همان کد بر روی ویندوز، لینوکس و در یک کانتینر اجرا می‌شود.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="اتصال به سرور DICOMweb">}}

<p>کلاینت را از یک <code>HttpClient</code> و URL ریشه سرویس سرور ایجاد کنید. احراز هویت، تنظیمات TLS، پروکسی‌ها و استخر اتصال در <code>HttpClient</code> باقی می‌مانند، جایی که برنامه شما قبلاً آنها را پیکربندی کرده است. کلاینت مالک <code>HttpClient</code> نیست و آن را آزاد (dispose) نمی‌کند.</p>

<div class="codeblock" id="code">
 <h3>ایجاد یک کلاینت DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>اگر یک استقرار سرویس WADO-URI یا سرویس دیگری را از URL متفاوتی ارائه دهد، <code>ServiceBaseUris</code> ریشه را تنها برای آن سرویس بازنویسی می‌کند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="جستجوی مطالعات با QIDO-RS">}}

<p>یک جستجو پارامترهای منطبق را با کلیدواژه DICOM به عنوان نام، فیلدهای بازگشتی و کنترل‌های صفحه‌بندی می‌گیرد. نتایج به صورت جریان datasets می‌آیند و به‌تدریج یک‌به‌یک خوانده می‌شوند در حالی که پاسخ هنوز در حال دانلود است.</p>

<div class="codeblock" id="code">
 <h3>یافتن مطالعات یک بیمار - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> و <code>SearchInstancesAsync</code> یک سطح پایین‌تر جستجو می‌کنند، در سراسر بایگانی یا داخل یک مطالعه یا سری. همان گزینه‌ها همچنین قابلیت جستجوی مبهم، تطبیق مقدار خالی و افست نتایج برای صفحه‌بندی را تنظیم می‌کنند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="دانلود یک مطالعه با WADO-RS">}}

<p>یک مطالعه می‌تواند هزاران نمونه داشته باشد. <code>RetrieveStudyAsync</code> آنها را به صورت جریان باز می‌گرداند، بنابراین هر فایل به محض دریافت بخش مربوطه از پاسخ تجزیه می‌شود و می‌تواند قبل از خواندن فایل بعدی ذخیره شود. مصرف حافظه با اندازه مطالعه افزایش نمی‌یابد.</p>

<div class="codeblock" id="code">
 <h3>ذخیره هر نمونه از یک مطالعه - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> و <code>RetrieveInstanceAsync</code> یک سری یا یک نمونه را به همان روش دانلود می‌کنند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="دریافت تصویر بندانگشتی یا تصویر رندر شده">}}

<p>یک نمایشگر یا صفحه کارهای لیست اغلب به تصویر نیاز دارند، نه به فایل DICOM. سرور آن را رندر می‌کند و یک JPEG یا PNG ارسال می‌نماید، بنابراین کلاینت فایل پیکسل اصلی را دانلود یا رمزگشایی نمی‌کند.</p>

<div class="codeblock" id="code">
 <h3>ذخیره یک تصویر بندانگشتی JPEG از یک نمونه - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p><code>RetrieveRenderedInstanceAsync</code> و <code>RetrieveRenderedFramesAsync</code> به جای آن یک تصویر تمام‌سایز باز می‌گردانند. <code>DicomWebRenderOptions</code> کنترل‌های استاندارد رندرینگ را به سرور می‌فرستد: کیفیت تصویر، اندازه تصویر نمایی، پنجره و پروفایل ICC.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="آپلود نمونه‌ها با STOW-RS">}}

<p>STOW-RS هر تعداد نمونه را در یک درخواست می‌فرستد. سرور با یک گزارش پاسخ می‌دهد که به‌صورت نمونه به نمونه فهرست می‌کند چه چیزی ذخیره شده و چه چیزی رد شده است.</p>

<div class="codeblock" id="code">
 <h3>آپلود یک پوشه از فایل‌های DICOM - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>برخی سرورها هنگام شکست یک یا چند نمونه، با خطای HTTP مانند ۴۰۹ پاسخ می‌دهند. در این صورت فراخوانی <code>DicomWebClientException</code> را پرتاب می‌کند، همان‌طور که بخش بعدی نشان می‌دهد، و همان گزارش در بدنهٔ پاسخ استثنا موجود است.</p>

<p><code>StoreStudyAsync</code> به یک مطالعه نام‌دار آپلود می‌کند. هر دو متد همچنین یک <code>IAsyncEnumerable&lt;DicomFile&gt;</code> می‌پذیرند، به‌طوری که فایل‌ها می‌توانند در حین ارسال درخواست تولید شوند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="مدیریت خطاهای سرور">}}

<p>هر عملیات تایپ‌شده پیش از بازگرداندن، وضعیت HTTP را بررسی می‌کند. وقتی سرور درخواست را رد می‌کند، فراخوانی <code>DicomWebClientException</code> را پرتاب می‌کند که شامل کد وضعیت، هدرها و ابتدای بدنهٔ پاسخ برای ثبت لاگ است.</p>

<div class="codeblock" id="code">
 <h3>گزارش درخواست ناموفق - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>متدهایی که پاسخ خام را باز می‌گردانند، مانند تصویر بندانگشتی بالا، بررسی وضعیت را به عهدهٔ شما می‌گذارند از طریق <code>IsSuccessStatusCode</code> یا <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="موارد دیگر پوشش داده شده توسط کلاینت">}}

<p>جستجو، دانلود و آپلود فراخوانی‌های رایج هستند. کلاینت همچنین یک گروه عملیات برای هر یک از سرویس‌های دیگر DICOMweb دارد:</p>

<ul>
<li>داده‌های انبوه، داده‌های پیکسل و فریم‌های انتخابی یک نمونه، بر حسب شماره فریم (یک‌پایه) همان‌طور که سرور ارسال می‌کند.</li>
<li>WADO-URI، بازیابی تک‌آدرس قدیمی که بسیاری از نمایشگرها و بایگانی‌ها هنوز استفاده می‌کنند، برای فایل‌های DICOM و تصاویر رندر شده.</li>
<li>لیست‌های کاری UPS-RS: ایجاد، جستجو، به‌روزرسانی و درخواست کارها، تغییر وضعیت آن‌ها، درخواست لغو، و اشتراک در رویدادهایی که از طریق WebSocket می‌آیند.</li>
<li>لیست کاری مودالیتی (MWL-RS)، مراحل انجام شدهٔ پروسیجر مودالیتی (MPPS-RS) و تعهد ذخیره‌سازی.</li>
<li>نمونه‌های غیر بیمار مانند پالت‌های رنگی و پروتکل‌های معلق، و سند قابلیت‌های سرور.</li>
<li><code>SendAsync</code> برای هر تراکنش استاندارد با مقادیر مسیر، پارامترهای پرس‌و‌جو، هدرها و بدنهٔ خودتان، زمانی که متد تایپ‌شده مناسب نیست.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">راهنمای کلاینت DICOMweb</a> هر عملیات و گزینه را مستند می‌کند، و <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">مرجع عملیات کلاینت</a> تمام متدها را فهرست می‌نماید.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="یک کتابخانه از پاسخ HTTP تا داده‌های پیکسل">}}

<p>فایلی که با کلاینت دانلود می‌شود یک <code>DicomFile</code> معمولی است. می‌توان آن را به‌صورت ناشناس، تبدیل فرمت، رندر یا سریالایز کرد در همان فرآیند، بدون هیچ مرحلهٔ تبدیل و بدون کتابخانهٔ دوم در زنجیره.</p>

<p>صفحات مرتبط: <a href="/medical/net/dicom-networking/">شبکه‌بندی DICOM</a> برای بایگانی‌هایی که به جای HTTP از DIMSE استفاده می‌کنند، <a href="/medical/net/anonymization/">ناشناس‌سازی</a> قبل از خروج یک مطالعه از بیمارستان، <a href="/medical/net/dicom-transfer-syntax-conversion/">تبدیل سینتکس انتقال</a> برای آنچه سرور ذخیره می‌کند، و <a href="/medical/net/dicom-to-json/">DICOM به JSON</a> برای قالبی که نتایج جستجوی QIDO‑RS استفاده می‌کند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="منابع آموزشی" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="مستندات" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="راهنمای توسعه‌دهنده" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="مراجع API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
