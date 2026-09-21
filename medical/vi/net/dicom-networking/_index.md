---
title: Mạng DICOM trong C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Kết nối ứng dụng .NET của bạn tới PACS. Xác thực bằng C-ECHO, gửi hình ảnh bằng C-STORE, truy vấn bằng C-FIND và nhận hình ảnh với SCP riêng của bạn. Một client và server DIMSE bằng C# thuần túy.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Mạng DICOM trong .NET C#" h2="Giao tiếp với PACS từ ứng dụng của bạn: C-ECHO, C-STORE, C-FIND, C-MOVE và C-GET, như một client và một server, trong C# quản lý mà không cần cài đặt gì trên máy." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Kết nối tới PACS từ mã của bạn">}}

<p>Đọc các tệp DICOM là phần dễ dàng của hình ảnh y tế. Ngay khi ứng dụng của bạn cần làm việc với hệ thống bệnh viện thực, nó phải giao tiếp bằng DIMSE: mở một association với PACS, gửi hình ảnh, hỏi có những study nào, và trả lời khi hệ thống khác gửi dữ liệu trở lại.</p>

<p><strong>Aspose.Medical for .NET</strong> cung cấp giao thức đó như một phần của thư viện. <code>Aspose.Medical.Dicom.Network</code> cung cấp cho bạn một client DIMSE và một server DIMSE, cả hai được viết bằng C# quản lý. Không có bộ công cụ native nào cần cài đặt, không có dịch vụ nào cần cấu hình và không phụ thuộc vào nền tảng, vì vậy cùng một mã có thể chạy trên Windows, Linux và trong container.</p>

<p>Ba thao tác bao phủ hầu hết các tích hợp, và mỗi thao tác chỉ cần vài dòng mã: kiểm tra kết nối bằng C-ECHO, đẩy hình ảnh bằng C-STORE, và tìm những gì ở phía đối tác bằng C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bắt đầu với C-ECHO">}}

<p>C-ECHO là lệnh ping của DICOM. Nó chứng minh rằng host, port và hai tiêu đề AE đều đúng trước khi có bất kỳ lỗi nào khác. Tạo một client một lần, cung cấp một handler để quan sát câu trả lời, và gửi yêu cầu.</p>

<div class="codeblock" id="code">
 <h3>Xác minh kết nối bằng C-ECHO - C#</h3>
 <pre><code class="cs">AssociationNegotiationOptions negotiation = new AssociationNegotiationOptions()
    .WithPresentationContext(new PresentationContext
    {
        AbstractSyntax = Uid.Verification,
        Role = null,
        TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
    });

DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = negotiation
    })
    .AddCEchoHandler((request, response, cancellationToken) =>
    {
        Console.WriteLine($"C-ECHO answered with status 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(new CEchoRequest());

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Trạng thái <code>0x0000</code> nghĩa là thành công. Các yêu cầu được xếp hàng và sau đó gửi trên một association, vì vậy một lô công việc không mở kết nối cho mỗi mục.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gửi hình ảnh bằng C-STORE">}}

<p>C-STORE là hành động mà một ứng dụng thực hiện sau khi tạo hoặc nhận một hình ảnh: nó đẩy instance vào kho lưu trữ. Xếp một yêu cầu cho mỗi instance và gửi chúng cùng nhau.</p>

<div class="codeblock" id="code">
 <h3>Gửi tệp DICOM tới PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Các presentation context mà bạn đề xuất quyết định archive sẽ chấp nhận gì. Nếu nó muốn một syntax nén, hãy chuyển mã trước khi gửi, như trang <a href="/medical/net/dicom-transfer-syntax-conversion/">chuyển đổi transfer syntax</a> mô tả, hoặc liệt kê các lựa chọn thay thế trong <code>AdditionalTransferSyntaxes</code> và để quá trình đàm phán chọn một.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tìm study bằng C-FIND">}}

<p>C-FIND trả lời câu hỏi "archive có gì". Các kết quả xuất hiện từng cái một, mỗi cái có dataset định danh riêng, và phản hồi cuối cùng sẽ đóng truy vấn.</p>

<div class="codeblock" id="code">
 <h3>Truy vấn study theo bệnh nhân - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.StudyRootQueryRetrieveInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddCFindHandler((request, response, cancellationToken) =>
    {
        // A match arrives with an identifier; the final response carries the status only
        if (response.Identifier is not null)
            Console.WriteLine(response.Identifier.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty));

        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(CFindRequest.CreateStudyQuery(
    patientId: "PATIENT-001",
    patientName: null,
    studyDateTime: null,
    accession: null,
    studyId: null,
    modalitiesInStudy: null,
    studyInstanceUid: null,
    priority: DimsePriority.Medium));

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Factory giống nhau tạo các mức truy vấn khác: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> và <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> tạo một truy vấn worklist cho modality, đây là truy vấn mà một modality gửi trước khi thực hiện quét.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nhận hình ảnh: SCP lưu trữ của riêng bạn">}}

<p>Thư viện cũng hoạt động như một server. Đăng ký một handler cho dịch vụ bạn muốn cung cấp, bắt đầu lắng nghe, và ứng dụng của bạn trở thành một node DICOM mà một modality hoặc một PACS khác có thể gửi đến.</p>

<div class="codeblock" id="code">
 <h3>Chấp nhận hình ảnh đến - C#</h3>
 <pre><code class="cs">DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Any, 11112)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.SecondaryCaptureImageStorage,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddSingletonCStoreHandler(new StoreHandler())
    .Build();

await server.StartAsync(CancellationToken.None);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Handler lưu trữ dữ liệu nhận được - C#</h3>
 <pre><code class="cs">public sealed class StoreHandler : ICStoreRequestHandler
{
    public ValueTask&lt;CStoreResponse&gt; Handle(CStoreRequest request, CancellationToken cancellationToken)
    {
        // Write what arrived, then answer Success
        new DicomFile(request.Dataset).Save($"{request.AffectedSopInstanceUid}.dcm");

        CStoreResponse response = new();
        response.Command.AddOrUpdate(Tag.Status, (ushort)0x0000);
        return ValueTask.FromResult(response);
    }
}</code></pre>
</div>

<p>Việc handler xử lý dataset phụ thuộc vào quyết định của bạn: ghi nó vào đĩa, đặt vào hàng đợi, ẩn danh trước bằng <a href="/medical/net/anonymization/">API ẩn danh</a>, hoặc chuyển mã nó sang syntax của archive.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các tính năng khác mà API mạng hỗ trợ">}}

<p>Ba dịch vụ trên là những dịch vụ phổ biến. Phần còn lại của DIMSE cũng có sẵn:</p>

<ul>
<li>Truy xuất: C-MOVE và C-GET, với các bộ đếm sub-operation được báo cáo trong các phản hồi.</li>
<li>Các N-service: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE và N-EVENT-REPORT, là nền tảng cho storage commitment và MPPS.</li>
<li>Kiểm soát association: presentation contexts, vai trò service class, đàm phán mở rộng, cửa sổ hoạt động bất đồng bộ, đàm phán danh tính người dùng, và hook chính sách có thể từ chối một association bằng cách gọi AE title.</li>
<li>TLS ở cả hai phía, thông qua <code>TlsInitiatorAuthenticator</code> và <code>TlsAcceptorAuthenticator</code>, với việc xác thực chứng chỉ của bạn nếu cần.</li>
<li>Timeout cho mọi giai đoạn, từ kết nối TCP đến giải phóng, và thông báo về vòng đời association để một node chạy lâu có thể ghi lại những gì đã xảy ra.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">Hướng dẫn mạng DICOM</a> ghi chép mọi tùy chọn và mọi handler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2=".NET thuần túy, từ socket tới dữ liệu pixel">}}

<p>Tất cả nội dung trên trang này là mã quản lý từ cùng một gói dùng để đọc và ghi tệp. Association, các codec và parser đều đến từ một thư viện, vì vậy một study nhận được qua mạng có thể được ẩn danh, chuyển mã hoặc tuần tự hoá mà không rời khỏi tiến trình và không có phụ thuộc native nào trong chuỗi.</p>

<p>Trang liên quan: <a href="/medical/net/dicom-transfer-syntax-conversion/">chuyển đổi transfer syntax</a> để biết gửi gì, <a href="/medical/net/anonymization/">ẩn danh</a> để biết cần loại bỏ gì trước, và <a href="/medical/net/dicom-tags/">thẻ DICOM</a> để đọc những gì đã đến.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài Nguyên Học Tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài Liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Hướng Dẫn Nhà Phát Triển" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
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
