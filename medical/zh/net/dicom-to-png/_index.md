---
title: 在 C# .NET 中将 DICOM 转换为 PNG | Aspose.Medical
weight: 11000
description: 在 C# .NET 中将 DICOM 图像渲染为像素数据，保持无损质量，支持窗宽/窗位控制和多帧。使用 Aspose.Medical API 的完整 DICOM LUT 流程并保存为 PNG。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# 中将 DICOM 转换为 PNG" h2="将 DICOM 图像渲染为原始像素数据，支持完整的灰度流水线。通过窗宽/窗位控制、叠加渲染和多帧处理来保持图像质量。使用任意 .NET 成像库保存为无损 PNG。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM 转 PNG 转换">}}

<p><strong>Aspose.Medical for .NET</strong> 通过标准 DICOM 灰度流水线（Modality LUT、VOI LUT、Presentation LUT、Output LUT）渲染 DICOM 图像，以生成正确窗位的 BGRA32 像素数据。PNG 是无损格式，在必须保留图像质量且不产生压缩伪影的情况下是首选——这对于诊断审查、归档和研究使用场景至关重要。</p>

<p>渲染输出为 <code>IRawImage</code> &mdash; 一个包含 <code>Width</code>、<code>Height</code> 和 <code>Pixels</code> 数组的 BGRA32 像素缓冲区。您可以使用任意 .NET 成像库（如 <code>System.Drawing</code>、<code>SkiaSharp</code> 或 <code>ImageSharp</code>）将此像素数据保存为 PNG。</p>

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

<p><code>RenderImage</code> 方法会自动应用存储在 DICOM 数据集中的窗宽/窗位值和 LUT 参数。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="将渲染图像保存为 PNG">}}

<p><code>IRawImage</code> 提供原始 BGRA32 像素数据，可使用任意 .NET 成像库保存为无损 PNG。以下是使用 <code>System.Drawing</code> 的示例：</p>

<div class="codeblock" id="code">
 <h3>将渲染的 DICOM 帧保存为 PNG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="自定义窗宽/窗位">}}

<p>通过设置 Window Center 和 Window Width 来控制渲染的像素值范围。不同的窗设置可以从同一 DICOM 数据中显示不同的解剖结构：</p>

<div class="codeblock" id="code">
 <h3>使用自定义窗宽/窗位渲染 - C#</h3>
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

<p><code>GrayscaleRenderOptions</code> 类还公开了 <code>RescaleSlope</code>、<code>RescaleIntercept</code>、<code>Invert</code> 和 <code>BitDepth</code> 属性，以便对渲染流水线进行完整控制。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="多帧 DICOM 渲染">}}

<p>CT、MRI 和超声等 DICOM 文件通常包含多个帧（切片）。将所有帧渲染为像素数据：</p>

<div class="codeblock" id="code">
 <h3>渲染多帧 DICOM 的所有帧 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="叠加层控制">}}

<p>DICOM 图像可能包含带有注释、测量或感兴趣区域的叠加平面。渲染时可控制叠加层的可见性和颜色：</p>

<div class="codeblock" id="code">
 <h3>渲染时带或不带叠加层 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="医疗图像的 PNG 与 JPG 对比">}}

<p>在 PNG 与 JPG 之间的选择取决于使用场景：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>特性</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>压缩方式</strong></td><td>无损</td><td>有损</td></tr>
<tr><td><strong>图像质量</strong></td><td>像素级完美 &mdash; 无伪影</td><td>可能出现轻微压缩伪影</td></tr>
<tr><td><strong>文件大小</strong></td><td>更大（通常是 JPG 的 2–10 倍）</td><td>更小</td></tr>
<tr><td><strong>透明度</strong></td><td>支持（alpha 通道）</td><td>不支持</td></tr>
<tr><td><strong>最佳用途</strong></td><td>诊断审查、归档、研究、叠加层</td><td>网络共享、缩略图、预览</td></tr>
<tr><td><strong>监管要求</strong></td><td>质量关键工作流的首选</td><td>非诊断用途可接受</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET 对两种输出目标使用相同的渲染流水线。<code>IRawImage</code> 像素数据在不同目标格式下保持一致——格式的选择仅影响由您的成像库执行的最终保存步骤。</p>

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
{{< blocks/products/pf/slr-element name="客户列表" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功案例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
