---
title: Chuyển đổi DICOM sang XML trong C# .NET | Aspose.Medical
weight: 3000
description: Serial hóa các tập dữ liệu DICOM sang định dạng DICOM XML chuẩn trong C# .NET. Cấu hình xử lý dữ liệu bulk, xử lý dựa trên luồng, và các thao tác bất đồng bộ với API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Chuyển đổi DICOM sang XML trong .NET C#" h2="Serial hóa các tập dữ liệu DICOM sang biểu diễn DICOM XML chuẩn (PS3.19). Cấu hình tham chiếu dữ liệu bulk, xuất dựa trên luồng và xử lý bất đồng bộ với thư viện .NET thuần." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Serial hóa DICOM XML dựa trên tiêu chuẩn">}}

<p><strong>Aspose.Medical for .NET</strong> chuyển đổi dữ liệu DICOM sang XML theo <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Mô hình DICOM PS3.19 Native DICOM</a>. Đây là tiêu chuẩn chính thức để biểu diễn các tập dữ liệu DICOM dưới dạng XML, được sử dụng bởi các dịch vụ DICOMweb, nền tảng tích hợp, và các hệ thống yêu cầu biểu diễn siêu dữ liệu hình ảnh y tế có thể đọc được bởi con người và được xác thực theo schema.</p>

<p>Class <code>DicomXmlSerializer</code> cung cấp các phương thức static cho cả việc serial hóa và deserial hóa. Không giống như các cách dump thẻ đơn giản, đầu ra tuân theo schema DICOM XML, trong đó mỗi phần tử được biểu diễn bằng tag, VR và các giá trị được định dạng chính xác &mdash; cho phép chuyển đổi vòng tròn không mất dữ liệu giữa DICOM nhị phân và XML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serial hóa DICOM sang XML trong C#">}}

<p>Sử dụng class <code>DicomXmlSerializer</code> để chuyển đổi một tập dữ liệu DICOM thành chuỗi XML. Cách đơn giản nhất tạo ra một tài liệu XML tuân thủ tiêu chuẩn:</p>

<div class="codeblock" id="code">
 <h3>Chuyển đổi DICOM sang XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serial hóa dựa trên luồng và bất đồng bộ">}}

<p>Đối với các tệp DICOM lớn hoặc các kịch bản truyền tải cao, thực hiện serial hóa trực tiếp tới một stream để tránh cấp phát các chuỗi lớn trong bộ nhớ. Cả hai phương thức đồng bộ và bất đồng bộ đều có sẵn:</p>

<div class="codeblock" id="code">
 <h3>Serial hóa stream đồng bộ - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Serial hóa stream bất đồng bộ - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Luồng pipeline cho các Study lớn">}}

<p>Các study toàn bộ không cần phải được giữ trong bộ nhớ. <code>DicomXmlSerializer</code> ghi vào một <code>PipeWriter</code> và đọc từ một <code>PipeReader</code>, do đó XML có thể được tạo ra và tiêu thụ khi nó chảy, và một chuỗi các dataset có thể được đọc một lần một dataset thông qua <code>DeserializeAsyncEnumerable</code>. Mỗi phương thức đều nhận một <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Serial và deserialize qua pipe - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Đọc một chuỗi các dataset từng cái một - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tùy chọn Serial hóa">}}

<p>Class <code>DicomXmlSerializerOptions</code> điều khiển cách dữ liệu DICOM được biểu diễn trong XML. Cấu hình chính liên quan tới việc xử lý bulk data cho các giá trị nhị phân lớn:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Thuộc tính</th>
<th>Kiểu</th>
<th>Mô tả</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Trình chuyển đổi tùy chỉnh để ghi dữ liệu lớn (ví dụ, dữ liệu pixel) dưới dạng tham chiếu URI BulkData thay vì nội suy</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Trình tải tùy chỉnh để giải quyết các URI BulkData trong quá trình deserialization</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Đối tượng tùy chọn mặc định được sử dụng khi không cung cấp tùy chọn tùy chỉnh</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Serial với tùy chọn tùy chỉnh - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Xử lý Bulk Data">}}

<p>Giá trị nhị phân lớn (dữ liệu pixel, waveform, tài liệu đóng gói) có thể được đưa ra ngoài dưới dạng tham chiếu URI BulkData thay vì được nội suy trong đầu ra XML. Điều này tuân theo tiêu chuẩn <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">phần tử BulkData DICOM PS3.19</a>.</p>

<p>Triển khai <code>IBulkDataConverter</code> để đưa dữ liệu lớn ra ngoài trong quá trình serial hóa, và <code>IBulkDataLoader</code> để giải quyết các URI trong quá trình deserialization. Đối với các trường hợp phổ biến, không cần viết trình tải nào: <code>DefaultBulkDataLoader.Instance</code> giải quyết các URI <code>file</code>, <code>http</code> và <code>https</code>, đồng thời nó cũng triển khai <code>IAsyncBulkDataLoader</code>, vì vậy bulk data được gọi bất đồng bộ trên các đường truyền streaming.</p>

<div class="codeblock" id="code">
 <h3>Xử lý bulk data tùy chỉnh - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
public class FileBulkDataConverter : IBulkDataConverter
{
    public string? GetBulkDataUri(IElement element)
    {
        // Externalize pixel data to a separate file
        if (element.Tag == Tag.PixelData)
            return "file:///bulk/pixeldata.raw";
        return null; // inline all other elements
    }
}

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Deserial XML sang DICOM">}}

<p>Phân tích DICOM XML lại thành các đối tượng Dataset. Hỗ trợ đầu vào dạng chuỗi, đầu vào dạng stream và các thao tác bất đồng bộ:</p>

<div class="codeblock" id="code">
 <h3>Deserial XML sang DICOM - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serial hóa XML vs JSON">}}

<p>Aspose.Medical hỗ trợ cả serial hóa DICOM XML (PS3.19) và DICOM JSON (PS3.18). Cả hai định dạng đều cung cấp chuyển đổi vòng tròn không mất dữ liệu, nhưng phục vụ các kịch bản tích hợp khác nhau:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tính năng</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Tiêu chuẩn</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>Kiểm tra schema</td><td>Có sẵn XML Schema (XSD)</td><td>Không có schema chính thức</td></tr>
<tr><td>Thích hợp cho</td><td>Tích hợp doanh nghiệp, HL7 CDA, log kiểm toán, đăng ký XDS</td><td>DICOMweb, REST APIs, FHIR ImagingStudy</td></tr>
<tr><td>Độ dễ đọc cho con người</td><td>Chi tiết nhưng tự mô tả</td><td>Ngắn gọn và được hỗ trợ rộng rãi</td></tr>
<tr><td>Dữ liệu bulk</td><td>Phần tử BulkData với URI</td><td>Thuộc tính BulkDataURI</td></tr>
<tr><td>Lớp Serializer</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Mã nguồn" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
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
