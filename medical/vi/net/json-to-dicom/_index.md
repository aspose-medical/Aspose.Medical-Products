---
title: Chuyển đổi JSON sang DICOM trong C# .NET | Aspose.Medical
weight: 6000

description: Xây dựng các tệp DICOM từ Mô hình JSON DICOM tiêu chuẩn (PS3.18) trong C# .NET. Đọc JSON từ chuỗi, luồng hoặc pipe, truyền một dãy dataset, và giải quyết các tham chiếu dữ liệu bulk bằng API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Chuyển đổi JSON sang DICOM trong .NET C#" h2="Đọc Mô hình JSON DICOM tiêu chuẩn (PS3.18) trở lại thành các dataset và tệp DICOM. Làm việc từ chuỗi, luồng hoặc pipe, truyền một dãy study, và giải quyết các tham chiếu dữ liệu bulk." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Từ DICOM JSON sang tệp DICOM">}}

<p><strong>Aspose.Medical cho .NET</strong> đọc <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">Mô hình JSON DICOM PS3.18</a>, dạng biểu diễn được các dịch vụ DICOMweb và các hệ thống trao đổi study qua HTTP sử dụng. Những gì nhận được dưới dạng JSON sẽ trở thành một <code>Dataset</code>, và một <code>Dataset</code> được ghi vào đĩa dưới dạng tệp DICOM.</p>

<p>Đây là hướng ngược lại của trang <a href="/medical/net/dicom-to-json/">DICOM sang JSON</a>, và cả hai đều sử dụng cùng một lớp, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Tạo tệp DICOM từ JSON - C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Một dataset không chứa File Meta Information sẽ được ghi với giao thức truyền tải mặc định, Implicit VR Little Endian, khi nó được gói trong một <code>DicomFile</code>.</p>

<p>Đọc DICOM JSON là tính năng có giấy phép. Nếu không áp dụng giấy phép tại chỗ, trình đọc sẽ ném ra <code>MedicalApiException</code>, vì vậy hãy áp dụng giấy phép trước, như hướng dẫn <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">cấp phép</a> mô tả.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Giữ lại File Meta Information">}}

<p><code>Deserialize</code> trả về chỉ dataset. Khi tài liệu JSON cũng chứa nhóm File Meta Information, ví dụ vì nó được tạo từ một tệp DICOM hoàn chỉnh, <code>DeserializeFile</code> trả về một <code>DicomFile</code> với nhóm đó nguyên vẹn, bao gồm giao thức truyền tải mà tệp khai báo.</p>

<div class="codeblock" id="code">
 <h3>Đọc một tệp DICOM hoàn chỉnh từ JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Luồng, pipe và async">}}

<p>Mỗi điểm vào đều có phiên bản overload cho stream và phiên bản bất đồng bộ, và các phiên bản bất đồng bộ cũng chấp nhận <code>PipeReader</code>. Một tài liệu nhận được từ phản hồi web hoặc từ đĩa sẽ được đọc mà không cần chuyển thành chuỗi trước, điều này quan trọng khi JSON chứa dữ liệu pixel.</p>

<div class="codeblock" id="code">
 <h3>Đọc JSON từ stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Một dãy dataset, từng cái một">}}

<p>Một truy vấn DICOMweb trả lời bằng một mảng các dataset, và tài liệu như vậy có thể rất lớn. <code>DeserializeList</code> đọc toàn bộ mảng vào bộ nhớ; <code>DeserializeAsyncEnumerable</code> trả về một dataset mỗi lần, vì vậy tài liệu không bao giờ được giữ toàn bộ trong bộ nhớ.</p>

<div class="codeblock" id="code">
 <h3>Phát luồng một mảng các dataset - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tham chiếu dữ liệu bulk">}}

<p>Mô hình JSON DICOM không chứa dữ liệu pixel trực tiếp. Các giá trị lớn được thay thế bằng một <code>BulkDataURI</code> trỏ tới các byte, giúp tài liệu JSON nhỏ gọn. Để giải quyết các tham chiếu này khi đọc, cung cấp cho serializer một bộ tải dữ liệu bulk. <code>DefaultBulkDataLoader</code> truy xuất các URI <code>file</code>, <code>http</code> và <code>https</code> mà không cần xác thực; đối với kho lưu trữ cần thông tin đăng nhập, bạn tự triển khai <code>IBulkDataLoader</code> hoặc <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Giải quyết BulkDataURI khi đọc - C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Quá trình vòng lại với DICOM sang JSON">}}

<p>Hai hướng này được thiết kế để sử dụng cùng nhau: một study xuất ra dưới dạng JSON, truyền qua dịch vụ web, và trở lại dưới dạng tệp DICOM. Không có thành phần nào trong quá trình phụ thuộc vào mã gốc, do đó vòng lại này chạy được trên Windows, Linux và macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM sang JSON và quay lại - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Đối với các tùy chọn kiểm soát định dạng JSON, xem trang <a href="/medical/net/dicom-to-json/">DICOM sang JSON</a>. Cặp tương tự cũng tồn tại cho XML: <a href="/medical/net/dicom-to-xml/">DICOM sang XML</a> và <a href="/medical/net/xml-to-dicom/">XML sang DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">Hướng dẫn tuần tự hoá JSON</a> bao phủ toàn bộ API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Hướng dẫn nhà phát triển" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="Tham khảo API" href="https://reference.aspose.com/medical/net/" >}}
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