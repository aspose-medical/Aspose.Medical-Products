---
title: خادم DICOMweb بلغة C# .NET - QIDO-RS، WADO-RS، STOW-RS | Aspose.Medical
weight: 9200

description: قم بإنشاء خادم DICOMweb أو PACS خاص بك بلغة C#. توفر Aspose.Medical تنفيذ QIDO-RS، WADO-RS، STOW-RS وخدمات قائمة العمل على ASP.NET Core؛ أنت توصل التخزين.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="خادم DICOMweb لـ .NET C#" h2="أضف نقاط النهاية QIDO-RS، WADO-RS وSTOW-RS إلى خدمة ASP.NET Core. المكتبة تتعامل مع البروتوكول؛ كودك يحدد أين يتم تخزين الدراسات." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="خادم DICOMweb الخاص بك بلغة C#">}}

<p>يجب على نظام PACS أو VNA أو أرشيف سحابي أو منصة بحث أن يستجيب لطلبات DICOMweb: عمليات بحث QIDO-RS، تنزيلات WADO-RS، تحميلات STOW-RS، وغالبًا خدمات قائمة العمل أيضًا. كل منها يمتلك مساراته، وأنواع وسائطه، وأجسام متعددة الأجزاء، وقواعد المطابقة، ورموز الحالة الخاصة به.</p>

<p><strong>Aspose.Medical for .NET</strong> تنفّذ جانب هذا البروتوكول منذ الإصدار 26.10. تقوم بقراءة كل طلب، تتحقق منه وفقًا للمعيار، تطابق مفاتيح البحث وفق قواعد DICOM، تقسم النتائج إلى صفحات، تحول صيغ النقل وتكتب الاستجابة. كودك يقوم بما لا يمكن إلا لك فعله: تخزين نسخة، العثور على المرشحين للبحث وإرجاع الملفات المخزنة. ثم يعمل المشاهدون، والأجهزة، والأرشيفات الأخرى مع خادمك كما هو الحال مع أي خادم DICOMweb آخر، وكذلك <a href="/medical/net/dicomweb-client/">عميل DICOMweb</a> الخاص بالمكتبة نفسها.</p>

<p>الخادم مدعوم بلغة C#. يعمل أينما يعمل ASP.NET Core: على Windows، على Linux، وفي حاوية.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="خادم كامل في ASP.NET Core">}}

<p>ثبت الحزمة <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. فإنها تجلب Aspose.Medical معها وتربط خدمات DICOMweb بحقن التبعيات وتوجيه نقاط النهاية. هذا الملف <code>Program.cs</code> هو خادم Studies يعمل؛ الفئتان اللتان يتم تسجيلهما موضحتان أدناه.</p>

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

<p>كل مسار Studies الآن يُجيب تحت <code>/dicomweb</code>: <code>/dicomweb/studies</code> للبحث والتحميل، <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> للاسترجاع، والمسارات الخاصة بالبيانات الوصفية، الإطارات، البيانات الضخمة والصور المرسومة. مستند القدرات عند جذر الخدمة يسرد بدقة المعاملات التي يدعمها الخادم.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="اتصل بالتخزين الخاص بك">}}

<p>تحتاج خدمة Studies إلى واجهة واحدة تحتوي على ثلاث طرق: تخزين نسخة، إرجاع الملفات المخزنة، وإرجاع مرشحي البحث. هذه التنفيذية تحتفظ بكل نسخة كملف في مجلد لكل دراسة وسلسلة.</p>

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

<p>المكتبة تتحقق من معرفات UID في كل مسار طلب وكل نسخة مرفوعة قبل استدعاء المزود. تُبلغ عن نسخة بدون UID صالحة كفاشلة ولا تصل أبدًا إلى <code>StoreAsync</code>.</p>

<p>المجلد يكفي لتجربة الخادم. الأرشيف في بيئة الإنتاج يحتفظ بنفس الثلاث طرق ويضع مخزن كائنات خلف <code>StoreAsync</code> و<code>RetrieveAsync</code> وفهرس قاعدة بيانات خلف <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="البحث: المكتبة تطبق قواعد المطابقة في DICOM">}}

