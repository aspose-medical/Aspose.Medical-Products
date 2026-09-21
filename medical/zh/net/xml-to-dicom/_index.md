---
title: 在 C# .NET 中将 XML 转换为 DICOM | Aspose.Medical
weight: 5000

description: 在 C# .NET 中从 PS3.19 的原生 DICOM 模型 XML 构建 DICOM 文件。支持从字符串、流或管道读取 XML，流式处理连续文档，并使用 Aspose.Medical API 解析 Bulk Data 引用。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="在 .NET C# 中将 XML 转换为 DICOM" h2="将 PS3.19 的原生 DICOM 模型 XML 读取回数据集和 DICOM 文件。支持从字符串、流或管道读取，流式处理连续文档，并解析 Bulk Data 引用。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="标准原生 DICOM 模型 XML">}}

<p><strong>Aspose.Medical for .NET</strong> 读取在 DICOM PS3.19 中定义的 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a>。这是一种写入标准本身的 XML 表示，并非 Aspose 发明的格式，这正是其在集成中有价值的原因：已经以 XML 方式交换 DICOM 的系统会生成本库可接受的文档。</p>

<p>文档根节点是 <code>NativeDicomModel</code>，每个属性都是一个携带标签、值表示和关键字的 <code>DicomAttribute</code> 元素：</p>

<div class="codeblock" id="code">
 <h3>Native DICOM Model 格式</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>此页面是 <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> 的逆向操作，两者均使用相同的类 <code>DicomXmlSerializer</code>。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="在 C# 中从 XML 创建 DICOM 文件">}}

<p><code>Deserialize</code> 将文档转换为 <code>Dataset</code>，然后将该数据集写入磁盘生成 DICOM 文件。</p>

<div class="codeblock" id="code">
 <h3>从 XML 创建 DICOM 文件 - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Native DICOM Model 没有文件元信息组，因此传输语法不属于文档内容。将数据集包装在 <code>DicomFile</code> 中时，会使用默认的传输语法 Implicit VR Little Endian 写入。若要使用其他传输语法存储文件，可进行转码，详见 <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> 页面。</p>

<p>读取 DICOM XML 是受许可的功能。如果未应用本地许可证，读取器会抛出 <code>MedicalApiException</code>，因此请先根据 <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">licensing guide</a> 的说明进行授权。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="流、管道和异步">}}

<p>每个入口都有流 overload 和异步 overload，异步 overload 还接受 <code>PipeReader</code>。来自网络响应的文档在读取时即被解析，无需先转换为字符串。</p>

<div class="codeblock" id="code">
 <h3>从流读取 XML - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="单个流中的连续文档">}}

<p>来自其他系统的导出常常在单个流中依次包含多个 <code>NativeDicomModel</code> 元素。<code>DeserializeAsyncEnumerable</code> 按输入顺序为每个元素产生一个数据集，从而在不将流全部加载到内存的情况下进行处理。这些元素直接相连：XML 声明只能出现在最开头，符合所有 XML 输入的规则。</p>

<div class="codeblock" id="code">
 <h3>流式处理连续文档 - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk Data 引用">}}

<p>像像素数据这类大值不会内联写入，而是以 <code>BulkData</code> 元素形式出现，携带指向字节的 URI，从而保持文档体积小。读取时若要解析这些引用，需要为序列化器提供 Bulk Data 加载器。<code>DefaultBulkDataLoader</code> 可在无认证的情况下获取 <code>file</code>、<code>http</code> 与 <code>https</code> URI；若归档需要凭证，请自行实现 <code>IBulkDataLoader</code> 或 <code>IAsyncBulkDataLoader</code>。</p>

<div class="codeblock" id="code">
 <h3>读取时解析 Bulk Data - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM 与 XML 的往返转换">}}

<p>这两个方向设计为配合使用：一个检查以 XML 形式输出，经过支持 XML 的系统后再返回为 DICOM 文件。所有操作均基于托管的 .NET，实现一次往返即可在 Windows、Linux 与 macOS 上运行。</p>

<div class="codeblock" id="code">
 <h3>DICOM 转 XML 再返回 - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>关于控制 XML 结构的选项，请参阅 <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> 页面。同样的配对也适用于 JSON：<a href="/medical/net/dicom-to-json/">DICOM to JSON</a> 与 <a href="/medical/net/json-to-dicom/">JSON to DICOM</a>。完整 API 说明请参考 <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">serialization guide</a>。</p>

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
{{< blocks/products/pf/slr-element name="客户列表" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功案例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}