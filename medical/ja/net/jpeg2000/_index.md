---
title: C# .NET における DICOM JPEG 2000 圧縮 | Aspose.Medical
weight: 2000
description: C# .NET で JPEG 2000 圧縮を使用して DICOM ファイルを読み取り、書き込み、トランスコードします。1 から 16 ビットのモノクロおよびカラー画像、ロスレスとロッシーモード、さらに Aspose.Medical API による HTJ2K をサポートします。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# における DICOM JPEG 2000 サポート" h2="JPEG 2000 圧縮を使用して DICOM ファイルを読み取り、書き込み、トランスコードします。ロスレスとロッシーモード、1 から 16 ビットのモノクロおよびカラー画素データ、HTJ2K を含むすべてが純粋な .NET で実現されます。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="医用画像における JPEG 2000">}}

<p><strong>JPEG 2000</strong>（ISO/IEC 15444）は、医用画像で最も広く使用されているウェーブレットベースの圧縮標準です。従来の JPEG とは異なり、単一コーデックでロスレスとロッシーの両方の圧縮を提供し、領域限定アクセスのためのプログレッシブデコードと優れた圧縮率を実現します。これにより、大規模な研究のアーカイブや制約のあるネットワーク上での画像転送に最適です。</p>

<p><strong>Aspose.Medical for .NET</strong>は、ネイティブ依存なしの純粋な C# 実装による JPEG 2000 コーデックを提供します。このライブラリは、JPEG 2000 Part 1 の転送構文で圧縮された DICOM ファイル（ロスレスおよびロッシー）を読み取り、レンダリングし、トランスコードできます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="サポートされている JPEG 2000 転送構文">}}

<table class="table table-bordered">
<thead>
<tr>
<th>転送構文</th>
<th>UID</th>
<th>モード</th>
<th>読み取り</th>
<th>書き込み</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 ロスレスのみ</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>ロスレス</td><td>モノクロおよびカラー、1〜16 ビット</td><td>モノクロ、パレットカラー、RGB、1〜16 ビット</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>ロッシーまたはロスレス</td><td>モノクロおよびカラー、1〜16 ビット</td><td>モノクロおよび RGB、1〜16 ビット</td></tr>
<tr><td>JPEG 2000 Part 2 マルチコンポーネント ロスレスのみ</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>ロスレス</td><td>非対応</td><td>非対応</td></tr>
<tr><td>JPEG 2000 Part 2 マルチコンポーネント</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>ロッシーまたはロスレス</td><td>非対応</td><td>非対応</td></tr>
<tr><td>HTJ2K ロスレスのみ</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>ロスレス</td><td>モノクロおよびカラー</td><td>モノクロおよびカラー</td></tr>
<tr><td>HTJ2K（RPCL オプション） ロスレスのみ</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>ロスレス</td><td>モノクロおよびカラー</td><td>モノクロおよびカラー</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>ロッシーまたはロスレス</td><td>モノクロおよびカラー</td><td>モノクロおよびカラー</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="1 から 16 ビットの画素データ">}}

<p>医用画像は、CT（通常は 12 ビットを 16 ビットで格納）や MRI などのモダリティの全ダイナミックレンジを捉えるために、サンプルあたり 16 ビットを使用することが多いです。Aspose.Medical は、JPEG 2000 用に 1 から 16 ビットまでのすべての深度を、8 ビットまたは 16 ビットサンプルで処理します。</p>

<ul>
<li><strong>読み取り（デ圧縮）</strong>：1〜16 ビット／サンプル、符号付きまたは符号なしのモノクロファイル（CT、MRI、X線）、パレットカラー画像および 3 コンポーネントカラー画像（RGB、YBR_RCT、YBR_ICT、その他 YBR_FULL などのカラーラベル、コード化されたものとしてデコード）です。CMYK、ICC プロファイル、JP2 パレット、サブサンプリングされたカラーコードストリームは、サイレントに誤った画像になる代わりに明確な例外で拒否されます。</li>
<li><strong>書き込み（圧縮）</strong>：1〜16 ビット／サンプル、符号付きまたは符号なしのモノクロおよび RGB 画像、そして JPEG 2000 ロスレスでのパレットカラー画像です。YBR 画像は、RGB にデコードされる場合を除きエンコードされません（ロッシー JPEG YBR ファイルと同様）。</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 圧縮 DICOM の読み取りと検査 - C#</h3>
 <pre><code class="cs">// Open a JPEG 2000 compressed DICOM file (8-bit or 16-bit)
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Check transfer syntax
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax
Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");

