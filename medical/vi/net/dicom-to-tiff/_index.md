---
title: Chuyển đổi DICOM sang TIFF trong C# .NET | Aspose.Medical
weight: 12000
description: Kết xuất hình ảnh DICOM thành dữ liệu pixel trong C# .NET và lưu dưới dạng TIFF với hỗ trợ đa trang cho các tệp DICOM đa khung. Sử dụng toàn bộ pipeline LUT của DICOM với Aspose.Medical API và lưu thành TIFF bằng bất kỳ thư viện xử lý ảnh .NET nào.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Chuyển đổi DICOM sang TIFF trong .NET C#" h2="Kết xuất hình ảnh DICOM thành dữ liệu pixel thô với hỗ trợ đầy đủ pipeline xám. Bảo tồn chất lượng hình ảnh bằng điều khiển window/level, kết xuất overlay và xử lý đa khung. Lưu thành TIFF &mdash; bao gồm TIFF đa trang &mdash; bằng bất kỳ thư viện xử lý ảnh .NET nào." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Chuyển đổi DICOM sang TIFF">}}

<p><strong>Aspose.Medical for .NET</strong> kết xuất hình ảnh DICOM thông qua pipeline xám tiêu chuẩn của DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT) để tạo dữ liệu pixel BGRA32 được window đúng cách. TIFF là định dạng đa năng, được sử dụng rộng rãi trong hình ảnh y tế, nghiên cứu khoa học và xuất bản vì nó hỗ trợ nén không mất dữ liệu, độ sâu bit cao, và &mdash; quan trọng nhất &mdash; <strong>tài liệu đa trang</strong>. Điều này khiến TIFF trở thành lựa chọn tự nhiên để chuyển đổi các tệp DICOM đa khung (CT, MRI, siêu âm) thành một tệp đầu ra duy nhất, bảo tồn tất cả các lát cắt.</p>

<p>Kết quả được kết xuất là một <code>IRawImage</code> &mdash; bộ đệm pixel BGRA32 với <code>Width</code>, <code>Height</code>, và mảng <code>Pixels</code>. Bạn có thể lưu dữ liệu pixel này thành TIFF bằng bất kỳ thư viện xử lý ảnh .NET nào như <code>System.Drawing</code>, <code>SkiaSharp</code>, hoặc <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kết xuất DICOM thành Dữ liệu Pixel trong C#">}}

<p>Tải một tệp DICOM, kết xuất khung mong muốn, và truy cập dữ liệu pixel:</p>

<div class="codeblock" id="code">
 <h3>Kết xuất ảnh DICOM - C#</h3>
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

<p>Phương thức <code>RenderImage</code> tự động áp dụng các giá trị window/level và các tham số LUT được lưu trong bộ dữ liệu DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lưu ảnh đã kết xuất dưới dạng TIFF">}}

<p><code>IRawImage</code> cung cấp dữ liệu pixel BGRA32 thô có thể được lưu dưới dạng TIFF bằng bất kỳ thư viện xử lý ảnh .NET nào. Dưới đây là một ví dụ sử dụng <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Lưu khung DICOM đã kết xuất dưới dạng TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF đa trang từ DICOM đa khung">}}

<p>Các tệp DICOM đa khung (ví dụ, chuỗi CT hoặc MRI) chứa nhiều lát cắt ảnh trong một tệp duy nhất. Khả năng đa trang của TIFF cho phép bạn lưu tất cả các khung trong một tệp đầu ra, bảo tồn toàn bộ nghiên cứu trong định dạng có thể đọc được rộng rãi. Sử dụng bộ mã hoá TIFF của <code>System.Drawing</code> với các tham số đa khung:</p>

<div class="codeblock" id="code">
 <h3>Chuyển đổi DICOM đa khung thành TIFF đa trang - C#</h3>
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

<p>Bạn cũng có thể lưu mỗi khung dưới dạng tệp TIFF riêng khi cần các lát cắt riêng lẻ:</p>

<div class="codeblock" id="code">
 <h3>Chuyển đổi mỗi khung thành TIFF riêng - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Window/Level tùy chỉnh">}}

<p>Kiểm soát dải giá trị pixel được kết xuất bằng cách đặt Window Center và Window Width. Các thiết lập window khác nhau sẽ hiển thị các cấu trúc giải phẫu khác nhau từ cùng một dữ liệu DICOM:</p>

<div class="codeblock" id="code">
 <h3>Kết xuất với window/level tùy chỉnh - C#</h3>
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

<p>Lớp <code>GrayscaleRenderOptions</code> cũng cung cấp các thuộc tính <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> và <code>BitDepth</code> để kiểm soát đầy đủ pipeline kết xuất.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kết xuất DICOM đa khung">}}

<p>Các tệp DICOM từ CT, MRI và siêu âm thường chứa nhiều khung (lát cắt). Kết xuất tất cả các khung thành dữ liệu pixel:</p>

<div class="codeblock" id="code">
 <h3>Kết xuất tất cả các khung từ DICOM đa khung - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Kiểm soát Overlay">}}

<p>Ảnh DICOM có thể chứa các mặt phẳng overlay với chú thích, đo lường hoặc vùng quan tâm. Kiểm soát hiển thị và màu sắc của overlay khi kết xuất:</p>

<div class="codeblock" id="code">
 <h3>Kết xuất có và không có overlay - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG cho Hình ảnh Y tế">}}

<p>Mỗi định dạng đầu ra phục vụ các trường hợp sử dụng khác nhau:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Đặc điểm</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Nén</strong></td><td>Không mất dữ liệu (LZW, ZIP) hoặc không nén</td><td>Không mất dữ liệu</td><td>Mất dữ liệu</td></tr>
<tr><td><strong>Đa trang</strong></td><td>Hỗ trợ &mdash; tất cả khung trong một tệp</td><td>Không hỗ trợ</td><td>Không hỗ trợ</td></tr>
<tr><td><strong>Chất lượng hình ảnh</strong></td><td>Pixel-perfect</td><td>Pixel-perfect</td><td>Có thể xuất hiện hiện tượng nén</td></tr>
<tr><td><strong>Kích thước tệp</strong></td><td>Lớn nhất</td><td>Trung bình</td><td>Nhỏ nhất</td></tr>
<tr><td><strong>Độ trong suốt</strong></td><td>Hỗ trợ</td><td>Hỗ trợ</td><td>Không hỗ trợ</td></tr>
<tr><td><strong>Phù hợp nhất cho</strong></td><td>Lưu trữ, chuỗi đa khung, in ấn, nghiên cứu</td><td>Xem chẩn đoán, web với chất lượng</td><td>Chia sẻ web, ảnh thu nhỏ</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET sử dụng cùng một pipeline kết xuất cho mọi đích xuất ra. Dữ liệu pixel <code>IRawImage</code> là giống nhau bất kể định dạng đích &mdash; lựa chọn định dạng chỉ ảnh hưởng đến bước lưu cuối cùng do thư viện xử lý ảnh của bạn thực hiện. TIFF là định dạng ưu tiên khi bạn cần bảo tồn toàn bộ nghiên cứu DICOM đa khung trong một tệp duy nhất hoặc khi đầu ra hướng tới lưu trữ, in ấn, hoặc xử lý ảnh tiếp theo.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Mã nguồn" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Tham chiếu API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Hỗ trợ sản phẩm" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Hỗ trợ miễn phí" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Hỗ trợ trả phí" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Tại sao chọn Aspose.Medical cho .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Danh sách khách hàng" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Câu chuyện thành công" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
