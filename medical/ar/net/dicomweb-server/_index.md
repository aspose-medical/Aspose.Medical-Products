---
title: خادم DICOMweb بلغة C# .NET - QIDO-RS، WADO-RS، STOW-RS | Aspose.Medical
weight: 9200

description: أنشئ خادم DICOMweb أو نظام PACS الخاص بك باستخدام C#. تقوم Aspose.Medical بتنفيذ QIDO-RS، WADO-RS، STOW-RS وخدمات قوائم العمل على ASP.NET Core؛ أنت تقوم بربط التخزين.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="خادم DICOMweb لـ .NET C#" h2="أضف نقاط النهاية QIDO-RS، WADO-RS وSTOW-RS إلى خدمة ASP.NET Core. المكتبة تتعامل مع البروتوكول؛ يحدد كودك مكان تخزين الدراسات." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="خادم DICOMweb الخاص بك باستخدام C#">}}

<p>يجب على PACS أو VNA أو أرشيف سحابي أو منصة بحث أن تستجيب لطلبات DICOMweb: عمليات البحث QIDO-RS، التحميلات WADO-RS والرفع STOW-RS، وغالبًا خدمات قوائم العمل أيضًا. كل منها يمتلك مساراته، وأنواع الوسائط، وأجسام multipart، وقواعد المطابقة، ورموز الحالة الخاصة به.</p>

<p>تنفذ <strong>Aspose.Medical for .NET</strong> جانب هذا البروتوكول منذ الإصدار 26.10. تقرأ كل طلب، وتتحقق منه وفقًا للمعيار، وتطابق مفاتيح البحث وفقًا لقواعد DICOM، وتصفّح النتائج، وتحول صيغ النقل وتكتب الاستجابة. يقوم كودك بما لا يستطيع أحد غيرك القيام به: تخزين نسخة، العثور على مرشّحات للبحث وإرجاع الملفات المخزنة. يتفاعل المشاهدون، والأنماط، والأنظمة الأرشيفية الأخرى مع خادمك كما لو كان أي خادم DICOMweb آخر، وكذلك <a href="/medical/net/dicomweb-client/">عميل DICOMweb</a> لنفس المكتبة.</p>

<p>الخادم مُدار بلغة C#. يعمل أينما يعمل ASP.NET Core: على Windows، على Linux، وفي حاوية.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="خادم كامل في ASP.NET Core">}}

<p>قم بتثبيت حزمة <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. فهي تجلب Aspose.Medical معها وتربط خدمات DICOMweb بالحقن الاعتمادي وتوجيه نقاط النهاية. هذا الملف <code>Program.cs</code> هو خادم Studies عملي؛ الفئتان اللتان يسجّلهما موضحان أدناه.</p>

<div class="codeblock" id="code">
 <h3>Program.cs لخادم DICOMweb - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>كل مسار Studies الآن يرد تحت <code>/dicomweb</code>: <code>/dicomweb/studies</code> للبحث والرفع، <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> للاسترجاع، والمسارات للبيانات الوصفية، الإطارات، البيانات الضخمة والصور المُصيّرة. مستند القدرات عند جذر الخدمة يسرد بالضبط المعاملات التي يدعمها الخادم.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ربط التخزين الخاص بك">}}

<p>تحتاج خدمة Studies إلى واجهة واحدة تحتوي على ثلاث طرق: تخزين نسخة، إرجاع الملفات المخزنة، وإرجاع مرشّحات البحث. تحتفظ هذه التنفيذية بكل نسخة كملف في مجلد لكل دراسة وسلسلة.</p>

<div class="codeblock" id="code">
 <h3>أرشيف مجلد خلف QIDO-RS، WADO-RS وSTOW-RS - C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>تتحقق المكتبة من معرّفات UID في كل مسار طلب وفي كل نسخة مرفوعة قبل استدعاء الموفر. تُبلغ عن نسخة بدون UID صالحة كفاشلة ولا تصل أبدًا إلى <code>StoreAsync</code>.</p>

<p>المجلد يكفي لتجربة الخادم. يحتفظ أرشيف الإنتاج بنفس الثلاث طرق ويضع مخزن كائنات خلف <code>StoreAsync</code> و<code>RetrieveAsync</code> وفهرس قاعدة بيانات خلف <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="البحث: المكتبة تطبق قواعد المطابقة في DICOM">}}

