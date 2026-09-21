---
title: HTJ2K in C# .NET - DICOM के लिए हाई-थ्रूपुट JPEG 2000 | Aspose.Medical
weight: 10000

description: C# से हाई-थ्रूपुट JPEG 2000 में DICOM छवियों को संपीड़ित और पढ़ें। लॉसलेस HTJ2K, RPCL वैरिएंट और लॉसी HTJ2K, मैनेज्ड .NET में लागू, बिना किसी नेटिव कोडेक के तैनात किए।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में HTJ2K" h2="DICOM के लिए हाई-थ्रूपुट JPEG 2000: तेज़ अभिलेखों और क्लाउड दृश्य के लिये मानक द्वारा जोड़ी गई संपीड़न, मैनेज्ड C# में लागू, बिना किसी नेटिव इंस्टॉलेशन के।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="HTJ2K क्या बदलता है">}}

<p>हाई-थ्रूपुट JPEG 2000, JPEG 2000 की वेवलेट और छवि गुणवत्ता को बनाए रखता है और वह भाग बदलता है जिसने इसे धीमा बनाया था। ब्लॉक कोडर नया है, और डिकोडिंग कई गुना तेज है, इसलिए DICOM मानक ने इसे तीन ट्रांसफर सिन्टैक्स में अपनाया और क्लाउड इमेजिंग प्लेटफ़ॉर्म्स ने इसे अपना लिया।</p>

<p>.NET टीम के लिए व्यावहारिक प्रश्न अलग है: कौन वास्तव में उन फ़ाइलों को उत्पन्न कर सकता है। अधिकांश लाइब्रेरीज़ HTJ2K को नेेटिव OpenJPH बिल्ड के माध्यम से पहुँचती हैं, जिसका अर्थ है प्रत्येक प्लेटफ़ॉर्म के लिए बाइनरी, कंटेनर में एक बिल्ड स्टेप और एक निर्भरता जिस पर सुरक्षा समीक्षा प्रश्न उठाएगी। <strong>Aspose.Medical for .NET</strong> कोडेक को मैनेज्ड कोड में उसी पैकेज के भीतर लागू करता है जो फ़ाइलों को पढ़ता और लिखता है, इसलिए HTJ2K Windows, Linux और कंटेनर में समान रूप से काम करता है, बिना किसी स्थापना के।</p>

<p>तीन ट्रांसफ़र सिन्टैक्स समर्थित हैं, और सभी तीन पढ़ने और लिखने दोनों को समर्थन देते हैं:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), हाई-थ्रूपुट JPEG 2000 लॉसलेस।</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), RPCL प्रोग्रेशन क्रम के साथ लॉसलेस वैरिएंट।</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), हाई-थ्रूपुट JPEG 2000।</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="एक स्टडी को HTJ2K में संपीड़ित करें">}}

<p>एक कॉल फ़ाइल को नई सिन्टैक्स में ले जाता है। डेटासेट, प्राइवेट टैग्स और फ़ाइल मेटा जानकारी उसके साथ चलती है।</p>

<div class="codeblock" id="code">
 <h3>एक DICOM फ़ाइल को HTJ2K में ट्रांसकोड करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>हमारे अपने टेस्ट सेट की 1714 × 1933 16-बिट छवि पर, फ़ाइल का आकार 6.3 MB से 2.9 MB हो जाता है, और पिक्सेल बिट दर बिट वापस आ जाते हैं। संख्याएँ मोडालिटी और छवि के अनुसार अलग होती हैं, इसलिए अपने डेटा पर मापें, जो आपके पास मौजूद फ़ाइलों पर एक लूप है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="लॉसलेस का अर्थ लॉसलेस है">}}

<p>डायग्नोस्टिक डेटा उस कोडेक को बर्दाश्त नहीं करता जो लगभग सही हो। HTJ2K लॉसलेस में और वापस ट्रांसकोड करें, और पिक्सेल डेटा मूल बाइट्स के समान होता है, यह एक गुण है जिसे आप अपने परीक्षण सूट में सत्यापित कर सकते हैं पहले कि आप किसी अभिलेख को पुनः संपीड़ित करने के लिए सहमत हों।</p>

