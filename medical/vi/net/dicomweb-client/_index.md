---
title: Khách hàng DICOMweb bằng C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Tìm kiếm, tải xuống và tải lên các nghiên cứu DICOM qua HTTP từ .NET. Một khách hàng DICOMweb bằng C# thuần cho QIDO-RS, WADO-RS và STOW-RS, hoạt động với PACS, VNA và các dịch vụ hình ảnh đám mây.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Khách hàng DICOMweb cho .NET C#" h2="Tìm kiếm, tải xuống và tải lên các nghiên cứu trên bất kỳ máy chủ DICOMweb nào bằng QIDO-RS, WADO-RS và STOW-RS, trong C# quản lý mà không cần cài đặt gì trên máy." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM qua HTTP, từ mã của bạn">}}

<p>DICOMweb là dạng HTTP của DICOM. Thay vì một kết nối DIMSE trên cổng 104, ứng dụng của bạn gửi các yêu cầu HTTPS thông thường: QIDO-RS để tìm kiếm, WADO-RS để tải xuống, STOW-RS để tải lên. Các dịch vụ hình ảnh đám mây như Azure Health Data Services DICOM service và Google Cloud Healthcare API hoạt động theo cách này, và các kho lưu trữ như Orthanc và dcm4chee cũng vậy.</p>

<p><strong>Aspose.Medical for .NET</strong> bao gồm một khách hàng DICOMweb kể từ phiên bản 26.10. <code>DicomWebClient</code> gói mọi giao dịch chuẩn trong một phương thức kiểu, và kết quả được trả về dưới các đối tượng <code>Dataset</code> và <code>DicomFile</code> giống như phần còn lại của thư viện đọc và ghi. Máy chủ phía bên kia có thể là bất kỳ triển khai DICOMweb nào; nó không bắt buộc phải được xây dựng bằng Aspose.Medical.</p>

<p>Khách hàng là C# quản lý dựa trên <code>HttpClient</code>. Không có bộ công cụ gốc và không có dịch vụ proxy, vì vậy cùng một mã chạy trên Windows, Linux và trong container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kết nối tới máy chủ DICOMweb">}}

<p>Tạo khách hàng từ một <code>HttpClient</code> và URL gốc của dịch vụ trên máy chủ. Xác thực, cài đặt TLS, proxy và pool kết nối vẫn nằm trên <code>HttpClient</code>, nơi ứng dụng của bạn đã cấu hình chúng. Khách hàng không sở hữu <code>HttpClient</code> và không giải phóng nó.</p>

<div class="codeblock" id="code">
 <h3>Tạo khách hàng DICOMweb - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Tìm kiếm các nghiên cứu với QIDO-RS">}}

<p>Một truy vấn tìm kiếm nhận các tham số khớp với từ khóa DICOM làm tên, các trường cần trả về, và điều khiển phân trang. Các kết quả khớp được đưa về dưới dạng luồng các dataset, đọc từng cái một trong khi phản hồi vẫn đang tải xuống.</p>

<div class="codeblock" id="code">
 <h3>Tim các nghiên cứu của bệnh nhân - C#</h3>
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

<p><code>SearchSeriesAsync</code> và <code>SearchInstancesAsync</code> tìm kiếm một cấp độ xuống, trên toàn bộ kho lưu trữ hoặc bên trong một nghiên cứu hoặc series. Các tùy chọn tương tự cũng đặt khớp mờ, khớp giá trị trống và độ dịch kết quả cho việc phân trang.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tải xuống một nghiên cứu với WADO-RS">}}

<p>Một nghiên cứu có thể chứa hàng ngàn instance. <code>RetrieveStudyAsync</code> trả về chúng dưới dạng luồng, do đó mỗi tệp được phân tích khi phần phản hồi của nó đến và có thể được lưu trước khi tệp tiếp theo được đọc. Việc sử dụng bộ nhớ không tăng theo kích thước của nghiên cứu.</p>

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

<p><code>RetrieveSeriesAsync</code> và <code>RetrieveInstanceAsync</code> tải xuống một series hoặc một instance theo cùng một cách.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lấy ảnh thu nhỏ hoặc ảnh đã render">}}

<p>Một trình xem hoặc màn hình danh sách công việc thường cần một hình ảnh, không phải tệp DICOM. Máy chủ render nó và gửi JPEG hoặc PNG, vì vậy khách hàng không tải xuống hay giải mã dữ liệu pixel gốc.</p>

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

