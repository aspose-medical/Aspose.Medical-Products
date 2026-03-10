---
title: C# .NET에서 DICOM을 JSON으로 변환 | Aspose.Medical
weight: 4000
description: C# .NET에서 DICOM 파일을 표준 DICOM JSON 형식으로 직렬화합니다. 숫자 처리, 대용량 데이터 URI, 키워드 출력 및 비동기 스트림 처리를 Aspose.Medical API로 구성합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 DICOM을 JSON으로 변환" h2="DICOM 데이터셋 및 파일을 표준 DICOM JSON 모델(PS3.18)로 직렬화합니다. 숫자 처리, 대용량 데이터 참조, 키워드 출력 및 스트림 기반 비동기 처리를 구성합니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="표준 DICOM JSON 모델">}}

<p><strong>Aspose.Medical for .NET</strong>은 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>에 따라 DICOM 데이터를 JSON으로 직렬화합니다. 이는 DICOM 데이터셋을 JSON으로 표현하는 공식 표준으로, DICOMweb 서비스, FHIR ImagingStudy 리소스 및 최신 헬스케어 API에서 사용됩니다.</p>

<p>DICOM JSON Model에서 각 속성은 태그를 JSON 키로 표시합니다(예: Patient Name의 경우 <code>"00100010"</code>). Value Representation과 값 배열은 중첩 속성으로 포함됩니다.</p>

<div class="codeblock" id="code">
 <h3>DICOM JSON Model 형식</h3>
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

{{< blocks/products/pf/feature-page-section h2="C#에서 DICOM을 JSON으로 직렬화">}}

<p><code>DicomJsonSerializer</code> 클래스를 사용하여 DICOM 파일 또는 데이터셋을 JSON으로 변환합니다. 가장 간단한 방법은 압축되고 표준을 준수하는 출력을 생성합니다.</p>

<div class="codeblock" id="code">
 <h3>DICOM을 JSON으로 변환 - C#</h3>
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

<p><code>Dataset</code>, 전체 <code>DicomFile</code>(File Meta Information 포함) 또는 데이터셋 배열을 직렬화할 수 있습니다.</p>

<div class="codeblock" id="code">
 <h3>DicomFile 및 데이터셋 배열 직렬화 - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="스트림 기반 및 비동기 직렬화">}}

<p>대용량 DICOM 파일이나 고처리량 시나리오에서는 메모리에서 큰 문자열을 할당하는 것을 피하기 위해 UTF-8 스트림으로 직접 직렬화합니다. 동기 방식과 비동기 방식 모두 제공됩니다.</p>

<div class="codeblock" id="code">
 <h3>스트림 및 비동기 직렬화 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="직렬화 옵션">}}

<p><code>DicomJsonSerializerOptions</code> 클래스는 DICOM 데이터가 JSON에 어떻게 표현되는지를 제어합니다.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>속성</th>
<th>형식</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>숫자 VR(IS, DS, SV, UV)이 JSON 숫자 또는 문자열로 기록되는지 제어합니다</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>DICOM 키워드(예: <code>"PatientName"</code>)를 JSON 키로 사용하고 태그(<code>"00100010"</code>) 대신 사용합니다. 참고: 비표준</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>DICOM 키워드를 추가 JSON 속성으로 포함합니다</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>DICOM 태그 이름을 추가 JSON 속성으로 포함합니다</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>대용량 데이터(예: 픽셀 데이터)를 URI 참조로 기록하기 위한 사용자 정의 변환기</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>역직렬화 중 대용량 데이터 URI를 해석하기 위한 사용자 정의 로더</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>직렬화 옵션 구성 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="숫자 처리">}}

<p>DICOM 숫자 Value Representation(IS, DS, SV, UV)은 JSON 숫자 또는 JSON 문자열로 직렬화될 수 있습니다. 이는 일부 DICOM 구현이 앞쪽 공백이나 비표준 형식으로 숫자를 저장하기 때문에 상호 운용성에 중요합니다.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>모드</th>
<th>JSON 출력</th>
<th>사용 사례</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>표준을 준수하며 계산에 이상적입니다. 값 파싱이 불가능하면 예외가 발생합니다.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>원본 문자열 표현을 보존합니다. 비표준 데이터를 라운드트립할 때 더 안전합니다.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="대용량 데이터 처리">}}

<p>대용량 바이너리 값(픽셀 데이터, 파형, 캡슐화된 문서)은 JSON 출력에 인라인되지 않고 대용량 데이터 참조로 처리할 수 있습니다. 이는 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a> 사양을 따릅니다.</p>

<p>직렬화 중 대용량 데이터를 외부화하려면 <code>IBulkDataConverter</code>를 구현하고, 역직렬화 중 URI를 해석하려면 <code>IBulkDataLoader</code>를 구현합니다.</p>

<div class="codeblock" id="code">
 <h3>맞춤형 대용량 데이터 처리 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="JSON을 DICOM으로 역직렬화">}}

<p>DICOM JSON을 Dataset 또는 DicomFile 객체로 다시 파싱합니다. 문자열 입력, 스트림 입력 및 비동기 스트림 작업을 지원합니다.</p>

<div class="codeblock" id="code">
 <h3>JSON을 DICOM으로 역직렬화 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="System.Text.Json 통합">}}

<p>Aspose.Medical은 표준 <code>System.Text.Json</code> 직렬화 파이프라인과 통합하기 위해 <code>DatasetJsonConverter</code>와 <code>DicomFileJsonConverter</code>를 제공합니다. 이를 통해 기존 JSON 처리 코드에서 DICOM 객체를 다른 .NET 유형과 함께 직렬화할 수 있습니다.</p>

<div class="codeblock" id="code">
 <h3>System.Text.Json 통합 - C#</h3>
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

{{< blocks/products/pf/slr-tab tabTitle="왜 Aspose.Medical for .NET인가?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="고객 목록" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="성공 사례" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
