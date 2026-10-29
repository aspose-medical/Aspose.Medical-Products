---
title: UPS-RS في C# .NET - عناصر عمل خطوة الإجراء الموحدة لـ DICOMweb | Aspose.Medical
weight: 9400

description: إنشاء، حجز وإكمال عناصر عمل UPS الخاصة بـ DICOM من C#، استقبال تغييرات الحالة عبر WebSocket، أو تشغيل خادم UPS-RS الخاص بك على ASP.NET Core باستخدام Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS لـ .NET C#" h2="توزيع أعمال التصوير بين الأنظمة باستخدام خطوة الإجراء الموحدة DICOM: إنشاء عناصر العمل، السماح للعمال بحجزها وإكمالها، ومتابعة كل تغيير، كعميل أو كخادم." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="قائمة انتظار عمل تتحدث DICOM">}}

<p>يجب على نموذج الذكاء الاصطناعي معالجة كل فحص CT للصدر الجديد. تقوم خدمة التشخيص عن بُعد بتعيين الدراسات للقراء. يتلقى مختبر ثلاثي الأبعاد أوامر المعالجة اللاحقة. كل من هذه يمثل قائمة انتظار مهام بين الأنظمة، وتحدد DICOM واحدة لذلك: خطوة الإجراء الموحدة (UPS)، التي تُقدم عبر HTTP بفضل خدمة UPS-RS من DICOMweb.</p>

<p>يحدد عنصر عمل UPS ما الذي يجب القيام به، لأي مريض وبأي مدخلات. يكون في حالة SCHEDULED عند إنشائه. يقوم العامل بحجزه ويحول حالته إلى IN PROGRESS، ثم ينهيه كـ COMPLETED، أو ينتهي كـ CANCELED. الأنظمة الأخرى تشترك وتستقبل كل تغيير حالة عبر WebSocket، لذا لا أحد يحتاج إلى الاستطلاع.</p>

<p><strong>Aspose.Medical for .NET</strong> تنفّذ كلا الجانبين في C# المدار: عميل مُعَرَّف لأي خادم UPS-RS، والجانب الخادمي لـ ASP.NET Core، حيث تتحقق المكتبة من قواعد UPS وتخزن الكود الخاص بك عناصر العمل.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="جدولة عنصر عمل">}}

<p>النظام الذي يرغب في إنجاز العمل ينشئ عنصر العمل. معرف SOP Instance UID يحددها من تلك اللحظة فصاعدًا؛ التسمية، تسمية قائمة العمل والأولوية تخبر العمال ما هو العنصر.</p>

<div class="codeblock" id="code">
 <h3>إنشاء عنصر عمل UPS - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// What to do, for which patient, and with which input
Dataset input = new();
input.AddOrUpdate(Tag.StudyInstanceUID, "1.2.826.0.1.3680043.10.1.100");

Dataset workitem = new();
workitem.AddOrUpdate(Tag.SOPInstanceUID, Uid.CreateDerivedFromUuid());
workitem.AddOrUpdate(Tag.ProcedureStepState, "SCHEDULED");
workitem.AddOrUpdate(Tag.TransactionUID, string.Empty);
workitem.AddOrUpdate(Tag.InputReadinessState, "READY");
workitem.AddOrUpdate(Tag.ProcedureStepLabel, "Lung nodule detection");
workitem.AddOrUpdate(Tag.WorklistLabel, "AI");
workitem.AddOrUpdate(Tag.ScheduledProcedureStepPriority, "HIGH");
workitem.AddOrUpdate(Tag.PatientName, "DOE^JANE");
workitem.AddOrUpdate(Tag.PatientID, "P001");
workitem.AddOrUpdate(new Sequence(Tag.InputInformationSequence, [input]));

DicomWebClientResult&lt;CreateWorkitemResult&gt; created = await client.Worklist.CreateAsync(workitem);
Console.WriteLine($"Scheduled {created.Value.WorkitemUid}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="احجزه، نفّذ العمل، أكمله">}}

<p>يبحث العامل في قائمة العمل عن ما يمكنه القيام به ويُحجز عنصر عمل عن طريق نقله إلى IN PROGRESS باستخدام معرف المعاملة UID الذي أنشأه. من تلك اللحظة، فقط حامل هذا UID يمكنه تحديث أو إكمال عنصر العمل، وبالتالي لا يعالج عاملان نفس المهمة.</p>

<div class="codeblock" id="code">
 <h3>العثور على عناصر العمل المجدولة، حجزها وإكمالها - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// Scheduled workitems on the AI worklist
DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters =
    [
        new DicomWebQueryParameter("ProcedureStepState", "SCHEDULED"),
        new DicomWebQueryParameter("WorklistLabel", "AI")
    ]
};

