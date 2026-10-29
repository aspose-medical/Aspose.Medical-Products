---
title: UPS-RS in C# .NET - DICOMweb 统一过程步骤工作项 | Aspose.Medical
weight: 9400

description: 从 C# 创建、认领并完成 DICOM UPS 工作项，通过 WebSocket 接收状态变化，或使用 Aspose.Medical 在 ASP.NET Core 上运行您自己的 UPS-RS 服务器。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="适用于 .NET C# 的 UPS-RS" h2="使用 DICOM 统一过程步骤在系统之间分配影像工作：创建工作项，让工作者认领并完成它们，并跟踪每一次更改，无论是作为客户端还是服务器。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="一个使用 DICOM 的工作队列">}}

<p>AI 模型必须处理每个新的胸部 CT。远程放射服务将研究分配给阅片医师。三维实验室收到后处理订单。这些都是系统之间的任务队列，DICOM 为此定义了一种机制：统一过程步骤（UPS），通过 DICOMweb 的 UPS-RS 服务以 HTTP 方式提供。</p>

<p>UPS 工作项说明要做什么、针对哪位患者以及使用何种输入。创建时状态为 SCHEDULED。工作者认领后将其转换为 IN PROGRESS，随后完成后状态为 COMPLETED，或以 CANCELED 结束。其他系统订阅后通过 WebSocket 接收每一次状态变化，无需轮询。</p>

<p><strong>Aspose.Medical for .NET</strong> 在托管 C# 中实现了双方：一个针对任意 UPS-RS 服务器的强类型客户端，以及 ASP.NET Core 的服务器端，其中库负责检查 UPS 规则，您的代码负责存储工作项。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="调度工作项">}}

<p>需要执行工作的系统创建工作项。其 SOP 实例 UID 从此用于唯一标识；标签、工作列表标签及优先级告诉工作者它的性质。</p>

<div class="codeblock" id="code">
 <h3>创建 UPS 工作项 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="认领它，完成工作，标记完成">}}

<p>工作者在工作列表中搜索可执行的任务，并通过使用生成的事务 UID 将工作项移动到 IN PROGRESS 来认领它。从此刻起，只有该 UID 的持有者能够更新或完成该工作项，因而不会出现两个工作者同时处理同一任务的情况。</p>

<div class="codeblock" id="code">
 <h3>查找、认领并完成已调度的工作项 - C#</h3>
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

<p>状态为 COMPLETED 时需要记录工作信息：执行者、执行内容、开始和结束时间以及输出。工作者在状态变更之前通过一次更新将这些信息保存。</p>

<div class="codeblock" id="code">
 <h3>已执行手术数据 - C#</h3>
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

<p>使用 Aspose.Medical 构建的服务器会强制执行这些规则。第二个尝试认领正在进行中的工作项的工作者会收到 HTTP 409，同样对已完成工作项的状态变更也会返回 409。缺少已执行手术数据的 COMPLETED 请求会返回 HTTP 400。客户端会将此类响应包装为带有 HTTP 响应的 <code>DicomWebClientException</code>。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="通过 WebSocket 监控工作列表">}}

<p>仪表盘、发起工作的系统或等待新任务的工作者使用其 AE 标题进行订阅，并打开通知连接。每条事件报告以包含工作项 UID 和新状态的数据集形式到达。</p>

<div class="codeblock" id="code">
 <h3>接收 UPS 事件报告 - C#</h3>
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

<p>除了整个工作列表外，客户端还可以使用 <code>SubscribeWorkitemAsync</code> 订阅单个工作项，或使用 <code>SubscribeFilteredWorklistAsync</code> 订阅工作列表的过滤子集（前提是服务器支持过滤）。取消接收会关闭连接；使用 <code>ConnectNotificationsAsync</code> 再次打开新连接以继续接收。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="您自己的 UPS-RS 服务器">}}

<p><code>Aspose.Medical.Dicom.Web.AspNetCore</code> 包为 ASP.NET Core 服务添加 UPS-RS 功能。库会处理所有请求：检查新工作项的负载、状态迁移、工作者的事务 UID 以及 COMPLETED 所需的数据，并对搜索应用 DICOM 匹配规则。您的代码负责存储工作项。</p>

<div class="codeblock" id="code">
 <h3>UPS-RS 服务器的 Program.cs - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>工作项存储 - C#</h3>
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

<p>每个已存储的工作项都有一个修订号。<code>TryReplaceAsync</code> 仅在修订号仍为库读取时才会替换，这样同时对同一工作项的两个请求不会相互覆盖；库会重试或报告冲突。将字典替换为您的数据库并保留此检查。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="服务器端通知">}}

<p>要发送事件报告，需要注册一个 <code>IDicomWebSubscriptionProvider</code> 并在 <code>MapDicomWeb</code> 之前调用 <code>app.UseWebSockets()</code>。该提供程序维护订阅信息以及每个订阅者的事件报告队列，库在工作项变更时将事件报告交给它。提供程序还会告知库经过身份验证的调用者的 AE 标题，因为事件报告会标明是哪个系统改变了状态。</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">workflow providers guide（工作流提供程序指南）</a> 包含完整的内存订阅提供程序，<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core guide（ASP.NET Core 指南）</a> 介绍了 WebSocket 设置。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS、MWL 与 MPPS">}}

<p>UPS 包含了旧版 Modality Worklist（模态工作列表）和 MPPS 在 RIS 与模态之间的功能，并且增加了非模态的工作者、认领和通知机制。许多模态仍仅通过 DIMSE 使用 MWL 和 MPPS；<a href="/medical/net/modality-worklist/">modality worklist page（模态工作列表页面）</a> 使用同一库展示了该工作流。</p>

<p>相关页面：用于其他 DICOMweb 服务的 <a href="/medical/net/dicomweb-client/">DICOMweb client（客户端）</a>，用于同一服务器上 QIDO-RS、WADO-RS 和 STOW-RS 的 <a href="/medical/net/dicomweb-server/">DICOMweb server（服务器）</a>，以及用于 DIMSE 的 <a href="/medical/net/dicom-networking/">DICOM networking（网络）</a>。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学习资源" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="文档" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="开发者指南" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="API 参考" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="产品支持" tabId="support" >}}
{{< blocks/products/pf/slr-element name="免费支持" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="付费支持" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="博客" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="为什么选择 Aspose.Medical for .NET？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="客户名单" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功案例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
