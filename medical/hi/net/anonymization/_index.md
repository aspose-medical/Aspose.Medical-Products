---
title: C# .NET में DICOM फ़ाइलों को अनाम किएँ | Aspose.Medical
weight: 1000
description: कॉन्फ़िगर करने योग्य गोपनीयता प्रोफ़ाइल्स का उपयोग करके C# .NET में DICOM फ़ाइलों को अनाम बनाएं। मरीज की PII हटाएँ, Aspose.Medical API के साथ HIPAA और GDPR अनुपालन सुनिश्चित करें।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="C# .NET में DICOM फ़ाइलों को अनाम किएँ" h2="DICOM PS 3.15 गोपनीयता प्रोफ़ाइल्स का उपयोग करके DICOM फ़ाइलों से रोगी की पहचान संबंधी जानकारी हटाएँ। शुद्ध .NET लाइब्रेरी के साथ HIPAA और GDPR अनुपालन सुनिश्चित करें।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="मानकों-आधारित DICOM अनामिकरण">}}

<p><strong>Aspose.Medical for .NET</strong> <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Confidentiality Profiles</a> पर आधारित एक व्यापक DICOM अनामिकरण API प्रदान करता है। यह API स्वास्थ्य‑सेवा डेवलपर्स को DICOM फ़ाइलों से रोगी की पहचान संबंधी जानकारी (PII) को हटाने या संशोधित करने में सक्षम बनाती है, जबकि इमेजिंग डेटा का क्लिनिकल मूल्य संरक्षित रहता है। सरल टैग‑हटाने वाले तरीकों के विपरीत, Aspose.Medical आधिकारिक DICOM मानक का पालन करता है ताकि उचित डि‑आईडेंटिफ़िकेशन सुनिश्चित किया जा सके।</p>

<p><code>Anonymizer</code> क्लास <code>ConfidentialityProfile</code> ऑब्जेक्ट्स के साथ काम करती है जो यह परिभाषित करते हैं कि प्रत्येक DICOM एट्रिब्यूट को कैसे संभालना है — चाहे उसे हटाना हो, डमी मान से बदलना हो, साफ़ करना हो, या जैसा है वैसा रखना हो। यह दृष्टिकोण नियामक आवश्यकताओं को पूरा करने वाला सुसंगत, ऑडिट‑योग्य अनामिकरण सुनिश्चित करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# में DICOM फ़ाइल को अनाम बनाएँ">}}

<p>DICOM फ़ाइल को अनाम करने का सबसे सरल तरीका डिफ़ॉल्ट गोपनीयता प्रोफ़ाइल का उपयोग करना है। यह DICOM PS 3.15 बेसिक प्रोफ़ाइल को लागू करता है, जो सभी मानक रोगी‑पहचान एट्रिब्यूट को संभालती है:</p>

<div class="codeblock" id="code">
 <h3>बेसिक DICOM अनामिकरण - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p><code>Anonymize</code> मेथड <strong>non-destructive</strong> है &mdash; यह मूल डेटासेट की क्लोन बनाता है और एक नई अनामीकृत कॉपी लौटाता है। मूल DICOM फ़ाइल अपरिवर्तित रहती है, जो ऑडिट ट्रेल्स और डेटा इंटेग्रिटी वर्कफ़्लो के लिए आवश्यक है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="कस्टम रोगी पहचान प्रतिस्थापन">}}

<p>कई वर्कफ़्लो में, आपको रोगी की जानकारी को केवल हटाने के बजाय विशिष्ट उपनाम मानों से बदलने की आवश्यकता होती है। <code>ConfidentialityProfile</code> क्लास आपको रोगी नाम और रोगी ID के लिए प्रतिस्थापन मान सेट करने की अनुमति देती है:</p>

<div class="codeblock" id="code">
 <h3>कस्टम रोगी पहचान के साथ अनामिकरण - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="इन‑प्लेस अनामिकरण">}}

