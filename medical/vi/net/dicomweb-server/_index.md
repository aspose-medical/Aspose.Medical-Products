---
title: Máy chủ DICOMweb bằng C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Xây dựng máy chủ DICOMweb hoặc PACS của riêng bạn bằng C#. Aspose.Medical triển khai QIDO-RS, WADO-RS, STOW-RS và các dịch vụ worklist trên ASP.NET Core; bạn chỉ cần kết nối lưu trữ.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Máy chủ DICOMweb cho .NET C#" h2="Thêm các endpoint QIDO-RS, WADO-RS và STOW-RS vào dịch vụ ASP.NET Core. Thư viện giao tiếp theo giao thức; mã của bạn quyết định nơi lưu trữ các study." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Máy chủ DICOMweb của bạn bằng C#">}}

<p>Một PACS, một VNA, một kho lưu trữ đám mây hoặc một nền tảng nghiên cứu phải đáp ứng các yêu cầu DICOMweb: tìm kiếm QIDO-RS, tải xuống WADO-RS và tải lên STOW-RS, và thường cả các dịch vụ worklist. Mỗi dịch vụ có các route, loại media, multipart body, quy tắc khớp và mã trạng thái riêng.</p>

<p><strong>Aspose.Medical for .NET</strong> đã triển khai phía giao thức này kể từ phiên bản 26.10. Nó đọc mọi yêu cầu, kiểm tra theo tiêu chuẩn, khớp các khóa tìm kiếm theo quy tắc DICOM, phân trang kết quả, chuyển đổi transfer syntax và ghi lại phản hồi. Mã của bạn thực hiện những gì chỉ bạn có thể làm: lưu một instance, tìm các ứng cử viên cho tìm kiếm và trả về các tệp đã lưu. Các trình xem, modality và các kho lưu trữ khác sau đó hoạt động với máy chủ của bạn giống như bất kỳ máy chủ DICOMweb nào khác, và cũng vậy với <a href="/medical/net/dicomweb-client/">DICOMweb client</a> của cùng thư viện.</p>

<p>Máy chủ được quản lý bằng C#. Nó chạy ở bất kỳ nơi nào ASP.NET Core chạy: trên Windows, Linux và trong container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Một máy chủ hoàn chỉnh trên ASP.NET Core">}}

<p>Cài đặt gói <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Gói này mang theo Aspose.Medical và kết nối các dịch vụ DICOMweb với dependency injection và routing endpoint. File <code>Program.cs</code> này là một máy chủ Studies hoạt động; hai lớp nó đăng ký được hiển thị bên dưới.</p>

<div class="codeblock" id="code">
 <h3>Program.cs của máy chủ DICOMweb - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>Mọi route Studies hiện nay trả lời dưới <code>/dicomweb</code>: <code>/dicomweb/studies</code> để tìm kiếm và tải lên, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> để truy xuất, và các route cho metadata, frames, bulk data và hình ảnh render. Tài liệu capabilities tại gốc dịch vụ liệt kê chính xác các giao dịch mà máy chủ hỗ trợ.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kết nối lưu trữ của bạn">}}

<p>Dịch vụ Studies yêu cầu một interface với ba phương thức: lưu một instance, trả về các tệp đã lưu, và trả về các ứng cử viên tìm kiếm. Triển khai này giữ mỗi instance dưới dạng tệp trong một thư mục cho mỗi study và series.</p>

<div class="codeblock" id="code">
 <h3>Kho lưu trữ dạng thư mục phía sau QIDO-RS, WADO-RS và STOW-RS - C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>Thư viện kiểm tra các UID trong mọi đường dẫn yêu cầu và trong mỗi instance được tải lên trước khi gọi nhà cung cấp. Một instance không có UID hợp lệ sẽ được báo là thất bại và không bao giờ đến <code>StoreAsync</code>.</p>

<p>Một thư mục là đủ để thử nghiệm máy chủ. Một kho lưu trữ production vẫn giữ ba phương thức này và đặt một object store phía sau <code>StoreAsync</code> và <code>RetrieveAsync</code>, cùng một index cơ sở dữ liệu phía sau <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tìm kiếm: thư viện áp dụng các quy tắc khớp DICOM">}}

