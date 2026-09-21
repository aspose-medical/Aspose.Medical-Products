---
title: C# .NET में DICOM के लिए JPEG XL | Aspose.Medical
weight: 10500

description: C# से JPEG XL में DICOM छवियों को संग्रहित करें। Lossless JPEG XL जो पिक्सेल को बिट दर बिट लौटाता है, एक ही प्रबंधित असेंबली में बिना किसी नेटिव कोडेक के तैनात किए।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में DICOM के लिए JPEG XL" h2="DICOM मानक में नवीनतम संपीड़न, जिसमें हमने मापी सबसे छोटी Lossless फ़ाइलें हैं, प्रबंधित C# में लागू किया गया और एक असेंबली में शिप किया गया।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL ने DICOM तक क्यों पहुँचा">}}

<p>मेडिकल अभिलेख बढ़ते हैं और कभी घटते नहीं हैं। JPEG XL वह कोडेक है जिसे इमेजिंग दुनिया ने दो दशकों के JPEG और JPEG 2000 अनुभव के बाद डिजाइन किया, और DICOM ने इसे ट्रांसफ़र सिंटैक्स के रूप में जोड़ा क्योंकि स्टोरेज टीमों के लिए यह महत्वपूर्ण है: वही पिक्सेल, फ़ाइल छोटा है।</p>

<p><strong>Aspose.Medical for .NET</strong> JPEG XL को लिखता और पढ़ता है C# पोर्ट libjxl के माध्यम से जो लाइब्रेरी के भीतर रहता है। पैकेज एक असेंबली शिप करता है, <code>Aspose.Medical.dll</code>, और इसके साथ कोई नेटिव बाइनरी नहीं, इसलिए ऐसा नया कोडेक डिप्लॉयमेंट प्रोजेक्ट नहीं बनता: वही असेंबली Windows, Linux, बिल्ड एजेंट और कंटेनर में चलती है।</p>

<p>पिक्सेल ले जाने वाले दो ट्रांसफ़र सिंटैक्स हैं:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), उन डायग्नोस्टिक डेटा के लिए जो अपरिवर्तित वापस आना चाहिए।</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), उन मामलों के लिए जहाँ छोटी फ़ाइल का महत्व सटीक कॉपी से अधिक है।</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="अध्ययन को संपीड़ित करें, हर पिक्सेल रखें">}}

<p>ट्रांसकोडिंग एक कॉल में होती है, और पिक्सेल के आसपास का डेटा सेट उसके साथ यात्रा करता है।</p>

<div class="codeblock" id="code">
 <h3>DICOM फ़ाइल को JPEG XL में ट्रांसकोड करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>हमने इसे हमारे अपने टेस्ट सेट की 1714 × 1933 16‑bit इमेज पर मापा: 6.3 MB अनकम्प्रेस्ड 2.7 MB JPEG XL Lossless बन जाता है, जो HTJ2K Lossless में समान इमेज से छोटी है। आपके अपने आंकड़े मोडालिटी पर निर्भर करेंगे, इसलिए चयन करने से पहले अपने फ़ाइलों के फ़ोल्डर पर तुलना चलाएँ।</p>

<p>Lossless शब्द को यहाँ शाब्दिक रूप से लेना चाहिए। JPEG XL में ट्रांसकोड करें और वापस, और पिक्सेल डेटा वही बाइट्स है जिससे आप शुरू किए थे, इसलिए एक अभिलेख को पुनः संपीड़ित किया जा सकता है बिना डायग्नोस्टिक गुणवत्ता पर चर्चा किए।</p>

<div class="codeblock" id="code">
 <h3>एक अनकम्प्रेस्ड सिंटैक्स पर वापस - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="पहले से JPEG XL के रूप में संग्रहित को पढ़ें">}}

<p>JPEG XL में आया फ़ाइल अन्य की तरह खुलता है। ट्रांसफ़र सिंटैक्स बताता है कि यह क्या है, और फ्रेम डिकोड होने पर पिक्सेल डेटा उपलब्ध हो जाता है।</p>

<div class="codeblock" id="code">
 <h3>JPEG XL फ़ाइल खोलें - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL या HTJ2K">}}

<p>दोनों नवीन हैं, दोनों ही जब आप Lossless मांगते हैं तो Lossless हैं, और लाइब्रेरी दोनों को लिखती और पढ़ती है। वे अलग-अलग प्रश्नों के उत्तर देती हैं।</p>

<table class="table table-bordered">
<thead>
<tr>
<th>प्रश्न</th>
<th>उत्तर</th>
</tr>
</thead>
<tbody>
<tr><td>हमारे परीक्षण में कौन छोटी फ़ाइल बनाता है</td><td>JPEG XL lossless, कुछ प्रतिशत से</td></tr>
<tr><td>कौन नेटवर्क पर प्रोग्रेसिव व्यूइंग के लिए बनाया गया है</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, विशेष रूप से RPCL वेरिएंट</td></tr>
<tr><td>कौन DICOM मानक में पहले प्रवेश किया</td><td>HTJ2K, इसलिए आज अधिक अभिलेख इसे स्वीकार करते हैं</td></tr>
<tr><td>यहाँ कौन नेटिव डिपेंडेंसी की लागत रखता है</td><td>कोई नहीं, दोनों एक ही असेंबली में प्रबंधित कोड हैं</td></tr>
</tbody>
</table>

<p>चयन आमतौर पर लिंक के अन्य पक्ष से आता है: ट्रांसकोड करें उस सिंटैक्स में जिसे अभिलेख स्वीकार करता है, और बाकी पाइपलाइन को समान रखें।</p>

<div class="codeblock" id="code">
 <h3>लक्षित अभिलेख को निर्णय लेने दें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="जहाँ यह फायदेमंद होता है">}}

<ul>
<li>दीर्घकालिक अभिलेख: वही अध्ययन, कम टेराबाइट, और कोई नुक़सान नहीं जो रेडियोलॉजिस्ट को न्यायसंगत करना पड़े।</li>
<li>क्लाउड स्टोरेज बिल: बचत हर महीने दोहराती है, जबकि ट्रांसकोडिंग एक बार चलती है।</li>
<li>शोध और AI के लिए डेटा सेट: छोटी प्रतियां संग्रह और प्रशिक्षण के बीच तेज़ी से जाती हैं।</li>
<li>डिप्लॉयमेंट: ऐसा नया कोडेक सामान्यतः प्रत्येक प्लेटफ़ॉर्म के लिए नेटिव बिल्ड की आवश्यकता रखता है; यहाँ यह वह असेंबली का हिस्सा है जिसे आप पहले से ही संदर्भित करते हैं।</li>
</ul>

<p>लाइब्रेरी उन कोडेक्स को भी लिखती है जो एक मौजूदा अभिलेख में होते हैं: JPEG, JPEG‑LS, JPEG 2000, HTJ2K और RLE। <a href="/medical/net/dicom-transfer-syntax-conversion/">ट्रांसफ़र सिंटैक्स परिवर्तन</a> पेज पूरे सेट को कवर करता है, <a href="/medical/net/htj2k/">HTJ2K</a> का अपना पेज है, और <a href="/medical/net/jpeg2000/">JPEG 2000</a> वह जगह है जहाँ दोनों नए कोडेक आते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="डॉक्यूमेंटेशन" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="API रेफ़रेंसेज़" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="नि:शुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="भुगतानयुक्त समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical .NET के लिए?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहक सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता की कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