<p>ऐसे परिदृश्यों में जहाँ मेमोरी दक्षता महत्वपूर्ण है या आपको मूल डेटा रखने की आवश्यकता नहीं है, API इन‑प्लेस अनामिकरण प्रदान करती है जो डेटासेट को सीधे संशोधित करती है:</p>

<div class="codeblock" id="code">
 <h3>इन‑प्लेस DICOM अनामिकरण - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p><code>Anonymize</code> और <code>AnonymizeInPlace</code> दोनों मेथड सीधे एक <code>Dataset</code> ऑब्जेक्ट स्वीकार करते हैं, जिससे आपको यह नियंत्रित करने की पूरी स्वतंत्रता मिलती है कि कौन सा डेटासेट प्रोसेस किया जाए।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="कॉन्फ़िगर करने योग्य गोपनीयता प्रोफ़ाइल विकल्प">}}

<p>DICOM PS 3.15 मानक एक <strong>Basic Profile</strong> के साथ कई वैकल्पिक प्रोफ़ाइल्स को परिभाषित करता है जो अनामिकरण के दौरान विशिष्ट डेटा वर्गों को कैसे संभालते हैं। आप इन विकल्पों को <code>ConfidentialityProfileOptions</code> enum का उपयोग करके संयोजित कर सकते हैं:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>विकल्प</th>
<th>विवरण</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>डिफ़ॉल्ट DICOM PS 3.15 बेसिक एप्लिकेशन लेवल कॉन्फिडेंशियलिटी प्रोफ़ाइल</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>अनामिकरण के दौरान सुरक्षित निजी एट्रिब्यूट्स को बरकरार रखें</td></tr>
<tr><td><code>RetainUIDs</code></td><td>मूल DICOM UID (Study, Series, Instance) को अपरिवर्तित रखें</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>डिवाइस पहचान एट्रिब्यूट्स (निर्माता, मॉडल, स्टेशन नाम) को बरकरार रखें</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>संस्था पहचान एट्रिब्यूट्स (नाम, पता, विभाग) को बरकरार रखें</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>शोध उद्देश्यों के लिए रोगी विशेषताओं (आयु, लिंग, आकार, वजन) को बरकरार रखें</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>लॉन्गिट्यूडिनल स्टडीज के लिए पूर्ण तिथि और समय मानों को बरकरार रखें</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>लॉन्गिट्यूडिनल स्टडीज के लिए संशोधित तिथियों को बरकरार रखें</td></tr>
<tr><td><code>CleanDesc</code></td><td>ऐसे टेक्स्ट विवरणों को साफ़ करें जो पहचान संबंधी जानकारी रख सकते हैं</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>ऐसे संरचित कंटेंट को साफ़ करें जो पहचान संबंधी जानकारी रख सकते हैं</td></tr>
<tr><td><code>CleanGraph</code></td><td>ऐसे ग्राफिकल डेटा को साफ़ करें जो रोगी की बर्न‑इन जानकारी रख सकता है</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>विशिष्ट प्रोफ़ाइल विकल्पों के साथ अनामिकरण - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="एट्रिब्यूट हैंडलिंग के लिए एक्शन कोड्स">}}

<p>गोपनीयता प्रोफ़ाइल में प्रत्येक एट्रिब्यूट को एक एक्शन कोड असाइन किया जाता है जो अनामिकरण के दौरान उसकी प्रोसेसिंग को परिभाषित करता है। ये DICOM PS 3.15 Table E.1-1a मानक का पालन करते हैं:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>कोड</th>
<th>क्रिया</th>
<th>विवरण</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>डमी मान से बदलें</td><td>VR के अनुरूप गैर‑शून्य लंबाई वाला मान रखे</td></tr>
<tr><td><strong>Z</strong></td><td>शून्य‑लंबाई या डमी</td><td>VR के अनुरूप शून्य‑लंबाई वाला मान या डमी मान डालें</td></tr>
<tr><td><strong>X</strong></td><td>हटाएँ</td><td>डेटासेट से एट्रिब्यूट को पूरी तरह हटाएँ</td></tr>
<tr><td><strong>K</strong></td><td>राखें</td><td>एट्रिब्यूट को अपरिवर्तित रखें (गैर‑सीक्वेंस के लिए) या साफ़ रखें (सीक्वेंस के लिए)</td></tr>
<tr><td><strong>C</strong></td><td>साफ़ करें</td><td>VR के अनुरूप समान अर्थ वाला साफ़ किया गया मान डालें</td></tr>
<tr><td><strong>U</strong></td><td>UID बदलें</td><td>इंस्टेंस सेट के भीतर आंतरिक रूप से संगत नया UID डालें</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="CSV, JSON और XML से कस्टम प्रोफ़ाइल">}}

