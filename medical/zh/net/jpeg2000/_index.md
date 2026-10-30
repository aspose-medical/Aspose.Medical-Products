---
title: C# .NET 中的 DICOM JPEG 2000 压缩 | Aspose.Medical
weight: 2000
description: 在 C# .NET 中读取、写入和转码使用 JPEG 2000 压缩的 DICOM 文件。支持 1 到 16 位的单色和彩色图像，无损和有损模式，并提供 Aspose.Medical API 的 HTJ2K 支持。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# 中的 DICOM JPEG 2000 支持" h2="读取、写入和转码使用 JPEG 2000 压缩的 DICOM 文件。支持无损和有损模式，1 到 16 位的单色和彩色像素数据，包含 HTJ2K——全部基于纯 .NET 实现。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="医学影像中的 JPEG 2000">}}

<p><strong>JPEG 2000</strong>（ISO/IEC 15444）是医学影像中最广泛使用的基于小波的压缩标准。不同于传统 JPEG，它在单一编解码器中同时提供无损和有损压缩，支持针对感兴趣区域的渐进式解码，并拥有更优的压缩比 — 使其非常适合对大型研究进行归档及在受限网络中传输图像。</p>

<p><strong>Aspose.Medical for .NET</strong> 提供了纯 C# 实现的 JPEG 2000 编解码器，无需本地依赖。该库可以读取、渲染以及转码使用 JPEG 2000 Part 1 传输语法（无损和有损）压缩的 DICOM 文件。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="支持的 JPEG 2000 传输语法">}}

<table class="table table-bordered">
<thead>
<tr>
<th>传输语法</th>
<th>UID</th>
<th>模式</th>
<th>读取</th>
<th>写入</th>
</tr>
</thead>
<tbody>
<tr><td>仅 JPEG 2000 无损</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>无损</td><td>单色和彩色，1 到 16 位</td><td>单色、调色板彩色和 RGB，1 到 16 位</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>有损或无损</td><td>单色和彩色，1 到 16 位</td><td>单色和 RGB，1 到 16 位</td></tr>
<tr><td>JPEG 2000 Part 2 多组件仅无损</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>无损</td><td>不支持</td><td>不支持</td></tr>
<tr><td>JPEG 2000 Part 2 多组件</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>有损或无损</td><td>不支持</td><td>不支持</td></tr>
<tr><td>仅 HTJ2K 无损</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>无损</td><td>单色和彩色</td><td>单色和彩色</td></tr>
<tr><td>仅 HTJ2K（带 RPCL 选项）无损</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>无损</td><td>单色和彩色</td><td>单色和彩色</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>有损或无损</td><td>单色和彩色</td><td>单色和彩色</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="像素数据 1 到 16 位">}}

<p>医学图像常使用每样本 16 位，以捕获 CT（通常在 16 位中存储 12 位）和 MRI 等模态的完整动态范围。Aspose.Medical 能够处理 JPEG 2000 从 1 到 16 位的所有深度，可使用 8 位或 16 位样本：</p>

<ul>
<li><strong>读取（解压缩）</strong>：单色文件（CT、MRI、X 射线）、调色板彩色文件以及三分量彩色文件（RGB、YBR_RCT、YBR_ICT 以及其他颜色标签如 YBR_FULL，解码为编码）均支持 1 到 16 位每样本，可为有符号或无符号。CMYK、ICC 配置文件、JP2 调色板以及子采样彩色码流将被拒绝并抛出明确异常，而不是静默产生错误图像。</li>
<li><strong>写入（压缩）</strong>：单色和 RGB 图像支持 1 到 16 位每样本，可为有符号或无符号；调色板彩色图像支持 JPEG 2000 无损。YBR 图像仅在解码为 RGB 时才会被编码，类似有损 JPEG YBR 文件的处理方式。</li>
</ul>

<div class="codeblock" id="code">
 <h3>读取并检查 JPEG 2000 压缩的 DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="转码为 JPEG 2000">}}

<p>使用 <code>Transcode</code> 方法将任意 DICOM 文件压缩为 JPEG 2000，或在 JPEG 2000 模式之间进行转换：</p>

<div class="codeblock" id="code">
 <h3>压缩 DICOM 为 JPEG 2000 无损 - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>压缩 DICOM 为 JPEG 2000 有损 - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="解压缩 JPEG 2000 DICOM 文件">}}

<p>将 JPEG 2000 文件解压缩为未压缩的传输语法，以便进行处理、分析，或兼容不支持 JPEG 2000 的系统：</p>

<div class="codeblock" id="code">
 <h3>将 JPEG 2000 解压缩为未压缩 - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>您还可以在一步操作中完成解压缩并转码为其他压缩格式：</p>

<div class="codeblock" id="code">
 <h3>在压缩格式之间转码 - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="渲染 JPEG 2000 DICOM 图像">}}

<p>JPEG 2000 压缩的 DICOM 文件可以渲染为像素数据，以供显示或导出，效果与任何其他传输语法相同：</p>

<div class="codeblock" id="code">
 <h3>渲染 JPEG 2000 压缩帧 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="无损 vs 有损 JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>方面</th>
<th>JPEG 2000 无损</th>
<th>JPEG 2000 有损</th>
</tr>
</thead>
<tbody>
<tr><td>传输语法</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>图像质量</td><td>像素完美 &mdash; 与原始完全相同</td><td>视觉相似，部分数据永久丢失</td></tr>
<tr><td>压缩比</td><td>通常为 2:1 至 3:1</td><td>通常为 10:1 至 30:1 或更高</td></tr>
<tr><td>适用场景</td><td>诊断归档、法律记录、首次阅读</td><td>初步审查、远程医疗、网络传输</td></tr>
<tr><td>往返安全</td><td>是</td><td>否 &mdash; 再次编码会进一步降低质量</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="高吞吐量 JPEG 2000 (HTJ2K)">}}

<p>HTJ2K（ISO/IEC 15444-15）用更快的块编码器取代了 JPEG 2000 中慢速的算术编码器。它保持相同的小波变换、进阶顺序和质量，且解码与编码速度提升数倍。Aspose.Medical 在纯 .NET 中实现了所有三种 DICOM HTJ2K 传输语法，支持单色和彩色图像，并能够在 HTJ2K 与所有其他支持的语法之间进行转码：</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; 仅无损</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; 带 RPCL 进阶顺序的无损</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; 有损或无损</li>
</ul>

<div class="codeblock" id="code">
 <h3>在 JPEG 2000 与 HTJ2K 之间转码（往返） - C#</h3>
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

{{< blocks/products/pf/slr-tab tabTitle="为何选择 Aspose.Medical for .NET？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="客户名单" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功案例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
