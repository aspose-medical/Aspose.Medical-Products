---
title: C# .NET 中的 DICOM 网络 - C-ECHO、C-STORE、C-FIND | Aspose.Medical
weight: 9000

description: 将您的 .NET 应用程序连接到 PACS。使用 C-ECHO 进行验证，使用 C-STORE 发送图像，使用 C-FIND 查询，并通过自定义 SCP 接收图像。纯 C# 实现的 DIMSE 客户端和服务器。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# 中的 DICOM 网络" h2="在自有应用中与 PACS 通信：C-ECHO、C-STORE、C-FIND、C-MOVE 和 C-GET，既可作为客户端也可作为服务器，使用托管 C#，无需在机器上安装任何组件。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="从代码中连接到 PACS">}}

<p>读取 DICOM 文件是医学影像的容易一半。当您的应用程序需要与真实的医院系统交互时，必须使用 DIMSE：与 PACS 建立关联，发送图像，查询有哪些检查，并在其他系统返回数据时作出响应。</p>

<p><strong>Aspose.Medical for .NET</strong> 将该协议作为库的一部分提供。<code>Aspose.Medical.Dicom.Network</code> 为您提供 DIMSE 客户端和 DIMSE 服务器，均使用托管 C# 编写。无需安装本地工具包，无需配置服务，也没有平台特定的依赖，因而相同代码可在 Windows、Linux 以及容器中运行。</p>

<p>三项操作即可覆盖大多数集成，每项只需几行代码：使用 C-ECHO 检查连接，使用 C-STORE 推送图像，使用 C-FIND 查询对端内容。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="从 C-ECHO 开始">}}

<p>C-ECHO 是 DICOM 的 ping。它在出现其他错误之前先验证主机、端口和两个 AE 标题是否正确。构建一次客户端，提供一个处理响应的处理器，然后发送请求。</p>

<div class="codeblock" id="code">
 <h3>使用 C-ECHO 验证连接 - C#</h3>
 <pre><code class="cs">AssociationNegotiationOptions negotiation = new AssociationNegotiationOptions()
    .WithPresentationContext(new PresentationContext
    {
        AbstractSyntax = Uid.Verification,
        Role = null,
        TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
    });

DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = negotiation
    })
    .AddCEchoHandler((request, response, cancellationToken) =>
    {
        Console.WriteLine($"C-ECHO answered with status 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(new CEchoRequest());

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>状态 <code>0x0000</code> 表示成功。请求会被排队，然后在同一关联上发送，因此批量操作不会为每个项目单独打开连接。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="使用 C-STORE 发送图像">}}

<p>C-STORE 是应用在生成或接收图像后执行的操作：将实例推送到归档。为每个实例排队一个请求并一起发送。</p>

<div class="codeblock" id="code">
 <h3>将 DICOM 文件发送到 PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>您提出的呈现上下文决定归档接受的内容。如果对方需要压缩的传输语法，请在发送前进行转码，正如 <a href="/medical/net/dicom-transfer-syntax-conversion/">传输语法转换</a> 页面所示，或在 <code>AdditionalTransferSyntaxes</code> 中列出可选项，让协商选择其中一种。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="使用 C-FIND 查找检查">}}

<p>C-FIND 回答 “归档中有什么” 的问题。匹配项逐个返回，每个都有自己的标识数据集，最终响应结束查询。</p>

<div class="codeblock" id="code">
 <h3>按患者查询检查 - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.StudyRootQueryRetrieveInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddCFindHandler((request, response, cancellationToken) =>
    {
        // A match arrives with an identifier; the final response carries the status only
        if (response.Identifier is not null)
            Console.WriteLine(response.Identifier.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty));

        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(CFindRequest.CreateStudyQuery(
    patientId: "PATIENT-001",
    patientName: null,
    studyDateTime: null,
    accession: null,
    studyId: null,
    modalitiesInStudy: null,
    studyInstanceUid: null,
    priority: DimsePriority.Medium));

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>同一工厂用于构建其他查询层级：<code>CreatePatientQuery</code>、<code>CreateSeriesQuery</code> 和 <code>CreateImageQuery</code>。<code>CreateWorklistQuery</code> 构建模态工作列表查询，即模态在扫描前发送的查询。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="接收图像：自定义存储 SCP">}}

<p>该库同样可以作为服务器。为您想提供的服务注册处理器，开始监听后，您的应用程序就成为一个 DICOM 节点，模态或其他 PACS 可以向其发送数据。</p>

<div class="codeblock" id="code">
 <h3>接受传入图像 - C#</h3>
 <pre><code class="cs">DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Any, 11112)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.SecondaryCaptureImageStorage,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddSingletonCStoreHandler(new StoreHandler())
    .Build();

await server.StartAsync(CancellationToken.None);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>存储传入数据的处理器 - C#</h3>
 <pre><code class="cs">public sealed class StoreHandler : ICStoreRequestHandler
{
    public ValueTask&lt;CStoreResponse&gt; Handle(CStoreRequest request, CancellationToken cancellationToken)
    {
        // Write what arrived, then answer Success
        new DicomFile(request.Dataset).Save($"{request.AffectedSopInstanceUid}.dcm");

        CStoreResponse response = new();
        response.Command.AddOrUpdate(Tag.Status, (ushort)0x0000);
        return ValueTask.FromResult(response);
    }
}</code></pre>
</div>

<p>处理器对数据集的操作由您决定：写入磁盘、放入队列、先使用 <a href="/medical/net/anonymization/">匿名化 API</a> 进行匿名化，或转码为归档的传输语法。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="网络 API 还能覆盖哪些内容">}}

<p>上述三项服务是常用的。其余的 DIMSE 功能同样支持：</p>

<ul>
<li>检索：C-MOVE 和 C-GET，响应中报告子操作计数。</li>
<li>N 系列服务：N-CREATE、N-SET、N-GET、N-ACTION、N-DELETE 和 N-EVENT-REPORT，这些构成了存储承诺和 MPPS 的基础。</li>
<li>关联控制：呈现上下文、服务类角色、扩展协商、异步操作窗口、用户身份协商，以及可通过调用 AE 标题拒绝关联的策略钩子。</li>
<li>双端 TLS，使用 <code>TlsInitiatorAuthenticator</code> 和 <code>TlsAcceptorAuthenticator</code>，如需可自行进行证书验证。</li>
<li>各阶段超时设置，从 TCP 连接到释放，并提供关联生命周期通知，以便长期运行的节点记录发生的事件。</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">DICOM 网络指南</a> 记录了所有选项和处理器。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="纯 .NET，从套接字到像素数据">}}

<p>本页所有内容均来自同一套读取写入文件的托管代码包。关联、编解码器和解析器都来自同一个库，因此通过网络接收的检查可以在进程内部完成匿名化、转码或序列化，无需离开进程，也不依赖任何本地组件。</p>

<p>相关页面：<a href="/medical/net/dicom-transfer-syntax-conversion/">传输语法转换</a>（了解发送内容），<a href="/medical/net/anonymization/">匿名化</a>（了解需先移除的内容），以及 <a href="/medical/net/dicom-tags/">DICOM 标签</a>（阅读已到达的数据）。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学习资源" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="文档" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="开发者指南" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
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
