---
title: UPS-RS trong C# .NET - DICOMweb Unified Procedure Step Workitems | Aspose.Medical
weight: 9400

description: Tạo, nhận và hoàn thành workitems UPS DICOM từ C#, nhận các thay đổi trạng thái qua WebSocket, hoặc chạy máy chủ UPS-RS của riêng bạn trên ASP.NET Core với Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS cho .NET C#" h2="Phân phối công việc hình ảnh giữa các hệ thống bằng DICOM Unified Procedure Step: tạo workitems, cho phép các worker nhận và hoàn thành chúng, và theo dõi mọi thay đổi, như một client hoặc như máy chủ." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Hàng đợi công việc giao tiếp bằng DICOM">}}

<p>Một mô hình AI phải xử lý mọi CT ngực mới. Dịch vụ teleradiology gán các study cho người đọc. Phòng thí nghiệm 3D nhận các đơn đặt hàng hậu xử lý. Mỗi trường hợp này là một hàng đợi nhiệm vụ giữa các hệ thống, và DICOM định nghĩa một hàng đợi cho nó: Unified Procedure Step (UPS), được cung cấp qua HTTP bởi dịch vụ UPS-RS của DICOMweb.</p>

<p>Một workitem UPS mô tả công việc cần thực hiện, cho bệnh nhân nào và với dữ liệu đầu vào nào. Nó ở trạng thái SCHEDULED khi được tạo. Một worker nhận nó và chuyển thành IN PROGRESS, sau đó hoàn thành thành COMPLETED, hoặc nó có thể kết thúc ở trạng thái CANCELED. Các hệ thống khác đăng ký và nhận mọi thay đổi trạng thái qua WebSocket, do đó không ai cần phải polling.</p>

<p><strong>Aspose.Medical for .NET</strong> thực hiện cả hai phía trong C# quản lý: một client kiểu dữ liệu cho bất kỳ máy chủ UPS-RS nào, và phía máy chủ cho ASP.NET Core, nơi thư viện kiểm tra các quy tắc UPS và mã của bạn lưu trữ các workitem.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lên lịch một workitem">}}

<p>Hệ thống muốn thực hiện công việc tạo ra workitem. SOP Instance UID của nó xác định workitem kể từ lúc tạo; nhãn, nhãn worklist và mức độ ưu tiên cho worker biết đó là gì.</p>

<div class="codeblock" id="code">
 <h3>Tạo một UPS workitem - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Nhận nó, thực hiện công việc, hoàn thành nó">}}

<p>Một worker tìm kiếm worklist để tìm công việc có thể thực hiện và nhận một workitem bằng cách chuyển nó sang IN PROGRESS với transaction UID do mình tạo. Từ thời điểm đó, chỉ người giữ UID này mới có thể cập nhật hoặc hoàn thành workitem, vì vậy hai worker sẽ không bao giờ xử lý cùng một nhiệm vụ.</p>

<div class="codeblock" id="code">
 <h3>Tìm, nhận và hoàn thành các workitem đã lên lịch - C#</h3>
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

<p>COMPLETED yêu cầu một bản ghi về công việc: ai thực hiện, gì đã được thực hiện, thời gian bắt đầu và kết thúc, và đầu ra. Worker lưu trữ nó cùng với một cập nhật trước khi thay đổi trạng thái.</p>

<div class="codeblock" id="code">
 <h3>Dữ liệu quy trình đã thực hiện - C#</h3>
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

<p>Một máy chủ được xây dựng bằng Aspose.Medical thực thi các quy tắc này. Một worker thứ hai cố gắng nhận một workitem đang tiến hành sẽ nhận được HTTP 409, và tương tự khi thay đổi trạng thái của một workitem đã hoàn thành. Yêu cầu COMPLETED thiếu dữ liệu quy trình đã thực hiện sẽ nhận HTTP 400. Client báo cáo các phản hồi này dưới dạng <code>DicomWebClientException</code> cùng với phản hồi HTTP.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Theo dõi worklist qua WebSocket">}}

