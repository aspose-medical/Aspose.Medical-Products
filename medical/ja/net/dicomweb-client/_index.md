---
title: C# .NET の DICOMweb クライアント - QIDO-RS、WADO-RS、STOW-RS | Aspose.Medical
weight: 9100

description: .NET から HTTP 経由で DICOM スタディを検索、ダウンロード、アップロードします。PACS、VNA、クラウド画像サービスと連携できる、QIDO-RS、WADO-RS、STOW-RS 用の純粋な C# DICOMweb クライアントです。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# 用 DICOMweb クライアント" h2="QIDO-RS、WADO-RS、STOW-RS を使用して、任意の DICOMweb サーバー上でスタディの検索、ダウンロード、アップロードを、インストール不要のマネージド C# で実行できます。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="自分のコードから DICOM を HTTP で扱う">}}

<p>DICOMweb は DICOM の HTTP 形式です。ポート 104 の DIMSE アソシエーションの代わりに、アプリケーションは普通の HTTPS リクエストを送ります: 検索には QIDO-RS、ダウンロードには WADO-RS、アップロードには STOW-RS を使用します。Azure Health Data Services の DICOM サービスや Google Cloud Healthcare API などのクラウド画像サービス、そして Orthanc や dcm4chee といったアーカイブもこの方式を採用しています。</p>

<p><strong>Aspose.Medical for .NET</strong> はバージョン 26.10 以降、DICOMweb クライアントを含みます。<code>DicomWebClient</code> はすべての標準トランザクションを型付きメソッドでラップし、結果はライブラリの他の部分が読み書きするのと同じ <code>Dataset</code> と <code>DicomFile</code> オブジェクトとして返されます。対向側のサーバーは任意の DICOMweb 実装でかまいません。Aspose.Medical で構築されている必要はありません。</p>

<p>クライアントは <code>HttpClient</code> 上に構築されたマネージド C# です。ネイティブツールキットやプロキシサービスは不要で、同じコードが Windows、Linux、コンテナ上で動作します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOMweb サーバーに接続する">}}

<p>サーバーのサービスルート URL と <code>HttpClient</code> からクライアントを作成します。認証、TLS 設定、プロキシ、接続プーリングは <code>HttpClient</code> に保持され、アプリケーション側で既に設定されたものが使用されます。クライアントは <code>HttpClient</code> の所有権を持たず、Dispose もしません。</p>

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

<p>あるデプロイが WADO-URI や別のサービスを異なる URL で提供する場合、<code>ServiceBaseUris</code> を使用してそのサービスだけのルートを上書きできます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="QIDO-RS でスタディを検索する">}}

<p>検索では DICOM キーワードを名前としたマッチングパラメータ、返すフィールド、ページング制御を指定します。マッチ結果はデータセットのストリームとして返され、レスポンスのダウンロード中に1件ずつ読み取れます。</p>

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

<p><code>SearchSeriesAsync</code> と <code>SearchInstancesAsync</code> は1レベル下の検索を行い、アーカイブ全体または特定のスタディ・シリーズ内を対象にします。同じオプションでファジーマッチ、空値マッチ、ページング用の結果オフセットも設定できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="WADO-RS でスタディをダウンロードする">}}

<p>スタディは数千件のインスタンスを含むことがあります。<code>RetrieveStudyAsync</code> はそれらをストリームとして返すため、レスポンスの一部が到着した時点で各ファイルを解析し、次のファイルを読む前に保存できます。メモリ使用量はスタディのサイズに比例して増加しません。</p>

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

<p><code>RetrieveSeriesAsync</code> と <code>RetrieveInstanceAsync</code> も同様の方法で、1シリーズまたは1インスタンスをダウンロードします。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="サムネイルやレンダリング画像を取得する">}}

<p>ビューアやワークリスト画面では DICOM ファイルではなく画像が必要なことが多いです。サーバーが画像をレンダリングし、JPEG や PNG で送信するため、クライアントは元のピクセルデータをダウンロード・デコードする必要がありません。</p>

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

