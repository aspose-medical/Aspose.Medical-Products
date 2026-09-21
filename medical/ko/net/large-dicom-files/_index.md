---
title: C# .NET에서 대용량 DICOM 파일 작업 | Aspose.Medical
weight: 11500

description: C#에서 멀티프레임 연구와 전체 슬라이드 이미지를 메모리에 로드하지 않고 열 수 있습니다. 픽셀 데이터를 제외하고 메타데이터를 읽고, 대형 요소를 연기하며, 파일을 스트림 및 파이프를 통해 이동합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 대용량 DICOM 파일" h2="멀티프레임 연구의 메타데이터를 픽셀 없이 읽고, 대형 요소를 필요할 때까지 연기하며, 전체 파일을 스트림 및 파이프를 통해 이동합니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="파일은 크지만, 질문은 보통 작습니다">}}

<p>전체 슬라이드 이미지, 긴 CT 시리즈 또는 OCT 볼륨은 수백 메가바이트에 이르며, 대부분이 픽셀 데이터입니다. 애플리케이션이 실제로 수행하는 작업은 보통 훨씬 작습니다: 폴더에 무엇이 있는지 목록화하고, 환자 식별자를 확인하고, 프레임 수를 세며, 연구가 어디로 가야 하는지 결정합니다. 이를 위해 모든 바이트를 로드하는 것이 단순 작업을 메모리 문제로 전환시킵니다.</p>

<p><strong>Aspose.Medical for .NET</strong>은 호출자가 파일을 얼마나 읽을지 결정하도록 합니다. 선택은 <code>DicomFile.Open</code>의 한 인수이며, 파일, 스트림 및 파이프 모두에 적용됩니다.</p>

<p>동일한 머신과 동일한 파일에서 테스트 세트의 128 프레임을 가진 14 MB 연구를 기준으로 측정했습니다:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>읽기 전략</th>
<th>열기 시간</th>
<th>할당된 메모리</th>
</tr>
</thead>
<tbody>
<tr><td>전체, 기본</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>대형 요소 건너뛰기</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>대형 요소 연기</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>파일이 커질수록 차이가 커집니다. 10,000개의 연구가 있는 폴더는 이것이 더 이상 미세 최적화가 아닌 경우입니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="메타데이터만 읽고, 픽셀은 그대로 두기">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code>는 크기 임계값을 초과하는 모든 요소를 읽지 않습니다. 반환되는 데이터셋에는 인덱스 또는 라우터에 필요한 태그만 포함됩니다.</p>

<div class="codeblock" id="code">
 <h3>픽셀 데이터 없이 연구 읽기 - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>임계값은 기본적으로 64 kB이며 킬로바이트 단위의 값을 사용합니다. 따라서 8 kB를 대형으로 간주하는 워크플로는 그렇게 지정할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="건너뛰는 대신 연기하기">}}

<p>픽셀이 필요할 수 있지만, 보통 나중에 필요하거나 전체가 아닐 경우 <code>ReadLargeOnDemand</code>가 쌍의 다른 절반을 담당합니다. 파일을 여는 비용은 건너뛰는 것과 동일하며, 코드가 해당 요소에 접근하는 순간 대형 요소가 읽힙니다.</p>

<div class="codeblock" id="code">
 <h3>프레임이 사용될 때만 로드하기 - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>연기된 읽기는 라이선스 기능이며, 다른 전략도 평가판에서 작동합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="픽셀에 접근하지 않고 폴더 인덱싱하기">}}

<p>동일한 전략은 스트림에도 적용되며, 이는 코드에서 아카이브 스캔 또는 클라우드 객체 저장소와 동일한 형태입니다.</p>

<div class="codeblock" id="code">
 <h3>아카이브 스캔 - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="스트림 및 파이프, 입출력">}}

<p>읽기와 쓰기 모두 스트림을 지원하며, 비동기 진입점도 <code>System.IO.Pipelines</code> 타입을 지원합니다. 연구는 네트워크 응답에서 저장소로 이동하면서 프로세스가 전체 파일을 하나의 배열로 보관하지 않아도 됩니다.</p>

<div class="codeblock" id="code">
 <h3>스트림을 통한 읽기 및 쓰기 - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>같은 개념이 텍스트 표현에도 적용됩니다: 많은 데이터셋이 포함된 문서는 <a href="/medical/net/json-to-dicom/">JSON to DICOM</a> 및 <a href="/medical/net/xml-to-dicom/">XML to DICOM</a> 페이지에서 한 번에 하나씩 읽습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="프레임별로">}}

<p>멀티프레임 데이터는 프레임별로 접근되므로, 500프레임 시리즈는 전체 픽셀 데이터 요소가 아니라 한 번에 하나의 프레임씩 처리됩니다.</p>

<div class="codeblock" id="code">
 <h3>프레임 순회 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="디자인을 결정하는 곳">}}

<ul>
<li>아카이브 인덱싱 및 마이그레이션: 수백만 개 파일이 있으며, 이동될 때까지는 헤더만 중요합니다.</li>
<li>라우터 및 저장소 노드: 연구를 받아 필요한 부분만 읽어 라우팅하고, 바이트를 전달합니다.</li>
<li>AI 파이프라인: 메타데이터에서 매니페스트를 구축하고, 실제 학습에 사용되는 하위 집합의 프레임을 가져옵니다.</li>
<li>메모리 제한이 있는 컨테이너: 작업 집합은 파일 크기가 아니라 전략을 따릅니다.</li>
<li>전체 슬라이드 및 OCT 데이터: 전체를 읽는 것이 전혀 옵션이 아닌 파일.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">메모리 관리 가이드</a>에서는 전략을 자세히 설명하고, <a href="/medical/net/dicom-networking/">DICOM 네트워킹</a>에서는 동일한 데이터가 DIMSE를 통해 전송되는 모습을 보여줍니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="학습 자료" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="문서" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="개발자 가이드" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
{{< blocks/products/pf/slr-element name="API 참조" href="https://reference.aspose.com/medical/net/" >}}
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
