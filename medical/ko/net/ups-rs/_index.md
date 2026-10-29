---
title: C# .NET의 UPS-RS - DICOMweb Unified Procedure Step 작업 항목 | Aspose.Medical
weight: 9400

description: C#에서 DICOM UPS 작업 항목을 생성하고, 청구하고, 완료하며, WebSocket을 통해 상태 변화를 수신하거나 Aspose.Medical을 사용하여 ASP.NET Core에서 자체 UPS-RS 서버를 실행합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#용 UPS-RS" h2="DICOM Unified Procedure Step을 사용하여 시스템 간에 영상 작업을 배분합니다: 작업 항목을 생성하고, 작업자를 통해 청구 및 완료하도록 하며, 클라이언트든 서버든 모든 변경 사항을 추적합니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM을 지원하는 작업 큐">}}

<p>AI 모델은 새로운 가슴 CT를 모두 처리해야 합니다. 원격 방사선 서비스는 연구를 독자에게 할당합니다. 3D 실험실은 후처리 주문을 받습니다. 이들 각각은 시스템 간 작업 큐이며, DICOM은 이를 위해 Unified Procedure Step(UPS)를 정의합니다. 이는 DICOMweb의 UPS-RS 서비스가 HTTP를 통해 제공합니다.</p>

<p>UPS 작업 항목은 수행할 작업, 대상 환자 및 입력 데이터를 정의합니다. 생성 시 SCHEDULED 상태가 됩니다. 작업자는 이를 청구하여 IN PROGRESS 상태로 만들고, 이후 COMPLETED로 완료하거나 CANCELED로 종료합니다. 다른 시스템은 구독하여 WebSocket을 통해 모든 상태 변화를 수신하므로 폴링이 필요 없습니다.</p>

<p><strong>Aspose.Medical for .NET</strong>은 관리형 C#에서 양쪽을 구현합니다: 모든 UPS-RS 서버용 타입화된 클라이언트와 ASP.NET Core용 서버 측으로, 라이브러리는 UPS 규칙을 검증하고 사용자의 코드는 작업 항목을 저장합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="작업 항목 예약">}}

<p>작업을 요청하는 시스템이 작업 항목을 생성합니다. 해당 SOP Instance UID가 이후 이를 식별하며, 라벨, 작업 목록 라벨 및 우선순위가 작업자에게 해당 작업이 무엇인지 알려줍니다.</p>

<div class="codeblock" id="code">
 <h3>UPS 작업 항목 생성 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="청구하고, 작업을 수행한 뒤 완료합니다">}}

<p>작업자는 작업 목록에서 수행 가능한 항목을 검색하고, 생성한 거래 UID와 함께 작업 항목을 IN PROGRESS 상태로 이동시켜 청구합니다. 이 시점부터 해당 UID를 보유한 사람만 작업 항목을 업데이트하거나 완료할 수 있어 두 작업자가 동일 작업을 동시에 처리하지 않습니다.</p>

<div class="codeblock" id="code">
 <h3>예약된 작업 항목 찾기, 청구 및 완료 - C#</h3>
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

<p>COMPLETED 상태에는 작업 기록이 필요합니다: 수행자, 수행 내용, 시작 및 종료 시간, 그리고 결과물. 작업자는 상태가 변경되기 전에 업데이트와 함께 이를 저장합니다.</p>

<div class="codeblock" id="code">
 <h3>수행된 절차 데이터 - C#</h3>
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

<p>Aspose.Medical로 구축된 서버가 이러한 규칙을 강제합니다. 진행 중인 작업 항목을 청구하려는 두 번째 작업자는 HTTP 409 응답을 받으며, 완료된 작업의 상태 변경 시에도 동일합니다. 수행된 절차 데이터가 없는 COMPLETED 요청은 HTTP 400을 반환합니다. 클라이언트는 이러한 응답을 <code>DicomWebClientException</code>으로 HTTP 응답과 함께 보고합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="WebSocket을 통한 작업 목록 추적">}}

