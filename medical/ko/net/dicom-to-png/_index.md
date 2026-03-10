---
title: C# .NET에서 DICOM을 PNG로 변환 | Aspose.Medical
weight: 11000
description: C# .NET에서 손실 없는 품질, 윈도우/레벨 제어 및 다중 프레임 지원으로 DICOM 이미지를 픽셀 데이터로 렌더링합니다. Aspose.Medical API와 전체 DICOM LUT 파이프라인을 사용하여 PNG로 저장합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 DICOM을 PNG로 변환" h2="전체 그레이스케일 파이프라인 지원으로 DICOM 이미지를 원시 픽셀 데이터로 렌더링합니다. 윈도우/레벨 제어, 오버레이 렌더링 및 다중 프레임 처리를 통해 이미지 품질을 유지합니다. 임의의 .NET 이미지 라이브러리를 사용하여 손실 없는 PNG로 저장합니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM to PNG 변환">}}

<p><strong>Aspose.Medical for .NET</strong>은 표준 DICOM 그레이스케일 파이프라인(Modality LUT, VOI LUT, Presentation LUT, Output LUT)을 통해 DICOM 이미지를 렌더링하여 올바르게 윈도우가 적용된 BGRA32 픽셀 데이터를 생성합니다. PNG는 손실 없는 포맷으로, 압축 아티팩트 없이 이미지 품질을 유지해야 하는 경우에 선호되는 선택이며, 진단 검토, 아카이브 및 연구 용도에 필수적입니다.</p>

<p>렌더링된 출력은 <code>IRawImage</code> &mdash; <code>Width</code>, <code>Height</code>, <code>Pixels</code> 배열을 가진 BGRA32 픽셀 버퍼입니다. <code>System.Drawing</code>, <code>SkiaSharp</code>, <code>ImageSharp</code>와 같은 임의의 .NET 이미지 라이브러리를 사용하여 이 픽셀 데이터를 PNG로 저장할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#에서 DICOM을 픽셀 데이터로 렌더링">}}

<p>DICOM 파일을 로드하고 원하는 프레임을 렌더링한 후 픽셀 데이터에 접근합니다:</p>

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

<p><code>RenderImage</code> 메서드는 DICOM 데이터셋에 저장된 윈도우/레벨 값과 LUT 매개변수를 자동으로 적용합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="렌더링된 이미지를 PNG로 저장">}}

<p><code>IRawImage</code>는 임의의 .NET 이미지 라이브러리를 사용하여 손실 없는 PNG로 저장할 수 있는 원시 BGRA32 픽셀 데이터를 제공합니다. 아래는 <code>System.Drawing</code>을 사용한 예시입니다:</p>

<div class="codeblock" id="code">
 <h3>렌더링된 DICOM 프레임을 PNG로 저장 - C#</h3>
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

// Save as lossless PNG
bitmap.Save("ct_scan.png", ImageFormat.Png);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="맞춤형 윈도우/레벨">}}

<p>Window Center와 Window Width를 설정하여 렌더링되는 픽셀 값 범위를 제어합니다. 동일한 DICOM 데이터에서 다른 윈도우 설정은 서로 다른 해부학적 구조를 드러냅니다.</p>

<div class="codeblock" id="code">
 <h3>맞춤형 윈도우/레벨로 렌더링 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);</code></pre>
</div>

<p><code>GrayscaleRenderOptions</code> 클래스는 <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code>, <code>BitDepth</code> 속성을 제공하여 렌더링 파이프라인을 완전히 제어할 수 있게 합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="다중 프레임 DICOM 렌더링">}}

<p>CT, MRI, 초음파 등의 DICOM 파일에는 종종 여러 프레임(슬라이스)이 포함됩니다. 모든 프레임을 픽셀 데이터로 렌더링합니다:</p>

<div class="codeblock" id="code">
 <h3>다중 프레임 DICOM의 모든 프레임 렌더링 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as PNG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="오버레이 제어">}}

<p>DICOM 이미지에는 주석, 측정값 또는 관심 영역을 포함하는 오버레이 평면이 존재할 수 있습니다. 렌더링 시 오버레이 가시성 및 색상을 제어합니다.</p>

<div class="codeblock" id="code">
 <h3>오버레이 포함 및 제외 렌더링 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible (default)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions { ShowOverlays = false };
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="의료 이미지의 PNG와 JPG 비교">}}

<p>PNG와 JPG 중 선택은 사용 사례에 따라 달라집니다:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>특징</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>압축</strong></td><td>무손실</td><td>손실</td></tr>
<tr><td><strong>이미지 품질</strong></td><td>픽셀-완벽 &mdash; 아티팩트 없음</td><td>소량의 압축 아티팩트 발생 가능</td></tr>
<tr><td><strong>파일 크기</strong></td><td>크다 (일반적으로 JPG 대비 2&ndash;10배)</td><td>작다</td></tr>
<tr><td><strong>투명도</strong></td><td>지원됨 (알파 채널)</td><td>지원되지 않음</td></tr>
<tr><td><strong>적합 용도</strong></td><td>진단 검토, 아카이브, 연구, 오버레이</td><td>웹 공유, 썸네일, 미리보기</td></tr>
<tr><td><strong>규제</strong></td><td>품질이 중요한 워크플로에 권장</td><td>비진단 용도에 허용 가능</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET은 두 출력 대상 모두에 동일한 렌더링 파이프라인을 사용합니다. <code>IRawImage</code> 픽셀 데이터는 대상 포맷에 관계없이 동일하며, 포맷 선택은 최종 저장 단계가 이미지 라이브러리에 의해 수행되는 방식에만 영향을 줍니다.</p>

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
