---
title: C# .NET में DICOM को XML में परिवर्तित करें | Aspose.Medical
weight: 3000
description: C# .NET में मानक DICOM XML फ़ॉर्मेट में DICOM डेटासेट को सीरियलाइज़ करें। Aspose.Medical API के साथ Bulk डेटा हैंडलिंग, स्ट्रीम-आधारित प्रोसेसिंग, और असिंक्रोनस ऑपरेशन्स को कॉन्फ़िगर करें।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में DICOM को XML में परिवर्तित करें" h2="मानक DICOM XML प्रतिनिधित्व (PS3.19) में DICOM डेटासेट को सीरियलाइज़ करें। शुद्ध .NET लाइब्रेरी के साथ Bulk डेटा रेफ़रेंसेज़, स्ट्रीम-आधारित आउटपुट, और असिंक्रोनस प्रोसेसिंग को कॉन्फ़िगर करें।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="मानक-आधारित DICOM XML सीरियलाइज़ेशन">}}

<p><strong>Aspose.Medical for .NET</strong> DICOM डेटा को XML में <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a> के अनुसार सीरियलाइज़ करता है। यह DICOM डेटासेट को XML में प्रस्तुत करने का आधिकारिक मानक है, जिसका उपयोग DICOMweb सेवाओं, इंटीग्रेशन प्लेटफ़ॉर्म, और उन सिस्टमों द्वारा किया जाता है जिन्हें मेडिकल इमेजिंग मेटाडेटा का मानव‑पठनीय, स्कीमा‑प्रमाणित प्रतिनिधित्व चाहिए।</p>

<p>The <code>DicomXmlSerializer</code> class provides static methods for both serialization and deserialization. Simple tag‑dump विधियों के विपरीत, आउटपुट DICOM XML स्कीमा के अनुरूप है जहाँ प्रत्येक तत्व को उसके टैग, VR, और सही फ़ॉर्मेटेड मानों के साथ दर्शाया जाता है — जिससे बाइनरी DICOM और XML के बीच लॉसलैस राउंड‑ट्रिप रूपांतरण सक्षम होता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# में DICOM को XML में सीरियलाइज़ करें">}}

<p><code>DicomXmlSerializer</code> क्लास का उपयोग करके DICOM डेटासेट को XML स्ट्रिंग में परिवर्तित करें। सबसे सरल तरीका मानक‑अनुपालक XML दस्तावेज़ उत्पन्न करता है:</p>

<div class="codeblock" id="code">
 <h3>DICOM को XML में परिवर्तित करें - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="स्ट्रीम-आधारित और असिंक्रोनस सीरियलाइज़ेशन">}}

<p>बड़े DICOM फ़ाइलों या उच्च‑थ्रूपुट परिदृश्यों के लिए, मेमोरी में बड़े स्ट्रिंग्स आवंटित करने से बचने हेतु सीधे स्ट्रीम पर सीरियलाइज़ करें। सिंक्रोनस और असिंक्रोनस दोनों मेथड उपलब्ध हैं:</p>

<div class="codeblock" id="code">
 <h3>सिंक्रोनस स्ट्रीम सीरियलाइज़ेशन - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>असिंक्रोनस स्ट्रीम सीरियलाइज़ेशन - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="बड़े अध्ययन के लिए पाइपलाइन स्ट्रीमिंग">}}

<p>पूरा अध्ययन मेमोरी में रखने की ज़रूरत नहीं है। <code>DicomXmlSerializer</code> एक <code>PipeWriter</code> में लिखता है और <code>PipeReader</code> से पढ़ता है, इसलिए XML प्रवाह के साथ निर्मित और उपयोग किया जा सकता है, और डेटासेट की एक श्रृंखला को <code>DeserializeAsyncEnumerable</code> द्वारा एक‑एक करके पढ़ा जा सकता है। प्रत्येक मेथड <code>CancellationToken</code> लेता है।</p>

<div class="codeblock" id="code">
 <h3>पाइप के माध्यम से सीरियलाइज़ और डीसीरियलाइज़ करें - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>डेटासेट की श्रृंखला को एक‑एक करके पढ़ें - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="सीरियलाइज़ेशन विकल्प">}}

