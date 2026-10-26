---
title: C# .NET용 DICOMweb 클라이언트 - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: HTTP를 통해 .NET에서 DICOM 연구를 검색, 다운로드 및 업로드합니다. QIDO-RS, WADO-RS 및 STOW-RS용 순수 C# DICOMweb 클라이언트로, PACS, VNA 및 클라우드 이미지 서비스와 연동됩니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#용 DICOMweb 클라이언트" h2="QIDO-RS, WADO-RS 및 STOW-RS를 사용하여 모든 DICOMweb 서버에서 연구를 검색, 다운로드 및 업로드하며, 관리되는 C#로 구현되어 머신에 별도 설치가 필요 없습니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="HTTP를 통한 DICOM, 직접 코드에서">}}

<p>DICOMweb은 DICOM의 HTTP 형태입니다. 포트 104에서의 DIMSE 연결 대신, 애플리케이션은 일반 HTTPS 요청을 보냅니다: 검색에는 QIDO-RS, 다운로드에는 WADO-RS, 업로드에는 STOW-RS를 사용합니다. Azure Health Data Services DICOM 서비스와 Google Cloud Healthcare API와 같은 클라우드 이미지 서비스도 이 방식을 사용하며, Orthanc 및 dcm4chee와 같은 아카이브도 마찬가지입니다.</p>

<p><strong>Aspose.Medical for .NET</strong>은 버전 26.10부터 DICOMweb 클라이언트를 포함합니다. <code>DicomWebClient</code>는 모든 표준 트랜잭션을 타입이 지정된 메서드로 래핑하며, 결과는 라이브러리의 다른 부분이 읽고 쓰는 동일한 <code>Dataset</code> 및 <code>DicomFile</code> 객체로 반환됩니다. 반대쪽 서버는 임의의 DICOMweb 구현일 수 있으며, Aspose.Medical로 구축될 필요는 없습니다.</p>

<p>클라이언트는 <code>HttpClient</code> 위에 구현된 관리형 C#입니다. 네이티브 툴킷이나 프록시 서비스가 없으므로 동일한 코드를 Windows, Linux 및 컨테이너에서 실행할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOMweb 서버에 연결">}}

<p>클라이언트를 <code>HttpClient</code>와 서버의 서비스 루트 URL을 사용해 생성합니다. 인증, TLS 설정, 프록시 및 연결 풀링은 이미 애플리케이션이 설정한 <code>HttpClient</code>에 유지됩니다. 클라이언트는 <code>HttpClient</code>를 소유하지 않으며, 이를 해제(dispose)하지도 않습니다.</p>

<div class="codeblock" id="code">
 <h3>DICOMweb 클라이언트 생성 - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>한 배포가 WADO-URI 또는 다른 서비스를 다른 URL에서 제공하는 경우, <code>ServiceBaseUris</code>는 해당 서비스에 대해서만 루트를 재정의합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="QIDO-RS를 사용한 연구 검색">}}

<p>검색은 DICOM 키워드를 이름으로 하는 매칭 파라미터, 반환할 필드 및 페이징 제어를 사용합니다. 매칭 결과는 데이터셋 스트림으로 전달되며, 응답이 다운로드되는 동안 하나씩 읽어들입니다.</p>

<div class="codeblock" id="code">
 <h3>환자 연구 찾기 - C#</h3>
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

<p><code>SearchSeriesAsync</code>와 <code>SearchInstancesAsync</code>는 한 단계 아래를 검색하며, 전체 아카이브 또는 특정 연구·시리즈 내부를 탐색합니다. 동일한 옵션으로 퍼지 매칭, 빈 값 매칭 및 페이징을 위한 결과 오프셋도 설정할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="WADO-RS를 사용한 연구 다운로드">}}

<p>하나의 연구에는 수천 개의 인스턴스가 포함될 수 있습니다. <code>RetrieveStudyAsync</code>는 이를 스트림으로 반환하므로, 응답의 각 부분이 도착할 때마다 파일을 파싱하고 다음 파일을 읽기 전에 저장할 수 있습니다. 메모리 사용량은 연구 크기에 비례하여 증가하지 않습니다.</p>

<div class="codeblock" id="code">
 <h3>연구의 모든 인스턴스 저장 - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code>와 <code>RetrieveInstanceAsync</code>는 시리즈 하나 혹은 인스턴스 하나를 동일한 방식으로 다운로드합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="썸네일 또는 렌더링 이미지 가져오기">}}

<p>뷰어 또는 워크리스트 화면은 종종 DICOM 파일이 아닌 이미지가 필요합니다. 서버가 이미지를 렌더링하여 JPEG 또는 PNG 형태로 전송하므로, 클라이언트는 원본 픽셀 데이터를 다운로드하거나 디코딩하지 않아도 됩니다.</p>

