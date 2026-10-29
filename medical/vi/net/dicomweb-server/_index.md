---
title: Máy chủ DICOMweb bằng C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Xây dựng máy chủ DICOMweb hoặc PACS của riêng bạn bằng C#. Aspose.Medical triển khai QIDO-RS, WADO-RS, STOW-RS và các dịch vụ worklist trên ASP.NET Core; bạn chỉ cần kết nối kho lưu trữ.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Máy chủ DICOMweb cho .NET C#" h2="Thêm các endpoint QIDO-RS, WADO-RS và STOW-RS vào một dịch vụ ASP.NET Core. Thư viện thực hiện giao thức; mã của bạn quyết định nơi lưu trữ các study." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Máy chủ DICOMweb của riêng bạn bằng C#">}}

<p>Một PACS, VNA, kho lưu trữ đám mây hoặc nền tảng nghiên cứu phải đáp ứng các yêu cầu DICOMweb: tìm kiếm QIDO-RS, tải xuống WADO-RS và tải lên STOW-RS, và thường cả các dịch vụ worklist. Mỗi dịch vụ có các route, media type, multipart body, quy tắc khớp và mã trạng thái riêng.</p>

<p><strong>Aspose.Medical for .NET</strong> đã triển khai phần giao thức này kể từ phiên bản 26.10. Nó đọc mọi yêu cầu, kiểm tra theo chuẩn, khớp các khóa tìm kiếm theo quy tắc DICOM, phân trang kết quả, chuyển đổi các transfer syntax và ghi phản hồi. Mã của bạn thực hiện những việc chỉ bạn có thể làm: lưu một instance, tìm các candidate cho một tìm kiếm và trả về các tệp đã lưu. Các viewer, modality và các kho lưu trữ khác sau đó sẽ làm việc với máy chủ của bạn như với bất kỳ máy chủ DICOMweb nào, và cũng vậy với <a href="/medical/net/dicomweb-client/">DICOMweb client</a> của cùng một thư viện.</p>

<p>Máy chủ được quản lý bằng C#. Nó chạy ở bất kỳ nơi nào ASP.NET Core chạy: trên Windows, Linux và trong container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Một máy chủ hoàn chỉnh trên ASP.NET Core">}}

<p>Cài đặt gói <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Gói này bao gồm Aspose.Medical và kết nối các dịch vụ DICOMweb với dependency injection và endpoint routing. <code>Program.cs</code> này là một máy chủ Studies hoạt động; hai lớp mà nó đăng ký được hiển thị bên dưới.</p>

<div class="codeblock" id="code">
 <h3>Program.cs của một máy chủ DICOMweb - C#</h3>
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

<p>Mọi route Studies hiện trả lời dưới <code>/dicomweb</code>: <code>/dicomweb/studies</code> cho tìm kiếm và tải lên, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> cho truy xuất, và các route cho metadata, frames, bulk data và hình ảnh render. Tài liệu capabilities tại gốc dịch vụ liệt kê chính xác các giao dịch mà máy chủ hỗ trợ.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kết nối kho lưu trữ của bạn">}}

<p>Dịch vụ Studies yêu cầu một interface với ba phương thức: lưu một instance, trả về các tệp đã lưu, và trả về các candidate cho tìm kiếm. Triển khai này giữ mỗi instance dưới dạng file trong một thư mục riêng cho mỗi study và series.</p>

<div class="codeblock" id="code">
 <h3>Kho lưu trữ thư mục phía sau QIDO-RS, WADO-RS và STOW-RS - C#</h3>
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

<p>Thư viện kiểm tra các UID trong mỗi đường dẫn yêu cầu và trong mỗi instance được tải lên trước khi gọi provider. Một instance không có UID hợp lệ sẽ được báo là thất bại và không bao giờ tới <code>StoreAsync</code>.</p>

