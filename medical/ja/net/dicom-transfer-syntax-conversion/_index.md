---
title: C# .NET における DICOM 転送構文変換 | Aspose.Medical
weight: 16000
description: C# .NET で DICOM ファイルを転送構文間でトランスコードします。Aspose.Medical API を使用して JPEG、JPEG 2000、JPEG-LS、RLE、非圧縮形式をサポートします。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="C# .NET における DICOM 転送構文変換" h2="非圧縮、JPEG、JPEG 2000、JPEG-LS、RLE の転送構文間で DICOM ファイルをトランスコードします。ネイティブ依存なしの純粋な .NET ライブラリです。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="転送構文とは何ですか？">}}

<p><strong>Transfer Syntax</strong> は、DICOM データが保存および送信のためにどのようにエンコードされるかを定義します。バイト順（エンディアン）、Value Representation が明示的か暗黙的か、そしてピクセルデータに適用される圧縮アルゴリズムという、3 つの主要な側面を指定します。すべての DICOM ファイルは File Meta Information ヘッダーで転送構文を宣言しています。</p>

<p>さまざまな医療機器、PACS サーバー、ビューイングアプリケーションは、異なる転送構文セットをサポートしています。<strong>Aspose.Medical for .NET</strong> は <code>Transcode</code> メソッドを提供し、転送構文間の変換を可能にし、相互運用性、ストレージ最適化、処理ツールとの互換性を実現します &mdash; すべてネイティブ依存なしの純粋な .NET ライブラリです。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# で DICOM ファイルをトランスコード">}}

<p><code>DicomFile.Transcode</code> メソッドは、DICOM ファイルを現在の転送構文からサポートされている任意の対象構文へ変換します。このメソッドは新しい <code>DicomFile</code> インスタンスを返します &mdash; 元のファイルは変更されません。</p>

<div class="codeblock" id="code">
 <h3>基本的な DICOM トランスコーディング - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p><code>Dataset</code> レベルでも直接トランスコードできます：</p>

<div class="codeblock" id="code">
 <h3>Dataset をトランスコード - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="サポートされている転送構文">}}

<p>以下の表は、標準的な DICOM 画像データ転送構文と、Aspose.Medical for .NET における現在のサポート状況を示しています。サポートされているすべてのコーデックは純粋な C# で実装されており、完全にプラットフォームに依存しません。</p>

<table class="table table-bordered">
<thead>
<tr>
<th>転送構文</th>
<th>UID</th>
<th>タイプ</th>
<th>ステータス</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>非圧縮</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>非圧縮</td><td>サポート</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>非圧縮</td><td>サポート</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>非圧縮</td><td>サポート</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>サポート</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>非可逆、8 ビット</td><td>サポート</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>非可逆、12 ビット</td><td>未サポート</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>ロスレス</td><td>サポート (8 ビットのみ)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>ロスレス</td><td>サポート (8 ビットのみ)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>ロスレス</td><td>サポート</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>ほぼロスレス</td><td>サポート</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>ロスレス</td><td>サポート (読み取り 8/16 ビット、書き込み 8 ビット)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>ロスリーまたはロスレス</td><td>サポート (読み取り 8/16 ビット、書き込み 8 ビット)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>ロスレス</td><td>サポート (読み取り 8/16 ビット、書き込み 8 ビット)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>ロスリーまたはロスレス</td><td>サポート (読み取り 8/16 ビット、書き込み 8 ビット)</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>ロスレス</td><td>サポート</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>ロスレス</td><td>近日公開</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>ロスレス</td><td>近日公開</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>ロスリーまたはロスレス</td><td>近日公開</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>ロスレス</td><td>近日公開</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>ロスレス</td><td>近日公開</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>ロスリーまたはロスレス</td><td>近日公開</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="一般的なトランスコーディングシナリオ">}}

<p>ワークフローごとに異なるトランスコード戦略が必要です。代表的なシナリオを以下に示します。</p>

<div class="codeblock" id="code">
 <h3>処理用にデコード - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>アーカイブ保存用に圧縮 - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>ネットワーク転送用に圧縮 - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="転送構文プロパティの確認">}}

<p><code>TransferSyntax</code> クラスは、エンコード特性を示すプロパティを公開します。これらを使用してファイルの現在の転送構文を確認したり、適切な対象構文を選択したりできます。</p>

<div class="codeblock" id="code">
 <h3>転送構文プロパティの読み取り - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>プロパティ</th>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>転送構文の固有識別子</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Value Representation が明示的にエンコードされているかどうか</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>バイト順がリトルエンディアンかどうか</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>ピクセルデータがカプセル化（圧縮）されているかどうか</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>圧縮方式がロッシーかどうか</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>構文が Deflate 圧縮を使用しているかどうか</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>転送構文が DICOM 標準で廃止されているかどうか</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>ロッシー圧縮方式の ISO 標準識別子</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ロッシー圧縮とロスレス圧縮の比較">}}

<p>DICOM ファイルをトランスコードする際、ロッシー圧縮とロスレス圧縮の違いを理解することは重要です。</p>

<table class="table table-bordered">
<thead>
<tr>
<th>側面</th>
<th>ロスレス</th>
<th>ロッシー</th>
</tr>
</thead>
<tbody>
<tr><td>画像品質</td><td>ピクセル単位で完全一致 &mdash; 元データが完全に保持される</td><td>サイズ縮小のためにデータが一部永久に失われる</td></tr>
<tr><td>圧縮率</td><td>通常 2:1〜3:1</td><td>通常 10:1〜30:1 以上</td></tr>
<tr><td>往復安全性</td><td>はい &mdash; デコードすればピクセルが完全に同一</td><td>いいえ &mdash; ロッシー再エンコードごとに品質がさらに劣化</td></tr>
<tr><td>利用シーン</td><td>アーカイブ、診断、法的記録</td><td>予備審査、遠隔医療、ネットワーク転送</td></tr>
<tr><td>サポートされるコーデック</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>重要:</strong> ロッシー圧縮されたファイルをロスレス構文にトランスコードしても、失われたデータは復元されません。元のロッシー圧縮による品質低下は永久的です。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学習リソース" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="ドキュメント" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="ソースコード" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API リファレンス" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="製品サポート" tabId="support" >}}
{{< blocks/products/pf/slr-element name="無料サポート" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="有料サポート" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ブログ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="なぜ Aspose.Medical for .NET なのか？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="導入実績" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
