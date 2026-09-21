---
title: HTJ2K 在 C# .NET - 面向 DICOM 的高吞吐量 JPEG 2000 | Aspose.Medical
weight: 10000

description: 在 C# 中压缩并读取使用高吞吐量 JPEG 2000 的 DICOM 图像。支持无损 HTJ2K、RPCL 变体以及有损 HTJ2K，全部以托管 .NET 实现，无需部署本地 codec。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K 在 .NET C#" h2="面向 DICOM 的高吞吐量 JPEG 2000：标准为快速归档和云端查看新增的压缩技术，以托管 C# 实现，无需安装任何本地组件。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="HTJ2K 的改变">}}

<p>高吞吐量 JPEG 2000 保持了 JPEG 2000 的小波和图像质量，但替换了导致其慢速的部分。块编码器是全新设计，解码速度提升一个数量级，这也是 DICOM 标准在三种传输语法中采用它以及云影像平台转向它的原因。</p>

<p>对于 .NET 团队来说，实际问题不同：谁能够真正生成这些文件。大多数库通过本地 OpenJPH 构建来实现 HTJ2K，这意味着每个平台都有二进制文件，需要在容器中增加构建步骤，并且会在安全审查时提出依赖问题。<strong>Aspose.Medical for .NET</strong> 在同一套读取写入文件的包内以托管代码实现 codec，因此 HTJ2K 在 Windows、Linux 以及容器中表现一致，无需额外安装。</p>

<p>支持三种传输语法，且这三种均支持读取和写入：</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201)，高吞吐量 JPEG 2000 无损。</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202)，使用 RPCL 进度顺序的无损变体。</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203)，高吞吐量 JPEG 2000。</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="将研究压缩为 HTJ2K">}}

<p>一次调用即可将文件转换为新语法。数据集、私有标签和文件元信息都会随之迁移。</p>

<div class="codeblock" id="code">
 <h3>将 DICOM 文件转码为 HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>在我们自己的测试集中的一幅 1714×1933、16 位的图像上，文件大小从 6.3 MB 降至 2.9 MB，像素位对位恢复。不同模态和图像的数值会有差异，请在自己的数据上进行测量，即对已有文件逐个循环处理。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="无损即无损">}}

<p>诊断数据不能容忍近似的 codec。将数据转码为 HTJ2K 无损再转回，像素数据与原始字节完全一致，这一属性可在自己的测试套件中进行断言，确保在重新压缩归档前满足无损要求。</p>

<div class="codeblock" id="code">
 <h3>恢复为未压缩语法 - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL，面向网络查看的变体">}}

<p>1.2.840.10008.1.2.4.202 语法以 RPCL 进度顺序存储相同的无损码流：先分辨率、后位置、再组件、最后层。仅读取流的开头的阅读器即可获得完整的低分辨率图像，这正是查看器在通过非受控链接打开大型研究时所需的。</p>

<div class="codeblock" id="code">
 <h3>使用 RPCL 进度顺序压缩 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="读取归档发送的内容">}}

<p>另一半工作是接收已经生成 HTJ2K 的系统提供的文件。打开文件，检查其存储的语法，然后处理像素数据。</p>

<div class="codeblock" id="code">
 <h3>读取 HTJ2K 文件 - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>多帧图像逐帧处理，因此长序列的内存消耗按帧而非按研究计。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTJ2K 的价值所在">}}

<ul>
<li>归档迁移：将已存储的研究重新压缩为 HTJ2K 无损，降低存储占用，同时保持诊断数据完整。</li>
<li>云端与 DICOMweb：解码速度决定了浏览器端或服务器端查看器在大图像上的即时感受。</li>
<li>AI 流水线：训练集的读取远多于写入，解码时间是反复出现的成本。</li>
<li>容器与无服务器：codec 随程序集一起打包，镜像无需本地库或编译器即可构建。</li>
</ul>

<p>该库还提供 JPEG XL——标准的另一项最新补充，以及归档中常见的旧 codec：JPEG、JPEG-LS、JPEG 2000 和 RLE。<a href="/medical/net/dicom-transfer-syntax-conversion/">传输语法转换</a>页面涵盖全部集合，<a href="/medical/net/jpeg2000/">JPEG 2000</a>页面则介绍 HTJ2K 的来源 codec。</p>

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

{{< blocks/products/pf/slr-tab tabTitle="为什么选择 Aspose.Medical for .NET？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="客户名单" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功案例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