<p>Một thư mục là đủ để thử máy chủ. Một kho lưu trữ sản xuất vẫn giữ ba phương thức này và đặt một object store phía sau <code>StoreAsync</code> và <code>RetrieveAsync</code> và một chỉ mục cơ sở dữ liệu phía sau <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tìm kiếm: thư viện áp dụng các quy tắc khớp DICOM">}}

<p>Khớp QIDO-RS không chỉ là so sánh chuỗi: ký tự đại diện, phạm vi ngày và giờ, danh sách UID, tên người mờ, giá trị rỗng và các khóa trong sequence. Provider không triển khai bất kỳ phần nào trong số này. Nó trả về các candidate theo thứ tự ổn định, và có thể trả về nhiều hơn số khớp. Thư viện áp dụng khớp DICOM chính xác cho mỗi candidate, giữ các trường được yêu cầu, và áp dụng <code>offset</code> và <code>limit</code>. Một phản hồi mặc định chứa tối đa 1000 kết quả, và <code>DicomWebStandardServiceOptions</code> có thể thay đổi giới hạn đó.</p>

<p>Để giữ tốc độ tìm kiếm nhanh trên một kho lưu trữ lớn, truyền các khóa lựa chọn vào cơ sở dữ liệu. <code>search.Plan.Filter</code> chứa các khóa tìm kiếm đã biên dịch, bao gồm thuộc tính, loại khớp và các giá trị của chúng:</p>

<div class="codeblock" id="code">
 <h3>Đọc Patient ID chính xác từ một tìm kiếm - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Một truy vấn cơ sở dữ liệu được xây dựng theo cách này có thể trả về các hàng thừa, ví dụ khi nó bỏ qua một khóa không thể dịch. Khớp cuối cùng trong thư viện sẽ loại bỏ chúng. Nó không được xóa các khớp đúng, vì thư viện không thể thêm chúng lại.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lưu: kết quả cho mỗi instance">}}

<p>Một yêu cầu STOW-RS có thể chứa hàng trăm instance. Thư viện phân tích multipart body và gọi <code>StoreAsync</code> cho mỗi instance lần lượt, vì vậy toàn bộ quá trình tải lên không bao giờ được giữ trong bộ nhớ đồng thời. Đối với mỗi instance, provider trả về <code>Stored</code>, <code>Warning</code> cho một instance được lưu với các thay đổi, hoặc <code>Failed</code> kèm mã lý do DICOM tùy chọn.</p>

<p>Thư viện tạo báo cáo STOT-RS từ các kết quả này và chọn mã trạng thái: 200 khi mọi instance được lưu, 202 khi một số có cảnh báo hoặc thất bại, 409 khi tất cả thất bại. Các instance trong một yêu cầu cho một study được đặt tên nhưng thuộc study khác sẽ bị từ chối trước khi provider được gọi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Truy xuất: transfer syntax, frames và hình ảnh render">}}

<p>Provider trả về các tệp đã lưu, và thư viện chuyển chúng dưới dạng mà client yêu cầu. Một client WADO-RS có thể yêu cầu một transfer syntax trong header <code>Accept</code>, và thư viện sẽ chuyển đổi mỗi tệp trước khi gửi, với các codec riêng, bao gồm HTJ2K và JPEG XL. Metadata ở dạng DICOM JSON hoặc XML, bulk data và các frame đơn đều đến từ cùng một provider, không cần mã bổ sung.</p>

<p>Hình ảnh render cần một lớp bổ sung, vì chỉ bạn mới quyết định những hình ảnh nào máy chủ có thể tạo ra. Lớp này cung cấp JPEG cho các instance đã render, các frame render và thumbnail, và thư viện sẽ render chúng:</p>

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

<p>Renderer tích hợp tạo JPEG từ một instance đơn-frame hoặc từ các frame mà yêu cầu chọn. Một instance đa-frame đã render toàn bộ, các định dạng hình ảnh khác, và các tham số viewport, window và ICC profile cần một <code>RetrieveAsync</code> do bạn tự triển khai trong lớp này.</p>

