---
title: C# .NET에서 DICOM을 TIFF로 변환 | Aspose.Medical
weight: 12000
description: C# .NET에서 DICOM 이미지를 픽셀 데이터로 렌더링하고 다중 프레임 DICOM 파일에 대한 다중 페이지 지원으로 TIFF에 저장합니다. Aspose.Medical API를 사용하여 전체 DICOM LUT 파이프라인을 활용하고, 원하는 .NET 이미지 라이브러리를 이용해 TIFF로 저장합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 DICOM을 TIFF로 변환" h2="전체 그레이스케일 파이프라인 지원으로 DICOM 이미지를 원시 픽셀 데이터로 렌더링합니다. 윈도우/레벨 제어, 오버레이 렌더링, 다중 프레임 처리를 통해 이미지 품질을 유지합니다. 원하는 .NET 이미지 라이브러리를 사용해 TIFF(다중 페이지 TIFF 포함)로 저장합니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM에서 TIFF 변환">}}

<p><strong>Aspose.Medical for .NET</strong>은 표준 DICOM 그레이스케일 파이프라인(Modality LUT, VOI LUT, Presentation LUT, Output LUT)을 통해 DICOM 이미지를 렌더링하여 올바르게 윈도우가 적용된 BGRA32 픽셀 데이터를 생성합니다. TIFF는 무손실 압축, 높은 비트 깊이, 그리고 &mdash; 가장 중요한 &mdash; <strong>다중 페이지 문서</strong>를 지원하는 등 의료 영상, 과학 연구 및 출판 분야에서 널리 사용되는 다목적 포맷입니다. 이는 다중 프레임 DICOM 파일(CT, MRI, 초음파)을 모든 슬라이스를 보존하는 단일 출력 파일로 변환하는 데 자연스러운 선택이 됩니다.</p>

<p>렌더링된 출력은 <code>IRawImage</code> &mdash; <code>Width</code>, <code>Height</code> 및 <code>Pixels</code> 배열을 포함하는 BGRA32 픽셀 버퍼입니다. 이 픽셀 데이터를 <code>System.Drawing</code>, <code>SkiaSharp</code>, <code>ImageSharp</code>와 같은 원하는 .NET 이미지 라이브러리를 사용해 TIFF로 저장할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#에서 DICOM을 픽셀 데이터로 렌더링">}}

<p>DICOM 파일을 로드하고, 원하는 프레임을 렌더링한 뒤 픽셀 데이터에 접근합니다:</p>

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

<p><code>RenderImage</code> 메서드는 DICOM 데이터셋에 저장된 윈도우/레벨 값 및 LUT 파라미터를 자동으로 적용합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="렌더링된 이미지를 TIFF로 저장">}}

<p><code>IRawImage</code>는 원시 BGRA32 픽셀 데이터를 제공하며, 이를 원하는 .NET 이미지 라이브러리를 사용해 TIFF로 저장할 수 있습니다. 아래는 <code>System.Drawing</code>을 사용한 예시입니다:</p>

<div class="codeblock" id="code">
 <h3>렌더링된 DICOM 프레임을 TIFF로 저장 - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("xray.dcm");
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

// Save as TIFF
bitmap.Save("xray.tiff", ImageFormat.Tiff);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="다중 프레임 DICOM에서 다중 페이지 TIFF">}}

<p>다중 프레임 DICOM 파일(예: CT 또는 MRI 시리즈)에는 하나의 파일에 여러 이미지 슬라이스가 포함됩니다. TIFF의 다중 페이지 기능을 사용하면 모든 프레임을 하나의 출력 파일에 저장하여 전체 연구를 보편적으로 읽을 수 있는 포맷으로 보존할 수 있습니다. <code>System.Drawing</code> TIFF 인코더를 다중 프레임 매개변수와 함께 사용하십시오:</p>

<div class="codeblock" id="code">
 <h3>다중 프레임 DICOM을 다중 페이지 TIFF로 변환 - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
int totalFrames = dicomFile.NumberOfFrames;

// Get the TIFF codec
var tiffCodec = ImageCodecInfo.GetImageEncoders()
    .First(c =&gt; c.FormatID == ImageFormat.Tiff.Guid);

// Multi-frame parameters
var multiFrameParams = new EncoderParameters(2);
multiFrameParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.MultiFrame);
multiFrameParams.Param[1] = new EncoderParameter(
    Encoder.Compression, (long)EncoderValue.CompressionLZW);

// Parameter for adding subsequent frames
var frameAddParams = new EncoderParameters(1);
frameAddParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.FrameDimensionPage);

