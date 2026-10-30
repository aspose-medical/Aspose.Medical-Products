---
title: C# .NET 中的 DICOM 传输语法转换 | Aspose.Medical
weight: 16000
description: 在 C# .NET 中对 DICOM 文件进行传输语法的转码。支持 JPEG、JPEG 2000、HTJ2K、JPEG XL、JPEG-LS、RLE 以及未压缩格式，使用 Aspose.Medical API。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# 中的 DICOM 传输语法转换" h2="在未压缩、JPEG、JPEG 2000、HTJ2K、JPEG XL、JPEG-LS 和 RLE 传输语法之间转码 DICOM 文件。纯 .NET 库，无原生依赖。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="什么是传输语法？">}}

<p><strong>传输语法</strong>定义了 DICOM 数据在存储和传输时的编码方式。它指定了三个关键方面：字节序（大小端）、值表示（VR）是显式还是隐式，以及应用于像素数据的压缩算法。每个 DICOM 文件在文件元信息头中声明其传输语法。</p>

<p>不同的医学设备、PACS 服务器和查看应用程序支持不同的传输语法集合。<strong>Aspose.Medical for .NET</strong>提供了 <code>Transcode</code> 方法，用于在传输语法之间转换，实现互操作性、存储优化以及与处理工具的兼容性 &mdash; 完全基于纯 .NET 库，无原生依赖。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="在 C# 中转码 DICOM 文件">}}

<p><code>DicomFile.Transcode</code> 方法将 DICOM 文件从当前传输语法转换为任意受支持的目标语法。该方法返回一个新的 <code>DicomFile</code> 实例 &mdash; 原始文件保持不变：</p>

<div class="codeblock" id="code">
 <h3>基本 DICOM 转码 - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>您也可以直接在 <code>Dataset</code> 级别进行转码：</p>

<div class="codeblock" id="code">
 <h3>转码 Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="支持的传输语法">}}

<p>下表列出了所有标准 DICOM 图像数据传输语法以及它们在 Aspose.Medical for .NET 中的当前支持状态。所有受支持的编解码器均以纯 C# 实现，且完全跨平台。</p>

<table class="table table-bordered">
<thead>
<tr>
<th>传输语法</th>
<th>UID</th>
<th>类型</th>
<th>状态</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>未压缩</strong></td></tr>
<tr><td>隐式 VR 小端序</td><td><code>1.2.840.10008.1.2</code></td><td>未压缩</td><td>支持</td></tr>
<tr><td>显式 VR 小端序</td><td><code>1.2.840.10008.1.2.1</code></td><td>未压缩</td><td>支持</td></tr>
<tr><td>显式 VR 大端序</td><td><code>1.2.840.10008.1.2.2</code></td><td>未压缩（已废弃）</td><td>支持</td></tr>
<tr><td>封装未压缩显式 VR 小端序</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>未压缩</td><td>不支持</td></tr>
<tr><td>Deflated 显式 VR 小端序</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflate</td><td>支持</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG 基线（Process 1）</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>有损，8 位</td><td>支持</td></tr>
<tr><td>JPEG 扩展（Process 2 &amp; 4）</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>有损，12 位</td><td>支持</td></tr>
<tr><td>JPEG 无损（Process 14）</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>无损</td><td>支持</td></tr>
<tr><td>JPEG 无损，First-Order Prediction（Process 14, SV1）</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>无损</td><td>支持</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS 无损</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>无损</td><td>支持</td></tr>
<tr><td>JPEG-LS 近无损</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>近无损</td><td>支持</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 仅无损</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>无损</td><td>支持</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>有损或无损</td><td>支持</td></tr>
<tr><td>JPEG 2000 Part 2 多组件仅无损</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>无损</td><td>不支持</td></tr>
<tr><td>JPEG 2000 Part 2 多组件</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>有损或无损</td><td>不支持</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE 无损</td><td><code>1.2.840.10008.1.2.5</code></td><td>无损</td><td>支持</td></tr>
<tr><td colspan="4"><strong>高通量 JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K 仅无损</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>无损</td><td>支持</td></tr>
<tr><td>HTJ2K 带 RPCL 选项仅无损</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>无损</td><td>支持</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>有损或无损</td><td>支持</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL 无损</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>无损</td><td>支持</td></tr>
<tr><td>JPEG XL JPEG 重新压缩</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>无损</td><td>仅解码（编码需要 JPEG 源流，而非像素数据）</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>有损或无损</td><td>支持（有损模式）</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="常见转码场景">}}

<p>不同的工作流需要不同的转码策略。以下是最常见的场景：</p>

<div class="codeblock" id="code">
 <h3>解压以进行处理 - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>压缩以进行归档存储 - C#</h3>
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
 <h3>压缩以进行网络传输 - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>使用最新编解码器：HTJ2K 和 JPEG XL - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="检查传输语法属性">}}

<p><code>TransferSyntax</code> 类公开了描述编码特性的属性。可使用这些属性检查文件当前的传输语法或选择合适的目标语法：</p>

<div class="codeblock" id="code">
 <h3>读取传输语法属性 - C#</h3>
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
<th>属性</th>
<th>类型</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>传输语法的唯一标识符</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>值表示是否显式编码</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>字节序是否为小端序</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>像素数据是否被封装（压缩）</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>压缩方式是否为有损</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>语法是否使用 deflate 压缩</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>传输语法是否已被 DICOM 标准废弃</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>有损压缩方法的 ISO 标准标识符</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="有损 vs 无损压缩">}}

<p>在转码 DICOM 文件时，了解有损和无损压缩之间的差异至关重要：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>方面</th>
<th>无损</th>
<th>有损</th>
</tr>
</thead>
<tbody>
<tr><td>图像质量</td><td>像素级完美 &mdash; 原始数据完整保留</td><td>为获得更小尺寸而永久丢失部分数据</td></tr>
<tr><td>压缩比率</td><td>通常 2:1 至 3:1</td><td>通常 10:1 至 30:1 或更高</td></tr>
<tr><td>往返安全</td><td>是 &mdash; 解压后得到相同像素</td><td>否 &mdash; 每次有损重新编码都会进一步降低质量</td></tr>
<tr><td>使用场景</td><td>归档、诊断、法律记录</td><td>初步审查、远程医疗、网络传输</td></tr>
<tr><td>支持的编解码器</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG Extended, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>重要提示：</strong> 将有损压缩的文件转码为无损语法并不能恢复丢失的数据。原始有损压缩导致的质量下降是永久性的。</p>

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
{{< blocks/products/pf/slr-element name="客户列表" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功案例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
