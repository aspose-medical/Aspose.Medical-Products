---
title: Client DICOMweb trong C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Tìm kiếm, tải xuống và tải lên các nghiên cứu DICOM qua HTTP từ .NET. Một client DICOMweb thuần C# cho QIDO-RS, WADO-RS và STOW-RS, hoạt động với PACS, VNA và các dịch vụ hình ảnh đám mây.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Client DICOMweb cho .NET C#" h2="Tìm kiếm, tải xuống và tải lên các nghiên cứu trên bất kỳ máy chủ DICOMweb nào với QIDO-RS, WADO-RS và STOW-RS, trong C# quản lý mà không cần cài đặt gì trên máy." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM qua HTTP, từ mã của bạn">}}

<p>DICOMweb là dạng HTTP của DICOM. Thay vì một kết nối DIMSE trên cổng 104, ứng dụng của bạn gửi các yêu cầu HTTPS thông thường: QIDO-RS để tìm kiếm, WADO-RS để tải xuống, STOW-RS để tải lên. Các dịch vụ hình ảnh đám mây như Azure Health Data Services DICOM service và Google Cloud Healthcare API hoạt động theo cách này, và các kho lưu trữ như Orthanc và dcm4chee cũng vậy.</p>

<p><strong>Aspose.Medical cho .NET</strong> đã bao gồm một client DICOMweb kể từ phiên bản 26.10. <code>DicomWebClient</code> bao bọc mọi giao dịch chuẩn trong một phương thức kiểu, và kết quả được trả về dưới dạng các đối tượng <code>Dataset</code> và <code>DicomFile</code> giống như phần còn lại của thư viện đọc và ghi. Máy chủ ở phía bên kia có thể là bất kỳ triển khai DICOMweb nào; không cần phải được xây dựng bằng Aspose.Medical.</p>

<p>Client được viết bằng C# quản lý dựa trên <code>HttpClient</code>. Không có bộ công cụ gốc và không có dịch vụ proxy, vì vậy cùng một đoạn mã chạy trên Windows, Linux và trong container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kết nối tới máy chủ DICOMweb">}}

<p>Tạo client từ một <code>HttpClient</code> và URL gốc của dịch vụ trên máy chủ. Xác thực, cài đặt TLS, proxy và pool kết nối được giữ trên <code>HttpClient</code>, nơi ứng dụng của bạn đã cấu hình chúng. Client không sở hữu <code>HttpClient</code> và không giải phóng nó.</p>

<div class="codeblock" id="code">
 <h3>Tạo client DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Nếu một triển khai phục vụ WADO-URI hoặc dịch vụ khác từ một URL khác, <code>ServiceBaseUris</code> sẽ ghi đè gốc chỉ cho dịch vụ đó.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tìm kiếm các nghiên cứu với QIDO-RS">}}

<p>Một tìm kiếm nhận các tham số khớp với từ khóa DICOM làm tên, các trường cần trả về và điều khiển phân trang. Các kết quả khớp được trả về dưới dạng luồng các dataset, đọc từng cái một trong khi phản hồi vẫn đang tải xuống.</p>

<div class="codeblock" id="code">
 <h3>Tìm các nghiên cứu của một bệnh nhân - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> và <code>SearchInstancesAsync</code> tìm kiếm một cấp thấp hơn, trên toàn bộ kho lưu trữ hoặc trong một nghiên cứu hoặc series. Các tùy chọn tương tự cũng cho phép khớp mờ, khớp giá trị rỗng và thiết lập độ dịch kết quả cho phân trang.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tải xuống một nghiên cứu với WADO-RS">}}

<p>Một nghiên cứu có thể chứa hàng ngàn instance. <code>RetrieveStudyAsync</code> trả về chúng dưới dạng luồng, do đó mỗi tệp được phân tích khi phần phản hồi của nó đến và có thể được lưu trước khi tệp tiếp theo được đọc. Bộ nhớ không tăng theo kích thước của nghiên cứu.</p>

<div class="codeblock" id="code">
 <h3>Lưu mọi instance của một nghiên cứu - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> và <code>RetrieveInstanceAsync</code> tải xuống một series hoặc một instance theo cùng cách.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lấy ảnh thu nhỏ hoặc ảnh đã render">}}

<p>Một trình xem hoặc màn hình danh sách công việc thường cần một hình ảnh, không phải tệp DICOM. Máy chủ render nó và gửi dưới dạng JPEG hoặc PNG, vì vậy client không tải xuống hoặc giải mã dữ liệu pixel gốc.</p>

