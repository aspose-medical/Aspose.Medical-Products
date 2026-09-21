---
title: Làm việc với các tệp DICOM lớn bằng C# .NET | Aspose.Medical
weight: 11500

description: Mở các nghiên cứu đa khung và hình ảnh slide toàn bộ bằng C# mà không tải chúng vào bộ nhớ. Đọc siêu dữ liệu mà không cần dữ liệu pixel, hoãn các phần tử lớn, và di chuyển tệp qua các stream và pipe.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Các tệp DICOM lớn trong .NET C#" h2="Đọc siêu dữ liệu của một nghiên cứu đa khung mà không có pixel, hoãn các phần tử lớn cho đến khi có yêu cầu, và di chuyển toàn bộ tệp qua các stream và pipe." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Tệp lớn, nhưng câu hỏi thường nhỏ">}}

<p>Một hình ảnh slide toàn bộ, một chuỗi CT dài hoặc một khối OCT có kích thước lên tới hàng trăm megabyte, và phần lớn là dữ liệu pixel. Công việc thực tế một ứng dụng thực hiện thường nhỏ hơn nhiều: liệt kê nội dung trong thư mục, kiểm tra mã định danh bệnh nhân, đếm số khung, quyết định nơi lưu trữ nghiên cứu. Tải toàn bộ byte để trả lời những yêu cầu này chính là nguyên nhân biến một nhiệm vụ đơn giản thành vấn đề bộ nhớ.</p>

<p><strong>Aspose.Medical for .NET</strong> cho phép người gọi quyết định lượng dữ liệu cần đọc từ tệp. Lựa chọn này được truyền qua một đối số của <code>DicomFile.Open</code>, và áp dụng cho tệp, stream và pipe một cách đồng nhất.</p>

<p>Được đo trên một nghiên cứu 14 MB với 128 khung từ bộ thử nghiệm của chúng tôi, trên cùng một máy và cùng một tệp:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Chiến lược đọc</th>
<th>Thời gian mở</th>
<th>Bộ nhớ được cấp phát</th>
</tr>
</thead>
<tbody>
<tr><td>Tất cả, mặc định</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Các phần tử lớn bị bỏ qua</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Các phần tử lớn được hoãn</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>Khoảng cách tăng lên theo kích thước tệp. Một thư mục chứa 10.000 nghiên cứu là trường hợp mà việc tối ưu hoá vi mô không còn hiệu quả nữa.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Đọc siêu dữ liệu, để nguyên pixel">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> loại bỏ mọi phần tử có kích thước vượt quá ngưỡng ra khỏi quá trình đọc. Bộ dữ liệu trả về chỉ chứa các tag mà một chỉ mục hay router cần.</p>

<div class="codeblock" id="code">
 <h3>Đọc một nghiên cứu mà không có dữ liệu pixel - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>Ngưỡng mặc định là 64 kB và nhận giá trị tính bằng kilobyte, vì vậy một quy trình làm việc coi 8 kB là lớn có thể đặt như vậy.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Hoãn thay vì bỏ qua">}}

<p>Khi pixel có thể cần thiết, nhưng có khả năng sẽ được sử dụng sau này và không phải tất cả, <code>ReadLargeOnDemand</code> là phần còn lại của cặp. Việc mở tệp tốn thời gian như khi bỏ qua, và một phần tử lớn được đọc ngay khi mã chạm tới nó.</p>

<div class="codeblock" id="code">
 <h3>Tải một khung chỉ khi nó được sử dụng - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>Đọc hoãn là tính năng có giấy phép; các chiến lược khác cũng hoạt động trong chế độ evaluation.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lập chỉ mục thư mục mà không chạm tới pixel">}}

<p>Chiến lược tương tự áp dụng cho stream, đây là cách một quá trình quét lưu trữ hoặc lưu trữ đối tượng đám mây xuất hiện trong mã.</p>

<div class="codeblock" id="code">
 <h3>Quét lưu trữ - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stream và pipe, vào và ra">}}

<p>Cả đọc và ghi đều chấp nhận stream, và các điểm vào bất đồng bộ cũng chấp nhận các kiểu <code>System.IO.Pipelines</code>. Một nghiên cứu có thể di chuyển từ phản hồi mạng tới lưu trữ mà không cần quá trình nào giữ toàn bộ tệp dưới dạng một mảng.</p>

<div class="codeblock" id="code">
 <h3>Đọc và ghi qua stream - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>Ý tưởng tương tự áp dụng cho các biểu diễn dạng văn bản: một tài liệu chứa nhiều dataset được đọc từng dataset một trên các trang <a href="/medical/net/json-to-dicom/">JSON to DICOM</a> và <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Khung từng khung">}}

<p>Dữ liệu đa khung được truy cập theo từng khung, vì vậy một chuỗi 500 khung chỉ tiêu thụ một khung tại một thời điểm thay vì toàn bộ phần tử dữ liệu pixel.</p>

<div class="codeblock" id="code">
 <h3>Duyệt các khung - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nơi quyết định thiết kế">}}

<ul>
<li>Chỉ mục và di chuyển lưu trữ: hàng triệu tệp, và chỉ phần header quan trọng cho đến khi có vật gì đó được di chuyển.</li>
<li>Router và nút lưu trữ: chấp nhận một nghiên cứu, đọc những gì cần thiết để định tuyến, và truyền byte tiếp.</li>
<li>Pipeline AI: xây dựng manifest từ siêu dữ liệu, sau đó lấy các khung cho tập con thực sự được huấn luyện.</li>
<li>Container có giới hạn bộ nhớ: tập làm việc tuân theo chiến lược, không phải kích thước tệp.</li>
<li>Dữ liệu slide toàn bộ và OCT: các tệp mà việc đọc toàn bộ không phải là lựa chọn.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">Hướng dẫn quản lý bộ nhớ</a> giải thích chi tiết các chiến lược, và <a href="/medical/net/dicom-networking/">Mạng DICOM</a> hiển thị dữ liệu tương tự đến qua DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Hướng dẫn nhà phát triển" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
{{< blocks/products/pf/slr-element name="Tham khảo API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Hỗ trợ sản phẩm" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Hỗ trợ miễn phí" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Hỗ trợ trả phí" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Tại sao nên chọn Aspose.Medical cho .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Danh sách khách hàng" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Câu chuyện thành công" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
