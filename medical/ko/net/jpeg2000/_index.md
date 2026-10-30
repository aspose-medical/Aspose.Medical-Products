---
title: C# .NET에서 DICOM JPEG 2000 압축 | Aspose.Medical
weight: 2000
description: C# .NET에서 JPEG 2000 압축을 사용해 DICOM 파일을 읽고, 쓰고, 트랜스코드합니다. 1~16비트의 흑백 및 컬러 이미지, 무손실 및 손실 모드, 그리고 Aspose.Medical API를 통한 HTJ2K를 지원합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 DICOM JPEG 2000 지원" h2="JPEG 2000 압축을 사용해 DICOM 파일을 읽고, 쓰고, 트랜스코드합니다. 무손실 및 손실 모드, 1~16비트의 흑백 및 컬러 픽셀 데이터, HTJ2K 포함 - 모두 순수 .NET에서 구현됩니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="의료 영상에서의 JPEG 2000">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444)는 의료 영상에서 가장 널리 사용되는 웨이브렛 기반 압축 표준입니다. 기존 JPEG와 달리 단일 코덱으로 무손실 및 손실 압축을 모두 제공하고, 관심 영역 접근을 위한 점진적 디코딩과 뛰어난 압축 비율을 제공함으로써 대규모 연구를 보관하고 제한된 네트워크를 통해 이미지를 전송하는 데 이상적입니다.</p>

<p><strong>Aspose.Medical for .NET</strong>은 네이티브 종속성이 없는 순수 C# 구현의 JPEG 2000 코덱을 제공합니다. 이 라이브러리는 JPEG 2000 Part 1 전송 구문으로 압축된 DICOM 파일을 읽고, 렌더링하며, 무손실 및 손실 모드 모두를 트랜스코드할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="지원되는 JPEG 2000 전송 구문">}}

<table class="table table-bordered">
<thead>
<tr>
<th>전송 구문</th>
<th>UID</th>
<th>모드</th>
<th>읽기</th>
<th>쓰기</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 무손실 전용</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>무손실</td><td>흑백 및 컬러, 1~16비트</td><td>흑백, 파레트 컬러 및 RGB, 1~16비트</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>손실 또는 무손실</td><td>흑백 및 컬러, 1~16비트</td><td>흑백 및 RGB, 1~16비트</td></tr>
<tr><td>JPEG 2000 Part 2 다중 구성요소 무손실 전용</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>무손실</td><td>지원되지 않음</td><td>지원되지 않음</td></tr>
<tr><td>JPEG 2000 Part 2 다중 구성요소</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>손실 또는 무손실</td><td>지원되지 않음</td><td>지원되지 않음</td></tr>
<tr><td>HTJ2K 무손실 전용</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>무손실</td><td>흑백 및 컬러</td><td>흑백 및 컬러</td></tr>
<tr><td>RPCL 옵션이 포함된 HTJ2K 무손실 전용</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>무손실</td><td>흑백 및 컬러</td><td>흑백 및 컬러</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>손실 또는 무손실</td><td>흑백 및 컬러</td><td>흑백 및 컬러</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="1~16비트 픽셀 데이터">}}

<p>의료 영상은 CT(대개 12비트를 16비트에 저장)와 MRI와 같은 모달리티의 전체 동적 범위를 포착하기 위해 종종 1샘플당 16비트를 사용합니다. Aspose.Medical은 JPEG 2000에 대해 1~16비트 모든 깊이를 8비트 또는 16비트 샘플로 처리합니다.</p>

<ul>
<li><strong>읽기 (디코딩)</strong>: 1~16비트 샘플, 부호형 또는 부호 없이, 흑백 파일(CT, MRI, X‑ray), 파레트 컬러 파일 및 3채널 컬러 파일(RGB, YBR_RCT, YBR_ICT 및 기타 색상 레이블)을 지원합니다. CMYK, ICC‑프로파일, JP2 파레트 및 서브샘플된 컬러 코드 스트림은 명확한 예외를 발생시켜 거부되며, 조용히 잘못된 이미지가 생성되지 않습니다.</li>
<li><strong>쓰기 (압축)</strong>: 1~16비트 샘플, 부호형 또는 부호 없이 흑백 및 RGB 이미지, 그리고 JPEG 2000 무손실을 사용하는 파레트 컬러 이미지를 지원합니다. YBR 이미지는 RGB로 디코딩되지 않는 한 인코딩되지 않으며, 손실 JPEG YBR 파일과 동일합니다.</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 압축 DICOM 읽고 검사하기 - C#</h3>
 <pre><code class="cs">// Open a JPEG 2000 compressed DICOM file (8-bit or 16-bit)
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Check transfer syntax
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax
Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");

