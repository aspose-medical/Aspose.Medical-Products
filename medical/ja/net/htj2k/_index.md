---
title: HTJ2K in C# .NET - DICOM 用ハイスループット JPEG 2000 | Aspose.Medical
weight: 10000

description: C# からハイスループット JPEG 2000 で DICOM 画像を圧縮・読み取り。ロスレス HTJ2K、RPCL 変種、ロスィ HTJ2K を、ネイティブコーデック不要のマネージド .NET で実装しています。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# における HTJ2K" h2="DICOM 用ハイスループット JPEG 2000：高速アーカイブとクラウド閲覧のために標準に追加された圧縮方式で、ネイティブ要素なしのマネージド C# で実装されています。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="HTJ2K の変更点">}}

<p>ハイスループット JPEG 2000 は JPEG 2000 のウェーブレットと画像品質を保持しつつ、処理速度を遅くしていた部分を置き換えます。ブロックコーダーが新しく、デコードは桁違いに高速化されました。このため DICOM 標準は 3 つの転送構文で採用し、クラウド画像プラットフォームも採用しています。</p>

<p>.NET チームにとって実務的な疑問は「誰が実際にそのファイルを生成できるか」です。ほとんどのライブラリはネイティブ OpenJPH ビルドを介して HTJ2K にアクセスするため、プラットフォームごとのバイナリやコンテナ内でのビルド工程、セキュリティレビューで問われる依存関係が発生します。<strong>Aspose.Medical for .NET</strong> はコーデックを同一パッケージ内のマネージドコードで実装しているため、Windows、Linux、コンテナいずれでもインストール不要で HTJ2K が動作します。</p>

<p>3 つの転送構文がサポートされ、すべて読み取りと書き込みの両方に対応しています。</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201)、ハイスループット JPEG 2000 ロスレス。</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202)、RPCL 進行順序を持つロスレス変種。</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203)、ハイスループット JPEG 2000。</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="研究（スタディ）を HTJ2K に圧縮">}}

<p>1 回の呼び出しでファイルを新しい構文へ変換します。データセット、プライベートタグ、ファイルメタ情報がすべて引き継がれます。</p>

<div class="codeblock" id="code">
 <h3>DICOM ファイルを HTJ2K にトランスコード - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>自前のテストセットの 1714 × 1933、16 ビット画像で、ファイルサイズは 6.3 MB から 2.9 MB に削減され、ピクセルはビット単位で元通りです。数値はモダリティや画像により異なるため、既存のファイル群で自分のデータを測定してください。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ロスレスはロスレスである">}}

<p>診断データはほぼ正確でないコーデックを許容できません。HTJ2K ロスレスでトランスコードし元に戻すと、ピクセルデータは開始時のバイトと完全に一致します。この特性は、アーカイブを再圧縮する前に自分のテストスイートで検証できます。</p>

<div class="codeblock" id="code">
 <h3>非圧縮構文に戻す - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL、ネットワーク上での閲覧向け変種">}}

<p>1.2.840.10008.1.2.4.202 構文は同じロスレスコードストリームを RPCL 進行順序（解像度→位置→成分→レイヤ）で保存します。ストリームの冒頭だけを取得したリーダーでも低解像度画像全体を得られるため、リンク先が制御できない大規模スタディを開くビューアに最適です。</p>

<div class="codeblock" id="code">
 <h3>RPCL 進行順序で圧縮 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="アーカイブが送信するものを読み取る">}}

<p>作業のもう片方は、既に HTJ2K を生成するシステムからの入力を受け入れることです。ファイルを開き、どの構文で保存されているか確認し、ピクセルデータを処理します。</p>

<div class="codeblock" id="code">
 <h3>HTJ2K ファイルを読み取る - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>マルチフレーム画像はフレーム単位で処理されるため、長いシリーズでもスタディ全体ではなくフレームごとのメモリ使用となります。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTJ2K が価値を発揮する場面">}}

<ul>
<li>アーカイブ移行：保存されたスタディを HTJ2K ロスレスに再圧縮し、容量を削減しつつ診断データを保持。</li>
<li>クラウドおよび DICOMweb：デコード速度が高速なため、ブラウザ側・サーバ側ビューアでも大画像を即時に表示可能。</li>
<li>AI パイプライン：トレーニングデータは書き込みよりも読み取りが圧倒的に多く、デコード時間が繰り返し発生するコストとなります。</li>
<li>コンテナとサーバーレス：コーデックがアセンブリに組み込まれるため、イメージにネイティブライブラリやコンパイラを組み込む必要がありません。</li>
</ul>

<p>このライブラリは JPEG XL（標準への最近の追加）も提供し、アーカイブで一般的に使用される旧コーデック（JPEG、JPEG‑LS、JPEG 2000、RLE）もサポートします。<a href="/medical/net/dicom-transfer-syntax-conversion/">転送構文変換</a>ページで全体を解説し、<a href="/medical/net/jpeg2000/">JPEG 2000</a>ページで HTJ2K の元となったコーデックを取り上げています。</p>

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
{{< blocks/products/pf/slr-element name="導入事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
