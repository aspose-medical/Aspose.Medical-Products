---
title: C# .NET에서 DICOM을 JPG로 변환 | Aspose.Medical
weight: 7000
description: C# .NET에서 윈도우/레벨 제어, 오버레이 렌더링 및 멀티프레임 지원을 갖춘 DICOM 이미지를 픽셀 데이터로 렌더링합니다. Aspose.Medical API를 사용하여 전체 DICOM LUT 파이프라인을 적용하고 JPG로 저장합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 DICOM을 JPG로 변환" h2="윈도우/레벨, Modality LUT, VOI LUT 및 오버레이 렌더링을 포함한 전체 그레이스케일 파이프라인을 지원하여 DICOM 이미지를 원시 픽셀 데이터로 렌더링합니다. .NET 이미지 라이브러리를 사용하여 JPG로 저장합니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM 이미지 렌더링 파이프라인">}}

<p><strong>Aspose.Medical for .NET</strong>은 DICOM 사양에 정의된 표준 DICOM 그레이스케일 파이프라인을 통해 DICOM 이미지를 렌더링합니다. 렌더링 과정은 Look-Up Tables (LUTs) 체인을 적용하여 원시 저장 픽셀 값을 표시 가능한 이미지로 변환합니다:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; 저장된 픽셀 값을 Rescale Slope/Intercept 또는 LUT Sequence를 사용하여 제조업체 독립적인 값으로 변환합니다.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; 윈도우/레벨 (Window Center 및 Window Width)을 적용하여 표시할 값 범위를 선택하며, Linear, Linear Exact 및 Sigmoid 변환을 지원합니다.</li>
<li><strong>Presentation LUT</strong> &mdash; 최종 값을 표시 가능한 P-Values로 매핑하며, INVERSE(이미지 반전)를 지원합니다.</li>
<li><strong>Output LUT</strong> &mdash; 그레이스케일 값을 디스플레이용 RGB로 변환하며, 사용자 정의 컬러맵이나 Palette Color 테이블을 사용한 색상화도 선택적으로 지원합니다.</li>
</ol>

<p>렌더링된 출력은 <code>IRawImage</code> &mdash; <code>Width</code>, <code>Height</code>, 및 <code>Pixels</code> 배열을 가진 BGRA32 픽셀 버퍼(채널당 8비트)입니다. 그런 다음 <code>System.Drawing</code>, <code>SkiaSharp</code>, <code>ImageSharp</code> 와 같은 .NET 이미지 라이브러리를 사용하여 이 픽셀 데이터를 JPG로 저장할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#에서 DICOM을 픽셀 데이터로 렌더링">}}

<p>DICOM 파일을 로드하고 프레임을 BGRA32 픽셀 데이터를 포함하는 <code>IRawImage</code>로 렌더링합니다:</p>

<div class="codeblock" id="code">
 <h3>DICOM 이미지 렌더링 - C#</h3>
 <pre><code class="cs">using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;

// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Render the first frame (index 0) through the full LUT pipeline
IRawImage image = dicomFile.RenderImage(0);

// Access the rendered pixel data
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = image.Pixels; // BGRA32 pixel buffer</code></pre>
</div>

<p><code>RenderImage</code> 메서드는 DICOM 파일에 저장된 윈도우/레벨 값과 LUT 매개변수를 사용하여 전체 그레이스케일 파이프라인을 자동으로 적용합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="렌더링된 이미지를 JPG로 저장">}}

<p><code>IRawImage</code>는 원시 BGRA32 픽셀 데이터를 제공하며, 이를 .NET 이미지 라이브러리를 사용하여 JPG로 저장할 수 있습니다. 아래는 <code>System.Drawing</code>을 사용한 예시입니다:</p>

<div class="codeblock" id="code">
 <h3>렌더링된 DICOM 프레임을 JPG로 저장 - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");
IRawImage image = dicomFile.RenderImage(0);

