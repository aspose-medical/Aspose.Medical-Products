---
title: C# .NET에서 DICOM JPEG 2000 압축 | Aspose.Medical
weight: 2000
description: C# .NET에서 JPEG 2000 압축을 사용하여 DICOM 파일을 읽고, 쓰고, 변환합니다. 8비트 및 16비트 이미지, 손실 없음 및 손실 모드, 다중 컴포넌트 데이터를 Aspose.Medical API와 함께 지원합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 DICOM JPEG 2000 지원" h2="JPEG 2000 압축을 사용하여 DICOM 파일을 읽고, 쓰고, 변환합니다. 손실 없음 및 손실 모드, 8비트 및 16비트 픽셀 데이터, 다중 컴포넌트 이미지 — 모두 순수 .NET에서 제공됩니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="의료 영상에서 JPEG 2000">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444)는 의료 영상에서 가장 널리 사용되는 웨이블 기반 압축 표준입니다. 기존 JPEG와 달리 단일 코덱에서 손실 없음과 손실 압축을 모두 제공하고, 관심 영역 접근을 위한 점진적 디코딩과 우수한 압축 비율을 지원합니다 &mdash; 대용량 연구를 보관하고 제한된 네트워크를 통해 이미지를 전송하는 데 이상적입니다.</p>

<p><strong>Aspose.Medical for .NET</strong>은 네이티브 종속성이 없는 순수 C# 구현의 JPEG 2000 코덱을 제공합니다. 이 라이브러리는 네 가지 표준 JPEG 2000 전송 구문 중 어느 것이든 압축된 DICOM 파일을 읽고, 렌더링하고, 변환할 수 있습니다.</p>

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
<tr><td>JPEG 2000 손실 없음 전용</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>손실 없음</td><td>8비트 및 16비트</td><td>8비트</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>손실 또는 손실 없음</td><td>8비트 및 16비트</td><td>8비트</td></tr>
<tr><td>JPEG 2000 Part 2 다중 컴포넌트 손실 없음 전용</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>손실 없음</td><td>8비트 및 16비트</td><td>8비트</td></tr>
<tr><td>JPEG 2000 Part 2 다중 컴포넌트</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>손실 또는 손실 없음</td><td>8비트 및 16비트</td><td>8비트</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8비트 및 16비트 픽셀 데이터">}}

<p>의료 영상은 종종 CT(보통 16비트에 12비트 저장) 및 MRI와 같은 모달리티의 전체 동적 범위를 포착하기 위해 샘플당 16비트를 사용합니다. Aspose.Medical은 JPEG 2000에 대해 두 비트 깊이를 모두 처리합니다:</p>

<ul>
<li><strong>읽기 (디코딩)</strong>: 8비트 및 16비트 JPEG 2000 압축 DICOM 파일 모두에 대한 완전한 지원. 라이브러리는 원본 Bits Allocated, Bits Stored, High Bit 값에 관계없이 픽셀 데이터를 올바르게 디코딩합니다.</li>
<li><strong>쓰기 (압축)</strong>: 현재 8비트 이미지만 지원합니다. 16비트 쓰기 지원은 향후 릴리스에서 계획되어 있습니다.</li>
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

{{< blocks/products/pf/feature-page-section h2="JPEG 2000으로 변환">}}

<p><code>Transcode</code> 메서드를 사용하여 임의의 DICOM 파일을 JPEG 2000으로 압축하거나 JPEG 2000 모드 간에 변환할 수 있습니다:</p>

<div class="codeblock" id="code">
 <h3>DICOM을 JPEG 2000 손실 없음으로 압축 - C#</h3>
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

<p>JPEG 2000 파일을 비압축 전송 구문으로 압축 해제하여 처리, 분석 또는 JPEG 2000을 지원하지 않는 시스템과의 호환성을 확보합니다:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000을 비압축 형태로 압축 해제 - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>한 번에 압축 해제와 다른 압축 형식으로 변환을 동시에 수행할 수도 있습니다:</p>

<div class="codeblock" id="code">
 <h3>압축 형식 간 변환 - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM 이미지 렌더링">}}

<p>JPEG 2000으로 압축된 DICOM 파일은 다른 전송 구문과 마찬가지로 픽셀 데이터로 렌더링하여 표시하거나 내보낼 수 있습니다:</p>

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

{{< blocks/products/pf/feature-page-section h2="손실 없음 vs 손실 JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>항목</th>
<th>JPEG 2000 손실 없음</th>
<th>JPEG 2000 손실</th>
</tr>
</thead>
<tbody>
<tr><td>전송 구문</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>이미지 품질</td><td>픽셀 완전 일치 &mdash; 원본과 동일</td><td>시각적으로 유사, 일부 데이터가 영구적으로 손실됨</td></tr>
<tr><td>압축 비율</td><td>통상 2:1 ~ 3:1</td><td>통상 10:1 ~ 30:1 이상</td></tr>
<tr><td>가장 적합한 경우</td><td>진단 보관, 법적 기록, 1차 판독</td><td>예비 검토, 원격의료, 네트워크 전송</td></tr>
<tr><td>왕복 안전성</td><td>예</td><td>아니오 &mdash; 재인코딩 시 품질이 더욱 저하됩니다</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 다중 컴포넌트">}}

<p>JPEG 2000 Part 2 (ISO/IEC 15444-2)는 다중 컴포넌트 변환 기능을 추가하여 표준 코덱을 확장합니다. 이는 컬러 의료 영상 및 다채널 데이터를 생성하는 모달리티에 사용됩니다. Aspose.Medical은 Part 2 전송 구문을 모두 지원합니다:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; 다채널 데이터의 최적 압축을 위한 컴포넌트 간 탈상관을 활용한 손실 없음 압축.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; 다중 컴포넌트 변환을 사용한 손실 또는 손실 없음 압축.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="고속 JPEG 2000 (HTJ2K) — 곧 제공">}}

<p>HTJ2K (ISO/IEC 15444-15)는 동일한 압축 효율성을 유지하면서 인코딩 및 디코딩 속도를 획기적으로 빠르게 설계된 차세대 JPEG 2000 확장입니다. 실시간 의료 영상 워크플로에 선호되는 코덱이 될 것으로 기대됩니다.</p>

<p>Aspose.Medical은 향후 릴리스에서 HTJ2K 지원을 추가하여 세 가지 전송 구문을 다룰 예정입니다:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; 손실 없음 전용</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; RPCL 진행 순서를 가진 손실 없음</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; 손실 또는 손실 없음</li>
</ul>

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
