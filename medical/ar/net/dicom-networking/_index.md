---
title: شبكة DICOM في C# .NET - C-ECHO، C-STORE، C-FIND | Aspose.Medical
weight: 9000

description: قم بربط تطبيق .NET الخاص بك بنظام PACS. تحقق باستخدام C-ECHO، أرسل الصور باستخدام C-STORE، استعلام باستخدام C-FIND، وتلقَ الصور عبر SCP الخاص بك. عميل وخادم DIMSE مكتوبين بلغة C# النقية.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="شبكة DICOM في .NET C#" h2="تواصل مع نظام PACS من تطبيقك الخاص: C-ECHO، C-STORE، C-FIND، C-MOVE و C-GET، كعميل وخادم، باستخدام C# المدار دون الحاجة لتثبيت أي شيء على الجهاز." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="الاتصال بنظام PACS من خلال الكود الخاص بك">}}

<p>قراءة ملفات DICOM هي النصف السهل من التصوير الطبي. في اللحظة التي يحتاج فيها تطبيقك للعمل مع نظام مستشفى حقيقي، يجب أن يتحدث بروتوكول DIMSE: فتح ارتباط مع نظام PACS، إرسال الصور، الاستفسار عن الدراسات المتوفرة، والرد عندما يرسل نظام آخر شيئًا.</p>

<p><strong>Aspose.Medical for .NET</strong> يوفر هذا البروتوكول كجزء من المكتبة. <code>Aspose.Medical.Dicom.Network</code> يمنحك عميل DIMSE وخادم DIMSE، كلاهما مكتوب بلغة C# المدار. لا يوجد مجموعة أدوات أصلية للتثبيت، ولا خدمة للتكوين، ولا شيء خاص بمنصة معينة، لذا يعمل نفس الكود على Windows وLinux وفي حاوية.</p>

<p>ثلاثة أشياء تغطي معظم عمليات التكامل، وكل واحدة منها تتطلب بضع أسطر من الشيفرة: التحقق من الاتصال باستخدام C-ECHO، دفع الصور باستخدام C-STORE، والبحث عما هو على الجانب الآخر باستخدام C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ابدأ بـ C-ECHO">}}

<p>C-ECHO هو اختبار الاتصال في DICOM. يثبت أن المضيف، المنفذ، وعنوانَي AE صحيحان قبل إلقاء أي خطأ. قم بإنشاء عميل مرة واحدة، أعطه معالجًا يراقب الرد، ثم أرسل الطلب.</p>

<div class="codeblock" id="code">
 <h3>تحقق من الاتصال باستخدام C-ECHO - C#</h3>
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

<p>الحالة <code>0x0000</code> تعني النجاح. يتم تجميع الطلبات في طابور ثم إرسالها عبر ارتباط واحد، لذا لا يتم فتح اتصال لكل عنصر في دفعة العمل.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="إرسال الصور باستخدام C-STORE">}}

<p>C-STORE هو ما تقوم به التطبيق بعد إنشاء أو استلام صورة: يدفع النسخة إلى الأرشيف. ضع طلبًا واحدًا لكل نسخة في الطابور وأرسلها معًا.</p>

<div class="codeblock" id="code">
 <h3>إرسال ملف DICOM إلى نظام PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>سياقات العرض التي تقترحها تحدد ما سيقبل الأرشيف. إذا كان يريد صيغة مضغوطة، قم بترميزها قبل الإرسال، كما توضح صفحة <a href="/medical/net/dicom-transfer-syntax-conversion/">تحويل صِيغة النقل</a>، أو أدرج البدائل في <code>AdditionalTransferSyntaxes</code> ودع التفاوض يختار واحدة.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="البحث عن الدراسات باستخدام C-FIND">}}

<p>C-FIND يجيب على السؤال "ما يمكن أن يوفره الأرشيف". تصل النتائج واحدة تلو الأخرى، كل منها مع مجموعة بيانات معرف خاصة به، والرد النهائي يغلق الاستعلام.</p>

