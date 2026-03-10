---
title: Chuyển đổi DICOM sang JSON trong C# .NET | Aspose.Medical
weight: 4000
description: Chuỗi hoá (serialize) các tệp DICOM sang định dạng DICOM JSON chuẩn trong C# .NET. Cấu hình xử lý số, URI dữ liệu lớn, xuất từ khóa, và xử lý luồng không đồng bộ với Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Chuyển đổi DICOM sang JSON trong .NET C#" h2="Chuỗi hoá các bộ dữ liệu và tệp DICOM sang Mô hình DICOM JSON chuẩn (PS3.18). Cấu hình xử lý số, tham chiếu dữ liệu lớn, xuất từ khóa, và xử lý bất đồng bộ dựa trên luồng." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Mô hình DICOM JSON Chuẩn">}}

<p><strong>Aspose.Medical for .NET</strong> chuỗi hoá dữ liệu DICOM sang JSON dựa trên <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">Mô hình DICOM PS3.18 JSON</a>. Đây là tiêu chuẩn chính thức để biểu diễn các bộ dữ liệu DICOM dưới dạng JSON, được sử dụng bởi các dịch vụ DICOMweb, tài nguyên FHIR ImagingStudy và các API chăm sóc sức khỏe hiện đại.</p>

<p>Trong Mô hình DICOM JSON, mỗi thuộc tính được biểu diễn bằng thẻ của nó làm khóa JSON (ví dụ, <code>"00100010"</code> cho Patient Name), với Value Representation và mảng giá trị được lồng dưới dạng thuộc tính:</p>

<div class="codeblock" id="code">
 <h3>Định dạng mô hình DICOM JSON</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Chuỗi hoá DICOM sang JSON trong C#">}}

<p>Sử dụng lớp <code>DicomJsonSerializer</code> để chuyển đổi một tệp DICOM hoặc bộ dữ liệu sang JSON. Cách tiếp cận đơn giản nhất tạo ra đầu ra gọn gàng, tuân thủ tiêu chuẩn:</p>

<div class="codeblock" id="code">
 <h3>Chuyển đổi DICOM sang JSON - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p>Bạn có thể chuỗi hoá một <code>Dataset</code>, một <code>DicomFile</code> hoàn chỉnh (bao gồm File Meta Information), hoặc một mảng các bộ dữ liệu:</p>

<div class="codeblock" id="code">
 <h3>Chuỗi hoá DicomFile và mảng bộ dữ liệu - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Chuỗi hoá dựa trên luồng và bất đồng bộ">}}

<p>Đối với các tệp DICOM lớn hoặc các trường hợp truyền tải cao, chuỗi hoá trực tiếp tới một luồng UTF-8 để tránh cấp phát các chuỗi lớn trong bộ nhớ. Cả phương pháp đồng bộ và bất đồng bộ đều có sẵn:</p>

<div class="codeblock" id="code">
 <h3>Chuỗi hoá luồng và bất đồng bộ - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tuỳ chọn chuỗi hoá">}}

<p>Lớp <code>DicomJsonSerializerOptions</code> kiểm soát cách dữ liệu DICOM được biểu diễn trong JSON:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Thuộc tính</th>
<th>Kiểu</th>
<th>Mô tả</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Kiểm soát việc các VR số (IS, DS, SV, UV) được ghi dưới dạng số JSON hay chuỗi</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Sử dụng các từ khóa DICOM (ví dụ, <code>"PatientName"</code>) thay vì thẻ (<code>"00100010"</code>) làm khóa JSON. Lưu ý: không chuẩn</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Bao gồm từ khóa DICOM như một thuộc tính JSON bổ sung</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Bao gồm tên thẻ DICOM như một thuộc tính JSON bổ sung</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Bộ chuyển đổi tùy chỉnh để ghi dữ liệu lớn (ví dụ, dữ liệu pixel) dưới dạng tham chiếu URI</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Bộ tải tùy chỉnh để giải quyết các URI dữ liệu lớn trong quá trình giải mã</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Cấu hình tuỳ chọn chuỗi hoá - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Xử lý số">}}

<p>Value Representation số của DICOM (IS, DS, SV, UV) có thể được chuỗi hoá dưới dạng số JSON hoặc chuỗi JSON. Điều này quan trọng cho khả năng tương tác, vì một số triển khai DICOM lưu số có khoảng trắng đầu hoặc định dạng không chuẩn:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Chế độ</th>
<th>Kết quả JSON</th>
<th>Trường hợp sử dụng</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>Tuân thủ tiêu chuẩn, lý tưởng cho tính toán. Ném ngoại lệ nếu giá trị không thể phân tích.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>Giữ nguyên biểu diễn chuỗi gốc. An toàn hơn cho việc vòng lại dữ liệu không chuẩn.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Xử lý dữ liệu lớn">}}

<p>Các giá trị nhị phân lớn (dữ liệu pixel, waveform, tài liệu được đóng gói) có thể được xử lý như các tham chiếu dữ liệu lớn thay vì nhúng trong đầu ra JSON. Điều này tuân theo quy định <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>.</p>

<p>Triển khai <code>IBulkDataConverter</code> để tách dữ liệu lớn ra ngoài trong quá trình chuỗi hoá, và <code>IBulkDataLoader</code> để giải quyết các URI trong quá trình giải mã:</p>

<div class="codeblock" id="code">
 <h3>Xử lý dữ liệu lớn tùy chỉnh - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Giải mã JSON thành DICOM">}}

<p>Phân tích DICOM JSON trở lại thành các đối tượng Dataset hoặc DicomFile. Hỗ trợ đầu vào kiểu chuỗi, đầu vào luồng, và các thao tác luồng bất đồng bộ:</p>

<div class="codeblock" id="code">
 <h3>Giải mã JSON thành DICOM - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tích hợp System.Text.Json">}}

<p>Aspose.Medical cung cấp <code>DatasetJsonConverter</code> và <code>DicomFileJsonConverter</code> để tích hợp với pipeline chuỗi hoá chuẩn của <code>System.Text.Json</code>. Điều này cho phép các đối tượng DICOM được chuỗi hoá cùng với các kiểu .NET khác trong mã xử lý JSON hiện có của bạn:</p>

<div class="codeblock" id="code">
 <h3>Tích hợp System.Text.Json - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
</div>

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
