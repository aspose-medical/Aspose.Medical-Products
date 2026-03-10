---
title: C# .NETでDICOMをPNGに変換 | Aspose.Medical
weight: 11000
description: C# .NETでDICOM画像をピクセルデータにレンダリングし、ロスレス品質、ウィンドウ/レベル制御、マルチフレーム対応を実現します。Aspose.Medical APIを使用して完全なDICOM LUTパイプラインを利用し、PNGに保存します。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="C# .NETでDICOMをPNGに変換" h2="完全なグレースケールパイプラインをサポートしてDICOM画像を生ピクセルデータにレンダリングします。ウィンドウ/レベル制御、オーバーレイレンダリング、マルチフレーム処理で画像品質を保護します。任意の.NETイメージングライブラリを使用してロスレスPNGに保存します。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOMからPNGへの変換">}}

<p><strong>Aspose.Medical for .NET</strong>は標準のDICOMグレースケールパイプライン（Modality LUT、VOI LUT、Presentation LUT、Output LUT）を介してDICOM画像をレンダリングし、正しくウィンドウ処理されたBGRA32ピクセルデータを生成します。PNGはロスレス形式であり、圧縮アーティファクトなしに画像品質を維持する必要がある場合に最適な選択肢です&mdash;診断レビュー、アーカイブ、研究用途に不可欠です。</p>

<p>レンダリングされた出力は<code>IRawImage</code>です&mdash;<code>Width</code>、<code>Height</code>、および<code>Pixels</code>配列を持つBGRA32ピクセルバッファです。このピクセルデータは、<code>System.Drawing</code>、<code>SkiaSharp</code>、<code>ImageSharp</code>など任意の.NETイメージングライブラリを使用してPNGに保存できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#でDICOMをピクセルデータにレンダリング">}}

<p>DICOMファイルを読み込み、目的のフレームをレンダリングし、ピクセルデータにアクセスします：</p>

<div class="codeblock" id="code">
 <h3>DICOM画像をレンダリング - C#</h3>
 <pre><code class="cs">using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;

// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Render the first frame (index 0) through the full LUT pipeline
IRawImage image = dicomFile.RenderImage(0);

// Access the rendered pixel data
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = image.Pixels; // BGRA32 pixel buffer</code></pre>
</div>

<p><code>RenderImage</code>メソッドは、DICOMデータセットに保存されたウィンドウ/レベル値とLUTパラメータを自動的に適用します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="レンダリング画像をPNGとして保存">}}

<p><code>IRawImage</code>は、任意の.NETイメージングライブラリを使用してロスレスPNGに保存できる生のBGRA32ピクセルデータを提供します。以下は<code>System.Drawing</code>を使用した例です：</p>

<div class="codeblock" id="code">
 <h3>レンダリングされたDICOMフレームをPNGとして保存 - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");
IRawImage image = dicomFile.RenderImage(0);

// Create a Bitmap from the BGRA32 pixel data
using var bitmap = new Bitmap(image.Width, image.Height, PixelFormat.Format32bppArgb);
var bitmapData = bitmap.LockBits(
    new Rectangle(0, 0, image.Width, image.Height),
    ImageLockMode.WriteOnly,
    PixelFormat.Format32bppArgb);

// Copy BGRA32 pixels to the bitmap
MemoryMarshal.AsBytes(image.Pixels.AsSpan())
    .CopyTo(new Span&lt;byte&gt;(
        (void*)bitmapData.Scan0,
        bitmapData.Stride * bitmapData.Height));

bitmap.UnlockBits(bitmapData);

// Save as lossless PNG
bitmap.Save("ct_scan.png", ImageFormat.Png);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="カスタムウィンドウ/レベル">}}

<p>Window CenterとWindow Widthを設定して、レンダリングされるピクセル値の範囲を制御します。異なるウィンドウ設定により、同じDICOMデータから異なる解剖学構造が明らかになります。</p>

<div class="codeblock" id="code">
 <h3>カスタムウィンドウ/レベルでレンダリング - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);</code></pre>
</div>

<p><code>GrayscaleRenderOptions</code>クラスは、<code>RescaleSlope</code>、<code>RescaleIntercept</code>、<code>Invert</code>、<code>BitDepth</code>プロパティも公開しており、レンダリングパイプラインをフルコントロールできます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="マルチフレーム DICOM レンダリング">}}

<p>CT、MRI、超音波のDICOMファイルはしばしば複数のフレーム（スライス）を含みます。すべてのフレームをピクセルデータにレンダリングします：</p>

<div class="codeblock" id="code">
 <h3>マルチフレーム DICOM のすべてのフレームをレンダリング - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as PNG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="オーバーレイ制御">}}

<p>DICOM画像には、注釈、測定、または関心領域を含むオーバーレイ平面が含まれることがあります。レンダリング時にオーバーレイの表示可否と色を制御します：</p>

<div class="codeblock" id="code">
 <h3>オーバーレイあり・なしでレンダリング - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible (default)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions { ShowOverlays = false };
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="医用画像におけるPNGとJPGの比較">}}

<p>PNGとJPGの選択は使用ケースに依存します：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>特性</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>圧縮</strong></td><td>ロスレス</td><td>有損</td></tr>
<tr><td><strong>画像品質</strong></td><td>ピクセル完璧 &mdash; アーティファクトなし</td><td>軽度の圧縮アーティファクトが発生する可能性あり</td></tr>
<tr><td><strong>ファイルサイズ</strong></td><td>大きい（通常 JPG の 2&ndash;10 倍）</td><td>小さい</td></tr>
<tr><td><strong>透過性</strong></td><td>対応（アルファチャンネル）</td><td>非対応</td></tr>
<tr><td><strong>推奨用途</strong></td><td>診断レビュー、アーカイブ、研究、オーバーレイ</td><td>Web共有、サムネイル、プレビュー</td></tr>
<tr><td><strong>規制面</strong></td><td>品質が重要なワークフローに推奨</td><td>診断以外の用途で許容可能</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NETは、両方の出力先に同一のレンダリングパイプラインを使用します。<code>IRawImage</code>のピクセルデータはターゲットフォーマットに関係なく同一です—フォーマットの選択は、イメージングライブラリが実行する最終的な保存ステップにのみ影響します。</p>

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

{{< blocks/products/pf/slr-tab tabTitle="なぜ Aspose.Medical for .NETなのか？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="顧客一覧" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