<div class="codeblock" id="code">
 <h3>एक अनकम्प्रेस्ड सिन्टैक्स पर वापस - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, नेटवर्क पर देखने के लिए बनाया गया वैरिएंट">}}

<p>1.2.840.10008.1.2.4.202 सिन्टैक्स वही लॉसलेस कोडस्ट्रीम RPCL प्रोग्रेशन क्रम में संग्रहीत करता है: पहले रिज़ॉल्यूशन, फिर पोज़िशन, फिर कंपोनेंट, फिर लेयर। एक रीडर जो केवल स्ट्रीम की शुरुआत लेता है, उसे पूर्ण लो रिज़ॉल्यूशन इमेज मिलती है, जो व्यूअर को बड़ी स्टडी को ऐसे लिंक पर खोलते समय चाहिए जिसे वह नियंत्रित नहीं करता।</p>

<div class="codeblock" id="code">
 <h3>RPCL प्रोग्रेशन क्रम के साथ संपीड़ित करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="अभिलेख जो भेजता है, उसे पढ़ें">}}

<p>काम का दूसरा हिस्सा उन सिस्टमों से HTJ2K स्वीकार करना है जो पहले से ही इसे बनाते हैं। फ़ाइल खोलें, जांचें कि यह किस रूप में संग्रहीत है, और पिक्सेल डेटा के साथ काम करें।</p>

<div class="codeblock" id="code">
 <h3>HTJ2K फ़ाइल पढ़ें - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>मल्टी-फ़्रेम इमेजेज़ को फ़्रेम दर फ़्रेम संभाला जाता है, इसलिए लंबी श्रृंखला में मेमोरी फ़्रेम प्रति खर्च होती है, न कि स्टडी प्रति।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTJ2K अपने स्थान को कैसे प्राप्त करता है">}}

<ul>
<li>अभिलेख माइग्रेशन: संग्रहीत स्टडी को HTJ2K लॉसलेस में पुनः संपीड़ित करें, फ़ूटप्रिंट घटाएँ, डायग्नोस्टिक डेटा को अपरिवर्तित रखें।</li>
<li>क्लाउड और DICOMweb: डिकोड गति ही है जो ब्राउज़र-साइड या सर्वर-साइड व्यूअर को बड़े इमेजेज़ पर तुरंत महसूस कराती है।</li>
<li>AI पाइपलाइन: प्रशिक्षण सेट्स को लिखने की तुलना में पढ़ा अधिक बार होता है, और डिकोड समय वह लागत है जो बार-बार आती है।</li>
<li>कंटेनर और सर्वरलेस: कोडेक असेंबली का हिस्सा है, इसलिए इमेज को बिल्ड में नेटिव लाइब्रेरी या कंपाइलर की आवश्यकता नहीं रहती।</li>
</ul>

<p>यह लाइब्रेरी JPEG XL भी प्रदान करती है, मानक का दूसरा हालिया जोड़, और पुराने कोडेक्स जो अभिलेख रख सकता है: JPEG, JPEG-LS, JPEG 2000 और RLE। <a href="/medical/net/dicom-transfer-syntax-conversion/">ट्रांसफर सिन्टैक्स कन्वर्ज़न</a> पेज पूरी सेट को कवर करता है, और <a href="/medical/net/jpeg2000/">JPEG 2000</a> पेज उस कोडेक को कवर करता है जिससे HTJ2K विकसित हुआ।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="सीखने के संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="डॉक्युमेंटेशन" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="API रेफ़रेंसेज़" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="प्रोडक्ट सपोर्ट" tabId="support" >}}
{{< blocks/products/pf/slr-element name="फ्री सपोर्ट" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="पेड सपोर्ट" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सक्सेस स्टोरीज़" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