// Create a Bitmap from the BGRA32 pixel data
using var bitmap = new Bitmap(image.Width, image.Height, PixelFormat.Format32bppArgb);
var bitmapData = bitmap.LockBits(
    new Rectangle(0, 0, image.Width, image.Height),
    ImageLockMode.WriteOnly,
    PixelFormat.Format32bppArgb);

// Copy BGRA32 pixels to the bitmap
MemoryMarshal.AsBytes(image.Pixels.AsSpan())
    .CopyTo(new Span&lt;byte&gt;(
        (void*)bitmapData.Scan0,
        bitmapData.Stride * bitmapData.Height));

bitmap.UnlockBits(bitmapData);

// Save as JPG
bitmap.Save("ct_scan.jpg", ImageFormat.Jpeg);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="맞춤형 윈도우/레벨 (Windowing)">}}

<p>윈도우/레벨(일명 windowing 또는 W/L)은 DICOM 이미지 렌더링에서 가장 중요한 매개변수이며, 픽셀 값 중 어느 범위를 가시적인 그레이스케일 범위에 매핑할지를 제어합니다. <strong>Window Center</strong>는 중간값을 정의하고, <strong>Window Width</strong>는 표시되는 값 범위를 정의합니다:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; 높은 대비, 표시되는 회색 레벨이 적음(연부 조직에 유용).</li>
<li><strong>Wide window</strong> &mdash; 낮은 대비, 표시되는 회색 레벨이 많음(뼈 또는 폐에 유용).</li>
</ul>

<div class="codeblock" id="code">
 <h3>맞춤형 윈도우/레벨로 렌더링 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window: center=40, width=400
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window: center=400, width=1800
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Lung window: center=-600, width=1500
var lung = new GrayscaleRenderOptions
{
    WindowCenter = -600,
    WindowWidth = 1500
};
IRawImage lungImage = dicomFile.RenderImage(lung, 0);</code></pre>
</div>

<p>일반적인 CT 윈도우 프리셋:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>조직 유형</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>연부 조직</td><td>40</td><td>400</td></tr>
<tr><td>폐</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>뼈</td><td>400</td><td>1800</td></tr>
<tr><td>뇌</td><td>40</td><td>80</td></tr>
<tr><td>간</td><td>60</td><td>150</td></tr>
<tr><td>종격동</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="그레이스케일 렌더링 옵션">}}

<p><code>GrayscaleRenderOptions</code> 클래스는 그레이스케일 렌더링 파이프라인에 대한 전체 제어를 제공합니다:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>속성</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>표시되는 값 범위의 중심(VOI LUT의 수평 중간값)</td></tr>
<tr><td><code>WindowWidth</code></td><td>표시되는 값 범위의 폭(대비를 제어)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Modality LUT 재조정 기울기 &mdash; 저장된 픽셀 값에 곱해짐</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Modality LUT 재조정 절편 &mdash; 기울기 곱셈 후 추가됨</td></tr>
<tr><td><code>Invert</code></td><td>그레이스케일 이미지를 반전시킴(INVERSE Presentation LUT 형태 적용)</td></tr>
<tr><td><code>BitDepth</code></td><td>비트 깊이 정보: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>반전된 그레이스케일 렌더링 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray.dcm");

// Render with inverted grayscale (white-on-black becomes black-on-white)
var options = new GrayscaleRenderOptions
{
    WindowCenter = 2048,
    WindowWidth = 4096,
    Invert = true
};

IRawImage image = dicomFile.RenderImage(options, 0);</code></pre>
</div>

<p><code>RenderOptionsFactory.Create(pixelData)</code>를 사용하여 DICOM 데이터셋의 픽셀 데이터 속성(비트 깊이, 재조정 매개변수, 윈도우/레벨, 반전 설정 등)으로부터 렌더 옵션을 자동으로 채웁니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="오버레이 렌더링">}}

<p>DICOM 이미지에는 오버레이 평면—이미지에 직접 삽입된 그래픽 또는 텍스트 주석—이 포함될 수 있습니다. <code>RenderOptions</code> 클래스는 오버레이를 렌더링할지 여부와 색상을 제어합니다:</p>