<div class="codeblock" id="code">
 <h3>Lưu ảnh thu nhỏ JPEG của một instance - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p><code>RetrieveRenderedInstanceAsync</code> và <code>RetrieveRenderedFramesAsync</code> trả về một ảnh toàn kích thước thay thế. <code>DicomWebRenderOptions</code> truyền các điều khiển render chuẩn tới máy chủ: chất lượng ảnh, kích thước viewport, window và hồ sơ ICC.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tải lên các instance với STOW-RS">}}

<p>STOW-RS gửi bất kỳ số lượng instance nào trong một yêu cầu. Máy chủ trả lời bằng một báo cáo liệt kê, từng instance một, những gì nó đã lưu và những gì nó từ chối.</p>

<div class="codeblock" id="code">
 <h3>Tải lên một thư mục các tệp DICOM - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>Một số máy chủ trả lời bằng lỗi HTTP, chẳng hạn 409, khi một hoặc nhiều instance thất bại. Lời gọi sau đó ném <code>DicomWebClientException</code>, như phần tiếp theo mô tả, và báo cáo giống nhau nằm trong phần thân phản hồi của ngoại lệ.</p>

<p><code>StoreStudyAsync</code> tải lên vào một nghiên cứu được đặt tên. Cả hai phương thức cũng chấp nhận một <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, vì vậy các tệp có thể được tạo ra trong khi yêu cầu đang được gửi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Xử lý lỗi máy chủ">}}

<p>Mọi thao tác có kiểu đều kiểm tra trạng thái HTTP trước khi trả về. Khi máy chủ từ chối yêu cầu, lời gọi ném <code>DicomWebClientException</code>, chứa mã trạng thái, các header và phần đầu của thân phản hồi để ghi log.</p>

<div class="codeblock" id="code">
 <h3>Báo cáo yêu cầu thất bại - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>Các phương thức trả về phản hồi thô, như thumbnail ở trên, để việc kiểm tra trạng thái cho bạn thông qua <code>IsSuccessStatusCode</code> hoặc <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các tính năng khác mà client hỗ trợ">}}

<p>Tìm kiếm, tải xuống và tải lên là các cuộc gọi phổ biến. Client còn có một nhóm thao tác cho mỗi dịch vụ DICOMweb khác:</p>

<ul>
<li>Dữ liệu bulk, dữ liệu pixel và các khung đã chọn của một instance, theo số khung bắt đầu từ 1, như máy chủ gửi.</li>
<li>WADO-URI, cách truy xuất URL đơn cũ mà nhiều trình xem và kho lưu trữ vẫn dùng, cho tệp DICOM và ảnh đã render.</li>
<li>Danh sách công việc UPS-RS: tạo, tìm kiếm, cập nhật và nhận workitems, thay đổi trạng thái, yêu cầu hủy, và đăng ký nhận sự kiện qua WebSocket.</li>
<li>Danh sách công việc modality (MWL-RS), các bước quy trình thực hiện modality (MPPS-RS) và cam kết lưu trữ.</li>
<li>Instance không liên quan tới bệnh nhân như bảng màu và giao thức treo, và tài liệu khả năng của máy chủ.</li>
<li><code>SendAsync</code> cho bất kỳ giao dịch chuẩn nào với các giá trị route, tham số truy vấn, header và body tự định nghĩa, khi phương thức có kiểu không phù hợp.</li>
</ul>

<p>Hướng dẫn <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">client DICOMweb</a> ghi lại mọi thao tác và tùy chọn, và <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">tài liệu tham chiếu các thao tác client</a> liệt kê mọi phương thức.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Một thư viện duy nhất từ phản hồi HTTP tới dữ liệu pixel">}}

<p>Một tệp được tải xuống bằng client là một <code>DicomFile</code> thông thường. Nó có thể được ẩn danh, chuyển mã, render hoặc tuần tự hoá trong cùng một quy trình, mà không cần bước chuyển đổi và không cần thư viện thứ hai trong chuỗi.</p>

<p>Trang liên quan: <a href="/medical/net/dicom-networking/">Mạng DICOM</a> cho các kho lưu trữ sử dụng DIMSE thay vì HTTP, <a href="/medical/net/anonymization/">ẩn danh</a> trước khi một nghiên cứu ra khỏi bệnh viện, <a href="/medical/net/dicom-transfer-syntax-conversion/">chuyển đổi cú pháp truyền</a> cho những gì máy chủ lưu, và <a href="/medical/net/dicom-to-json/">DICOM sang JSON</a> cho định dạng kết quả tìm kiếm QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Hướng dẫn nhà phát triển" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="Tham chiếu API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
