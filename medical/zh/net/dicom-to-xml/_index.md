---
title: 在 C# .NET 中将 DICOM 转换为 XML | Aspose.Medical
weight: 3000
description: 在 C# .NET 中将 DICOM 数据集序列化为标准 DICOM XML 格式。使用 Aspose.Medical API 配置批量数据处理、基于流的处理以及异步操作。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="在 .NET C# 中将 DICOM 转换为 XML" h2="将 DICOM 数据集序列化为标准 DICOM XML 表示（PS3.19）。使用纯 .NET 库配置批量数据引用、基于流的输出和异步处理。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="基于标准的 DICOM XML 序列化">}}

<p><strong>Aspose.Medical for .NET</strong> 将 DICOM 数据序列化为符合 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a> 的 XML。它是用于在 XML 中表示 DICOM 数据集的官方标准，被 DICOMweb 服务、集成平台以及需要可读且符合模式验证的医学影像元数据的系统所使用。</p>

<p>The <code>DicomXmlSerializer</code> 类提供用于序列化和反序列化的静态方法。不同于简单的标签转储方式，输出符合 DICOM XML 架构，每个元素都包含其标签、VR 以及正确格式化的值 &mdash; 实现二进制 DICOM 与 XML 之间的无损往返转换。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="在 C# 中将 DICOM 序列化为 XML">}}

<p>使用 <code>DicomXmlSerializer</code> 类将 DICOM 数据集转换为 XML 字符串。最简单的方式生成符合标准的 XML 文档：</p>

<div class="codeblock" id="code">
 <h3>将 DICOM 转换为 XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="基于流的异步序列化">}}

<p>对于大型 DICOM 文件或高吞吐场景，直接序列化到流以避免在内存中分配大型字符串。同步和异步方法均可用：</p>

<div class="codeblock" id="code">
 <h3>同步流序列化 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>异步流序列化 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="大型检查的流水线流式处理">}}

<p>整个检查无需一次性加载到内存中。<code>DicomXmlSerializer</code> 写入 <code>PipeWriter</code> 并从 <code>PipeReader</code> 读取，因此 XML 可以在流动过程中生成并消费，并且可以通过 <code>DeserializeAsyncEnumerable</code> 一次读取一个数据集。所有方法均接受 <code>CancellationToken</code>。</p>

<div class="codeblock" id="code">
 <h3>通过管道进行序列化和反序列化 - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>一次读取一个数据集序列 - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="序列化选项">}}

<p><code>DicomXmlSerializerOptions</code> 类控制 DICOM 数据在 XML 中的表示方式。主要配置涉及对大型二进制值的批量数据处理：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>属性</th>
<th>类型</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>自定义转换器，用于将大型数据（例如像素数据）写入为 BulkData URI 引用，而不是内联。</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>自定义加载器，用于在反序列化期间解析 BulkData URI。</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>当未提供自定义选项时使用的默认选项实例。</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>使用自定义选项进行序列化 - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="批量数据处理">}}

<p>大型二进制值（像素数据、波形、封装文档）可外部化为 BulkData URI 引用，而不是内联在 XML 输出中。这遵循 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">DICOM PS3.19 BulkData 元素</a> 规范。</p>

<p>实现 <code>IBulkDataConverter</code> 可在序列化期间外部化大型数据，实现 <code>IBulkDataLoader</code> 可在反序列化期间解析 URI。对于常见情况根本不需要编写加载器：<code>DefaultBulkDataLoader.Instance</code> 能解析 <code>file</code>、<code>http</code> 和 <code>https</code> URI，并且实现了 <code>IAsyncBulkDataLoader</code>，因此在流式路径上可异步获取批量数据。</p>

<div class="codeblock" id="code">
 <h3>自定义批量数据处理 - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
public class FileBulkDataConverter : IBulkDataConverter
{
    public string? GetBulkDataUri(IElement element)
    {
        // Externalize pixel data to a separate file
        if (element.Tag == Tag.PixelData)
            return "file:///bulk/pixeldata.raw";
        return null; // inline all other elements
    }
}

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="将 XML 反序列化为 DICOM">}}

<p>将 DICOM XML 解析回 Dataset 对象。支持字符串输入、流输入和异步操作：</p>

<div class="codeblock" id="code">
 <h3>将 XML 反序列化为 DICOM - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="XML 与 JSON 序列化对比">}}

<p>Aspose.Medical 同时支持 DICOM XML（PS3.19）和 DICOM JSON（PS3.18）序列化。这两种格式均提供无损的往返转换，但适用于不同的集成场景：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>特性</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>标准</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>模式验证</td><td>提供 XML Schema (XSD)</td><td>无正式模式</td></tr>
<tr><td>最佳适用</td><td>企业集成、HL7 CDA、审计日志、XDS 注册表</td><td>DICOMweb、REST API、FHIR ImagingStudy</td></tr>
<tr><td>可读性</td><td>冗长但自解释</td><td>紧凑且广泛支持</td></tr>
<tr><td>批量数据</td><td>带 URI 的 BulkData 元素</td><td>BulkDataURI 属性</td></tr>
<tr><td>序列化类</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学习资源" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="文档" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="源码" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
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
