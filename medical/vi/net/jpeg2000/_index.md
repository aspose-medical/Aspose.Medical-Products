---
title: Nén DICOM JPEG 2000 trong C# .NET | Aspose.Medical
weight: 2000
description: Đọc, ghi và chuyển đổi định dạng (transcode) các tệp DICOM với nén JPEG 2000 trong C# .NET. Hỗ trợ hình ảnh màu 8-bit và đơn sắc 16-bit, các chế độ lossless và lossy, cùng HTJ2K với API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Hỗ trợ DICOM JPEG 2000 trong .NET C#" h2="Đọc, ghi và chuyển đổi định dạng các tệp DICOM với nén JPEG 2000. Các chế độ lossless và lossy, dữ liệu pixel màu 8-bit và đơn sắc 16-bit, bao gồm HTJ2K - tất cả trong .NET thuần." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 trong Ảnh Y tế">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) là tiêu chuẩn nén dựa trên wavelet được sử dụng rộng rãi nhất trong ảnh y tế. Không giống như JPEG truyền thống, nó cung cấp cả nén lossless và lossy trong một codec duy nhất, giải mã tiến trình cho truy cập vùng quan tâm, và tỷ lệ nén vượt trội &mdash; làm cho nó lý tưởng cho việc lưu trữ các nghiên cứu lớn và truyền ảnh qua mạng có băng thông hạn chế.</p>

<p><strong>Aspose.Medical for .NET</strong> cung cấp một triển khai thuần C# của codec JPEG 2000 mà không có phụ thuộc native. Thư viện có thể đọc, render và chuyển đổi định dạng (transcode) các tệp DICOM được nén với bất kỳ một trong bốn syntax truyền tải chuẩn JPEG 2000.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các syntax truyền tải JPEG 2000 được hỗ trợ">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Chế độ</th>
<th>Đọc</th>
<th>Ghi</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 Chỉ Lossless</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>RGB 8-bit, đơn sắc 16-bit</td><td>đơn sắc 16-bit, RGB 8-bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy hoặc lossless</td><td>RGB 8-bit, đơn sắc 16-bit</td><td>đơn sắc 16-bit, RGB 8-bit</td></tr>
<tr><td>JPEG 2000 Phần 2 Multi-component Chỉ Lossless</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Không được hỗ trợ</td><td>Không được hỗ trợ</td></tr>
<tr><td>JPEG 2000 Phần 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy hoặc lossless</td><td>Không được hỗ trợ</td><td>Không được hỗ trợ</td></tr>
<tr><td>HTJ2K Chỉ Lossless</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>đơn sắc và màu</td><td>đơn sắc và màu</td></tr>
<tr><td>HTJ2K với tùy chọn RPCL Chỉ Lossless</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>đơn sắc và màu</td><td>đơn sắc và màu</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy hoặc lossless</td><td>đơn sắc và màu</td><td>đơn sắc và màu</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dữ liệu Pixel 8-bit và 16-bit">}}

<p>Hình ảnh y tế thường sử dụng 16 bit mỗi mẫu để nắm bắt toàn bộ dải động của các mô thức như CT (thường là 12-bit được lưu trong 16-bit) và MRI. Aspose.Medical xử lý cả hai độ sâu bit cho JPEG 2000:</p>

<ul>
<li><strong>Đọc (giải nén)</strong>: các tệp đơn sắc 16-bit (CT, MRI, X-ray) và các tệp màu ba thành phần 8-bit (RGB, YBR_RCT, YBR_ICT). Các luồng mã màu Palette, CMYK, ICC-profile và sub-sampled sẽ bị từ chối với một ngoại lệ rõ ràng thay vì hình ảnh sai một cách âm thầm.</li>
<li><strong>Ghi (nén)</strong>: hình ảnh đơn sắc 16-bit và RGB 8-bit. Mã hoá đơn sắc 8-bit và màu 16-bit không khả dụng; hãy sử dụng HTJ2K hoặc JPEG XL cho những trường hợp đó, cả hai đều chấp nhận đơn sắc và màu ở bất kỳ độ sâu bit nào.</li>
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
 <h3>Nén DICOM sang JPEG 2000 Lossless - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Nén DICOM sang JPEG 2000 Lossy - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Giải nén các tệp DICOM JPEG 2000">}}

<p>Giải nén các tệp JPEG 2000 sang một syntax truyền tải không nén để xử lý, phân tích, hoặc để tương thích với các hệ thống không hỗ trợ JPEG 2000:</p>

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

{{< blocks/products/pf/feature-page-section h2="Render ảnh DICOM JPEG 2000">}}

<p>Các tệp DICOM nén JPEG 2000 có thể được render thành dữ liệu pixel để hiển thị hoặc xuất, giống như bất kỳ syntax truyền tải nào khác:</p>

<div class="codeblock" id="code">
 <h3>Render khung ảnh nén JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Lossless so với Lossy JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Khía cạnh</th>
<th>JPEG 2000 Lossless</th>
<th>JPEG 2000 Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Transfer Syntax</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Chất lượng hình ảnh</td><td>Pixel-perfect &mdash; giống hệt bản gốc</td><td>Trông tương đồng, một số dữ liệu bị mất vĩnh viễn</td></tr>
<tr><td>Tỷ lệ nén</td><td>Thường 2:1 đến 3:1</td><td>Thường 10:1 đến 30:1 hoặc cao hơn</td></tr>
<tr><td>Phù hợp nhất cho</td><td>Lưu trữ chẩn đoán, hồ sơ pháp lý, đọc chính</td><td>Đánh giá sơ bộ, telemedicine, truyền tải qua mạng</td></tr>
<tr><td>An toàn vòng quay</td><td>Có</td><td>Không &mdash; mã hoá lại sẽ làm chất lượng giảm thêm</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 High-Throughput (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) thay thế bộ mã số học chậm của JPEG 2000 bằng một bộ mã khối nhanh hơn. Nó giữ cùng một phép biến đổi wavelet, thứ tự tiến trình và chất lượng, đồng thời giải mã và mã hoá nhanh hơn nhiều lần. Aspose.Medical triển khai cả ba syntax truyền tải DICOM HTJ2K trong .NET thuần, cho hình ảnh đơn sắc và màu, và chuyển đổi giữa HTJ2K và mọi syntax được hỗ trợ khác:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; chỉ lossless</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; lossless với thứ tự tiến trình RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; lossy hoặc lossless</li>
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