<div class="codeblock" id="code">
 <h3>오버레이 제어로 렌더링 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible in white (default behavior)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions
{
    ShowOverlays = false
};
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

<p>DICOM은 두 가지 오버레이 유형을 지원합니다: <strong>Graphics</strong> 오버레이(주석, 측정) 및 <strong>Region of Interest</strong> (ROI) 오버레이. 두 유형 모두 <code>ShowOverlays</code> 설정으로 제어됩니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="멀티프레임 DICOM 렌더링">}}

<p>CT, MRI, 초음파의 많은 DICOM 파일은 다중 프레임(슬라이스)을 포함합니다. 프레임 수를 확인하고 각 프레임을 개별적으로 렌더링할 수 있습니다:</p>

<div class="codeblock" id="code">
 <h3>멀티프레임 DICOM의 모든 프레임 렌더링 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;
Console.WriteLine($"Total frames: {totalFrames}");

// Render all frames to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as JPG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="VOI LUT 변환 유형">}}

<p>DICOM 표준은 윈도우/레벨 매핑 적용 방식을 제어하는 여러 VOI LUT 변환 함수를 정의합니다. Aspose.Medical for .NET은 모든 표준 유형을 지원합니다:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>유형</th>
<th>설명</th>
<th>사용 사례</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>중심에서 오프셋이 있는 표준 선형 매핑</td><td>가장 일반적이며, 일반 CT/MR 영상에 사용</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>중심 오프셋 없이 선형 매핑</td><td>정밀한 픽셀 값 매핑이 필요할 때</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>보다 부드러운 대비 전이를 위한 S-곡선 변환</td><td>연부 조직 시각화, 유방 촬영술</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>DICOM 데이터셋에 lookup table로 정의된 사용자 정의 LUT</td><td>벤더 특화 또는 모달리티 특화 디스플레이 곡선</td></tr>
</tbody>
</table>

<p>렌더링 엔진은 DICOM 데이터셋 속성을 기반으로 적절한 VOI LUT 변환을 자동으로 선택합니다. 사용자 정의 <code>GrayscaleRenderOptions</code>를 사용할 경우 기본적으로 Linear 변환이 적용됩니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="지원되는 모달리티">}}

<p>Aspose.Medical for .NET은 모든 일반적인 의료 영상 모달리티의 DICOM 이미지 렌더링을 지원합니다.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>모달리티</th>
<th>설명</th>
<th>일반 형식</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Computed Tomography</td><td>그레이스케일, 멀티프레임, 12&ndash;16 비트</td></tr>
<tr><td><strong>MR</strong></td><td>Magnetic Resonance</td><td>그레이스케일, 멀티프레임, 12&ndash;16 비트</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Computed / Digital Radiography</td><td>그레이스케일, 단일 프레임, 10&ndash;16 비트</td></tr>
<tr><td><strong>US</strong></td><td>Ultrasound</td><td>그레이스케일 또는 RGB, 멀티프레임</td></tr>
<tr><td><strong>MG</strong></td><td>Mammography</td><td>그레이스케일, 고해상도, 12&ndash;16 비트</td></tr>
<tr><td><strong>XA</strong></td><td>X-Ray Angiography</td><td>그레이스케일, 멀티프레임</td></tr>
<tr><td><strong>NM</strong></td><td>Nuclear Medicine</td><td>그레이스케일, 멀티프레임</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Positron Emission Tomography)</td><td>그레이스케일, 멀티프레임</td></tr>
</tbody>
</table>

<p>그레이스케일(MONOCHROME1/MONOCHROME2) 및 컬러(RGB, YBR_FULL, PALETTE COLOR) 포토메트릭 해석을 모두 지원합니다. 렌더링 엔진은 적절한 색 공간 변환을 자동으로 처리합니다.</p>

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

{{< blocks/products/pf/slr-tab tabTitle=".NET용 Aspose.Medical을 선택해야 하는 이유?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="고객 목록" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="성공 사례" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
