---
title: 适用于 DICOM 的 JPEG XL（C# .NET）| Aspose.Medical
weight: 10500

description: 从 C# 将 DICOM 图像存储为 JPEG XL。无损 JPEG XL 能逐位返回像素，使用单个托管程序集，无需部署本机编解码器。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="适用于 DICOM 的 JPEG XL（.NET C#）" h2="DICOM 标准中最新的压缩方案，我们测得的无损文件体积最小，采用托管 C# 实现，并封装在单个程序集内。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="为何 JPEG XL 被纳入 DICOM">}}

<p>医学档案不断增长且不会缩减。JPEG XL 是影像界在二十年 JPEG 和 JPEG 2000 经验基础上设计的编解码器，DICOM 将其作为传输语法加入，原因在于存储团队关心：相同像素下文件更小。</p>

<p><strong>Aspose.Medical for .NET</strong> 通过库内部的 libjxl C# 移植版实现 JPEG XL 的写入与读取。该包仅包含一个程序集 <code>Aspose.Medical.dll</code>，且不附带本机二进制文件，因此这样全新的编解码器不会演变为部署工程：同一程序集可在 Windows、Linux、构建代理和容器中运行。</p>

<p>有两种传输语法携带像素：</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110)，用于必须保持原样的诊断数据。</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112)，用于对文件大小要求高于完全一致的场景。</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="压缩研究，保留每个像素">}}

<p>转码只需一次调用，且围绕像素的数据集随之一起传输。</p>

<div class="codeblock" id="code">
 <h3>将 DICOM 文件转码为 JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>我们在自有测试集中的一幅 1714×1933 像素、16 位的图像上进行测量：未压缩时 6.3 MB，使用 JPEG XL 无损后降至 2.7 MB，体积小于相同图像的 HTJ2K 无损。实际效果取决于模态，建议在一批文件夹中进行比较后再作决定。</p>

<p>这里的 “无损” 必须字面理解。转码为 JPEG XL 再转回，像素数据与原始字节完全相同，因而可以在不影响诊断质量的前提下重新压缩存档。</p>

<div class="codeblock" id="code">
 <h3>恢复为未压缩语法 - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="读取已存储为 JPEG XL 的内容">}}

<p>以 JPEG XL 形式到达的文件可像其他文件一样打开。传输语法指明其类型，帧解码后即可获取像素数据。</p>

<div class="codeblock" id="code">
 <h3>打开 JPEG XL 文件 - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL 或 HTJ2K">}}

<p>两者均为新近技术，均可在请求无损时提供无损压缩，库同时支持写入和读取。它们针对不同需求提供答案。</p>

<table class="table table-bordered">
<thead>
<tr>
<th>问题</th>
<th>回答</th>
</tr>
</thead>
<tbody>
<tr><td>在我们的测试中哪个产生的文件更小</td><td>JPEG XL 无损，略小几百分比</td></tr>
<tr><td>哪个针对网络的渐进式观看进行构建</td><td><a href="/medical/net/htj2k/">HTJ2K</a>，尤其是 RPCL 变体</td></tr>
<tr><td>哪个最早进入 DICOM 标准</td><td>HTJ2K，因此当前更多档案支持它</td></tr>
<tr><td>哪个需要本机依赖</td><td>都不需要，均为单一程序集的托管代码</td></tr>
</tbody>
</table>

<p>通常的选择取决于链的另一端：转码为档案接受的传输语法，其余流水线保持不变。</p>

<div class="codeblock" id="code">
 <h3>让目标档案自行决定 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="适用场景">}}

<ul>
<li>长期档案：相同研究占用更少 TB，且不存在需向放射科医师解释的质量损失。</li>
<li>云存储费用：节省可每月重复，而转码仅需执行一次。</li>
<li>研究与 AI 数据集：更小的副本在存储与训练之间传输更快。</li>
<li>部署：如此新的编解码器通常需要每个平台的本机构建；而这里它已随您引用的程序集一起提供。</li>
</ul>

<p>库同样支持写入现有档案常用的编解码器：JPEG、JPEG-LS、JPEG 2000、HTJ2K 与 RLE。<a href="/medical/net/dicom-transfer-syntax-conversion/">传输语法转换</a> 页面覆盖全部集合，<a href="/medical/net/htj2k/">HTJ2K</a> 有专属页面，<a href="/medical/net/jpeg2000/">JPEG 2000</a> 则是这两种新编解码器的来源。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学习资源" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="文档" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="开发者指南" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
