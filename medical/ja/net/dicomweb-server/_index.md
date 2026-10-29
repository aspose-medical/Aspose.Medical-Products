---
title: C# .NET の DICOMweb サーバー - QIDO-RS、WADO-RS、STOW-RS | Aspose.Medical
weight: 9200

description: C# で独自の DICOMweb サーバーまたは PACS を構築しましょう。Aspose.Medical は ASP.NET Core 上で QIDO-RS、WADO-RS、STOW-RS およびワークリストサービスを実装し、ストレージを接続するだけです。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# 用 DICOMweb サーバー" h2="ASP.NET Core サービスに QIDO-RS、WADO-RS、STOW-RS エンドポイントを追加します。このライブラリはプロトコルを扱い、実際のスタディの保存先はご自身のコードで決定します。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="C# で構築する独自の DICOMweb サーバー">}}

<p>PACS、VNA、クラウドアーカイブ、または研究プラットフォームは DICOMweb リクエストに応答する必要があります。QIDO-RS 検索、WADO-RS ダウンロード、STOW-RS アップロード、さらに多くの場合ワークリストサービスも含まれます。それぞれが固有のルート、メディアタイプ、マルチパートボディ、マッチングルール、ステータスコードを持ちます。</p>

<p><strong>Aspose.Medical for .NET</strong> はバージョン 26.10 以降、このプロトコル側を実装しています。すべてのリクエストを読み取り、標準と照合し、DICOM ルールに従って検索キーをマッチングし、結果をページングし、転送構文を変換してレスポンスを書き出します。ご自身のコードは、インスタンスの保存、検索候補の取得、保存されたファイルの返却という、唯一実行できる処理を行います。ビューア、モダリティ、その他のアーカイブは、他の DICOMweb サーバーと同様にあなたのサーバーと連携し、同じライブラリの <a href="/medical/net/dicomweb-client/">DICOMweb client</a> も同様に動作します。</p>

<p>サーバーは C# で管理されます。ASP.NET Core が動作する環境、すなわち Windows、Linux、コンテナ上で実行できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ASP.NET Core による完全なサーバー">}}

<p><code>Aspose.Medical.Dicom.Web.AspNetCore</code> パッケージをインストールします。このパッケージは Aspose.Medical を同梱し、DICOMweb サービスを依存性注入とエンドポイントルーティングに接続します。この <code>Program.cs</code> は動作する Studies サーバーの例で、登録される 2 つのクラスは以下に示されています。</p>

<div class="codeblock" id="code">
 <h3>DICOMweb サーバーの Program.cs - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>すべての Studies ルートは <code>/dicomweb</code> 以下で応答します。検索とアップロードは <code>/dicomweb/studies</code>、取得は <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code>、メタデータ、フレーム、バルクデータ、レンダリング画像用のルートも提供します。サービスルートにある capabilities ドキュメントには、サーバーがサポートするトランザクションが正確に記載されています。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ストレージを接続する">}}

<p>Studies サービスは 3 つのメソッドを持つ 1 つのインターフェイスを必要とします：インスタンスの保存、保存されたファイルの返却、検索候補の返却です。この実装では、各インスタンスをスタディおよびシリーズごとのフォルダー内のファイルとして保持します。</p>

<div class="codeblock" id="code">
 <h3>QIDO-RS、WADO-RS、STOW-RS の背後にあるフォルダーアーカイブ - C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>ライブラリは、プロバイダーを呼び出す前にすべてのリクエストパスおよびアップロードされたインスタンスの UID を検証します。無効な UID を持つインスタンスは失敗として報告され、<code>StoreAsync</code> に到達することはありません。</p>

<p>サーバーの試用にはフォルダーのみで十分です。実運用のアーカイブでも同じ 3 つのメソッドを使用し、<code>StoreAsync</code> と <code>RetrieveAsync</code> の背後にオブジェクトストアを、<code>FindCandidatesAsync</code> の背後にデータベースインデックスを配置します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="検索：ライブラリは DICOM マッチングルールを適用します">}}

<p>QIDO-RS のマッチングは単なる文字列比較以上のものです：ワイルドカード、日付・時間範囲、UID のリスト、あいまいな人物名、空の値、シーケンス内のキーなどです。プロバイダーはこれらを実装していません。プロバイダーは安定した順序で候補を返し、マッチ以上の候補を返すことがあります。ライブラリは各候補に対して正確な DICOM マッチングを適用し、要求されたフィールドを保持し、<code>offset</code> と <code>limit</code> を適用します。デフォルトでは 1 レスポンスあたり最大 1000 件が含まれ、<code>DicomWebStandardServiceOptions</code> でこの上限を変更できます。</p>

<p>大規模アーカイブで検索を高速に保つため、選択的キーをデータベースに渡します。<code>search.Plan.Filter</code> にはコンパイルされた検索キーが格納され、属性、マッチ種別、各値が含まれます：</p>

<div class="codeblock" id="code">
 <h3>検索から正確な Patient ID を取得する - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>このように構築されたデータベースクエリは、変換できないキーを無視した場合などに余分な行を返すことがあります。ライブラリは最終的なマッチでそれらを除外します。真のマッチを削除してはいけません。なぜなら、ライブラリは削除されたマッチを再び追加できないからです。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="保存：各インスタンスごとの結果">}}

<p>STOW-RS リクエストは数百件のインスタンスを含むことができます。ライブラリはマルチパートボディを解析し、各インスタンスに対して順番に <code>StoreAsync</code> を呼び出すため、アップロード全体が一度にメモリに保持されることはありません。各インスタンスについて、プロバイダーは <code>Stored</code>、変更が加えられたインスタンスの場合は <code>Warning</code>、またはオプションの DICOM 理由コードを伴う <code>Failed</code> を返します。</p>

