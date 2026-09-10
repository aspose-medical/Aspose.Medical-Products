---
title: C# .NET における DICOM 転送構文変換 | Aspose.Medical
weight: 16000
description: C# .NET で転送構文間の DICOM ファイルをトランスコードします。Aspose.Medical API を使用して JPEG、JPEG 2000、HTJ2K、JPEG XL、JPEG-LS、RLE、非圧縮フォーマットをサポートします。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# における DICOM 転送構文変換" h2="非圧縮、JPEG、JPEG 2000、HTJ2K、JPEG XL、JPEG-LS、RLE の転送構文間で DICOM ファイルをトランスコードします。ネイティブ依存性のない純粋な .NET ライブラリです。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="転送構文とは何ですか？">}}

<p>Transfer Syntax（転送構文）は、DICOM データが保存および送信のためにどのようにエンコードされるかを定義します。バイト順序（エンディアン）、Value Representation が明示的か暗黙的か、ピクセルデータに適用される圧縮アルゴリズムという、3 つの主要な側面を指定します。すべての DICOM ファイルは File Meta Information ヘッダーでその転送構文を宣言しています。</p>

<p>医療機器、PACS サーバー、ビューアアプリケーションは、さまざまな転送構文セットをサポートしています。<strong>Aspose.Medical for .NET</strong> は <code>Transcode</code> メソッドを提供し、転送構文間の変換を行うことで相互運用性、保存最適化、処理ツールとの互換性を実現します — すべてネイティブ依存性のない純粋な .NET ライブラリです。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# で DICOM ファイルをトランスコードする">}}

<p><code>DicomFile.Transcode</code> メソッドは、DICOM ファイルを現在の転送構文から任意のサポート対象構文へ変換します。このメソッドは新しい <code>DicomFile</code> インスタンスを返します — 元のファイルは変更されません：</p>

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

<p>データセットレベルで直接トランスコードすることもできます：</p>

<div class="codeblock" id="code">
<h3>データセットをトランスコードする - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="サポートされている転送構文">}}

<p>以下の表は、すべての標準 DICOM 画像データ転送構文と、Aspose.Medical for .NET における現在のサポート状況を示しています。サポートされているすべてのコーデックは純粋な C# で実装されており、プラットフォームに依存しません。</p>

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
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Uncompressed</td><td>Supported</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Uncompressed</td><td>Supported</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Uncompressed (retired)</td><td>Supported</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Uncompressed</td><td>Not supported</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Supported</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Lossy, 8-bit</td><td>Supported</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Lossy, 12-bit</td><td>Not supported</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Lossless</td><td>Supported (8-bit only)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Lossless</td><td>Supported (8-bit only)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Near-lossless</td><td>Supported</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>Supported (read 8-bit color and 16-bit monochrome; write 16-bit monochrome or 8-bit RGB)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy or lossless</td><td>Supported (read 8-bit color and 16-bit monochrome; write 16-bit monochrome or 8-bit RGB)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Not supported</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy or lossless</td><td>Not supported</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy or lossless</td><td>Supported</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Lossless</td><td>Decode only (encoding needs a JPEG source stream, not pixel data)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Lossy or lossless</td><td>Supported (lossy mode)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="一般的なトランスコーディングシナリオ">}}

<p>異なるワークフローでは異なるトランスコーディング戦略が必要です。最も一般的なシナリオは次のとおりです：</p>

<div class="codeblock" id="code">
<h3>処理用にデコードする - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
<h3>アーカイブ保存用に圧縮する - C#</h3>
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
<h3>ネットワーク送信用に圧縮する - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
<h3>最新のコーデックを使用する：HTJ2K と JPEG XL - C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="転送構文プロパティの検査">}}

<p><code>TransferSyntax</code> クラスは、エンコード特性を示すプロパティを公開します。これらを使用してファイルの現在の転送構文を検査したり、適切なターゲット構文を選択したりします：</p>

<div class="codeblock" id="code">
<h3>転送構文プロパティを読み取る - C#</h3>
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
<th>型</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>転送構文の一意識別子</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Value Representation が明示的にエンコードされているかどうか</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>バイト順序がリトルエンディアンかどうか</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>ピクセルデータがカプセル化（圧縮）されているかどうか</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>圧縮方式が非可逆かどうか</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>構文が Deflate 圧縮を使用しているかどうか</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>転送構文が DICOM 標準で廃止されているかどうか</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>非可逆圧縮方式の ISO 標準識別子</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="非可逆と可逆の圧縮">}}

<p>DICOM ファイルのトランスコーディング時に、非可逆と可逆の圧縮の違いを理解することは極めて重要です：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>側面</th>
<th>可逆</th>
<th>非可逆</th>
</tr>
</thead>
<tbody>
<tr><td>画像品質</td><td>ピクセル単位で完全に元データが保持される</td><td>サイズ削減のために一部データが永久に失われる</td></tr>
<tr><td>圧縮率</td><td>一般的に 2:1〜3:1</td><td>一般的に 10:1〜30:1 以上</td></tr>
<tr><td>ラウンドトリップ安全性</td><td>はい — デコードすると同一ピクセルが得られる</td><td>いいえ — 非可逆再エンコードのたびに品質が低下する</td></tr>
<tr><td>使用ケース</td><td>アーカイブ、診断、法的記録</td><td>予備レビュー、遠隔医療、ネットワーク送信</td></tr>
<tr><td>サポートされるコーデック</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>重要：</strong>非可逆圧縮されたファイルを可逆構文にトランスコードしても、失われたデータは復元されません。元の非可逆圧縮による品質劣化は永久的です。</p>

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
{{< blocks/products/pf/slr-element name="顧客一覧" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
