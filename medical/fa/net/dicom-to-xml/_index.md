---
title: تبدیل DICOM به XML در C# .NET | Aspose.Medical
weight: 3000
description: تسریس مجموعه داده‌های DICOM به قالب استاندارد DICOM XML در C# .NET. پیکربندی مدیریت داده‌های انبوه، پردازش مبتنی بر جریان و عملیات ناهمزمان با Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تبدیل DICOM به XML در .NET C#" h2="تسریس مجموعه داده‌های DICOM به نمایش استاندارد DICOM XML (PS3.19). پیکربندی ارجاعات داده‌های انبوه، خروجی مبتنی بر جریان، و پردازش ناهمزمان با کتابخانه خالص .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="سری‌سازی DICOM XML مبتنی بر استانداردها">}}

<p><strong>Aspose.Medical for .NET</strong> داده‌های DICOM را به XML سریالایز می‌کند مطابق با <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">مدل بومی DICOM PS3.19</a>. این استاندارد رسمی برای نمایش مجموعه داده‌های DICOM در XML است، که توسط سرویس‌های DICOMweb، پلتفرم‌های یکپارچه‌سازی و سیستم‌هایی که به نمایشی قابل خواندن برای انسان و اعتبارسنجی شده توسط اسکیما از متادیتاهای تصویربرداری پزشکی نیاز دارند، استفاده می‌شود.</p>

<p>کلاس <code>DicomXmlSerializer</code> متدهای ایستا برای هر دو عملیات سریالیزه‌سازی و دی‌سریالیزه‌سازی فراهم می‌کند. برخلاف روش‌های سادهٔ استخراج تگ، خروجی با اسکیماهای DICOM XML سازگار است به‌طوری‌که هر عنصر با تگ، VR و مقادیر به‌درستی قالب‌بندی‌شدهٔ خود نمایش داده می‌شود &mdash; که امکان تبدیل بدون افت داده به‌صورت دور‌گرد بین DICOM باینری و XML را فراهم می‌آورد.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="سریالیزه‌سازی DICOM به XML در C#">}}

<p>از کلاس <code>DicomXmlSerializer</code> برای تبدیل یک مجموعه دادهٔ DICOM به رشتهٔ XML استفاده کنید. ساده‌ترین روش یک سند XML مطابقت‌کننده با استاندارد تولید می‌کند:</p>

<div class="codeblock" id="code">
 <h3>تبدیل DICOM به XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="سریالیزه‌سازی مبتنی بر جریان و ناهمزمان">}}

<p>برای فایل‌های DICOM بزرگ یا سناریوهای پردازش پرسرعت، به‌صورت مستقیم به یک جریان سریالیزه کنید تا از تخصیص رشته‌های بزرگ در حافظه جلوگیری شود. هر دو متد همزمان و ناهمزمان در دسترس هستند:</p>

<div class="codeblock" id="code">
 <h3>سریالیزه‌سازی همزمان به‌صورت جریان - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>سریالیزه‌سازی ناهمزمان به‌صورت جریان - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="پخش لوله‌ای برای مطالعات بزرگ">}}

<p>کل مطالعات نیازی به نگهداری در حافظه ندارند. <code>DicomXmlSerializer</code> به <code>PipeWriter</code> می‌نویسد و از <code>PipeReader</code> می‌خوانَد، بنابراین XML می‌تواند همان‌طور که جریان دارد تولید و مصرف شود، و یک دنباله از مجموعه داده‌ها می‌تواند به‌صورت تک‌تک از طریق <code>DeserializeAsyncEnumerable</code> خوانده شود. هر متد یک <code>CancellationToken</code> می‌گیرد.</p>

<div class="codeblock" id="code">
 <h3>سریالیزه و دی‌سریالیزه از طریق لوله - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>خواندن دنباله‌ای از مجموعه داده‌ها به‌صورت تک‌تک - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="گزینه‌های سریالیزه‌سازی">}}