<p>ライブラリはこれらの結果から STOW-RS レポートを作成し、ステータスを選択します：すべてのインスタンスが保存された場合は 200、いくつかが警告または失敗した場合は 202、すべてが失敗した場合は 409。指定されたスタディ名と異なるスタディに属するインスタンスは、プロバイダーが呼び出される前に拒否されます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="取得：転送構文、フレーム、レンダリング画像">}}

<p>プロバイダーは保存されたファイルを返し、ライブラリはクライアントが要求した形式でそれらを提供します。WADO-RS クライアントは <code>Accept</code> ヘッダーで転送構文を指定でき、ライブラリは送信前に各ファイルを変換します（独自のコーデック、HTJ2K、JPEG XL を含む）。DICOM JSON または XML のメタデータ、バルクデータ、単一フレームは同じプロバイダーから取得され、追加のコードは不要です。</p>

<p>レンダリング画像にはさらに 1 つのクラスが必要です。どの画像をサーバーが生成できるかはあなたが決めるからです。このクラスはレンダリングされたインスタンス、フレーム、サムネイル用に JPEG を提供し、ライブラリがそれらをレンダリングします：</p>

<div class="codeblock" id="code">
 <h3>レンダリング JPEG 画像とサムネイルを提供する - C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>組み込みのレンダラは、単一フレームインスタンスまたはリクエストで選択されたフレームから JPEG を生成します。マルチフレームインスタンス全体のレンダリングや他の画像フォーマット、ビューポート、ウィンドウ、ICC プロファイルパラメータは、このクラスで独自の <code>RetrieveAsync</code> を実装する必要があります。</p>

<p>サムネイルは患者プライバシーに配慮して処理されます。組み込みのレンダラは、Burned In Annotation と Recognizable Visual Features 属性が両方とも <code>NO</code> のインスタンスからのみサムネイルを作成します。これは画像自体が患者データを含まないことを示します。多くの画像はこれらの属性を持たず、その場合サーバーは 406 を返します。もしアーカイブでそのような画像のサムネイルが必要な場合は、独自の <code>RetrieveAsync</code> で生成し、患者情報が隠されることを確認してください。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ワークリスト、手技ステップ、およびその他のサービス">}}

<p>他の DICOMweb サービスも同様のモデルに従います。サービスを登録し、対応するストレージインターフェイスを実装します。</p>

<ul>
<li><code>AddStandardWorklistService</code> を使用した UPS-RS ワークリスト：ワークアイテムの作成、検索、更新、状態変更を行い、状態ルールはライブラリがチェックし、イベント通知は WebSocket で送信されます。</li>
<li><code>AddStandardModalityWorklistService</code> を使用したモダリティワークリスト（MWL-RS）。これによりモダリティは RIS のスケジュールされた手技ステップを照会できます。</li>
<li>モダリティ実施手技ステップ（MPPS-RS）、ストレージコミットメント、カラー パレットやハンギングプロトコルなどの非患者インスタンス。</li>
<li>古いビューア向けの WADO-URI 取得（Studies サービスが提供）。</li>
</ul>

<p>あるトランザクションの標準動作が適合しない場合は、<code>AddHandler</code> で独自のハンドラを登録します。ライブラリは引き続きリクエストのルーティングとチェックを行い、他のトランザクションは標準ハンドラを保持します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="セキュリティとホスティングはあなたの手に委ねられます">}}

<p>DICOMweb エンドポイントは通常の ASP.NET Core エンドポイントです。認証、認可、CORS、TLS、転送ヘッダー、リクエストサイズ制限は、サービス全体と同様に設定できます。</p>

<div class="codeblock" id="code">
 <h3>認証済みユーザーを必須にする - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>プロトコル部分は ASP.NET Core に依存しません。<code>DicomWebApplication</code> は <code>DicomWebRequest</code> をレスポンスに変換するため、同じサービスを別の HTTP サーバーの背後やネットワークなしのテスト環境で実行できます。</p>

<div class="codeblock" id="code">
 <h3>ASP.NET Core なしでも同じサーバー - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="クライアントとサーバーが同一ライブラリから">}}

<p>Aspose.Medical の <a href="/medical/net/dicomweb-client/">DICOMweb クライアント</a> は、ユニットテストやコンソールプログラムからサーバーをテストし、サーバーが受信するすべてのファイルは通常の <code>DicomFile</code> であり、同一プロセス内で匿名化、トランスコーディング、レンダリングが可能です。</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb 開発者ガイド</a> ではすべてのサービスとストレージインターフェイスを解説し、<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core ガイド</a> ではエンドポイントのマッピングと WebSocket について説明しています。</p>

<p>関連ページ：HTTP ではなく DIMSE 経由でスタディを送信するシステム向けの <a href="/medical/net/dicom-networking/">DICOM ネットワーキング</a>、受信スタディの <a href="/medical/net/anonymization/">匿名化</a>、<a href="/medical/net/dicom-transfer-syntax-conversion/">転送構文変換</a>、および QIDO-RS 結果が使用する形式の <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学習リソース" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="ドキュメント" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="開発者ガイド" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="API リファレンス" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="製品サポート" tabId="support" >}}
{{< blocks/products/pf/slr-element name="無料サポート" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="有料サポート" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ブログ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle=".NET 向け Aspose.Medical を選ぶ理由" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="顧客一覧" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
