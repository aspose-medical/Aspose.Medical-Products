---
title: C# .NET में बड़े DICOM फ़ाइलों के साथ काम करें | Aspose.Medical
weight: 11500

description: C# में मल्टी‑फ़्रेम स्टडीज़ और पूर्ण स्लाइड इमेजेज़ को मेमोरी में लोड किए बिना खोलें। पिक्सेल डेटा के बिना मेटाडाटा पढ़ें, बड़े तत्वों को स्थगित करें, और फ़ाइलों को स्ट्रीम्स और पाइप्स के माध्यम से स्थानांतरित करें।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में बड़े DICOM फ़ाइलें" h2="मल्टी‑फ़्रेम स्टडी का मेटाडाटा पिक्सेल के बिना पढ़ें, बड़े तत्वों को तब तक स्थगित रखें जब तक कोई अनुरोध न करे, और संपूर्ण फ़ाइलों को स्ट्रीम्स और पाइप्स के माध्यम से स्थानांतरित करें।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="फ़ाइल बड़ी है, प्रश्न आमतौर पर छोटा होता है">}}

<p>एक पूर्ण स्लाइड इमेज, एक लंबी CT श्रृंखला या एक OCT वॉल्यूम सैकड़ों मेगाबाइट्स की होती है, और अधिकांश भाग पिक्सेल डेटा होता है। एक एप्लिकेशन जो वास्तव में करता है वह अक्सर बहुत छोटा होता है: फ़ोल्डर में क्या है इसकी सूची बनाना, रोगी पहचानकर्ता जाँचना, फ़्रेम गिनना, तय करना कि स्टडी कहाँ जाएगी। इसका उत्तर देने के लिए हर बाइट लोड करना वही है जो एक साधारण कार्य को मेमोरी समस्या में बदल देता है।</p>

<p><strong>Aspose.Medical for .NET</strong> कॉलर को यह निर्णय लेने देता है कि फ़ाइल का कितना भाग पढ़ा जाए। यह विकल्प <code>DicomFile.Open</code> पर एक तर्क के रूप में है, और यह फ़ाइलों, स्ट्रीम्स और पाइप्स पर समान रूप से लागू होता है।</p>

<p>हमारे परीक्षण सेट से 128 फ़्रेम वाले 14 MB के स्टडी पर मापा गया, उसी मशीन और उसी फ़ाइल पर:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>पढ़ने की रणनीति</th>
<th>खोलने का समय</th>
<th>आवंटित मेमोरी</th>
</tr>
</thead>
<tbody>
<tr><td>सब कुछ, डिफ़ॉल्ट</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>बड़े तत्वों को छोड़ दिया गया</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>बड़े तत्व स्थगित</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>फ़ाइल के आकार के साथ अंतर बढ़ता है। 10,000 स्टडीज़ वाले फ़ोल्डर में यह वह स्थिति है जहाँ यह माइक्रो‑ऑप्टिमाइज़ेशन नहीं रह जाता।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="मेटाडाटा पढ़ें, पिक्सेल को जैसा है वैसा ही रहने दें">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> आकार सीमा से बड़े प्रत्येक तत्व को पढ़े जाने से बाहर रखता है। वापस आने वाला डेटा सेट वह टैग्स रखता है जिसकी एक इंडेक्स या राउटर को आवश्यकता होती है।</p>

<div class="codeblock" id="code">
 <h3>पिक्सेल डेटा के बिना स्टडी पढ़ें - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>डिफ़ॉल्ट सीमा 64 kB है और यह किलॉबाइट में मान लेता है, इसलिए एक वर्कफ़्लो जो 8 kB को बड़ा मानता है, उसे इस प्रकार निर्दिष्ट कर सकता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="छोड़ने के बजाय स्थगित करें">}}

<p>जब पिक्सेल की आवश्यकता हो सकती है, लेकिन संभवतः बाद में और संभवतः सभी नहीं, तो <code>ReadLargeOnDemand</code> इस जोड़ी का दूसरा भाग है। फ़ाइल खोलने की लागत छोड़ने के समान है, और बड़ा तत्व तब पढ़ा जाता है जब कोड इसे एक्सेस करता है।</p>

