---
title: C# .NET 中的 DICOMweb 客户端 - QIDO-RS、WADO-RS、STOW-RS | Aspose.Medical
weight: 9100

description: 在 .NET 中通过 HTTP 搜索、下载和上传 DICOM 研究。一个纯 C# 编写的 DICOMweb 客户端，支持 QIDO-RS、WADO-RS 和 STOW-RS，可与 PACS、VNA 和云影像服务协同工作。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="适用于 .NET C# 的 DICOMweb 客户端" h2="使用 QIDO-RS、WADO-RS 和 STOW-RS 在任何 DICOMweb 服务器上搜索、下载和上传研究，使用托管的 C#，无需在机器上安装任何东西。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="通过 HTTP 使用 DICOM，来自您自己的代码">}}

<p>DICOMweb 是 DICOM 的 HTTP 形式。不是在 104 端口上的 DIMSE 关联，您的应用程序发送普通的 HTTPS 请求：用于搜索的 QIDO-RS、用于下载的 WADO-RS、用于上传的 STOW-RS。Azure Health Data Services 的 DICOM 服务和 Google Cloud Healthcare API 等云影像服务以及 Orthanc、dcm4chee 等归档系统都采用这种方式。</p>

<p><strong>Aspose.Medical for .NET</strong> 自 26.10 版起包含 DICOMweb 客户端。<code>DicomWebClient</code> 将每个标准事务封装为强类型方法，结果以库中其余部分读取和写入的相同 <code>Dataset</code> 和 <code>DicomFile</code> 对象返回。对端服务器可以是任意 DICOMweb 实现；不一定必须使用 Aspose.Medical 构建。</p>

<p>该客户端基于 <code>HttpClient</code> 的托管 C# 实现。没有本地工具包，也没有代理服务，因此相同的代码可在 Windows、Linux 和容器中运行。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="连接到 DICOMweb 服务器">}}

<p>从 <code>HttpClient</code> 和服务器的服务根 URL 创建客户端。认证、TLS 设置、代理和连接池均保留在 <code>HttpClient</code> 中，由您的应用程序已经配置。客户端不拥有 <code>HttpClient</code>，也不会对其进行释放。</p>

<div class="codeblock" id="code">
 <h3>创建 DICOMweb 客户端 - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>如果某个部署提供 WADO-URI 或其他服务的不同 URL，<code>ServiceBaseUris</code> 可仅为该服务覆盖根 URL。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="使用 QIDO-RS 搜索研究">}}

<p>搜索采用匹配参数（以 DICOM 关键字为名称）、返回的字段以及分页控制。匹配结果以数据集流的形式返回，在响应仍在下载时一次读取一个。</p>

<div class="codeblock" id="code">
 <h3>查找患者的研究 - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> 和 <code>SearchInstancesAsync</code> 在更低层级进行搜索，可在整个归档或单个研究或序列内搜索。相同的选项还可设置模糊匹配、空值匹配以及分页的结果偏移。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="使用 WADO-RS 下载研究">}}

<p>一个研究可能包含成千上万的实例。<code>RetrieveStudyAsync</code> 将它们作为流返回，因此每个文件在收到相应的响应片段时就被解析，并可在读取下一个之前保存。内存占用不会随研究大小增长。</p>

<div class="codeblock" id="code">
 <h3>保存研究的每个实例 - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> 和 <code>RetrieveInstanceAsync</code> 以相同方式下载单个序列或单个实例。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="获取缩略图或渲染图像">}}

<p>查看器或工作列表界面通常需要图片，而不是 DICOM 文件。服务器对其进行渲染并发送 JPEG 或 PNG，因此客户端无需下载或解码原始像素数据。</p>

<div class="codeblock" id="code">
 <h3>保存实例的 JPEG 缩略图 - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p><code>RetrieveRenderedInstanceAsync</code> 和 <code>RetrieveRenderedFramesAsync</code> 则返回完整尺寸的图像。<code>DicomWebRenderOptions</code> 将标准渲染控制参数（图像质量、视口大小、窗位以及 ICC 配置文件）传递给服务器。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="使用 STOW-RS 上传实例">}}

<p>STOW-RS 在一次请求中发送任意数量的实例。服务器返回报告，逐实例列出它存储的内容以及拒绝的内容。</p>

<div class="codeblock" id="code">
 <h3>上传 DICOM 文件夹 - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>某些服务器在一个或多个实例失败时会返回 HTTP 错误（例如 409）。此时调用会抛出 <code>DicomWebClientException</code>，如下一节所示，且相同的报告位于异常的响应体中。</p>

<p><code>StoreStudyAsync</code> 将实例上传到一个已命名的研究中。这两种方法也接受 <code>IAsyncEnumerable&lt;DicomFile&gt;</code>，因此可以在请求发送期间生成文件。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="处理服务器错误">}}

<p>每个强类型操作在返回前都会检查 HTTP 状态码。当服务器拒绝请求时，调用会抛出 <code>DicomWebClientException</code>，其中携带状态码、响应头以及响应体的开头部分供日志使用。</p>

<div class="codeblock" id="code">
 <h3>报告失败请求 - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>返回原始响应的函数（如上面的缩略图）将状态检查交由使用者，通过 <code>IsSuccessStatusCode</code> 或 <code>EnsureSuccessStatusCode</code> 手动完成。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="客户端还涵盖的功能">}}

<p>搜索、下载和上传是常用调用。客户端还为其他 DICOMweb 服务提供了相应的操作组：</p>

<ul>
<li>实例的批量数据、像素数据和指定帧（基于 1 的帧号），保持服务器发送的原始形式。</li>
<li>WADO-URI，许多查看器和归档仍使用的单 URL 检索方式，用于获取 DICOM 文件和渲染图像。</li>
<li>UPS-RS 工作列表：创建、搜索、更新和认领工作项，改变其状态，请求取消，并通过 WebSocket 订阅到达的事件。</li>
<li>模态工作列表（MWL-RS）、模态已执行过程步骤（MPPS-RS）以及存储承诺。</li>
<li>非患者实例，如颜色调色板和悬挂协议，以及服务器的功能文档。</li>
<li>当没有合适的强类型方法时，可使用 <code>SendAsync</code> 进行任意标准事务，携带自定义路由值、查询参数、请求头和请求体。</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb 客户端指南</a>记录了所有操作和选项，<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">客户端操作参考</a>列出了每个方法。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="一个库，从 HTTP 响应到像素数据">}}

<p>使用客户端下载的文件是普通的 <code>DicomFile</code>。它可以在同一进程中完成匿名化、转码、渲染或序列化，无需额外的转换步骤，也无需在链中使用第二个库。</p>

<p>相关页面：<a href="/medical/net/dicom-networking/">DICOM 网络</a>（适用于使用 DIMSE 而非 HTTP 的归档），<a href="/medical/net/anonymization/">匿名化</a>（在研究离开医院前），<a href="/medical/net/dicom-transfer-syntax-conversion/">传输语法转换</a>（针对服务器存储的格式），以及 <a href="/medical/net/dicom-to-json/">DICOM 转 JSON</a>（用于 QIDO-RS 搜索结果的格式）。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学习资源" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="文档" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="开发者指南" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="API 参考" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
