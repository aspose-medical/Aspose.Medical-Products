---
title: C# .NET में JSON को DICOM में परिवर्तित करें | Aspose.Medical
weight: 6000

description: C# .NET में मानक DICOM JSON मॉडल (PS3.18) से DICOM फ़ाइलें बनाएं। स्ट्रिंग, स्ट्रीम या पाइप से JSON पढ़ें, डेटासेट्स की एक श्रृंखला को स्ट्रीम करें, और Aspose.Medical API के साथ बल्क डेटा रेफ़रेंस को हल करें।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में JSON को DICOM में परिवर्तित करें" h2="मानक DICOM JSON मॉडल (PS3.18) को डेटासेट्स और DICOM फ़ाइलों में पुनः पढ़ें। स्ट्रिंग, स्ट्रीम या पाइप से काम करें, स्टडीज़ की एक श्रृंखला को स्ट्रीम करें, और बल्क डेटा रेफ़रेंस को हल करें।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM JSON से DICOM फ़ाइल तक">}}

<p><strong>Aspose.Medical for .NET</strong> पढ़ता है <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>, वह प्रतिनिधित्व जो DICOMweb सेवाओं और HTTP के माध्यम से स्टडीज़ का आदान-प्रदान करने वाले सिस्टमों द्वारा उपयोग किया जाता है। जो JSON के रूप में आता है वह एक <code>Dataset</code> बन जाता है, और एक <code>Dataset</code> को डिस्क पर DICOM फ़ाइल के रूप में लिखा जाता है।</p>

<p>यह <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> पृष्ठ की विपरीत दिशा है, और दोनों एक ही क्लास, <code>DicomJsonSerializer</code> का उपयोग करते हैं।</p>

<div class="codeblock" id="code">
 <h3>JSON से DICOM फ़ाइल बनाएं - C#</h3>
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

<p>एक डेटासेट जिसमें फ़ाइल मेटा सूचना नहीं होती, उसे डिफ़ॉल्ट ट्रांसफ़र सिंटैक्स, Implicit VR Little Endian, के साथ लिखा जाता है, जब इसे <code>DicomFile</code> में रैप किया जाता है।</p>

<p>DICOM JSON पढ़ना एक लाइसेंस्ड फ़ीचर है। यदि ऑन-प्रेमाइसे लाइसेंस लागू नहीं किया गया तो रीडर <code>MedicalApiException</code> फेंकता है, इसलिए पहले लाइसेंस लागू करें, जैसा कि <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">लाइसेंस गाइड</a> में बताया गया है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="फ़ाइल मेटा सूचना रखें">}}

<p><code>Deserialize</code> केवल डेटासेट लौटाता है। जब JSON दस्तावेज़ में फ़ाइल मेटा सूचना समूह भी होता है, उदाहरण के लिए क्योंकि वह एक पूर्ण DICOM फ़ाइल से उत्पन्न हुआ था, तो <code>DeserializeFile</code> एक <code>DicomFile</code> लौटाता है जिसमें वह समूह पूरी तरह से बना रहता है, जिसमें फ़ाइल द्वारा घोषित ट्रांसफ़र सिंटैक्स भी शामिल है।</p>

<div class="codeblock" id="code">
 <h3>JSON से पूर्ण DICOM फ़ाइल पढ़ें - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="स्ट्रीम, पाइप और असिंक्रोनस">}}

<p>प्रत्येक एंट्री पॉइंट के पास एक स्ट्रीम ओवरलोड और एक असिंक्रोनस ओवरलोड होता है, और असिंक्रोनस वाले <code>PipeReader</code> को भी स्वीकार करते हैं। वेब प्रतिक्रिया या डिस्क से प्राप्त होने वाला दस्तावेज़ पहले स्ट्रिंग में बदले बिना पढ़ा जाता है, जो तब महत्त्वपूर्ण होता है जब JSON में पिक्सेल डेटा होता है।</p>

<div class="codeblock" id="code">
 <h3>स्ट्रीम से JSON पढ़ें - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="डेटासेट्स की एक श्रृंखला, एक बार में एक">}}

<p>DICOMweb क्वेरी डेटासेट्स की एक एरे के साथ उत्तर देती है, और ऐसा दस्तावेज़ बड़ा हो सकता है। <code>DeserializeList</code> पूरी एरे को मेमोरी में पढ़ता है; <code>DeserializeAsyncEnumerable</code> एक समय में एक डेटासेट प्रदान करता है, इसलिए दस्तावेज़ कभी भी पूरी तरह मेमोरी में नहीं रहता।</p>

<div class="codeblock" id="code">
 <h3>डेटासेट्स की एरे को स्ट्रीम करें - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="बल्क डेटा रेफ़रेंस">}}

<p>DICOM JSON मॉडल पिक्सेल डेटा इनलाइन नहीं रखता। बड़े मानों को <code>BulkDataURI</code> से बदला जाता है जो बाइट्स की ओर संकेत करता है, जिससे JSON दस्तावेज़ छोटा रहता है। पढ़ते समय उन रेफ़रेंस को हल करने के लिए, सीरियलाइज़र को एक बल्क डेटा लोडर प्रदान करें। <code>DefaultBulkDataLoader</code> बिना प्रमाणीकरण के <code>file</code>, <code>http</code> और <code>https</code> URI को फ़ेच करता है; यदि एक आर्काइव को प्रमाणपत्रों की आवश्यकता है, तो स्वयं <code>IBulkDataLoader</code> या <code>IAsyncBulkDataLoader</code> लागू करें।</p>

<div class="codeblock" id="code">
 <h3>पढ़ते समय BulkDataURI हल करें - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM से JSON तक राउंड ट्रिप">}}

<p>दोनों दिशाओं को साथ में उपयोग करने के लिए डिज़ाइन किया गया है: एक स्टडी JSON के रूप में निकलती है, वेब सेवा के माध्यम से यात्रा करती है, और फिर DICOM फ़ाइल के रूप में वापस आती है। प्रक्रिया में कोई भी नेटिव कोड निर्भर नहीं करता, इसलिए वही राउंड ट्रिप Windows, Linux और macOS पर चलता है।</p>

<div class="codeblock" id="code">
 <h3>DICOM से JSON और वापस - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>JSON के स्वरूप को नियंत्रित करने वाले विकल्पों के लिए, <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> पृष्ठ देखें। XML के लिए भी वही जोड़ी मौजूद है: <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> और <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">JSON सीरियलाइज़ेशन गाइड</a> पूरे API को कवर करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="API रेफ़रेंसेज़" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="नि:शुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="भुगतानित समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता की कहानियां" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}