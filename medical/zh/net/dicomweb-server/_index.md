---
title: C# .NET 中的 DICOMweb 服务器 - QIDO-RS、WADO-RS、STOW-RS | Aspose.Medical
weight: 9200

description: 使用 C# 构建您自己的 DICOMweb 服务器或 PACS。Aspose.Medical 在 ASP.NET Core 上实现了 QIDO-RS、WADO-RS、STOW-RS 以及工作列表服务；您只需连接存储即可。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="适用于 .NET C# 的 DICOMweb 服务器" h2="向 ASP.NET Core 服务添加 QIDO-RS、WADO-RS 和 STOW-RS 端点。库负责协议实现，您的代码决定研究数据的存储位置。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="使用 C# 构建您自己的 DICOMweb 服务器">}}

<p>PACS、VNA、云存档或研究平台都必须响应 DICOMweb 请求：QIDO-RS 搜索、WADO-RS 下载、STOW-RS 上传，且通常还包括工作列表服务。每项服务都有其专属的路由、媒体类型、multipart 请求体、匹配规则和状态码。</p>

<p><strong>Aspose.Medical for .NET</strong> 自 26.10 版本起实现了该协议端。它读取每个请求，依据标准进行校验，按 DICOM 规则匹配搜索键，分页返回结果，转换传输语法并写入响应。您的代码负责只能由您完成的工作：存储实例、查找搜索候选并返回已存文件。查看器、模态设备和其他存档即可像使用任何其他 DICOMweb 服务器一样使用您的服务器，同样的 <a href="/medical/net/dicomweb-client/">DICOMweb client</a> 也可以使用该库。</p>

<p>服务器使用受管 C# 编写。它可以运行在任何支持 ASP.NET Core 的环境中：Windows、Linux 以及容器中。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="完整的 ASP.NET Core 服务器">}}

<p>安装 <code>Aspose.Medical.Dicom.Web.AspNetCore</code> 包。它会随同 Aspose.Medical 一起引入，并将 DICOMweb 服务连接到依赖注入和端点路由。此 <code>Program.cs</code> 是一个可运行的 Studies 服务器；下面展示了它注册的两个类。</p>

<div class="codeblock" id="code">
 <h3>DICOMweb 服务器的 Program.cs - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>所有 Studies 路由现在都在 <code>/dicomweb</code> 下响应：<code>/dicomweb/studies</code> 用于搜索和上传，<code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> 用于检索，以及用于元数据、帧、批量数据和渲染图像的路由。服务根目录下的功能（capabilities）文档列出了服务器支持的全部事务。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="连接您的存储">}}

<p>Studies 服务需要一个接口，包含三种方法：存储实例、返回已存文件以及返回搜索候选。此实现将每个实例以文件形式保存在每个 Study 和 Series 对应的文件夹中。</p>

<div class="codeblock" id="code">
 <h3>QIDO-RS、WADO-RS 与 STOW-RS 背后的文件夹存档 - C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>库会在调用提供者之前检查每个请求路径以及每个上传实例中的 UID。若实例的 UID 无效，将被标记为失败，且永远不会进入 <code>StoreAsync</code>。</p>

<p>使用文件夹即可尝试服务器。正式环境的存档仍保留这三个方法，只是将对象存储放在 <code>StoreAsync</code> 和 <code>RetrieveAsync</code> 后面，将数据库索引放在 <code>FindCandidatesAsync</code> 后面。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="搜索：库应用 DICOM 匹配规则">}}

<p>QIDO-RS 的匹配不仅仅是字符串比较，还包括通配符、日期时间范围、UID 列表、模糊人名、空值以及序列内的键。提供者本身不实现这些功能。它以稳定的顺序返回候选项，可能返回的数量超过匹配数。库对每个候选项执行严格的 DICOM 匹配，保留请求的字段，并应用 <code>offset</code> 与 <code>limit</code>。默认情况下，一个响应最多包含 1000 条结果，<code>DicomWebStandardServiceOptions</code> 可修改此限制。</p>

<p>为在大型存档上保持搜索速度，应将选择性的键传递给数据库。<code>search.Plan.Filter</code> 保存了编译后的搜索键，包括属性、匹配类型以及每个键的取值：</p>

<div class="codeblock" id="code">
 <h3>从搜索中读取精确的 Patient ID - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>以这种方式构建的数据库查询可能会返回多余的行，例如当它忽略了无法翻译的键时。库的最终匹配会剔除这些多余行。它不能丢失真实的匹配项，因为库无法再将其补回。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="存储：每个实例的结果">}}

