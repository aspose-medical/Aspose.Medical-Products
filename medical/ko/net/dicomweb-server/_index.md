---
title: C# .NET 기반 DICOMweb 서버 - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: C#에서 자체 DICOMweb 서버 또는 PACS를 구축하십시오. Aspose.Medical은 ASP.NET Core에서 QIDO-RS, WADO-RS, STOW-RS 및 워크리스트 서비스를 구현합니다; 저장소를 연결하면 됩니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#용 DICOMweb 서버" h2="ASP.NET Core 서비스에 QIDO-RS, WADO-RS 및 STOW-RS 엔드포인트를 추가하세요. 라이브러리는 프로토콜을 처리하고; 코드가 연구(Study)가 저장될 위치를 결정합니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="C# 기반 자체 DICOMweb 서버">}}

<p>PACS, VNA, 클라우드 아카이브 또는 연구 플랫폼은 DICOMweb 요청에 응답해야 합니다: QIDO-RS 검색, WADO-RS 다운로드 및 STOW-RS 업로드, 그리고 종종 워크리스트 서비스도 포함됩니다. 각각은 고유한 라우트, 미디어 타입, 멀티파트 본문, 매칭 규칙 및 상태 코드를 가지고 있습니다.</p>

<p><strong>Aspose.Medical for .NET</strong>은 버전 26.10부터 해당 프로토콜을 구현합니다. 모든 요청을 읽고 표준에 맞춰 검증하며, DICOM 규칙에 따라 검색 키를 매칭하고, 결과를 페이지화하고, 전송 구문을 변환하여 응답을 작성합니다. 사용자의 코드는 인스턴스를 저장하고, 검색 후보를 찾아 저장된 파일을 반환하는 작업을 수행합니다. 뷰어, 모달리티 및 기타 아카이브는 이제 모든 DICOMweb 서버와 마찬가지로 귀하의 서버와 작동하며, 동일한 라이브러리의 <a href="/medical/net/dicomweb-client/">DICOMweb client</a>도 마찬가지입니다.</p>

<p>서버는 C#로 관리됩니다. ASP.NET Core가 실행되는 어디서든 작동합니다: Windows, Linux 및 컨테이너에서도.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ASP.NET Core에서 완전한 서버">}}

<p><code>Aspose.Medical.Dicom.Web.AspNetCore</code> 패키지를 설치합니다. 이 패키지는 Aspose.Medical을 포함하고 DICOMweb 서비스를 의존성 주입 및 엔드포인트 라우팅에 연결합니다. 이 <code>Program.cs</code>는 작동하는 Studies 서버이며, 등록된 두 클래스가 아래에 표시됩니다.</p>

<div class="codeblock" id="code">
 <h3>DICOMweb 서버의 Program.cs - C#</h3>
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

<p>모든 Studies 라우트는 이제 <code>/dicomweb</code> 아래에서 응답합니다: 검색 및 업로드를 위한 <code>/dicomweb/studies</code>, 검색을 위한 <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code>, 그리고 메타데이터, 프레임, 벌크 데이터 및 렌더링 이미지에 대한 라우트가 있습니다. 서비스 루트에 있는 기능 문서는 서버가 지원하는 트랜잭션을 정확히 나열합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="스토리지 연결">}}

<p>Studies 서비스는 세 가지 메서드를 가진 하나의 인터페이스가 필요합니다: 인스턴스 저장, 저장된 파일 반환, 검색 후보 반환. 이 구현은 각 인스턴스를 연구와 시리즈별 폴더에 파일로 보관합니다.</p>

<div class="codeblock" id="code">
 <h3>QIDO-RS, WADO-RS 및 STOW-RS 뒤의 폴더 아카이브 - C#</h3>
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

<p>라이브러리는 제공자를 호출하기 전에 모든 요청 경로와 업로드된 각 인스턴스의 UID를 검사합니다. 유효한 UID가 없는 인스턴스는 실패로 보고되며 <code>StoreAsync</code>에 도달하지 않습니다.</p>

