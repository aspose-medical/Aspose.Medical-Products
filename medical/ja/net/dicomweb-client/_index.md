---
title: C# .NET 用 DICOMweb クライアント - QIDO-RS、WADO-RS、STOW-RS | Aspose.Medical
weight: 9100

description: .NET から HTTP 経由で DICOM 研究（スタディ）を検索、ダウンロード、アップロードします。QIDO-RS、WADO-RS、STOW-RS 用の純粋な C# 実装の DICOMweb クライアントで、PACS、VNA、クラウド画像サービスと連携します。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# 用 DICOMweb クライアント" h2="QIDO-RS、WADO-RS、STOW-RS を使用して任意の DICOMweb サーバー上のスタディを検索、ダウンロード、アップロードします。マネージド C# で実装され、マシンに何もインストールする必要はありません。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="自分のコードから HTTP 経由で DICOM を扱う">}}

<p>DICOMweb は DICOM の HTTP 形式です。ポート 104 での DIMSE アソシエーションの代わりに、アプリケーションは通常の HTTPS リクエストを送ります：検索は QIDO-RS、ダウンロードは WADO-RS、アップロードは STOW-RS。Azure Health Data Services の DICOM サービスや Google Cloud Healthcare API などのクラウド画像サービス、また Orthanc や dcm4chee といったアーカイブも同様に動作します。</p>

<p><strong>Aspose.Medical for .NET</strong> はバージョン 26.10 以降、DICOMweb クライアントを含んでいます。<code>DicomWebClient</code> はすべての標準トランザクションを型付きメソッドでラップし、結果はライブラリの他の部分が読み書きするのと同じ <code>Dataset</code> および <code>DicomFile</code> オブジェクトとして返されます。対向側のサーバーは任意の DICOMweb 実装で構いません；Aspose.Medical で構築されている必要はありません。</p>

<p>クライアントは <code>HttpClient</code> 上に構築されたマネージド C# です。ネイティブツールキットやプロキシサービスは不要なので、同じコードが Windows、Linux、コンテナ上で動作します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOMweb サーバーに接続する">}}

<p><code>HttpClient</code> とサーバーのサービスルート URL からクライアントを作成します。認証、TLS 設定、プロキシ、接続プーリングはすべて <code>HttpClient</code> 上に保持され、アプリケーション側で既に設定されたものが使用されます。クライアントは <code>HttpClient</code> を所有せず、破棄もしません。</p>

<div class="codeblock" id="code">
 <h3>DICOMweb クライアントを作成する - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>あるデプロイが WADO-URI や別のサービスを異なる URL で提供する場合、<code>ServiceBaseUris</code> はそのサービスだけのルートを上書きします。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="QIDO-RS でスタディを検索する">}}

<p>検索では、名前に DICOM キーワードを使用した条件パラメータ、返却するフィールド、ページング制御を指定します。マッチ結果はデータセットのストリームとして返され、レスポンスがダウンロード中でも1件ずつ読み取れます。</p>

<div class="codeblock" id="code">
 <h3>患者のスタディを検索する - C#</h3>
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

<p><code>SearchSeriesAsync</code> と <code>SearchInstancesAsync</code> は1レベル下の検索を行い、アーカイブ全体または特定のスタディやシリーズ内で検索できます。同じオプションでファジーマッチ、空値マッチ、ページング用の結果オフセットも設定できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="WADO-RS でスタディをダウンロードする">}}

<p>スタディには数千のインスタンスが含まれることがあります。<code>RetrieveStudyAsync</code> はそれらをストリームで返すため、レスポンスの一部が届くたびに各ファイルを解析し、次のファイルを読み込む前に保存できます。メモリ使用量はスタディのサイズに比例して増えません。</p>

<div class="codeblock" id="code">
 <h3>スタディのすべてのインスタンスを保存する - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> と <code>RetrieveInstanceAsync</code> は同様の方法でシリーズまたは単一インスタンスをダウンロードします。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="サムネイルまたはレンダリング画像を取得する">}}

<p>ビューアやワークリスト画面は DICOM ファイルではなく画像が必要なことが多いです。サーバーが画像をレンダリングして JPEG または PNG で送信するため、クライアントは元のピクセルデータをダウンロードしたりデコードしたりする必要がありません。</p>

