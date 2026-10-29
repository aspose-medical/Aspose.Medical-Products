---
title: Client DICOMweb trong C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Tìm kiếm, tải xuống và tải lên các study DICOM qua HTTP từ .NET. Một client DICOMweb thuần C# cho QIDO-RS, WADO-RS và STOW-RS hoạt động với PACS, VNA và các dịch vụ hình ảnh đám mây.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Client DICOMweb cho .NET C#" h2="Tìm kiếm, tải xuống và tải lên các study trên bất kỳ máy chủ DICOMweb nào với QIDO-RS, WADO-RS và STOW-RS, bằng C# quản lý mà không cần cài đặt gì trên máy." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM qua HTTP, từ mã của bạn">}}

<p>DICOMweb là dạng HTTP của DICOM. Thay vì một kết nối DIMSE trên cổng 104, ứng dụng của bạn gửi các yêu cầu HTTPS thông thường: QIDO-RS để tìm kiếm, WADO-RS để tải xuống, STOW-RS để tải lên. Các dịch vụ hình ảnh đám mây như Azure Health Data Services DICOM service và Google Cloud Healthcare API hoạt động theo cách này, cũng như các kho lưu trữ như Orthanc và dcm4chee.</p>

<p><strong>Aspose.Medical cho .NET</strong> bao gồm một client DICOMweb kể từ phiên bản 26.10. <code>DicomWebClient</code> bọc mỗi giao dịch tiêu chuẩn trong một phương thức có kiểu, và kết quả được trả về dưới dạng các đối tượng <code>Dataset</code> và <code>DicomFile</code> giống như phần còn lại của thư viện đọc và ghi. Máy chủ ở phía bên kia có thể là bất kỳ triển khai DICOMweb nào; nó không bắt buộc phải được xây dựng bằng Aspose.Medical.</p>

<p>Client được viết bằng C# quản lý trên nền <code>HttpClient</code>. Không có bộ công cụ gốc và không có dịch vụ proxy, vì vậy cùng một đoạn mã có thể chạy trên Windows, Linux và trong container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kết nối tới máy chủ DICOMweb">}}

<p>Tạo client từ một <code>HttpClient</code> và URL gốc của dịch vụ trên máy chủ. Xác thực, cài đặt TLS, proxy và việc gộp kết nối vẫn được giữ trên <code>HttpClient</code>, nơi ứng dụng của bạn đã cấu hình chúng. Client không sở hữu <code>HttpClient</code> và không giải phóng nó.</p>

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

<p>Nếu một triển khai cung cấp WADO-URI hoặc dịch vụ khác từ một URL khác, <code>ServiceBaseUris</code> sẽ ghi đè gốc chỉ cho dịch vụ đó.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tìm kiếm các study bằng QIDO-RS">}}

<p>Một tìm kiếm nhận các tham số khớp với từ khóa DICOM làm tên, các trường cần trả về, và các điều khiển phân trang. Các kết quả khớp được truyền dưới dạng luồng datasets, đọc từng cái một trong khi phản hồi vẫn đang tải xuống.</p>

<div class="codeblock" id="code">
 <h3>Tìm các study của bệnh nhân - C#</h3>
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

<p><code>SearchSeriesAsync</code> và <code>SearchInstancesAsync</code> tìm kiếm một cấp độ xuống dưới, trên toàn bộ kho lưu trữ hoặc bên trong một study hoặc series. Các tùy chọn giống nhau cũng cho phép thiết lập khớp mờ, khớp giá trị rỗng và độ dịch kết quả cho việc phân trang.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tải xuống một study bằng WADO-RS">}}

<p>Một study có thể chứa hàng nghìn instance. <code>RetrieveStudyAsync</code> trả về chúng dưới dạng luồng, vì vậy mỗi tệp được phân tích khi phần tương ứng của phản hồi đến và có thể được lưu trước khi tệp tiếp theo được đọc. Việc sử dụng bộ nhớ không tăng theo kích thước của study.</p>

<div class="codeblock" id="code">
 <h3>Lưu mọi instance của một study - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Lấy hình thu nhỏ hoặc hình ảnh đã render">}}

<p>Một trình xem hoặc màn hình danh sách công việc thường cần một hình ảnh, không phải tệp DICOM. Máy chủ render và gửi một JPEG hoặc PNG, vì vậy client không tải xuống hay giải mã dữ liệu pixel gốc.</p>