<p>서버를 테스트하기에 폴더만으로도 충분합니다. 실제 운영 아카이브는 동일한 세 메서드를 유지하고 <code>StoreAsync</code>와 <code>RetrieveAsync</code> 뒤에 객체 저장소를, <code>FindCandidatesAsync</code> 뒤에 데이터베이스 인덱스를 배치합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="검색: 라이브러리는 DICOM 매칭 규칙을 적용합니다">}}

<p>QIDO-RS 매칭은 단순 문자열 비교를 넘어섭니다: 와일드카드, 날짜·시간 범위, UID 목록, 퍼지(person name) 이름, 빈 값 및 시퀀스 내부 키 등. 제공자는 이를 구현하지 않습니다. 후보를 안정적인 순서로 반환하며, 일치 수보다 더 많이 반환할 수도 있습니다. 라이브러리는 각 후보에 정확한 DICOM 매치를 적용하고 요청된 필드를 유지하며 <code>offset</code>과 <code>limit</code>을 적용합니다. 기본적으로 하나의 응답은 최대 1000개의 결과를 포함하며, <code>DicomWebStandardServiceOptions</code>를 통해 이 제한을 변경할 수 있습니다.</p>

<p>대규모 아카이브에서 검색 속도를 유지하려면 선택적인 키를 데이터베이스에 전달합니다. <code>search.Plan.Filter</code>는 컴파일된 검색 키를 보관하며, 각 속성, 매칭 종류 및 값들을 포함합니다:</p>

<div class="codeblock" id="code">
 <h3>검색에서 정확한 Patient ID 읽기 - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>이와 같이 구성된 데이터베이스 쿼리는 예를 들어 변환할 수 없는 키를 무시할 경우 추가 행을 반환할 수 있습니다. 라이브러리의 최종 매치는 이러한 행을 제거합니다. 실제 매치를 삭제해서는 안 됩니다, 왜냐하면 라이브러리는 이를 다시 추가할 수 없기 때문입니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="저장: 각 인스턴스에 대한 결과">}}

<p>STOW-RS 요청은 수백 개의 인스턴스를 포함할 수 있습니다. 라이브러리는 멀티파트 본문을 파싱하고 각 인스턴스마다 차례로 <code>StoreAsync</code>를 호출하므로 전체 업로드가 한 번에 메모리에 유지되지 않습니다. 각 인스턴스에 대해 제공자는 <code>Stored</code>, 변경된 인스턴스에 대해 <code>Warning</code>, 또는 선택적인 DICOM 사유 코드와 함께 <code>Failed</code>를 반환합니다.</p>

<p>라이브러리는 이러한 결과를 기반으로 STOW-RS 보고서를 생성하고 상태 코드를 선택합니다: 모든 인스턴스가 저장된 경우 200, 일부가 경고 또는 실패인 경우 202, 모두 실패한 경우 409. 하나의 명명된 연구에 대한 요청에 포함된 다른 연구에 속하는 인스턴스는 제공자가 호출되기 전에 거부됩니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="검색: 전송 구문, 프레임 및 렌더링 이미지">}}

<p>제공자는 저장된 파일을 반환하고, 라이브러리는 클라이언트가 요청한 형태로 전달합니다. WADO-RS 클라이언트는 <code>Accept</code> 헤더에서 전송 구문을 요청할 수 있으며, 라이브러리는 자체 코덱(HTJ2K 및 JPEG XL 포함)을 사용해 각 파일을 전송 전에 변환합니다. DICOM JSON 또는 XML 형태의 메타데이터, 벌크 데이터 및 단일 프레임 역시 별도 코드 없이 동일한 제공자로부터 제공됩니다.</p>

<p>렌더링된 이미지는 추가 클래스를 필요로 합니다. 서버가 어떤 이미지를 생성할지 결정하는 것은 사용자이기 때문입니다. 이 클래스는 렌더링된 인스턴스, 렌더링 프레임 및 썸네일에 JPEG를 제공하고, 라이브러리는 이를 렌더링합니다:</p>

<div class="codeblock" id="code">
 <h3>렌더링된 JPEG 이미지 및 썸네일 제공 - C#</h3>
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

<p>내장 렌더러는 단일 프레임 인스턴스 또는 요청이 선택한 프레임에서 JPEG를 생성합니다. 전체 멀티프레임 인스턴스 렌더링, 다른 이미지 포맷, 그리고 뷰포트, 윈도우 및 ICC 프로파일 파라미터는 이 클래스에서 사용자가 구현한 <code>RetrieveAsync</code>가 필요합니다.</p>

