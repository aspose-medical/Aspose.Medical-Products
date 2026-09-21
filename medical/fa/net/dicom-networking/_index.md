---
title: شبکه‌سازی DICOM در C# .NET - C-ECHO، C-STORE، C-FIND | Aspose.Medical
weight: 9000

description: برنامه .NET خود را به یک PACS متصل کنید. با C-ECHO اعتبارسنجی کنید، تصاویر را با C-STORE ارسال کنید، با C-FIND پرس و جو کنید و تصاویر را با SCP شخصی خود دریافت کنید. یک مشتری و سرور DIMSE به صورت خالص C#.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="شبکه‌سازی DICOM در .NET C#" h2="از برنامه خود به یک PACS ارتباط برقرار کنید: C-ECHO، C-STORE، C-FIND، C-MOVE و C-GET، به عنوان مشتری و به عنوان سرور، در C# مدیریت‌شده بدون نیاز به نصب چیزی بر روی ماشین." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="از کد خود به یک PACS متصل شوید">}}

<p>خواندن فایل‌های DICOM نیمی آسان از تصویربرداری پزشکی است. به محض اینکه برنامه شما مجبور شود با یک سیستم واقعی بیمارستان کار کند، باید به زبان DIMSE صحبت کند: یک ارتباط با PACS باز کنید، تصاویر را ارسال کنید، بپرسید چه مطالعاتی موجود است، و زمانی که سیستم دیگری چیزی برمی‌گرداند پاسخ دهید.</p>

<p><strong>Aspose.Medical برای .NET</strong> این پروتکل را به عنوان بخشی از کتابخانه ارائه می‌دهد. <code>Aspose.Medical.Dicom.Network</code> یک مشتری DIMSE و یک سرور DIMSE در اختیار شما می‌گذارد که هر دو به زبان C# مدیریت‌شده نوشته شده‌اند. هیچ ابزار بومی برای نصب وجود ندارد، سرویسی برای پیکربندی نیست و هیچ وابستگی پلتفرمی خاصی ندارد، بنابراین همان کد بر روی ویندوز، لینوکس و در یک کانتینر اجرا می‌شود.</p>

<p>سه مورد بیشتر ادغام‌ها را پوشش می‌دهند و هر کدام تنها چند خط کد هستند: ارتباط را با C-ECHO بررسی کنید، تصاویر را با C-STORE ارسال کنید و آنچه در سمت دیگر است را با C-FIND بیابید.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="با C-ECHO شروع کنید">}}

<p>C-ECHO پینگ DICOM است. این نشان می‌دهد که میزبان، پورت و دو عنوان AE صحیح‌اند پیش از آنکه چیز دیگری مورد بررسی قرار گیرد. یکبار یک مشتری بسازید، یک هندلر بدهید که پاسخ را مشاهده کند و درخواست را ارسال کنید.</p>

<div class="codeblock" id="code">
 <h3>تأیید اتصال با C-ECHO - C#</h3>
 <pre><code class="cs">AssociationNegotiationOptions negotiation = new AssociationNegotiationOptions()
    .WithPresentationContext(new PresentationContext
    {
        AbstractSyntax = Uid.Verification,
        Role = null,
        TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
    });

DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = negotiation
    })
    .AddCEchoHandler((request, response, cancellationToken) =>
    {
        Console.WriteLine($"C-ECHO answered with status 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(new CEchoRequest());

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>وضعیت <code>0x0000</code> به معنی موفقیت است. درخواست‌ها در صف قرار می‌گیرند و سپس در یک ارتباط ارسال می‌شوند، به طوری که یک دسته کار برای هر آیتم اتصال جدیدی باز نمی‌کند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ارسال تصاویر با C-STORE">}}

<p>C-STORE کاری است که یک برنامه پس از تولید یا دریافت یک تصویر انجام می‌دهد: نمونه را به بایگانی ارسال می‌کند. برای هر نمونه یک درخواست در صف بگذارید و آنها را به‌صورت گروهی ارسال کنید.</p>

<div class="codeblock" id="code">
 <h3>ارسال یک فایل DICOM به PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>سیناریوهای ارائه‌ای که پیشنهاد می‌دهید تصمیم می‌گیرد بایگانی چه چیزی را می‌پذیرد. اگر سینتکس فشرده‌ای می‌خواهد، قبل از ارسال تبدیل کنید، همان‌طور که صفحه <a href="/medical/net/dicom-transfer-syntax-conversion/">تبدیل سینتکس انتقال</a> نشان می‌دهد، یا گزینه‌های جایگزین را در <code>AdditionalTransferSyntaxes</code> فهرست کنید و اجازه دهید مذاکره یکی را انتخاب کند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="یافتن مطالعات با C-FIND">}}

<p>C-FIND به سؤال «بایگانی چه دارد» پاسخ می‌دهد. نتایج به‌صورت تک‌تک می‌آیند، هر کدام با مجموعه داده شناساگر خود، و یک پاسخ نهایی پرس و جو را خاتمه می‌دهد.</p>

<div class="codeblock" id="code">
 <h3>پرس‌وجوی مطالعات بر اساس بیمار - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.StudyRootQueryRetrieveInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddCFindHandler((request, response, cancellationToken) =>
    {
        // A match arrives with an identifier; the final response carries the status only
        if (response.Identifier is not null)
            Console.WriteLine(response.Identifier.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty));

        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(CFindRequest.CreateStudyQuery(
    patientId: "PATIENT-001",
    patientName: null,
    studyDateTime: null,
    accession: null,
    studyId: null,
    modalitiesInStudy: null,
    studyInstanceUid: null,
    priority: DimsePriority.Medium));

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>همان کارخانه سطوح پرس و جو دیگر را می‌سازد: <code>CreatePatientQuery</code>، <code>CreateSeriesQuery</code> و <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> یک پرس و جو برای فهرست کار مدالیته می‌سازد، که پرس و جویی است که یک مدالیته پیش از اسکن انجام می‌دهد.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="دریافت تصاویر: SCP ذخیره‌سازی شخصی شما">}}

