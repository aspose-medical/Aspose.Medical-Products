---
title: C# .NET 下的 DICOMweb 服务器 - QIDO-RS、WADO-RS、STOW-RS | Aspose.Medical
weight: 9200

description: 使用 C# 构建自己的 DICOMweb 服务器或 PACS。Aspose.Medical 在 ASP.NET Core 上实现了 QIDO-RS、WADO-RS、STOW-RS 以及工作列表服务；您只需连接存储即可。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# DICOMweb 服务器" h2="向 ASP.NET Core 服务添加 QIDO-RS、WADO-RS 和 STOW-RS 端点。库实现协议；您的代码决定研究数据的存储位置。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="使用 C# 的自定义 DICOMweb 服务器">}}

<p>PACS、VNA、云归档或科研平台必须响应 DICOMweb 请求：QIDO-RS 查询、WADO-RS 下载、STOW-RS 上传，通常还包括工作列表服务。每项服务都有各自的路由、媒体类型、multipart 体、匹配规则和状态码。</p>

<p><strong>Aspose.Medical for .NET</strong> 自 26.10 版起实现了该协议端。它读取所有请求，依据标准进行校验，按照 DICOM 规则匹配搜索键，对结果进行分页，转换传输语法并写入响应。您的代码完成只有您能做的工作：存储实例、查找搜索候选并返回已存文件。查看器、模态设备及其他归档系统即可像使用任何 DICOMweb 服务器一样使用您的服务器，同样，<a href="/medical/net/dicomweb-client/">DICOMweb client</a> 也可以使用同一库。</p>

<p>服务器采用托管的 C# 编写。它可在任何运行 ASP.NET Core 的环境中运行：Windows、Linux 以及容器中。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ASP.NET Core 完整服务器">}}

<p>安装 <code>Aspose.Medical.Dicom.Web.AspNetCore</code> 包。它会引入 Aspose.Medical 并将 DICOMweb 服务连接到依赖注入和端点路由。此 <code>Program.cs</code> 是一个可运行的 Studies 服务器；下面展示了它注册的两个类。</p>

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

<p>所有 Studies 路由现在都位于 <code>/dicomweb</code> 下：<code>/dicomweb/studies</code> 用于查询和上传，<code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> 用于检索，以及用于元数据、帧、批量数据和渲染图像的路由。服务根路径的功能文档精确列出了服务器支持的事务。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="连接您的存储">}}

<p>Studies 服务需要一个包含三种方法的接口：存储实例、返回已存文件以及返回搜索候选。此实现将每个实例作为文件保存在按研究和序列划分的文件夹中。</p>

<div class="codeblock" id="code">
 <h3>基于文件夹的归档，支持 QIDO-RS、WADO-RS 和 STOW-RS - C#</h3>
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

<p>库在调用提供程序之前，会检查每个请求路径和每个上传实例中的 UID。没有有效 UID 的实例将被标记为失败，并且永不进入 <code>StoreAsync</code>。</p>

<p>仅使用文件夹即可尝试服务器。生产环境的归档仍保留这三种方法，并在 <code>StoreAsync</code> 与 <code>RetrieveAsync</code> 后接入对象存储，在 <code>FindCandidatesAsync</code> 后接入数据库索引。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="搜索：库应用 DICOM 匹配规则">}}

<p>QIDO-RS 的匹配不仅仅是字符串比较：通配符、日期和时间范围、UID 列表、模糊人名、空值以及序列内的键。提供程序并未实现这些功能。它以稳定顺序返回候选项，且可能返回超出匹配的结果。库对每个候选项应用精确的 DICOM 匹配，保留请求的字段，并使用 <code>offset</code> 与 <code>limit</code>。默认情况下，一个响应最多包含 1000 条结果，<code>DicomWebStandardServiceOptions</code> 可修改此限制。</p>

<p>为在大型归档上保持搜索速度，需要将选择性的键传递给数据库。<code>search.Plan.Filter</code> 保存已编译的搜索键，包含属性、匹配类型以及每个键的取值：</p>

<div class="codeblock" id="code">
 <h3>从搜索中读取精确的 Patient ID - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>以这种方式构建的数据库查询可能会返回多余的行，例如忽略了无法翻译的键。库在最终匹配时会剔除这些行。它不能丢弃真实的匹配，因为库无法再次补回。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="存储：每个实例都有结果">}}

