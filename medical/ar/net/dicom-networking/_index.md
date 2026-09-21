---
title: شبكات DICOM في C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: قم بربط تطبيق .NET الخاص بك بـ PACS. تحقق باستخدام C-ECHO، أرسل الصور باستخدام C-STORE، استعلم باستخدام C-FIND، وتلقَ الصور عبر SCP الخاص بك. عميل وخادم DIMSE مكتوبين بلغة C# النقية.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="شبكات DICOM في .NET C#" h2="تواصل مع PACS من تطبيقك الخاص: C-ECHO، C-STORE، C-FIND، C-MOVE و C-GET، كعميل وكخادم، باستخدام C# المُدار دون الحاجة لتثبيت أي شيء على الجهاز." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="اتصل بـ PACS من خلال الكود الخاص بك">}}

<p>قراءة ملفات DICOM هي النصف السهل من التصوير الطبي. في اللحظة التي يحتاج فيها تطبيقك للعمل مع نظام مستشفى حقيقي، يجب أن يتحدث بروتوكول DIMSE: افتح ارتباطًا مع PACS، أرسل الصور، استعلم عن الدراسات المتوفرة، وأجب عندما يرسل نظام آخر شيئًا مرة أخرى.</p>

<p><strong>Aspose.Medical for .NET</strong> توفر هذا البروتوكول كجزء من المكتبة. <code>Aspose.Medical.Dicom.Network</code> يمنحك عميل DIMSE وخادم DIMSE، كلاهما مكتوب بلغة C# المُدار. لا توجد مجموعة أدوات أصلية للتثبيت، ولا خدمة لتكوينها ولا شيء خاص بنظام تشغيل، لذا يمكن تشغيل نفس الكود على Windows وLinux وفي حاوية.</p>

<p>ثلاثة أشياء تغطي معظم عمليات التكامل، وكل واحدة منها تتطلب بضع أسطر فقط: تحقق من الرابط باستخدام C-ECHO، ادفع الصور باستخدام C-STORE، واعثر على ما هو على الجانب الآخر باستخدام C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ابدأ بـ C-ECHO">}}

<p>C-ECHO هو اختبار الاتصال في DICOM. يثبت أن المضيف، المنفذ، وعنوانَي AE صحيحان قبل إلقاء أي إشارة خطأ على شيء آخر. أنشئ عميلًا مرة واحدة، زوده بمعالج يراقب الرد، ثم أرسل الطلب.</p>

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

<p>الحالة <code>0x0000</code> تعني نجاح. يتم تجميع الطلبات في قائمة ثم إرسالها على ارتباط واحد، لذا لا يفتح دفعة العمل اتصالًا لكل عنصر.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="إرسال الصور باستخدام C-STORE">}}

<p>C-STORE هو ما تفعله التطبيقات بعد إنشاء أو استلام صورة: تدفع النسخة إلى الأرشيف. ضع طلبًا واحدًا لكل نسخة في قائمة الانتظار وأرسلها معًا.</p>

<div class="codeblock" id="code">
 <h3>إرسال ملف DICOM إلى PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>سياقات العرض التي تقترحها تحدد ما سيقبله الأرشيف. إذا كان يحتاج إلى صيغة مضغوطة، قم بالتحويل قبل الإرسال، كما توضح صفحة <a href="/medical/net/dicom-transfer-syntax-conversion/">تحويل صيغ النقل</a>، أو أدرج البدائل في <code>AdditionalTransferSyntaxes</code> ودع التفاوض يختار واحدة.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="البحث عن الدراسات باستخدام C-FIND">}}

<p>C-FIND يجيب على السؤال "ما الذي يحتويه الأرشيف". تصل النتائج واحدة تلو الأخرى، كلٌ بمجموعة بيانات مُعرف خاصة به، وتغلق الاستجابة النهائية الاستعلام.</p>

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

<p>نفس المصنع يبني مستويات الاستعلام الأخرى: <code>CreatePatientQuery</code>، <code>CreateSeriesQuery</code> و<code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> ينشئ استعلام قائمة عمل للجهاز، وهو الاستعلام الذي يطلبه الجهاز قبل الفحص.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="استلام الصور: SCP التخزين الخاص بك">}}

<p>المكتبة تعمل كخادم أيضًا. سجّل معالجًا للخدمة التي تريد تقديمها، ابدأ الاستماع، وسيصبح تطبيقك عقدة DICOM يمكن للجهاز أو PACS آخر الإرسال إليها.</p>

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

<p>ما يفعله المعالج ببيانات المجموعة هو قرارك: احفظها على القرص، ضعها في قائمة انتظار، قم بتجريد هويتها أولًا باستخدام <a href="/medical/net/anonymization/">واجهة برمجة تطبيقات التجريد</a>، أو حولها إلى صيغة الأرشيف.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ما الذي تغطيه واجهة برمجة تطبيقات الشبكات أيضًا">}}

<p>الخدمات الثلاثة المذكورة أعلاه هي الخدمات الشائعة. بقية بروتوكول DIMSE متوفر أيضًا:</p>

<ul>
<li>استرجاع: C-MOVE و C-GET، مع عدادات العمليات الفرعية المبلغة في الردود.</li>
<li>خدمات N: N-CREATE، N-SET، N-GET، N-ACTION، N-DELETE و N-EVENT-REPORT، والتي تُبنى عليها التزام التخزين و MPPS.</li>
<li>التحكم في الارتباط: سياقات العرض، أدوار فئة الخدمة، التفاوض الموسع، نافذة العمليات غير المتزامنة، التفاوض على هوية المستخدم، وخط ربط سياسات يمكنه رفض الارتباط عن طريق استدعاء عنوان AE.</li>
<li>TLS على الجانبين، عبر <code>TlsInitiatorAuthenticator</code> و<code>TlsAcceptorAuthenticator</code>, مع التحقق من الشهادة الخاصة بك إذا احتجت ذلك.</li>
<li>مهلات لكل مرحلة، من اتصال TCP إلى الإغلاق، وإشعارات لدورة حياة الارتباط حتى يتمكن العقدة طويلة التشغيل من تسجيل ما حدث.</li>
</ul>

<p>الدليل <a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">دليل شبكات DICOM</a> يوثق كل خيار وكل معالج.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2=".NET نقيًا، من المقبس إلى بيانات البكسل">}}

<p>كل ما في هذه الصفحة هو كود مُدار من نفس الحزمة التي تقرأ وتكتب الملفات. الارتباط، وبرمجيات الترميز، والمحلل تأتي من مكتبة واحدة، لذا يمكن أن تُجرد (anonymized) أو تُحوّل (transcoded) أو تُسلسِل (serialized) الدراسة المستلمة عبر الشبكة دون مغادرة العملية ودون أي اعتماد أصلي في أي جزء من السلسلة.</p>

<p>صفحات ذات صلة: <a href="/medical/net/dicom-transfer-syntax-conversion/">تحويل صيغ النقل</a> لتحديد ما يُرسل، <a href="/medical/net/anonymization/">التجريد</a> لتحديد ما يُزيل أولًا، و<a href="/medical/net/dicom-tags/">علامات DICOM</a> لقراءة ما وصل.</p>

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