<p><code>RetrieveRenderedInstanceAsync</code> と <code>RetrieveRenderedFramesAsync</code> は代わりにフルサイズ画像を返します。<code>DicomWebRenderOptions</code> で画像品質、ビューポートサイズ、ウィンドウ設定、ICC プロファイルなど、標準のレンダリング制御をサーバーに渡します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="STOW-RS でインスタンスをアップロードする">}}

<p>STOW-RS は1リクエストで任意の数のインスタンスを送信します。サーバーはレポートで、インスタンスごとに保存されたか拒否されたかを一覧で返します。</p>

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

<p>一部のサーバーはインスタンスが失敗した場合に 409 などの HTTP エラーで応答します。その場合、呼び出しは <code>DicomWebClientException</code> をスローし、例外のレスポンスボディに同じレポートが含まれます（次のセクションを参照）。</p>

<p><code>StoreStudyAsync</code> は名前付きスタディにアップロードします。両メソッドとも <code>IAsyncEnumerable&lt;DicomFile&gt;</code> を受け取れるため、リクエスト送信中にファイルを生成できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="サーバーエラーの処理">}}

<p>すべての型付き操作は返却前に HTTP ステータスを確認します。サーバーがリクエストを拒否した場合、呼び出しは <code>DicomWebClientException</code> をスローし、ステータスコード、ヘッダー、レスポンスボディの先頭部分をログに保持します。</p>

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

<p>サムネイル取得など、生のレスポンスを返すメソッドは、<code>IsSuccessStatusCode</code> または <code>EnsureSuccessStatusCode</code> を使用してステータスチェックを呼び出し側に委ねます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="クライアントがカバーするその他の機能">}}

<p>検索、ダウンロード、アップロードは共通の呼び出しです。クライアントは他の DICOMweb サービスにもそれぞれの操作グループを提供します。</p>

<ul>
<li>インスタンスのバルクデータ、ピクセルデータ、選択フレームを 1 起点のフレーム番号で取得（サーバーが送信した通り）。</li>
<li>WADO-URI（多くのビューアやアーカイブがまだ使用している従来の単一 URL 取得）で DICOM ファイルやレンダリング画像を取得します。</li>
<li>UPS-RS ワークリスト：ワークアイテムの作成、検索、更新、取得、状態変更、キャンセル要求、WebSocket 経由で届くイベントの購読。</li>
<li>モダリティ ワークリスト (MWL-RS)、モダリティ実施手順ステップ (MPPS-RS)、ストレージコミットメント。</li>
<li>カラーパレットやハンギングプロトコルといった非患者インスタンス、サーバーの機能文書。</li>
<li><code>SendAsync</code> で独自のルート値、クエリパラメータ、ヘッダー、ボディを指定して任意の標準トランザクションを実行できる。</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb クライアント ガイド</a> ではすべての操作とオプションが文書化され、<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">クライアント操作リファレンス</a> では全メソッドが一覧化されています。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTTP 応答からピクセルデータまでを1つのライブラリで">}}

<p>クライアントでダウンロードしたファイルは通常の <code>DicomFile</code> です。同じプロセス内で匿名化、トランスコーディング、レンダリング、シリアライズが可能で、変換ステップや別ライブラリをチェーンに入れる必要はありません。</p>

<p>関連ページ: <a href="/medical/net/dicomweb-server/">DICOMweb サーバー</a>（自分の ASP.NET Core サービス側のプロトコル実装）、<a href="/medical/net/dicom-networking/">DICOM ネットワーキング</a>（DIMSE を使用するアーカイブ向け）、<a href="/medical/net/anonymization/">匿名化</a>（スタディが病院を離れる前に）、<a href="/medical/net/dicom-transfer-syntax-conversion/">転送構文変換</a>（サーバーが保存する形式）、そして <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>（QIDO-RS 検索結果が使用するフォーマット）。</p>

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

{{< blocks/products/pf/slr-tab tabTitle="なぜ .NET 用 Aspose.Medical なのか？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="顧客一覧" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