<p>کلاس <code>DicomXmlSerializerOptions</code> نحوهٔ نمایش داده‌های DICOM در XML را کنترل می‌کند. پیکربندی اصلی شامل مدیریت داده‌های انبوه برای مقادیر باینری بزرگ است:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>ویژگی</th>
<th>نوع</th>
<th>توضیح</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>مبدل سفارشی برای نوشتن داده‌های بزرگ (مثلاً داده‌های پیکسل) به‌صورت ارجاع BulkData URI به‌جای درون‌خطی کردن</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>بارگذار سفارشی برای حل‌کردن URI‌های BulkData هنگام دی‌سریالیزه‌سازی</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>نسخه پیش‌فرض گزینه‌ها که زمانی استفاده می‌شود که گزینه سفارشی ارائه نشود</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>سریالیزه‌سازی با گزینه‌های سفارشی - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="مدیریت داده‌های انبوه">}}

<p>مقادیر باینری بزرگ (داده‌های پیکسل، موج‌ها، اسناد بسته‌بندی‌شده) می‌توانند به‌صورت ارجاعات BulkData URI خارجی شوند به‌جای این که در خروجی XML درون‌خطی شوند. این مطابق با مشخصات <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">عنصر BulkData در DICOM PS3.19</a> است.</p>

<p>برای خارجی‌سازی داده‌های بزرگ در هنگام سریالیزه‌سازی <code>IBulkDataConverter</code> را پیاده‌سازی کنید و برای حل‌کردن URI‌ها در دی‌سریالیزه‌سازی <code>IBulkDataLoader</code> را پیاده‌سازی کنید. در موارد معمول نیازی به نوشتن بارگذار نیست: <code>DefaultBulkDataLoader.Instance</code> URI‌های <code>file</code>، <code>http</code> و <code>https</code> را حل می‌کند و همچنین <code>IAsyncBulkDataLoader</code> را پیاده‌سازی می‌نماید، بنابراین داده‌های انبوه به‌صورت ناهمزمان در مسیرهای جریان‌داری دریافت می‌شوند.</p>

<div class="codeblock" id="code">
 <h3>مدیریت سفارشی داده‌های انبوه - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
public class FileBulkDataConverter : IBulkDataConverter
{
    public string? GetBulkDataUri(IElement element)
    {
        // Externalize pixel data to a separate file
        if (element.Tag == Tag.PixelData)
            return "file:///bulk/pixeldata.raw";
        return null; // inline all other elements
    }
}

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="دی‌سریالیزه‌سازی XML به DICOM">}}

<p>XML DICOM را به اشیاء Dataset بازخوانی کنید. ورودی رشته، ورودی جریان و عملیات ناهمزمان را پشتیبانی می‌کند:</p>

<div class="codeblock" id="code">
 <h3>دی‌سریالیزه‌سازی XML به DICOM - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="سریالیزه‌سازی XML در مقابل JSON">}}

<p>Aspose.Medical هر دو سریالیزه‌سازی DICOM XML (PS3.19) و DICOM JSON (PS3.18) را پشتیبانی می‌کند. هر دو قالب تبدیل بدون افت داده را فراهم می‌آورند، اما برای سناریوهای یکپارچه‌سازی متفاوتی مناسب هستند:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>ویژگی</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>استاندارد</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>اعتبارسنجی اسکیما</td><td>XML Schema (XSD) موجود</td><td>بدون اسکیما رسمی</td></tr>
<tr><td>بهترین کاربرد برای</td><td>یکپارچه‌سازی سازمانی، HL7 CDA، لاگ‌های حسابرسی، رجیستری‌های XDS</td><td>DICOMweb، REST APIها، FHIR ImagingStudy</td></tr>
<tr><td>قابلیت خواندن توسط انسان</td><td>کامل اما خود توصیف‌کننده</td><td>فشرده و به‌طور گسترده پشتیبانی‌شده</td></tr>
<tr><td>داده‌های انبوه</td><td>عنصر BulkData با URI</td><td>خاصیت BulkDataURI</td></tr>
<tr><td>کلاس سریالایزر</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

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
