---
title: Ẩn danh các tệp DICOM trong C# .NET | Aspose.Medical
weight: 1000
description: Ẩn danh các tệp DICOM trong C# .NET sử dụng các hồ sơ bảo mật có thể cấu hình. Loại bỏ thông tin cá nhân (PII) của bệnh nhân, đảm bảo tuân thủ HIPAA và GDPR với API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Ẩn danh các tệp DICOM trong .NET C#" h2="Loại bỏ thông tin nhận dạng bệnh nhân khỏi các tệp DICOM bằng các hồ sơ bảo mật DICOM PS 3.15. Đảm bảo tuân thủ HIPAA và GDPR với thư viện .NET thuần." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Ẩn danh DICOM dựa trên tiêu chuẩn">}}

<p><strong>Aspose.Medical for .NET</strong> cung cấp một API ẩn danh DICOM toàn diện dựa trên <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">Các hồ sơ bảo mật DICOM PS 3.15</a>. API cho phép các nhà phát triển y tế loại bỏ hoặc chỉnh sửa thông tin nhận dạng bệnh nhân (PII) khỏi các tệp DICOM trong khi vẫn bảo toàn giá trị lâm sàng của dữ liệu hình ảnh. Khác với các phương pháp chỉ xóa thẻ đơn giản, Aspose.Medical tuân theo tiêu chuẩn DICOM chính thức để đảm bảo việc ẩn danh đúng cách.</p>

<p>Lớp <code>Anonymizer</code> hoạt động cùng các đối tượng <code>ConfidentialityProfile</code> xác định chính xác cách xử lý mỗi thuộc tính DICOM &mdash; có nên xóa, thay thế bằng giá trị giả, làm sạch, hay giữ nguyên. Cách tiếp cận này đảm bảo việc ẩn danh nhất quán, có thể kiểm toán và đáp ứng các yêu cầu pháp quy.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ẩn danh một tệp DICOM trong C#">}}

<p>Cách đơn giản nhất để ẩn danh một tệp DICOM là sử dụng hồ sơ bảo mật mặc định. Điều này áp dụng Hồ sơ Cơ bản DICOM PS 3.15, xử lý tất cả các thuộc tính nhận dạng bệnh nhân tiêu chuẩn:</p>

<div class="codeblock" id="code">
 <h3>Ẩn danh DICOM cơ bản - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>Phương thức <code>Anonymize</code> là <strong>không phá hủy</strong> &mdash; nó sao chép tập dữ liệu gốc và trả về một bản sao đã ẩn danh mới. Tệp DICOM gốc vẫn không thay đổi, điều này quan trọng cho việc theo dõi audit và quy trình đảm bảo tính toàn vẹn dữ liệu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Thay thế danh tính bệnh nhân tùy chỉnh">}}

<p>Trong nhiều quy trình, bạn cần thay thế thông tin bệnh nhân bằng các giá trị bí danh cụ thể thay vì chỉ xóa chúng. Lớp <code>ConfidentialityProfile</code> cho phép bạn đặt giá trị thay thế cho tên bệnh nhân và ID bệnh nhân:</p>

<div class="codeblock" id="code">
 <h3>Ẩn danh với danh tính bệnh nhân tùy chỉnh - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create a confidentiality profile with custom replacement values
ConfidentialityProfile profile = new()
{
    PatientName = "ANONYMOUS PATIENT",
    PatientId = "00000000"
};

// Create an anonymizer with this profile
Anonymizer anonymizer = new(profile);

// Anonymize the file
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("custom_anonymized.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ẩn danh tại chỗ">}}

<p>Đối với các trường hợp mà hiệu suất bộ nhớ quan trọng hoặc bạn không cần giữ dữ liệu gốc, API cung cấp chức năng ẩn danh tại chỗ, sửa đổi tập dữ liệu trực tiếp:</p>

<div class="codeblock" id="code">
 <h3>Ẩn danh DICOM tại chỗ - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Cả hai phương thức <code>Anonymize</code> và <code>AnonymizeInPlace</code> cũng chấp nhận một đối tượng <code>Dataset</code> trực tiếp, cho phép bạn kiểm soát hoàn toàn tập dữ liệu sẽ được xử lý.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Các tùy chọn hồ sơ bảo mật có thể cấu hình">}}

<p>Tiêu chuẩn DICOM PS 3.15 định nghĩa một <strong>Hồ sơ Cơ bản</strong> cùng với một số hồ sơ tùy chọn kiểm soát cách xử lý các loại dữ liệu cụ thể trong quá trình ẩn danh. Bạn có thể kết hợp các tùy chọn này bằng enum <code>ConfidentialityProfileOptions</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tùy chọn</th>
<th>Mô tả</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Hồ sơ Bảo mật Mức Ứng dụng Cơ bản DICOM PS 3.15 mặc định</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Giữ lại các thuộc tính riêng an toàn trong quá trình ẩn danh</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Giữ nguyên các UID DICOM gốc (Study, Series, Instance) không thay đổi</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Giữ lại các thuộc tính nhận dạng thiết bị (nhà sản xuất, mẫu, tên trạm)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Giữ lại các thuộc tính nhận dạng tổ chức (tên, địa chỉ, phòng ban)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Giữ lại đặc điểm bệnh nhân (tuổi, giới tính, kích thước, cân nặng) cho mục đích nghiên cứu</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Giữ lại các giá trị ngày giờ đầy đủ cho các nghiên cứu dọc thời gian</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Giữ lại ngày sửa đổi cho các nghiên cứu dọc thời gian</td></tr>
<tr><td><code>CleanDesc</code></td><td>Làm sạch mô tả văn bản có thể chứa thông tin nhận dạng</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Làm sạch nội dung có cấu trúc có thể chứa thông tin nhận dạng</td></tr>
<tr><td><code>CleanGraph</code></td><td>Làm sạch dữ liệu đồ họa có thể chứa thông tin bệnh nhân đã được nhúng</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Ẩn danh với các tùy chọn hồ sơ cụ thể - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Mã hành động cho xử lý thuộc tính">}}

