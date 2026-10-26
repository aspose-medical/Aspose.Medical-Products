---
title: C# .NET 用 DICOMweb クライアント - QIDO-RS、WADO-RS、STOW-RS | Aspose.Medical
weight: 9100

description: .NET から HTTP 経由で DICOM スタディを検索、ダウンロード、アップロードします。QIDO-RS、WADO-RS、STOW-RS 用の純粋な C# DICOMweb クライアントで、PACS、VNA、クラウド画像サービスと連携します。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# 用 DICOMweb クライアント" h2="任意の DICOMweb サーバーで QIDO-RS、WADO-RS、STOW-RS を使用してスタディを検索、ダウンロード、アップロードします。マシンに何もインストール不要のマネージド C# です。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="自分のコードから HTTP 経由で DICOM を使用">}}

<p>DICOMweb は DICOM の HTTP 版です。ポート 104 の DIMSE アソシエーションの代わりに、アプリケーションは通常の HTTPS リクエストを送ります：検索には QIDO-RS、ダウンロードには WADO-RS、アップロードには STOW-RS を使用します。Azure Health Data Services の DICOM サービスや Google Cloud Healthcare API などのクラウド画像サービス、そして Orthanc や dcm4chee といったアーカイブも同様に動作します。</p>

<p><strong>Aspose.Medical for .NET</strong> はバージョン 26.10 以降、DICOMweb クライアントを含みます。<code>DicomWebClient</code> はすべての標準トランザクションを型付けされたメソッドでラップし、結果はライブラリの他の部分が読み書きするのと同じ <code>Dataset</code> と <code>DicomFile</code> オブジェクトとして返されます。相手側のサーバーは任意の DICOMweb 実装で構いません；Aspose.Medical で構築されている必要はありません。</p>

<p>クライアントは <code>HttpClient</code> 上に構築されたマネージド C# です。ネイティブツールキットやプロキシサービスは不要で、同じコードが Windows、Linux、コンテナ上で動作します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOMweb サーバーに接続">}}

<p>クライアントは <code>HttpClient</code> とサーバーのサービスルート URL から作成します。認証、TLS 設定、プロキシ、接続プーリングは <code>HttpClient</code> に保持され、アプリケーション側で既に構成されています。クライアントは <code>HttpClient</code> を所有せず、Dispose もしません。</p>

<div class="codeblock" id="code">
 <h3>DICOMweb クライアントを作成 - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>あるデプロイで WADO-URI や別のサービスが別の URL から提供される場合、<code>ServiceBaseUris</code> はそのサービスだけのルートを上書きします。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="QIDO-RS でスタディを検索">}}

<p>検索では DICOM キーワード名のマッチングパラメータ、返却フィールド、ページング制御を指定します。マッチ結果はデータセットのストリームとして届き、応答がダウンロード中でも 1 件ずつ読み取れます。</p>

<div class="codeblock" id="code">
 <h3>患者のスタディを検索 - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> と <code>SearchInstancesAsync</code> は 1 レベル下の検索を行い、全アーカイブまたは特定のスタディやシリーズ内で検索できます。同じオプションでファジーマッチング、空値マッチング、ページング用の結果オフセットも設定できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="WADO-RS でスタディをダウンロード">}}

<p>スタディは数千件のインスタンスを含むことがあります。<code>RetrieveStudyAsync</code> はそれらをストリームとして返すため、応答の一部が到着した時点で各ファイルを解析し、次のファイルを読み取る前に保存できます。メモリ使用量はスタディのサイズに比例して増加しません。</p>

<div class="codeblock" id="code">
 <h3>スタディのすべてのインスタンスを保存 - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> と <code>RetrieveInstanceAsync</code> はシリーズまたはインスタンスを同様の方法でダウンロードします。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="サムネイルまたはレンダリング画像を取得">}}

<p>ビューアやワークリスト画面は DICOM ファイルではなく画像が必要なことが多いです。サーバーが画像をレンダリングして JPEG や PNG で送信するため、クライアントは元のピクセルデータをダウンロードしたりデコードしたりする必要がありません。</p>