<div class="codeblock" id="code">
 <h3>استعلام عن الدراسات حسب المريض - C#</h3>
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

<p>نفس المصنع يبني مستويات الاستعلام الأخرى: <code>CreatePatientQuery</code>، <code>CreateSeriesQuery</code> و <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> يبني استعلام قائمة عمل الجهاز، وهو الاستعلام الذي يطلبه الجهاز قبل الفحص؛ صفحة <a href="/medical/net/modality-worklist/">قائمة عمل الجهاز</a> تُظهر ذلك مع MPPS وجانب خادم قائمة العمل.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="استلام الصور: SCP التخزين الخاص بك">}}

<p>المكتبة تعمل أيضًا كخادم. سجِّل معالجًا للخدمة التي تريد تقديمها، ابدأ الاستماع، ويصبح تطبيقك عقدة DICOM يمكن لجهاز تصوير أو نظام PACS آخر الإرسال إليها.</p>

<div class="codeblock" id="code">
 <h3>قبول الصور الواردة - C#</h3>
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
 <h3>المعالج الذي يخزن ما يصل - C#</h3>
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

<p>ما يفعله المعالج مع مجموعة البيانات هو قرارك: حفظها على القرص، وضعها في طابور، إخفاء هويتها أولاً باستخدام <a href="/medical/net/anonymization/">واجهة برمجة إخفاء الهوية</a>، أو ترميزها إلى صيغة الأرشيف.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ما الذي تغطيه واجهة برمجة الشبكات الأخرى">}}

<p>الخدمات الثلاثة أعلاه هي الشائعة. باقي أوامر DIMSE متوفرة أيضًا:</p>

<ul>
<li>الاسترجاع: C-MOVE و C-GET، مع عدّادات العمليات الفرعية المبلغ عنها في الردود.</li>
<li>خدمات N: N-CREATE، N-SET، N-GET، N-ACTION، N-DELETE و N‑EVENT‑REPORT، وهي التي تُبنى عليها التزام التخزين وMPPS.</li>
<li>التحكم في الارتباط: سياقات العرض، أدوار فئة الخدمة، التفاوض الموسع، نافذة العمليات غير المتزامنة، التفاوض على هوية المستخدم، وخطاف سياسة يمكنه رفض الارتباط عبر استدعاء عنوان AE.</li>
<li>TLS على الجانبين، عبر <code>TlsInitiatorAuthenticator</code> و<code>TlsAcceptorAuthenticator</code>، مع التحقق من الشهادة الخاص بك إذا احتجت ذلك.</li>
<li>مهلات لكل مرحلة، من اتصال TCP إلى الإطلاق، وإشعارات لدورة حياة الارتباط حتى يتمكن العقدة طويلة التشغيل من تسجيل ما حدث.</li>
</ul>

<p>دليل <a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">شبكة DICOM</a> يوثق كل خيار وكل معالج.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2=".NET النقية، من المقبس إلى بيانات البكسل">}}

<p>كل ما في هذه الصفحة هو شفرة مدارة من نفس الحزمة التي تقرأ وتكتب الملفات. الارتباط، codecs والمحلل يأتون من مكتبة واحدة، لذلك يمكن إخفاء هوية دراسة تم استلامها عبر الشبكة أو تحويل ترميزها أو تسلسله دون مغادرة العملية ودون أي تبعية أصلية في أي جزء من السلسلة.</p>

<p>الصفحات ذات الصلة: <a href="/medical/net/dicom-transfer-syntax-conversion/">تحويل صِيغة النقل</a> لما يجب إرساله، <a href="/medical/net/anonymization/">إخفاء الهوية</a> لما يجب إزالته أولاً، و<a href="/medical/net/dicom-tags/">وسوم DICOM</a> لقراءة ما وصل.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="الوثائق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="دليل المطور" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="مراجع API" href="https://reference.aspose.com/medical/net/" >}}
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