<div class="codeblock" id="code">
 <h3>인스턴스의 JPEG 썸네일 저장 - C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code>와 <code>RetrieveRenderedFramesAsync</code>는 대신 전체 크기의 이미지를 반환합니다. <code>DicomWebRenderOptions</code>는 이미지 품질, 뷰포트 크기, 윈도우 및 ICC 프로파일과 같은 표준 렌더링 제어 값을 서버에 전달합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="STOW-RS를 사용한 인스턴스 업로드">}}

<p>STOW-RS는 하나의 요청에 임의 개수의 인스턴스를 전송합니다. 서버는 저장된 인스턴스와 거부된 인스턴스를 각각 나열한 보고서를 응답합니다.</p>

<div class="codeblock" id="code">
 <h3>DICOM 파일 폴더 업로드 - C#</h3>
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

<p>일부 서버는 하나 이상의 인스턴스가 실패했을 때 409와 같은 HTTP 오류를 반환합니다. 이 경우 호출은 <code>DicomWebClientException</code>을 발생시키며(다음 섹션에 설명), 동일한 보고서가 예외의 응답 본문에 포함됩니다.</p>

<p><code>StoreStudyAsync</code>는 지정된 연구에 업로드합니다. 두 메서드 모두 <code>IAsyncEnumerable&lt;DicomFile&gt;</code>를 허용하므로, 요청을 전송하는 동안 파일을 순차적으로 생성할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="서버 오류 처리">}}

<p>모든 타입드 연산은 반환 전에 HTTP 상태를 확인합니다. 서버가 요청을 거부하면 호출은 <code>DicomWebClientException</code>을 throw하고, 이 예외에는 로그를 위해 상태 코드, 헤더 및 응답 본문의 시작 부분이 포함됩니다.</p>

<div class="codeblock" id="code">
 <h3>실패한 요청 보고 - C#</h3>
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

<p>위의 썸네일처럼 원시 응답을 반환하는 메서드는 <code>IsSuccessStatusCode</code> 또는 <code>EnsureSuccessStatusCode</code>를 사용해 상태 검사를 직접 수행하도록 합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="클라이언트가 지원하는 기타 기능">}}

<p>검색, 다운로드 및 업로드는 일반적인 호출입니다. 클라이언트는 다른 DICOMweb 서비스마다 별도의 작업 그룹을 제공합니다:</p>

<ul>
<li>인스턴스의 대용량 데이터, 픽셀 데이터 및 선택 프레임을 1 기반 프레임 번호로, 서버가 전송한 그대로 제공합니다.</li>
<li>WADO-URI: 많은 뷰어와 아카이브가 아직 사용하는 기존 단일 URL 조회 방식으로, DICOM 파일 및 렌더링 이미지를 가져옵니다.</li>
<li>UPS-RS 워크리스트: 작업 항목을 생성, 검색, 업데이트 및 클레임하고, 상태를 변경하며, 취소를 요청하고, WebSocket을 통해 도착하는 이벤트를 구독합니다.</li>
<li>모달리티 워크리스트(MWL-RS), 모달리티 수행 절차 단계(MPPS-RS) 및 저장 커밋.</li>
<li>색상 팔레트, 행잉 프로토콜 등 환자 외 인스턴스와 서버의 기능 문서.</li>
<li>타입드 메서드에 맞지 않을 때, 사용자 지정 라우트 값, 쿼리 파라미터, 헤더 및 본문을 사용해 모든 표준 트랜잭션을 수행하는 <code>SendAsync</code>.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb 클라이언트 가이드</a>에서는 모든 작업 및 옵션을 문서화하고, <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">클라이언트 작업 레퍼런스</a>에서는 모든 메서드를 나열합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTTP 응답부터 픽셀 데이터까지 하나의 라이브러리">}}

<p>클라이언트로 다운로드한 파일은 일반적인 <code>DicomFile</code>입니다. 동일한 프로세스 내에서 익명화, 트랜스코딩, 렌더링 또는 직렬화를 수행할 수 있으며, 별도의 변환 단계나 두 번째 라이브러리가 필요하지 않습니다.</p>

<p>관련 페이지: HTTP 대신 DIMSE를 사용하는 아카이브를 위한 <a href="/medical/net/dicom-networking/">DICOM 네트워킹</a>, 연구가 병원을 떠나기 전 <a href="/medical/net/anonymization/">익명화</a>, 서버가 저장하는 데이터를 위한 <a href="/medical/net/dicom-transfer-syntax-conversion/">전송 구문 변환</a>, 그리고 QIDO-RS 검색 결과가 사용하는 형식을 위한 <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="학습 자료" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="문서" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="개발자 가이드" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="API 레퍼런스" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="제품 지원" tabId="support" >}}
{{< blocks/products/pf/slr-element name="무료 지원" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="유료 지원" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="블로그" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="왜 Aspose.Medical을 .NET에 사용해야 할까요?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="고객 목록" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="성공 사례" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