<p>مطابقة QIDO-RS أكثر من مجرد مقارنة سلاسل نصية: أحرف البدل، نطاقات التاريخ والوقت، قوائم UID، أسماء أشخاص تقريبية، قيم فارغة ومفاتيح داخل التسلسلات. الموفر لا ينفذ أيًا من ذلك. يُرجِع المرشّحات بترتيب ثابت، وقد يُرجِع أكثر من ما يتطابق. تطبق المكتبة المطابقة الدقيقة لـ DICOM على كل مرشح، تحتفظ بالحقول المطلوبة، وتطبّق <code>offset</code> و<code>limit</code>. استجابة واحدة تحتوي كحد أقصى على 1000 نتيجة افتراضيًا، وتقوم <code>DicomWebStandardServiceOptions</code> بتعديل هذا الحد.</p>

<p>للحفاظ على سرعة البحث في أرشيف كبير، مرّر المفاتيح الانتقائية إلى قاعدة البيانات. يحتوي <code>search.Plan.Filter</code> على مفاتيح البحث المجمّعة، مع السمة، ونوع المطابقة، وقيم كل منها:</p>

<div class="codeblock" id="code">
 <h3>قراءة معرف المريض (Patient ID) بدقة من البحث - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>قد تُعيد استعلام قاعدة البيانات المبني بهذه الطريقة صفوفًا إضافية، مثلاً عندما يتجاهل مفتاحًا لا يستطيع ترجمته. تُزيل المطابقة النهائية في المكتبة تلك الصفوف. ولا يُسمح بحذف التطابقات الصحيحة، لأن المكتبة لا تستطيع إضافتها مرة أخرى.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="التخزين: نتيجة لكل نسخة">}}

<p>يمكن لطلب STOW-RS أن يحمل مئات النسخ. تقوم المكتبة بتحليل جسم multipart وتستدعي <code>StoreAsync</code> لكل نسخة على التوالي، لذا لا يُحتفظ بالتحميل بالكامل في الذاكرة دفعة واحدة. لكل نسخة يُرجِع الموفر <code>Stored</code>، أو <code>Warning</code> إذا خُزّنت النسخة مع تغييرات، أو <code>Failed</code> مع رمز سبب DICOM اختياري.</p>

<p>تبني المكتبة تقرير STOW-RS من هذه النتائج وتختار الحالة: 200 عندما تُخزن كل نسخة، 202 عندما تكون بعض النسخ تحذيرات أو فاشلة، 409 عندما تفشل جميعها. تُرفض النسخ في طلب لم دراسة معينة تنتمي إلى دراسة أخرى قبل استدعاء الموفر.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="استرجاع: صيغ النقل، الإطارات، والصور المُصيّرة">}}

<p>يرجع الموفر الملفات المخزنة، وتقوم المكتبة بتسليمها بالشكل الذي طلبه العميل. يمكن لعميل WADO-RS طلب صیغة نقل في رأس <code>Accept</code>، وتقوم المكتبة بتحويل كل ملف قبل إرساله، مع ترميزات خاصة بها، بما فيها HTJ2K وJPEG XL. البيانات الوصفية بصيغة DICOM JSON أو XML، والبيانات الضخمة والإطارات الفردية تأتي من نفس الموفر دون رمز إضافي.</p>

<p>تحتاج الصور المُصيّرة إلى فئة إضافية، لأنك أنت فقط من يقرر أي الصور يمكن للخادم إنتاجها. هذه الفئة توفر JPEG للنسخ المصيّرة، والإطارات المصيّرة، والصوريّات المصغرة، وتقوم المكتبة بصياغتها:</p>

<div class="codeblock" id="code">
 <h3>توفير صور JPEG مُصيّرة وصور مصغرة - C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>المُصيّر المدمج يُنتج JPEG من نسخة ذات إطار واحد أو من الإطارات التي يحددها الطلب. نسخة متعددة الإطارات مُصيّرة ككل، صيغ صور أخرى، ومعلمات المنفذ، النافذة، وملف تعريف ICC تحتاج إلى <code>RetrieveAsync</code> الخاص بك في هذه الفئة.</p>

