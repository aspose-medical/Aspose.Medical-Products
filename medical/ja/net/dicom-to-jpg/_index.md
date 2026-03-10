---
title: C# .NET で DICOM を JPG に変換 | Aspose.Medical
weight: 7000
description: ウィンドウ/レベル制御、オーバーレイ描画、マルチフレームサポートを備えた C# .NET で DICOM 画像をピクセルデータへレンダリングします。Aspose.Medical API の完全な DICOM LUT パイプラインを使用し、JPG に保存します。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# で DICOM を JPG に変換" h2="ウィンドウ/レベル、モダリティ LUT、VOI LUT、オーバーレイ描画を含む完全なグレースケールパイプラインで DICOM 画像を生ピクセルデータにレンダリングします。任意の .NET 画像ライブラリを使用して JPG に保存します。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM 画像レンダリング パイプライン">}}

<p><strong>Aspose.Medical for .NET</strong> は、DICOM 仕様で定義された標準的な DICOM グレースケールパイプラインを介して DICOM 画像をレンダリングします。レンダリングプロセスは、ルックアップテーブル（LUT）のチェーンを適用して、保存された生のピクセル値を表示可能な画像に変換します：</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; 格納されたピクセル値を Rescale Slope/Intercept または LUT シーケンスを使用してメーカー非依存の値に変換します。</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; ウィンドウ/レベル（Window Center と Window Width）を適用して表示する値の範囲を選択し、Linear、Linear Exact、Sigmoid 変換をサポートします。</li>
<li><strong>Presentation LUT</strong> &mdash; 最終的な値を表示可能な P-Values にマッピングし、INVERSE（画像反転）をサポートします。</li>
<li><strong>Output LUT</strong> &mdash; グレースケール値を表示用に RGB に変換し、カスタムカラーマップまたは Palette Color テーブルを使用したオプションのカラー化を提供します。</li>
</ol>

<p>レンダリングされた出力は <code>IRawImage</code> です &mdash; 幅 <code>Width</code>、高さ <code>Height</code>、および <code>Pixels</code> 配列を持つ BGRA32 ピクセルバッファ（チャンネルあたり 8 ビット）です。これを <code>System.Drawing</code>、<code>SkiaSharp</code>、<code>ImageSharp</code> など任意の .NET 画像ライブラリを使用して JPG に保存できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# で DICOM をピクセルデータにレンダリング">}}

<p>DICOM ファイルを読み込み、フレームを BGRA32 ピクセルデータを含む <code>IRawImage</code> にレンダリングします：</p>

<div class="codeblock" id="code">
 <h3>DICOM 画像をレンダリング - C#</h3>
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

<p><code>RenderImage</code> メソッドは、DICOM ファイルに保存されたウィンドウ/レベル値と LUT パラメータを使用して、完全なグレースケールパイプラインを自動的に適用します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="レンダリング画像を JPG として保存">}}

<p><code>IRawImage</code> は、生の BGRA32 ピクセルデータを提供し、任意の .NET 画像ライブラリを使用して JPG に保存できます。以下は <code>System.Drawing</code> を使用した例です：</p>

<div class="codeblock" id="code">
 <h3>レンダリングした DICOM フレームを JPG として保存 - C#</h3>
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

// Save as JPG
bitmap.Save("ct_scan.jpg", ImageFormat.Jpeg);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="カスタム ウィンドウ/レベル（ウィンドウ処理）">}}

<p>Window/level（ウィンドウ処理または W/L とも呼ばれます）は、DICOM 画像レンダリングにおいて最も重要なパラメータです。ピクセル値のどの範囲を表示可能なグレースケール範囲にマップするかを制御します。<strong>Window Center</strong> は中点を定義し、<strong>Window Width</strong> は表示される値の範囲を定義します：</p>

<ul>
<li><strong>Narrow window</strong> &mdash; 高コントラスト、表示される灰度レベルが少ない（軟部組織に有用）。</li>
<li><strong>Wide window</strong> &mdash; 低コントラスト、表示される灰度レベルが多い（骨や肺に有用）。</li>
</ul>

<div class="codeblock" id="code">
 <h3>カスタムウィンドウ/レベルでレンダリング - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window: center=40, width=400
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window: center=400, width=1800
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Lung window: center=-600, width=1500
var lung = new GrayscaleRenderOptions
{
    WindowCenter = -600,
    WindowWidth = 1500
};
IRawImage lungImage = dicomFile.RenderImage(lung, 0);</code></pre>
</div>

<p>一般的な CT ウィンドウプリセット：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>組織タイプ</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>軟部組織</td><td>40</td><td>400</td></tr>
<tr><td>肺</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>骨</td><td>400</td><td>1800</td></tr>
<tr><td>脳</td><td>40</td><td>80</td></tr>
<tr><td>肝臓</td><td>60</td><td>150</td></tr>
<tr><td>縦隔</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="グレースケールレンダーオプション">}}