<p>썸네일은 환자 개인정보 보호를 위해 신중히 처리됩니다. 내장 렌더러는 Burned In Annotation 및 Recognizable Visual Features 속성이 모두 <code>NO</code>인 인스턴스에서만 썸네일을 생성합니다. 이는 해당 이미지가 환자 데이터를 표시하지 않음을 의미합니다. 많은 이미지가 이러한 속성을 포함하지 않으며, 이런 경우 서버는 406을 반환합니다. 아카이브에 이러한 이미지의 썸네일이 필요하면, 자체 <code>RetrieveAsync</code>에서 생성하고 환자 데이터를 숨기도록 해야 합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="워크리스트, 절차 단계 및 기타 서비스">}}

<p>다른 DICOMweb 서비스도 동일한 모델을 따릅니다: 서비스를 등록하고 저장소 인터페이스를 구현합니다.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> 워크리스트를 <code>AddStandardWorklistService</code>로: 작업 항목을 생성, 검색, 업데이트하고 상태를 변경합니다. 상태 규칙은 라이브러리가 검사하며 이벤트 알림은 WebSocket을 통해 전송됩니다.</li>
<li><a href="/medical/net/modality-worklist/">Modality worklist</a> (MWL-RS)를 <code>AddStandardModalityWorklistService</code>와 함께 사용하여, 모달리티가 RIS의 예정된 절차 단계를 조회할 수 있게 합니다.</li>
<li>Modality performed procedure steps (MPPS-RS), 저장 커밋 및 컬러 팔레트, hanging protocol 등 환자와 무관한 인스턴스.</li>
<li>구버전 뷰어를 위한 WADO-URI 검색, Studies 서비스가 제공.</li>
</ul>

<p>특정 트랜잭션의 표준 동작이 맞지 않을 경우 <code>AddHandler</code>로 자체 핸들러를 등록하십시오. 라이브러리는 여전히 라우팅 및 요청 검증을 수행하고, 다른 트랜잭션은 표준 핸들러를 유지합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="보안 및 호스팅은 여러분의 손에 달려 있습니다">}}

<p>DICOMweb 엔드포인트는 일반적인 ASP.NET Core 엔드포인트입니다. 인증, 인가, CORS, TLS, 전달된 헤더 및 요청 크기 제한은 서비스의 다른 부분과 동일하게 구성됩니다.</p>

<div class="codeblock" id="code">
 <h3>인증된 사용자 요구 - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>프로토콜 부분은 ASP.NET Core에 의존하지 않습니다. <code>DicomWebApplication</code>은 <code>DicomWebRequest</code>를 응답으로 변환하므로, 동일한 서비스를 다른 HTTP 서버 뒤에서 실행하거나 네트워크 없이 테스트 환경에서도 사용할 수 있습니다.</p>

<div class="codeblock" id="code">
 <h3>ASP.NET Core 없이 동일한 서버 - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="클라이언트와 서버를 하나의 라이브러리로">}}

<p>Aspose.Medical의 <a href="/medical/net/dicomweb-client/">DICOMweb client</a>는 유닛 테스트와 콘솔 프로그램에서 서버를 테스트하며, 서버가 수신하는 모든 파일은 동일 프로세스 내에서 익명화, 트랜스코딩 또는 렌더링이 가능한 일반 <code>DicomFile</code>입니다.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb 개발자 가이드</a>는 모든 서비스와 저장소 인터페이스를 다루며, <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core 가이드</a>는 엔드포인트 매핑 및 WebSocket을 다룹니다.</p>

<p>관련 페이지: HTTP 대신 DIMSE를 통해 연구를 전송하는 시스템을 위한 <a href="/medical/net/dicom-networking/">DICOM 네트워킹</a>, 들어오는 연구의 <a href="/medical/net/anonymization/">익명화</a>, <a href="/medical/net/dicom-transfer-syntax-conversion/">전송 구문 변환</a>, 그리고 QIDO-RS 결과 형식을 위한 <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> 등.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="학습 자료" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="문서" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="개발자 가이드" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="API 참조" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
