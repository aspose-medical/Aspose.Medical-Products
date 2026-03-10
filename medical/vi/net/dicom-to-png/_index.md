---
title: Chuyển đổi DICOM sang PNG trong C# .NET | Aspose.Medical
weight: 11000
description: Hiển thị hình ảnh DICOM thành dữ liệu pixel trong C# .NET với chất lượng không mất dữ liệu, điều khiển window/level và hỗ trợ đa khung. Sử dụng toàn bộ pipeline LUT DICOM với Aspose.Medical API và lưu dưới dạng PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Chuyển đổi DICOM sang PNG trong .NET C#" h2="Hiển thị hình ảnh DICOM thành dữ liệu pixel thô với hỗ trợ đầy đủ pipeline xám. Bảo toàn chất lượng ảnh bằng điều khiển window/level, hiển thị overlay và xử lý đa khung. Lưu dưới dạng PNG không mất dữ liệu bằng bất kỳ thư viện ảnh .NET nào." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Chuyển đổi DICOM sang PNG">}}

<p><strong>Aspose.Medical for .NET</strong> hiển thị hình ảnh DICOM qua pipeline xám chuẩn của DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT) để tạo dữ liệu pixel BGRA32 được window đúng. PNG là định dạng không mất dữ liệu, làm cho nó trở thành lựa chọn ưu tiên khi cần bảo toàn chất lượng ảnh mà không có hiện tượng nén &mdash; thiết yếu cho việc đánh giá chẩn đoán, lưu trữ và nghiên cứu.</p>

<p>Kết quả hiển thị là một <code>IRawImage</code> &mdash; một bộ đệm pixel BGRA32 với <code>Width</code>, <code>Height</code>, và một mảng <code>Pixels</code>. Bạn có thể lưu dữ liệu pixel này thành PNG bằng bất kỳ thư viện ảnh .NET nào như <code>System.Drawing</code>, <code>SkiaSharp</code>, hoặc <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Hiển thị DICOM thành Dữ liệu Pixel trong C#">}}

<p>Tải một tệp DICOM, hiển thị khung mong muốn, và truy cập dữ liệu pixel:</p>

<div class="codeblock" id="code">
 <h3>Hiển thị hình ảnh DICOM - C#</h3>
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

<p>Phương thức <code>RenderImage</code> tự động áp dụng các giá trị window/level và các tham số LUT được lưu trong dataset DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lưu hình ảnh đã hiển thị dưới dạng PNG">}}

<p><code>IRawImage</code> cung cấp dữ liệu pixel BGRA32 thô có thể được lưu thành PNG không mất dữ liệu bằng bất kỳ thư viện ảnh .NET nào. Dưới đây là một ví dụ sử dụng <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Lưu khung DICOM đã hiển thị dưới dạng PNG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Cửa sổ/Mức Tùy Chỉnh">}}

<p>Kiểm soát phạm vi giá trị pixel được hiển thị bằng cách thiết lập Window Center và Window Width. Các thiết lập cửa sổ khác nhau sẽ hiển thị các cấu trúc giải phẫu khác nhau từ cùng một dữ liệu DICOM:</p>

<div class="codeblock" id="code">
 <h3>Hiển thị với cửa sổ/mức tùy chỉnh - C#</h3>
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

<p>Lớp <code>GrayscaleRenderOptions</code> cũng cung cấp các thuộc tính <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> và <code>BitDepth</code> để kiểm soát toàn bộ pipeline hiển thị.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Hiển thị DICOM đa khung">}}

<p>Các tệp DICOM từ CT, MRI và siêu âm thường chứa nhiều khung (lớp cắt). Hiển thị tất cả các khung thành dữ liệu pixel:</p>

<div class="codeblock" id="code">
 <h3>Hiển thị tất cả các khung từ DICOM đa khung - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Kiểm soát Overlay">}}

<p>Hình ảnh DICOM có thể chứa các lớp overlay với chú thích, đo lường hoặc vùng quan tâm. Kiểm soát hiển thị và màu sắc của overlay khi hiển thị:</p>

<div class="codeblock" id="code">
 <h3>Hiển thị có và không có overlay - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG cho Hình ảnh Y tế">}}

<p>Lựa chọn giữa PNG và JPG phụ thuộc vào trường hợp sử dụng:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Đặc điểm</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Nén</strong></td><td>Lossless</td><td>Lossy</td></tr>
<tr><td><strong>Chất lượng ảnh</strong></td><td>Pixel-perfect &mdash; không có hiện tượng nén</td><td>Có thể xuất hiện hiện tượng nén nhẹ</td></tr>
<tr><td><strong>Kích thước tệp</strong></td><td>Lớn hơn (thường 2&ndash;10x so với JPG)</td><td>Nhỏ hơn</td></tr>
<tr><td><strong>Trong suốt</strong></td><td>Hỗ trợ (kênh alpha)</td><td>Không hỗ trợ</td></tr>
<tr><td><strong>Thích hợp cho</strong></td><td>Đánh giá chẩn đoán, lưu trữ, nghiên cứu, overlay</td><td>Chia sẻ web, thumbnail, preview</td></tr>
<tr><td><strong>Quy định</strong></td><td>Ưu tiên cho quy trình quan trọng về chất lượng</td><td>Chấp nhận được cho mục đích không chuẩn đoán</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET sử dụng cùng một pipeline hiển thị cho cả hai định dạng đầu ra. Dữ liệu pixel của <code>IRawImage</code> là giống hệt bất kể định dạng mục tiêu — việc lựa chọn định dạng chỉ ảnh hưởng đến bước lưu cuối cùng được thực hiện bởi thư viện ảnh của bạn.</p>

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