<p><code>DicomXmlSerializerOptions</code> क्लास यह नियंत्रित करती है कि DICOM डेटा XML में कैसे प्रस्तुत किया जाए। मुख्य कॉन्फ़िगरेशन बड़े बाइनरी मानों के लिए Bulk डेटा हैंडलिंग शामिल करता है:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>प्रॉपर्टी</th>
<th>प्रकार</th>
<th>विवरण</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>बड़े डेटा (उदा., पिक्सेल डेटा) को BulkData URI रेफ़रेंसेज़ के रूप में लिखने के लिए कस्टम कन्वर्टर</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>डीसीरियलाइज़ेशन के दौरान BulkData URI को हल करने के लिए कस्टम लोडर</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>जब कोई कस्टम विकल्प प्रदान नहीं किया जाता तो उपयोग किया जाने वाला डिफ़ॉल्ट विकल्प instance</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>कस्टम विकल्पों के साथ सीरियलाइज़ करें - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk डेटा हैंडलिंग">}}

<p>बड़े बाइनरी मान (पिक्सेल डेटा, वेवफॉर्म, एंकैप्सुलेटेड दस्तावेज़) को XML आउटपुट में इनलाइन करने के बजाय BulkData URI रेफ़रेंसेज़ के रूप में बाहरीकृत किया जा सकता है। यह <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">DICOM PS3.19 BulkData एलिमेंट</a> विशिष्टता का अनुसरण करता है।</p>

<p>सीरियलाइज़ेशन के दौरान बड़े डेटा को बाहरीकृत करने के लिए <code>IBulkDataConverter</code> को लागू करें, और डीसीरियलाइज़ेशन के दौरान URI को हल करने के लिए <code>IBulkDataLoader</code> को लागू करें। सामान्य मामलों में लोडर लिखने की आवश्यकता नहीं है: <code>DefaultBulkDataLoader.Instance</code> <code>file</code>, <code>http</code> और <code>https</code> URI को हल करता है, और यह <code>IAsyncBulkDataLoader</code> को भी लागू करता है, इसलिए स्ट्रीमिंग पाथ पर Bulk डेटा असिंक्रोनस रूप से प्राप्त किया जाता है।</p>

<div class="codeblock" id="code">
 <h3>कस्टम Bulk डेटा हैंडलिंग - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
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

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="XML को DICOM में डीसीरियलाइज़ करें">}}

<p>DICOM XML को फिर से Dataset ऑब्जेक्ट में पार्स करें। स्ट्रिंग इनपुट, स्ट्रीम इनपुट, और असिंक्रोनस ऑपरेशन्स का समर्थन करता है:</p>

<div class="codeblock" id="code">
 <h3>XML को DICOM में डीसीरियलाइज़ करें - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="XML बनाम JSON सीरियलाइज़ेशन">}}

<p>Aspose.Medical दोनों DICOM XML (PS3.19) और DICOM JSON (PS3.18) सीरियलाइज़ेशन को सपोर्ट करता है। दोनों फ़ॉर्मैट लॉसलैस राउंड‑ट्रिप रूपांतरण प्रदान करते हैं, परन्तु विभिन्न इंटीग्रेशन परिदृश्यों की सेवा करते हैं:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>फ़ीचर</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>मानक</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>स्कीमा वैधता</td><td>XML स्कीमा (XSD) उपलब्ध</td><td>कोई औपचारिक स्कीमा नहीं</td></tr>
<tr><td>सबसे उपयुक्त</td><td>एंटरप्राइज़ इंटीग्रेशन, HL7 CDA, ऑडिट लॉग्स, XDS रजिस्ट्रीज़</td><td>DICOMweb, REST APIs, FHIR ImagingStudy</td></tr>
<tr><td>मानव पठनीयता</td><td>विवरणात्मक लेकिन स्व-व्याख्यात्मक</td><td>संक्षिप्त और व्यापक रूप से समर्थित</td></tr>
<tr><td>Bulk डेटा</td><td>BulkData एलिमेंट साथ में URI</td><td>BulkDataURI प्रॉपर्टी</td></tr>
<tr><td>सीरियलाइज़र क्लास</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="सोर्स कोड" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API रेफ़रेंसेज़" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="मुफ़्त समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="भुगतानित समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Aspose.Medical for .NET क्यों?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहक सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