<div class="codeblock" id="code">
 <h3>インスタンスの JPEG サムネイルを保存する - C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> と <code>RetrieveRenderedFramesAsync</code> は代わりにフルサイズ画像を返します。<code>DicomWebRenderOptions</code> で画像品質、ビューポートサイズ、ウィンドウ設定、ICC プロファイルといった標準的なレンダリング制御をサーバーに渡します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="STOW-RS でインスタンスをアップロードする">}}

<p>STOW-RS は任意の数のインスタンスを1つのリクエストで送信します。サーバーはレポートでインスタンスごとに保存されたものと拒否されたものを一覧で返します。</p>

<div class="codeblock" id="code">
 <h3>DICOM ファイルのフォルダーをアップロードする - C#</h3>
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

<p>一部のサーバーは、1つ以上のインスタンスが失敗した場合に 409 などの HTTP エラーで応答します。その場合、次のセクションで示すように呼び出しは <code>DicomWebClientException</code> をスローし、同じレポートが例外のレスポンスボディに含まれます。</p>

<p><code>StoreStudyAsync</code> は指定されたスタディへアップロードします。両メソッドとも <code>IAsyncEnumerable&lt;DicomFile&gt;</code> を受け取れるため、リクエスト送信中にファイルを逐次生成できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="サーバーエラーの処理">}}

<p>すべての型付き操作は返却前に HTTP ステータスを確認します。サーバーがリクエストを拒否した場合、呼び出しは <code>DicomWebClientException</code> をスローし、ステータスコード、ヘッダー、レスポンスボディの先頭部分をログ用に保持します。</p>

<div class="codeblock" id="code">
 <h3>失敗したリクエストを報告する - C#</h3>
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

<p>上記のサムネイル取得のように生のレスポンスを返すメソッドは、ステータスチェックを <code>IsSuccessStatusCode</code> または <code>EnsureSuccessStatusCode</code> に委ねます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="クライアントがカバーするその他の機能">}}

<p>検索、ダウンロード、アップロードは一般的な呼び出しです。クライアントは他の DICOMweb サービスごとに操作グループを提供します。</p>

<ul>
<li>インスタンスのバルクデータ、ピクセルデータ、選択フレームを、サーバーが送信した 1 基準のフレーム番号で取得します。</li>
<li>WADO-URI、従来の単一 URL 取得で、多くのビューアやアーカイブが未だに使用している DICOM ファイルやレンダリング画像の取得。</li>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> ワークリスト：作成、検索、更新、取得、状態変更、キャンセル要求、WebSocket 経由で届くイベントの購読が可能です。</li>
<li><a href="/medical/net/modality-worklist/">Modality worklist</a>（MWL-RS）、モダリティ実施手順ステップ（MPPS-RS）およびストレージコミットメント。</li>
<li>カラーパレットやハンギングプロトコルなどの患者外インスタンス、サーバーの機能（Capabilities）ドキュメント。</li>
<li><code>SendAsync</code> で独自のルート値、クエリパラメータ、ヘッダー、ボディを使用して任意の標準トランザクションを実行できる。</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb クライアント ガイド</a> ではすべての操作とオプションを解説し、<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">クライアント操作リファレンス</a> で全メソッドを一覧化しています。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTTP 応答からピクセルデータまでを1つのライブラリで">}}

<p>クライアントでダウンロードしたファイルは通常の <code>DicomFile</code> です。同一プロセス内で匿名化、トランスコーディング、レンダリング、シリアライズが可能で、変換ステップや別のライブラリをチェーンに加える必要はありません。</p>

<p>関連ページ：独自の ASP.NET Core サービスでプロトコルの相手側を実装するための <a href="/medical/net/dicomweb-server/">DICOMweb サーバー</a>、HTTP ではなく DIMSE を使用するアーカイブ向けの <a href="/medical/net/dicom-networking/">DICOM ネットワーキング</a>、スタディが病院を出る前の <a href="/medical/net/anonymization/">匿名化</a>、サーバーが保存するための <a href="/medical/net/dicom-transfer-syntax-conversion/">転送シンタックス変換</a>、そして QIDO-RS 検索結果が使用する形式の <a href="/medical/net/dicom-to-json/">DICOM から JSON への変換</a>。</p>

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

{{< blocks/products/pf/slr-tab tabTitle="なぜ .NET 向け Aspose.Medical なのか？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="顧客リスト" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
