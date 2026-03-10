---
title: C# .NET で DICOM を TIFF に変換 | Aspose.Medical
weight: 12000
description: C# .NET で DICOM 画像をピクセル データへレンダリングし、マルチフレーム DICOM ファイル向けのマルチページ対応 TIFF として保存します。Aspose.Medical API を使用して完全な DICOM LUT パイプラインを利用し、任意の .NET 画像ライブラリで TIFF に保存します。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# で DICOM を TIFF に変換" h2="完全なグレースケール パイプラインを用いて DICOM 画像を生ピクセル データにレンダリングします。ウィンドウ/レベル制御、オーバーレイ描画、マルチフレーム処理で画像品質を保持します。任意の .NET 画像ライブラリを使用して TIFF（マルチページ TIFF を含む）として保存します。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM から TIFF への変換">}}

<p><strong>Aspose.Medical for .NET</strong> は標準の DICOM グレースケール パイプライン（Modality LUT、VOI LUT、Presentation LUT、Output LUT）を通じて DICOM 画像をレンダリングし、正しくウィンドウ処理された BGRA32 ピクセル データを生成します。TIFF は可逆圧縮や高ビット深度をサポートし、&mdash; 特に重要なのは &mdash; <strong>マルチページ文書</strong>であるため、医療画像、科学研究、出版で広く使用される多用途フォーマットです。このため、TIFF はマルチフレーム DICOM ファイル（CT、MRI、超音波）をすべてのスライスを保持した単一の出力ファイルに変換する自然な選択肢となります。</p>

<p>レンダリングされた出力は <code>IRawImage</code> — <code>Width</code>、<code>Height</code>、および <code>Pixels</code> 配列を持つ BGRA32 ピクセル バッファです。このピクセル データは、<code>System.Drawing</code>、<code>SkiaSharp</code>、<code>ImageSharp</code> などの任意の .NET 画像ライブラリを使用して TIFF に保存できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# で DICOM をピクセル データにレンダリング">}}

<p>DICOM ファイルを読み込み、目的のフレームをレンダリングし、ピクセル データにアクセスします：</p>

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

<p><code>RenderImage</code> メソッドは、DICOM データセットに保存されたウィンドウ/レベル値と LUT パラメータを自動的に適用します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="レンダリングされた画像を TIFF として保存">}}

<p><code>IRawImage</code> は、生の BGRA32 ピクセル データを提供し、任意の .NET 画像ライブラリを使用して TIFF に保存できます。以下は <code>System.Drawing</code> を使用した例です：</p>

<div class="codeblock" id="code">
 <h3>レンダリングされた DICOM フレームを TIFF として保存 - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("xray.dcm");
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

// Save as TIFF
bitmap.Save("xray.tiff", ImageFormat.Tiff);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="マルチフレーム DICOM からのマルチページ TIFF">}}

<p>マルチフレーム DICOM ファイル（例：CT や MRI シリーズ）には単一ファイル内に複数の画像スライスが含まれます。TIFF のマルチページ機能により、すべてのフレームを 1 つの出力ファイルに保存でき、汎用的に読み取れる形式で研究全体を保持できます。<code>System.Drawing</code> の TIFF エンコーダをマルチフレーム パラメータと共に使用します：</p>

<div class="codeblock" id="code">
 <h3>マルチフレーム DICOM をマルチページ TIFF に変換 - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
int totalFrames = dicomFile.NumberOfFrames;

// Get the TIFF codec
var tiffCodec = ImageCodecInfo.GetImageEncoders()
    .First(c =&gt; c.FormatID == ImageFormat.Tiff.Guid);

// Multi-frame parameters
var multiFrameParams = new EncoderParameters(2);
multiFrameParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.MultiFrame);
multiFrameParams.Param[1] = new EncoderParameter(
    Encoder.Compression, (long)EncoderValue.CompressionLZW);

// Parameter for adding subsequent frames
var frameAddParams = new EncoderParameters(1);
frameAddParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.FrameDimensionPage);

// Parameter for flushing the final frame
var flushParams = new EncoderParameters(1);
flushParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.Flush);

Bitmap? tiffBitmap = null;

