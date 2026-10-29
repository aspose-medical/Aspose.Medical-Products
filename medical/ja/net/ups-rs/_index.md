---
title: C# .NET の UPS-RS - DICOMweb 統合手順ステップ ワークアイテム | Aspose.Medical
weight: 9400

description: C# から DICOM UPS ワークアイテムを作成、取得、完了し、WebSocket で状態変化を受信するか、Aspose.Medical を使用して ASP.NET Core 上で独自の UPS-RS サーバーを実行します。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# 用 UPS-RS" h2="DICOM Unified Procedure Step を使用してシステム間で画像処理を割り当てます：ワークアイテムを作成し、ワーカーが取得・完了できるようにし、クライアントとしてもサーバーとしてもすべての変更を追跡します。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM に対応した作業キュー">}}

<p>AI モデルは新しい胸部 CT をすべて処理しなければなりません。遠隔放射線サービスは検査をリーダーに割り当てます。3D ラボは後処理のオーダーを受け取ります。これらはすべてシステム間のタスクキューであり、DICOM はそれ用に Unified Procedure Step (UPS) を定義しています。これは DICOMweb の UPS‑RS サービスによって HTTP で提供されます。</p>

<p>UPS ワークアイテムは、何を行うか、どの患者に対して、どの入力を使用するかを示します。作成時に SCHEDULED になります。ワーカーが取得して IN PROGRESS にし、完了すると COMPLETED、あるいは CANCELED で終了します。他のシステムはサブスクライブして WebSocket 経由ですべての状態変化を受信するため、ポーリングは不要です。</p>

<p><strong>Aspose.Medical for .NET</strong> は、マネージド C# で両側を実装します：任意の UPS‑RS サーバー用の型付きクライアントと、ASP.NET Core 用のサーバーサイドです。ライブラリは UPS ルールをチェックし、コードはワークアイテムを保存します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ワークアイテムをスケジュールする">}}

<p>作業を依頼するシステムがワークアイテムを作成します。その SOP Instance UID が以降の識別子となります。ラベル、ワークリストラベル、優先度がワーカーに内容を伝えます。</p>

<div class="codeblock" id="code">
 <h3>UPS ワークアイテムの作成 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="取得して作業を行い、完了する">}}

<p>ワーカーはワークリストを検索し、実行可能な項目を見つけ、生成したトランザクション UID を使ってそのワークアイテムを IN PROGRESS に移動させて取得します。その時点からこの UID の所有者だけがワークアイテムを更新または完了できるため、二人のワーカーが同じタスクを同時に処理することはありません。</p>

<div class="codeblock" id="code">
 <h3>スケジュールされたワークアイテムを検索、取得、完了 - C#</h3>
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

<p>COMPLETED には作業の記録が必要です：実施者、実施内容、開始時刻と終了時刻、そして出力。ワーカーは状態が変わる前に更新とともにそれを保存します。</p>

<div class="codeblock" id="code">
 <h3>実施された手順データ - C#</h3>
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

<p>Aspose.Medical で構築されたサーバーはこれらのルールを強制します。進行中のワークアイテムを取得しようとする二番目のワーカーは HTTP 409 を受け取り、完了したワークアイテムの状態変更も同様です。実施された手順データがない COMPLETED 要求は HTTP 400 を返します。クライアントはこのような応答を HTTP 応答付きの <code>DicomWebClientException</code> として報告します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="WebSocket でワークリストを追跡する">}}

<p>ダッシュボードや、作業を依頼したシステム、または新しいタスクを待機するワーカーは、自身の AE タイトルでサブスクライブし、通知接続を開きます。すべてのイベントレポートはワークアイテム UID と新しい状態を含むデータセットとして届きます。</p>

<div class="codeblock" id="code">
 <h3>UPS イベントレポートの受信 - C#</h3>
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

<p>全体のワークリストに加えて、クライアントは <code>SubscribeWorkitemAsync</code> で単一のワークアイテムに、サーバーがフィルタをサポートしていれば <code>SubscribeFilteredWorklistAsync</code> でワークリストのフィルタ済み部分にサブスクライブできます。受信をキャンセルすると接続が閉じます。続行するには <code>ConnectNotificationsAsync</code> で新しい接続を開きます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="独自の UPS-RS サーバー">}}

<p><code>Aspose.Medical.Dicom.Web.AspNetCore</code> パッケージは ASP.NET Core サービスに UPS‑RS を追加します。ライブラリはすべてのリクエストを処理し、新しいワークアイテムのペイロード、状態遷移、ワーカーのトランザクション UID、COMPLETED が要求するデータをチェックし、検索に対して DICOM のマッチングルールを適用します。コードはワークアイテムを保存します。</p>

<div class="codeblock" id="code">
 <h3>UPS‑RS サーバーの Program.cs - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>ワークアイテムの保存 - C#</h3>
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

<p>保存された各ワークアイテムにはリビジョンがあります。<code>TryReplaceAsync</code> は、リビジョンがライブラリが読み取ったものと同じ場合にのみ置き換えを行うため、同時に同じワークアイテムを変更する二つのリクエストが互いに上書きすることはできません。ライブラリは再試行するか、競合として報告します。辞書をデータベースに置き換え、同じチェックを保持してください。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="サーバー上の通知">}}

<p>イベントレポートを送信するには、<code>IDicomWebSubscriptionProvider</code> を登録し、<code>MapDicomWeb</code> の前に <code>app.UseWebSockets()</code> を呼び出します。プロバイダーはサブスクライバーごとにサブスクリプションとイベントレポートのキューを保持し、ワークアイテムが変わるたびにライブラリからイベントレポートを受け取ります。また、プロバイダーは認証された呼び出し元の AE タイトルをライブラリに伝えます。これはイベントレポートが状態を変更したシステムを示すためです。</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">workflow providers guide</a> には完全なインメモリ・サブスクリプションプロバイダーが含まれ、<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core guide</a> では WebSocket 設定について説明しています。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS、MWL、MPPS">}}

<p>UPS は、従来の Modality Worklist と MPPS が RIS とモダリティ間で行っていたことをカバーし、モダリティ以外のワーカーの取得や通知を追加します。多くのモダリティは依然として DIMSE 上で MWL と MPPS のみを使用しています。<a href="/medical/net/modality-worklist/">modality worklist page</a> は同じライブラリでそのワークフローを示しています。</p>

<p>関連ページ: 他の DICOMweb サービス用の <a href="/medical/net/dicomweb-client/">DICOMweb client</a>、同一サーバー上の QIDO‑RS、WADO‑RS、STOW‑RS 用の <a href="/medical/net/dicomweb-server/">DICOMweb server</a>、そして DIMSE 用の <a href="/medical/net/dicom-networking/">DICOM networking</a>。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学習リソース" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="ドキュメント" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="開発者ガイド" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="API リファレンス" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="製品サポート" tabId="support" >}}
{{< blocks/products/pf/slr-element name="無料サポート" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="有料サポート" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ブログ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="なぜ Aspose.Medical for .NET なのか？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="顧客一覧" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