<p>مطابقة QIDO-RS أكثر من مجرد مقارنة سلاسل نصية: أحرف بدل، نطاقات التاريخ والوقت، قوائم من UID، أسماء أشخاص غير دقيقة، قيم فارغة ومفاتيح داخل التسلسلات. المزود لا ينفذ أيًا من ذلك. إنه يُرجع المرشحين بترتيب ثابت، وقد يُرجع أكثر من المطابقة. المكتبة تطبق مطابقة DICOM الدقيقة لكل مرشح، تحتفظ بالحقول المطلوبة، وتطبق <code>offset</code> و<code>limit</code>. استجابة واحدة تحتوي على حد أقصى 1000 نتيجة افتراضيًا، و<code>DicomWebStandardServiceOptions</code> يغيّر هذا الحد.</p>

<p>للحفاظ على سرعة البحث في أرشيف كبير، مرر المفاتيح الانتقائية إلى قاعدة البيانات. يحتوي <code>search.Plan.Filter</code> على مفاتيح البحث المجمعة، مع السمة، نوع المطابقة، وقيم كل منها:</p>

<div class="codeblock" id="code">
 <h3>قراءة معرف المريض الدقيق من البحث - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>استعلام قاعدة البيانات المبني بهذه الطريقة قد يُرجع صفوفًا إضافية، على سبيل المثال عندما يتجاهل مفتاحًا لا يستطيع ترجمته. المطابقة النهائية في المكتبة تزيلها. لا يجب حذف المطابقات الحقيقية، لأن المكتبة لا تستطيع إعادتها.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="التخزين: نتيجة لكل نسخة">}}

<p>يمكن لطلب STOW-RS أن يحمل مئات النسخ. المكتبة تحلّل الجسم متعدد الأجزاء وتستدعي <code>StoreAsync</code> لكل نسخة على حدة، لذا لا يُحتفظ بالتحميل الكامل في الذاكرة مرة واحدة. لكل نسخة يُرجع المزود <code>Stored</code>، أو <code>Warning</code> لنسخة مخزنة مع تغييرات، أو <code>Failed</code> مع رمز سبب DICOM اختياري.</p>

<p>المكتبة تبني تقرير STOW-RS من هذه النتائج وتحدد الحالة: 200 عندما تُخزن كل نسخة، 202 عندما تكون بعض النسخ لديها تحذيرات أو فشل، 409 عندما يُفشل جميعها. تُرفض النسخ في طلب لدراسة واحدة مسماة تنتمي إلى دراسة أخرى قبل استدعاء المزود.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الاسترجاع: صيغ النقل، الإطارات والصور المرسومة">}}

<p>المزود يُرجع الملفات المخزنة، وتقوم المكتبة بتسليمها بالشكل الذي طلبه العميل. يستطيع عميل WADO-RS طلب صيغة نقل في رأس <code>Accept</code>، وتقوم المكتبة بتحويل كل ملف قبل إرساله، مع الترميزات الخاصة بها، بما في ذلك HTJ2K وJPEG XL. البيانات الوصفية في DICOM JSON أو XML، البيانات الضخمة وإطارات مفردة تُستورد من نفس المزود دون أي شيفرة إضافية.</p>

<p>الصور المرسومة تحتاج إلى فئة إضافية، لأنك وحدك تقرر أي الصور يمكن للخادم إنتاجها. هذه الفئة توفر JPEG للنسخ المرسومة، الإطارات المرسومة والصور المصغرة، وتقوم المكتبة برسمها:</p>

<div class="codeblock" id="code">
 <h3>توفير صور JPEG مرسومة وصور مصغرة - C#</h3>
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

<p>المُخرج المدمج ينتج JPEG من نسخة ذات إطار واحد أو من الإطارات التي يختارها الطلب. نسخة متعددة الإطارات مرسومة ككل، صورة أوسع أو أعلى من 65500 بكسل (الصور المصغرة تتجاوز هذه الصور)، صيغ صور أخرى، ومعلمات منطقة العرض، النافذة وملف تعريف ICC تحتاج إلى <code>RetrieveAsync</code> الخاص بك في هذه الفئة.</p>