for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var frameBitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = frameBitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    frameBitmap.UnlockBits(bitmapData);

    if (i == 0)
    {
        // Save the first frame and start multi-page TIFF
        tiffBitmap = (Bitmap)frameBitmap.Clone();
        tiffBitmap.Save("ct_series.tiff", tiffCodec, multiFrameParams);
    }
    else
    {
        // Add subsequent frames
        tiffBitmap!.SaveAdd(frameBitmap, frameAddParams);
    }
}

// Flush and close the multi-page TIFF
tiffBitmap?.SaveAdd(flushParams);
tiffBitmap?.Dispose();</code></pre>
</div>

<p>個々のスライスが必要な場合は、各フレームを別々の TIFF ファイルとして保存することもできます。</p>

<div class="codeblock" id="code">
 <h3>各フレームを別々の TIFF に変換 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

for (int i = 0; i &lt; dicomFile.NumberOfFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var bitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = bitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    bitmap.UnlockBits(bitmapData);
    bitmap.Save($"frame_{i:D4}.tiff", ImageFormat.Tiff);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="カスタム ウィンドウ/レベル">}}

<p>Window Center と Window Width を設定することで、レンダリングするピクセル値の範囲を制御できます。異なるウィンドウ設定により、同じ DICOM データから異なる解剖学的構造が明らかになります。</p>

<div class="codeblock" id="code">
 <h3>カスタム ウィンドウ/レベルでレンダリング - C#</h3>
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
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

<p><code>GrayscaleRenderOptions</code> クラスは、<code>RescaleSlope</code>、<code>RescaleIntercept</code>、<code>Invert</code>、<code>BitDepth</code> プロパティも公開しており、レンダリング パイプラインを完全に制御できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="マルチフレーム DICOM のレンダリング">}}

<p>CT、MRI、超音波の DICOM ファイルはしばしば複数のフレーム（スライス）を含みます。すべてのフレームをピクセル データにレンダリングします：</p>

<div class="codeblock" id="code">
 <h3>マルチフレーム DICOM からすべてのフレームをレンダリング - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as TIFF using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="オーバーレイ制御">}}

<p>DICOM 画像には注釈、測定、または関心領域を含むオーバーレイ平面が含まれることがあります。レンダリング時にオーバーレイの表示/非表示と色を制御します。</p>

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
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="医療画像における TIFF と PNG と JPG の比較">}}

<th>特性</th>

<table class="table table-bordered">
<thead>
<tr>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
<tr><td><strong>圧縮</strong></td><td>可逆 (LZW、ZIP) または非圧縮</td><td>可逆</td><td>非可逆</td></tr>
</tr>
</thead>
<tbody>
<tr><td><strong>マルチページ</strong></td><td>サポート — すべてのフレームが 1 ファイルに</td><td>未サポート</td><td>未サポート</td></tr>
<tr><td><strong>画像品質</strong></td><td>ピクセル単位で完全</td><td>ピクセル単位で完全</td><td>圧縮アーティファクトが発生する可能性あり</td></tr>
<tr><td><strong>ファイルサイズ</strong></td><td>最大</td><td>中</td><td>最小</td></tr>
<tr><td><strong>透過性</strong></td><td>サポート</td><td>サポート</td><td>未サポート</td></tr>
<tr><td><strong>推奨用途</strong></td><td>保存、マルチフレームシリーズ、印刷、研究</td><td>診断レビュー、品質を保ったウェブ表示</td><td>ウェブ共有、サムネイル</td></tr>
<p>Aspose.Medical for .NET はすべての出力対象に対して同一のレンダリング パイプラインを使用します。<code>IRawImage</code> のピクセル データはターゲット形式に関わらず同一であり、形式の選択は最終的に画像ライブラリが実行する保存処理にのみ影響します。単一ファイルで完全なマルチフレーム DICOM 研究を保持する必要がある場合や、保存・印刷・さらなる画像処理を目的とする場合、TIFF が推奨フォーマットです。</p>
</tbody>
</table>

学習リソース

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="ドキュメント" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="ソースコード" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="API リファレンス" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="製品サポート" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="無料サポート" tabId="support" >}}
{{< blocks/products/pf/slr-element name="有償サポート" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="ブログ" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="なぜ Aspose.Medical for .NET なのか？" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="顧客一覧" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="導入事例" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Success Stories" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
