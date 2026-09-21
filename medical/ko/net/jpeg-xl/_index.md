---
title: C# .NET용 DICOM용 JPEG XL | Aspose.Medical
weight: 10500

description: C#에서 DICOM 이미지를 JPEG XL로 저장합니다. 비트 단위로 픽셀을 그대로 반환하는 무손실 JPEG XL이며, 네이티브 코덱 없이 단일 관리 어셈블리만으로 배포됩니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#용 DICOM JPEG XL" h2="DICOM 표준에서 최신 압축 방식으로, 측정한 가장 작은 무손실 파일을 제공합니다. 관리되는 C#으로 구현되어 하나의 어셈블리 안에 포함됩니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL이 DICOM에 채택된 이유">}}

<p>의료 아카이브는 계속 증가하고 절감되지 않습니다. JPEG XL은 20년 이상의 JPEG 및 JPEG 2000 경험을 바탕으로 영상 분야가 설계한 코덱이며, DICOM은 저장팀이 중요하게 여기는 이유, 즉 동일한 픽셀에 대해 파일 크기가 더 작다는 이유로 이를 전송 구문으로 추가했습니다.</p>

<p><strong>Aspose.Medical for .NET</strong>은 라이브러리 내부에 포함된 libjxl의 C# 포트를 통해 JPEG XL을 읽고 씁니다. 패키지는 하나의 어셈블리 <code>Aspose.Medical.dll</code>만을 제공하며, 그 외에 네이티브 바이너리가 없습니다. 따라서 이렇게 새로운 코덱이라도 배포 프로젝트가 되지 않으며, 동일한 어셈블리가 Windows, Linux, 빌드 에이전트 및 컨테이너에서 모두 실행됩니다.</p>

<p>픽셀을 전달하는 두 가지 전송 구문:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), 변형 없이 그대로 복원되어야 하는 진단 데이터용.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), 파일 크기가 정확한 복사보다 더 중요할 때 사용합니다.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="연구(Study)를 압축하면서 모든 픽셀을 보존">}}

<p>트랜스코딩은 한 번의 호출로 이루어지며, 픽셀 주변의 데이터셋도 함께 전송됩니다.</p>

<div class="codeblock" id="code">
 <h3>DICOM 파일을 JPEG XL로 트랜스코딩 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>우리는 자체 테스트 세트의 1714×1933 16비트 이미지로 측정했습니다: 압축 전 6.3 MB가 JPEG XL 무손실로 2.7 MB가 되어 HTJ2K 무손실 이미지보다 작습니다. 실제 수치는 모달리티에 따라 다르므로, 선택하기 전에 파일 폴더 전체에 대해 비교해 보시기 바랍니다.</p>

<p>‘무손실’이라는 표현을 문자 그대로 받아들여야 합니다. JPEG XL로 트랜스코딩했다가 다시 되돌려도 픽셀 데이터가 처음 시작한 바이트와 동일하므로, 진단 품질에 대한 논의 없이 아카이브를 재압축할 수 있습니다.</p>

<div class="codeblock" id="code">
 <h3>비압축 구문으로 복귀 - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="이미 JPEG XL로 저장된 데이터를 읽기">}}

<p>JPEG XL 형식의 파일은 다른 파일과 동일하게 열립니다. 전송 구문이 파일 유형을 지정하며, 프레임이 디코딩되면 픽셀 데이터에 접근할 수 있습니다.</p>

<div class="codeblock" id="code">
 <h3>JPEG XL 파일 열기 - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL 또는 HTJ2K">}}

<p>두 코덱 모두 최신이며, 무손실 옵션을 선택하면 무손실 압축을 제공합니다. 라이브러리는 두 코덱을 모두 읽고 쓸 수 있으며, 각각 다른 요구에 대응합니다.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>질문</th>
<th>답변</th>
</tr>
</thead>
<tbody>
<tr><td>우리 테스트에서 더 작은 파일을 만든 것은 어느 것인가</td><td>JPEG XL 무손실, 몇 퍼센트 정도 작음</td></tr>
<tr><td>네트워크를 통한 점진적 보기용으로 설계된 것은?</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, 특히 RPCL 변형</td></tr>
<tr><td>DICOM 표준에 먼저 도입된 것은?</td><td>HTJ2K, 따라서 현재 더 많은 아카이브에서 지원됨</td></tr>
<tr><td>여기서 네이티브 의존성을 요구하는 것은?</td><td>없음, 두 코덱 모두 하나의 어셈블리 안에 포함된 관리 코드</td></tr>
</tbody>
</table>

<p>선택은 보통 아카이브가 수용하는 전송 구문으로 트랜스코딩하고, 나머지 파이프라인은 동일하게 유지하는 경우가 많습니다.</p>

<div class="codeblock" id="code">
 <h3>대상 아카이브에 결정하도록 하세요 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="가치가 발생하는 경우">}}

<ul>
<li>장기 보관: 동일한 연구를 훨씬 적은 테라바이트로 저장하며, 방사선 전문의에게 품질 저하를 입증할 필요가 없습니다.</li>
<li>클라우드 스토리지 비용: 절감 효과가 매월 반복되는 반면, 트랜스코딩은 한 번만 수행됩니다.</li>
<li>연구 및 AI용 데이터셋: 작은 복사본이 스토리지와 학습 환경 사이를 빠르게 이동합니다.</li>
<li>배포: 이렇게 새로운 코덱은 보통 플랫폼별 네이티브 빌드를 필요로 하지만, 여기서는 이미 참조하는 어셈블리의 일부로 제공됩니다.</li>
</ul>

<p>이 라이브러리는 기존 아카이브에서 사용되는 JPEG, JPEG‑LS, JPEG 2000, HTJ2K 및 RLE 코덱도 기록합니다. 전체 코덱 목록은 <a href="/medical/net/dicom-transfer-syntax-conversion/">전송 구문 변환</a> 페이지에서 확인할 수 있으며, <a href="/medical/net/htj2k/">HTJ2K</a> 전용 페이지와 <a href="/medical/net/jpeg2000/">JPEG 2000</a> 페이지에서 각각 자세히 다룹니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="학습 자료" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="문서" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="개발자 가이드" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="API 참조" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="제품 지원" tabId="support" >}}
{{< blocks/products/pf/slr-element name="무료 지원" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="유료 지원" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="블로그" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="왜 Aspose.Medical for .NET인가?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="고객 목록" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="성공 사례" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
