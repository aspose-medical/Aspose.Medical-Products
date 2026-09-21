---
title: تبدیل JSON به DICOM در C# .NET | Aspose.Medical
weight: 6000

description: فایل‌های DICOM را از مدل استاندارد DICOM JSON (PS3.18) در C# .NET بسازید. JSON را از یک رشته، یک جریان یا یک لوله بخوانید، یک دنباله از datasets را به صورت جریان ارائه دهید و مراجع داده‌های bulk را با API Aspose.Medical حل کنید.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تبدیل JSON به DICOM در .NET C#" h2="مدل استاندارد DICOM JSON (PS3.18) را به‌صورت datasets و فایل‌های DICOM بازخوانی کنید. از یک رشته، یک جریان یا یک لوله کار کنید، یک دنباله از studies را به‌صورت جریان ارائه دهید و مراجع داده‌های bulk را حل کنید." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="از DICOM JSON به یک فایل DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> مدل <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON</a> را می‌خواند، نمایه‌ای که توسط سرویس‌های DICOMweb و سیستم‌هایی که مطالعات را از طریق HTTP مبادله می‌کنند استفاده می‌شود. آنچه به‌صورت JSON می‌آید به یک <code>Dataset</code> تبدیل می‌شود و یک <code>Dataset</code> به‌عنوان یک فایل DICOM روی دیسک نوشته می‌شود.</p>

<p>این جهت معکوس صفحه <a href="/medical/net/dicom-to-json/">DICOM به JSON</a> است و هر دو از یک کلاس یکسان، <code>DicomJsonSerializer</code>، استفاده می‌کنند.</p>

<div class="codeblock" id="code">
 <h3>ایجاد فایل DICOM از JSON - C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>دست‌ست (Dataset)ی که File Meta Information ندارد، هنگام بسته شدن در یک <code>DicomFile</code> با سینتکس انتقال پیش‌فرض Implicit VR Little Endian نوشته می‌شود.</p>

<p>خواندن DICOM JSON یک ویژگی دارای لایسنس است. بدون اعمال لایسنس محلی، خواننده یک <code>MedicalApiException</code> پرتاب می‌کند، بنابراین ابتدا لایسنس را اعمال کنید، همان‌گونه که <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">راهنمای لایسنس</a> توضیح می‌دهد.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="حفظ File Meta Information">}}

<p><code>Deserialize</code> فقط دیتاست را برمی‌گرداند. وقتی سند JSON همچنین گروه File Meta Information را حمل می‌کند، برای مثال چون از یک فایل DICOM کامل تولید شده است، <code>DeserializeFile</code> یک <code>DicomFile</code> با آن گروه به‌صورت کامل باز می‌گرداند، شامل سینتکس انتقالی که فایل اعلام کرده است.</p>

<div class="codeblock" id="code">
 <h3>خواندن یک فایل DICOM کامل از JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="جریان‌ها، لوله‌ها و ناهمزمانی">}}

<p>هر نقطه ورودی یک overload برای stream و یک overload برای حالت asynchronous دارد و نسخه‌های asynchronous همچنین یک <code>PipeReader</code> را می‌پذیرند. سندی که از پاسخ وب یا از دیسک می‌آید بدون این‌که ابتدا به رشته تبدیل شود خوانده می‌شود، که وقتی JSON شامل داده‌های پیکسل باشد اهمیت دارد.</p>

<div class="codeblock" id="code">
 <h3>خواندن JSON از یک stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="دنباله‌ای از datasets، یکی‌به‌یکی">}}

<p>یک پرس‌وجوی DICOMweb با آرایه‌ای از datasets پاسخ می‌دهد و چنین سندی می‌تواند بزرگ باشد. <code>DeserializeList</code> کل آرایه را در حافظه می‌خواند؛ <code>DeserializeAsyncEnumerable</code> یک dataset را به‌صورت گام به گام باز می‌گرداند، به طوری که سند هرگز به‌صورت کامل در حافظه نگه‌داشته نمی‌شود.</p>

<div class="codeblock" id="code">
 <h3>پخش (stream) یک آرایه از datasets - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="مراجع داده‌های Bulk">}}

<p>مدل DICOM JSON داده‌های پیکسل را به‌صورت درون‌متنی (inline) حمل نمی‌کند. مقادیر بزرگ با یک <code>BulkDataURI</code> که به بایت‌ها اشاره دارد جایگزین می‌شوند، که باعث کوچک‌ماندن سند JSON می‌شود. برای حل این مراجع هنگام خواندن، یک بارگذاری‌کننده داده‌های bulk به serializer بدهید. <code>DefaultBulkDataLoader</code> URIهای <code>file</code>، <code>http</code> و <code>https</code> را بدون احراز هویت دریافت می‌کند؛ برای آرشیوی که به اعتبارنامه نیاز دارد، خودتان <code>IBulkDataLoader</code> یا <code>IAsyncBulkDataLoader</code> را پیاده‌سازی کنید.</p>

<div class="codeblock" id="code">
 <h3>حل BulkDataURI هنگام خواندن - C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="گردش‌به‌گرد (Round trip) با DICOM به JSON">}}

<p>دو جهت برای استفاده همراه طراحی شده‌اند: یک مطالعه به‌صورت JSON صادر می‌شود، از طریق یک سرویس وب عبور می‌کند و به‌عنوان یک فایل DICOM بازمی‌گردد. هیچ بخشی از این فرآیند به کد بومی وابسته نیست، بنابراین همان گردش‌به‌گرد بر روی Windows, Linux و macOS اجرا می‌شود.</p>

<div class="codeblock" id="code">
 <h3>DICOM به JSON و بازگشت - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>برای گزینه‌هایی که نحوه نمایش JSON را کنترل می‌کنند، صفحه <a href="/medical/net/dicom-to-json/">DICOM به JSON</a> را ببینید. همان جفت گزینه برای XML وجود دارد: <a href="/medical/net/dicom-to-xml/">DICOM به XML</a> و <a href="/medical/net/xml-to-dicom/">XML به DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">راهنمای سریال‌سازی JSON</a> تمام API را پوشش می‌دهد.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="منابع آموزشی" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="مستندات" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="راهنمای توسعه‌دهنده" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
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