<p>一次 STOW-RS 请求可能携带数百个实例。库解析 multipart 请求体，并依次调用 <code>StoreAsync</code> 处理每个实例，从而避免一次性将整个上传内容加载到内存。对于每个实例，提供者会返回 <code>Stored</code>、<code>Warning</code>（表示实例已存储但有所更改）或 <code>Failed</code>（可附带可选的 DICOM 原因码）。</p>

<p>库根据这些结果生成 STOW-RS 报告并选择返回状态码：当所有实例均成功存储时返回 200；部分出现警告或失败时返回 202；全部失败时返回 409。请求中属于其他 Study 的实例会在调用提供者前被拒绝。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="检索：传输语法、帧和渲染图像">}}

<p>提供者返回已存文件，库按照客户端请求的形式交付。WADO-RS 客户端可以在 <code>Accept</code> 头中指定传输语法，库会在发送前对每个文件进行转换，内部已包含 HTJ2K、JPEG XL 等编解码器。DICOM JSON 或 XML 元数据、批量数据以及单帧图像均来源于同一提供者，无需额外代码。</p>

<p>渲染图像需要额外的类，因为由您决定服务器可以生成哪些图像。该类为渲染实例、渲染帧和缩略图提供 JPEG 支持，库会使用它进行渲染：</p>

<div class="codeblock" id="code">
 <h3>提供渲染的 JPEG 图像和缩略图 - C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>内置渲染器可从单帧实例或请求选择的帧生成 JPEG。对于整体渲染的多帧实例、其他图像格式，以及视口、窗宽窗位和 ICC 配置文件参数，需要您在该类中实现自定义的 <code>RetrieveAsync</code>。</p>

<p>缩略图在处理时会特别关注患者隐私。内置渲染器仅在实例的 Burned In Annotation 与 Recognizable Visual Features 属性均为 <code>NO</code> 时生成缩略图，表示该图像声明不包含患者信息。许多图像并未携带这些属性，对此类图像服务器会返回 406。如果您的存档需要此类图像的缩略图，请在自定义的 <code>RetrieveAsync</code> 中生成，并确保隐藏所有患者信息。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="工作列表、流程步骤及其他服务">}}

<p>其他 DICOMweb 服务遵循相同的模型：注册服务并实现其存储接口。</p>

<ul>
<li>使用 <code>AddStandardWorklistService</code> 的 UPS-RS 工作列表：创建、搜索、更新工作项并修改其状态，状态规则由库检查，事件通知通过 WebSocket 发送。</li>
<li>使用 <code>AddStandardModalityWorklistService</code> 的模态工作列表（MWL-RS），让模态设备能够查询您 RIS 中已排程的流程步骤。</li>
<li>模态执行的流程步骤（MPPS-RS）、存储承诺以及非患者实例，如颜色调色板和悬挂协议。</li>
<li>为旧版查看器提供的 WADO-URI 检索，由 Studies 服务提供。</li>
</ul>

<p>当某个事务的标准行为不符合需求时，可使用 <code>AddHandler</code> 注册自定义处理器。库仍会进行路由和请求校验，其他事务继续保留其标准处理器。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="安全性与托管由您掌控">}}

<p>DICOMweb 端点即普通的 ASP.NET Core 端点。认证、授权、CORS、TLS、转发头以及请求大小限制的配置方式与服务的其他部分相同：</p>

<div class="codeblock" id="code">
 <h3>要求已认证的用户 - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>协议部分并不依赖于 ASP.NET Core。<code>DicomWebApplication</code> 将 <code>DicomWebRequest</code> 转换为响应，因此相同的服务可在其他 HTTP 服务器后面运行，或在无需网络的测试环境中使用。</p>

<div class="codeblock" id="code">
 <h3>无需 ASP.NET Core 的相同服务器 - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="客户端与服务器同源于一个库">}}

<p>Aspose.Medical 的 <a href="/medical/net/dicomweb-client/">DICOMweb client</a> 可通过单元测试或控制台程序对您的服务器进行测试，服务器接收的每个文件都是常规的 <code>DicomFile</code>，可在同一进程内进行匿名化、转码或渲染。</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb 开发者指南</a> 详细介绍了所有服务和存储接口，而 <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core 指南</a> 则涵盖了端点映射和 WebSockets 的使用。</p>

<p>相关页面：<a href="/medical/net/dicom-networking/">DICOM 网络</a>（用于通过 DIMSE 而非 HTTP 传输研究），<a href="/medical/net/anonymization/">匿名化</a>（对传入的研究），<a href="/medical/net/dicom-transfer-syntax-conversion/">传输语法转换</a>，以及 <a href="/medical/net/dicom-to-json/">DICOM 转 JSON</a>（用于 QIDO-RS 结果的格式）。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学习资源" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="文档" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="开发者指南" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="API 参考" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
