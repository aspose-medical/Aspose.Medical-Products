---
title: Chuyển đổi XML sang DICOM trong C# .NET | Aspose.Medical
weight: 5000

description: Tạo tệp DICOM từ XML Model DICOM Gốc của PS3.19 trong C# .NET. Đọc XML từ chuỗi, luồng hoặc pipe, luồng các tài liệu liên tiếp, và giải quyết các tham chiếu dữ liệu hàng loạt bằng API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Chuyển đổi XML sang DICOM trong .NET C#" h2="Đọc XML Model DICOM Gốc của PS3.19 trở lại thành các dataset và tệp DICOM. Làm việc từ chuỗi, luồng hoặc pipe, luồng các tài liệu liên tiếp, và giải quyết các tham chiếu dữ liệu hàng loạt." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="XML Model DICOM Gốc chuẩn">}}

<p><strong>Aspose.Medical cho .NET</strong> đọc <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a> được định nghĩa trong DICOM PS3.19. Đây là biểu diễn XML được ghi vào chuẩn gốc, không phải định dạng do Aspose tạo ra, điều này khiến nó hữu ích cho việc tích hợp: một hệ thống đã trao đổi DICOM dưới dạng XML sẽ tạo ra các tài liệu mà thư viện này chấp nhận.</p>

<p>Gốc của tài liệu là <code>NativeDicomModel</code>, và mỗi thuộc tính là một phần tử <code>DicomAttribute</code> chứa thẻ, biểu diễn giá trị và từ khóa:</p>

<div class="codeblock" id="code">
 <h3>Định dạng Native DICOM Model</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>Trang này là hướng ngược lại của <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>, và cả hai đều sử dụng cùng một lớp, <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tạo tệp DICOM từ XML trong C#">}}

<p><code>Deserialize</code> chuyển một tài liệu thành <code>Dataset</code>, và một dataset được ghi vào đĩa dưới dạng tệp DICOM.</p>

<div class="codeblock" id="code">
 <h3>Tạo tệp DICOM từ XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Native DICOM Model không có nhóm File Meta Information, vì vậy transfer syntax không phải là một phần của tài liệu. Một dataset được bao bọc trong <code>DicomFile</code> được ghi với transfer syntax mặc định, Implicit VR Little Endian. Để lưu tệp với một transfer syntax khác, hãy chuyển mã nó, như trang <a href="/medical/net/dicom-transfer-syntax-conversion/">chuyển đổi transfer syntax</a> chỉ ra.</p>

<p>Đọc DICOM XML là tính năng có bản quyền. Nếu không áp dụng giấy phép tại chỗ, trình đọc sẽ ném một <code>MedicalApiException</code>, vì vậy hãy áp dụng giấy phép trước, như hướng dẫn trong <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">hướng dẫn cấp phép</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Luồng, pipe và bất đồng bộ">}}

<p>Mỗi điểm vào đều có một overload cho stream và một overload bất đồng bộ, và các overload bất đồng bộ cũng chấp nhận <code>PipeReader</code>. Một tài liệu đến từ phản hồi web được phân tích khi đang đọc, mà không cần chuyển thành chuỗi trước.</p>

<div class="codeblock" id="code">
 <h3>Đọc XML từ stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tài liệu liên tiếp trong một stream">}}

<p>Một xuất khẩu từ hệ thống khác thường chứa một phần tử <code>NativeDicomModel</code> tiếp theo sau một phần tử khác trong một stream duy nhất. <code>DeserializeAsyncEnumerable</code> trả về một dataset cho mỗi phần tử, theo thứ tự nhập, vì vậy stream được xử lý mà không phải lưu toàn bộ trong bộ nhớ. Các phần tử liên tiếp nhau trực tiếp: một khai báo XML chỉ được phép ở đầu tiên, như trong bất kỳ đầu vào XML nào.</p>

<div class="codeblock" id="code">
 <h3>Stream các tài liệu liên tiếp - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tham chiếu dữ liệu hàng loạt">}}

<p>Giá trị lớn như dữ liệu pixel không được ghi inline. Chúng xuất hiện dưới dạng một phần tử <code>BulkData</code> với URI chỉ tới các byte, giúp tài liệu giữ kích thước nhỏ. Để giải quyết các tham chiếu này khi đọc, cung cấp cho serializer một bulk data loader. <code>DefaultBulkDataLoader</code> truy xuất các URI <code>file</code>, <code>http</code> và <code>https</code> mà không cần xác thực; đối với kho lưu trữ cần thông tin đăng nhập, hãy tự triển khai <code>IBulkDataLoader</code> hoặc <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Giải quyết dữ liệu hàng loạt khi đọc - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Quy trình vòng lại với DICOM sang XML">}}

<p>Hai hướng này được thiết kế để sử dụng cùng nhau: một study xuất ra dưới dạng XML, đi qua một hệ thống giao tiếp bằng XML, và quay trở lại dưới dạng tệp DICOM. Tất cả được quản lý bằng .NET, nên vòng đi lại này hoạt động trên Windows, Linux và macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM sang XML và quay lại - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Đối với các tùy chọn kiểm soát cấu trúc XML, xem trang <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>. Cặp tương tự cũng có cho JSON: <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> và <a href="/medical/net/json-to-dicom/">JSON to DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">Hướng dẫn serialization</a> bao phủ toàn bộ API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Hướng dẫn nhà phát triển" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="Tài liệu tham chiếu API" href="https://reference.aspose.com/medical/net/" >}}
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