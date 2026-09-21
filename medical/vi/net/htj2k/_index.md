---
title: HTJ2K trong C# .NET - High-Throughput JPEG 2000 cho DICOM | Aspose.Medical
weight: 10000

description: Nén và đọc hình ảnh DICOM bằng High-Throughput JPEG 2000 từ C#. HTJ2K lossless, biến thể RPCL và HTJ2K lossy, được triển khai trong .NET quản lý mà không cần codec gốc để triển khai.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K trong .NET C#" h2="High-Throughput JPEG 2000 cho DICOM: thuật toán nén được tiêu chuẩn thêm vào để lưu trữ nhanh và xem trên đám mây, được triển khai trong C# quản lý mà không cần cài đặt thành phần gốc nào." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Những thay đổi của HTJ2K">}}

<p>High-Throughput JPEG 2000 giữ lại wavelet và chất lượng hình ảnh của JPEG 2000 và thay thế phần làm cho nó chậm. Bộ mã khối là mới, và quá trình giải mã nhanh hơn hàng chục lần, vì vậy tiêu chuẩn DICOM đã chấp nhận nó trong ba transfer syntax và các nền tảng hình ảnh đám mây cũng chuyển sang sử dụng.</p>

<p>Đối với một nhóm .NET, câu hỏi thực tiễn lại khác: ai có thể thực sự tạo ra những tệp này. Hầu hết các thư viện tiếp cận HTJ2K thông qua một bản dựng OpenJPH gốc, đồng nghĩa với một binary cho mỗi nền tảng, một bước xây dựng trong container và một phụ thuộc mà quy trình đánh giá bảo mật sẽ hỏi tới. <strong>Aspose.Medical for .NET</strong> triển khai codec trong mã quản lý bên trong cùng một gói đọc và ghi các tệp, vì vậy HTJ2K hoạt động nhất quán trên Windows, Linux và trong container, mà không cần cài đặt gì.</p>

<p>Ba transfer syntax được hỗ trợ, và cả ba đều hỗ trợ đọc và ghi:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), High-Throughput JPEG 2000 không mất dữ liệu.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), biến thể không mất dữ liệu với thứ tự tiến trình RPCL.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nén một study sang HTJ2K">}}

<p>Một lời gọi duy nhất chuyển tệp sang syntax mới. Bộ dữ liệu, các thẻ private và thông tin meta của tệp đều đi cùng với nó.</p>

<div class="codeblock" id="code">
 <h3>Chuyển mã một tệp DICOM sang HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>Trên một hình ảnh 16-bit kích thước 1714 x 1933 từ bộ dữ liệu kiểm thử của chúng tôi, kích thước tệp giảm từ 6.3 MB xuống 2.9 MB, và các pixel trở lại hoàn toàn giống nguyên bản. Các con số này khác nhau tùy modality và hình ảnh, vì vậy hãy đo trên dữ liệu của riêng bạn, bằng cách lặp qua các tệp hiện có.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossless nghĩa là không mất dữ liệu">}}

<p>Dữ liệu chẩn đoán không chấp nhận codec chỉ gần đúng. Chuyển mã sang HTJ2K lossless và quay lại, dữ liệu pixel sẽ hoàn toàn giống với byte bạn bắt đầu, đây là thuộc tính bạn có thể xác nhận trong bộ kiểm thử của mình trước khi đồng ý nén lại một archive.</p>

<div class="codeblock" id="code">
 <h3>Quay lại một syntax không nén - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, biến thể được tạo ra để xem qua mạng">}}

<p>Syntax 1.2.840.10008.1.2.4.202 lưu cùng một codestream lossless theo thứ tự tiến trình RPCL: độ phân giải trước, sau đó vị trí, sau đó thành phần, cuối cùng là lớp. Một trình đọc chỉ lấy phần đầu của stream sẽ nhận được một hình ảnh độ phân giải thấp đầy đủ, đó là những gì một viewer cần khi mở một study lớn qua một liên kết mà nó không kiểm soát.</p>

<div class="codeblock" id="code">
 <h3>Nén với thứ tự tiến trình RPCL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Đọc những gì một archive gửi cho bạn">}}

<p>Một nửa công việc còn lại là chấp nhận HTJ2K từ các hệ thống đã tạo ra nó. Mở tệp, kiểm tra nó được lưu dưới dạng gì, và làm việc với dữ liệu pixel.</p>

<div class="codeblock" id="code">
 <h3>Đọc một tệp HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Hình ảnh đa khung được xử lý theo từng khung, vì vậy một chuỗi dài tốn bộ nhớ theo mỗi khung thay vì theo toàn bộ study.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nơi HTJ2K khẳng định vị thế của mình">}}

<ul>
<li>Di chuyển archive: nén lại một study đã lưu sang HTJ2K lossless, giảm footprint, giữ nguyên dữ liệu chẩn đoán.</li>
<li>Đám mây và DICOMweb: tốc độ giải mã là yếu tố khiến viewer phía trình duyệt hoặc phía server cảm thấy ngay lập tức trên các hình ảnh lớn.</li>
<li>AI pipelines: các bộ dữ liệu đào tạo được đọc thường xuyên hơn nhiều so với viết, và thời gian giải mã là chi phí lặp lại.</li>
<li>Containers và serverless: codec là một phần của assembly, vì vậy một image không cần thư viện gốc hay trình biên dịch trong quá trình build.</li>
</ul>

<p>Thư viện cũng cung cấp JPEG XL, bổ sung mới khác của tiêu chuẩn, và các codec cũ mà một archive có thể chứa: JPEG, JPEG-LS, JPEG 2000 và RLE. Trang <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> bao phủ toàn bộ bộ, và trang <a href="/medical/net/jpeg2000/">JPEG 2000</a> giới thiệu codec mà HTJ2K phát triển từ đó.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Hướng dẫn nhà phát triển" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="Tham chiếu API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Hỗ trợ sản phẩm" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Hỗ trợ miễn phí" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Hỗ trợ trả phí" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Tại sao Aspose.Medical cho .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Danh sách khách hàng" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Câu chuyện thành công" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
