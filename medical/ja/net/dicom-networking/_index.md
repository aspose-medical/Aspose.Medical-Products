---
title: C# .NET における DICOM ネットワーキング - C-ECHO、C-STORE、C-FIND | Aspose.Medical
weight: 9000

description: .NET アプリケーションを PACS に接続します。C-ECHO で接続確認、C-STORE で画像送信、C-FIND でクエリ、独自の SCP で画像受信を行います。純粋な C# による DIMSE クライアントとサーバーです。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# における DICOM ネットワーキング" h2="独自アプリケーションから PACS と通信します：C-ECHO、C-STORE、C-FIND、C-MOVE、C-GET をクライアントおよびサーバーとして、マシンに何もインストールせずにマネージド C# で実現します。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="独自コードから PACS に接続する">}}

<p>DICOM ファイルの読み取りは医用画像処理の比較的簡単な側面です。アプリケーションが実際の病院システムと連携する際には、DIMSE を使用する必要があります：PACS とアソシエーションを確立し、画像を送信し、どの検査があるか問い合わせ、別のシステムから返答があった場合に応答します。</p>

<p><strong>Aspose.Medical for .NET</strong> はそのプロトコルをライブラリの一部として提供します。<code>Aspose.Medical.Dicom.Network</code> は DIMSE クライアントと DIMSE サーバーを提供し、どちらもマネージド C# で実装されています。ネイティブツールキットのインストールやサービスの構成は不要で、プラットフォーム依存もありません。そのため、同じコードが Windows、Linux、コンテナ上で動作します。</p>

<p>ほとんどの統合は 3 つの操作でカバーでき、各操作は数行のコードです：C-ECHO でリンク確認、C-STORE で画像プッシュ、C-FIND で相手側のデータ検索。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="まずは C-ECHO から">}}

<p>C-ECHO は DICOM の ping です。ホスト、ポート、2 つの AE タイトルが正しいことを他の問題が発生する前に確認します。クライアントを一度作成し、応答を監視するハンドラを設定して、リクエストを送信します。</p>

<div class="codeblock" id="code">
 <h3>C-ECHO で接続を検証する - C#</h3>
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

<p>ステータス <code>0x0000</code> は成功を意味します。リクエストはキューに蓄積され、単一のアソシエーションで送信されるため、バッチ処理でもアイテムごとに接続を開く必要はありません。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-STORE で画像を送信する">}}

<p>C-STORE は、アプリケーションが画像を生成または受信した後に行う操作で、インスタンスをアーカイブへプッシュします。インスタンスごとにリクエストをキューに入れ、まとめて送信します。</p>

<div class="codeblock" id="code">
 <h3>DICOM ファイルを PACS に送信する - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>提案するプレゼンテーションコンテキストは、アーカイブが受け入れる形式を決定します。圧縮された転送構文が必要な場合は、送信前にトランスコードします（<a href="/medical/net/dicom-transfer-syntax-conversion/">転送構文変換</a> ページ参照）。あるいは <code>AdditionalTransferSyntaxes</code> に代替を列挙し、交渉で選択させます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-FIND で検査を検索する">}}

<p>C-FIND は「アーカイブに何があるか」という質問に答えます。マッチは1件ずつ届き、各マッチには固有の識別子データセットが付随し、最終応答でクエリが終了します。</p>

<div class="codeblock" id="code">
 <h3>患者単位で検査を問い合わせる - C#</h3>
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

<p>同じファクトリが他のクエリレベルも生成します：<code>CreatePatientQuery</code>、<code>CreateSeriesQuery</code>、<code>CreateImageQuery</code>。<code>CreateWorklistQuery</code> はモダリティのワークリストクエリを生成し、スキャン前にモダリティが行う問い合わせです。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="画像受信：独自のストア SCP">}}

<p>このライブラリはサーバー機能も提供します。提供したいサービスのハンドラを登録し、リスニングを開始すると、アプリケーションはモダリティや他の PACS が送信できる DICOM ノードになります。</p>

<div class="codeblock" id="code">
 <h3>受信画像を受け入れる - C#</h3>
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
 <h3>受信データを保存するハンドラ - C#</h3>
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

<p>ハンドラがデータセットに対して行う処理は自由です：ディスクに書き込む、キューに入れる、先に <a href="/medical/net/anonymization/">匿名化 API</a> で匿名化する、またはアーカイブの構文にトランスコードする、など。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ネットワーク API がカバーするその他の機能">}}

<p>上記の 3 つのサービスが一般的に使用されるものです。その他の DIMSE 機能も利用可能です：</p>

<ul>
<li>取得: C-MOVE と C-GET、サブオペレーションのカウントがレスポンスに含まれます。</li>
<li>N 系サービス: N-CREATE、N-SET、N-GET、N-ACTION、N-DELETE、N-EVENT-REPORT。これらはストレージコミットメントや MPPS の基礎となります。</li>
<li>アソシエーション制御: プレゼンテーションコンテキスト、サービスクラスロール、拡張ネゴシエーション、非同期操作ウィンドウ、ユーザーID ネゴシエーション、そして AE タイトルでアソシエーションを拒否できるポリシーフック。</li>
<li>双方向 TLS は <code>TlsInitiatorAuthenticator</code> と <code>TlsAcceptorAuthenticator</code> を通じて提供され、必要に応じて独自の証明書検証が可能です。</li>
<li>TCP 接続からリリースまでの各段階のタイムアウト設定、そしてアソシエーションライフサイクルの通知により、長時間稼働するノードでも発生した事象をログに記録できます。</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">DICOM ネットワーキング ガイド</a> では、すべてのオプションとハンドラが詳述されています。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ソケットからピクセルデータまで、純粋な .NET">}}

<p>このページのすべては、ファイルの入出力を行う同一パッケージのマネージドコードです。アソシエーション、コーデック、パーサはすべて同一ライブラリから提供されるため、ネットワーク経由で受信した検査をプロセス外に出さず、またネイティブ依存なしで匿名化、トランスコード、シリアライズできます。</p>

<p>関連ページ: 送信対象の <a href="/medical/net/dicom-transfer-syntax-conversion/">転送構文変換</a>、先に除去すべき情報の <a href="/medical/net/anonymization/">匿名化</a>、受信データの読み取りに関する <a href="/medical/net/dicom-tags/">DICOM タグ</a>。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学習リソース" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="ドキュメント" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="開発者ガイド" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="API リファレンス" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="製品サポート" tabId="support" >}}
{{< blocks/products/pf/slr-element name="無料サポート" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="有料サポート" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ブログ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="なぜ Aspose.Medical for .NET なのか？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="顧客リスト" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
