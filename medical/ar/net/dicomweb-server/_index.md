---
title: خادم DICOMweb بلغة C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: قم بإنشاء خادم DICOMweb أو PACS خاص بك بلغة C#. يطبق Aspose.Medical بروتوكولات QIDO-RS، WADO-RS، STOW-RS وخدمات قوائم العمل على ASP.NET Core؛ أنت تقوم بربط التخزين.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="خادم DICOMweb لـ .NET C#" h2="أضف نقاط النهاية QIDO-RS، WADO-RS وSTOW-RS إلى خدمة ASP.NET Core. المكتبة تتعامل مع البروتوكول؛ رمزك يحدد مكان تخزين الدراسات." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="خادم DICOMweb الخاص بك بلغة C#">}}

<p>يجب على نظام PACS أو VNA أو أرشيف سحابي أو منصة بحثية أن تستجيب لطلبات DICOMweb: عمليات البحث QIDO-RS، التحميلات WADO-RS والرفع STOW-RS، وغالبًا خدمات قوائم العمل أيضًا. كل منها يمتلك مساراته، وأنواع الوسائط، وأجسام multipart، وقواعد المطابقة، ورموز الحالة الخاصة به.</p>

<p><strong>Aspose.Medical for .NET</strong> ينفّذ جانب هذا البروتوكول منذ الإصدار 26.10. يقرأ كل طلب، يتحقق منه وفقًا للمعايير، يطابق مفاتيح البحث وفق قواعد DICOM، يقسّم النتائج إلى صفحات، يحوّل صيغ النقل ويكتب الاستجابة. يقوم رمزك بما لا يستطيع القيام به سوى أنت: تخزين نسخة، العثور على مرشَّحين للبحث وإرجاع الملفات المخزنة. عندئذٍ تعمل عارضات الصور، وأجهزة التصوير، والأرشيفات الأخرى مع خادمك كما تعمل مع أي خادم DICOMweb آخر، وكذلك <a href="/medical/net/dicomweb-client/">عميل DICOMweb</a> نفسه في المكتبة.</p>

<p>الخادم مُدار بلغة C#. يعمل في أي مكان يعمل فيه ASP.NET Core: على Windows، على Linux، وداخل حاوية.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="خادم كامل في ASP.NET Core">}}

<p>قم بتثبيت حزمة <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. فهي تجلب Aspose.Medical وتربط خدمات DICOMweb بحقن التبعيات وتوجيه النقاط النهاية. هذا الملف <code>Program.cs</code> هو خادم Studies يعمل؛ الفئتان اللتان يسجلهما موضحة أدناه.</p>

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

<p>كل مسار Studies الآن يستجيب تحت <code>/dicomweb</code>: <code>/dicomweb/studies</code> للبحث والرفع، <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> للاسترجاع، وكذلك المسارات للبيانات الوصفية، الإطارات، البيانات الضخمة والصور المُولّدة. وثيقة القدرات عند جذر الخدمة تسرد بالضبط المعاملات التي يدعمها الخادم.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ربط التخزين الخاص بك">}}

<p>خدمة Studies تحتاج إلى واجهة واحدة بثلاث طرق: تخزين نسخة، إرجاع الملفات المخزنة، وإرجاع مرشحي البحث. هذا التنفيذ يحتفظ بكل نسخة كملف داخل مجلد لكل دراسة وسلسلة.</p>

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

<p>المكتبة تتحقق من معرِّفات UID في كل مسار طلب وفي كل نسخة مرفوعة قبل استدعاء المزود. تُبلغ عن نسخة بدون UID صالح كفاشلة ولا تصل أبدًا إلى <code>StoreAsync</code>.</p>

<p>المجلد يكفي لتجربة الخادم. الأرشيف في بيئة الإنتاج يحتفظ بنفس الثلاث طرق ويضع مخزن كائنات خلف <code>StoreAsync</code> و<code>RetrieveAsync</code> ومؤشر قاعدة بيانات خلف <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="البحث: المكتبة تطبق قواعد مطابقة DICOM">}}

