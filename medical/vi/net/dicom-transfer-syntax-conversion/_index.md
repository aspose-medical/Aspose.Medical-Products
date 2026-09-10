---
title: Chuyển đổi Transfer Syntax DICOM trong C# .NET | Aspose.Medical
weight: 16000
description: Transcode các tệp DICOM giữa các transfer syntax trong C# .NET. Hỗ trợ JPEG, JPEG 2000, JPEG-LS, RLE và các định dạng không nén với Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Chuyển đổi Transfer Syntax DICOM trong .NET C#" h2="Transcode các tệp DICOM giữa các transfer syntax không nén, JPEG, JPEG 2000, JPEG-LS và RLE. Thư viện .NET thuần không có phụ thuộc native." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Transfer Syntax là gì?">}}

<p>Một <strong>Transfer Syntax</strong> xác định cách dữ liệu DICOM được mã hóa để lưu trữ và truyền tải. Nó quy định ba khía cạnh chính: thứ tự byte (endianness), việc Value Representations là rõ ràng hay ngầm, và thuật toán nén áp dụng cho dữ liệu pixel. Mỗi tệp DICOM khai báo transfer syntax của nó trong phần đầu File Meta Information.</p>

<p>Các thiết bị y tế, máy chủ PACS và ứng dụng xem ảnh hỗ trợ các bộ transfer syntax khác nhau. <strong>Aspose.Medical for .NET</strong> cung cấp phương thức <code>Transcode</code> để chuyển đổi giữa các transfer syntax, cho phép tương tác liên hệ, tối ưu lưu trữ và tương thích với các công cụ xử lý &mdash; tất cả trong một thư viện .NET thuần không có phụ thuộc native.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transcode một tệp DICOM trong C#">}}

<p>Phương thức <code>DicomFile.Transcode</code> chuyển đổi một tệp DICOM từ transfer syntax hiện tại sang bất kỳ transfer syntax đích nào được hỗ trợ. Phương thức trả về một thể hiện <code>DicomFile</code> mới &mdash; tập tin gốc không bị thay đổi:</p>

<div class="codeblock" id="code">
 <h3>Chuyển đổi DICOM cơ bản - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Bạn cũng có thể thực hiện transcode ngay ở mức <code>Dataset</code>:</p>

<div class="codeblock" id="code">
 <h3>Transcode một Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các Transfer Syntax được hỗ trợ">}}

<p>Bảng dưới đây liệt kê tất cả các transfer syntax dữ liệu ảnh DICOM tiêu chuẩn và trạng thái hỗ trợ hiện tại trong Aspose.Medical cho .NET. Tất cả các codec được hỗ trợ được triển khai bằng C# thuần và hoàn toàn độc lập nền tảng.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Loại</th>
<th>Trạng thái</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Không nén</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Không nén</td><td>Được hỗ trợ</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Không nén</td><td>Được hỗ trợ</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Không nén</td><td>Được hỗ trợ</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Được hỗ trợ</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Có mất dữ liệu, 8-bit</td><td>Được hỗ trợ</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Có mất dữ liệu, 12-bit</td><td>Không được hỗ trợ</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Không mất dữ liệu</td><td>Được hỗ trợ (chỉ 8-bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Không mất dữ liệu</td><td>Được hỗ trợ (chỉ 8-bit)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Không mất dữ liệu</td><td>Được hỗ trợ</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Gần không mất dữ liệu</td><td>Được hỗ trợ</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Không mất dữ liệu</td><td>Được hỗ trợ (đọc 8/16-bit, ghi 8-bit)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Có mất dữ liệu hoặc không mất dữ liệu</td><td>Được hỗ trợ (đọc 8/16-bit, ghi 8-bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Không mất dữ liệu</td><td>Được hỗ trợ (đọc 8/16-bit, ghi 8-bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Có mất dữ liệu hoặc không mất dữ liệu</td><td>Được hỗ trợ (đọc 8/16-bit, ghi 8-bit)</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Không mất dữ liệu</td><td>Được hỗ trợ</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Không mất dữ liệu</td><td>Sắp ra mắt</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Không mất dữ liệu</td><td>Sắp ra mắt</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Có mất dữ liệu hoặc không mất dữ liệu</td><td>Sắp ra mắt</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Không mất dữ liệu</td><td>Sắp ra mắt</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Không mất dữ liệu</td><td>Sắp ra mắt</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Có mất dữ liệu hoặc không mất dữ liệu</td><td>Sắp ra mắt</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các kịch bản Transcode thường gặp">}}

<p>Các quy trình làm việc khác nhau yêu cầu các chiến lược transcode khác nhau. Dưới đây là các kịch bản phổ biến nhất:</p>

<div class="codeblock" id="code">
 <h3>Giải nén để xử lý - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Nén để lưu trữ lâu dài - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Nén để truyền tải qua mạng - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kiểm tra các thuộc tính Transfer Syntax">}}

<p>Lớp <code>TransferSyntax</code> cung cấp các thuộc tính mô tả đặc điểm mã hóa. Sử dụng chúng để kiểm tra transfer syntax hiện tại của tệp hoặc chọn một transfer syntax đích phù hợp:</p>

<div class="codeblock" id="code">
 <h3>Đọc các thuộc tính transfer syntax - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>Thuộc tính</th>
<th>Kiểu</th>
<th>Mô tả</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>Định danh duy nhất của transfer syntax</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Cho biết Value Representations được mã hóa rõ ràng hay không</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Cho biết thứ tự byte là little endian hay không</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Cho biết dữ liệu pixel được bao gói (nén) hay không</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Cho biết phương pháp nén có mất dữ liệu hay không</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Cho biết transfer syntax sử dụng nén deflate hay không</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Cho biết transfer syntax đã bị tiêu chuẩn DICOM loại bỏ hay không</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>Định danh theo tiêu chuẩn ISO của phương pháp nén có mất dữ liệu</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nén Lossy so với Lossless">}}

<p>Hiểu sự khác biệt giữa nén lossless và lossy là rất quan trọng khi transcode các tệp DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Khía cạnh</th>
<th>Không mất dữ liệu</th>
<th>Có mất dữ liệu</th>
</tr>
</thead>
<tbody>
<tr><td>Image quality</td><td>Pixel-perfect &mdash; original data fully preserved</td><td>Some data permanently lost to achieve smaller size</td></tr>
<tr><td>Compression ratio</td><td>Typically 2:1 to 3:1</td><td>Typically 10:1 to 30:1 or higher</td></tr>
<tr><td>Round-trip safe</td><td>Yes &mdash; decompress and get identical pixels</td><td>No &mdash; each lossy re-encode further degrades quality</td></tr>
<tr><td>Use cases</td><td>Archival, diagnostics, legal records</td><td>Preliminary review, telemedicine, network transmission</td></tr>
<tr><td>Supported codecs</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>Quan trọng:</strong> Việc transcode một tệp đã được nén lossily sang transfer syntax lossless không khôi phục lại dữ liệu đã mất. Sự suy giảm chất lượng do nén lossily ban đầu là vĩnh viễn.</p>

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