<p>این کتابخانه همچنین یک سرور است. یک هندلر برای سرویسی که می‌خواهید ارائه دهید ثبت کنید، به گوش کردن بپردازید و برنامه شما به یک گره DICOM تبدیل می‌شود که یک مدالیته یا PACS دیگر می‌تواند به آن ارسال کند.</p>

<div class="codeblock" id="code">
 <h3>پذیرش تصاویر ورودی - C#</h3>
 <pre><code class="cs">DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Any, 11112)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.SecondaryCaptureImageStorage,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddSingletonCStoreHandler(new StoreHandler())
    .Build();

await server.StartAsync(CancellationToken.None);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>هندلری که ورودی‌ها را ذخیره می‌کند - C#</h3>
 <pre><code class="cs">public sealed class StoreHandler : ICStoreRequestHandler
{
    public ValueTask&lt;CStoreResponse&gt; Handle(CStoreRequest request, CancellationToken cancellationToken)
    {
        // Write what arrived, then answer Success
        new DicomFile(request.Dataset).Save($"{request.AffectedSopInstanceUid}.dcm");

        CStoreResponse response = new();
        response.Command.AddOrUpdate(Tag.Status, (ushort)0x0000);
        return ValueTask.FromResult(response);
    }
}</code></pre>
</div>

<p>کاری که هندلر با مجموعه داده انجام می‌دهد، تصمیم شماست: آن را بر روی دیسک بنویسید، در صف بگذارید، ابتدا با <a href="/medical/net/anonymization/">API ناشناس‌سازی</a> ناشناس کنید، یا به سینتکس بایگانی تبدیل کنید.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="موارد دیگر که API شبکه پوشش می‌دهد">}}

<p>سه سرویس فوق رایج‌ترین‌ها هستند. بقیه‌ٔ DIMSE نیز موجود است:</p>

<ul>
<li>دریافت: C-MOVE و C-GET، با شمارنده‌های عملیات فرعی که در پاسخ‌ها گزارش می‌شوند.</li>
<li>سرویس‌های N: N-CREATE، N-SET، N-GET، N-ACTION، N-DELETE و N-EVENT-REPORT، که پایه‌گذار تعهد ذخیره‌سازی و MPPS هستند.</li>
<li>کنترل ارتباط: زمینه‌های ارائه، نقش‌های کلاس سرویس، مذاکرهٔ گسترش‌یافته، پنجرهٔ عملیات‌های ناهمزمان، مذاکرهٔ هویت کاربر، و قلاب سیاست که می‌تواند یک ارتباط را با فراخوانی عنوان AE رد کند.</li>
<li>TLS در هر دو طرف، از طریق <code>TlsInitiatorAuthenticator</code> و <code>TlsAcceptorAuthenticator</code>، با اعتبارسنجی گواهی‌نامهٔ خودتان در صورت نیاز.</li>
<li>زمان‌برهای (Timeout) برای هر مرحله، از اتصال TCP تا آزادسازی، و اعلان‌ها برای چرخهٔ حیات ارتباط تا یک گرهٔ طولانی‌مدت بتواند آنچه رخ داد را لاگ کند.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">راهنمای شبکه‌سازی DICOM</a> تمام گزینه‌ها و تمام هندلرها را مستند می‌کند.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="خالص .NET، از سوکت تا داده‌های پیکسل">}}

<p>تمام مطالب در این صفحه کد مدیریت‌شده‌ای از همان بسته است که فایل‌ها را می‌خواند و می‌نویسد. ارتباط، کدک‌ها و تجزیه‌گر از یک کتابخانه می‌آیند، بنابراین یک مطالعهٔ دریافت‌شده از طریق شبکه می‌تواند ناشناس، تبدیل‌شده یا سریال‌سازی شود بدون اینکه از فرایند خارج شود و بدون وابستگی بومی در هیچ‌جای زنجیره.</p>

<p>صفحات مرتبط: <a href="/medical/net/dicom-transfer-syntax-conversion/">تبدیل سینتکس انتقال</a> برای آنچه باید ارسال شود، <a href="/medical/net/anonymization/">ناشناس‌سازی</a> برای آنچه ابتدا باید حذف شود، و <a href="/medical/net/dicom-tags/">برچسب‌های DICOM</a> برای خواندن آنچه رسید.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="منابع آموزشی" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="مستندات" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="راهنمای توسعه‌دهنده" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="مرجع‌های API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="پشتیبانی محصول" tabId="support" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی رایگان" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="پشتیبانی پولی" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="وبلاگ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="چرا Aspose.Medical برای .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="لیست مشتریان" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="داستان‌های موفقیت" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