<p>Một dashboard, hệ thống đã đặt công việc hoặc một worker đang chờ nhiệm vụ mới đăng ký với AE Title của mình và mở một kết nối thông báo. Mỗi báo cáo sự kiện đến dưới dạng dataset chứa UID của workitem và trạng thái mới của nó.</p>

<div class="codeblock" id="code">
 <h3>Nhận báo cáo sự kiện UPS - C#</h3>
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

<p>Ngoài toàn bộ worklist, client có thể đăng ký một workitem riêng với <code>SubscribeWorkitemAsync</code> hoặc một phần đã lọc của worklist với <code>SubscribeFilteredWorklistAsync</code>, nếu máy chủ hỗ trợ bộ lọc. Hủy nhận sẽ đóng kết nối; mở một kết nối mới với <code>ConnectNotificationsAsync</code> để tiếp tục.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Máy chủ UPS-RS của riêng bạn">}}

<p>Gói <code>Aspose.Medical.Dicom.Web.AspNetCore</code> thêm UPS-RS vào một dịch vụ ASP.NET Core. Thư viện xử lý mọi yêu cầu: kiểm tra payload của workitem mới, các chuyển đổi trạng thái, transaction UID của worker và dữ liệu mà COMPLETED yêu cầu, và áp dụng các quy tắc khớp DICOM vào các truy vấn. Mã của bạn lưu trữ các workitem.</p>

<div class="codeblock" id="code">
 <h3>Program.cs của máy chủ UPS-RS - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Lưu trữ workitem - C#</h3>
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

<p>Mỗi workitem được lưu có một revision. <code>TryReplaceAsync</code> chỉ thay thế khi revision vẫn là phiên bản mà thư viện đã đọc, vì vậy hai yêu cầu thay đổi cùng một workitem tại cùng thời điểm không thể ghi đè lên nhau; thư viện sẽ thử lại hoặc báo cáo xung đột. Thay thế dictionary bằng cơ sở dữ liệu của bạn và duy trì kiểm tra này.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Thông báo trên máy chủ">}}

<p>Để gửi báo cáo sự kiện, đăng ký một <code>IDicomWebSubscriptionProvider</code> và gọi <code>app.UseWebSockets()</code> trước <code>MapDicomWeb</code>. Provider duy trì các subscription và một hàng đợi báo cáo sự kiện cho mỗi subscriber, và thư viện cung cấp cho nó một báo cáo sự kiện mỗi khi workitem thay đổi. Provider cũng thông báo cho thư viện AE Title của người gọi đã xác thực, vì các báo cáo sự kiện nêu tên hệ thống đã thay đổi trạng thái.</p>

<p>Hướng dẫn <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">các provider workflow</a> chứa một provider subscription trong bộ nhớ hoàn chỉnh, và <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">hướng dẫn ASP.NET Core</a> đề cập đến cài đặt WebSocket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL và MPPS">}}

<p>UPS bao phủ các chức năng mà Modality Worklist và MPPS cũ thực hiện giữa RIS và một modality, và nó thêm các worker không phải là modality, cho phép nhận và thông báo. Nhiều modality vẫn chỉ sử dụng MWL và MPPS qua DIMSE; <a href="/medical/net/modality-worklist/">trang modality worklist</a> hiển thị workflow đó bằng cùng thư viện.</p>

<p>Các trang liên quan: <a href="/medical/net/dicomweb-client/">client DICOMweb</a> cho các dịch vụ DICOMweb khác, <a href="/medical/net/dicomweb-server/">server DICOMweb</a> cho QIDO-RS, WADO-RS và STOW-RS trên cùng máy chủ, và <a href="/medical/net/dicom-networking/">mạng DICOM</a> cho DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Hướng dẫn nhà phát triển" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="Tham chiếu API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Hỗ trợ sản phẩm" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Hỗ trợ miễn phí" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Hỗ trợ trả phí" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Tại sao chọn Aspose.Medical cho .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Danh sách khách hàng" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Câu chuyện thành công" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
