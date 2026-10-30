---
title: Nén DICOM JPEG 2000 trong C# .NET | Aspose.Medical
weight: 2000
description: Đọc, ghi và chuyển đổi định dạng các tệp DICOM với nén JPEG 2000 trong C# .NET. Hỗ trợ hình ảnh đơn màu và màu từ 1 đến 16 bit, chế độ không mất dữ liệu và mất dữ liệu, cộng thêm HTJ2K với API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Hỗ trợ DICOM JPEG 2000 trong .NET C#" h2="Đọc, ghi và chuyển đổi định dạng các tệp DICOM với nén JPEG 2000. Các chế độ không mất và mất dữ liệu, dữ liệu pixel đơn màu và màu từ 1 đến 16 bit, kèm HTJ2K – tất cả trong .NET thuần." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 trong Chẩn đoán Y tế">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) là tiêu chuẩn nén dựa trên wavelet được sử dụng rộng rãi nhất trong chẩn đoán y tế. Khác với JPEG truyền thống, nó cung cấp cả nén không mất và mất dữ liệu trong một codec duy nhất, giải mã tiến trình cho truy cập vùng quan tâm, và tỷ lệ nén vượt trội &mdash; khiến nó lý tưởng cho việc lưu trữ các nghiên cứu lớn và truyền ảnh qua mạng có băng thông hạn chế.</p>

<p><strong>Aspose.Medical cho .NET</strong> cung cấp một triển khai thuần C# của codec JPEG 2000 không có phụ thuộc gốc. Thư viện có thể đọc, kết xuất và chuyển đổi các tệp DICOM được nén bằng cả hai cú pháp truyền tải JPEG 2000 Phần 1, bao gồm cả không mất và mất dữ liệu.</p>

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
<tr><td>JPEG 2000 Chỉ Không Mất Dữ Liệu</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Không mất dữ liệu</td><td>Đơn màu và màu, 1 đến 16 bit</td><td>Đơn màu, màu bảng màu và RGB, 1 đến 16 bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Mất dữ liệu hoặc không mất dữ liệu</td><td>Đơn màu và màu, 1 đến 16 bit</td><td>Đơn màu và RGB, 1 đến 16 bit</td></tr>
<tr><td>JPEG 2000 Phần 2 Đa Thành phần Chỉ Không Mất Dữ Liệu</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Không mất dữ liệu</td><td>Không hỗ trợ</td><td>Không hỗ trợ</td></tr>
<tr><td>JPEG 2000 Phần 2 Đa Thành phần</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Mất dữ liệu hoặc không mất dữ liệu</td><td>Không hỗ trợ</td><td>Không hỗ trợ</td></tr>
<tr><td>HTJ2K Chỉ Không Mất Dữ Liệu</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Không mất dữ liệu</td><td>Đơn màu và màu</td><td>Đơn màu và màu</td></tr>
<tr><td>HTJ2K với tùy chọn RPCL Chỉ Không Mất Dữ Liệu</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Không mất dữ liệu</td><td>Đơn màu và màu</td><td>Đơn màu và màu</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Mất dữ liệu hoặc không mất dữ liệu</td><td>Đơn màu và màu</td><td>Đơn màu và màu</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dữ liệu pixel từ 1 đến 16 bit">}}

<p>Hình ảnh y tế thường sử dụng 16 bit trên mỗi mẫu để nắm bắt dải động đầy đủ của các mô thức như CT (thường lưu trữ 12-bit trong 16-bit) và MRI. Aspose.Medical xử lý mọi độ sâu từ 1 đến 16 bit cho JPEG 2000, trong các mẫu 8-bit hoặc 16-bit:</p>

<ul>
<li><strong>Đọc (giải nén)</strong>: các tệp đơn màu (CT, MRI, X-ray), tệp màu bảng màu và tệp màu ba thành phần (RGB, YBR_RCT, YBR_ICT, và các nhãn màu khác như YBR_FULL, giải mã dưới dạng mã) với 1 đến 16 bit trên mỗi mẫu, có dấu hoặc không dấu. Các luồng mã màu CMYK, ICC-profile, JP2 palette và sub-sampled bị từ chối với một ngoại lệ rõ ràng thay vì tạo ra hình ảnh sai một cách im lặng.</li>
<li><strong>Ghi (nén)</strong>: các ảnh đơn màu và RGB với 1 đến 16 bit trên mỗi mẫu, có dấu hoặc không dấu, và ảnh màu bảng màu với JPEG 2000 Không Mất Dữ Liệu. Ảnh YBR không được mã hoá trừ khi chúng giải mã thành RGB, như các tệp JPEG YBR mất dữ liệu.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Đọc và kiểm tra DICOM nén JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Chuyển đổi sang JPEG 2000">}}

<p>Sử dụng phương thức <code>Transcode</code> để nén bất kỳ tệp DICOM nào sang JPEG 2000 hoặc để chuyển đổi giữa các chế độ JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Nén DICOM sang JPEG 2000 Không Mất Dữ Liệu - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Nén DICOM sang JPEG 2000 Mất Dữ Liệu - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Giải nén các tệp DICOM JPEG 2000">}}

<p>Giải nén các tệp JPEG 2000 sang cú pháp truyền tải không nén để xử lý, phân tích, hoặc tương thích với các hệ thống không hỗ trợ JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Giải nén JPEG 2000 sang không nén - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Bạn cũng có thể giải nén và chuyển đổi sang các định dạng nén khác trong một bước duy nhất:</p>

<div class="codeblock" id="code">
 <h3>Chuyển đổi giữa các định dạng nén - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kết xuất hình ảnh DICOM JPEG 2000">}}

<p>Các tệp DICOM nén JPEG 2000 có thể được kết xuất thành dữ liệu pixel để hiển thị hoặc xuất, giống như bất kỳ cú pháp truyền tải nào khác:</p>

<div class="codeblock" id="code">
 <h3>Kết xuất khung ảnh nén JPEG 2000 - C#</h3>
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
<tr><td>Chất lượng ảnh</td><td>Pixel-perfect &mdash; giống hệt bản gốc</td><td>Gần tương đồng thị giác, một số dữ liệu bị mất vĩnh viễn</td></tr>
<tr><td>Tỷ lệ nén</td><td>Thường là 2:1 đến 3:1</td><td>Thường là 10:1 đến 30:1 hoặc cao hơn</td></tr>
<tr><td>Thích hợp cho</td><td>Lưu trữ chẩn đoán, hồ sơ pháp lý, đọc chính</td><td>Xem sơ bộ, y tế từ xa, truyền tải qua mạng</td></tr>
<tr><td>An toàn qua vòng lặp</td><td>Có</td><td>Không &mdash; mã hoá lại sẽ làm giảm chất lượng hơn</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 thông suất cao (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) thay thế bộ mã số học chậm của JPEG 2000 bằng một bộ mã khối nhanh hơn. Nó giữ nguyên biến đổi wavelet, thứ tự tiến trình và chất lượng, đồng thời giải mã và mã hoá nhanh hơn nhiều lần. Aspose.Medical triển khai cả ba cú pháp truyền tải DICOM HTJ2K trong .NET thuần, cho ảnh đơn màu và màu, và chuyển đổi giữa HTJ2K và mọi cú pháp khác được hỗ trợ:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; chỉ không mất dữ liệu</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; không mất dữ liệu với thứ tự tiến trình RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; mất dữ liệu hoặc không mất dữ liệu</li>
</ul>

<div class="codeblock" id="code">
 <h3>Chuyển đổi JPEG 2000 sang HTJ2K và ngược lại - C#</h3>
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