<p><code>RetrieveRenderedInstanceAsync</code> và <code>RetrieveRenderedFramesAsync</code> trả về một hình ảnh kích thước đầy đủ thay thế. <code>DicomWebRenderOptions</code> truyền các điều khiển render chuẩn tới máy chủ: chất lượng ảnh, kích thước viewport, cửa sổ và hồ sơ ICC.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tải lên các instance với STOW-RS">}}

<p>STOW-RS gửi bất kỳ số lượng instance nào trong một yêu cầu. Máy chủ trả lời bằng một báo cáo liệt kê, mỗi instance, những gì nó đã lưu và những gì nó từ chối.</p>

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

<p>Một số máy chủ trả lời bằng lỗi HTTP, chẳng hạn 409, khi một hoặc nhiều instance thất bại. Lúc đó cuộc gọi ném <code>DicomWebClientException</code>, như phần tiếp theo chỉ ra, và cùng báo cáo nằm trong phần thân phản hồi của ngoại lệ.</p>

<p><code>StoreStudyAsync</code> tải lên vào một nghiên cứu đã đặt tên. Cả hai phương pháp cũng chấp nhận <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, vì vậy các tệp có thể được tạo ra trong khi yêu cầu đang được gửi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Xử lý lỗi máy chủ">}}

<p>Mỗi thao tác kiểu kiểm tra trạng thái HTTP trước khi trả về. Khi máy chủ từ chối một yêu cầu, cuộc gọi ném <code>DicomWebClientException</code>, chứa mã trạng thái, header và phần đầu của thân phản hồi để ghi log.</p>

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

<p>Các phương thức trả về phản hồi nguyên thô, như ảnh thu nhỏ ở trên, để bạn tự kiểm tra trạng thái qua <code>IsSuccessStatusCode</code> hoặc <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Những gì khác mà khách hàng hỗ trợ">}}

<p>Tìm kiếm, tải xuống và tải lên là các cuộc gọi phổ biến. Khách hàng cũng có một nhóm thao tác cho mỗi dịch vụ DICOMweb khác.</p>

<ul>
<li>Dữ liệu bulk, dữ liệu pixel và các khung đã chọn của một instance, theo số khung bắt đầu từ 1, như máy chủ gửi.</li>
<li>WADO-URI, phương thức truy xuất URL duy nhất cũ mà nhiều trình xem và kho lưu trữ vẫn sử dụng, cho tệp DICOM và ảnh đã render.</li>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> danh sách công việc: tạo, tìm kiếm, cập nhật và yêu cầu workitems, thay đổi trạng thái, yêu cầu hủy, và đăng ký các sự kiện đến qua WebSocket.</li>
<li><a href="/medical/net/modality-worklist/">Danh sách công việc modality</a> (MWL-RS), các bước quy trình thực hiện modality (MPPS-RS) và cam kết lưu trữ.</li>
<li>Instance không phải bệnh nhân như bảng màu và giao thức treo, và tài liệu khả năng của máy chủ.</li>
<li><code>SendAsync</code> cho bất kỳ giao dịch chuẩn nào với các giá trị route, tham số truy vấn, header và body của bạn, khi phương thức kiểu không phù hợp.</li>
</ul>

<p>Hướng dẫn <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">khách hàng DICOMweb</a> ghi chép mọi thao tác và tùy chọn, và <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">tham khảo thao tác khách hàng</a> liệt kê mọi phương thức.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Một thư viện từ phản hồi HTTP tới dữ liệu pixel">}}

<p>Một tệp được tải xuống bằng khách hàng là một <code>DicomFile</code> bình thường. Nó có thể được ẩn danh, chuyển mã, render hoặc tuần tự hoá trong cùng một tiến trình, mà không cần bước chuyển đổi hay thư viện thứ hai trong chuỗi.</p>

<p>Trang liên quan: <a href="/medical/net/dicomweb-server/">DICOMweb server</a> cho phía còn lại của giao thức trong dịch vụ ASP.NET Core của bạn, <a href="/medical/net/dicom-networking/">DICOM networking</a> cho các kho lưu trữ dùng DIMSE thay vì HTTP, <a href="/medical/net/anonymization/">ẩn danh</a> trước khi một nghiên cứu rời bệnh viện, <a href="/medical/net/dicom-transfer-syntax-conversion/">chuyển đổi syntax truyền</a> cho những gì máy chủ lưu, và <a href="/medical/net/dicom-to-json/">DICOM sang JSON</a> cho định dạng kết quả tìm kiếm QIDO-RS sử dụng.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Hướng dẫn dành cho nhà phát triển" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
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
