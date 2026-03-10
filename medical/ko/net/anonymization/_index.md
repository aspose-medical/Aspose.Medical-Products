---
title: C# .NET에서 DICOM 파일 익명화 | Aspose.Medical
weight: 1000
description: 구성 가능한 기밀성 프로파일을 사용하여 C# .NET에서 DICOM 파일을 익명화합니다. 환자 PII를 제거하고 Aspose.Medical API로 HIPAA 및 GDPR 준수를 보장합니다.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#에서 DICOM 파일 익명화" h2="DICOM PS 3.15 기밀성 프로파일을 사용하여 DICOM 파일에서 환자 식별 정보를 제거합니다. 순수 .NET 라이브러리로 HIPAA 및 GDPR 준수를 보장합니다." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="표준 기반 DICOM 익명화">}}

<p><strong>Aspose.Medical for .NET</strong>은 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Confidentiality Profiles</a>를 기반으로 하는 포괄적인 DICOM 익명화 API를 제공합니다. 이 API는 의료 개발자가 DICOM 파일에서 환자 식별 정보(PII)를 제거하거나 수정하면서 영상 데이터의 임상 가치를 유지할 수 있게 합니다. 단순한 태그 제거 방식과 달리 Aspose.Medical은 공식 DICOM 표준을 따라 적절한 비식별화를 보장합니다.</p>

<p><code>Anonymizer</code> 클래스는 각 DICOM 속성을 어떻게 처리할지 정확히 정의하는 <code>ConfidentialityProfile</code> 객체와 함께 작동합니다 — 제거, 더미 값으로 교체, 정리 혹은 변경 없이 유지 중 어느 방식이든 선택할 수 있습니다. 이 접근 방식은 규제 요구사항을 충족하는 일관되고 감사 가능한 익명화를 보장합니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#에서 DICOM 파일 익명화">}}

<p>DICOM 파일을 익명화하는 가장 간단한 방법은 기본 기밀성 프로파일을 사용하는 것입니다. 이는 DICOM PS 3.15 Basic Profile를 적용하여 모든 표준 환자 식별 속성을 처리합니다:</p>

<div class="codeblock" id="code">
 <h3>기본 DICOM 익명화 - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p><code>Anonymize</code> 메서드는 <strong>비파괴적</strong>이며 &mdash; 원본 데이터셋을 복제하여 새로운 익명화된 복사본을 반환합니다. 원본 DICOM 파일은 변경되지 않아 감사 로그 및 데이터 무결성 워크플로에 필수적입니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="맞춤형 환자 ID 교체">}}

<p>많은 워크플로에서는 환자 정보를 단순히 제거하는 대신 특정 별칭 값으로 교체해야 합니다. <code>ConfidentialityProfile</code> 클래스를 사용하면 환자 이름 및 환자 ID에 대한 교체 값을 설정할 수 있습니다:</p>

<div class="codeblock" id="code">
 <h3>맞춤형 환자 ID를 사용한 익명화 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="인플레이스 익명화">}}

<p>메모리 효율이 중요하거나 원본 데이터를 유지할 필요가 없는 경우, API는 데이터셋을 직접 수정하는 인플레이스 익명화를 제공합니다:</p>

<div class="codeblock" id="code">
 <h3>인플레이스 DICOM 익명화 - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p><code>Anonymize</code>와 <code>AnonymizeInPlace</code> 메서드 모두 <code>Dataset</code> 객체를 직접 받아들이며, 처리할 데이터셋을 완전히 제어할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="구성 가능한 기밀성 프로파일 옵션">}}

<p>DICOM PS 3.15 표준은 <strong>Basic Profile</strong>과 여러 선택적 프로파일을 정의하여 익명화 중 특정 데이터 범주의 처리 방식을 제어합니다. 이러한 옵션은 <code>ConfidentialityProfileOptions</code> 열거형을 사용해 조합할 수 있습니다:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>옵션</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>DICOM PS 3.15 기본 응용 수준 기밀성 프로파일</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>익명화 중 안전한 개인 속성을 유지함</td></tr>
<tr><td><code>RetainUIDs</code></td><td>원본 DICOM UID(Study, Series, Instance)를 변경 없이 유지</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>장치 식별 속성(제조사, 모델, 스테이션 이름)을 유지</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>기관 식별 속성(이름, 주소, 부서)을 유지</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>연구 목적을 위해 환자 특성(연령, 성별, 키, 체중)을 유지</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>종단 연구를 위해 전체 날짜 및 시간 값을 유지</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>종단 연구를 위해 수정된 날짜를 유지</td></tr>
<tr><td><code>CleanDesc</code></td><td>식별 정보를 포함할 수 있는 텍스트 설명을 정리</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>식별 정보를 포함할 수 있는 구조화된 콘텐츠를 정리</td></tr>
<tr><td><code>CleanGraph</code></td><td>내장된 환자 정보를 포함할 수 있는 그래픽 데이터를 정리</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>특정 프로파일 옵션을 사용한 익명화 - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="속성 처리용 액션 코드">}}

