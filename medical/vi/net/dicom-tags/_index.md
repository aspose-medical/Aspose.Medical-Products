---
title: Đọc & Sửa Thẻ DICOM trong C# .NET | Aspose.Medical
weight: 15000
description: Đọc, thêm, cập nhật và xóa thẻ DICOM trong C# .NET. Truy cập siêu dữ liệu thẻ, làm việc với thẻ riêng, liệt kê theo nhóm, và thao tác các chuỗi sử dụng Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Đọc & Sửa Thẻ DICOM trong .NET C#" h2="Truy cập, thêm, cập nhật và xóa các phần tử dữ liệu DICOM bằng API an toàn kiểu. Làm việc với các thẻ tiêu chuẩn, thẻ riêng, chuỗi và toàn bộ từ điển thẻ DICOM." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Truy cập Thẻ DICOM An toàn Kiểu">}}

<p><strong>Aspose.Medical for .NET</strong> cung cấp một API toàn diện, an toàn kiểu để làm việc với các thẻ DICOM thông qua lớp <code>Dataset</code>. Mỗi phần tử dữ liệu DICOM được biểu diễn bằng một <code>Tag</code> &mdash; một cặp số nhóm và phần tử xác định duy nhất thuộc tính. API cung cấp truy cập kiểu mạnh mẽ với các phương thức generic, mẫu truy xuất an toàn tránh ngoại lệ, và hỗ trợ giá trị đơn, mảng và truy cập theo chỉ mục.</p>

<p>Các thẻ DICOM phổ biến có sẵn dưới dạng các trường tĩnh trên lớp <code>Tag</code> (ví dụ: <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), loại bỏ nhu cầu phải ghi nhớ các mã số thẻ. Mỗi thẻ chứa <code>TagMetadata</code> với từ khóa, mô tả, Value Representation (VR), độ đa trị, và trạng thái đã ngừng sử dụng.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Đọc Thẻ DICOM trong C#">}}

<p>Lớp <code>Dataset</code> cung cấp nhiều phương thức để lấy giá trị thẻ tùy theo nhu cầu của bạn &mdash; giá trị đơn, tất cả các giá trị, truy cập theo chỉ mục, hoặc truy xuất an toàn với giá trị mặc định:</p>

<div class="codeblock" id="code">
 <h3>Đọc giá trị thẻ DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Get a single value (throws if tag is missing or multi-valued)
string patientName = dataset.GetSingleValue&lt;string&gt;(Tag.PatientName);

// Safe retrieval with a default value
string institution = dataset.GetSingleValueOrDefault&lt;string&gt;(
    Tag.InstitutionName, "Unknown");

// Get all values from a multi-valued tag
double[] pixelSpacing = dataset.GetValues&lt;double&gt;(Tag.PixelSpacing);

// Get a value by index (supports negative indexing with ^)
double firstPosition = dataset.GetValue&lt;double&gt;(Tag.ImagePositionPatient, 0);

// Check if a tag exists before accessing it
if (dataset.Contains(Tag.PatientBirthDate))
{
    var birthDate = dataset.GetSingleValue&lt;DateOnly&gt;(Tag.PatientBirthDate);
}

// TryGet pattern for safe access without exceptions
if (dataset.TryGetSingleValue&lt;string&gt;(Tag.PatientID, out var patientId))
{
    // Use patientId
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Thêm và Cập Nhật Thẻ DICOM">}}

<p>Sử dụng <code>AddOrUpdate</code> để đặt giá trị thẻ. Phương thức sẽ tạo phần tử nếu thẻ chưa tồn tại, hoặc thay thế giá trị nếu đã có. Bạn có thể đặt giá trị đơn, mảng, hoặc thêm các phần tử đã được định kiểu đầy đủ:</p>

<div class="codeblock" id="code">
 <h3>Thêm và cập nhật thẻ DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Set a single value
dataset.AddOrUpdate(Tag.PatientName, "John Doe");
dataset.AddOrUpdate(Tag.PatientAge, new Age { Number = 42, Units = Age.Unit.Years });

// Set multiple values on a multi-valued tag
dataset.AddOrUpdate&lt;double&gt;(Tag.PixelSpacing, [0.5, 0.5]);

// Add multiple elements at once using typed element classes
dataset.AddOrUpdateRange(new IElement[]
{
    new PersonName(Tag.PatientName, ["John Doe"]),
    new LongString(Tag.PatientID, ["64AD6A3F"]),
    new Date(Tag.PatientBirthDate, [new DateOnly(2002, 10, 10)])
});

// Save the modified file
dicomFile.Save("updated.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Xóa Thẻ DICOM">}}