<p><code>GrayscaleRenderOptions</code> クラスは、グレースケールレンダリングパイプラインをフルコントロールできるようにします：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>プロパティ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>表示される値範囲の中心（VOI LUT の水平中点）</td></tr>
<tr><td><code>WindowWidth</code></td><td>表示される値範囲の幅（コントラストを制御）</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Modality LUT のリスケールスロープ &mdash; 保存されたピクセル値に乗算されます</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Modality LUT のリスケールインターセプト &mdash; スロープ乗算後に加算されます</td></tr>
<tr><td><code>Invert</code></td><td>グレースケール画像を反転させます（INVERSE Presentation LUT シェイプを適用）</td></tr>
<tr><td><code>BitDepth</code></td><td>ビット深度情報: BitsAllocated、BitsStored、HighBit、IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>反転グレースケールレンダリング - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray.dcm");

// Render with inverted grayscale (white-on-black becomes black-on-white)
var options = new GrayscaleRenderOptions
{
    WindowCenter = 2048,
    WindowWidth = 4096,
    Invert = true
};

IRawImage image = dicomFile.RenderImage(options, 0);</code></pre>
</div>

<p><code>RenderOptionsFactory.Create(pixelData)</code> を使用して、DICOM データセットのピクセルデータ属性（ビット深度、リスケールパラメータ、ウィンドウ/レベル、反転設定）からレンダーオプションを自動的に設定します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="オーバーレイ描画">}}

<p>DICOM 画像はオーバーレイプレーン（画像に焼き付けられたグラフィックやテキスト注釈）を含むことがあります。<code>RenderOptions</code> クラスは、オーバーレイを描画するかどうか、またその色を制御します：</p>

<div class="codeblock" id="code">
 <h3>オーバーレイ制御でレンダリング - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible in white (default behavior)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions
{
    ShowOverlays = false
};
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

<p>DICOM は 2 種類のオーバーレイをサポートします：<strong>Graphics</strong> オーバーレイ（注釈、測定）と <strong>Region of Interest</strong>（ROI）オーバーレイです。両方のタイプは <code>ShowOverlays</code> 設定で制御されます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="マルチフレーム DICOM レンダリング">}}

<p>CT、MRI、超音波の多くの DICOM ファイルは複数のフレーム（スライス）を含みます。フレーム数を確認し、各フレームを個別にレンダリングできます：</p>

<div class="codeblock" id="code">
 <h3>マルチフレーム DICOM のすべてのフレームをレンダリング - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;
Console.WriteLine($"Total frames: {totalFrames}");

// Render all frames to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as JPG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="VOI LUT 変換タイプ">}}

<p>DICOM 標準は、ウィンドウ/レベルマッピングの適用方法を制御する複数の VOI LUT 変換関数を定義しています。Aspose.Medical for .NET はすべての標準タイプをサポートします：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
<th>使用例</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>中心からのオフセットを伴う標準的な線形マッピング</td><td>最も一般的で、一般的な CT/MR 画像に使用</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>中心からのオフセットなしの線形マッピング</td><td>正確なピクセル値マッピングが必要な場合</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>滑らかなコントラスト遷移のための S カーブ変換</td><td>軟部組織の可視化、マンモグラフィ</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>DICOM データセット内のルックアップテーブルとして定義されたカスタム LUT</td><td>ベンダー固有またはモダリティ固有の表示曲線</td></tr>
</tbody>
</table>

<p>レンダリングエンジンは DICOM データセット属性に基づいて適切な VOI LUT 変換を自動的に選択します。カスタム <code>GrayscaleRenderOptions</code> を使用する場合、デフォルトで Linear 変換が適用されます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="サポートされているモダリティ">}}

<p>Aspose.Medical for .NET は、すべての一般的な医用画像モダリティからの DICOM 画像のレンダリングをサポートします：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>モダリティ</th>
<th>説明</th>
<th>典型的なフォーマット</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>コンピュータ断層撮影</td><td>グレースケール、マルチフレーム、12&ndash;16 ビット</td></tr>
<tr><td><strong>MR</strong></td><td>磁気共鳴画像</td><td>グレースケール、マルチフレーム、12&ndash;16 ビット</td></tr>
<tr><td><strong>CR / DX</strong></td><td>コンピュータ放射線撮影 / デジタル放射線撮影</td><td>グレースケール、シングルフレーム、10&ndash;16 ビット</td></tr>
<tr><td><strong>US</strong></td><td>超音波</td><td>グレースケールまたは RGB、マルチフレーム</td></tr>
<tr><td><strong>MG</strong></td><td>マンモグラフィ</td><td>グレースケール、高解像度、12&ndash;16 ビット</td></tr>
<tr><td><strong>XA</strong></td><td>X線血管造影</td><td>グレースケール、マルチフレーム</td></tr>
<tr><td><strong>NM</strong></td><td>核医学</td><td>グレースケール、マルチフレーム</td></tr>
<tr><td><strong>PT</strong></td><td>PET（陽電子放射断層撮影）</td><td>グレースケール、マルチフレーム</td></tr>
</tbody>
</table>

<p>グレースケール（MONOCHROME1/MONOCHROME2）とカラー（RGB、YBR_FULL、PALETTE COLOR）の両方の光度解釈をサポートします。レンダリングエンジンは適切な色空間変換を自動的に処理します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学習リソース" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="ドキュメンテーション" href="https://docs.aspose.com/medical/net/" >}}
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
{{< blocks/products/pf/slr-element name="導入事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
