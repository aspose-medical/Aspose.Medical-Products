---
title: C# .NET에서 DICOM JPEG 2000 압축 | Aspose.Medical
weight: 2000
description: C# .NET에서 JPEG 2000 압축을 사용하여 DICOM 파일을 읽고, 쓰고, 트랜스코드합니다. 8비트 컬러 및 16비트 모노크롬 이미지를 지원하며, 무손실 및 손실 모드와 Aspose.Medical API를 통한 HTJ2K를 지원합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 DICOM JPEG 2000 지원" h2="JPEG 2000 압축을 사용하여 DICOM 파일을 읽고, 쓰고, 트랜스코드합니다. 무손실 및 손실 모드, 8비트 컬러 및 16비트 모노크롬 픽셀 데이터, HTJ2K 포함 - 모두 순수 .NET에서 구현됩니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="의료 영상에서 JPEG 2000">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) 은 의료 영상에서 가장 널리 사용되는 웨이브렛 기반 압축 표준입니다. 기존 JPEG과 달리 단일 코덱에서 무손실 및 손실 압축을 모두 제공하고, 영역 관심 접근을 위한 프로그레시브 디코딩 및 우수한 압축률을 제공합니다 &mdash; 대용량 연구를 보관하고 제한된 네트워크를 통한 이미지 전송에 이상적입니다.</p>

<p><strong>Aspose.Medical for .NET</strong> 은 네이티브 종속성이 없는 순수 C# 구현의 JPEG 2000 코덱을 제공합니다. 이 라이브러리는 네 가지 표준 JPEG 2000 전송 구문 중 어느 것이든 압축된 DICOM 파일을 읽고, 렌더링하고, 트랜스코드할 수 있습니다.</p>

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
<tr><td>JPEG 2000 무손실 전용</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>무손실</td><td>8비트 RGB, 16비트 모노크롬</td><td>16비트 모노크롬, 8비트 RGB</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>손실 또는 무손실</td><td>8비트 RGB, 16비트 모노크롬</td><td>16비트 모노크롬, 8비트 RGB</td></tr>
<tr><td>JPEG 2000 Part 2 다중 구성 요소 무손실 전용</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>무손실</td><td>지원되지 않음</td><td>지원되지 않음</td></tr>
<tr><td>JPEG 2000 Part 2 다중 구성 요소</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>손실 또는 무손실</td><td>지원되지 않음</td><td>지원되지 않음</td></tr>
<tr><td>HTJ2K 무손실 전용</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>무손실</td><td>모노크롬 및 컬러</td><td>모노크롬 및 컬러</td></tr>
<tr><td>HTJ2K RPCL 옵션 무손실 전용</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>무손실</td><td>모노크롬 및 컬러</td><td>모노크롬 및 컬러</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>손실 또는 무손실</td><td>모노크롬 및 컬러</td><td>모노크롬 및 컬러</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8비트 및 16비트 픽셀 데이터">}}

<p>의료 영상은 종종 CT(보통 16비트에 저장된 12비트) 및 MRI와 같은 모달리티의 전체 동적 범위를 포착하기 위해 샘플당 16비트를 사용합니다. Aspose.Medical 은 JPEG 2000에 대해 두 비트 깊이를 모두 처리합니다:</p>

<ul>
<li><strong>읽기 (디코딩)</strong>: 16비트 모노크롬 파일(CT, MRI, X-ray) 및 8비트 3채널 컬러 파일(RGB, YBR_RCT, YBR_ICT). 팔레트, CMYK, ICC 프로파일 및 서브샘플된 컬러 코덱 스트림은 조용히 잘못된 이미지가 생성되는 대신 명확한 예외로 거부됩니다.</li>
<li><strong>쓰기 (인코딩)</strong>: 16비트 모노크롬 및 8비트 RGB 이미지. 8비트 모노크롬 및 16비트 컬러 인코딩은 지원되지 않으며, 이러한 경우 HTJ2K 또는 JPEG XL을 사용하십시오. 두 코덱 모두 비트 깊이에 관계없이 모노크롬 및 컬러를 지원합니다.</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 압축 DICOM 읽기 및 검사 - C#</h3>
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

<p><code>Transcode</code> 메서드를 사용하여 임의의 DICOM 파일을 JPEG 2000으로 압축하거나 JPEG 2000 모드 간에 변환합니다:</p>

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

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM 파일 디코딩">}}

<p>JPEG 2000 파일을 비압축 전송 구문으로 디코딩하여 처리, 분석 또는 JPEG 2000을 지원하지 않는 시스템과의 호환성을 확보합니다:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000을 비압축으로 디코딩 - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>단일 단계로 디코딩하고 다른 압축 형식으로 트랜스코드할 수도 있습니다:</p>

<div class="codeblock" id="code">
 <h3>압축 형식 간 트랜스코드 - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM 이미지 렌더링">}}

<p>JPEG 2000으로 압축된 DICOM 파일은 다른 전송 구문과 마찬가지로 픽셀 데이터로 렌더링되어 표시하거나 내보낼 수 있습니다:</p>

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
<tr><td>이미지 품질</td><td>픽셀 완전 일치 &mdash; 원본과 동일</td><td>시각적으로 유사하지만 일부 데이터가 영구적으로 손실됨</td></tr>
<tr><td>압축 비율</td><td>보통 2:1~3:1</td><td>보통 10:1~30:1 또는 그 이상</td></tr>
<tr><td>적합 분야</td><td>진단 보관, 법적 기록, 1차 판독</td><td>예비 검토, 원격 진료, 네트워크 전송</td></tr>
<tr><td>왕복 안전성</td><td>예</td><td>아니오 &mdash; 재인코딩 시 품질이 추가로 저하됨</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="고처리량 JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) 는 JPEG 2000의 느린 산술 코더를 보다 빠른 블록 코더로 대체합니다. 동일한 웨이브렛 변환, 진행 순서 및 품질을 유지하면서 디코딩 및 인코딩 속도가 몇 배 빠릅니다. Aspose.Medical 은 순수 .NET에서 모든 세 가지 DICOM HTJ2K 전송 구문을 구현하며, 모노크롬 및 컬러 이미지를 지원하고 HTJ2K와 다른 모든 지원 구문 간 트랜스코드를 수행합니다:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; 무손실 전용</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; RPCL 진행 순서를 가진 무손실</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; 손실 또는 무손실</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000을 HTJ2K로, 다시 원본으로 트랜스코드 - C#</h3>
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
{{< blocks/products/pf/slr-element name="API 레퍼런스" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="제품 지원" tabId="support" >}}
{{< blocks/products/pf/slr-element name="무료 지원" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="유료 지원" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="블로그" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle=".NET용 Aspose.Medical을 선택해야 하는 이유는?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="고객 리스트" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="성공 사례" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
