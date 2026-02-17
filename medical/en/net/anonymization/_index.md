---
title: Anonymize DICOM Files in C# .NET | Aspose.Medical
weight: 1000
description: Anonymize DICOM files in C# .NET using configurable confidentiality profiles. Remove patient PII, ensure HIPAA and GDPR compliance with Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Anonymize DICOM Files in .NET C#" h2="Remove patient identifying information from DICOM files using DICOM PS 3.15 confidentiality profiles. Ensure HIPAA and GDPR compliance with a pure .NET library." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standards-Based DICOM Anonymization">}}

<p><strong>Aspose.Medical for .NET</strong> provides a comprehensive DICOM anonymization API based on the <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Confidentiality Profiles</a>. The API enables healthcare developers to remove or modify patient identifying information (PII) from DICOM files while preserving the clinical value of the imaging data. Unlike simple tag-removal approaches, Aspose.Medical follows the official DICOM standard to ensure proper de-identification.</p>

<p>The <code>Anonymizer</code> class works with <code>ConfidentialityProfile</code> objects that define exactly how each DICOM attribute should be handled &mdash; whether it should be removed, replaced with a dummy value, cleaned, or kept unchanged. This approach ensures consistent, auditable anonymization that meets regulatory requirements.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonymize a DICOM File in C#">}}

<p>The simplest way to anonymize a DICOM file is to use the default confidentiality profile. This applies the DICOM PS 3.15 Basic Profile, which handles all standard patient-identifying attributes:</p>

<div class="codeblock" id="code">
 <h3>Basic DICOM anonymization - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>The <code>Anonymize</code> method is <strong>non-destructive</strong> &mdash; it clones the original dataset and returns a new anonymized copy. The original DICOM file remains unchanged, which is essential for audit trails and data integrity workflows.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Custom Patient Identity Replacement">}}

<p>In many workflows, you need to replace patient information with specific alias values rather than simply removing it. The <code>ConfidentialityProfile</code> class allows you to set replacement values for the patient name and patient ID:</p>

<div class="codeblock" id="code">
 <h3>Anonymize with custom patient identity - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="In-Place Anonymization">}}

<p>For scenarios where memory efficiency is important or you don't need to keep the original data, the API provides in-place anonymization that modifies the dataset directly:</p>

<div class="codeblock" id="code">
 <h3>In-place DICOM anonymization - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Both <code>Anonymize</code> and <code>AnonymizeInPlace</code> methods also accept a <code>Dataset</code> object directly, giving you full control over which dataset is processed.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Configurable Confidentiality Profile Options">}}

<p>The DICOM PS 3.15 standard defines a <strong>Basic Profile</strong> along with several optional profiles that control how specific categories of data are handled during anonymization. You can combine these options using the <code>ConfidentialityProfileOptions</code> enum:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Option</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>The default DICOM PS 3.15 Basic Application Level Confidentiality Profile</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Retain safe private attributes during anonymization</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Keep original DICOM UIDs (Study, Series, Instance) unchanged</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Retain device identification attributes (manufacturer, model, station name)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Retain institution identification attributes (name, address, department)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Retain patient characteristics (age, sex, size, weight) for research purposes</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Retain full date and time values for longitudinal studies</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Retain modified dates for longitudinal studies</td></tr>
<tr><td><code>CleanDesc</code></td><td>Clean text descriptions that may contain identifying information</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Clean structured content that may contain identifying information</td></tr>
<tr><td><code>CleanGraph</code></td><td>Clean graphical data that may contain burned-in patient information</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Anonymize with specific profile options - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Action Codes for Attribute Handling">}}

<p>Each attribute in a confidentiality profile is assigned an action code that defines how it should be processed during anonymization. These follow the DICOM PS 3.15 Table E.1-1a standard:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Code</th>
<th>Action</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Replace with dummy</td><td>Replace with a non-zero length value consistent with the VR</td></tr>
<tr><td><strong>Z</strong></td><td>Zero-length or dummy</td><td>Replace with a zero-length value or a dummy value consistent with the VR</td></tr>
<tr><td><strong>X</strong></td><td>Remove</td><td>Remove the attribute completely from the dataset</td></tr>
<tr><td><strong>K</strong></td><td>Keep</td><td>Keep the attribute unchanged (for non-sequences) or clean (for sequences)</td></tr>
<tr><td><strong>C</strong></td><td>Clean</td><td>Replace with a cleaned value of similar meaning consistent with the VR</td></tr>
<tr><td><strong>U</strong></td><td>Replace UID</td><td>Replace with a new UID that is internally consistent within the set of instances</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Custom Profiles from CSV, JSON, and XML">}}

<p>For advanced use cases, you can define custom anonymization profiles by loading rules from external CSV, JSON, or XML files. This allows you to tailor the anonymization behavior to your specific organizational policies or regulatory requirements:</p>

<div class="codeblock" id="code">
 <h3>Load anonymization profile from CSV - C#</h3>
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
 <h3>Load anonymization profile from JSON - C#</h3>
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

<p>Custom profiles can also be loaded from XML files (<code>LoadFromXmlFile</code>), streams (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>), or directly from strings (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HIPAA and GDPR Compliance">}}

<p>DICOM files routinely contain Protected Health Information (PHI) as defined by HIPAA and personal data as defined by GDPR. Aspose.Medical for .NET helps you meet regulatory requirements by providing:</p>

<ul>
<li><strong>DICOM PS 3.15 compliance</strong>: The anonymization engine follows the official DICOM standard for de-identification, ensuring recognized best practices are applied to all relevant attributes.</li>
<li><strong>Comprehensive PII removal</strong>: Patient name, ID, date of birth, address, and other identifiers are handled according to the configured confidentiality profile.</li>
<li><strong>UID replacement</strong>: Study, Series, and SOP Instance UIDs can be replaced with new, internally consistent UIDs to prevent re-identification through cross-referencing.</li>
<li><strong>Configurable retention</strong>: When research or clinical needs require it, specific categories of data (patient characteristics, dates, device information) can be selectively retained using the standard option profiles.</li>
<li><strong>Auditable process</strong>: The standards-based approach with defined action codes provides a clear, documentable anonymization process for regulatory audits.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Learning Resources" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Source Code" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API References" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Product Support" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Free Support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Paid Support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Why Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Customers List" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Success Stories" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