<div class="codeblock" id="code">
 <h3>केवल उपयोग होने पर फ्रेम लोड करें - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>स्थगित पढ़ना एक लाइसेंस प्राप्त सुविधा है; अन्य रणनीतियाँ भी मूल्यांकन में काम करती हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="पिक्सेल को छुए बिना फ़ोल्डर को इंडेक्स करें">}}

<p>एक ही रणनीति स्ट्रीम पर लागू होती है, जो कोड से एक आर्काइव स्कैन या क्लाउड ऑब्जेक्ट स्टोर जैसा दिखता है।</p>

<div class="codeblock" id="code">
 <h3>आर्काइव स्कैन करें - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="स्ट्रीम्स और पाइप्स, अंदर और बाहर">}}

<p>पढ़ना और लिखना दोनों स्ट्रीम्स को स्वीकार करते हैं, और असिंक्रोनस एंट्री पॉइंट्स भी <code>System.IO.Pipelines</code> प्रकारों को स्वीकार करते हैं। एक स्टडी नेटवर्क प्रतिक्रिया से स्टोरेज तक बिना पूरी फ़ाइल को एक ऐरे के रूप में धारण किए यात्रा कर सकती है।</p>

<div class="codeblock" id="code">
 <h3>स्ट्रीम्स के माध्यम से पढ़ें और लिखें - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>ही वही विचार टेक्स्ट प्रतिनिधित्वों पर लागू होता है: कई डेटा सेट वाले दस्तावेज़ को <a href="/medical/net/json-to-dicom/">JSON to DICOM</a> और <a href="/medical/net/xml-to-dicom/">XML to DICOM</a> पृष्ठों पर एक बार में एक डेटा सेट पढ़ा जाता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="फ़्रेम दर फ़्रेम">}}

<p>मल्टी‑फ़्रेम डेटा को प्रत्येक फ़्रेम के अनुसार संबोधित किया जाता है, इसलिए 500 फ़्रेम वाली श्रृंखला को एक बार में एक फ़्रेम के रूप में प्रोसेस किया जाता है न कि पूरे पिक्सेल डेटा तत्व के रूप में।</p>

<div class="codeblock" id="code">
 <h3>फ़्रेम्स को ट्रैवर्स करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="जहाँ यह डिज़ाइन तय करता है">}}

<ul>
<li>आर्काइव इंडेक्सिंग और माइग्रेशन: लाखों फ़ाइलें, और केवल हेडर महत्वपूर्ण होता है जब तक कुछ नहीं ले जाया जाता।</li>
<li>राउटर और स्टोर नोड्स: एक स्टडी को स्वीकार करें, उसे रूट करने के लिए आवश्यक पढ़ें, बाइट्स को आगे पास करें।</li>
<li>AI पाइपलाइन्स: मेटाडाटा से मैनिफेस्ट बनाएं, फिर उस उपसमुच्चय के लिए फ़्रेम्स खींचें जिस पर वास्तव में प्रशिक्षण दिया गया है।</li>
<li>मेमोरी सीमा वाले कंटेनर: कार्य सेट रणनीति का अनुसरण करता है, फ़ाइल आकार नहीं।</li>
<li>पूर्ण स्लाइड और OCT डेटा: ऐसी फ़ाइलें जहाँ सब कुछ पढ़ना बिल्कुल संभव नहीं है।</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">मेमोरी प्रबंधन गाइड</a> रणनीतियों को विस्तार से समझाता है, और <a href="/medical/net/dicom-networking/">DICOM नेटवर्किंग</a> वही डेटा DIMSE के माध्यम से आने को दर्शाता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
{{< blocks/products/pf/slr-element name="API संदर्भ" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="निःशुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="भुगतानयुक्त समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों .NET के लिए Aspose.Medical?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता की कहानियां" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
