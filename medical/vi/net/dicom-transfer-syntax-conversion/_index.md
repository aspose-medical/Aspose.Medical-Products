---
title: Chuyển Đổi Cú Pháp Truyền DICOM trong C# .NET | Aspose.Medical
weight: 16000
description: Mã hoá lại (transcode) các tệp DICOM giữa các cú pháp truyền trong C# .NET. Hỗ trợ JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, RLE và các định dạng không nén với API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Chuyển Đổi Cú Pháp Truyền DICOM trong .NET C#" h2="Mã hoá lại (transcode) các tệp DICOM giữa các cú pháp truyền không nén, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS và RLE. Thư viện .NET thuần không phụ thuộc vào mã gốc." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Cú Pháp Truyền Là Gì?">}}

<p>Một <strong>Transfer Syntax</strong> xác định cách dữ liệu DICOM được mã hoá để lưu trữ và truyền tải. Nó chỉ ra ba khía cạnh chính: thứ tự byte (endianness), việc các Value Representations là explicit hay implicit, và thuật toán nén được áp dụng cho dữ liệu pixel. Mỗi tệp DICOM khai báo cú pháp truyền của nó trong tiêu đề File Meta Information.</p>

<p>Các thiết bị y tế, máy chủ PACS và các ứng dụng xem ảnh hỗ trợ các bộ cú pháp truyền khác nhau. <strong>Aspose.Medical for .NET</strong> cung cấp phương thức <code>Transcode</code> để chuyển đổi giữa các cú pháp truyền, cho phép tương thích, tối ưu hoá lưu trữ và tương thích với các công cụ xử lý &mdash; tất cả trong một thư viện .NET thuần không có phụ thuộc native.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Mã hoá lại (Transcode) một Tệp DICOM trong C#">}}

<p>Phương thức <code>DicomFile.Transcode</code> chuyển đổi một tệp DICOM từ cú pháp truyền hiện tại sang bất kỳ cú pháp đích nào được hỗ trợ. Phương thức này trả về một thể hiện <code>DicomFile</code> mới — tệp gốc vẫn không thay đổi:</p>

<div class="codeblock" id="code">
 <h3>Mã hoá lại DICOM Cơ Bản - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Bạn cũng có thể mã hoá lại ở mức <code>Dataset</code> một cách trực tiếp:</p>

<div class="codeblock" id="code">
 <h3>Mã hoá lại một Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các Cú Pháp Truyền Được Hỗ Trợ">}}