<p>Phương thức <code>Remove</code> hỗ trợ xóa một thẻ, nhiều thẻ cùng lúc, hoặc các thẻ đáp ứng một điều kiện &mdash; hữu ích để loại bỏ toàn bộ nhóm phần tử:</p>

<div class="codeblock" id="code">
 <h3>Xóa thẻ DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Remove a single tag
dataset.Remove(Tag.PatientAddress);

// Remove multiple tags at once
dataset.Remove(Tag.PatientName, Tag.PatientID, Tag.PatientBirthDate);

// Remove all tags in a specific group (e.g., group 0x0002 - File Meta Information)
dataset.Remove(element => element.Tag.Group == 0x0002);

dicomFile.Save("cleaned.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Liệt Kê Thẻ Theo Nhóm">}}

<p>Các thẻ DICOM được tổ chức theo nhóm &mdash; ví dụ, nhóm <code>0x0010</code> chứa thông tin bệnh nhân, nhóm <code>0x0008</code> chứa thông tin nghiên cứu, và nhóm <code>0x0028</code> chứa các thuộc tính pixel ảnh. Sử dụng <code>EnumerateGroup</code> để duyệt qua tất cả các phần tử trong một nhóm cụ thể:</p>

<div class="codeblock" id="code">
 <h3>Liệt kê phần tử theo nhóm - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Enumerate all patient-related tags (group 0x0010)
foreach (IElement element in dataset.EnumerateGroup(0x0010))
{
    Tag tag = element.Tag;
    string keyword = tag.Metadata.Keyword;
    string vr = element.ValueRepresentation.ToString();

    Console.WriteLine($"({tag.Group:X4},{tag.Element:X4}) {keyword} [{vr}]");
}

