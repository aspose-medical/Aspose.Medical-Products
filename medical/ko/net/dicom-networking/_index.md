---
title: C# .NET에서 DICOM 네트워킹 - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: .NET 애플리케이션을 PACS에 연결하십시오. C-ECHO로 확인하고, C-STORE로 이미지를 전송하며, C-FIND로 쿼리하고, 자체 SCP로 이미지를 수신합니다. 순수 C#으로 구현된 DIMSE 클라이언트 및 서버입니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 DICOM 네트워킹" h2="자체 애플리케이션에서 PACS와 통신합니다: C-ECHO, C-STORE, C-FIND, C-MOVE 및 C-GET를 클라이언트와 서버 모두에서, 설치가 필요 없는 관리형 C#으로." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="코드에서 직접 PACS에 연결">}}

<p>DICOM 파일을 읽는 것은 의료 영상의 쉬운 절반입니다. 애플리케이션이 실제 병원 시스템과 연동해야 할 순간, DIMSE를 사용해야 합니다: PACS와 연관(association)을 열고, 이미지를 전송하고, 어떤 연구가 있는지 질의하며, 다른 시스템이 무언가를 보낼 때 응답합니다.</p>

<p><strong>Aspose.Medical for .NET</strong>은(는) 해당 프로토콜을 라이브러리의 일부로 제공합니다. <code>Aspose.Medical.Dicom.Network</code>는 관리형 C#으로 작성된 DIMSE 클라이언트와 DIMSE 서버를 제공합니다. 설치할 네이티브 툴킷이나 구성할 서비스가 없으며 플랫폼에 종속되지 않으므로 동일한 코드가 Windows, Linux 및 컨테이너에서 실행됩니다.</p>

<p>대부분의 통합을 포괄하는 세 가지가 있으며, 각각 몇 줄이면 됩니다: C-ECHO로 연결을 확인하고, C-STORE로 이미지를 푸시하며, C-FIND로 상대편에 무엇이 있는지 찾습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-ECHO부터 시작하세요">}}

<p>C-ECHO는 DICOM 핑입니다. 다른 문제가 발생하기 전에 호스트, 포트 및 두 AE 타이틀이 올바른지 확인해 줍니다. 클라이언트를 한 번 만들고, 응답을 관찰하는 핸들러를 제공한 뒤 요청을 전송합니다.</p>

<div class="codeblock" id="code">
 <h3>C-ECHO로 연결 확인 - C#</h3>
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

<p>상태 <code>0x0000</code>은 성공을 의미합니다. 요청은 큐에 쌓인 후 하나의 연관으로 전송되므로, 작업 배치가 항목당 연결을 열지 않습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-STORE로 이미지 전송">}}

<p>C-STORE는 애플리케이션이 이미지를 생성하거나 수신한 후 수행하는 작업으로, 인스턴스를 아카이브에 푸시합니다. 인스턴스당 하나의 요청을 큐에 넣고 함께 전송합니다.</p>

<div class="codeblock" id="code">
 <h3>DICOM 파일을 PACS에 전송 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>제안하는 프레젠테이션 컨텍스트는 아카이브가 수락할 항목을 결정합니다. 압축 구문이 필요하면 전송 전에 트랜스코딩하세요, <a href="/medical/net/dicom-transfer-syntax-conversion/">전송 구문 변환</a> 페이지가 보여주듯이, 혹은 <code>AdditionalTransferSyntaxes</code>에 대안을 나열하고 협상이 하나를 선택하도록 할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-FIND로 연구 검색">}}

<p>C-FIND는 “아카이브에 무엇이 있는가”라는 질문에 답합니다. 일치 항목은 하나씩 도착하며 각각 고유 식별자 데이터셋을 가지고, 최종 응답이 쿼리를 종료합니다.</p>

<div class="codeblock" id="code">
 <h3>환자별 연구 조회 - C#</h3>
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

<p>같은 팩터리가 다른 쿼리 레벨을 구축합니다: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> 및 <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code>는 모달리티 작업 목록 쿼리를 구축하는데, 이는 스캔 전에 모달리티가 요청하는 쿼리입니다; <a href="/medical/net/modality-worklist/">모달리티 작업 목록 페이지</a>에서 MPPS 및 작업 목록 서버 측과 함께 확인할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="이미지 수신: 자체 저장 SCP">}}

<p>이 라이브러리는 서버 역할도 수행합니다. 제공하려는 서비스에 대한 핸들러를 등록하고, 수신을 시작하면 애플리케이션이 모달리티나 다른 PACS가 보낼 수 있는 DICOM 노드가 됩니다.</p>

<div class="codeblock" id="code">
 <h3>입력 이미지 수락 - C#</h3>
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
 <h3>도착한 데이터를 저장하는 핸들러 - C#</h3>
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

<p>핸들러가 데이터셋을 어떻게 처리할지는 사용자의 결정입니다: 디스크에 기록하거나, 큐에 넣거나, <a href="/medical/net/anonymization/">익명화 API</a>를 사용해 먼저 익명화하거나, 아카이브 구문으로 트랜스코딩할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="네트워킹 API가 다루는 기타 항목">}}

<p>위의 세 서비스가 일반적인 서비스입니다. 나머지 DIMSE도 제공됩니다:</p>

<ul>
<li>검색: C-MOVE와 C-GET, 하위 작업 카운터가 응답에 보고됩니다.</li>
<li>N-서비스: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE 및 N-EVENT-REPORT, 이들로 스토리지 커밋먼트와 MPPS가 구성됩니다.</li>
<li>연관 제어: 프레젠테이션 컨텍스트, 서비스 클래스 역할, 확장 협상, 비동기 작업 윈도우, 사용자 신원 협상, 그리고 AE 타이틀을 호출하여 연관을 거부할 수 있는 정책 훅.</li>
<li>양쪽 모두 TLS, <code>TlsInitiatorAuthenticator</code>와 <code>TlsAcceptorAuthenticator</code>를 통해, 필요 시 자체 인증서 검증을 사용할 수 있습니다.</li>
<li>TCP 연결부터 해제까지 모든 단계에 대한 타임아웃 및 연관 수명 주기에 대한 알림으로 장기 실행 노드가 발생한 일을 기록할 수 있습니다.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">DICOM 네트워킹 가이드</a>는 모든 옵션과 모든 핸들러를 문서화합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="소켓부터 픽셀 데이터까지 순수 .NET">}}

<p>이 페이지의 모든 내용은 파일을 읽고 쓰는 동일 패키지에서 제공되는 관리형 코드입니다. 연관, 코덱 및 파서는 하나의 라이브러리에서 제공되므로 네트워크를 통해 수신된 연구를 프로세스를 벗어나지 않고, 체인 어디에도 네이티브 종속성이 없이 익명화, 트랜스코딩 또는 직렬화할 수 있습니다.</p>

<p>관련 페이지: <a href="/medical/net/dicom-transfer-syntax-conversion/">전송 구문 변환</a> (전송할 내용), <a href="/medical/net/anonymization/">익명화</a> (먼저 제거할 내용), 그리고 <a href="/medical/net/dicom-tags/">DICOM 태그</a> (도착한 내용 읽기)입니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="학습 자료" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="문서" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="개발자 가이드" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="API 레퍼런스" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="제품 지원" tabId="support" >}}
{{< blocks/products/pf/slr-element name="무료 지원" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="유료 지원" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="블로그" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="왜 .NET용 Aspose.Medical인가?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="고객 목록" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="성공 사례" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