// Parameter for flushing the final frame
var flushParams = new EncoderParameters(1);
flushParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.Flush);

Bitmap? tiffBitmap = null;

for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var frameBitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = frameBitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    frameBitmap.UnlockBits(bitmapData);

    if (i == 0)
    {
        // Save the first frame and start multi-page TIFF
        tiffBitmap = (Bitmap)frameBitmap.Clone();
        tiffBitmap.Save("ct_series.tiff", tiffCodec, multiFrameParams);
    }
    else
    {
        // Add subsequent frames
        tiffBitmap!.SaveAdd(frameBitmap, frameAddParams);
    }
}

// Flush and close the multi-page TIFF
tiffBitmap?.SaveAdd(flushParams);
tiffBitmap?.Dispose();</code></pre>
</div>

<p>개별 슬라이스가 필요할 경우 각 프레임을 별도의 TIFF 파일로 저장할 수도 있습니다:</p>

<div class="codeblock" id="code">
 <h3>각 프레임을 별도 TIFF 파일로 변환 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

for (int i = 0; i &lt; dicomFile.NumberOfFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var bitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = bitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    bitmap.UnlockBits(bitmapData);
    bitmap.Save($"frame_{i:D4}.tiff", ImageFormat.Tiff);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="맞춤 윈도우/레벨">}}

<p>Window Center와 Window Width를 설정하여 렌더링되는 픽셀 값 범위를 제어합니다. 서로 다른 윈도우 설정은 동일한 DICOM 데이터에서 다양한 해부학적 구조를 드러냅니다.</p>

<div class="codeblock" id="code">
 <h3>맞춤 윈도우/레벨로 렌더링 - C#</h3>
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
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

<p><code>GrayscaleRenderOptions</code> 클래스는 <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code>, <code>BitDepth</code> 속성도 제공하여 렌더링 파이프라인을 완벽하게 제어할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="다중 프레임 DICOM 렌더링">}}

<p>CT, MRI, 초음파의 DICOM 파일은 종종 여러 프레임(슬라이스)을 포함합니다. 모든 프레임을 픽셀 데이터로 렌더링합니다:</p>

<div class="codeblock" id="code">
 <h3>다중 프레임 DICOM의 모든 프레임 렌더링 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as TIFF using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="오버레이 제어">}}

<p>DICOM 이미지에는 주석, 측정값 또는 관심 영역이 포함된 오버레이 평면이 있을 수 있습니다. 렌더링 시 오버레이 표시 여부와 색상을 제어합니다.</p>

<div class="codeblock" id="code">
 <h3>오버레이 포함/미포함 렌더링 - C#</h3>
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
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="의료 이미지용 TIFF vs PNG vs JPG">}}

<p>각 출력 포맷은 서로 다른 사용 사례에 적합합니다:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>특징</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>압축</strong></td><td>무손실(LZW, ZIP) 또는 비압축</td><td>무손실</td><td>손실</td></tr>
<tr><td><strong>다중 페이지</strong></td><td>지원됨 &mdash; 모든 프레임을 하나의 파일에 포함</td><td>지원되지 않음</td><td>지원되지 않음</td></tr>
<tr><td><strong>이미지 품질</strong></td><td>픽셀 정확</td><td>픽셀 정확</td><td>압축 아티팩트 가능</td></tr>
<tr><td><strong>파일 크기</strong></td><td>가장 큼</td><td>보통</td><td>가장 작음</td></tr>
<tr><td><strong>투명도</strong></td><td>지원됨</td><td>지원됨</td><td>지원되지 않음</td></tr>
<tr><td><strong>적합 용도</strong></td><td>보관, 다중 프레임 시리즈, 인쇄, 연구</td><td>진단 검토, 품질을 유지한 웹</td><td>웹 공유, 썸네일</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET은 모든 출력 대상에 대해 동일한 렌더링 파이프라인을 사용합니다. <code>IRawImage</code> 픽셀 데이터는 대상 포맷에 관계없이 동일하며, 포맷 선택은 이미지 라이브러리에서 수행되는 최종 저장 단계에만 영향을 미칩니다. 전체 다중 프레임 DICOM 연구를 단일 파일에 보존하거나 출력이 보관, 인쇄, 혹은 추가 이미지 처리용인 경우 TIFF가 선호되는 포맷입니다.</p>

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

{{< blocks/products/pf/slr-tab tabTitle="왜 Aspose.Medical for .NET인가?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="고객 목록" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="성공 사례" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