<p>Mỗi thuộc tính trong một hồ sơ bảo mật được gán một mã hành động xác định cách xử lý nó trong quá trình ẩn danh. Các mã này tuân theo tiêu chuẩn DICOM PS 3.15 Bảng E.1-1a:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Mã</th>
<th>Hành động</th>
<th>Mô tả</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Thay thế bằng giá trị giả</td><td>Thay thế bằng một giá trị độ dài khác không, phù hợp với VR</td></tr>
<tr><td><strong>Z</strong></td><td>Độ dài 0 hoặc giả</td><td>Thay thế bằng giá trị độ dài 0 hoặc giá trị giả phù hợp với VR</td></tr>
<tr><td><strong>X</strong></td><td>Xóa</td><td>Xóa hoàn toàn thuộc tính khỏi tập dữ liệu</td></tr>
<tr><td><strong>K</strong></td><td>Giữ</td><td>Giữ nguyên thuộc tính (đối với không phải chuỗi) hoặc làm sạch (đối với chuỗi)</td></tr>
<tr><td><strong>C</strong></td><td>Làm sạch</td><td>Thay thế bằng giá trị đã được làm sạch, có nghĩa tương tự, phù hợp với VR</td></tr>
<tr><td><strong>U</strong></td><td>Thay thế UID</td><td>Thay thế bằng UID mới, đồng nhất nội bộ trong bộ các instance</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Hồ sơ tùy chỉnh từ CSV, JSON và XML">}}

<p>Đối với các trường hợp sử dụng nâng cao, bạn có thể định nghĩa các hồ sơ ẩn danh tùy chỉnh bằng cách tải quy tắc từ các tệp CSV, JSON hoặc XML bên ngoài. Điều này cho phép bạn điều chỉnh hành vi ẩn danh theo chính sách tổ chức hoặc yêu cầu pháp lý cụ thể của mình:</p>

<div class="codeblock" id="code">
 <h3>Tải hồ sơ ẩn danh từ CSV - C#</h3>
 <pre><code class="cs">// CSV format: TagPattern;Action
// 0010,0010;Z   (Anonymize PatientName)
// 0010,0020;D   (Replace PatientID with dummy)
// 0020,000D;U   (Replace StudyInstanceUID)

var profile = ConfidentialityProfile.LoadFromCsvFile(
    "anonymization_rules.csv",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Tải hồ sơ ẩn danh từ JSON - C#</h3>
 <pre><code class="cs">// JSON format:
// [
//     { "Tag": "0010,0010", "Action": "Z" },
//     { "Tag": "0010,0020", "Action": "D" },
//     { "Tag": "0020,000D", "Action": "U" }
// ]

var profile = ConfidentialityProfile.LoadFromJsonFile(
    "anonymization_rules.json",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<p>Các hồ sơ tùy chỉnh cũng có thể được tải từ tệp XML (<code>LoadFromXmlFile</code>), luồng (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) hoặc trực tiếp từ chuỗi (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tuân thủ HIPAA và GDPR">}}

<p>Các tệp DICOM thường chứa Thông tin Y tế Bảo vệ (PHI) theo định nghĩa của HIPAA và dữ liệu cá nhân theo định nghĩa của GDPR. Aspose.Medical cho .NET giúp bạn đáp ứng các yêu cầu pháp lý bằng cách cung cấp:</p>

<ul>
<li><strong>Tuân thủ DICOM PS 3.15</strong>: Công cụ ẩn danh tuân theo tiêu chuẩn DICOM chính thức cho việc loại bỏ nhận dạng, đảm bảo các thực tiễn tốt nhất được áp dụng cho mọi thuộc tính liên quan.</li>
<li><strong>Loại bỏ PII toàn diện</strong>: Tên bệnh nhân, ID, ngày sinh, địa chỉ và các định danh khác được xử lý theo hồ sơ bảo mật đã cấu hình.</li>
<li><strong>Thay thế UID</strong>: Các UID của Study, Series và SOP Instance có thể được thay thế bằng UID mới, đồng nhất nội bộ để ngăn ngừa việc nhận dạng lại thông qua tham chiếu chéo.</li>
<li><strong>Giữ lại có thể cấu hình</strong>: Khi nhu cầu nghiên cứu hoặc lâm sàng yêu cầu, các loại dữ liệu cụ thể (đặc điểm bệnh nhân, ngày tháng, thông tin thiết bị) có thể được giữ lại một cách có chọn lọc bằng các hồ sơ tùy chọn tiêu chuẩn.</li>
<li><strong>Quy trình có thể kiểm toán</strong>: Cách tiếp cận dựa trên tiêu chuẩn với các mã hành động được định nghĩa cung cấp quy trình ẩn danh rõ ràng, có thể tài liệu hoá cho các cuộc kiểm toán pháp lý.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tài nguyên học tập" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Tài liệu" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Mã nguồn" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Tham khảo API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Hỗ trợ sản phẩm" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Hỗ trợ miễn phí" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Hỗ trợ trả phí" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Tại sao nên chọn Aspose.Medical cho .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Danh sách khách hàng" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Câu chuyện thành công" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
