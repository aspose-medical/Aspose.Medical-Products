---
title: Chuyển DICOM sang JPG trong C# .NET | Aspose.Medical
weight: 7000
description: Kết xuất hình ảnh DICOM thành dữ liệu pixel trong C# .NET với điều khiển window/level, render overlay và hỗ trợ đa khung. Sử dụng toàn bộ pipeline LUT DICOM với Aspose.Medical API và lưu thành JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Chuyển DICOM sang JPG trong .NET C#" h2="Kết xuất hình ảnh DICOM thành dữ liệu pixel thô với hỗ trợ đầy đủ pipeline thang xám bao gồm window/level, Modality LUT, VOI LUT và render overlay. Lưu thành JPG bằng bất kỳ thư viện xử lý ảnh .NET nào." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Pipeline Kết xuất Hình ảnh DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> kết xuất hình ảnh DICOM qua pipeline thang xám tiêu chuẩn DICOM được định nghĩa trong đặc tả DICOM. Quá trình kết xuất áp dụng một chuỗi các Bảng Tra cứu (LUT) để biến đổi các giá trị pixel lưu trữ thô thành hình ảnh có thể hiển thị:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; chuyển đổi các giá trị pixel lưu trữ thành giá trị độc lập với nhà sản xuất bằng cách sử dụng Rescale Slope/Intercept hoặc một LUT Sequence.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; áp dụng window/level (Window Center và Window Width) để chọn phạm vi giá trị hiển thị, hỗ trợ các biến đổi Linear, Linear Exact và Sigmoid.</li>
<li><strong>Presentation LUT</strong> &mdash; ánh xạ các giá trị cuối cùng thành P-Values có thể hiển thị, bao gồm hỗ trợ INVERSE (đảo ngược hình ảnh).</li>
<li><strong>Output LUT</strong> &mdash; chuyển đổi các giá trị thang xám sang RGB để hiển thị, có thể tùy chọn màu hóa bằng các bản đồ màu tùy chỉnh hoặc bảng Palette Color.</li>
</ol>

<p>Kết quả kết xuất là một <code>IRawImage</code> &mdash; bộ đệm pixel BGRA32 (8-bit mỗi kênh) với <code>Width</code>, <code>Height</code>, và một mảng <code>Pixels</code>. Bạn có thể lưu dữ liệu pixel này thành JPG bằng bất kỳ thư viện xử lý ảnh .NET nào như <code>System.Drawing</code>, <code>SkiaSharp</code>, hoặc <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kết xuất DICOM thành Dữ liệu Pixel trong C#">}}

<p>Tải một tệp DICOM và kết xuất một khung thành <code>IRawImage</code> chứa dữ liệu pixel BGRA32:</p>

<div class="codeblock" id="code">
 <h3>Kết xuất hình ảnh DICOM - C#</h3>
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

<p>Phương thức <code>RenderImage</code> tự động áp dụng toàn bộ pipeline thang xám sử dụng các giá trị window/level và các tham số LUT được lưu trong tệp DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lưu hình ảnh đã kết xuất dưới dạng JPG">}}

<p><code>IRawImage</code> cung cấp dữ liệu pixel BGRA32 thô có thể lưu thành JPG bằng bất kỳ thư viện xử lý ảnh .NET nào. Dưới đây là một ví dụ sử dụng <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Lưu khung DICOM đã kết xuất thành JPG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Window/Level Tùy chỉnh (Windowing)">}}

<p>Window/level (còn gọi là windowing hoặc W/L) là tham số quan trọng nhất cho việc kết xuất hình ảnh DICOM. Nó điều khiển phạm vi giá trị pixel nào được ánh xạ vào dải thang xám có thể nhìn thấy. <strong>Window Center</strong> định nghĩa điểm giữa và <strong>Window Width</strong> định nghĩa dải giá trị hiển thị:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; độ tương phản cao, hiển thị ít mức xám hơn (hữu ích cho mô mềm).</li>
<li><strong>Wide window</strong> &mdash; độ tương phản thấp hơn, hiển thị nhiều mức xám hơn (hữu ích cho xương hoặc phổi).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Kết xuất với window/level tùy chỉnh - C#</h3>
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

<p>Các thiết lập trước window CT phổ biến:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Loại Mô</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Mô mềm</td><td>40</td><td>400</td></tr>
<tr><td>Phổi</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Xương</td><td>400</td><td>1800</td></tr>
<tr><td>Não</td><td>40</td><td>80</td></tr>
<tr><td>Gan</td><td>60</td><td>150</td></tr>
<tr><td>Mediastinum</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tùy chọn Kết xuất Thang xám">}}