<p>مطابقة QIDO-RS هي أكثر من مجرد مقارنة سلاسل نصية: أحرف البدل، نطاقات التاريخ والوقت، قوائم UID، أسماء أشخاص غير دقيقة، قيم فارغة ومفاتيح داخل المتتاليات. المزود لا ينفّذ أيًا من ذلك. هو يُعيد المرشَّحين بترتيب ثابت، وقد يُعيد أكثر من المطابقة. المكتبة تطبق مطابقة DICOM الدقيقة على كل مرشح، تحتفظ بالحقول المطلوبة، وتطبق <code>offset</code> و<code>limit</code>. استجابة واحدة تحتوي على أقصى 1000 نتيجة افتراضيًا، ويمكن لـ <code>DicomWebStandardServiceOptions</code> تعديل هذا الحد.</p>

<p>للحفاظ على سرعة البحث في أرشيف كبير، مرّر المفاتيح الانتقائية إلى قاعدة البيانات. <code>search.Plan.Filter</code> يحتوي على مفاتيح البحث المجمّعة، مع السمة، نوع المطابقة، وقيم كل منها:</p>

<div class="codeblock" id="code">
 <h3>قراءة معرف مريض (Patient ID) دقيق من بحث - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>قد يُعيد استعلام قاعدة البيانات المبني بهذه الطريقة صفوفًا إضافية، مثلاً عندما يتجاهل مفتاحًا لا يستطيع ترجمته. المطابقة النهائية في المكتبة تُزيلها. لا يجب حذف المطابقات الصحيحة، لأن المكتبة لا تستطيع إعادتها.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="التخزين: نتيجة لكل نسخة">}}

<p>يمكن لطلب STOW-RS أن يحمل مئات النسخ. المكتبة تُحلل جسم multipart وتستدعي <code>StoreAsync</code> لكل نسخة على حدة، لذا لا يتم حفظ كامل الرفع في الذاكرة مرة واحدة. لكل نسخة يُرجع المزود <code>Stored</code>، <code>Warning</code> لنسخة مخزنة مع تغييرات، أو <code>Failed</code> مع رمز سبب DICOM اختياري.</p>

<p>المكتبة تُنشئ تقرير STOW-RS من هذه النتائج وتختار الحالة: 200 عندما تُخزن كل نسخة، 202 عندما تكون بعض النسخ تحذيرات أو فاشلة، 409 عندما تفشل جميع النسخ. تُرفض النسخ في طلب دراسة واحدة مسماة تنتمي إلى دراسة أخرى قبل استدعاء المزود.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الاسترجاع: صيغ النقل، الإطارات والصور المُولّدة">}}

<p>المزود يُعيد الملفات المخزنة، والمكتبة تُقدمها بالشكل الذي طلبه العميل. يمكن لعميل WADO-RS طلب صِفة نقل في رأس <code>Accept</code>، وتقوم المكتبة بتحويل كل ملف قبل إرساله، باستخدام مشفراتها الخاصة، بما في ذلك HTJ2K وJPEG XL. البيانات الوصفية بصيغة DICOM JSON أو XML، البيانات الضخمة والإطارات المفردة تأتي من نفس المزود دون الحاجة إلى كود إضافي.</p>

<p>الصور المُولّدة تحتاج إلى فئة أخرى، لأنك وحدك تقرّر أي الصور يمكن أن ينتجها الخادم. هذه الفئة توفر JPEG للنسخ المُولّدة، الإطارات المُولّدة والصور المصغرة، وتقوم المكتبة بإنشائها:</p>

<div class="codeblock" id="code">
 <h3>تقديم صور JPEG مُولّدة وصور مصغرة - C#</h3>
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

<p>المُولد المدمج ينتج JPEG من نسخة ذات إطار واحد أو من الإطارات التي يحددها الطلب. نسخة متعددة الإطارات مُولّدة ككل، صيغ صور أخرى، ومعلمات viewport، window وICC profile تحتاج إلى **your own** <code>RetrieveAsync</code> في هذه الفئة.</p>

