---
title: Nén JPEG 2000 DICOM trong C# .NET | Aspose.Medical
weight: 2000
description: Đọc, ghi và chuyển mã tệp DICOM với nén JPEG 2000 trong C# .NET. Hỗ trợ hình ảnh 8-bit và 16-bit, chế độ không mất dữ liệu và mất dữ liệu, dữ liệu đa thành phần với API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Hỗ trợ JPEG 2000 DICOM trong .NET C#" h2="Đọc, ghi và chuyển mã tệp DICOM với nén JPEG 2000. Chế độ không mất và mất dữ liệu, dữ liệu điểm ảnh 8-bit và 16-bit, hình ảnh đa thành phần — tất cả trong .NET thuần." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 trong Hình ảnh Y tế">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) là tiêu chuẩn nén dựa trên wavelet được sử dụng rộng rãi nhất trong hình ảnh y tế. Khác với JPEG truyền thống, nó cung cấp cả nén không mất và nén mất dữ liệu trong một codec duy nhất, giải mã dần để truy cập vùng quan tâm, và tỉ lệ nén ưu việt &mdash; khiến nó lý tưởng cho việc lưu trữ các nghiên cứu lớn và truyền ảnh qua mạng có băng thông hạn chế.</p>

<p><strong>Aspose.Medical for .NET</strong> cung cấp một triển khai C# thuần của codec JPEG 2000 mà không có phụ thuộc gốc. Thư viện có thể đọc, hiển thị và chuyển mã các tệp DICOM đã nén bằng bất kỳ một trong bốn cú pháp truyền tải chuẩn JPEG 2000.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các cú pháp truyền tải JPEG 2000 được hỗ trợ">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Cú pháp truyền tải</th>
<th>UID</th>
<th>Chế độ</th>
<th>Đọc</th>
<th>Ghi</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Không mất</td><td>8-bit và 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Mất dữ liệu hoặc không mất</td><td>8-bit và 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Không mất</td><td>8-bit và 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Mất dữ liệu hoặc không mất</td><td>8-bit và 16-bit</td><td>8-bit</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dữ liệu điểm ảnh 8-bit và 16-bit">}}

<p>Hình ảnh y tế thường sử dụng 16 bit mỗi mẫu để nắm bắt toàn bộ dải động của các mô hình như CT (thường lưu 12-bit trong 16-bit) và MRI. Aspose.Medical xử lý cả hai độ sâu bit cho JPEG 2000:</p>

<ul>
<li><strong>Đọc (giải nén)</strong>: Hỗ trợ đầy đủ cho cả tệp DICOM nén JPEG 2000 8-bit và 16-bit. Thư viện giải mã đúng dữ liệu điểm ảnh bất kể các giá trị Bits Allocated, Bits Stored và High Bit ban đầu.</li>
<li><strong>Ghi (nén)</strong>: Hiện đang hỗ trợ hình ảnh 8-bit. Hỗ trợ ghi 16-bit dự kiến sẽ có trong phiên bản tương lai.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Đọc và kiểm tra DICOM đã nén JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Chuyển mã sang JPEG 2000">}}

<p>Sử dụng phương thức <code>Transcode</code> để nén bất kỳ tệp DICOM nào sang JPEG 2000 hoặc để chuyển đổi giữa các chế độ JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Nén DICOM sang JPEG 2000 Không mất - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Nén DICOM sang JPEG 2000 Mất dữ liệu - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Giải nén các tệp DICOM JPEG 2000">}}

<p>Giải nén các tệp JPEG 2000 sang một cú pháp truyền tải không nén để xử lý, phân tích, hoặc tương thích với các hệ thống không hỗ trợ JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Giải nén JPEG 2000 sang không nén - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Bạn cũng có thể giải nén và chuyển mã sang các định dạng nén khác trong một bước duy nhất:</p>

<div class="codeblock" id="code">
 <h3>Chuyển mã giữa các định dạng nén - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Hiển thị hình ảnh DICOM JPEG 2000">}}

<p>Các tệp DICOM nén JPEG 2000 có thể được hiển thị thành dữ liệu điểm ảnh để hiển thị hoặc xuất, giống như bất kỳ cú pháp truyền tải nào khác:</p>

<div class="codeblock" id="code">
 <h3>Hiển thị khung nén JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Không mất vs Mất dữ liệu">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Khía cạnh</th>
<th>JPEG 2000 Không mất</th>
<th>JPEG 2000 Mất dữ liệu</th>
</tr>
</thead>
<tbody>
<tr><td>Cú pháp truyền tải</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Chất lượng hình ảnh</td><td>Pixel-perfect &mdash; giống hệt bản gốc</td><td>Tương tự về mặt hình ảnh, một số dữ liệu bị mất vĩnh viễn</td></tr>
<tr><td>Tỷ lệ nén</td><td>Thường từ 2:1 đến 3:1</td><td>Thường từ 10:1 đến 30:1 hoặc cao hơn</td></tr>
<tr><td>Thích hợp cho</td><td>Lưu trữ chẩn đoán, hồ sơ pháp lý, đọc chính</td><td>Đánh giá sơ bộ, y tế từ xa, truyền tải qua mạng</td></tr>
<tr><td>An toàn khi vòng lại</td><td>Có</td><td>Không &mdash; mã lại sẽ làm giảm chất lượng hơn</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 Đa Thành phần">}}

<p>JPEG 2000 Part 2 (ISO/IEC 15444-2) mở rộng codec chuẩn với khả năng biến đổi đa thành phần. Điều này được sử dụng cho hình ảnh y tế màu sắc và các mô hình tạo dữ liệu đa kênh. Aspose.Medical hỗ trợ cả hai cú pháp truyền tải Part 2:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; nén không mất với việc giảm tương quan giữa các thành phần để nén tối ưu dữ liệu đa kênh.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; nén mất hoặc không mất với các phép biến đổi đa thành phần.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K) — Sắp ra mắt">}}

<p>HTJ2K (ISO/IEC 15444-15) là một phần mở rộng thế hệ mới của JPEG 2000 được thiết kế để tăng tốc độ mã hóa và giải mã đáng kể trong khi duy trì cùng mức hiệu quả nén. Nó dự kiến sẽ trở thành codec ưu tiên cho quy trình làm việc hình ảnh y tế thời gian thực.</p>

<p>Aspose.Medical sẽ bổ sung hỗ trợ HTJ2K trong một phiên bản tương lai, bao gồm ba cú pháp truyền tải:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; Chỉ không mất</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; Không mất với thứ tự tiến trình RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; Mất dữ liệu hoặc không mất</li>
</ul>

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

{{< blocks/products/pf/slr-tab tabTitle="Tại sao lại chọn Aspose.Medical cho .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Danh sách khách hàng" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Câu chuyện thành công" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
