---
title: 在 C# .NET 中将 DICOM 转换为 JSON | Aspose.Medical
weight: 4000
description: 在 C# .NET 中将 DICOM 文件序列化为标准 DICOM JSON 格式。使用 Aspose.Medical API 配置数字处理、批量数据 URI、关键字输出以及异步流处理。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="在 .NET C# 中将 DICOM 转换为 JSON" h2="将 DICOM 数据集和文件序列化为标准 DICOM JSON 模型（PS3.18）。配置数字处理、批量数据引用、关键字输出以及基于流的异步处理。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="标准 DICOM JSON 模型">}}

<p><strong>Aspose.Medical for .NET</strong> 将 DICOM 数据序列化为遵循 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a> 的 JSON。该模型是表示 DICOM 数据集的官方标准，已被 DICOMweb 服务、FHIR ImagingStudy 资源及现代医疗 API 所采用。</p>

<p>在 DICOM JSON 模型中，每个属性均以其标签作为 JSON 键（例如患者姓名对应 <code>"00100010"</code>），其值表示（VR）和数值数组作为嵌套属性：</p>

<div class="codeblock" id="code">
 <h3>DICOM JSON 模型格式</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="在 C# 中将 DICOM 序列化为 JSON">}}

<p>使用 <code>DicomJsonSerializer</code> 类将 DICOM 文件或数据集转换为 JSON。最简方法会生成紧凑且符合标准的输出：</p>

<div class="codeblock" id="code">
 <h3>将 DICOM 转换为 JSON - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p>您可以序列化 <code>Dataset</code>、完整的 <code>DicomFile</code>（包括文件元信息），或数据集数组：</p>

<div class="codeblock" id="code">
 <h3>序列化 DicomFile 和数据集数组 - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="基于流的异步序列化">}}

<p>对于大型 DICOM 文件或高吞吐场景，可直接序列化到 UTF-8 流，以避免在内存中分配大字符串。同步和异步方法均可使用：</p>

<div class="codeblock" id="code">
 <h3>流式及异步序列化 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="序列化选项">}}

<p><code>DicomJsonSerializerOptions</code> 类用于控制 DICOM 数据在 JSON 中的表示方式：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>属性</th>
<th>类型</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>控制数值 VR（IS、DS、SV、UV）是写为 JSON 数字还是字符串</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>使用 DICOM 关键字（例如 <code>"PatientName"</code>）而非标签（<code>"00100010"</code>）作为 JSON 键。注意：非标准</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>在 JSON 中额外包含 DICOM 关键字属性</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>在 JSON 中额外包含 DICOM 标签名称属性</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>自定义转换器，用于将大数据（如像素数据）写为 URI 引用</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>自定义加载器，用于在反序列化期间解析批量数据 URI</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>配置序列化选项 - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="数字处理">}}

<p>DICOM 数值型 Value Representation（IS、DS、SV、UV）可以序列化为 JSON 数字或 JSON 字符串。这对互操作性至关重要，因为部分 DICOM 实现会以前导空格或非标准格式存储数字：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>模式</th>
<th>JSON 输出</th>
<th>使用场景</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>符合标准，适合计算。若值无法解析则抛出异常。</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>保留原始字符串表示。对非标准数据的往返更安全。</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="批量数据处理">}}

<p>大型二进制值（像素数据、波形、封装文档）可作为批量数据引用处理，而不是内嵌在 JSON 输出中。这遵循 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 批量数据</a> 规范。</p>

<p>实现 <code>IBulkDataConverter</code> 可在序列化期间外部化大数据，实现 <code>IBulkDataLoader</code> 可在反序列化期间解析 URI：</p>

<div class="codeblock" id="code">
 <h3>自定义批量数据处理 - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="将 JSON 反序列化为 DICOM">}}

<p>将 DICOM JSON 解析回 Dataset 或 DicomFile 对象。支持字符串输入、流输入以及异步流操作：</p>

<div class="codeblock" id="code">
 <h3>将 JSON 反序列化为 DICOM - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="System.Text.Json 集成">}}

<p>Aspose.Medical 提供 <code>DatasetJsonConverter</code> 与 <code>DicomFileJsonConverter</code>，以便与标准的 <code>System.Text.Json</code> 序列化管道集成。这使得 DICOM 对象可以与其他 .NET 类型一起在现有的 JSON 处理代码中序列化。</p>

<div class="codeblock" id="code">
 <h3>System.Text.Json 集成 - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
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
