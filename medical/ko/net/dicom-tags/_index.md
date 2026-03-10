---
title: C# .NET에서 DICOM 태그 읽기 및 수정 | Aspose.Medical
weight: 15000
description: C# .NET에서 DICOM 태그를 읽고, 추가하고, 업데이트하고, 제거합니다. 태그 메타데이터에 접근하고, 개인 태그를 다루며, 그룹별로 열거하고, Aspose.Medical API를 사용해 시퀀스를 조작합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 DICOM 태그 읽기 및 수정" h2="형식 안전 API를 사용해 DICOM 데이터 요소에 접근하고, 추가하고, 업데이트하고, 제거합니다. 표준 태그, 개인 태그, 시퀀스 및 전체 DICOM 태그 사전을 활용합니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="형식 안전 DICOM 태그 접근">}}

<p><strong>Aspose.Medical for .NET</strong>은 <code>Dataset</code> 클래스를 통해 DICOM 태그를 다루기 위한 포괄적이고 형식 안전한 API를 제공합니다. 모든 DICOM 데이터 요소는 고유하게 attribute를 식별하는 그룹 번호와 요소 번호의 쌍인 <code>Tag</code> &mdash; 로 표현됩니다. 이 API는 제네릭 메서드를 이용한 강력한 형식 지정 접근, 예외를 방지하는 안전한 검색 패턴, 단일 값, 배열 및 인덱스 접근을 지원합니다.</p>

<p>일반적인 DICOM 태그는 <code>Tag</code> 클래스의 정적 필드로 제공됩니다(예: <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>). 따라서 숫자 태그 코드를 외울 필요가 없습니다. 각 태그는 키워드, 설명, Value Representation(VR), 값 다중성 및 폐기 상태를 포함하는 <code>TagMetadata</code>를 갖습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#에서 DICOM 태그 읽기">}}

<p><code>Dataset</code> 클래스는 필요에 따라 태그 값을 가져오는 다양한 메서드를 제공합니다 &mdash; 단일 값, 모든 값, 인덱스 접근, 또는 기본값을 사용한 안전한 검색:</p>

<div class="codeblock" id="code">
 <h3>C#에서 DICOM 태그 값 읽기</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM 태그 추가 및 업데이트">}}

<p>태그 값을 설정하려면 <code>AddOrUpdate</code>를 사용합니다. 해당 태그가 없으면 요소를 생성하고, 이미 존재하면 값을 교체합니다. 단일 값, 배열 또는 완전한 형식의 요소를 설정할 수 있습니다:</p>

<div class="codeblock" id="code">
 <h3>C#에서 DICOM 태그 추가 및 업데이트</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM 태그 제거">}}

<p><code>Remove</code> 메서드는 단일 태그, 여러 태그를 한 번에, 또는 조건에 맞는 태그를 제거하는 것을 지원합니다 &mdash; 전체 요소 그룹을 한 번에 제거할 때 유용합니다.</p>

<div class="codeblock" id="code">
 <h3>C#에서 DICOM 태그 제거</h3>
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

{{< blocks/products/pf/feature-page-section h2="그룹별 태그 열거">}}

<p>DICOM 태그는 그룹으로 조직됩니다 &mdash; 예를 들어, 그룹 <code>0x0010</code>은 환자 정보를, 그룹 <code>0x0008</code>은 연구 식별을, 그룹 <code>0x0028</code>은 이미지 픽셀 속성을 포함합니다. 특정 그룹의 모든 요소를 순회하려면 <code>EnumerateGroup</code>을 사용합니다:</p>

<div class="codeblock" id="code">
 <h3>C#에서 그룹별 요소 열거</h3>
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

{{< blocks/products/pf/feature-page-section h2="요소 수준 데이터 조작">}}

<p>각 DICOM 요소는 해당 요소 내 개별 값을 읽고, 추가하고, 삽입하고, 제거하고, 교체하는 메서드를 제공합니다. 이는 여러 값을 갖는 태그에서 값 목록을 세밀하게 제어해야 할 때 유용합니다.</p>

<div class="codeblock" id="code">
 <h3>C#에서 요소 값 조작</h3>
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

{{< blocks/products/pf/feature-page-section h2="태그 사전 및 메타데이터">}}

<p><code>TagDictionary</code> 클래스는 알려진 DICOM 태그와 그 메타데이터(키워드, 설명, Value Representation, 값 다중성, 폐기 상태)를 레지스트리 형태로 제공합니다. 키워드로 태그를 조회하거나 프로그래밍 방식으로 메타데이터를 검사할 수 있습니다:</p>

<div class="codeblock" id="code">
 <h3>C#에서 태그 사전 조회</h3>
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

{{< blocks/products/pf/feature-page-section h2="개인 태그 지원">}}

<p>DICOM은 벤더와 기관이 독점 데이터에 대한 개인 태그를 정의하도록 허용합니다. Aspose.Medical for .NET은 개인 태그의 읽기, 쓰기 및 등록을 완전하게 지원합니다. XML 파일에서 맞춤형 태그 사전을 로드하여 개인 요소를 적절히 처리할 수 있습니다:</p>

<div class="codeblock" id="code">
 <h3>C#에서 개인 태그 작업</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM 시퀀스 작업">}}

<p>DICOM 시퀀스(VR 타입 SQ)는 중첩된 데이터셋을 포함하며, DICOM 파일 내에서 트리 구조를 형성합니다. <code>Sequence</code> 클래스는 시퀀스 항목에 대한 형식화된 접근을 제공합니다.</p>

<div class="codeblock" id="code">
 <h3>C#에서 DICOM 시퀀스 읽기 및 생성</h3>
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

{{< blocks/products/pf/feature-page-section h2="일반적인 DICOM 태그 그룹">}}

<p>다음 표는 자주 사용되는 DICOM 태그 그룹과 <code>Tag</code> 클래스를 통해 접근 가능한 예시 태그를 나열합니다:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>그룹</th>
<th>카테고리</th>
<th>예시 태그</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>파일 메타 정보</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>연구 식별</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>환자 정보</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>획득 매개변수</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>이미지 위치 지정</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>이미지 픽셀 속성</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>픽셀 데이터</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="학습 자료" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="문서" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="소스 코드" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API 참조" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="제품 지원" tabId="support" >}}
{{< blocks/products/pf/slr-element name="무료 지원" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="유료 지원" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="블로그" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle=".NET용 Aspose.Medical을 선택하는 이유?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="고객 목록" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="성공 사례" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