<p>تُعامل الصور المصغرة بحذر لحماية خصوصية المريض. يُنشئ المُصيّر المدمج مصغّرًا فقط من نسخة يكون كل من خصائص Burned In Annotation وRecognizable Visual Features لها القيمة <code>NO</code>، مما يعني أن الصورة نفسها تُعلن أنها لا تُظهر بيانات المريض. العديد من الصور لا تحمل هذه الخصائص، وبالتالي يرد الخادم بالرمز 406. إذا كان أرشيفك يحتاج إلى مصغّرات لمثل هذه الصور، أنشئها في <code>RetrieveAsync</code> الخاص بك وتأكد من إخفاء أي بيانات مريض.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="قوائم العمل، خطوات الإجراء والخدمات الأخرى">}}

<p>تتبع خدمات DICOMweb الأخرى نفس النموذج: سجِّل الخدمة، ونفّذ واجهة التخزين الخاصة بها.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> قوائم العمل مع <code>AddStandardWorklistService</code>: إنشاء، بحث وتحديث عناصر العمل وتغيير حالتهم، مع فحص قواعد الحالة بواسطة المكتبة وإرسال إشعارات الأحداث عبر WebSocket.</li>
<li>قائمة عمل الأنماط <a href="/medical/net/modality-worklist/">modality worklist</a> (MWL-RS) باستخدام <code>AddStandardModalityWorklistService</code>، حتى تتمكن الأنماط من استعلام خطوات الإجراء المجدولة في نظام RIS الخاص بك.</li>
<li>خطوات الإجراء المنفذة للأنماط (MPPS-RS)، التزام التخزين، والنسخ غير المتعلقة بالمريض مثل لوحات الألوان وبروتوكولات التعليق.</li>
<li>استرجاع WADO-URI للمشاهدات القديمة، يُقدَّم عبر خدمة Studies.</li>
</ul>

<p>عندما لا يتناسب السلوك القياسي لأحد المعاملات، سجِّل معالجك الخاص له باستخدام <code>AddHandler</code>. لا تزال المكتبة تُوجّه وتتحقق من الطلب، وتبقى المعاملات الأخرى تحت معالجاتها القياسية.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الأمان والاستضافة يبقيان بين يديك">}}

<p>نقاط النهاية DICOMweb هي نقاط نهاية عادية في ASP.NET Core. يتم تكوين المصادقة، التفويض، CORS، TLS، رؤوس التوجيه وحجم الطلبات بنفس الطريقة كما في باقي خدمتك:</p>

<div class="codeblock" id="code">
 <h3>طلب مستخدم موثَّق - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>جزء البروتوكول لا يعتمد على ASP.NET Core. يقوم <code>DicomWebApplication</code> بتحويل <code>DicomWebRequest</code> إلى استجابة، لذا يمكن تشغيل نفس الخدمات خلف خادم HTTP آخر أو في اختبار دون شبكة:</p>

<div class="codeblock" id="code">
 <h3>نفس الخادم بدون ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="العميل والخادم من مكتبة واحدة">}}

<p>يختبر <a href="/medical/net/dicomweb-client/">عميل DICOMweb</a> من Aspose.Medical خادمك من خلال اختبار وحدة أو برنامج وحدة تحكم، وكل ملف يتلقاه الخادم هو <code>DicomFile</code> عادي يمكن إجهاض هويته، تحويل صيغته أو صياغته في نفس العملية.</p>

<p>دليل <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb developer guide</a> يغطي كل خدمة وواجهة تخزين، و<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core guide</a> يغطي تعيين نقاط النهاية وWebSockets.</p>

<p>الصفحات ذات الصلة: <a href="/medical/net/dicom-networking/">شبكات DICOM</a> للأنظمة التي ترسل الدراسات عبر DIMSE بدلاً من HTTP، <a href="/medical/net/anonymization/">إجهاض الهوية</a> للدراسات الواردة، <a href="/medical/net/dicom-transfer-syntax-conversion/">تحويل صيغ النقل</a>، و<a href="/medical/net/dicom-to-json/">DICOM إلى JSON</a> للنسق الذي تستخدمه نتائج QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="التوثيق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="دليل المطور" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="مراجع API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="دعم المنتج" tabId="support" >}}
{{< blocks/products/pf/slr-element name="دعم مجاني" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="دعم مدفوع" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="المدونة" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="لماذا Aspose.Medical للـ .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="قائمة العملاء" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="قصص نجاح" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
