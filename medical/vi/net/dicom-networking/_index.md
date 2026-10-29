---
title: Mạng DICOM trong C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Kết nối ứng dụng .NET của bạn tới một PACS. Xác minh bằng C-ECHO, gửi hình ảnh bằng C-STORE, truy vấn bằng C-FIND, và nhận hình ảnh bằng SCP riêng của bạn. Một client và server DIMSE hoàn toàn bằng C#.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Mạng DICOM trong .NET C#" h2="Giao tiếp với PACS từ ứng dụng của bạn: C-ECHO, C-STORE, C-FIND, C-MOVE và C-GET, dưới vai trò client và server, bằng C# quản lý mà không cần cài đặt gì trên máy." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Kết nối tới PACS từ mã của bạn">}}

<p>Đọc các tệp DICOM là nửa công việc dễ dàng trong hình ảnh y tế. Ngay khi ứng dụng của bạn phải làm việc với hệ thống bệnh viện thực, nó phải giao tiếp bằng DIMSE: mở một association với PACS, gửi hình ảnh, hỏi có những study nào, và trả lời khi hệ thống khác gửi dữ liệu trở lại.</p>

<p><strong>Aspose.Medical for .NET</strong> cung cấp giao thức đó như một phần của thư viện. <code>Aspose.Medical.Dicom.Network</code> cung cấp cho bạn một client DIMSE và một server DIMSE, cả hai đều được viết bằng C# quản lý. Không có bộ công cụ native nào cần cài đặt, không có dịch vụ nào cần cấu hình và không phụ thuộc vào nền tảng, vì vậy cùng một mã chạy trên Windows, Linux và trong container.</p>

<p>Ba điều bao phủ hầu hết các tích hợp, và mỗi điều chỉ mất vài dòng: kiểm tra kết nối bằng C-ECHO, đẩy hình ảnh bằng C-STORE, và tìm những gì ở phía bên kia bằng C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bắt đầu với C-ECHO">}}

<p>C-ECHO là lệnh ping DICOM. Nó chứng minh rằng host, cổng và hai AE title là đúng trước khi có bất kỳ lỗi nào khác. Tạo một client một lần, cung cấp cho nó một handler để quan sát câu trả lời, và gửi yêu cầu.</p>

<div class="codeblock" id="code">
 <h3>Xác minh kết nối với C-ECHO - C#</h3>
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

<p>Trạng thái <code>0x0000</code> có nghĩa là thành công. Các yêu cầu được xếp hàng và sau đó gửi trên một association, vì vậy một loạt công việc không mở kết nối cho mỗi mục.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gửi hình ảnh bằng C-STORE">}}

<p>C-STORE là những gì một ứng dụng thực hiện sau khi đã tạo hoặc nhận được một hình ảnh: nó đẩy instance tới kho lưu trữ. Xếp một yêu cầu cho mỗi instance và gửi chúng cùng nhau.</p>

<div class="codeblock" id="code">
 <h3>Gửi tệp DICOM tới PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Các presentation context bạn đề xuất quyết định những gì kho lưu trữ sẽ chấp nhận. Nếu nó muốn một syntax nén, hãy chuyển mã trước khi gửi, như trang <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> trình bày, hoặc liệt kê các lựa chọn thay thế trong <code>AdditionalTransferSyntaxes</code> và để quá trình thương lượng chọn một.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tìm các study bằng C-FIND">}}

<p>C-FIND trả lời câu hỏi "kho lưu trữ có gì". Các kết quả đến từng cái một, mỗi cái có dataset định danh riêng, và phản hồi cuối cùng đóng truy vấn.</p>

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

<p>Cùng một factory tạo các mức truy vấn khác: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> và <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> xây dựng một truy vấn modality worklist, là truy vấn mà modality thực hiện trước khi quét; trang <a href="/medical/net/modality-worklist/">modality worklist page</a> hiển thị nó cùng với MPPS và phía server worklist.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nhận hình ảnh: SCP lưu trữ của bạn">}}

<p>Thư viện cũng là một server. Đăng ký một handler cho dịch vụ bạn muốn cung cấp, bắt đầu lắng nghe, và ứng dụng của bạn trở thành một nút DICOM mà một modality hoặc PACS khác có thể gửi tới.</p>

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
 <h3>Handler lưu trữ những gì đến - C#</h3>
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

<p>Việc handler xử lý dataset tùy thuộc vào quyết định của bạn: ghi ra đĩa, đưa vào hàng đợi, ẩn danh trước bằng <a href="/medical/net/anonymization/">anonymization API</a>, hoặc chuyển mã sang syntax của kho lưu trữ.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các tính năng khác mà API mạng bao phủ">}}

<p>Ba dịch vụ ở trên là những dịch vụ phổ biến. Phần còn lại của DIMSE cũng có sẵn:</p>

<ul>
<li>Khôi phục: C-MOVE và C-GET, với bộ đếm sub-operation được báo cáo trong phản hồi.</li>
<li>Các N-service: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE và N-EVENT-REPORT, là nền tảng cho storage commitment và MPPS.</li>
<li>Kiểm soát association: presentation contexts, service class roles, extended negotiation, cửa sổ asynchronous operations, user identity negotiation, và hook chính sách có thể từ chối một association bằng cách gọi AE title.</li>
<li>TLS ở cả hai phía, thông qua <code>TlsInitiatorAuthenticator</code> và <code>TlsAcceptorAuthenticator</code>, với việc xác thực chứng chỉ tự định nếu cần.</li>
<li>Timeout cho mỗi giai đoạn, từ kết nối TCP đến release, và thông báo cho vòng đời association để một node chạy lâu có thể ghi lại những gì đã xảy ra.</li>
</ul>

<p>Hướng dẫn <a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">DICOM networking</a> ghi chép mọi tùy chọn và mỗi handler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2=".NET thuần, từ socket tới dữ liệu pixel">}}

<p>Mọi thứ trên trang này đều là mã quản lý từ cùng một gói đọc và ghi tệp. Association, codec và parser đến từ một thư viện, vì vậy một study nhận qua mạng có thể được ẩn danh, chuyển mã hoặc tuần tự hoá mà không rời khỏi tiến trình và không cần phụ thuộc native ở bất kỳ đâu trong chuỗi.</p>

<p>Trang liên quan: <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> để biết gì gửi, <a href="/medical/net/anonymization/">anonymization</a> để biết gì cần loại bỏ trước, và <a href="/medical/net/dicom-tags/">DICOM tags</a> để đọc những gì đã đến.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Hướng dẫn nhà phát triển" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="Tham chiếu API" href="https://reference.aspose.com/medical/net/" >}}
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
