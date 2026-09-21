---
title: 在 C# .NET 中将 JSON 转换为 DICOM | Aspose.Medical
weight: 6000

description: 在 C# .NET 中根据标准 DICOM JSON Model（PS3.18）构建 DICOM 文件。可以从字符串、流或管道读取 JSON，流式传输数据集序列，并使用 Aspose.Medical API 解析 BulkData 引用。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="在 .NET C# 中将 JSON 转换为 DICOM" h2="将标准 DICOM JSON Model（PS3.18）读取回数据集和 DICOM 文件。支持从字符串、流或管道读取，从而流式传输研究序列，并解析 BulkData 引用。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="从 DICOM JSON 到 DICOM 文件">}}

<p><strong>Aspose.Medical for .NET</strong> 读取 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>，该表示方式用于 DICOMweb 服务以及通过 HTTP 交换研究的系统。以 JSON 形式到达的数据会转换为 <code>Dataset</code>，随后 <code>Dataset</code> 被写入磁盘生成 DICOM 文件。</p>

<p>这是 <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> 页面所示的逆向操作，两者均使用同一类 <code>DicomJsonSerializer</code>。</p>

<div class="codeblock" id="code">
 <h3>从 JSON 创建 DICOM 文件 - C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>当数据集未携带 File Meta Information 并被包装为 <code>DicomFile</code> 时，将使用默认传输语法 Implicit VR Little Endian 进行写入。</p>

<p>读取 DICOM JSON 是付费功能。若未应用本地许可证，读取器会抛出 <code>MedicalApiException</code>，因此请先按照 <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">许可证指南</a> 进行授权。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="保留 File Meta Information">}}

<p><code>Deserialize</code> 仅返回数据集本身。当 JSON 文档同时包含 File Meta Information 组（例如来源于完整的 DICOM 文件）时，<code>DeserializeFile</code> 会返回包含该组完整信息的 <code>DicomFile</code>，包括文件声明的传输语法。</p>

<div class="codeblock" id="code">
 <h3>从 JSON 读取完整 DICOM 文件 - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="流、管道和异步">}}

<p>每个入口点都提供流重载和异步重载，异步版本还接受 <code>PipeReader</code>。来自 Web 响应或磁盘的文档会直接读取，而无需先转换为字符串，这在 JSON 包含像素数据时尤为重要。</p>

<div class="codeblock" id="code">
 <h3>从流读取 JSON - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="一次一个数据集的序列">}}

<p>DICOMweb 查询返回一个数据集数组，该文档可能很大。<code>DeserializeList</code> 会将整个数组读取到内存中；<code>DeserializeAsyncEnumerable</code> 则一次产生一个数据集，从而不会一次性加载完整文档。</p>

<div class="codeblock" id="code">
 <h3>流式传输数据集数组 - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="BulkData 引用">}}

<p>DICOM JSON Model 不内嵌像素数据。大的数值会被替换为指向字节的 <code>BulkDataURI</code>，从而保持 JSON 文档体积小。读取时若需解析这些引用，需要为序列化器提供 BulkData 加载器。<code>DefaultBulkDataLoader</code> 能在无认证的情况下获取 <code>file</code>、<code>http</code> 与 <code>https</code> URI；若存档需要凭证，请自行实现 <code>IBulkDataLoader</code> 或 <code>IAsyncBulkDataLoader</code>。</p>

<div class="codeblock" id="code">
 <h3>读取时解析 BulkDataURI - C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM 与 JSON 的往返转换">}}

<p>这两个方向设计为配合使用：研究先以 JSON 形式输出，经过 Web 服务传输后再转换回 DICOM 文件。整个过程不依赖本机代码，可在 Windows、Linux 与 macOS 上实现相同的往返转换。</p>

<div class="codeblock" id="code">
 <h3>DICOM 转 JSON 再返回 - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>有关控制 JSON 结构的选项，请参阅 <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> 页面。对应的 XML 方向为 <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> 与 <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>。完整的 API 请参考 <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">JSON 序列化指南</a>。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学习资源" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="文档" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="开发者指南" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
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