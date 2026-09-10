---
title: C# .NET에서 DICOM을 XML로 변환 | Aspose.Medical
weight: 3000
description: C# .NET에서 DICOM 데이터셋을 표준 DICOM XML 형식으로 직렬화합니다. Aspose.Medical API를 사용해 Bulk Data 처리, 스트림 기반 처리 및 비동기 작업을 구성하세요.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 DICOM을 XML로 변환" h2="표준 DICOM XML 표현(PS3.19)으로 DICOM 데이터셋을 직렬화합니다. 순수 .NET 라이브러리를 사용해 Bulk Data 참조, 스트림 기반 출력 및 비동기 처리를 구성하세요." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="표준 기반 DICOM XML 직렬화">}}

<p><strong>Aspose.Medical for .NET</strong>은 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a>에 따라 DICOM 데이터를 XML로 직렬화합니다. 이는 XML로 DICOM 데이터셋을 표현하기 위한 공식 표준으로, DICOMweb 서비스, 통합 플랫폼, 의료 영상 메타데이터의 인간이 읽을 수 있고 스키마 검증된 표현을 필요로 하는 시스템에서 사용됩니다.</p>

<p><code>DicomXmlSerializer</code> 클래스는 직렬화와 역직렬화를 위한 정적 메서드를 제공합니다. 단순 태그 덤프 방식과 달리, 출력은 각 요소가 태그, VR, 적절히 포맷된 값으로 표현되는 DICOM XML 스키마를 준수하여 이진 DICOM과 XML 간의 무손실 라운드‑트립 변환을 가능하게 합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#에서 DICOM을 XML로 직렬화">}}

<p><code>DicomXmlSerializer</code> 클래스를 사용하여 DICOM 데이터셋을 XML 문자열로 변환합니다. 가장 간단한 방법은 표준을 준수하는 XML 문서를 생성합니다:</p>

<div class="codeblock" id="code">
 <h3>DICOM을 XML로 변환 - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="스트림 기반 및 비동기 직렬화">}}

<p>대용량 DICOM 파일이나 고처리량 시나리오에서는 스트림에 직접 직렬화하여 메모리에서 큰 문자열을 할당하는 것을 방지합니다. 동기 및 비동기 메서드가 모두 제공됩니다:</p>

<div class="codeblock" id="code">
 <h3>동기 스트림 직렬화 - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>비동기 스트림 직렬화 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="대규모 연구를 위한 파이프라인 스트리밍">}}

<p>전체 연구를 메모리에 유지할 필요가 없습니다. <code>DicomXmlSerializer</code>는 <code>PipeWriter</code>에 쓰고 <code>PipeReader</code>에서 읽어 XML을 흐름에 따라 생성·소비할 수 있으며, 데이터셋 시퀀스를 <code>DeserializeAsyncEnumerable</code>을 통해 하나씩 읽을 수 있습니다. 모든 메서드는 <code>CancellationToken</code>을 인수로 받습니다.</p>

<div class="codeblock" id="code">
 <h3>파이프를 통한 직렬화 및 역직렬화 - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>데이터셋 시퀀스를 하나씩 읽기 - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="직렬화 옵션">}}

<p><code>DicomXmlSerializerOptions</code> 클래스는 DICOM 데이터가 XML에 어떻게 표현되는지를 제어합니다. 주요 설정은 대용량 이진 값을 위한 Bulk Data 처리를 포함합니다:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>속성</th>
<th>형식</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>대용량 데이터(예: 픽셀 데이터)를 인라인 대신 BulkData URI 참조로 기록하기 위한 사용자 정의 변환기</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>역직렬화 시 BulkData URI를 해석하기 위한 사용자 정의 로더</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>사용자 정의 옵션이 제공되지 않을 때 사용되는 기본 옵션 인스턴스</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>사용자 정의 옵션으로 직렬화 - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk Data 처리">}}

<p>대용량 이진 값(픽셀 데이터, 파형, 캡슐화된 문서)은 XML 출력에 인라인하는 대신 BulkData URI 참조로 외부화할 수 있습니다. 이는 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">DICOM PS3.19 BulkData 요소</a> 사양을 따릅니다.</p>

<p>직렬화 중 대용량 데이터를 외부화하려면 <code>IBulkDataConverter</code>를 구현하고, 역직렬화 중 URI를 해석하려면 <code>IBulkDataLoader</code>를 구현합니다. 일반적인 경우에는 로더를 작성할 필요가 없습니다: <code>DefaultBulkDataLoader.Instance</code>는 <code>file</code>, <code>http</code>, <code>https</code> URI를 해석하며, <code>IAsyncBulkDataLoader</code>도 구현하여 스트리밍 경로에서 Bulk Data를 비동기로 가져옵니다.</p>

<div class="codeblock" id="code">
 <h3>맞춤형 Bulk Data 처리 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="XML을 DICOM으로 역직렬화">}}

<p>DICOM XML을 다시 Dataset 객체로 파싱합니다. 문자열 입력, 스트림 입력 및 비동기 작업을 지원합니다:</p>

<div class="codeblock" id="code">
 <h3>XML을 DICOM으로 역직렬화 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="XML vs JSON 직렬화">}}

<p>Aspose.Medical은 DICOM XML(PS3.19)과 DICOM JSON(PS3.18) 직렬화를 모두 지원합니다. 두 형식 모두 무손실 라운드‑트립 변환을 제공하지만, 적용되는 통합 시나리오가 다릅니다:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>기능</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>표준</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>스키마 검증</td><td>XML 스키마 (XSD) 사용 가능</td><td>정식 스키마 없음</td></tr>
<tr><td>추천 용도</td><td>엔터프라이즈 통합, HL7 CDA, 감사 로그, XDS 레지스트리</td><td>DICOMweb, REST API, FHIR ImagingStudy</td></tr>
<tr><td>가독성</td><td>자세하지만 자체 설명적</td><td>컴팩트하고 널리 지원</td></tr>
<tr><td>대용량 데이터</td><td>BulkData 요소와 URI</td><td>BulkDataURI 속성</td></tr>
<tr><td>직렬화 클래스</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
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

{{< blocks/products/pf/slr-tab tabTitle=".NET용 Aspose.Medical을 선택해야 하는 이유?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="고객 목록" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="성공 사례" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