<p>उन्नत उपयोग मामलों के लिए, आप बाहरी CSV, JSON या XML फ़ाइलों से नियम लोड करके कस्टम अनामिकरण प्रोफ़ाइल परिभाषित कर सकते हैं। यह आपको आपके विशेष संगठनात्मक नीतियों या नियामक आवश्यकताओं के अनुसार अनामिकरण व्यवहार को अनुकूलित करने की अनुमति देता है:</p>

<div class="codeblock" id="code">
 <h3>CSV से अनामिकरण प्रोफ़ाइल लोड करें - C#</h3>
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
 <h3>JSON से अनामिकरण प्रोफ़ाइल लोड करें - C#</h3>
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

<p>कस्टम प्रोफ़ाइल को XML फ़ाइलों (<code>LoadFromXmlFile</code>), स्ट्रीम्स (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) या सीधे स्ट्रिंग्स (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>) से भी लोड किया जा सकता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HIPAA और GDPR अनुपालन">}}

<p>DICOM फ़ाइलों में नियमित रूप से HIPAA द्वारा परिभाषित प्रोटेक्टेड हेल्थ इंफ़ॉर्मेशन (PHI) और GDPR द्वारा परिभाषित व्यक्तिगत डेटा होता है। Aspose.Medical for .NET नियामक आवश्यकताओं को पूरा करने में मदद करता है, निम्न प्रदान करके:</p>

<ul>
<li><strong>DICOM PS 3.15 compliance</strong>: अनामिकरण इंजन आधिकारिक DICOM मानक का पालन करता है, जिससे सभी संबंधित एट्रिब्यूट्स पर मान्य सर्वोत्तम प्रथाएँ लागू होती हैं।</li>
<li><strong>Comprehensive PII removal</strong>: रोगी का नाम, ID, जन्म तिथि, पता, और अन्य पहचानकर्ता कॉन्फ़िगर की गई गोपनीयता प्रोफ़ाइल के अनुसार संभाले जाते हैं।</li>
<li><strong>UID replacement</strong>: स्टडी, सीरीज़, और SOP इंस्टेंस UID को नए, आंतरिक रूप से संगत UID से बदल सकते हैं ताकि क्रॉस‑रेफ़रेंसिंग के माध्यम से पुनः पहचान से बचा जा सके।</li>
<li><strong>Configurable retention</strong>: जब शोध या क्लिनिकल जरूरतों के कारण आवश्यक हो, तो डेटा के विशिष्ट वर्ग (रोगी विशेषताएँ, तिथियाँ, डिवाइस जानकारी) को मानक विकल्प प्रोफ़ाइल्स का उपयोग करके चयनात्मक रूप से बरकरार रखा जा सकता है।</li>
<li><strong>Auditable process</strong>: एक्शन कोड्स के साथ मानकों पर आधारित दृष्टिकोण एक स्पष्ट, दस्तावेज़ीकृत अनामिकरण प्रक्रिया प्रदान करता है जो नियामक ऑडिट्स के लिए उपयुक्त है।</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="डॉक्यूमेंटेशन" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="सोर्स कोड" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API संदर्भ" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="नि:शुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="वित्तीय समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता कहानियां" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