List&lt;string&gt; scheduled = [];
await using (DicomWebClientStream&lt;Dataset&gt; found = await client.Worklist.SearchAsync(search))
{
    await foreach (Dataset item in found)
        scheduled.Add(item.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID));
}

foreach (string workitemUid in scheduled)
{
    // Claim the workitem; only the holder of this transaction UID can update or finish it
    string transactionUid = Uid.CreateDerivedFromUuid().Code;
    await client.Worklist.ChangeStateAsync(workitemUid, WorkitemState.InProgress, transactionUid);

    // ... run the job ...

    await client.Worklist.UpdateAsync(workitemUid, transactionUid, Performed());
    await client.Worklist.ChangeStateAsync(workitemUid, WorkitemState.Completed, transactionUid);
}</code></pre>
</div>

<p>يتطلب حالة COMPLETED سجلًا للعمل: من نفّذه، ما الذي تم تنفيذه، وقت البدء والانتهاء، والناتج. يقوم العامل بتخزينه مع تحديث قبل أن يغيّر الحالة.</p>

<div class="codeblock" id="code">
 <h3>بيانات الإجراء المنفذ - C#</h3>
 <pre><code class="cs">// What COMPLETED requires: who did the work, what was done, when, and the output (here none)
public static Dataset Performed()
{
    Dataset performed = new();
    performed.AddOrUpdate(new Sequence(Tag.PerformedStationNameCodeSequence, [Code("AI-NODE-1", "99LOCAL", "AI node 1")]));
    performed.AddOrUpdate(new Sequence(Tag.PerformedWorkitemCodeSequence, [Code("NODULE", "99LOCAL", "Lung nodule detection")]));
    performed.AddOrUpdate(Tag.PerformedProcedureStepStartDateTime, DateTimeOffset.Now.AddMinutes(-2).ToString("yyyyMMddHHmmss"));
    performed.AddOrUpdate(Tag.PerformedProcedureStepEndDateTime, DateTimeOffset.Now.ToString("yyyyMMddHHmmss"));
    performed.AddOrUpdate(new Sequence(Tag.OutputInformationSequence, []));

    Dataset update = new();
    update.AddOrUpdate(new Sequence(Tag.UnifiedProcedureStepPerformedProcedureSequence, [performed]));
    return update;
}

private static Dataset Code(string value, string scheme, string meaning)
{
    Dataset code = new();
    code.AddOrUpdate(Tag.CodeValue, value);
    code.AddOrUpdate(Tag.CodingSchemeDesignator, scheme);
    code.AddOrUpdate(Tag.CodeMeaning, meaning);
    return code;
}</code></pre>
</div>

<p>يطبق خادم مبني بـ Aspose.Medical هذه القواعد. العامل الثاني الذي يحاول حجز عنصر عمل قيد التقدم يحصل على HTTP 409، وكذلك تغيير حالة عنصر مكتمل. طلب COMPLETED بدون بيانات الإجراء المنفذ يحصل على HTTP 400. يُبلغ العميل عن هذه الإجابات كـ <code>DicomWebClientException</code> مع استجابة HTTP.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="متابعة قائمة العمل عبر WebSocket">}}

<p>لوحة معلومات، أو النظام الذي طلب العمل أو عامل ينتظر مهام جديدة يشترك باستخدام عنوان AE الخاص به ويفتح اتصال إشعارات. كل تقرير حدث يصل كمجموعة بيانات تحتوي على UID عنصر العمل وحالته الجديدة.</p>

<div class="codeblock" id="code">
 <h3>استقبال تقارير أحداث UPS - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// Every change on the worklist, pushed over a WebSocket
await client.Worklist.SubscribeWorklistAsync("DASHBOARD");
await using DicomWebNotificationConnection connection = await client.ConnectNotificationsAsync("DASHBOARD");

while (await connection.ReceiveAsync(cancellationToken) is Dataset report)
{
    Console.WriteLine($"{report.GetSingleValueOrDefault(Tag.AffectedSOPInstanceUID, string.Empty)}: " +
                      $"{report.GetSingleValueOrDefault(Tag.ProcedureStepState, string.Empty)}");
}</code></pre>
</div>

<p>إلى جانب قائمة العمل بالكامل، يمكن للعميل الاشتراك في عنصر عمل واحد باستخدام <code>SubscribeWorkitemAsync</code> أو في جزء مُفلتر من قائمة العمل باستخدام <code>SubscribeFilteredWorklistAsync</code>، إذا كان الخادم يدعم الفلاتر. إلغاء الاستقبال يغلق الاتصال؛ افتح اتصالًا جديدًا باستخدام <code>ConnectNotificationsAsync</code> للمتابعة.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="خادم UPS-RS الخاص بك">}}

