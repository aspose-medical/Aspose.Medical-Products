---
title: 在 C# .NET 中将 DICOM 转换为 JPG | Aspose.Medical
weight: 7000
description: 在 C# .NET 中将 DICOM 图像渲染为像素数据，支持窗口/层控制、叠加渲染和多帧。使用 Aspose.Medical API 的完整 DICOM LUT 流程并保存为 JPG。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# 中将 DICOM 转换为 JPG" h2="将 DICOM 图像渲染为原始像素数据，完整支持灰度管线，包括窗口/层、Modality LUT、VOI LUT 与叠加渲染。使用任何 .NET 图像库保存为 JPG。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM 图像渲染管线">}}

<p><strong>Aspose.Medical for .NET</strong> 渲染 DICOM 图像，使用 DICOM 规范中定义的标准灰度管线。渲染过程应用一系列查找表（LUT）将原始存储的像素值转换为可显示的图像：</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; 使用 Rescale Slope/Intercept 或 LUT 序列将存储的像素值转换为与厂商无关的值。</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; 应用窗口/层（Window Center 和 Window Width）选择显示的数值范围，支持 Linear、Linear Exact 与 Sigmoid 变换。</li>
<li><strong>Presentation LUT</strong> &mdash; 将最终数值映射为可显示的 P-Values，支持 INVERSE（图像反转）。</li>
<li><strong>Output LUT</strong> &mdash; 将灰度值转换为 RGB 以供显示，可使用自定义颜色映射或调色板表进行可选的着色。</li>
</ol>

<p>渲染输出为 <code>IRawImage</code> — 一个 BGRA32 像素缓冲区（每通道 8 位），包含 <code>Width</code>、<code>Height</code> 和 <code>Pixels</code> 数组。随后可使用任何 .NET 图像库（如 <code>System.Drawing</code>、<code>SkiaSharp</code> 或 <code>ImageSharp</code>）将该像素数据保存为 JPG。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="在 C# 中将 DICOM 渲染为像素数据">}}

<p>加载 DICOM 文件并将帧渲染为包含 BGRA32 像素数据的 <code>IRawImage</code>：</p>

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

<p><code>RenderImage</code> 方法会自动使用 DICOM 文件中存储的窗口/层数值和 LUT 参数，应用完整的灰度管线。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="保存渲染图像为 JPG">}}

<p><code>IRawImage</code> 提供原始 BGRA32 像素数据，可使用任何 .NET 图像库保存为 JPG。以下示例使用 <code>System.Drawing</code>：</p>

<div class="codeblock" id="code">
 <h3>将渲染的 DICOM 帧保存为 JPG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="自定义窗口/层（Windowing）">}}

<p>窗口/层（亦称 windowing 或 W/L）是 DICOM 图像渲染中最重要的参数。它控制将哪一范围的像素值映射到可见的灰度范围。<strong>Window Center</strong> 定义中点，<strong>Window Width</strong> 定义显示的数值范围：</p>

<ul>
<li><strong>Narrow window</strong> &mdash; 高对比度，显示的灰阶较少（适用于软组织）。</li>
<li><strong>Wide window</strong> &mdash; 低对比度，显示的灰阶更多（适用于骨骼或肺部）。</li>
</ul>

<div class="codeblock" id="code">
 <h3>使用自定义窗口/层渲染 - C#</h3>
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

<p>常用 CT 窗口预设：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>组织类型</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>软组织</td><td>40</td><td>400</td></tr>
<tr><td>肺</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>骨骼</td><td>400</td><td>1800</td></tr>
<tr><td>脑部</td><td>40</td><td>80</td></tr>
<tr><td>肝脏</td><td>60</td><td>150</td></tr>
<tr><td>纵隔</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="灰度渲染选项">}}

