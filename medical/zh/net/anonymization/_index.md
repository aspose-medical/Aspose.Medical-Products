---
title: 在 C# .NET 中匿名化 DICOM 文件 | Aspose.Medical
weight: 1000
description: 在 C# .NET 中使用可配置的保密性配置文件匿名化 DICOM 文件。移除患者 PII，确保符合 HIPAA 和 GDPR 要求，使用 Aspose.Medical API。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="在 .NET C# 中匿名化 DICOM 文件" h2="使用 DICOM PS 3.15 保密性配置文件从 DICOM 文件中移除患者身份信息。通过纯 .NET 库确保符合 HIPAA 和 GDPR 要求。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="基于标准的 DICOM 匿名化">}}

<p><strong>Aspose.Medical for .NET</strong> 提供基于 <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Confidentiality Profiles</a> 的完整 DICOM 匿名化 API。该 API 使医疗开发者能够从 DICOM 文件中删除或修改患者身份信息（PII），同时保留影像数据的临床价值。不同于简单的标签删除方法，Aspose.Medical 遵循官方 DICOM 标准以确保正确的去标识化。</p>

<p><code>Anonymizer</code> 类与 <code>ConfidentialityProfile</code> 对象配合使用，精确定义每个 DICOM 属性的处理方式——是删除、替换为虚拟值、清理还是保持不变。此方法确保了一致且可审计的匿名化，满足监管要求。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="在 C# 中匿名化 DICOM 文件">}}

<p>匿名化 DICOM 文件的最简单方法是使用默认的保密性配置文件。这将应用 DICOM PS 3.15 基本配置文件，处理所有标准的患者身份属性：</p>

<div class="codeblock" id="code">
 <h3>基本 DICOM 匿名化 - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p><code>Anonymize</code> 方法是 <strong>非破坏性</strong> 的 &mdash; 它克隆原始数据集并返回一个新的匿名化副本。原始 DICOM 文件保持不变，这对审计追踪和数据完整性工作流至关重要。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="自定义患者身份替换">}}

<p>在许多工作流中，您需要用特定的别名值替换患者信息，而非仅仅删除。<code>ConfidentialityProfile</code> 类允许您为患者姓名和患者 ID 设置替换值：</p>

<div class="codeblock" id="code">
 <h3>使用自定义患者身份进行匿名化 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="就地匿名化">}}

<p>对于内存效率重要或不需要保留原始数据的场景，API 提供就地匿名化，直接修改数据集：</p>

<div class="codeblock" id="code">
 <h3>就地 DICOM 匿名化 - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p><code>Anonymize</code> 和 <code>AnonymizeInPlace</code> 方法也都直接接受 <code>Dataset</code> 对象，让您完全控制要处理的数据集。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="可配置的保密性配置文件选项">}}

<p>DICOM PS 3.15 标准定义了一个 <strong>Basic Profile</strong> 以及若干可选配置文件，用于控制匿名化期间特定数据类别的处理方式。您可以使用 <code>ConfidentialityProfileOptions</code> 枚举组合这些选项：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>选项</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>默认的 DICOM PS 3.15 基本应用层保密性配置文件</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>在匿名化过程中保留安全的私有属性</td></tr>
<tr><td><code>RetainUIDs</code></td><td>保持原始 DICOM UID（Study、Series、Instance）不变</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>保留设备标识属性（制造商、型号、工作站名称）</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>保留机构标识属性（名称、地址、部门）</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>为研究目的保留患者特征（年龄、性别、体型、体重）</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>保留纵向研究的完整日期和时间值</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>保留纵向研究的修改日期</td></tr>
<tr><td><code>CleanDesc</code></td><td>清理可能包含身份信息的文本描述</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>清理可能包含身份信息的结构化内容</td></tr>
<tr><td><code>CleanGraph</code></td><td>清理可能包含嵌入式患者信息的图形数据</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>使用特定配置文件选项进行匿名化 - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="属性处理的操作码">}}

<p>保密性配置文件中的每个属性都分配了一个操作码，定义其在匿名化期间的处理方式。这些遵循 DICOM PS 3.15 表 E.1-1a 标准：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>代码</th>
<th>操作</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>使用虚拟值替代</td><td>使用与 VR 相一致的非零长度值进行替代</td></tr>
<tr><td><strong>Z</strong></td><td>零长度或虚拟</td><td>使用零长度值或与 VR 相一致的虚拟值进行替代</td></tr>
<tr><td><strong>X</strong></td><td>删除</td><td>从数据集中完全移除该属性</td></tr>
<tr><td><strong>K</strong></td><td>保留</td><td>保持属性不变（非序列）或清理（序列）</td></tr>
<tr><td><strong>C</strong></td><td>清理</td><td>使用与 VR 相一致的、意义相似的清理后值进行替代</td></tr>
<tr><td><strong>U</strong></td><td>替换 UID</td><td>使用在实例集合内部一致的新 UID 进行替代</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="从 CSV、JSON 和 XML 定制配置文件">}}

<p>对于高级用例，您可以通过从外部 CSV、JSON 或 XML 文件加载规则来定义自定义匿名化配置文件。这使您能够根据特定的组织策略或监管要求定制匿名化行为：</p>

<div class="codeblock" id="code">
 <h3>从 CSV 加载匿名化配置文件 - C#</h3>
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
 <h3>从 JSON 加载匿名化配置文件 - C#</h3>
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

<p>自定义配置文件也可以从 XML 文件（<code>LoadFromXmlFile</code>）、流（<code>LoadFromJsonStream</code>、<code>LoadFromXmlStream</code>）或直接从字符串（<code>LoadFromCsvString</code>、<code>LoadFromJsonString</code>、<code>LoadFromXmlString</code>）加载。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HIPAA 与 GDPR 合规性">}}

<p>DICOM 文件通常包含 HIPAA 所定义的受保护健康信息（PHI）以及 GDPR 所定义的个人数据。Aspose.Medical for .NET 通过以下方式帮助您满足监管要求：</p>

<ul>
<li><strong>DICOM PS 3.15 合规</strong>：匿名化引擎遵循官方 DICOM 标准进行去标识化，确保对所有相关属性应用公认的最佳实践。</li>
<li><strong>全面的 PII 移除</strong>：患者姓名、ID、出生日期、地址等标识信息根据配置的保密性配置文件进行处理。</li>
<li><strong>UID 替换</strong>：Study、Series 和 SOP Instance UID 可替换为新的、在内部一致的 UID，以防止通过交叉引用重新识别。</li>
<li><strong>可配置的保留</strong>：当研究或临床需求需要时，可使用标准选项配置文件有选择地保留特定类别的数据（患者特征、日期、设备信息）。</li>
<li><strong>可审计的流程</strong>：基于标准的做法结合定义的操作码，提供清晰、可文档化的匿名化流程，以满足监管审计。</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学习资源" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="文档" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="源代码" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API 参考" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="产品支持" tabId="support" >}}
{{< blocks/products/pf/slr-element name="免费支持" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="付费支持" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="博客" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="为什么选择 Aspose.Medical for .NET？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="客户名单" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功案例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