<p>Khớp QIDO-RS không chỉ là so sánh chuỗi: ký tự đại diện, khoảng ngày và giờ, danh sách UID, tên người mờ, giá trị rỗng và khóa bên trong sequences. Nhà cung cấp không thực hiện bất kỳ điều nào trong số này. Nó trả về các ứng cử viên theo thứ tự ổn định, và có thể trả về nhiều hơn mức khớp. Thư viện áp dụng khớp DICOM chính xác cho mỗi ứng cử viên, giữ các trường được yêu cầu, và áp dụng <code>offset</code> và <code>limit</code>. Mỗi phản hồi mặc định chứa tối đa 1000 kết quả, và <code>DicomWebStandardServiceOptions</code> có thể thay đổi giới hạn này.</p>

<p>Để giữ tốc độ tìm kiếm nhanh trên kho lưu trữ lớn, chuyển các khóa chọn lọc tới cơ sở dữ liệu. <code>search.Plan.Filter</code> chứa các khóa tìm kiếm đã biên dịch, bao gồm thuộc tính, loại khớp và các giá trị của chúng.</p>

<div class="codeblock" id="code">
 <h3>Đọc Patient ID chính xác từ một tìm kiếm - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Một truy vấn cơ sở dữ liệu được xây dựng như vậy có thể trả về các hàng thừa, ví dụ khi nó bỏ qua một khóa không thể dịch. Khớp cuối cùng trong thư viện sẽ loại bỏ chúng. Nó không được xóa các khớp đúng, vì thư viện không thể thêm chúng trở lại.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lưu: một kết quả cho mỗi instance">}}

<p>Một yêu cầu STOW-RS có thể chứa hàng trăm instance. Thư viện phân tích multipart body và gọi <code>StoreAsync</code> cho mỗi instance lần lượt, vì vậy toàn bộ quá trình tải lên không bao giờ được giữ trong bộ nhớ đồng thời. Đối với mỗi instance, nhà cung cấp trả về <code>Stored</code>, <code>Warning</code> nếu instance được lưu với thay đổi, hoặc <code>Failed</code> kèm mã lý do DICOM tùy chọn.</p>

<p>Thư viện xây dựng báo cáo STOW-RS từ các kết quả này và chọn mã trạng thái: 200 khi mọi instance đều được lưu, 202 khi một số có cảnh báo hoặc thất bại, 409 khi tất cả thất bại. Các instance trong một yêu cầu cho một study cụ thể nhưng thuộc study khác sẽ bị từ chối trước khi gọi nhà cung cấp.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Truy xuất: transfer syntaxes, frames và hình ảnh render">}}

<p>Nhà cung cấp trả về các tệp đã lưu, và thư viện cung cấp chúng dưới dạng mà client yêu cầu. Một client WADO-RS có thể yêu cầu một transfer syntax trong header <code>Accept</code>, và thư viện sẽ chuyển đổi mỗi tệp trước khi gửi, với các codec riêng, bao gồm HTJ2K và JPEG XL. Metadata dưới dạng DICOM JSON hoặc XML, bulk data và single frames đều được lấy từ cùng một nhà cung cấp, mà không cần code bổ sung.</p>

<p>Các hình ảnh render cần một lớp bổ sung, vì chỉ bạn mới quyết định hình ảnh nào máy chủ có thể tạo ra. Lớp này cung cấp JPEG cho các instance đã render, các frame đã render và thumbnail, và thư viện sẽ render chúng:</p>

<div class="codeblock" id="code">
 <h3>Cung cấp hình ảnh JPEG đã render và thumbnail - C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>Renderer tích hợp tạo JPEG từ một instance single-frame hoặc từ các frame mà yêu cầu chọn. Một instance multi-frame đã render toàn bộ, các định dạng ảnh khác, và các tham số viewport, window và ICC profile cần <code>RetrieveAsync</code> do bạn tự triển khai trong lớp này.</p>