<p>一次 STOW-RS 请求可能携带数百个实例。库解析 multipart 体并依次调用 <code>StoreAsync</code> 处理每个实例，从而避免一次性将整个上传内容载入内存。对于每个实例，提供程序返回 <code>Stored</code>、<code>Warning</code>（实例已存储但有更改）或 <code>Failed</code>（可选的 DICOM 原因码）。</p>

<p>库依据这些结果生成 STOW-RS 报告并选择相应的状态码：所有实例均成功存储时返回 200，部分出现警告或失败时返回 202，全部失败时返回 409。请求中属于另一个研究的实例将在调用提供程序前被拒绝。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="检索：传输语法、帧和渲染图像">}}

<p>提供程序返回已存文件，库会按客户端请求的形式交付。WADO-RS 客户端可在 <code>Accept</code> 头中指定传输语法，库会在发送前转换每个文件，并内置了自身的编解码器、HTJ2K 和 JPEG XL。DICOM JSON 或 XML 元数据、批量数据以及单帧均来自同一提供程序，无需额外代码。</p>

<p>渲染图像需要额外的类，因为只有您决定服务器可以生成哪些图像。该类为渲染实例、渲染帧和缩略图提供 JPEG，并由库完成渲染：</p>

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

<p>内置渲染器可从单帧实例或请求选择的帧生成 JPEG。整个渲染的多帧实例、其他图像格式，以及视口、窗宽/窗位和 ICC 配置文件等参数，需要您在此类中自行实现 <code>RetrieveAsync</code>。</p>

<p>缩略图的处理需关注患者隐私。内置渲染器仅在实例的 Burned In Annotation 与 Recognizable Visual Features 属性均为 <code>NO</code> 时生成缩略图，表示该图像未包含患者信息。许多图像并未携带这些属性，此类图像的请求将返回 406。如果您的归档需要此类图像的缩略图，请在自己的 <code>RetrieveAsync</code> 中生成，并确保遮蔽所有患者数据。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="工作列表、步骤及其他服务">}}

<p>其他 DICOMweb 服务遵循相同的模型：注册服务并实现其存储接口。</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> 工作列表，使用 <code>AddStandardWorklistService</code>：创建、查询、更新工作项并修改其状态，库会检查状态规则并通过 WebSocket 发送事件通知。</li>
<li>使用 <code>AddStandardModalityWorklistService</code> 的 <a href="/medical/net/modality-worklist/">模态工作列表</a>（MWL-RS），使模态设备能够查询您 RIS 中计划的过程步骤。</li>
<li>模态已执行过程步骤（MPPS-RS）、存储承诺以及非患者实例，如颜色调色板和悬挂协议。</li>
<li>为老旧查看器提供的 WADO-URI 检索，由 Studies 服务提供。</li>
</ul>

<p>当某个事务的标准行为不符合需求时，可使用 <code>AddHandler</code> 注册自定义处理器。库仍会进行路由和请求校验，其他事务保持其标准处理器。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="安全性和托管由您掌控">}}

<p>DICOMweb 端点就是普通的 ASP.NET Core 端点。认证、授权、CORS、TLS、转发头及请求大小限制的配置方式与服务其他部分相同：</p>

<div class="codeblock" id="code">
 <h3>要求已认证的用户 - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>协议部分并不依赖于 ASP.NET Core。<code>DicomWebApplication</code> 将 <code>DicomWebRequest</code> 转换为响应，因此相同的服务可以在其他 HTTP 服务器后面运行，或在无需网络的测试环境中运行。</p>

<div class="codeblock" id="code">
 <h3>不使用 ASP.NET Core 的相同服务器 - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="客户端与服务器同属一个库">}}

<p>Aspose.Medical 的 <a href="/medical/net/dicomweb-client/">DICOMweb client</a> 可在单元测试或控制台程序中测试您的服务器，服务器接收的每个文件都是常规的 <code>DicomFile</code>，可在同一进程中进行匿名化、转码或渲染。</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb 开发者指南</a>涵盖所有服务和存储接口，<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core 指南</a>则介绍端点映射和 WebSocket 的使用。</p>

<p>相关页面：<a href="/medical/net/dicom-networking/">DICOM 网络</a>（用于通过 DIMSE 而非 HTTP 发送研究的系统），<a href="/medical/net/anonymization/">匿名化</a>（对入库研究），<a href="/medical/net/dicom-transfer-syntax-conversion/">传输语法转换</a>，以及 <a href="/medical/net/dicom-to-json/">DICOM 转 JSON</a>（QIDO-RS 结果使用的格式）。</p>

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
{{< blocks/products/pf/slr-element name="客户列表" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功案例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
