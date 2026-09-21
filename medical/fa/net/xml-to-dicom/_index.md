---
title: تبدیل XML به DICOM در C# .NET | Aspose.Medical
weight: 5000

description: ساخت فایل‌های DICOM از XML مدل بومی DICOM نسخه PS3.19 در C# .NET. خواندن XML از رشته، جریان یا لوله، پخش اسناد متوالی، و حل مراجع داده‌های حجیم با API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="تبدیل XML به DICOM در .NET C#" h2="خواندن XML مدل بومی DICOM نسخه PS3.19 به دیتاست‌ها و فایل‌های DICOM. کار از رشته، جریان یا لوله، پخش اسناد متوالی، و حل مراجع داده‌های حجیم." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="XML استاندارد مدل بومی DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> مدل بومی DICOM تعریف‌شده در DICOM PS3.19 را می‌خواند. این نمایه‌سازی XML است که در خود استاندارد نوشته شده است، نه قالبی که Aspose ساخته، که همین باعث می‌شود برای یکپارچه‌سازی مفید باشد: سیستمی که پیشاپیش DICOM را به صورت XML رد و بدل می‌کند، اسنادی را تولید می‌کند که این کتابخانه می‌پذیرد.</p>

<p>ریشهٔ سند <code>NativeDicomModel</code> است و هر ویژگی یک عنصر <code>DicomAttribute</code> است که برچسب، نمایه مقدار و کلیدواژهٔ خود را حمل می‌کند:</p>

<div class="codeblock" id="code">
 <h3>فرمت مدل بومی DICOM</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>این صفحه جهت معکوس <a href="/medical/net/dicom-to-xml/">DICOM به XML</a> است و هر دو از همان کلاس <code>DicomXmlSerializer</code> استفاده می‌کنند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ایجاد فایل DICOM از XML در C#">}}

<p><code>Deserialize</code> یک سند را به یک <code>Dataset</code> تبدیل می‌کند و دیتاست به‌عنوان یک فایل DICOM بر روی دیسک نوشته می‌شود.</p>

<div class="codeblock" id="code">
 <h3>ایجاد فایل DICOM از XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>مدل بومی DICOM گروه اطلاعات متا فایل را ندارد، بنابراین سینتکس انتقال بخشی از سند نیست. یک دیتاست که در یک <code>DicomFile</code> بسته‌بندی شده است با استفاده از سینتکس انتقال پیش‌فرض، Implicit VR Little Endian، نوشته می‌شود. برای ذخیرهٔ فایل با یک سینتکس دیگر، آن را تبدیل کنید، همان‌طور که صفحهٔ <a href="/medical/net/dicom-transfer-syntax-conversion/">تبدیل سینتکس انتقال</a> نشان می‌دهد.</p>

<p>خواندن DICOM XML یک ویژگی دارای لایسنس است. بدون اعمال لایسنس محلی، خواننده یک <code>MedicalApiException</code> پرتاب می‌کند، بنابراین ابتدا لایسنس را اعمال کنید، همان‌طور که <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">راهنمای لایسنس</a> توضیح می‌دهد.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="جریان‌ها، لوله‌ها و ناهمگام">}}

<p>هر نقطهٔ ورودی یک بارگیری برای جریان و یک بارگیری ناهمگام دارد و نسخه‌های ناهمگام همچنین یک <code>PipeReader</code> می‌پذیرند. سندی که از پاسخ وب می‌آید در حین خواندن تجزیه می‌شود، بدون آن‌که ابتدا به رشته تبدیل شود.</p>

<div class="codeblock" id="code">
 <h3>خواندن XML از یک جریان - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="اسناد متوالی در یک جریان">}}

<p>یک خروجی از سیستم دیگر اغلب یک عنصر <code>NativeDicomModel</code> پس از دیگری را در یک جریان واحد نگه می‌دارد. <code>DeserializeAsyncEnumerable</code> برای هر عنصر یک دیتاست تولید می‌کند، به ترتیب ورودی، به‌طوری که جریان بدون نگهداری در حافظه پردازش می‌شود. عناصر به‌صورت مستقیم پشت سر هم می‌آیند: یک اعلان XML تنها در ابتدا مجاز است، همان‌طور که در هر ورودی XML است.</p>

<div class="codeblock" id="code">
 <h3>پخش اسناد متوالی - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="مراجع داده‌های حجیم">}}

<p>مقدارهای بزرگ مانند داده‌های پیکسل به‌صورت درون‌خطی نوشته نمی‌شوند. آن‌ها به‌صورت عنصر <code>BulkData</code> با یک URI که به بایت‌ها اشاره می‌کند ظاهر می‌شوند، که باعث کوچک‌ماندن سند می‌شود. برای حل این مراجع هنگام خواندن، به serializer یک بارگذار داده‌های حجیم بدهید. <code>DefaultBulkDataLoader</code> URIهای <code>file</code>، <code>http</code> و <code>https</code> را بدون احراز هویت دریافت می‌کند؛ برای آرشیوی که به اعتبارنامه نیاز دارد، خودتان <code>IBulkDataLoader</code> یا <code>IAsyncBulkDataLoader</code> را پیاده‌سازی کنید.</p>

<div class="codeblock" id="code">
 <h3>حل داده‌های حجیم هنگام خواندن - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="گردش دوطرفه با DICOM به XML">}}

<p>دو جهت برای استفادهٔ همزمان طراحی شده‌اند: یک مطالعه به‌صورت XML خروجی می‌شود، از سیستمی که XML می‌داند می‌گذرد، و به‌عنوان یک فایل DICOM باز می‌گردد. همه چیز در .NET مدیریت می‌شود، بنابراین همان گردش دوطرفه بر روی Windows، Linux و macOS اجرا می‌شود.</p>

<div class="codeblock" id="code">
 <h3>DICOM به XML و برگرداندن - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>برای گزینه‌هایی که ظاهر XML را کنترل می‌کند، به صفحهٔ <a href="/medical/net/dicom-to-xml/">DICOM به XML</a> مراجعه کنید. همان جفت برای JSON نیز موجود است: <a href="/medical/net/dicom-to-json/">DICOM به JSON</a> و <a href="/medical/net/json-to-dicom/">JSON به DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">راهنمای سریالیزاسیون</a> تمام API را پوشش می‌دهد.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="منابع یادگیری" tabId="resources" >}}
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