---
title: C# .NET에서 JSON을 DICOM으로 변환 | Aspose.Medical
weight: 6000

description: C# .NET에서 표준 DICOM JSON 모델(PS3.18)로부터 DICOM 파일을 생성합니다. 문자열, 스트림 또는 파이프에서 JSON을 읽고, 데이터셋 시퀀스를 스트리밍하며, Aspose.Medical API로 Bulk Data 참조를 해결합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 JSON을 DICOM으로 변환" h2="표준 DICOM JSON 모델(PS3.18)을 다시 데이터셋 및 DICOM 파일로 읽어들입니다. 문자열, 스트림 또는 파이프에서 작업하고, 연구 시퀀스를 스트리밍하며, Bulk Data 참조를 해결합니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM JSON에서 DICOM 파일로">}}

<p><strong>Aspose.Medical for .NET</strong>은 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>을 읽으며, 이는 DICOMweb 서비스와 HTTP를 통해 연구를 교환하는 시스템에서 사용되는 표현 방식입니다. JSON으로 도착한 데이터는 <code>Dataset</code>이 되고, <code>Dataset</code>은 DICOM 파일로 디스크에 기록됩니다.</p>

<p>이는 <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> 페이지의 반대 방향이며, 두 페이지 모두 동일한 클래스 <code>DicomJsonSerializer</code>를 사용합니다.</p>

<div class="codeblock" id="code">
 <h3>JSON에서 DICOM 파일 생성 - C#</h3>
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

<p>File Meta Information을 포함하지 않은 데이터셋은 <code>DicomFile</code>로 래핑될 때 기본 전송 구문인 Implicit VR Little Endian으로 기록됩니다.</p>

<p>DICOM JSON 읽기는 라이선스가 필요한 기능입니다. 온프레미스 라이선스를 적용하지 않으면 리더가 <code>MedicalApiException</code>을 발생시키므로, <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">라이선스 가이드</a>에 따라 먼저 라이선스를 적용하십시오.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="File Meta Information 유지">}}

<p><code>Deserialize</code>는 데이터셋만 반환합니다. JSON 문서에 File Meta Information 그룹이 포함된 경우(예: 전체 DICOM 파일에서 생성된 경우) <code>DeserializeFile</code>은 해당 그룹이 그대로 유지된 <code>DicomFile</code>을 반환하며, 파일이 선언한 전송 구문도 포함됩니다.</p>

<div class="codeblock" id="code">
 <h3>JSON에서 전체 DICOM 파일 읽기 - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="스트림, 파이프 및 비동기">}}

<p>각 진입점은 스트림 오버로드와 비동기 오버로드를 제공하며, 비동기 버전은 <code>PipeReader</code>도 허용합니다. 웹 응답이나 디스크에서 도착한 문서는 먼저 문자열로 변환되지 않고 바로 읽히며, JSON에 픽셀 데이터가 포함될 경우 이것이 중요합니다.</p>

<div class="codeblock" id="code">
 <h3>스트림에서 JSON 읽기 - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="데이터셋 시퀀스, 한 번에 하나씩">}}

<p>DICOMweb 쿼리는 데이터셋 배열로 응답하며, 이러한 문서는 규모가 클 수 있습니다. <code>DeserializeList</code>는 전체 배열을 메모리로 읽어들이고, <code>DeserializeAsyncEnumerable</code>는 한 번에 하나씩 데이터셋을 제공하므로 문서 전체를 메모리에 보관하지 않습니다.</p>

<div class="codeblock" id="code">
 <h3>데이터셋 배열 스트리밍 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Bulk Data 참조">}}

<p>DICOM JSON Model은 픽셀 데이터를 인라인으로 포함하지 않습니다. 큰 값은 바이트를 가리키는 <code>BulkDataURI</code>로 대체되어 JSON 문서가 작게 유지됩니다. 읽는 동안 이러한 참조를 해결하려면 직렬화기에 bulk data 로더를 제공하십시오. <code>DefaultBulkDataLoader</code>는 인증 없이 <code>file</code>, <code>http</code>, <code>https</code> URI를 가져옵니다; 인증이 필요한 아카이브의 경우 직접 <code>IBulkDataLoader</code> 또는 <code>IAsyncBulkDataLoader</code>를 구현하십시오.</p>

<div class="codeblock" id="code">
 <h3>읽는 동안 BulkDataURI 해결 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM ↔ JSON 라운드 트립">}}

<p>두 방향은 함께 사용하도록 설계되었습니다: 연구가 JSON 형태로 나가 웹 서비스로 전달된 뒤 DICOM 파일로 다시 돌아옵니다. 이 과정은 네이티브 코드에 의존하지 않으므로 동일한 라운드 트립이 Windows, Linux, macOS에서 모두 실행됩니다.</p>

<div class="codeblock" id="code">
 <h3>DICOM to JSON 및 복귀 - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>JSON 형태를 제어하는 옵션은 <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> 페이지를 참고하십시오. 동일한 쌍이 XML에도 있습니다: <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> 및 <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>. 전체 API를 다루는 <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">JSON 직렬화 가이드</a>를 확인하세요.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="학습 자료" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="문서" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="개발자 가이드" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="API 레퍼런스" href="https://reference.aspose.com/medical/net/" >}}
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