<p>기밀성 프로파일의 각 속성은 익명화 중 처리 방식을 정의하는 액션 코드가 지정됩니다. 이는 DICOM PS 3.15 Table E.1-1a 표준을 따릅니다:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>코드</th>
<th>액션</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>더미 값으로 교체</td><td>VR에 일치하는 비-제로 길이 값을 교체</td></tr>
<tr><td><strong>Z</strong></td><td>길이 0 또는 더미</td><td>VR에 일치하는 길이 0 값이나 더미 값을 교체</td></tr>
<tr><td><strong>X</strong></td><td>제거</td><td>데이터셋에서 해당 속성을 완전히 제거</td></tr>
<tr><td><strong>K</strong></td><td>유지</td><td>속성을 변화 없이 유지(시퀀스가 아닌 경우) 또는 정리(시퀀스인 경우)</td></tr>
<tr><td><strong>C</strong></td><td>정리</td><td>VR에 일치하는 유사 의미의 정리된 값으로 교체</td></tr>
<tr><td><strong>U</strong></td><td>UID 교체</td><td>인스턴스 집합 내에서 내부적으로 일관된 새로운 UID로 교체</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="CSV, JSON, XML에서 사용자 지정 프로파일">}}

<p>고급 사용 사례에서는 외부 CSV, JSON, XML 파일에서 규칙을 로드하여 사용자 지정 익명화 프로파일을 정의할 수 있습니다. 이를 통해 조직의 정책이나 규제 요구사항에 맞게 익명화 동작을 조정할 수 있습니다:</p>

<div class="codeblock" id="code">
 <h3>CSV에서 익명화 프로파일 로드 - C#</h3>
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
 <h3>JSON에서 익명화 프로파일 로드 - C#</h3>
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

<p>사용자 지정 프로파일은 XML 파일(<code>LoadFromXmlFile</code>), 스트림(<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) 또는 문자열 직접(<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>)에서도 로드할 수 있습니다.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HIPAA 및 GDPR 준수">}}

<p>DICOM 파일에는 HIPAA에서 정의한 보호된 의료 정보(PHI)와 GDPR에서 정의한 개인 데이터가 정기적으로 포함됩니다. Aspose.Medical for .NET은 다음과 같은 기능을 제공하여 규제 요구사항을 충족하도록 돕습니다:</p>

<ul>
<li><strong>DICOM PS 3.15 준수</strong>: 익명화 엔진은 공식 DICOM 비식별화 표준을 따르며, 모든 관련 속성에 인정된 모범 사례가 적용됩니다.</li>
<li><strong>포괄적인 PII 제거</strong>: 환자 이름, ID, 생년월일, 주소 및 기타 식별자는 구성된 기밀성 프로파일에 따라 처리됩니다.</li>
<li><strong>UID 교체</strong>: Study, Series, SOP Instance UID를 새로운 내부 일관성을 가진 UID로 교체하여 교차 참조를 통한 재식별을 방지합니다.</li>
<li><strong>구성 가능한 보존</strong>: 연구 또는 임상 필요에 따라 특정 데이터 범주(환자 특성, 날짜, 장치 정보)를 표준 옵션 프로파일을 사용해 선택적으로 보존할 수 있습니다.</li>
<li><strong>감사 가능한 프로세스</strong>: 정의된 액션 코드를 활용한 표준 기반 접근 방식은 규제 감사를 위한 명확하고 문서화 가능한 익명화 프로세스를 제공합니다.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="학습 자료" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="문서" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="소스 코드" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API 레퍼런스" href="https://reference.aspose.com/medical/net/" >}}
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
