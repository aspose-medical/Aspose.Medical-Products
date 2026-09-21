---
title: HTJ2K in C# .NET - DICOM용 고처리량 JPEG 2000 | Aspose.Medical
weight: 10000

description: C#에서 고처리량 JPEG 2000을 사용하여 DICOM 이미지를 압축하고 읽습니다. 무손실 HTJ2K, RPCL 변형 및 손실 HTJ2K가 네이티브 코덱 없이 관리되는 .NET으로 구현되었습니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K in .NET C#" h2="DICOM용 고처리량 JPEG 2000: 빠른 아카이브와 클라우드 뷰잉을 위해 표준에 추가된 압축 방식으로, 네이티브 설치 없이 관리되는 C#으로 구현되었습니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="HTJ2K가 바꾸는 것">}}

<p>고처리량 JPEG 2000은 JPEG 2000의 웨이브릿과 이미지 품질을 유지하면서 느리게 만든 부분을 대체합니다. 블록 코더가 새롭게 도입되었고, 디코딩 속도가 10배 정도 빨라졌습니다. 이것이 DICOM 표준이 세 가지 전송 구문에서 이를 채택하고 클라우드 이미지 플랫폼이 이를 채택한 이유입니다.</p>

<p>.NET 팀에게 실제적인 질문은 누가 실제로 해당 파일을 생성할 수 있느냐입니다. 대부분의 라이브러리는 네이티브 OpenJPH 빌드를 통해 HTJ2K에 접근하는데, 이는 플랫폼별 바이너리와 컨테이너 안의 빌드 단계, 보안 검토 시 거론되는 의존성을 의미합니다. <strong>Aspose.Medical for .NET</strong>은 코덱을 관리 코드로 동일 패키지 안에 구현하여 파일을 읽고 쓰므로, HTJ2K는 Windows, Linux 및 컨테이너 환경에서 설치 없이 동일하게 동작합니다.</p>

<p>세 가지 전송 구문을 지원하며, 세 구문 모두 읽기와 쓰기를 지원합니다:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), 고처리량 JPEG 2000 무손실.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), RPCL 진행 순서를 사용한 무손실 변형.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), 고처리량 JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="연구 데이터를 HTJ2K로 압축">}}

<p>한 번의 호출로 파일을 새 구문으로 전환합니다. 데이터셋, 프라이빗 태그 및 파일 메타 정보가 함께 이동합니다.</p>

<div class="codeblock" id="code">
 <h3>DICOM 파일을 HTJ2K로 트랜스코드 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>우리 자체 테스트 세트의 1714×1933 16비트 이미지에 대해, 파일 크기가 6.3 MB에서 2.9 MB로 감소하고 픽셀은 비트 단위로 동일하게 복원됩니다. 숫자는 모달리티와 이미지마다 다르므로, 이미 보유하고 있는 파일에 대해 한 번 루프를 돌려 직접 측정해 보세요.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="무손실은 무손실을 의미합니다">}}

<p>진단 데이터는 거의 정확한 코덱을 용납하지 않습니다. HTJ2K 무손실로 트랜스코드하고 다시 변환하면 픽셀 데이터가 원본 바이트와 동일하게 유지됩니다. 이는 아카이브를 재압축하기 전에 자체 테스트 스위트에서 검증할 수 있는 특성입니다.</p>

<div class="codeblock" id="code">
 <h3>압축되지 않은 구문으로 되돌리기 - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, 네트워크를 통한 뷰잉을 위해 만든 변형">}}

<p>1.2.840.10008.1.2.4.202 구문은 동일한 무손실 코스트림을 RPCL 진행 순서(해상도 → 위치 → 구성 요소 → 레이어)로 저장합니다. 스트림의 시작 부분만 읽는 리더는 저해상도 이미지를 완전하게 얻을 수 있어, 연결을 제어하지 못하는 큰 연구를 열 때 뷰어가 필요로 하는 정보를 제공합니다.</p>

<div class="codeblock" id="code">
 <h3>RPCL 진행 순서로 압축 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="아카이브가 전송하는 내용을 읽기">}}

<p>작업의 나머지 절반은 이미 HTJ2K를 생성하는 시스템으로부터 받는 것을 처리하는 것입니다. 파일을 열어 저장 형식을 확인하고 픽셀 데이터를 다룹니다.</p>

<div class="codeblock" id="code">
 <h3>HTJ2K 파일 읽기 - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>멀티프레임 이미지는 프레임별로 처리되므로, 긴 시리즈는 연구 전체가 아니라 프레임당 메모리를 사용합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTJ2K가 가치가 있는 영역">}}

<ul>
<li>아카이브 마이그레이션: 저장된 연구를 HTJ2K 무손실로 재압축하여 용량을 줄이고 진단 데이터를 온전하게 유지합니다.</li>
<li>클라우드 및 DICOMweb: 디코드 속도가 브라우저 또는 서버 측 뷰어가 대용량 이미지를 즉시 표시하게 합니다.</li>
<li>AI 파이프라인: 학습 데이터는 쓰기보다 읽기가 훨씬 빈번하며, 디코드 시간은 반복되는 비용이 됩니다.</li>
<li>컨테이너 및 서버리스: 코덱이 어셈블리 안에 포함되어 있어 이미지는 네이티브 라이브러리나 컴파일러가 필요하지 않습니다.</li>
</ul>

<p>이 라이브러리에는 표준에 최근 추가된 JPEG XL과 아카이브에 흔히 포함되는 이전 코덱들(JPEG, JPEG‑LS, JPEG 2000 및 RLE)도 포함됩니다. <a href="/medical/net/dicom-transfer-syntax-conversion/">전송 구문 변환</a> 페이지에서 전체 세트를 다루며, <a href="/medical/net/jpeg2000/">JPEG 2000</a> 페이지에서는 HTJ2K가 파생된 코덱을 자세히 설명합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="학습 자료" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="문서" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="개발자 가이드" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="API 참고" href="https://reference.aspose.com/medical/net/" >}}
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