<p>Lớp <code>GrayscaleRenderOptions</code> cung cấp khả năng kiểm soát đầy đủ pipeline kết xuất thang xám:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Thuộc tính</th>
<th>Mô tả</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Trung tâm của phạm vi giá trị được hiển thị (điểm giữa ngang của VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Độ rộng của phạm vi giá trị được hiển thị (kiểm soát độ tương phản)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Hệ số rescale của Modality LUT &mdash; nhân với các giá trị pixel lưu trữ</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Hệ số intercept của Modality LUT &mdash; cộng sau khi nhân hệ số</td></tr>
<tr><td><code>Invert</code></td><td>Đảo ngược hình ảnh thang xám (áp dụng dạng INVERSE của Presentation LUT)</td></tr>
<tr><td><code>BitDepth</code></td><td>Thông tin độ sâu bit: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Kết xuất thang xám đảo ngược - C#</h3>
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

<p>Sử dụng <code>RenderOptionsFactory.Create(pixelData)</code> để tự động điền các tùy chọn kết xuất từ các thuộc tính dữ liệu pixel của bộ dữ liệu DICOM, bao gồm độ sâu bit, tham số rescale, window/level và cài đặt đảo ngược.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render Overlay">}}

<p>Các hình ảnh DICOM có thể chứa các mặt overlay &mdash; đồ họa hoặc chú thích văn bản được ghi trực tiếp lên hình ảnh. Lớp <code>RenderOptions</code> điều khiển việc có render overlay hay không và màu sắc chúng hiển thị như thế nào:</p>

<div class="codeblock" id="code">
 <h3>Kết xuất với điều khiển overlay - C#</h3>
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

<p>DICOM hỗ trợ hai loại overlay: overlay <strong>Graphics</strong> (chú thích, đo lường) và overlay <strong>Region of Interest</strong> (ROI). Cả hai loại đều được điều khiển bằng cài đặt <code>ShowOverlays</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kết xuất DICOM Đa khung">}}

<p>Nhiều tệp DICOM từ CT, MRI và siêu âm chứa nhiều khung (slice). Bạn có thể kiểm tra số lượng khung và kết xuất mỗi khung riêng lẻ:</p>

<div class="codeblock" id="code">
 <h3>Kết xuất tất cả các khung từ DICOM đa khung - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Các Kiểu Biến đổi VOI LUT">}}

<p>Tiêu chuẩn DICOM định nghĩa một số hàm biến đổi VOI LUT để kiểm soát cách áp dụng ánh xạ window/level. Aspose.Medical for .NET hỗ trợ tất cả các kiểu chuẩn.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Kiểu</th>
<th>Mô tả</th>
<th>Trường hợp sử dụng</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Ánh xạ tuyến tính chuẩn với độ dịch lệch từ trung tâm</td><td>Thông dụng nhất, dùng cho hình ảnh CT/MR chung</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Ánh xạ tuyến tính không có độ dịch lệch từ trung tâm</td><td>Khi cần ánh xạ giá trị pixel chính xác</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>Biến đổi dạng đường cong S cho chuyển đổi độ tương phản mượt hơn</td><td>Hiển thị mô mềm, mammography</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>LUT tùy chỉnh được định nghĩa dưới dạng bảng tra cứu trong bộ dữ liệu DICOM</td><td>Đường cong hiển thị riêng của nhà cung cấp hoặc riêng cho từng modality</td></tr>
</tbody>
</table>

<p>Engine kết xuất tự động chọn kiểu biến đổi VOI LUT phù hợp dựa trên các thuộc tính của bộ dữ liệu DICOM. Khi sử dụng <code>GrayscaleRenderOptions</code> tùy chỉnh, biến đổi Linear được áp dụng mặc định.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các Modality được hỗ trợ">}}

<p>Aspose.Medical for .NET hỗ trợ kết xuất hình ảnh DICOM từ mọi modality chẩn đoán y tế phổ biến:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modality</th>
<th>Mô tả</th>
<th>Định dạng Điển hình</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Chụp cắt lớp vi tính</td><td>Thang xám, đa khung, 12&ndash;16 bit</td></tr>
<tr><td><strong>MR</strong></td><td>Cộng hưởng từ</td><td>Thang xám, đa khung, 12&ndash;16 bit</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Computed / Digital Radiography</td><td>Thang xám, một khung, 10&ndash;16 bit</td></tr>
<tr><td><strong>US</strong></td><td>Siêu âm</td><td>Thang xám hoặc RGB, đa khung</td></tr>
<tr><td><strong>MG</strong></td><td>Mammography</td><td>Thang xám, độ phân giải cao, 12&ndash;16 bit</td></tr>
<tr><td><strong>XA</strong></td><td>X-Ray Angiography</td><td>Thang xám, đa khung</td></tr>
<tr><td><strong>NM</strong></td><td>Nuclear Medicine</td><td>Thang xám, đa khung</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Positron Emission Tomography)</td><td>Thang xám, đa khung</td></tr>
</tbody>
</table>

<p>Cả các cách diễn giải photometric thang xám (MONOCHROME1/MONOCHROME2) và màu (RGB, YBR_FULL, PALETTE COLOR) đều được hỗ trợ. Engine kết xuất tự động xử lý chuyển đổi không gian màu phù hợp.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên Học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Mã nguồn" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Tham chiếu API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Hỗ trợ Sản phẩm" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Hỗ trợ miễn phí" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Hỗ trợ trả phí" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Tại sao chọn Aspose.Medical cho .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Danh sách Khách hàng" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Câu chuyện thành công" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
