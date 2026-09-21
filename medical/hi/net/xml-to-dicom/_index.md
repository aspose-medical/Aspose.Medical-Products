---
title: C# .NET में XML को DICOM में परिवर्तित करें | Aspose.Medical
weight: 5000

description: C# .NET में PS3.19 के Native DICOM Model XML से DICOM फ़ाइलें बनाएं। XML को स्ट्रिंग, स्ट्रीम या पाइप से पढ़ें, क्रमिक दस्तावेज़ों को स्ट्रीम करें, और Aspose.Medical API के साथ bulk data रेफ़रेंस को हल करें।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में XML को DICOM में परिवर्तित करें" h2="PS3.19 के Native DICOM Model XML को datasets और DICOM फ़ाइलों में पुनः पढ़ें। स्ट्रिंग, स्ट्रीम या पाइप से काम करें, क्रमिक दस्तावेज़ों को स्ट्रीम करें, और bulk data रेफ़रेंस को हल करें।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="मानक Native DICOM Model XML">}}

<p><strong>Aspose.Medical for .NET</strong> DICOM PS3.19 में परिभाषित <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a> को पढ़ता है। यह मानक में ही लिखा गया XML प्रतिनिधित्व है, कोई ऐसा फ़ॉर्मेट नहीं जो Aspose ने आविष्कृत किया हो, यही कारण है कि यह इंटीग्रेशन के लिए उपयोगी है: एक सिस्टम जो पहले से ही DICOM को XML के रूप में एक्सचेंज करता है, इस लाइब्रेरी द्वारा स्वीकार किए गए दस्तावेज़ उत्पन्न करता है।</p>

<p>दस्तावेज़ की रूट <code>NativeDicomModel</code> है, और प्रत्येक एट्रिब्यूट एक <code>DicomAttribute</code> एलिमेंट है जो उसका टैग, वैल्यू रिप्रज़ेंटेशन और कीवर्ड धारण करता है:</p>

<div class="codeblock" id="code">
 <h3>Native DICOM Model फ़ॉर्मेट</h3>
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

<p>यह पृष्ठ <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> की विपरीत दिशा है, और दोनों एक ही क्लास, <code>DicomXmlSerializer</code> का उपयोग करते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# में XML से DICOM फ़ाइल बनाएं">}}

<p><code>Deserialize</code> एक दस्तावेज़ को <code>Dataset</code> में बदलता है, और dataset को डिस्क पर DICOM फ़ाइल के रूप में लिखा जाता है।</p>

<div class="codeblock" id="code">
 <h3>XML से DICOM फ़ाइल बनाएं - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Native DICOM Model में File Meta Information समूह नहीं है, इसलिए ट्रांसफ़र सिंटैक्स दस्तावेज़ का भाग नहीं है। एक dataset जो <code>DicomFile</code> में लिपटा हो, डिफ़ॉल्ट ट्रांसफ़र सिंटैक्स, Implicit VR Little Endian के साथ लिखा जाता है। फ़ाइल को किसी अन्य के साथ संग्रहित करने के लिए, इसे ट्रांसकोड करें, जैसा कि <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> पृष्ठ दिखाता है।</p>

<p>DICOM XML पढ़ना एक लाइसेंस प्राप्त फ़ीचर है। बिना ऑन‑प्रेमाइस लाइसेंस लागू किए रीडर एक <code>MedicalApiException</code> फेंकेगा, इसलिए पहले लाइसेंस लागू करें, जैसा कि <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">licensing guide</a> में वर्णित है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="स्ट्रीम, पाइप और असिंक्रोनस">}}

<p>प्रत्येक एंट्री पॉइंट के पास एक स्ट्रीम ओवरलोड और एक असिंक्रोनस ओवरलोड होता है, और असिंक्रोनस वेरिएंट एक <code>PipeReader</code> भी स्वीकार करता है। वेब प्रतिक्रिया से प्राप्त होने वाला दस्तावेज़ पढ़ते समय ही पार्स हो जाता है, बिना पहले उसे स्ट्रिंग में बदले।</p>

<div class="codeblock" id="code">
 <h3>स्ट्रीम से XML पढ़ें - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="एक ही स्ट्रीम में क्रमिक दस्तावेज़">}}

<p>किसी अन्य सिस्टम से निर्यात अक्सर एक ही स्ट्रीम में एक के बाद एक <code>NativeDicomModel</code> एलिमेंट रखता है। <code>DeserializeAsyncEnumerable</code> प्रत्येक एलिमेंट के लिए एक dataset देता है, इनपुट क्रम में, इसलिए स्ट्रीम को मेमोरी में रखे बिना प्रोसेस किया जाता है। एलिमेंट सीधे एक के बाद एक आते हैं: XML घोषणा केवल शुरू में ही अनुमति है, जैसे किसी भी XML इनपुट में।</p>

<div class="codeblock" id="code">
 <h3>क्रमिक दस्तावेज़ों को स्ट्रीम करें - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk data रेफ़रेंसेस">}}

<p>पिक्सेल डेटा जैसे बड़े मान इनलाइन नहीं लिखे जाते। वे एक <code>BulkData</code> एलिमेंट के रूप में प्रकट होते हैं जिसमें एक URI होता है जो बाइट्स की ओर इंगित करता है, जिससे दस्तावेज़ छोटा रहता है। पढ़ते समय उन रेफ़रेंसेस को हल करने के लिए, सीरियलाइज़र को एक bulk data लोडर दें। <code>DefaultBulkDataLoader</code> बिना प्रमाणीकरण के <code>file</code>, <code>http</code> और <code>https</code> URIs को फ़ेच करता है; यदि किसी आर्काइव को क्रेडेंशियल्स चाहिए, तो स्वयं <code>IBulkDataLoader</code> या <code>IAsyncBulkDataLoader</code> लागू करें।</p>

<div class="codeblock" id="code">
 <h3>पढ़ते समय bulk data को हल करें - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM से XML के साथ राउंड ट्रिप">}}

<p>इन दोनों दिशाओं को एक साथ उपयोग करने के लिए डिज़ाइन किया गया है: एक स्टडी XML के रूप में निकलती है, XML बोलने वाले सिस्टम से गुजरती है, और फिर एक DICOM फ़ाइल के रूप में वापस आती है। सब कुछ Managed .NET में है, इसलिए वही राउंड ट्रिप Windows, Linux और macOS पर चलता है।</p>

<div class="codeblock" id="code">
 <h3>DICOM से XML और वापस - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>XML के स्वरूप को नियंत्रित करने वाले विकल्पों के लिए, <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> पृष्ठ देखें। वही जोड़ी JSON के लिए भी मौजूद है: <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> और <a href="/medical/net/json-to-dicom/">JSON to DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">serialization guide</a> पूरी API को कवर करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="API संदर्भ" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="निःशुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="भुगतानित समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}