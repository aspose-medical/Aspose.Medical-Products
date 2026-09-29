---
title: Anonymize a DICOM Study in C# .NET | Aspose.Medical
weight: 1100
description: Anonymize every file of a DICOM study in C# .NET and keep Study, Series and Frame of Reference UIDs consistent across files, so the de-identified study still opens as one study.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Anonymize a Whole DICOM Study in .NET C#" h2="De-identify every file of a study with DICOM PS 3.15 confidentiality profiles and keep the replacement UIDs consistent across files, so viewers and archives still see one study with its series." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Why a Study Needs Consistent UIDs">}}

<p>A DICOM study is not one file. A CT or MR study is often hundreds of files, one per image, and they belong together only through their UIDs: every file carries the same Study Instance UID, the images of one series share a Series Instance UID, the images of one scan share a Frame of Reference UID, and presentation states, key object selections and derived images point to other images by their SOP Instance UIDs.</p>

<p>The <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Basic Profile</a> replaces these UIDs, because a UID can lead back to the hospital and the patient. The standard also requires the new UIDs to be internally consistent within the set of instances (action code U). If every file gets unrelated new UIDs, a viewer shows each image as a separate study, and the anonymized data is no longer usable for research or for a second opinion.</p>

<p><strong>Aspose.Medical for .NET</strong> keeps the replacement UIDs consistent for all files that go through the same <code>Anonymizer</code> object. This page shows how to anonymize a whole study, how to check the result, and what the current API does not cover.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonymize a Whole Study in C#">}}

<p>Create one <code>Anonymizer</code> and pass every file of the study through it. The anonymizer keeps a map from each original UID to its replacement, so the same original UID gets the same new UID in every file:</p>

<div class="codeblock" id="code">
 <h3>Anonymize all files of a study - C#</h3>
 <pre><code class="cs">string inputFolder = "study";
string outputFolder = "study_anonymized";
Directory.CreateDirectory(outputFolder);

// One anonymizer for the whole study: it remembers every UID it replaces,
// so the same original UID gets the same new UID in every file
ConfidentialityProfile profile = ConfidentialityProfile.CreateDefault(ConfidentialityProfileOptions.BasicProfile);
Anonymizer anonymizer = new(profile);

foreach (string path in Directory.EnumerateFiles(inputFolder, "*.dcm", SearchOption.AllDirectories))
{
    DicomFile file = DicomFile.Open(path);
    anonymizer.AnonymizeInPlace(file);

    // Name the output by the new SOP Instance UID: original file names can contain patient data
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine(outputFolder, sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p>Only one file is in memory at a time, so the size of the study does not matter. The same <code>Anonymizer</code> can also process several studies in one run: each original UID has its own replacement, so different studies stay different.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="What Stays Linked After Anonymization">}}

<p>With the Basic Profile and one <code>Anonymizer</code> for the whole study:</p>

<ul>
<li><strong>Study Instance UID</strong> is replaced with one new UID, the same in every file of the study.</li>
<li><strong>Series Instance UID</strong> is replaced with one new UID per series, so the series structure of the study is kept.</li>
<li><strong>Frame of Reference UID</strong> is replaced consistently, so images of one scan still share a patient coordinate system.</li>
<li><strong>SOP Instance UID</strong> is replaced with a new UID per file, and the Media Storage SOP Instance UID in the file meta information is updated to match.</li>
<li><strong>References inside sequences</strong> that the profile keeps, such as the Referenced SOP Instance UID in the evidence sequence of a key object selection, are replaced with the same new UIDs as the files they point to.</li>
</ul>

<p>The new UIDs use the 2.25 root, derived from a random UUID, as defined in DICOM PS 3.5. They carry no information about the original UIDs.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Check the Result">}}

<p>Before you share an anonymized study, check that it is still one study. Read the attributes only, without the pixel data, and count the distinct UIDs:</p>

<div class="codeblock" id="code">
 <h3>Count studies and series after anonymization - C#</h3>
 <pre><code class="cs">HashSet&lt;string&gt; studies = [];
