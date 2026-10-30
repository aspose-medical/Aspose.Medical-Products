---
title: Chuyển đổi Transfer Syntax DICOM trong C# .NET | Aspose.Medical
weight: 16000
description: Mã hoá lại DICOM files giữa các transfer syntax trong C# .NET. Hỗ trợ JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, RLE và các định dạng không nén với Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Chuyển đổi Transfer Syntax DICOM trong .NET C#" h2="Mã hoá lại các tệp DICOM giữa các transfer syntax không nén, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS và RLE. Thư viện .NET thuần với không có phụ thuộc native." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Transfer Syntax là gì?">}}

<p>Một <strong>Transfer Syntax</strong> định nghĩa cách dữ liệu DICOM được mã hoá để lưu trữ và truyền tải. Nó chỉ ra ba khía cạnh chính: thứ tự byte (endianness), việc các Value Representation là rõ ràng hay ngầm, và thuật toán nén áp dụng cho dữ liệu píxel. Mỗi tệp DICOM khai báo transfer syntax của nó trong phần header File Meta Information.</p>

<p>Các thiết bị y tế, máy chủ PACS và các ứng dụng xem ảnh hỗ trợ các bộ transfer syntax khác nhau. <strong>Aspose.Medical for .NET</strong> cung cấp phương thức <code>Transcode</code> để chuyển đổi giữa các transfer syntax, cho phép khả năng tương tác, tối ưu lưu trữ, và tương thích với các công cụ xử lý &mdash; tất cả trong một thư viện .NET thuần không có phụ thuộc native.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Mã hoá lại một tệp DICOM trong C#">}}

<p>Phương thức <code>DicomFile.Transcode</code> chuyển đổi một tệp DICOM từ transfer syntax hiện tại sang bất kỳ transfer syntax đích nào được hỗ trợ. Phương thức trả về một thể hiện <code>DicomFile</code> mới &mdash; tệp gốc không bị thay đổi:</p>

<div class="codeblock" id="code">
 <h3>Mã hoá lại DICOM cơ bản - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Bạn cũng có thể mã hoá lại ở mức <code>Dataset</code> trực tiếp:</p>

<div class="codeblock" id="code">
 <h3>Mã hoá lại một Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các Transfer Syntax được hỗ trợ">}}

<p>Bảng dưới đây liệt kê tất cả các transfer syntax tiêu chuẩn cho dữ liệu ảnh DICOM và trạng thái hỗ trợ hiện tại trong Aspose.Medical for .NET. Tất cả các codec được hỗ trợ được triển khai bằng C# thuần và hoàn toàn độc lập nền tảng.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Type</th>
<th>Status</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Uncompressed</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Uncompressed</td><td>Supported</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Uncompressed</td><td>Supported</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Uncompressed (retired)</td><td>Supported</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Uncompressed</td><td>Không hỗ trợ</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Được hỗ trợ</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Lossy, 8-bit</td><td>Được hỗ trợ</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Lossy, 12-bit</td><td>Được hỗ trợ</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Lossless</td><td>Được hỗ trợ</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Lossless</td><td>Được hỗ trợ</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Lossless</td><td>Được hỗ trợ</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Near-lossless</td><td>Được hỗ trợ</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>Được hỗ trợ</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy or lossless</td><td>Được hỗ trợ</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Không hỗ trợ</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy or lossless</td><td>Không hỗ trợ</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Lossless</td><td>Được hỗ trợ</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Được hỗ trợ</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Được hỗ trợ</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy or lossless</td><td>Được hỗ trợ</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Lossless</td><td>Được hỗ trợ</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Lossless</td><td>Chỉ giải mã (mã hoá yêu cầu luồng nguồn JPEG, không phải dữ liệu píxel)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Lossy or lossless</td><td>Được hỗ trợ (chế độ lossy)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các kịch bản mã hoá lại phổ biến">}}

<p>Các quy trình làm việc khác nhau yêu cầu các chiến lược mã hoá lại khác nhau. Dưới đây là các kịch bản phổ biến nhất:</p>

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

{{< blocks/products/pf/feature-page-section h2="Kiểm tra các thuộc tính Transfer Syntax">}}

<p>Lớp <code>TransferSyntax</code> cung cấp các thuộc tính mô tả đặc tính mã hoá. Sử dụng chúng để kiểm tra transfer syntax hiện tại của một tệp hoặc để chọn một transfer syntax đích phù hợp:</p>

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
<th>Property</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>Mã định danh duy nhất của transfer syntax</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Cho biết Value Representation được mã hoá một cách rõ ràng hay không</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Cho biết thứ tự byte là little endian hay không</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Cho biết dữ liệu píxel được đóng gói (nén) hay không</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Cho biết phương pháp nén là lossy hay không</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Cho biết syntax sử dụng nén deflate hay không</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Cho biết transfer syntax đã bị loại bỏ theo tiêu chuẩn DICOM hay chưa</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>Mã định danh theo tiêu chuẩn ISO của phương pháp nén lossy</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nén Lossy vs Lossless">}}

<p>Hiểu sự khác biệt giữa nén lossy và lossless là rất quan trọng khi mã hoá lại các tệp DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Aspect</th>
<th>Lossless</th>
<th>Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Chất lượng hình ảnh</td><td>Pixel-perfect &mdash; dữ liệu gốc được bảo toàn hoàn toàn</td><td>Một số dữ liệu bị mất vĩnh viễn để đạt kích thước nhỏ hơn</td></tr>
<tr><td>Tỷ lệ nén</td><td>Typically 2:1 to 3:1</td><td>Typically 10:1 to 30:1 or higher</td></tr>
<tr><td>An toàn vòng quay</td><td>Có &mdash; giải nén và nhận được các pixel giống hệt</td><td>Không &mdash; mỗi lần mã hoá lại lossy sẽ làm giảm chất lượng hơn</td></tr>
<tr><td>Các trường hợp sử dụng</td><td>Lưu trữ, chẩn đoán, hồ sơ pháp lý</td><td>Đánh giá sơ bộ, y tế từ xa, truyền qua mạng</td></tr>
<tr><td>Các codec được hỗ trợ</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG Extended, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>Quan trọng:</strong> Mã hoá lại từ một tệp đã nén lossy sang transfer syntax lossless không khôi phục dữ liệu đã mất. Sự suy giảm chất lượng từ quá trình nén lossy ban đầu là vĩnh viễn.</p>

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
