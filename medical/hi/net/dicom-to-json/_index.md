---
title: C# .NET में DICOM को JSON में परिवर्तित करें | Aspose.Medical
weight: 4000
description: C# .NET में DICOM फ़ाइलों को मानक DICOM JSON फ़ॉर्मेट में सीरियलाइज़ करें। number handling, bulk data URIs, keyword output, और async stream processing को Aspose.Medical API के साथ कॉन्फ़िगर करें।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में DICOM को JSON में परिवर्तित करें" h2="DICOM डेटा सेट और फ़ाइलों को मानक DICOM JSON Model (PS3.18) में सीरियलाइज़ करें। number handling, bulk data references, keyword output, और stream‑based async processing को कॉन्फ़िगर करें।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="मानक DICOM JSON Model">}}

<p><strong>Aspose.Medical for .NET</strong> DICOM डेटा को JSON में सीरियलाइज़ करता है जो <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a> का अनुसरण करता है। यह DICOM डेटा सेट को JSON में प्रस्तुत करने के लिए आधिकारिक मानक है, जिसका उपयोग DICOMweb सेवाओं, FHIR ImagingStudy संसाधनों, और आधुनिक स्वास्थ्य‑सेवा APIs द्वारा किया जाता है।</p>

<p>DICOM JSON Model में, प्रत्येक एट्रिब्यूट को उसका टैग JSON कुंजी के रूप में दर्शाया जाता है (उदाहरण के लिए, Patient Name के लिए <code>"00100010"</code>), साथ में Value Representation और मान एरे को नेस्टेड प्रॉपर्टीज़ के रूप में प्रस्तुत किया जाता है:</p>

<div class="codeblock" id="code">
 <h3>DICOM JSON Model स्वरूप</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# में DICOM को JSON में सीरियलाइज़ करें">}}

<p><code>DicomJsonSerializer</code> क्लास का उपयोग करके DICOM फ़ाइल या डेटा सेट को JSON में परिवर्तित करें। सबसे सरल तरीका कॉम्पैक्ट, मानकों‑अनुकूल आउटपुट उत्पन्न करता है:</p>

<div class="codeblock" id="code">
 <h3>DICOM को JSON में परिवर्तित करें - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p>आप <code>Dataset</code>, एक पूर्ण <code>DicomFile</code> (File Meta Information सहित), या डेटा सेट की एरे को सीरियलाइज़ कर सकते हैं:</p>

<div class="codeblock" id="code">
 <h3>DicomFile और डेटा सेट एरे को सीरियलाइज़ करें - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="स्ट्रीम‑आधारित और Async सीरियलाइज़ेशन">}}

<p>बड़े DICOM फ़ाइलों या हाई‑थ्रूपुट स्थितियों में, मेमोरी में बड़े स्ट्रिंग्स को आवंटित करने से बचने के लिए सीधे UTF-8 स्ट्रीम में सीरियलाइज़ करें। सिंक्रोनस और async दोनों मेथड उपलब्ध हैं:</p>

<div class="codeblock" id="code">
 <h3>स्ट्रीम और async सीरियलाइज़ेशन - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="सीरियलाइज़ेशन विकल्प">}}

<p><code>DicomJsonSerializerOptions</code> क्लास नियंत्रित करता है कि DICOM डेटा JSON में कैसे प्रस्तुत किया जाता है:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>प्रॉपर्टी</th>
<th>प्रकार</th>
<th>विवरण</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>नियंत्रित करता है कि संख्यात्मक VRs (IS, DS, SV, UV) को JSON संख्याओं या स्ट्रिंग्स के रूप में लिखा जाए</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>JSON कुंजियों के रूप में टैग (<code>"00100010"</code>) के बजाय DICOM कीवर्ड (<code>"PatientName"</code>) का उपयोग करें। नोट: गैर‑मानक</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>DICOM कीवर्ड को एक अतिरिक्त JSON एट्रिब्यूट के रूप में शामिल करें</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>DICOM टैग नाम को एक अतिरिक्त JSON एट्रिब्यूट के रूप में शामिल करें</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>बड़ी डेटा (जैसे पिक्सेल डेटा) को URI रेफ़रेंस के रूप में लिखने के लिए कस्टम कनवर्टर</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>डिसीरियलाइज़ेशन के दौरान bulk data URI को हल करने के लिए कस्टम लोडर</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>सीरियलाइज़ेशन विकल्प कॉन्फ़िगर करें - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="संख्या हैंडलिंग">}}

<p>DICOM संख्यात्मक Value Representations (IS, DS, SV, UV) को JSON संख्याओं या JSON स्ट्रिंग्स के रूप में सीरियलाइज़ किया जा सकता है। यह इंटरऑपरेबिलिटी के लिए महत्वपूर्ण है, क्योंकि कुछ DICOM कार्यान्वयन संख्याओं को अग्रणी स्पेसेस या गैर‑मानक फॉर्मेटिंग के साथ संग्रहीत करते हैं:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>मोड</th>
<th>JSON आउटपुट</th>
<th>उपयोग मामला</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>मानक‑अनुरूप, गणना के लिए आदर्श। यदि मान पार्स नहीं किया जा सकता तो अपवाद उत्पन्न होता है।</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>मूल स्ट्रिंग प्रतिनिधित्व को संरक्षित करता है। गैर‑मानक डेटा के राउंड‑ट्रिपिंग के लिए सुरक्षित।</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="बड़ी डेटा हैंडलिंग">}}

<p>बड़ी बाइनरी मान (पिक्सेल डेटा, वेवफ़ॉर्म, संलग्न दस्तावेज़) को JSON आउटपुट में इनलाइन करने के बजाय bulk data रेफ़रेंस के रूप में हैंडल किया जा सकता है। यह <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a> स्पेसिफिकेशन का पालन करता है।</p>

<p>सीरियलाइज़ेशन के दौरान बड़ी डेटा को बाहरीकृत करने के लिए <code>IBulkDataConverter</code> लागू करें, और डिसीरियलाइज़ेशन के दौरान URI को हल करने के लिए <code>IBulkDataLoader</code> लागू करें:</p>

<div class="codeblock" id="code">
 <h3>कस्टम bulk डेटा हैंडलिंग - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JSON को DICOM में डिसीरियलाइज़ करें">}}

<p>DICOM JSON को वापस Dataset या DicomFile ऑब्जेक्ट्स में पार्स करें। स्ट्रिंग इनपुट, स्ट्रीम इनपुट, और async स्ट्रीम ऑपरेशन्स को सपोर्ट करता है:</p>

<div class="codeblock" id="code">
 <h3>JSON को DICOM में डिसीरियलाइज़ करें - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="System.Text.Json इंटीग्रेशन">}}

<p>Aspose.Medical मानक <code>System.Text.Json</code> सीरियलाइज़ेशन पाइपलाइन के साथ एकीकरण के लिए <code>DatasetJsonConverter</code> और <code>DicomFileJsonConverter</code> प्रदान करता है। इससे DICOM ऑब्जेक्ट्स को आपके मौजूदा JSON प्रोसेसिंग कोड में अन्य .NET टाइप्स के साथ सीरियलाइज़ किया जा सकता है:</p>

<div class="codeblock" id="code">
 <h3>System.Text.Json इंटीग्रेशन - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="डॉक्यूमेंटेशन" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="सोर्स कोड" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API संदर्भ" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="निःशुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="सशुल्क समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता कहानियां" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
