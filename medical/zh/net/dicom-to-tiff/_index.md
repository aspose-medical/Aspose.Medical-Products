---
title: 在 C# .NET 中将 DICOM 转换为 TIFF | Aspose.Medical
weight: 12000
description: 在 C# .NET 中渲染 DICOM 图像为像素数据，并在支持多帧 DICOM 文件的多页 TIFF 中保存。使用 Aspose.Medical API 的完整 DICOM LUT 流程，并使用任意 .NET 成像库将其保存为 TIFF。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="在 .NET C# 中将 DICOM 转换为 TIFF" h2="将 DICOM 图像渲染为原始像素数据，完整支持灰度管线。通过窗口/层级控制、叠加渲染和多帧处理保持图像质量。使用任何 .NET 成像库将其保存为 TIFF —— 包括多页 TIFF —— 。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM 转 TIFF 转换">}}

<p><strong>Aspose.Medical for .NET</strong> 通过标准的 DICOM 灰度管线（Modality LUT、VOI LUT、Presentation LUT、Output LUT）渲染 DICOM 图像，生成正确窗口化的 BGRA32 像素数据。TIFF 是一种多功能格式，因支持无损压缩、高位深度以及 —— 最重要的是 —— <strong>多页文档</strong>，被广泛用于医学成像、科学研究和出版。这使得 TIFF 成为将多帧 DICOM 文件（CT、MRI、超声）转换为单个输出文件、保留所有切片的天然选择。</p>

<p>渲染输出是一个 <code>IRawImage</code> —— 一个具有 <code>Width</code>、<code>Height</code> 和 <code>Pixels</code> 数组的 BGRA32 像素缓冲区。您可以使用任意 .NET 成像库（如 <code>System.Drawing</code>、<code>SkiaSharp</code> 或 <code>ImageSharp</code>）将该像素数据保存为 TIFF。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="在 C# 中将 DICOM 渲染为像素数据">}}

<p>加载 DICOM 文件，渲染所需帧，并访问像素数据：</p>

<div class="codeblock" id="code">
 <h3>渲染 DICOM 图像 - C#</h3>
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

<p><code>RenderImage</code> 方法会自动应用存储在 DICOM 数据集中的窗口/层级值和 LUT 参数。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="将渲染图像保存为 TIFF">}}

<p><code>IRawImage</code> 提供可使用任意 .NET 成像库保存为 TIFF 的原始 BGRA32 像素数据。以下是使用 <code>System.Drawing</code> 的示例：</p>

<div class="codeblock" id="code">
 <h3>将渲染的 DICOM 帧保存为 TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="多帧 DICOM 的多页 TIFF">}}

<p>多帧 DICOM 文件（例如 CT 或 MRI 序列）在单个文件中包含多个图像切片。TIFF 的多页功能使您能够将所有帧存储在一个输出文件中，以通用可读的格式保留完整的检查。使用带有多帧参数的 <code>System.Drawing</code> TIFF 编码器：</p>

<div class="codeblock" id="code">
 <h3>将多帧 DICOM 转换为多页 TIFF - C#</h3>
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

<p>当需要单独的切片时，您也可以将每一帧保存为独立的 TIFF 文件：</p>

<div class="codeblock" id="code">
 <h3>将每帧转换为单独的 TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="自定义窗口/层级">}}

<p>通过设置窗口中心（Window Center）和窗口宽度（Window Width）来控制渲染的像素值范围。不同的窗口设置可从相同的 DICOM 数据中显示不同的解剖结构：</p>

<div class="codeblock" id="code">
 <h3>使用自定义窗口/层级渲染 - C#</h3>
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

<p><code>GrayscaleRenderOptions</code> 类还公开了 <code>RescaleSlope</code>、<code>RescaleIntercept</code>、<code>Invert</code> 和 <code>BitDepth</code> 属性，以便对渲染管线进行完整控制。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="多帧 DICOM 渲染">}}

<p>来自 CT、MRI 和超声的 DICOM 文件通常包含多个帧（切片）。将所有帧渲染为像素数据：</p>

<div class="codeblock" id="code">
 <h3>渲染多帧 DICOM 中的所有帧 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="叠加层控制">}}

<p>DICOM 图像可能包含带有注释、测量或感兴趣区的叠加平面。渲染时可控制叠加的可见性和颜色：</p>

<div class="codeblock" id="code">
 <h3>渲染时显示或隐藏叠加层 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="医学图像的 TIFF 与 PNG 与 JPG 对比">}}

<p>每种输出格式适用于不同的使用场景：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>特性</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>压缩</strong></td><td>无损（LZW、ZIP）或未压缩</td><td>无损</td><td>有损</td></tr>
<tr><td><strong>多页</strong></td><td>支持 —— 所有帧在一个文件中</td><td>不支持</td><td>不支持</td></tr>
<tr><td><strong>图像质量</strong></td><td>像素级完美</td><td>像素级完美</td><td>可能出现压缩伪影</td></tr>
<tr><td><strong>文件大小</strong></td><td>最大</td><td>中等</td><td>最小</td></tr>
<tr><td><strong>透明度</strong></td><td>支持</td><td>支持</td><td>不支持</td></tr>
<tr><td><strong>最佳用途</strong></td><td>归档、多帧序列、打印、科研</td><td>诊断审阅、质量要求的网页显示</td><td>网络共享、缩略图</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET 对所有输出目标使用相同的渲染管线。<code>IRawImage</code> 像素数据在不同目标格式下保持一致 —— 格式的选择仅影响由您的成像库执行的最终保存步骤。当需要在单个文件中保留完整的多帧 DICOM 检查，或输出用于归档、打印或进一步图像处理时，TIFF 是首选格式。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学习资源" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="文档" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="源代码" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API 参考" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="产品支持" tabId="support" >}}
{{< blocks/products/pf/slr-element name="免费支持" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="付费支持" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="博客" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="为什么选择 Aspose.Medical for .NET？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="客户名单" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功案例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
