---
title: C# .NET에서 DICOM 전송 구문 변환 | Aspose.Medical
weight: 16000
description: C# .NET에서 전송 구문 간에 DICOM 파일을 트랜스코딩합니다. Aspose.Medical API를 이용해 JPEG, JPEG 2000, JPEG-LS, RLE 및 비압축 형식을 지원합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 DICOM 전송 구문 변환" h2="비압축, JPEG, JPEG 2000, JPEG-LS 및 RLE 전송 구문 간에 DICOM 파일을 트랜스코딩합니다. 네이티브 의존성이 없는 순수 .NET 라이브러리입니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="전송 구문이란?">}}

<p><strong>Transfer Syntax</strong>는 DICOM 데이터가 저장 및 전송을 위해 어떻게 인코딩되는지를 정의합니다. 여기서는 바이트 순서(엔디언), Value Representation이 명시적인지 암시적인지, 그리고 픽셀 데이터에 적용되는 압축 알고리즘이라는 세 가지 핵심 요소를 지정합니다. 모든 DICOM 파일은 파일 메타 정보 헤더에 전송 구문을 선언합니다.</p>

<p>다양한 의료 기기, PACS 서버 및 뷰어 애플리케이션은 서로 다른 전송 구문 집합을 지원합니다. <strong>Aspose.Medical for .NET</strong>은 <code>Transcode</code> 메서드를 제공하여 전송 구문 간 변환을 가능하게 하고, 상호 운용성, 저장 최적화 및 처리 도구와의 호환성을 지원합니다 &mdash; 모두 네이티브 의존성이 없는 순수 .NET 라이브러리에서 구현됩니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#에서 DICOM 파일 트랜스코딩">}}

<p><code>DicomFile.Transcode</code> 메서드는 DICOM 파일을 현재 전송 구문에서 지원되는 대상 구문으로 변환합니다. 이 메서드는 새로운 <code>DicomFile</code> 인스턴스를 반환하며 &mdash; 원본 파일은 변경되지 않습니다:</p>

<div class="codeblock" id="code">
 <h3>기본 DICOM 트랜스코딩 - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p><code>Dataset</code> 수준에서 직접 트랜스코딩할 수도 있습니다:</p>

<div class="codeblock" id="code">
 <h3>Dataset 트랜스코딩 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="지원되는 전송 구문">}}

<p>다음 표에는 표준 DICOM 이미지 데이터 전송 구문 전체와 Aspose.Medical for .NET에서의 현재 지원 상태가 나와 있습니다. 지원되는 모든 코덱은 순수 C#로 구현되었으며 완전히 플랫폼에 독립적입니다.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>전송 구문</th>
<th>UID</th>
<th>유형</th>
<th>상태</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>비압축</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>비압축</td><td>지원됨</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>비압축</td><td>지원됨</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>비압축</td><td>지원됨</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>지원됨</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>손실, 8-bit</td><td>지원됨</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>손실, 12-bit</td><td>지원되지 않음</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>무손실</td><td>지원됨 (8-bit만)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>무손실</td><td>지원됨 (8-bit만)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>무손실</td><td>지원됨</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>거의 무손실</td><td>지원됨</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>무손실</td><td>지원됨 (읽기 8/16-bit, 쓰기 8-bit)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>손실 또는 무손실</td><td>지원됨 (읽기 8/16-bit, 쓰기 8-bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>무손실</td><td>지원됨 (읽기 8/16-bit, 쓰기 8-bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>손실 또는 무손실</td><td>지원됨 (읽기 8/16-bit, 쓰기 8-bit)</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>무손실</td><td>지원됨</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>무손실</td><td>조만간 제공</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>무손실</td><td>조만간 제공</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>손실 또는 무손실</td><td>조만간 제공</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>무손실</td><td>조만간 제공</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>무손실</td><td>조만간 제공</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>손실 또는 무손실</td><td>조만간 제공</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="일반적인 트랜스코딩 시나리오">}}

<p>다양한 작업 흐름에 따라 다른 트랜스코딩 전략이 필요합니다. 가장 일반적인 시나리오는 다음과 같습니다:</p>

<div class="codeblock" id="code">
 <h3>처리용 복호화 - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>아카이브 저장용 압축 - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>네트워크 전송용 압축 - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="전송 구문 속성 조사">}}

<p><code>TransferSyntax</code> 클래스는 인코딩 특성을 설명하는 속성을 노출합니다. 이를 사용하여 파일의 현재 전송 구문을 확인하거나 적절한 대상 구문을 선택할 수 있습니다:</p>

<div class="codeblock" id="code">
 <h3>전송 구문 속성 읽기 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>속성</th>
<th>형식</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>전송 구문의 고유 식별자</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Value Representation이 명시적으로 인코딩되는지 여부</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>바이트 순서가 리틀 엔디언인지 여부</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>픽셀 데이터가 캡슐화(압축)되어 있는지 여부</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>압축 방식이 손실인지 여부</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>구문이 deflate 압축을 사용하는지 여부</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>DICOM 표준에 의해 전송 구문이 폐기되었는지 여부</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>손실 압축 방식의 ISO 표준 식별자</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="손실 대비 무손실 압축">}}

<p>DICOM 파일을 트랜스코딩할 때 손실 압축과 무손실 압축의 차이를 이해하는 것이 중요합니다:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>측면</th>
<th>무손실</th>
<th>손실</th>
</tr>
</thead>
<tbody>
<tr><td>이미지 품질</td><td>픽셀-완전 &mdash; 원본 데이터가 완전히 보존됨</td><td>작은 크기를 위해 일부 데이터가 영구적으로 손실됨</td></tr>
<tr><td>압축 비율</td><td>보통 2:1~3:1</td><td>보통 10:1~30:1 이상</td></tr>
<tr><td>왕복 안전성</td><td>예 &mdash; 복호화 시 동일한 픽셀 확보</td><td>아니오 &mdash; 손실 재인코딩마다 품질이 추가로 저하</td></tr>
<tr><td>사용 사례</td><td>아카이브, 진단, 법적 기록</td><td>예비 검토, 원격의료, 네트워크 전송</td></tr>
<tr><td>지원 코덱</td><td>JPEG 무손실, JPEG-LS, JPEG 2000 무손실, RLE</td><td>JPEG Baseline, JPEG-LS 거의 무손실, JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>중요:</strong> 손실 압축된 파일을 무손실 구문으로 트랜스코딩해도 손실된 데이터는 복원되지 않습니다. 원본 손실 압축으로 인한 품질 저하는 영구적입니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="학습 자료" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="문서" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="소스 코드" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
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