HashSet&lt;string&gt; series = [];

foreach (string path in Directory.EnumerateFiles("study_anonymized", "*.dcm"))
{
    // Read the attributes only, without the pixel data
    DicomFile file = DicomFile.Open(path, ReadDicomFileOptions.Default, TagDataReadingStrategies.SkipLargeTags());
    studies.Add(file.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID));
    series.Add(file.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID));
}

Console.WriteLine($"{studies.Count} study, {series.Count} series");</code></pre>
</div>

<p>One study with the same number of series as the original means the UIDs were replaced consistently. More studies than expected means the files went through different <code>Anonymizer</code> objects.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonymize Large Studies in Parallel">}}

<p>The UID map of the <code>Anonymizer</code> is safe to use from several threads. For studies with thousands of files, share one anonymizer between the threads of a parallel loop:</p>

<div class="codeblock" id="code">
 <h3>Parallel study anonymization - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study_anonymized");

ConfidentialityProfile profile = ConfidentialityProfile.CreateDefault(ConfidentialityProfileOptions.BasicProfile);
Anonymizer anonymizer = new(profile);

// All threads share one anonymizer, and with it one set of replacement UIDs
Parallel.ForEach(Directory.EnumerateFiles("study", "*.dcm", SearchOption.AllDirectories), path =>
{
    DicomFile file = DicomFile.Open(path);
    anonymizer.AnonymizeInPlace(file);

    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study_anonymized", sopInstanceUid + ".dcm"));
});</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Keep Links to Referenced Images">}}

<p>The Basic Profile removes two sequences that point to other images: Referenced Image Sequence (0008,1140) and Source Image Sequence (0008,2112). This is allowed by the standard, but a presentation state then loses the images it applies to, and a derived image loses its source images.</p>

<p>To keep these links, change the action for the two sequences to K. With action K the anonymizer keeps the sequence and processes its items, so each Referenced SOP Instance UID gets the same new UID as the image it points to:</p>

<div class="codeblock" id="code">
 <h3>Keep image references in a study - C#</h3>
 <pre><code class="cs">ConfidentialityProfile profile = ConfidentialityProfile.CreateDefault(ConfidentialityProfileOptions.BasicProfile);

// Referenced Image Sequence and Source Image Sequence are removed by the Basic Profile.
// Action K keeps them, and the anonymizer then processes their items, so each
// Referenced SOP Instance UID gets the same new UID as the image it points to.
string[] sequences = ["0008,1140", "0008,2112"];
foreach (Regex rule in profile.Keys.Where(rule =&gt; sequences.Contains(rule.ToString())).ToList())
    profile[rule] = ConfidentialityProfileActions.K;

Anonymizer anonymizer = new(profile);</code></pre>
</div>

<p>Use this anonymizer in the study loop above. The same approach works for any rule of the profile: <code>ConfidentialityProfile</code> is a dictionary of tag patterns and actions, and you can change it before you create the <code>Anonymizer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Limits to Know Before You Start">}}

<ul>
<li><strong>One study, one run.</strong> The UID map lives in memory inside one <code>Anonymizer</code> object, and the new UIDs are random. A second <code>Anonymizer</code>, a second run of the program or a second server process gives the same original UID a different new UID. The map cannot be saved or loaded, so files that arrive later cannot be added to a study that is already anonymized. Collect all files of a study first, then anonymize them together.</li>
<li><strong>Burned-in text is not removed.</strong> Anonymization works on DICOM attributes. Text that is part of the image itself, such as a patient name burned into an ultrasound frame or a scanned document, stays in the pixel data.</li>
<li><strong>Original UIDs can be kept.</strong> If your data does not need new UIDs, for example inside one organization, the <code>RetainUIDs</code> option keeps all UIDs unchanged and the study structure stays as it was. See the <a href="/medical/net/anonymization/">DICOM anonymization page</a> for all profile options.</li>
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