<div class="codeblock" id="code">
 <h3>Lưu hình thu nhỏ JPEG của một instance - C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> và <code>RetrieveRenderedFramesAsync</code> trả về một hình ảnh kích thước đầy đủ thay thế. <code>DicomWebRenderOptions</code> truyền các điều khiển render tiêu chuẩn tới máy chủ: chất lượng hình ảnh, kích thước viewport, window và ICC profile.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tải lên các instance bằng STOW-RS">}}

<p>STOW-RS gửi bất kỳ số lượng instance nào trong một yêu cầu. Máy chủ trả lời bằng một báo cáo liệt kê, từng instance một, những gì đã được lưu và những gì đã bị từ chối.</p>

<div class="codeblock" id="code">
 <h3>Tải lên một thư mục chứa các tệp DICOM - C#</h3>
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

<p>Một số máy chủ trả lời bằng lỗi HTTP, chẳng hạn 409, khi một hoặc nhiều instance thất bại. Lời gọi sau đó ném <code>DicomWebClientException</code>, như phần tiếp theo chỉ ra, và báo cáo tương tự nằm trong phần nội dung phản hồi của ngoại lệ.</p>

<p><code>StoreStudyAsync</code> tải lên vào một study có tên. Cả hai phương thức đều chấp nhận một <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, vì vậy các tệp có thể được tạo ra trong khi yêu cầu đang được gửi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Xử lý lỗi máy chủ">}}

<p>Mỗi thao tác có kiểu đều kiểm tra trạng thái HTTP trước khi trả về. Khi máy chủ từ chối một yêu cầu, lời gọi ném <code>DicomWebClientException</code>, chứa mã trạng thái, các header và phần đầu của nội dung phản hồi để ghi log.</p>

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

<p>Những phương thức trả về phản hồi thô, như hình thu nhỏ ở trên, để bạn tự kiểm tra trạng thái qua <code>IsSuccessStatusCode</code> hoặc <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Những gì khác mà client hỗ trợ">}}

<p>Tìm kiếm, tải xuống và tải lên là các cuộc gọi thường dùng. Client còn có một nhóm thao tác cho mỗi dịch vụ DICOMweb khác:</p>

<ul>
<li>Dữ liệu bulk, dữ liệu pixel và các khung đã chọn của một instance, theo số khung bắt đầu từ 1, như máy chủ gửi.</li>
<li>WADO-URI, cách truy xuất bằng một URL duy nhất cũ mà nhiều trình xem và kho lưu trữ vẫn sử dụng, cho tệp DICOM và hình ảnh đã render.</li>
<li>Danh sách công việc UPS-RS: tạo, tìm kiếm, cập nhật và nhận workitem, thay đổi trạng thái, yêu cầu hủy, và đăng ký nhận sự kiện qua WebSocket.</li>
<li>Danh sách công việc thiết bị (MWL-RS), các bước quy trình thực hiện thiết bị (MPPS-RS) và cam kết lưu trữ.</li>
<li>Instance không liên quan đến bệnh nhân như bảng màu và giao thức treo, và tài liệu khả năng của máy chủ.</li>
<li><code>SendAsync</code> cho bất kỳ giao dịch tiêu chuẩn nào với các giá trị route, tham số truy vấn, header và body do bạn cung cấp, khi phương thức có kiểu không phù hợp.</li>
</ul>

<p>The <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">hướng dẫn client DICOMweb</a> ghi chép mọi thao tác và tùy chọn, và <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">tài liệu tham chiếu các thao tác của client</a> liệt kê mọi phương thức.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Một thư viện duy nhất từ phản hồi HTTP đến dữ liệu pixel">}}

<p>Một tệp được tải về bằng client là một <code>DicomFile</code> thông thường. Nó có thể được ẩn danh, chuyển mã, render hoặc tuần tự hoá trong cùng một tiến trình, mà không cần bước chuyển đổi và không cần thư viện thứ hai trong chuỗi.</p>

<p>Trang liên quan: <a href="/medical/net/dicomweb-server/">DICOMweb server</a> cho phía đối nghịch của giao thức trong dịch vụ ASP.NET Core của bạn, <a href="/medical/net/dicom-networking/">DICOM networking</a> cho các kho lưu trữ sử dụng DIMSE thay vì HTTP, <a href="/medical/net/anonymization/">anonymization</a> trước khi một study rời bệnh viện, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> cho dữ liệu mà máy chủ lưu trữ, và <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> cho định dạng kết quả tìm kiếm QIDO-RS sử dụng.</p>

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

{{< blocks/products/pf/slr-tab tabTitle="Tại sao lại chọn Aspose.Medical cho .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Danh sách khách hàng" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Câu chuyện thành công" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
