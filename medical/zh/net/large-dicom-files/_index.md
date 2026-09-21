---
title: 在 C# .NET 中处理大型 DICOM 文件 | Aspose.Medical
weight: 11500

description: 在 C# 中打开多帧研究和全切片图像，而无需将其加载到内存。读取不含像素数据的元数据，延迟读取大型元素，并通过流和管道传输文件。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# 大型 DICOM 文件" h2="读取多帧研究的元数据而不加载像素，延迟大型元素的读取直至被请求，并通过流和管道传输完整文件。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="文件很大，但问题通常很小">}}

<p>全切片图像、长 CT 序列或 OCT 体数据通常有数百兆字节，其中大部分是像素数据。实际应用的工作往往要小得多：列出文件夹中的内容、检查患者标识、统计帧数、决定研究的去向。为了解答这些只加载每个字节就会把简单任务变成内存问题。</p>

<p><strong>Aspose.Medical for .NET</strong> 允许调用者决定读取文件的多少。此选项是 <code>DicomFile.Open</code> 的一个参数，适用于文件、流和管道。</p>

<p>在相同机器和相同文件上，对我们测试集中的 14 MB、128 帧的研究进行测量：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>读取策略</th>
<th>打开耗时</th>
<th>分配内存</th>
</tr>
</thead>
<tbody>
<tr><td>全部（默认）</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>跳过大型元素</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>延迟大型元素</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>随着文件增大，差距会扩大。包含 10,000 例研究的文件夹正是此时不再是微小优化的场景。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="读取元数据，像素保持不触碰">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> 会跳过所有超过大小阈值的元素。返回的数据集仅包含索引或路由器所需的标签。</p>

<div class="codeblock" id="code">
 <h3>读取不含像素数据的研究 - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>阈值默认为 64 kB，单位为千字节，因此工作流若将 8 kB 视为大型，可自行设置。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="延迟而非跳过">}}

<p>当像素可能稍后需要且不一定全部使用时，<code>ReadLargeOnDemand</code> 是另一种策略。打开文件的开销与跳过相同，只有在代码实际访问时才读取大型元素。</p>

<div class="codeblock" id="code">
 <h3>仅在使用时加载帧 - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>延迟读取为付费功能；其他策略在评估版中同样可用。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="在不触及像素的情况下索引文件夹">}}

<p>相同的策略同样适用于流，这正是代码中归档扫描或云对象存储的表现方式。</p>

<div class="codeblock" id="code">
 <h3>扫描归档 - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="流与管道，进出自如">}}

<p>读取和写入均支持流，异步入口同样接受 <code>System.IO.Pipelines</code> 类型。研究可以直接从网络响应传输到存储，无需将整个文件一次性加载为数组。</p>

<div class="codeblock" id="code">
 <h3>通过流进行读写 - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>相同的思路适用于文本表示：在 <a href="/medical/net/json-to-dicom/">JSON to DICOM</a> 与 <a href="/medical/net/xml-to-dicom/">XML to DICOM</a> 页面上，包含多个数据集的文档会一次读取一个数据集。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="逐帧处理">}}

<p>多帧数据按帧寻址，因此 500 帧的系列会一次读取一帧，而不是一次性加载完整的像素数据元素。</p>

<div class="codeblock" id="code">
 <h3>遍历帧 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="决定设计的场景">}}

<ul>
<li>归档索引与迁移：数百万文件，仅在移动时才关注头部信息。</li>
<li>路由器和存储节点：接收研究，读取路由所需的信息，再转发字节。</li>
<li>AI 流水线：基于元数据构建清单，然后仅提取实际用于训练的帧子集。</li>
<li>受内存限制的容器：工作集遵循读取策略，而非文件整体大小。</li>
<li>全切片及 OCT 数据：根本无法一次性读取全部内容的文件。</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">内存管理指南</a> 详细阐述了这些策略，<a href="/medical/net/dicom-networking/">DICOM 网络</a> 则展示了通过 DIMSE 接收相同数据的方式。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学习资源" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="文档" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="开发者指南" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
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