<p><code>GrayscaleRenderOptions</code> 类提供对灰度渲染管线的完整控制：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>属性</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>显示数值范围的中心（VOI LUT 的水平中点）</td></tr>
<tr><td><code>WindowWidth</code></td><td>显示数值范围的宽度（控制对比度）</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Modality LUT 重缩放斜率 &mdash; 与存储的像素值相乘</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Modality LUT 重缩放截距 &mdash; 在斜率相乘后加上</td></tr>
<tr><td><code>Invert</code></td><td>反转灰度图像（应用 INVERSE Presentation LUT 形状）</td></tr>
<tr><td><code>BitDepth</code></td><td>位深信息：BitsAllocated、BitsStored、HighBit、IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>反转灰度渲染 - C#</h3>
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

<p>使用 <code>RenderOptionsFactory.Create(pixelData)</code> 可自动从 DICOM 数据集的像素数据属性填充渲染选项，包括位深、重缩放参数、窗口/层以及反转设置。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="叠加渲染">}}

<p>DICOM 图像可以包含叠加层 — 烧录在图像上的图形或文字注释。<code>RenderOptions</code> 类控制是否渲染叠加层以及呈现的颜色：</p>

<div class="codeblock" id="code">
 <h3>使用叠加控制渲染 - C#</h3>
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

<p>DICOM 支持两种叠加类型：<strong>Graphics</strong> 叠加（注释、测量）和 <strong>Region of Interest</strong>（ROI）叠加。两者均由 <code>ShowOverlays</code> 设置控制。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="多帧 DICOM 渲染">}}

<p>许多来自 CT、MRI 和超声的 DICOM 文件包含多个帧（切片）。您可以检查帧数并逐帧渲染：</p>

<div class="codeblock" id="code">
 <h3>渲染多帧 DICOM 的所有帧 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="VOI LUT 变换类型">}}

<p>DICOM 标准定义了多种 VOI LUT 变换函数，用于控制窗口/层映射的应用方式。Aspose.Medical for .NET 支持所有标准类型：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>类型</th>
<th>描述</th>
<th>用例</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>带中心偏移的标准线性映射</td><td>最常用，用于通用 CT/MR 成像</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>无中心偏移的线性映射</td><td>需要精确像素值映射时使用</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>用于更平滑对比度过渡的 S 曲线变换</td><td>软组织可视化、乳腺摄影</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>在 DICOM 数据集中定义的自定义查找表</td><td>特定厂商或特定模态的显示曲线</td></tr>
</tbody>
</table>

<p>渲染引擎会根据 DICOM 数据集属性自动选择合适的 VOI LUT 变换。使用自定义 <code>GrayscaleRenderOptions</code> 时，默认应用 Linear 变换。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="支持的模态">}}

<p>Aspose.Medical for .NET 支持渲染所有常见医学成像模态的 DICOM 图像：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>模态</th>
<th>描述</th>
<th>典型格式</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>计算机断层扫描</td><td>灰度，多帧，12&ndash;16 位</td></tr>
<tr><td><strong>MR</strong></td><td>磁共振成像</td><td>灰度，多帧，12&ndash;16 位</td></tr>
<tr><td><strong>CR / DX</strong></td><td>计算机/数字射线摄影</td><td>灰度，单帧，10&ndash;16 位</td></tr>
<tr><td><strong>US</strong></td><td>超声</td><td>灰度或 RGB， 多帧</td></tr>
<tr><td><strong>MG</strong></td><td>乳腺摄影</td><td>灰度，高分辨率，12&ndash;16 位</td></tr>
<tr><td><strong>XA</strong></td><td>X 射线血管造影</td><td>灰度，多帧</td></tr>
<tr><td><strong>NM</strong></td><td>核医学</td><td>灰度，多帧</td></tr>
<tr><td><strong>PT</strong></td><td>PET（正电子发射断层扫描）</td><td>灰度，多帧</td></tr>
</tbody>
</table>

<p>同时支持灰度（MONOCHROME1/MONOCHROME2）和彩色（RGB、YBR_FULL、PALETTE COLOR）光度学解释。渲染引擎会自动处理相应的颜色空间转换。</p>

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
