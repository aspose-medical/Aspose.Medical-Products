---
title: C# .NET で大容量 DICOM ファイルを扱う | Aspose.Medical
weight: 11500

description: C# でマルチフレームの検査や全スライド画像をメモリにロードせずに開く。ピクセルデータなしでメタデータを読み取り、大きな要素は遅延させ、ファイルをストリームやパイプで転送する。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# における大容量 DICOM ファイル" h2="マルチフレーム検査のメタデータをピクセルなしで読み取り、大きな要素は必要になるまで遅延させ、ファイル全体をストリームやパイプで転送する。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="ファイルは大きいが、問いはたいてい小さい">}}

<p>全スライド画像、長大な CT シリーズ、または OCT ボリュームは数百メガバイトであり、その大部分はピクセルデータです。アプリケーションが実際に行う作業は、フォルダー内の項目一覧取得、患者識別子の確認、フレーム数のカウント、検査の配置先決定など、はるかに小さなものです。そのために全バイトを読み込むことが、シンプルな処理をメモリ問題に変えてしまいます。</p>

<p><strong>Aspose.Medical for .NET</strong> は呼び出し側がファイルのどれだけを読み取るかを決定できるようにします。その選択は <code>DicomFile.Open</code> の引数一つで行われ、ファイル、ストリーム、パイプすべてに同様に適用されます。</p>

<p>同一マシン・同一ファイルで、テストセットの 14 MB、128 フレームの検査を対象に測定した結果：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>読み取り戦略</th>
<th>オープン時間</th>
<th>割り当てメモリ</th>
</tr>
</thead>
<tbody>
<tr><td>すべて（デフォルト）</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>大きな要素をスキップ</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>大きな要素を遅延</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>ファイルが大きくなるほど差は拡大します。10,000 件の検査が入ったフォルダーは、もはやマイクロ最適化では済まないケースです。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="メタデータを読み取り、ピクセルはそのままにする">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> はサイズ閾値を超えるすべての要素を読み取りから除外します。返されるデータセットには、インデックスやルータが必要とするタグだけが含まれます。</p>

<div class="codeblock" id="code">
 <h3>ピクセルデータなしで検査を読む - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>閾値はデフォルトで 64 kB で、単位はキロバイトです。そのため、8 kB を大きいとみなすワークフローは閾値を設定すれば対応できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="スキップではなく遅延">}}

<p>ピクセルが後で必要になる可能性はあるが、すべてが必要とは限らない場合、<code>ReadLargeOnDemand</code> が対になる選択肢です。ファイルを開くコストはスキップ時と同じで、大きな要素はコードがアクセスした瞬間に読み取られます。</p>

<div class="codeblock" id="code">
 <h3>フレームが使用されたときにだけロードする - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>遅延読み取りは有償機能です。他の戦略は評価版でも利用できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ピクセルに触れずにフォルダーをインデックス化">}}

<p>同じ戦略はストリームにも適用でき、アーカイブのスキャンやクラウドオブジェクトストアからのアクセスはコード上ではストリームとして扱われます。</p>

<div class="codeblock" id="code">
 <h3>アーカイブをスキャンする - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ストリームとパイプ、入出力">}}

<p>読み取りも書き込みもストリームを受け付け、非同期エントリポイントは <code>System.IO.Pipelines</code> 型もサポートします。検査はネットワーク応答からストレージへと、プロセスがファイル全体を単一配列として保持することなく転送可能です。</p>

<div class="codeblock" id="code">
 <h3>ストリームを介した読み書き - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>同様の考え方はテキスト表現にも適用されます。多数のデータセットを含むドキュメントは、<a href="/medical/net/json-to-dicom/">JSON to DICOM</a> と <a href="/medical/net/xml-to-dicom/">XML to DICOM</a> のページで、データセット単位で順次読み取られます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="フレーム単位で">}}

<p>マルチフレームデータはフレーム単位でアドレス指定できるため、500 フレームのシリーズでも全ピクセルデータ要素を読み込むのではなく、必要なフレームだけを逐次取得します。</p>

<div class="codeblock" id="code">
 <h3>フレームを巡回する - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="設計を左右するポイント">}}

<ul>
<li>アーカイブのインデックス作成と移行: 数百万のファイルがあり、何かが移動されるまでヘッダーだけが重要です。</li>
<li>ルータやストアノード: 検査を受け取り、ルーティングに必要な情報だけを読み取り、バイトを転送します。</li>
<li>AI パイプライン: メタデータからマニフェストを構築し、実際に学習に使用するサブセットのフレームだけを取得します。</li>
<li>メモリ制限付きコンテナ: ワーキングセットはファイルサイズではなく、選択した戦略に従います。</li>
<li>全スライドおよび OCT データ: すべてを読み込むことが不可能なファイル。</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">メモリ管理ガイド</a> では各戦略を詳しく解説しており、<a href="/medical/net/dicom-networking/">DICOM ネットワーキング</a> では同様のデータが DIMSE 経由で到着する様子を示しています。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学習リソース" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="ドキュメント" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="開発者ガイド" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
{{< blocks/products/pf/slr-element name="API リファレンス" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="製品サポート" tabId="support" >}}
{{< blocks/products/pf/slr-element name="無料サポート" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="有料サポート" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ブログ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="なぜ Aspose.Medical for .NET を選ぶのか？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="顧客一覧" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