<p>يُعالج الصور المصغرة بعناية لحماية خصوصية المريض. المُولد المدمج يُنشئ صورة مصغرة فقط من نسخة تكون سمتي Burned In Annotation وRecognizable Visual Features فيها <code>NO</code>، مما يعني أن الصورة نفسها تُعلن أنها لا تُظهر بيانات المريض. العديد من الصور لا تحمل هذه السمات، وبالتالي يرد الخادم بـ 406. إذا كان أرشيفك يحتاج إلى صور مصغرة لمثل هذه الصور، فأنشئها في <code>RetrieveAsync</code> الخاص بك وتأكد من إخفاء أي بيانات مريض.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="قوائم العمل، خطوات الإجراء وغيرها من الخدمات">}}

<p>الخدمات الأخرى لـ DICOMweb تتبع النموذج نفسه: سجِّل الخدمة، نفّذ واجهة التخزين الخاصة بها.</p>

<ul>
<li>قوائم عمل UPS-RS باستخدام <code>AddStandardWorklistService</code>: إنشاء، بحث وتحديث عناصر العمل وتغيير حالتها، مع فحص قواعد الحالة بواسطة المكتبة وإرسال إشعارات الأحداث عبر WebSocket.</li>
<li>قائمة عمل الأجهزة (MWL-RS) باستخدام <code>AddStandardModalityWorklistService</code>، بحيث يمكن للأجهزة الاستعلام عن خطوات الإجراءات المجدولة في نظام RIS الخاص بك.</li>
<li>خطوات الإجراءات المنفذة للأجهزة (MPPS-RS)، التزام التخزين والنسخ غير المتعلقة بالمريض مثل لوحات اللون وبروتوكولات التعليق.</li>
<li>استرجاع WADO-URI للعارضات القديمة، يُقدَّم بواسطة خدمة Studies.</li>
</ul>

<p>عندما لا يتوافق سلوك المعاملة القياسي مع متطلباتك، سجِّل معالجك الخاص لها باستخدام <code>AddHandler</code>. لا تزال المكتبة تقوم بتوجيه الطلب والتحقق منه، وتبقى المعاملات الأخرى تحت معالجاتها القياسية.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الأمان والاستضافة يبقيان بيدك">}}

<p>نقاط النهاية DICOMweb هي نقاط نهاية عادية في ASP.NET Core. يتم تكوين المصادقة، التفويض، CORS، TLS، رؤوس الإحالة وحدود حجم الطلب بنفس الطريقة التي تُطبق على باقي خدمتك:</p>

<div class="codeblock" id="code">
 <h3>طلب مستخدم مصدَّق - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>جزء البروتوكول لا يعتمد على ASP.NET Core. <code>DicomWebApplication</code> يحول <code>DicomWebRequest</code> إلى استجابة، وبالتالي يمكن تشغيل نفس الخدمات خلف خادم HTTP آخر أو في اختبار دون شبكة.</p>

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

<p><a href="/medical/net/dicomweb-client/">عميل DICOMweb</a> الخاص بـ Aspose.Medical يختبر خادمك من خلال اختبار وحدة أو برنامج وحدة تحكم، وكل ملف يتلقاه الخادم هو <code>DicomFile</code> عادي يمكن إجهاله، تحويل ترميزه أو توليده في نفس العملية.</p>

<p>دليل مطوري <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb</a> يغطي كل خدمة وواجهة تخزين، و<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">دليل ASP.NET Core</a> يغطي تعيين نقاط النهاية وWebSockets.</p>

<p>صفحات ذات صلة: <a href="/medical/net/dicom-networking/">شبكة DICOM</a> للأنظمة التي ترسل الدراسات عبر DIMSE بدلاً من HTTP، <a href="/medical/net/anonymization/">إجهال</a> الدراسات الواردة، <a href="/medical/net/dicom-transfer-syntax-conversion/">تحويل صيغ النقل</a>، و<a href="/medical/net/dicom-to-json/">DICOM إلى JSON</a> للتنسيق الذي تستخدمه نتائج QIDO-RS.</p>

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
{{< blocks/products/pf/slr-element name="مدونة" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="لماذا Aspose.Medical لـ .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="قائمة العملاء" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="قصص نجاح" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