// Inspect pixel data bit depth
PixelData pixelData = PixelData.Create(dicomFile.Dataset);
Console.WriteLine($"Bits Allocated: {pixelData.BitsAllocated}");
Console.WriteLine($"Bits Stored: {pixelData.BitsStored}");
Console.WriteLine($"High Bit: {pixelData.HighBit}");
Console.WriteLine($"Samples Per Pixel: {pixelData.SamplesPerPixel}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 にトランスコード">}}

<p><code>Transcode</code> メソッドを使用して任意の DICOM ファイルを JPEG 2000 に圧縮したり、JPEG 2000 モード間で変換したりできます。</p>

<div class="codeblock" id="code">
 <h3>DICOM を JPEG 2000 ロスレスに圧縮 - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>DICOM を JPEG 2000 ロッシーに圧縮 - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM ファイルのデ圧縮">}}

<p>JPEG 2000 ファイルを非圧縮の転送構文にデ圧縮して、処理・解析、または JPEG 2000 をサポートしないシステムとの互換性を確保します。</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 を非圧縮にデ圧縮 - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>また、1 歩でデ圧縮と他の圧縮形式へのトランスコードを同時に実行できます。</p>

<div class="codeblock" id="code">
 <h3>圧縮形式間のトランスコード - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM 画像のレンダリング">}}

<p>JPEG 2000 圧縮 DICOM ファイルは、他の転送構文と同様に、表示やエクスポート用の画素データにレンダリングできます。</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 圧縮フレームをレンダリング - C#</h3>
 <pre><code class="cs">// Open JPEG 2000 DICOM file
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Render the first frame — decompression is handled automatically
using PixelImage&lt;Bgra32&gt; image = dicomFile.RenderImage(0);

// Copy the BGRA32 pixel data for display or export
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = new Bgra32[width * height];
image.CopyPixelsTo(pixels);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ロスレス vs ロッシー JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>項目</th>
<th>JPEG 2000 ロスレス</th>
<th>JPEG 2000 ロッシー</th>
</tr>
</thead>
<tbody>
<tr><td>転送構文</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>画質</td><td>ピクセル単位で完全一致 &mdash; 元画像と同一</td><td>見た目は類似するが、一部データは永久に失われる</td></tr>
<tr><td>圧縮率</td><td>通常 2:1 から 3:1</td><td>通常 10:1 から 30:1 以上</td></tr>
<tr><td>適した用途</td><td>診断用アーカイブ、法的記録、一次読影</td><td>予備的レビュー、遠隔医療、ネットワーク転送</td></tr>
<tr><td>往復安全性</td><td>はい</td><td>いいえ &mdash; 再エンコードするとさらに品質が低下します</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="高速スループット JPEG 2000 (HTJ2K)">}}

<p>HTJ2K（ISO/IEC 15444-15）は、JPEG 2000 の遅い算術コーダーを高速なブロックコーダーに置き換えます。ウェーブレット変換、プログレッション順序、品質は同じで、デコードおよびエンコードが数倍速くなります。Aspose.Medical は、純粋な .NET で DICOM HTJ2K の 3 つの転送構文すべてを実装し、モノクロおよびカラー画像に対応し、HTJ2K と他のすべてのサポート構文間のトランスコードを提供します。</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; ロスレスのみ</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; RPCL プログレッション順序のロスレス</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; ロッシーまたはロスレス</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 と HTJ2K の相互トランスコード - C#</h3>
 <pre><code class="cs">// Transcode a JPEG 2000 file to HTJ2K, and back to classic JPEG 2000
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile htj2kFile = j2kFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2kFile.Save("htj2k_lossless.dcm");

// HTJ2K with RPCL progression order, lossless
DicomFile rpclFile = j2kFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
rpclFile.Save("htj2k_rpcl.dcm");

// HTJ2K lossy
DicomFile htj2kLossy = j2kFile.Transcode(TransferSyntax.HTJ2K);
htj2kLossy.Save("htj2k_lossy.dcm");

// Any HTJ2K file decodes back to an uncompressed transfer syntax
DicomFile uncompressed = htj2kFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decoded.dcm");</code></pre>
</div>

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

{{< blocks/products/pf/slr-tab tabTitle=".NET 向け Aspose.Medical を選ぶ理由" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="顧客一覧" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
