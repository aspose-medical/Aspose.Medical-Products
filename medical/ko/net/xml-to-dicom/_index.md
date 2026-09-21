---
title: C# .NET에서 XML을 DICOM으로 변환 | Aspose.Medical
weight: 5000

description: C# .NET에서 PS3.19의 Native DICOM Model XML을 사용하여 DICOM 파일을 생성합니다. 문자열, 스트림 또는 파이프에서 XML을 읽고, 연속 문서를 스트리밍하며, Aspose.Medical API를 이용해 대용량 데이터 참조를 해결합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 XML을 DICOM으로 변환" h2="PS3.19의 Native DICOM Model XML을 다시 데이터셋 및 DICOM 파일로 읽어옵니다. 문자열, 스트림 또는 파이프에서 작업하고, 연속 문서를 스트리밍하며, 대용량 데이터 참조를 해결합니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="표준 Native DICOM Model XML">}}

<p><strong>Aspose.Medical for .NET</strong>은 DICOM PS3.19에 정의된 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a>을 읽습니다. 이는 표준 자체에 기록된 XML 표현이며, Aspose가 만든 형식이 아니라 통합에 유용합니다: 이미 DICOM을 XML로 교환하는 시스템이 이 라이브러리가 받아들이는 문서를 생성합니다.</p>

<p>문서 루트는 <code>NativeDicomModel</code>이며, 각 속성은 태그, 값 표현 및 키워드를 포함하는 <code>DicomAttribute</code> 요소입니다:</p>

<div class="codeblock" id="code">
 <h3>Native DICOM Model 형식</h3>
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

<p>이 페이지는 <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>의 역방향이며, 두 경우 모두 동일한 클래스 <code>DicomXmlSerializer</code>를 사용합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#에서 XML로부터 DICOM 파일 생성">}}

<p><code>Deserialize</code>는 문서를 <code>Dataset</code>으로 변환하고, 데이터셋은 DICOM 파일로 디스크에 저장됩니다.</p>

<div class="codeblock" id="code">
 <h3>XML에서 DICOM 파일 생성 - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Native DICOM Model에는 File Meta Information 그룹이 없으므로 전송 구문이 문서에 포함되지 않습니다. <code>DicomFile</code>에 래핑된 데이터셋은 기본 전송 구문인 Implicit VR Little Endian으로 기록됩니다. 다른 전송 구문으로 저장하려면 변환하세요, 이는 <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> 페이지에 나와 있습니다.</p>

<p>DICOM XML 읽기는 라이선스 기능입니다. 온프레미스 라이선스를 적용하지 않으면 리더가 <code>MedicalApiException</code>을 발생시키므로, 먼저 라이선스를 적용하십시오. 자세한 내용은 <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">licensing guide</a>를 참고하십시오.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="스트림, 파이프 및 비동기">}}

<p>모든 진입점은 스트림 오버로드와 비동기 오버로드를 제공하며, 비동기 버전은 <code>PipeReader</code>도 허용합니다. 웹 응답에서 도착하는 문서는 문자열로 변환하지 않고 바로 읽으면서 파싱됩니다.</p>

<div class="codeblock" id="code">
 <h3>스트림에서 XML 읽기 - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="단일 스트림에서 연속 문서">}}

<p>다른 시스템에서의 내보내기는 종종 단일 스트림에 <code>NativeDicomModel</code> 요소가 순차적으로 들어 있습니다. <code>DeserializeAsyncEnumerable</code>는 각 요소당 하나의 데이터셋을 입력 순서대로 반환하므로 스트림을 메모리에 보관하지 않고 처리할 수 있습니다. 요소들은 직접 이어지며, XML 선언은 어느 XML 입력과 마찬가지로 시작 부분에만 허용됩니다.</p>

<div class="codeblock" id="code">
 <h3>연속 문서를 스트리밍 - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="대용량 데이터 참조">}}

<p>픽셀 데이터와 같은 큰 값은 인라인으로 기록되지 않고, 바이트를 가리키는 URI를 가진 <code>BulkData</code> 요소로 나타나 문서 크기를 작게 유지합니다. 읽는 동안 이러한 참조를 해결하려면 직렬 변환기에 대용량 데이터 로더를 제공하십시오. <code>DefaultBulkDataLoader</code>는 인증 없이 <code>file</code>, <code>http</code>, <code>https</code> URI를 가져옵니다; 인증이 필요한 아카이브의 경우 <code>IBulkDataLoader</code> 또는 <code>IAsyncBulkDataLoader</code>를 직접 구현하십시오.</p>

<div class="codeblock" id="code">
 <h3>읽는 동안 대용량 데이터 해결 - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM to XML과의 라운드 트립">}}

<p>두 방향은 함께 사용하도록 설계되었습니다: 연구가 XML 형태로 내보내지고, XML을 사용하는 시스템을 거쳐 다시 DICOM 파일로 반환됩니다. 모든 것이 .NET으로 관리되므로 동일한 라운드 트립이 Windows, Linux, macOS에서 실행됩니다.</p>

<div class="codeblock" id="code">
 <h3>DICOM to XML 및 복귀 - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>XML의 형태를 제어하는 옵션은 <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> 페이지를 참고하십시오. 동일한 페어가 JSON에도 존재합니다: <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> 및 <a href="/medical/net/json-to-dicom/">JSON to DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">serialization guide</a>는 전체 API를 다룹니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="학습 자료" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="문서" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="개발자 가이드" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="API 참조" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="제품 지원" tabId="support" >}}
{{< blocks/products/pf/slr-element name="무료 지원" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="유료 지원" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="블로그" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle=".NET용 Aspose.Medical이 왜 필요한가?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="고객 목록" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="성공 사례" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}