<p>Thumbnail được xử lý cẩn thận để bảo vệ quyền riêng tư của bệnh nhân. Renderer tích hợp chỉ tạo thumbnail từ một instance có các thuộc tính Burned In Annotation và Recognizable Visual Features đều là <code>NO</code>, nghĩa là hình ảnh tự khẳng định không hiển thị dữ liệu bệnh nhân. Nhiều hình ảnh không có các thuộc tính này, và đối với chúng máy chủ trả về 406. Nếu kho lưu trữ của bạn cần thumbnail cho các hình ảnh như vậy, hãy tạo chúng trong <code>RetrieveAsync</code> của riêng bạn và đảm bảo chúng ẩn mọi dữ liệu bệnh nhân.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklist, bước quy trình và các dịch vụ khác">}}

<p>Các dịch vụ DICOMweb khác tuân theo mô hình tương tự: đăng ký dịch vụ, triển khai giao diện lưu trữ của nó.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> worklist với <code>AddStandardWorklistService</code>: tạo, tìm kiếm và cập nhật workitem và thay đổi trạng thái của chúng, với các quy tắc trạng thái được thư viện kiểm tra và thông báo sự kiện được gửi qua WebSocket.</li>
<li><a href="/medical/net/modality-worklist/">Modality worklist</a> (MWL-RS) với <code>AddStandardModalityWorklistService</code>, cho phép các modality truy vấn các bước quy trình đã lên lịch trong RIS của bạn.</li>
<li>Modality performed procedure steps (MPPS-RS), storage commitment và các instance không liên quan đến bệnh nhân như color palette và hanging protocol.</li>
<li>Truy xuất WADO-URI cho các viewer cũ, được phục vụ bởi dịch vụ Studies.</li>
</ul>

<p>Khi hành vi chuẩn của một giao dịch không phù hợp, bạn có thể đăng ký handler riêng cho nó bằng <code>AddHandler</code>. Thư viện vẫn sẽ định tuyến và kiểm tra yêu cầu, và các giao dịch khác vẫn giữ các handler chuẩn của chúng.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bảo mật và triển khai vẫn trong kiểm soát của bạn">}}

<p>Các endpoint DICOMweb là các endpoint thông thường của ASP.NET Core. Xác thực, ủy quyền, CORS, TLS, forwarded headers và giới hạn kích thước yêu cầu được cấu hình tương tự như phần còn lại của dịch vụ của bạn:</p>

<div class="codeblock" id="code">
 <h3>Yêu cầu người dùng đã xác thực - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Phần giao thức không phụ thuộc vào ASP.NET Core. <code>DicomWebApplication</code> chuyển một <code>DicomWebRequest</code> thành phản hồi, vì vậy cùng các dịch vụ có thể chạy phía sau một server HTTP khác hoặc trong môi trường test mà không cần mạng.</p>

<div class="codeblock" id="code">
 <h3>Máy chủ tương tự mà không cần ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Client và server từ cùng một thư viện">}}

<p><a href="/medical/net/dicomweb-client/">DICOMweb client</a> của Aspose.Medical kiểm thử máy chủ của bạn từ một unit test hoặc chương trình console, và mỗi file mà máy chủ nhận được là một <code>DicomFile</code> thông thường có thể được ẩn danh, chuyển mã hoặc render trong cùng một tiến trình.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">Hướng dẫn nhà phát triển DICOMweb</a> bao gồm mọi dịch vụ và giao diện lưu trữ, và <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">hướng dẫn ASP.NET Core</a> đề cập đến việc ánh xạ endpoint và WebSockets.</p>

<p>Trang liên quan: <a href="/medical/net/dicom-networking/">Mạng DICOM</a> cho các hệ thống gửi studies qua DIMSE thay vì HTTP, <a href="/medical/net/anonymization/">ẩn danh</a> các study đến, <a href="/medical/net/dicom-transfer-syntax-conversion/">chuyển đổi transfer syntax</a>, và <a href="/medical/net/dicom-to-json/">DICOM sang JSON</a> cho định dạng kết quả QIDO-RS sử dụng.</p>

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
