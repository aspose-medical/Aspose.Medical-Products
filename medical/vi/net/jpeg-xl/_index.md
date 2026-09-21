---
title: JPEG XL cho DICOM trong C# .NET | Aspose.Medical
weight: 10500

description: Lưu ảnh DICOM dưới dạng JPEG XL từ C#. JPEG XL không mất dữ liệu trả lại các pixel bit‑đối‑bit, trong một assembly quản lý duy nhất mà không cần mã codec gốc để triển khai.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL cho DICOM trong .NET C#" h2="Phương pháp nén mới nhất trong tiêu chuẩn DICOM, với các tệp không mất dữ liệu nhỏ nhất mà chúng tôi đo được, được triển khai bằng C# quản lý và được cung cấp trong một assembly duy nhất." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Tại sao JPEG XL được đưa vào DICOM">}}

<p>Kho lưu trữ y tế ngày càng lớn và không bao giờ giảm. JPEG XL là codec mà cộng đồng hình ảnh thiết kế sau hai thập kỷ kinh nghiệm với JPEG và JPEG 2000, và DICOM đã thêm nó như một transfer syntax vì lý do mà các đội ngũ lưu trữ quan tâm: cùng một pixel, tệp sẽ nhỏ hơn.</p>

<p><strong>Aspose.Medical cho .NET</strong> ghi và đọc JPEG XL thông qua một cổng C# của libjxl nằm trong thư viện. Gói phần mềm cung cấp một assembly, <code>Aspose.Medical.dll</code>, và không có binary gốc nào kèm theo, vì vậy một codec mới như vậy không biến thành một dự án triển khai: cùng một assembly chạy trên Windows, Linux, trên máy build và trong container.</p>

<p>Hai transfer syntax mang các pixel:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), cho dữ liệu chẩn đoán cần được trả lại nguyên vẹn.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), cho các trường hợp mà tệp nhỏ hơn quan trọng hơn so với một bản sao hoàn toàn giống.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nén một nghiên cứu, giữ nguyên mọi pixel">}}

<p>Quá trình chuyển mã chỉ cần một lời gọi, và tập dữ liệu xung quanh các pixel được truyền cùng với nó.</p>

<div class="codeblock" id="code">
 <h3>Chuyển mã tệp DICOM sang JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Chúng tôi đo trên một ảnh 16‑bit kích thước 1714 × 1933 từ tập kiểm thử của mình: kích thước 6,3 MB chưa nén trở thành 2,7 MB trong JPEG XL không mất dữ liệu, nhỏ hơn so với cùng ảnh trong HTJ2K không mất dữ liệu. Các con số của bạn sẽ phụ thuộc vào mô thức, vì vậy hãy thực hiện so sánh trên một thư mục các tệp của bạn trước khi quyết định.</p>

<p>‘Lossless’ ở đây phải được hiểu theo nghĩa đen. Chuyển mã sang JPEG XL và trở lại, dữ liệu pixel bằng đúng byte bạn bắt đầu, vì vậy một kho lưu trữ có thể được nén lại mà không gây thảo luận về chất lượng chẩn đoán.</p>

<div class="codeblock" id="code">
 <h3>Quay lại một syntax không nén - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Đọc những gì đã được lưu dưới dạng JPEG XL">}}

<p>Một tệp nhận được dưới dạng JPEG XL được mở như bất kỳ tệp nào khác. Transfer syntax cho biết loại tệp, và dữ liệu pixel có sẵn sau khi khung được giải mã.</p>

<div class="codeblock" id="code">
 <h3>Mở tệp JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL hay HTJ2K">}}

<p>Cả hai đều mới, cả hai đều không mất dữ liệu khi bạn yêu cầu không mất dữ liệu, và thư viện có thể ghi và đọc cả hai. Chúng trả lời các câu hỏi khác nhau.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Câu hỏi</th>
<th>Trả lời</th>
</tr>
</thead>
<tbody>
<tr><td>Trong thử nghiệm của chúng tôi, cái nào tạo tệp nhỏ hơn</td><td>JPEG XL không mất dữ liệu, khoảng vài phần trăm</td></tr>
<tr><td>Cái nào được xây dựng cho việc xem tiến trình qua mạng</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, đặc biệt là biến thể RPCL</td></tr>
<tr><td>Trong DICOM, cái nào được đưa vào tiêu chuẩn trước</td><td>HTJ2K, vì vậy nhiều kho lưu trữ hơn hiện nay chấp nhận nó</td></tr>
<tr><td>Trong trường hợp này, cái nào yêu cầu phụ thuộc native</td><td>Không, cả hai đều là mã quản lý trong một assembly</td></tr>
</tbody>
</table>

<p>Lựa chọn thường xuất phát từ phía bên kia của liên kết: chuyển mã sang syntax mà kho lưu trữ chấp nhận, và giữ nguyên phần còn lại của quy trình.</p>

<div class="codeblock" id="code">
 <h3>Để kho lưu trữ đích quyết định - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nơi mà nó có lợi">}}

<ul>
<li>Kho lưu trữ dài hạn: cùng các nghiên cứu, ít terabyte hơn, và không có mất mát nào cần biện minh với bác sĩ hình ảnh.</li>
<li>Chi phí lưu trữ đám mây: tiết kiệm lặp lại mỗi tháng, trong khi chuyển mã chỉ chạy một lần.</li>
<li>Bộ dữ liệu cho nghiên cứu và AI: các bản sao nhỏ hơn di chuyển nhanh hơn giữa lưu trữ và đào tạo.</li>
<li>Triển khai: một codec mới như vậy thường đòi hỏi bản dựng native cho mỗi nền tảng; ở đây nó là một phần của assembly bạn đã tham chiếu.</li>
</ul>

<p>Thư viện cũng ghi các codec mà một kho lưu trữ hiện có chứa: JPEG, JPEG‑LS, JPEG 2000, HTJ2K và RLE. Trang <a href="/medical/net/dicom-transfer-syntax-conversion/">chuyển đổi transfer syntax</a> bao quát toàn bộ bộ, <a href="/medical/net/htj2k/">HTJ2K</a> có trang riêng, và <a href="/medical/net/jpeg2000/">JPEG 2000</a> là nguồn gốc của cả hai codec mới.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Hướng dẫn cho nhà phát triển" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