// Inspect pixel data bit depth
PixelData pixelData = PixelData.Create(dicomFile.Dataset);
Console.WriteLine($"Bits Allocated: {pixelData.BitsAllocated}");
Console.WriteLine($"Bits Stored: {pixelData.BitsStored}");
Console.WriteLine($"High Bit: {pixelData.HighBit}");
Console.WriteLine($"Samples Per Pixel: {pixelData.SamplesPerPixel}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000으로 트랜스코드">}}

<p><code>Transcode</code> 메서드를 사용하여 임의의 DICOM 파일을 JPEG 2000으로 압축하거나 JPEG 2000 모드 간 변환할 수 있습니다.</p>

<div class="codeblock" id="code">
 <h3>DICOM을 JPEG 2000 무손실로 압축 - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>DICOM을 JPEG 2000 손실로 압축 - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM 파일 압축 해제">}}

<p>JPEG 2000 파일을 비압축 전송 구문으로 압축 해제하여 처리, 분석 또는 JPEG 2000을 지원하지 않는 시스템과의 호환성을 확보합니다.</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000을 비압축 형식으로 압축 해제 - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>단일 단계로 압축 해제와 다른 압축 포맷으로의 트랜스코드를 동시에 수행할 수도 있습니다.</p>

<div class="codeblock" id="code">
 <h3>압축 포맷 간 트랜스코드 - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM 이미지 렌더링">}}

<p>JPEG 2000 압축 DICOM 파일은 다른 전송 구문과 마찬가지로 픽셀 데이터로 렌더링되어 화면에 표시하거나 내보낼 수 있습니다.</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 압축 프레임 렌더링 - C#</h3>
 <pre><code class="cs">// Open JPEG 2000 DICOM file
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Render the first frame — decompression is handled automatically
using PixelImage&lt;Bgra32&gt; image = dicomFile.RenderImage(0);

// Copy the BGRA32 pixel data for display or export
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = new Bgra32[width * height];
image.CopyPixelsTo(pixels);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="무손실 vs 손실 JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>항목</th>
<th>JPEG 2000 무손실</th>
<th>JPEG 2000 손실</th>
</tr>
</thead>
<tbody>
<tr><td>전송 구문</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>이미지 품질</td><td>픽셀 완벽 — 원본과 동일</td><td>시각적으로 유사하지만 일부 데이터가 영구적으로 손실됨</td></tr>
<tr><td>압축 비율</td><td>일반적으로 2:1~3:1</td><td>일반적으로 10:1~30:1 이상</td></tr>
<tr><td>적합 대상</td><td>진단 보관, 법적 기록, 1차 판독</td><td>예비 검토, 원격 의료, 네트워크 전송</td></tr>
<tr><td>왕복 안정성</td><td>예</td><td>아니오 — 재인코딩 시 품질이 추가로 저하됨</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="고처리량 JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15)는 JPEG 2000의 느린 산술 코더를 더 빠른 블록 코더로 교체합니다. 동일한 웨이브렛 변환, 진행 순서 및 품질을 유지하면서 디코딩·인코딩 속도가 수배 빨라집니다. Aspose.Medical은 순수 .NET으로 구현된 세 가지 DICOM HTJ2K 전송 구문을 모두 지원하며, 흑백 및 컬러 이미지에 적용하고 HTJ2K와 다른 모든 지원 구문 간에 트랜스코드할 수 있습니다.</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) — 무손실 전용</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) — RPCL 진행 순서를 사용하는 무손실</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) — 손실 또는 무손실</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000을 HTJ2K로, 다시 HTJ2K를 JPEG 2000으로 트랜스코드 - C#</h3>
 <pre><code class="cs">// Transcode a JPEG 2000 file to HTJ2K, and back to classic JPEG 2000
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile htj2kFile = j2kFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2kFile.Save("htj2k_lossless.dcm");

// HTJ2K with RPCL progression order, lossless
DicomFile rpclFile = j2kFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
rpclFile.Save("htj2k_rpcl.dcm");

// HTJ2K lossy
DicomFile htj2kLossy = j2kFile.Transcode(TransferSyntax.HTJ2K);
htj2kLossy.Save("htj2k_lossy.dcm");

// Any HTJ2K file decodes back to an uncompressed transfer syntax
DicomFile uncompressed = htj2kFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decoded.dcm");</code></pre>
</div>

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

{{< blocks/products/pf/slr-tab tabTitle="왜 .NET용 Aspose.Medical인가?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="고객 목록" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="성공 사례" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