<p>Thumbnail được xử lý cẩn thận để bảo vệ quyền riêng tư bệnh nhân. Renderer tích hợp chỉ tạo thumbnail từ một instance có các thuộc tính Burned In Annotation và Recognizable Visual Features đều là <code>NO</code>, nghĩa là hình ảnh tự khai báo không hiển thị dữ liệu bệnh nhân. Nhiều hình ảnh không có các thuộc tính này, và đối với chúng máy chủ sẽ trả về 406. Nếu kho lưu trữ của bạn cần thumbnail cho các hình ảnh như vậy, hãy tạo chúng trong <code>RetrieveAsync</code> của riêng bạn và đảm bảo chúng ẩn mọi dữ liệu bệnh nhân.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklist, bước quy trình và các dịch vụ khác">}}

<p>Các dịch vụ DICOMweb khác tuân theo cùng mô hình: đăng ký dịch vụ, triển khai giao diện lưu trữ của nó.</p>

<ul>
<li>Worklist UPS-RS với <code>AddStandardWorklistService</code>: tạo, tìm kiếm và cập nhật workitem và thay đổi trạng thái của chúng, với các quy tắc trạng thái được thư viện kiểm tra và thông báo sự kiện được gửi qua WebSocket.</li>
<li>Worklist modality (MWL-RS) với <code>AddStandardModalityWorklistService</code>, cho phép các modality truy vấn các bước quy trình đã lên lịch trong RIS của bạn.</li>
<li>Modality performed procedure steps (MPPS-RS), cam kết lưu trữ và các instance không liên quan đến bệnh nhân như bảng màu và hanging protocols.</li>
<li>Truy xuất WADO-URI cho các viewer cũ, được phục vụ bởi dịch vụ Studies.</li>
</ul>

<p>Khi hành vi chuẩn của một giao dịch không phù hợp, đăng ký trình xử lý riêng của bạn cho nó bằng <code>AddHandler</code>. Thư viện vẫn thực hiện routing và kiểm tra yêu cầu, và các giao dịch khác giữ các handler chuẩn của chúng.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bảo mật và triển khai vẫn nằm trong tay bạn">}}

<p>Các endpoint DICOMweb là các endpoint ASP.NET Core thông thường. Xác thực, ủy quyền, CORS, TLS, header chuyển tiếp và giới hạn kích thước yêu cầu được cấu hình giống như phần còn lại của dịch vụ của bạn:</p>

<div class="codeblock" id="code">
 <h3>Yêu cầu người dùng đã xác thực - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Phần giao thức không phụ thuộc vào ASP.NET Core. <code>DicomWebApplication</code> chuyển <code>DicomWebRequest</code> thành phản hồi, vì vậy các dịch vụ tương tự có thể chạy phía sau một máy chủ HTTP khác hoặc trong môi trường test mà không cần mạng:</p>

<div class="codeblock" id="code">
 <h3>Máy chủ tương tự mà không cần ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Client và server từ một thư viện">}}

<p><a href="/medical/net/dicomweb-client/">DICOMweb client</a> của Aspose.Medical kiểm tra máy chủ của bạn từ một unit test hoặc chương trình console, và mỗi tệp mà máy chủ nhận được là một <code>DicomFile</code> thông thường có thể được ẩn danh, chuyển mã hoặc render trong cùng một tiến trình.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">Hướng dẫn phát triển DICOMweb</a> bao phủ mọi dịch vụ và giao diện lưu trữ, và <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">hướng dẫn ASP.NET Core</a> đề cập đến việc mapping endpoint và WebSockets.</p>

<p>Các trang liên quan: <a href="/medical/net/dicom-networking/">Mạng DICOM</a> cho các hệ thống gửi study qua DIMSE thay vì HTTP, <a href="/medical/net/anonymization/">ẩn danh</a> các study đến, <a href="/medical/net/dicom-transfer-syntax-conversion/">chuyển đổi transfer syntax</a>, và <a href="/medical/net/dicom-to-json/">DICOM sang JSON</a> cho định dạng kết quả QIDO-RS sử dụng.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Hướng dẫn nhà phát triển" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
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