// Iterate over all elements in the dataset
foreach (IElement element in dataset)
{
    // Process each element
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Thao Tác Dữ Liệu Cấp Phần Tử">}}

<p>Mỗi phần tử DICOM cung cấp các phương thức để đọc, thêm, chèn, xóa và thay thế các giá trị riêng lẻ trong phần tử. Điều này hữu ích cho các thẻ đa giá trị khi bạn cần kiểm soát chi tiết danh sách giá trị:</p>

<div class="codeblock" id="code">
 <h3>Thao tác giá trị phần tử - C#</h3>
 <pre><code class="cs">// Create an element with multiple values
var element = new FloatingPointDouble(
    Tag.TableOfYBreakPoints, [50.1, 100.2, 150.3]);

// Access values by index (supports ^ for indexing from end)
double first = element.Get&lt;double&gt;(0);
double last = element.Get&lt;double&gt;(^1);

// Safe access with default
double safe = element.GetOrDefault&lt;double&gt;(10); // returns 0.0 if out of range

// Get all values or a range
double[] all = element.GetValues&lt;double&gt;();
double[] subset = element.GetValues&lt;double&gt;(1..3);

// Modify element values
element.Add(200.4);
element.AddRange([300.5, 400.6]);
element.Insert(2, 75.0);
element.RemoveAt(0);
element.Replace([1.0, 2.0, 3.0]);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Từ Điển Thẻ và Siêu Dữ Liệu">}}

<p>Lớp <code>TagDictionary</code> cung cấp một kho dữ liệu các thẻ DICOM đã biết cùng với siêu dữ liệu của chúng &mdash; từ khóa, mô tả, Value Representation, độ đa trị và trạng thái ngừng sử dụng. Bạn có thể tra cứu thẻ theo từ khóa hoặc kiểm tra siêu dữ liệu của chúng bằng chương trình:</p>

<div class="codeblock" id="code">
 <h3>Truy vấn từ điển thẻ - C#</h3>
 <pre><code class="cs">// Look up a tag by its keyword
Tag? tag = TagDictionary.Default.FindByKeyword("PatientName");

// Access tag metadata from the dictionary
TagMetadata meta = TagDictionary.Default[Tag.PatientName];
Console.WriteLine($"Keyword: {meta.Keyword}");
Console.WriteLine($"Name: {meta.Name}");
Console.WriteLine($"VR: {string.Join("/", meta.ValueRepresentations)}");
Console.WriteLine($"VM: {meta.Multiplicity}");
Console.WriteLine($"Retired: {meta.Retired}");

// Access metadata directly from a tag instance
TagMetadata info = Tag.Rows.Metadata;

// Parse a tag from its string representation
Tag parsed = Tag.Parse("0010,0010");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Hỗ Trợ Thẻ Riêng">}}

<p>DICOM cho phép các nhà cung cấp và tổ chức định nghĩa thẻ riêng cho dữ liệu độc quyền. Aspose.Medical cho .NET hỗ trợ đầy đủ việc đọc, ghi và đăng ký các thẻ riêng. Bạn có thể tải từ điển thẻ tùy chỉnh từ các tệp XML để xử lý đúng các phần tử riêng:</p>

<div class="codeblock" id="code">
 <h3>Làm việc với thẻ riêng - C#</h3>
 <pre><code class="cs">// Load a custom private tag dictionary from XML
// XML format:
// &lt;dictionaries&gt;
//   &lt;dictionary creator="MY ORGANIZATION"&gt;
//     &lt;tag group="3011" element="xx00" vr="SL" vm="1"&gt;Custom Value&lt;/tag&gt;
//   &lt;/dictionary&gt;
// &lt;/dictionaries&gt;
TagDictionary.Default.Load("custom-tag-dictionary.xml");

// Check if a tag is private
Tag tag = Tag.GetOrCreate(0x3011, 0x0010);
bool isPrivate = tag.IsPrivate;

// Get a valid private tag (creates Private Creator element if needed)
Dataset dataset = dicomFile.Dataset;
Tag privateTag = dataset.GetPrivateTag(tag);

// Register a private creator programmatically
PrivateCreator creator = new("MY ORGANIZATION");
TagDictionary privateDictionary =
    TagDictionary.Default.GetPrivateDictionary(creator);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Làm việc với Chuỗi DICOM">}}

<p>Các chuỗi DICOM (loại VR SQ) chứa các dataset lồng nhau, tạo thành cấu trúc cây trong tệp DICOM. Lớp <code>Sequence</code> cung cấp truy cập định kiểu đến các mục chuỗi:</p>

<div class="codeblock" id="code">
 <h3>Đọc và tạo chuỗi DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Read a sequence element
IElement seqElement = dataset.GetOrDefault(Tag.ReferencedStudySequence);
if (seqElement is Sequence sequence)
{
    // Iterate over sequence items (each item is a Dataset)
    foreach (Dataset item in sequence.Items)
    {
        string uid = item.GetSingleValueOrDefault&lt;string&gt;(
            Tag.ReferencedSOPInstanceUID, "");
    }

    // Access an item by index
    Dataset firstItem = sequence.Get(0);
}

// Create a new sequence
var items = new List&lt;Dataset&gt;
{
    new(new IElement[]
    {
        new UniqueIdentifier(Tag.ReferencedSOPClassUID, ["1.2.840.10008.5.1.4.1.1.2"]),
        new UniqueIdentifier(Tag.ReferencedSOPInstanceUID, ["1.2.3.4.5.6.7.8.9"])
    })
};
dataset.Add(new Sequence(Tag.ReferencedStudySequence, items));</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các Nhóm Thẻ DICOM Thông Thường">}}

<p>Bảng dưới đây liệt kê các nhóm thẻ DICOM thường dùng và các thẻ mẫu có thể truy cập qua lớp <code>Tag</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Nhóm</th>
<th>Loại</th>
<th>Thẻ Mẫu</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Thông tin Siêu dữ liệu Tập tin</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Nhận dạng Nghiên cứu</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Thông tin Bệnh nhân</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Tham số Thu thập</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Định vị Hình ảnh</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Thuộc tính Pixel Ảnh</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Dữ liệu Pixel</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài Nguyên Học Tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài Liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Mã Nguồn" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Tham chiếu API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Hỗ Trợ Sản Phẩm" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Hỗ Trợ Miễn Phí" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Hỗ Trợ Trả Phí" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Tại sao lại là Aspose.Medical cho .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Danh sách Khách hàng" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Câu chuyện Thành công" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
