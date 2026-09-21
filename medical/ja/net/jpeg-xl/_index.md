---
title: C# .NET 用 JPEG XL for DICOM | Aspose.Medical
weight: 10500

description: C# から DICOM 画像を JPEG XL で保存します。ピクセルをビット単位でそのまま返すロスレス JPEG XL を、ネイティブ コーデック不要の単一のマネージド アセンブリで提供します。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="C# .NET 用 JPEG XL for DICOM" h2="DICOM 標準で最新の圧縮方式で、測定した中で最小のロスレスファイルを実現し、マネージド C# で実装され、単一のアセンブリにパッケージされています。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL が DICOM に採用された理由">}}

<p>医療アーカイブは増え続け、縮むことはありません。JPEG XL は JPEG と JPEG 2000 の二十年にわたる経験を経て画像業界が設計したコーデックで、DICOM はストレージチームが関心を持つ理由、すなわち同じピクセルでもファイルサイズが小さくなるという点で転送構文として追加しました。</p>

<p><strong>Aspose.Medical for .NET</strong> は、ライブラリ内部にある libjxl の C# ポートを通じて JPEG XL の書き込みと読み取りを行います。パッケージは <code>Aspose.Medical.dll</code> という単一のアセンブリのみを配布し、ネイティブバイナリは付属しません。そのため、このような新しいコーデックでも導入プロジェクト化されず、同じアセンブリが Windows、Linux、ビルドエージェント、コンテナ上で動作します。</p>

<p>ピクセルを運ぶ転送構文が 2 つあります：</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110)、診断データが変更されずに戻る必要がある場合に使用します。</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112)、ファイルサイズの小ささが正確なコピーより重要なケースで使用します。</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="スタディを圧縮し、すべてのピクセルを保持">}}

<p>トランスコーディングは 1 回の呼び出しで完了し、ピクセル周辺のデータセットも一緒に転送されます。</p>

<div class="codeblock" id="code">
 <h3>DICOM ファイルを JPEG XL にトランスコード - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>当社のテストセットから 1714×1933 の 16 ビット画像で測定した結果、非圧縮 6.3 MB が JPEG XL ロスレスで 2.7 MB になり、同じ画像の HTJ2K ロスレスよりも小さくなります。実際の数値はモダリティによって異なるため、選択前にご自身のファイルが入ったフォルダーで比較してください。</p>

<p>「ロスレス」は文字通りに受け止めてください。JPEG XL にトランスコードし、再び戻してもピクセルデータは開始時のバイトと同一になるため、診断品質に関する議論なくアーカイブを再圧縮できます。</p>

<div class="codeblock" id="code">
 <h3>非圧縮構文に戻す - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="既に JPEG XL として保存されているものを読み取る">}}

<p>JPEG XL 形式のファイルは他の形式と同様に開くことができ、転送構文がその形式を示し、フレームがデコードされればピクセルデータが取得可能です。</p>

<div class="codeblock" id="code">
 <h3>JPEG XL ファイルを開く - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL または HTJ2K">}}

<p>どちらも比較的新しく、ロスレスを要求すればロスレスであり、ライブラリは両方の書き込みと読み取りに対応しています。用途に応じて異なる質問に答えます。</p>

<table class="table table-bordered">
<thead>
<tr>
<th>質問</th>
<th>回答</th>
</tr>
</thead>
<tbody>
<tr><td>テストでどちらがより小さなファイルを生成したか</td><td>JPEG XL ロスレスが数パーセント小さい</td></tr>
<tr><td>ネットワーク上でプログレッシブビューを想定しているか</td><td><a href="/medical/net/htj2k/">HTJ2K</a>（特に RPCL バリアント）</td></tr>
<tr><td>どちらが先に DICOM 標準に採用されたか</td><td>HTJ2K。現在、より多くのアーカイブが受け入れています。</td></tr>
<tr><td>どちらがネイティブ依存性を必要とするか</td><td>どちらも不要です。両方とも単一アセンブリのマネージドコードです。</td></tr>
</tbody>
</table>

<p>選択は通常、リンクの反対側、すなわちアーカイブが受け入れる構文にトランスコードし、パイプラインの他の部分はそのままにするという形で決まります。</p>

<div class="codeblock" id="code">
 <h3>対象アーカイブに決定させる - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="効果が現れる場面">}}

<ul>
<li>長期アーカイブ：同じスタディでもテラバイト数が減り、放射線科医に説明する損失もありません。</li>
<li>クラウドストレージ費用：削減分は毎月繰り返され、トランスコーディングは一度だけ実行されます。</li>
<li>研究・AI 用データセット：コピーが小さくなることで、ストレージとトレーニング間の転送が高速化します。</li>
<li>導入：このような新しいコーデックは通常プラットフォームごとにネイティブビルドが必要ですが、ここでは既に参照しているアセンブリの一部として提供されます。</li>
</ul>

<p>ライブラリは既存アーカイブが含むコーデック、すなわち JPEG、JPEG‑LS、JPEG 2000、HTJ2K、RLE も書き込み可能です。<a href="/medical/net/dicom-transfer-syntax-conversion/">転送構文変換</a>ページで全セットをカバーし、<a href="/medical/net/htj2k/">HTJ2K</a> は専用ページ、<a href="/medical/net/jpeg2000/">JPEG 2000</a> は新しいコーデック両方の元となるページです。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学習リソース" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="ドキュメント" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="開発者ガイド" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="API リファレンス" href="https://reference.aspose.com/medical/net/" >}}
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