<div class="codeblock" id="code">
 <h3>インスタンスの JPEG サムネイルを保存 - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p><code>RetrieveRenderedInstanceAsync</code> と <code>RetrieveRenderedFramesAsync</code> は代わりにフルサイズ画像を返します。<code>DicomWebRenderOptions</code> は画像品質、ビューポートサイズ、ウィンドウ設定、ICC プロファイルといった標準的なレンダリング制御をサーバーに渡します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="STOW-RS でインスタンスをアップロード">}}

<p>STOW-RS は 1 回のリクエストで任意の数のインスタンスを送信します。サーバーはレポートで、インスタンスごとに保存されたものと拒否されたものを一覧で返します。</p>

<div class="codeblock" id="code">
 <h3>DICOM ファイルのフォルダーをアップロード - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>いくつかのサーバーは、1 つ以上のインスタンスが失敗した場合に 409 などの HTTP エラーで応答します。その場合、呼び出しは <code>DicomWebClientException</code> をスローし（次のセクション参照）、同じレポートが例外のレスポンスボディに含まれます。</p>

<p><code>StoreStudyAsync</code> は指定した名前のスタディへアップロードします。両メソッドは <code>IAsyncEnumerable&lt;DicomFile&gt;</code> も受け取れるため、リクエスト送信中にファイルを生成できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="サーバーエラーの処理">}}

<p>すべての型付き操作は返却前に HTTP ステータスを確認します。サーバーがリクエストを拒否した場合、呼び出しは <code>DicomWebClientException</code> をスローし、ステータスコード、ヘッダー、レスポンスボディの冒頭がログとして保持されます。</p>

<div class="codeblock" id="code">
 <h3>失敗したリクエストを報告 - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>上記のサムネイルのように生のレスポンスを返すメソッドは、<code>IsSuccessStatusCode</code> または <code>EnsureSuccessStatusCode</code> によってステータスチェックを呼び出し元に委ねます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="クライアントが対応するその他の機能">}}

<p>検索、ダウンロード、アップロードは一般的な呼び出しです。クライアントは他の DICOMweb サービスそれぞれに対しても操作グループを提供します：</p>

<ul>
<li>インスタンスのバルクデータ、ピクセルデータ、選択フレームを 1 から始まるフレーム番号で取得（サーバーが送信した通り）。</li>
<li>WADO-URI：多くのビューアやアーカイブがまだ使用する古い単一 URL 取得で、DICOM ファイルやレンダリング画像を取得します。</li>
<li>UPS-RS ワークリスト：ワークアイテムの作成、検索、更新、取得、状態変更、キャンセル要求、WebSocket 経由で届くイベントの購読。</li>
<li>モダリティワークリスト（MWL-RS）、モダリティ実施手順ステップ（MPPS-RS）および保存コミットメント。</li>
<li>カラーパレットやハンギングプロトコルなど患者非対象インスタンス、そしてサーバーの機能ドキュメント。</li>
<li><code>SendAsync</code>：型付きメソッドに合わない場合に、独自のルート値、クエリパラメータ、ヘッダー、ボディを指定して任意の標準トランザクションを実行。</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb クライアントガイド</a> ではすべての操作とオプションが文書化され、<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">クライアント操作リファレンス</a> にはすべてのメソッドが一覧化されています。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTTP 応答からピクセルデータまでをカバーする 1 つのライブラリ">}}

<p>クライアントでダウンロードしたファイルは通常の <code>DicomFile</code> です。匿名化、トランスコーディング、レンダリング、シリアライズを同一プロセス内で行えるため、変換手順や別のライブラリをチェーンに追加する必要がありません。</p>

<p>関連ページ：HTTP ではなく DIMSE を使用するアーカイブ向けの <a href="/medical/net/dicom-networking/">DICOM ネットワーキング</a>、スタディが病院を出る前の <a href="/medical/net/anonymization/">匿名化</a>、サーバーが保存する形式の <a href="/medical/net/dicom-transfer-syntax-conversion/">転送構文変換</a>、そして QIDO-RS 検索結果が使用する形式の <a href="/medical/net/dicom-to-json/">DICOM から JSON への変換</a>。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学習リソース" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="ドキュメント" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="開発者ガイド" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="API リファレンス" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="製品サポート" tabId="support" >}}
{{< blocks/products/pf/slr-element name="無料サポート" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="有料サポート" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ブログ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle=".NET 用 Aspose.Medical を選ぶ理由" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="導入事例一覧" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