<p>Bảng sau liệt kê tất cả các cú pháp truyền dữ liệu hình ảnh DICOM tiêu chuẩn và trạng thái hỗ trợ hiện tại trong Aspose.Medical cho .NET. Tất cả các codec được hỗ trợ được triển khai bằng C# thuần và hoàn toàn độc lập nền tảng.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Cú Pháp Truyền</th>
<th>UID</th>
<th>Loại</th>
<th>Trạng Thái</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Không Nén</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Không Nén</td><td>Được Hỗ Trợ</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Không Nén</td><td>Được Hỗ Trợ</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Không Nén (đã ngừng dùng)</td><td>Được Hỗ Trợ</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Không Nén</td><td>Không được hỗ trợ</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Được Hỗ Trợ</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Lossy, 8-bit</td><td>Được Hỗ Trợ</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Lossy, 12-bit</td><td>Không được hỗ trợ</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Lossless</td><td>Được Hỗ Trợ (chỉ 8-bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Lossless</td><td>Được Hỗ Trợ (chỉ 8-bit)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Lossless</td><td>Được Hỗ Trợ</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Near-lossless</td><td>Được Hỗ Trợ</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>Được Hỗ Trợ (đọc 8-bit màu và 16-bit đơn sắc; ghi 16-bit đơn sắc hoặc 8-bit RGB)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy or lossless</td><td>Được Hỗ Trợ (đọc 8-bit màu và 16-bit đơn sắc; ghi 16-bit đơn sắc hoặc 8-bit RGB)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Không được hỗ trợ</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy or lossless</td><td>Không được hỗ trợ</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Lossless</td><td>Được Hỗ Trợ</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Được Hỗ Trợ</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Được Hỗ Trợ</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy or lossless</td><td>Được Hỗ Trợ</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Lossless</td><td>Được Hỗ Trợ</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Lossless</td><td>Chỉ giải mã (mã hoá cần một luồng nguồn JPEG, không phải dữ liệu pixel)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Lossy or lossless</td><td>Được Hỗ Trợ (chế độ lossy)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các Kịch Bản Mã Hoá Lại Thông Dụng">}}

<p>Các quy trình làm việc khác nhau yêu cầu các chiến lược mã hoá lại khác nhau. Dưới đây là những kịch bản phổ biến nhất:</p>

<div class="codeblock" id="code">
 <h3>Giải nén để xử lý - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Nén để lưu trữ lưu ký - C#</h3>
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
 <h3>Nén để truyền qua mạng - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Sử dụng các codec mới nhất: HTJ2K và JPEG XL - C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kiểm Tra Các Thuộc Tính Cú Pháp Truyền">}}

<p>Lớp <code>TransferSyntax</code> cung cấp các thuộc tính mô tả các đặc điểm mã hoá. Sử dụng chúng để kiểm tra cú pháp truyền hiện tại của tệp hoặc để chọn một cú pháp đích phù hợp:</p>

<div class="codeblock" id="code">
 <h3>Đọc các thuộc tính cú pháp truyền - C#</h3>
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
<th>Thuộc Tính</th>
<th>Kiểu</th>
<th>Mô Tả</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>Định danh duy nhất của cú pháp truyền</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Cho biết các Value Representation có được mã hoá một cách explicit hay không</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Cho biết thứ tự byte là little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Cho biết dữ liệu pixel có được encapsulated (nén) không</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Cho biết phương pháp nén là lossy hay không</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Cho biết cú pháp này có sử dụng nén deflate không</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Cho biết cú pháp truyền đã bị tiêu chuẩn DICOM ngừng sử dụng hay chưa</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>Định danh tiêu chuẩn ISO của phương pháp nén lossy</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nén Lossy và Lossless">}}

<p>Hiểu sự khác biệt giữa nén lossy và lossless là rất quan trọng khi mã hoá lại các tệp DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Khía Cạnh</th>
<th>Lossless</th>
<th>Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Chất lượng hình ảnh</td><td>Pixel-perfect &mdash; dữ liệu gốc được bảo toàn hoàn toàn</td><td>Một số dữ liệu bị mất vĩnh viễn để đạt kích thước nhỏ hơn</td></tr>
<tr><td>Tỷ lệ nén</td><td>Thường là 2:1 đến 3:1</td><td>Thường là 10:1 đến 30:1 hoặc cao hơn</td></tr>
<tr><td>An toàn khi vòng lại</td><td>Có &mdash; giải nén và nhận được pixel giống hệt</td><td>Không &mdash; mỗi lần mã hoá lại lossily sẽ làm chất lượng giảm tiếp</td></tr>
<tr><td>Trường hợp sử dụng</td><td>Lưu trữ, chẩn đoán, hồ sơ pháp lý</td><td>Đánh giá sơ bộ, y tế từ xa, truyền qua mạng</td></tr>
<tr><td>Codec được hỗ trợ</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>Quan trọng:</strong> Mã hoá lại từ một tệp nén lossy sang cú pháp lossless không khôi phục lại dữ liệu đã mất. Sự suy giảm chất lượng từ quá trình nén lossy ban đầu là vĩnh viễn.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài Nguyên Học Tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài Liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Mã Nguồn" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Tham Khảo API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Hỗ Trợ Sản Phẩm" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Hỗ Trợ Miễn Phí" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Hỗ Trợ Trả Phí" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Tại sao chọn Aspose.Medical cho .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Danh Sách Khách Hàng" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Câu Chuyện Thành Công" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