<p>تُعامل الصور المصغرة بحرص لحماية خصوصية المريض. المُخرج المدمج يُنشئ صورة مصغرة فقط من نسخة يكون فيها كل من سمتي Burned In Annotation وRecognizable Visual Features كلاهما <code>NO</code>، مما يعني أن الصورة نفسها تُعلن أنها لا تُظهر بيانات مريض. العديد من الصور لا تحمل هذه السمات، وبالتالي يرد الخادم بـ 406. إذا كان أرشيفك يحتاج إلى صور مصغرة لمثل هذه الصور، أنشئها في <code>RetrieveAsync</code> الخاص بك وتأكد من إخفاء أي بيانات مريض.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="قوائم العمل، خطوات الإجراءات وغيرها من الخدمات">}}

<p>الخدمات الأخرى لـ DICOMweb تتبع نفس النموذج: تسجيل الخدمة، تنفيذ واجهة التخزين الخاصة بها.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> قوائم العمل مع <code>AddStandardWorklistService</code>: إنشاء، بحث وتحديث عناصر العمل وتغيير حالتها، مع فحص قواعد الحالة بواسطة المكتبة وإرسال إشعارات الأحداث عبر WebSocket.</li>
<li><a href="/medical/net/modality-worklist/">قائمة عمل الجهاز</a> (MWL-RS) مع <code>AddStandardModalityWorklistService</code>، بحيث يمكن للأجهزة استعلام خطوات الإجراءات المجدولة في نظام RIS الخاص بك.</li>
<li>خطوات الإجراءات المنفذة بواسطة الجهاز (MPPS-RS)، التزام التخزين والنسخ غير المتعلقة بالمريض مثل لوحات الألوان وبروتوكولات التعليق.</li>
<li>استرجاع WADO-URI للمشاهدات القديمة، تُقدم عبر خدمة Studies.</li>
</ul>

<p>عندما لا يتناسب سلوك المعاملة القياسي مع احتياجاتك، سجل معالجك الخاص لها باستخدام <code>AddHandler</code>. لا تزال المكتبة توجيه وتفحص الطلب، وتبقى المعاملات الأخرى تحتفظ بمعالجاتها القياسية.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الأمان والاستضافة تبقى تحت سيطرتك">}}

<p>نقاط النهاية لـ DICOMweb هي نقاط نهاية عادية في ASP.NET Core. يتم تكوين المصادقة، التفويض، CORS، TLS، رؤوس التحويل وحدود حجم الطلب بنفس الطريقة كما لبقية خدمتك:</p>

<div class="codeblock" id="code">
 <h3>يتطلب مستخدمًا مُوثقًا - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>جزء البروتوكول لا يعتمد على ASP.NET Core. <code>DicomWebApplication</code> يحول <code>DicomWebRequest</code> إلى استجابة، لذا يمكن تشغيل الخدمات نفسها خلف خادم HTTP آخر أو في اختبار بدون شبكة.</p>

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

<p><a href="/medical/net/dicomweb-client/">عميل DICOMweb</a> الخاص بـ Aspose.Medical يختبر خادمك من خلال اختبار وحدة أو برنامج وحدة تحكم، وكل ملف يستقبله الخادم هو <code>DicomFile</code> عادي يمكن تشفيره تجريديًا، أو تحويل صيغته أو رسمه في نفس العملية.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">دليل مطور DICOMweb</a> يغطي كل خدمة وواجهة تخزين، و<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">دليل ASP.NET Core</a> يغطي تخصيص نقاط النهاية وWebSockets.</p>

<p>صفحات ذات صلة: <a href="/medical/net/dicom-networking/">شبكات DICOM</a> للأنظمة التي تُرسل الدراسات عبر DIMSE بدلاً من HTTP، <a href="/medical/net/anonymization/">التجريد</a> للدراسات الواردة، <a href="/medical/net/dicom-transfer-syntax-conversion/">تحويل صيغ النقل</a>، و<a href="/medical/net/dicom-to-json/">DICOM إلى JSON</a> للصيغة التي تستخدمها نتائج QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="الوثائق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="دليل المطور" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="مراجع API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="دعم المنتج" tabId="support" >}}
{{< blocks/products/pf/slr-element name="دعم مجاني" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="دعم مدفوع" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="المدونة" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="لماذا Aspose.Medical لـ .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="قائمة العملاء" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="قصص نجاح" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