<p>대시보드, 작업을 주문한 시스템 또는 새로운 작업을 기다리는 작업자는 자신의 AE 타이틀로 구독하고 알림 연결을 엽니다. 모든 이벤트 보고서는 작업 항목 UID와 새로운 상태를 포함한 데이터셋으로 도착합니다.</p>

<div class="codeblock" id="code">
 <h3>UPS 이벤트 보고서 수신 - C#</h3>
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

<p>전체 작업 목록 외에도 클라이언트는 <code>SubscribeWorkitemAsync</code>를 사용해 단일 작업 항목을 구독하거나, 서버가 필터를 지원하는 경우 <code>SubscribeFilteredWorklistAsync</code>로 작업 목록의 일부를 필터링하여 구독할 수 있습니다. 수신을 취소하면 연결이 종료되며, 계속하려면 <code>ConnectNotificationsAsync</code>로 새 연결을 엽니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="직접 구축하는 UPS-RS 서버">}}

<p><code>Aspose.Medical.Dicom.Web.AspNetCore</code> 패키지는 ASP.NET Core 서비스에 UPS-RS를 추가합니다. 라이브러리는 모든 요청을 처리하며, 새로운 작업 항목의 페이로드, 상태 전이, 작업자의 거래 UID 및 COMPLETED에 필요한 데이터를 검증하고, 검색 시 DICOM 매칭 규칙을 적용합니다. 사용자의 코드는 작업 항목을 저장합니다.</p>

<div class="codeblock" id="code">
 <h3>UPS-RS 서버의 Program.cs - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>작업 항목 저장소 - C#</h3>
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

<p>각 저장된 작업 항목은 리비전 번호를 가집니다. <code>TryReplaceAsync</code>는 라이브러리가 읽은 리비전과 동일할 때만 교체하므로, 동시에 두 요청이 같은 작업 항목을 변경하려 해도 서로 덮어쓰지 못합니다; 라이브러리는 재시도하거나 충돌을 보고합니다. 사전을 데이터베이스로 교체하고 이 검증을 유지하십시오.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="서버의 알림">}}

<p>이벤트 보고서를 전송하려면 <code>IDicomWebSubscriptionProvider</code>를 등록하고 <code>MapDicomWeb</code> 이전에 <code>app.UseWebSockets()</code>를 호출합니다. 제공자는 구독 정보를 유지하고 구독자당 하나의 이벤트 보고서 큐를 관리하며, 작업 항목이 변경될 때마다 라이브러리가 이벤트 보고서를 제공합니다. 또한 제공자는 인증된 호출자의 AE 타이틀을 라이브러리에 알려주며, 이는 이벤트 보고서가 상태를 변경한 시스템을 명시하기 때문입니다.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">워크플로우 제공자 가이드</a>에는 완전한 인메모리 구독 제공자가 포함되어 있으며, <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core 가이드</a>에서는 WebSocket 설정을 다룹니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL 및 MPPS">}}

<p>UPS는 기존 Modality Worklist와 MPPS가 RIS와 장비 사이에서 수행하는 역할을 포괄하며, 장비가 아닌 작업자를 추가하여 청구 및 알림 기능을 제공합니다. 많은 장비가 아직도 DIMSE를 통해 MWL 및 MPPS만을 지원합니다; <a href="/medical/net/modality-worklist/">Modality Worklist 페이지</a>에서 동일한 라이브러리로 해당 워크플로를 확인할 수 있습니다.</p>

<p>관련 페이지: 다른 DICOMweb 서비스용 <a href="/medical/net/dicomweb-client/">DICOMweb 클라이언트</a>, 동일 서버에서 QIDO-RS, WADO-RS 및 STOW-RS를 제공하는 <a href="/medical/net/dicomweb-server/">DICOMweb 서버</a>, 그리고 DIMSE용 <a href="/medical/net/dicom-networking/">DICOM 네트워킹</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="학습 자료" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="문서" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="개발자 가이드" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="API 참조" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="제품 지원" tabId="support" >}}
{{< blocks/products/pf/slr-element name="무료 지원" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="유료 지원" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="블로그" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="왜 .NET용 Aspose.Medical인가?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="고객 목록" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="성공 사례" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
