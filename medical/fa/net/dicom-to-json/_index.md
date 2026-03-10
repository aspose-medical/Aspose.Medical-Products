---
title: تبدیل DICOM به JSON در C# .NET | Aspose.Medical
weight: 4000
description: فایل‌های DICOM را به فرمت استاندارد DICOM JSON در C# .NET سریالیز کنید. با استفاده از API Aspose.Medical، پردازش عددی، URIهای داده حجیم، خروجی کلیدواژه‌ها و پردازش ناهمزمان جریان را پیکربندی کنید.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تبدیل DICOM به JSON در .NET C#" h2="دیتاست‌ها و فایل‌های DICOM را به مدل استاندارد DICOM JSON (PS3.18) سریالیز کنید. پردازش عددی، ارجاع به داده‌های حجیم، خروجی کلیدواژه‌ها و پردازش ناهمزمان مبتنی بر جریان را پیکربندی کنید." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="مدل استاندارد DICOM JSON">}}

<p><strong>Aspose.Medical برای .NET</strong> داده‌های DICOM را به JSON سریالیز می‌کند بر مبنای <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">مدل DICOM PS3.18 JSON</a>. این استاندارد رسمی برای نمایش دیتاست‌های DICOM در قالب JSON است که توسط سرویس‌های DICOMweb، منابع FHIR ImagingStudy و APIهای مدرن حوزه سلامت استفاده می‌شود.</p>

<p>در مدل DICOM JSON، هر خصوصیت توسط تگ خود به‌عنوان کلید JSON نمایش داده می‌شود (مثال: <code>"00100010"</code> برای Patient Name)، به‌همراه نمایه مقدار (Value Representation) و آرایه مقدار به‌عنوان خواص تو در تو:</p>

<div class="codeblock" id="code">
 <h3>قالب مدل DICOM JSON</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="سریالیز DICOM به JSON در C#">}}

<p>از کلاس <code>DicomJsonSerializer</code> برای تبدیل یک فایل یا دیتاست DICOM به JSON استفاده کنید. ساده‌ترین روش خروجی فشرده و مطابق با استانداردها تولید می‌کند:</p>

<div class="codeblock" id="code">
 <h3>تبدیل DICOM به JSON - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p>می‌توانید یک <code>Dataset</code>، یک <code>DicomFile</code> کامل (شامل File Meta Information) یا آرایه‌ای از دیتاست‌ها را سریالیز کنید:</p>

<div class="codeblock" id="code">
 <h3>سریالیز DicomFile و آرایه‌های دیتاست - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="سریالیز مبتنی بر جریان و ناهمزمان">}}

<p>برای فایل‌های DICOM بزرگ یا سناریوهای پردازش پرسرعت، به‌صورت مستقیم به یک جریان UTF-8 سریالیز کنید تا از تخصیص رشته‌های بزرگ در حافظه جلوگیری شود. هم روش‌های همزمان و هم ناهمزمان در دسترس هستند:</p>

<div class="codeblock" id="code">
 <h3>سریالیز جریان و ناهمزمان - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="گزینه‌های سریالیز">}}

<p>کلاس <code>DicomJsonSerializerOptions</code> نحوه نمایش داده‌های DICOM در JSON را کنترل می‌کند:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>خاصیت</th>
<th>نوع</th>
<th>توضیح</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>کنترل می‌کند که آیا VRهای عددی (IS، DS، SV، UV) به‌عنوان اعداد JSON یا رشته‌های JSON نوشته شوند</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>استفاده از کلیدواژه‌های DICOM (مانند <code>"PatientName"</code>) به‌جای تگ‌ها (<code>"00100010"</code>) به‌عنوان کلیدهای JSON. توجه: غیر استاندارد</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>درج کلیدواژه DICOM به‌عنوان یک ویژگی JSON اضافی</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>درج نام تگ DICOM به‌عنوان یک ویژگی JSON اضافی</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>مبدل سفارشی برای نوشتن داده‌های بزرگ (مانند داده‌های پیکسل) به‌صورت ارجاع URI</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>بارگذار سفارشی برای حل URIهای داده‌های حجیم هنگام دی‌سریالیز</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>پیکربندی گزینه‌های سریالیز - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="پردازش عددی">}}

<p>نمایه‌های عددی DICOM (IS، DS، SV، UV) می‌توانند به‌عنوان اعداد JSON یا رشته‌های JSON سریالیز شوند. این مسأله برای قابلیت هم‌کاری مهم است، زیرا برخی پیاده‌سازی‌های DICOM اعداد را با فاصله‌های پیشرو یا قالب‌بندی غیر استاندارد ذخیره می‌کنند:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>حالت</th>
<th>خروجی JSON</th>
<th>سناریوی استفاده</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>مطابق با استاندارد، مناسب برای محاسبه. در صورت عدم توانایی در تجزیه مقدار، خطا می‌اندازد.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>نمایش رشته‌ای اصلی را حفظ می‌کند. برای عبور به‌دور و برگشت داده‌های غیر استانداری امن‌تر است.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="مدیریت داده‌های حجیم">}}

<p>مقدارهای باینری بزرگ (داده‌های پیکسل، موج‌سازها، اسناد بسته‌بندی‌شده) می‌توانند به‌عنوان ارجاع به داده‌های حجیم مدیریت شوند به‌جای این‌که به‌صورت درون‌خطی در خروجی JSON قرار گیرند. این مطابق با مشخصات <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a> است.</p>

<p>برای جداسازی داده‌های بزرگ هنگام سریالیز، <code>IBulkDataConverter</code> را پیاده‌سازی کنید، و برای حل URIها هنگام دی‌سریالیز، <code>IBulkDataLoader</code> را پیاده‌سازی نمایید:</p>

<div class="codeblock" id="code">
 <h3>مدیریت سفارشی داده‌های حجیم - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="دی‌سریالیز JSON به DICOM">}}

<p>JSON DICOM را دوباره به اشیاء Dataset یا DicomFile تبدیل کنید. پشتیبانی از ورودی رشته‌ای، ورودی جریان و عملیات ناهمزمان جریان دارد:</p>

<div class="codeblock" id="code">
 <h3>دی‌سریالیز JSON به DICOM - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="یکپارچه‌سازی System.Text.Json">}}

<p>Aspose.Medical، <code>DatasetJsonConverter</code> و <code>DicomFileJsonConverter</code> را برای یکپارچه‌سازی با خط لوله سریالیز استاندارد <code>System.Text.Json</code> فراهم می‌کند. این امکان را می‌دهد که اشیاء DICOM همراه با سایر انواع .NET در کدهای پردازش JSON موجود شما سریالیز شوند:</p>

<div class="codeblock" id="code">
 <h3>یکپارچه‌سازی System.Text.Json - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="منابع آموزشی" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="مستندات" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="کد منبع" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="مراجعه به API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="پشتیبانی محصول" tabId="support" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی رایگان" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی پرداختی" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="وبلاگ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="چرا Aspose.Medical برای .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="فهرست مشتریان" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="موفقیت‌ها" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