<p>تضيف الحزمة <code>Aspose.Medical.Dicom.Web.AspNetCore</code> UPS-RS إلى خدمة ASP.NET Core. تتعامل المكتبة مع كل طلب: تتحقق من محتوى عنصر العمل الجديد، وتحولات الحالة، ومعرف المعاملة UID للعامل والبيانات التي يتطلبها COMPLETED، وتطبّق قواعد مطابقة DICOM على عمليات البحث. يخزن الكود الخاص بك عناصر العمل.</p>

<div class="codeblock" id="code">
 <h3>Program.cs لخادم UPS-RS - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>التخزين الخاص بعناصر العمل - C#</h3>
 <pre><code class="cs">// Workitems with a revision each; the library applies the UPS state rules and the search matching
public sealed class WorkitemStore : IDicomWebWorkitemProvider
{
    private readonly ConcurrentDictionary&lt;string, DicomWebVersionedDataset&gt; _items = new();

    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string uid in _items.Keys.Order(StringComparer.Ordinal))
            if (_items.TryGetValue(uid, out DicomWebVersionedDataset? item))
                yield return new Dataset(item.Dataset);
        await Task.CompletedTask;
    }

    public ValueTask&lt;DicomWebVersionedDataset?&gt; GetAsync(string workitemUid, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryGetValue(workitemUid, out DicomWebVersionedDataset? item) ? Copy(item) : null);

    public ValueTask&lt;bool&gt; TryCreateAsync(string workitemUid, Dataset workitem, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryAdd(workitemUid, NewRevision(workitem)));

    // Replaces the workitem only if nobody changed it since it was read
    public ValueTask&lt;bool&gt; TryReplaceAsync(string workitemUid, string expectedRevision, Dataset workitem,
        CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryGetValue(workitemUid, out DicomWebVersionedDataset? current)
                             &amp;&amp; current.Revision == expectedRevision
                             &amp;&amp; _items.TryUpdate(workitemUid, NewRevision(workitem), current));

    private static DicomWebVersionedDataset NewRevision(Dataset dataset) =&gt;
        new() { Dataset = new Dataset(dataset), Revision = Guid.NewGuid().ToString("N") };

    private static DicomWebVersionedDataset Copy(DicomWebVersionedDataset item) =&gt;
        new() { Dataset = new Dataset(item.Dataset), Revision = item.Revision };
}</code></pre>
</div>

<p>كل عنصر عمل مخزن يمتلك نسخة (revision). تقوم الدالة <code>TryReplaceAsync</code> باستبداله فقط عندما تكون النسخة لا تزال هي التي قرأتها المكتبة، لذا لا يمكن لطلبين يغيران نفس عنصر العمل في نفس اللحظة أن يكتبا فوق بعضها؛ المكتبة تعيد المحاولة أو تُبلغ عن تعارض. استبدل القاموس بقاعدة البيانات الخاصة بك واحتفظ بهذا الفحص.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الإشعارات على الخادم">}}

<p>لإرسال تقارير الأحداث، سجّل <code>IDicomWebSubscriptionProvider</code> واستدعِ <code>app.UseWebSockets()</code> قبل <code>MapDicomWeb</code>. يحتفظ الموفر بالاشتراكات وواحدة طابور تقارير الأحداث لكل مشترك، وتزود المكتبة الموفر بتقرير حدث كلما تغير عنصر العمل. كما يخبر الموفر المكتبة بعنوان AE للمكالمَة المصادقة، لأن تقارير الأحداث تسمي النظام الذي غير الحالة.</p>

<p>دليل <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">مزودي سير العمل</a> يحتوي على موفر اشتراك كامل في الذاكرة، و<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">دليل ASP.NET Core</a> يغطي إعدادات WebSocket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS، MWL و MPPS">}}

<p>يغطي UPS ما تقوم به قوائم العمل الخاصة بالوسائط القديمة (Modality Worklist) وMPPS بين نظام RIS والجهاز، ويضيف عمالًا ليسوا وسائط، مع حجز وإشعارات. لا تزال العديد من الوسائط تتحدث فقط MWL وMPPS عبر DIMSE؛ تُظهر <a href="/medical/net/modality-worklist/">صفحة قائمة عمل الوسائط</a> ذلك سير العمل باستخدام نفس المكتبة.</p>

<p>الصفحات ذات الصلة: <a href="/medical/net/dicomweb-client/">عميل DICOMweb</a> لخدمات DICOMweb الأخرى، <a href="/medical/net/dicomweb-server/">خادم DICOMweb</a> لـ QIDO-RS وWADO-RS وSTOW-RS على نفس الخادم، و<a href="/medical/net/dicom-networking/">شبكات DICOM</a> لـ DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="الوثائق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="دليل المطور" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
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
