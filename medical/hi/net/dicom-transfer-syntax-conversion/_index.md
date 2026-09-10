---
title: C# .NET में DICOM ट्रांसफ़र सिंटैक्स रूपांतरण | Aspose.Medical
weight: 16000
description: C# .NET में ट्रांसफ़र सिंटैक्स के बीच DICOM फ़ाइलों को ट्रांसकोड करें। JPEG, JPEG 2000, JPEG-LS, RLE, और अनकम्प्रेस्ड फ़ॉर्मैट्स के लिए Aspose.Medical API के साथ समर्थन।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में DICOM ट्रांसफ़र सिंटैक्स रूपांतरण" h2="अनकम्प्रेस्ड, JPEG, JPEG 2000, JPEG-LS, और RLE ट्रांसफ़र सिंटैक्स के बीच DICOM फ़ाइलों को ट्रांसकोड करें। शुद्ध .NET लाइब्रेरी जिसमें कोई नेटिव निर्भरता नहीं है।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="ट्रांसफ़र सिंटैक्स क्या है?">}}

<p>A <strong>ट्रांसफ़र सिंटैक्स</strong> यह निर्धारित करता है कि DICOM डेटा को संग्रहीत करने और स्थानांतरित करने के लिए कैसे एन्कोड किया जाता है। यह तीन प्रमुख पहलुओं को निर्दिष्ट करता है: बाइट क्रम (एंडियननेस), वैल्यू रिप्रेजेंटेशन्स explicit या implicit हैं, तथा पिक्सेल डेटा पर लागू संपीड़न एल्गोरिदम। प्रत्येक DICOM फ़ाइल अपने ट्रांसफ़र सिंटैक्स को फ़ाइल मेटा इनफ़ॉर्मेशन हेडर में घोषित करती है।</p>

<p>विभिन्न मेडिकल डिवाइस, PACS सर्वर, और व्यूइंग एप्लिकेशन विभिन्न ट्रांसफ़र सिंटैक्स सेटों का समर्थन करते हैं। <strong>Aspose.Medical for .NET</strong> <code>Transcode</code> मेथड प्रदान करता है जो ट्रांसफ़र सिंटैक्स के बीच रूपांतरण करता है, जिससे इंटरऑपरेबिलिटी, स्टोरेज ऑप्टिमाइजेशन, और प्रोसेसिंग टूल्स के साथ संगतता संभव होती है &mdash; यह सब शुद्ध .NET लाइब्रेरी में बिना किसी नेटिव डिपेंडेंसी के।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# में DICOM फ़ाइल को ट्रांसकोड करें">}}

<p><code>DicomFile.Transcode</code> मेथड एक DICOM फ़ाइल को उसकी वर्तमान ट्रांसफ़र सिंटैक्स से किसी भी समर्थित लक्ष्य सिंटैक्स में परिवर्तित करता है। मेथड एक नया <code>DicomFile</code> इंस्टेंस लौटाता है — मूल फ़ाइल अपरिवर्तित रहती है:</p>

<div class="codeblock" id="code">
 <h3>बेसिक DICOM ट्रांसकोडिंग - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>आप सीधे <code>Dataset</code> स्तर पर भी ट्रांसकोड कर सकते हैं:</p>

<div class="codeblock" id="code">
 <h3>Dataset को ट्रांसकोड करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="समर्थित ट्रांसफ़र सिंटैक्स">}}

<p>नीचे दी गई तालिका में सभी मानक DICOM इमेज डेटा ट्रांसफ़र सिंटैक्स और .NET के लिए Aspose.Medical में उनका वर्तमान समर्थन स्थिति दर्शाई गई है। सभी समर्थित कोडेक शुद्ध C# में लागू किए गये हैं और पूरी तरह प्लेटफ़ॉर्म-निर्भर नहीं हैं।</p>

<table class="table table-bordered">
<thead>
<tr>
<th>ट्रांसफ़र सिंटैक्स</th>
<th>UID</th>
<th>प्रकार</th>
<th>स्थिति</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>अनकम्प्रेस्ड</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>अनकम्प्रेस्ड</td><td>समर्थित</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>अनकम्प्रेस्ड</td><td>समर्थित</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>अनकम्प्रेस्ड</td><td>समर्थित</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>डिफ्लेटेड</td><td>समर्थित</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Lossy, 8-बिट</td><td>समर्थित</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Lossy, 12-बिट</td><td>समर्थित नहीं</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Lossless</td><td>समर्थित (सिर्फ 8-बिट)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Lossless</td><td>समर्थित (सिर्फ 8-बिट)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Lossless</td><td>समर्थित</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Near-lossless</td><td>समर्थित</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>समर्थित (पढ़ना 8/16-बिट, लिखना 8-बिट)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy or lossless</td><td>समर्थित (पढ़ना 8/16-बिट, लिखना 8-बिट)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>समर्थित (पढ़ना 8/16-बिट, लिखना 8-बिट)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy or lossless</td><td>समर्थित (पढ़ना 8/16-बिट, लिखना 8-बिट)</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Lossless</td><td>समर्थित</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>शीघ्र उपलब्ध</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>शीघ्र उपलब्ध</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy or lossless</td><td>शीघ्र उपलब्ध</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Lossless</td><td>शीघ्र उपलब्ध</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Lossless</td><td>शीघ्र उपलब्ध</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Lossy or lossless</td><td>शीघ्र उपलब्ध</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="सामान्य ट्रांसकोडिंग परिदृश्य">}}

<p>विभिन्न वर्कफ़्लोज़ को विभिन्न ट्रांसकोडिंग रणनीतियों की आवश्यकता होती है। यहाँ सबसे सामान्य परिदृश्य हैं:</p>

<div class="codeblock" id="code">
 <h3>प्रोसेसिंग के लिए डिकम्प्रेस करें - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>आर्काइव स्टोरेज के लिए संपीड़ित करें - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>नेटवर्क ट्रांसमिशन के लिए संपीड़ित करें - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ट्रांसफ़र सिंटैक्स गुणों का निरीक्षण करें">}}

<p><code>TransferSyntax</code> क्लास एन्कोडिंग विशेषताओं को दर्शाने वाले प्रॉपर्टीज़ उजागर करती है। इनका उपयोग फ़ाइल के वर्तमान ट्रांसफ़र सिंटैक्स को निरीक्षण करने या उपयुक्त लक्ष्य सिंटैक्स चुनने के लिए करें:</p>

<div class="codeblock" id="code">
 <h3>ट्रांसफ़र सिंटैक्स प्रॉपर्टीज़ पढ़ें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>प्रॉपर्टी</th>
<th>प्रकार</th>
<th>विवरण</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>ट्रांसफ़र सिंटैक्स का अनूठा पहचानकर्ता</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>क्या वैल्यू रिप्रेजेंटेशन्स स्पष्ट रूप से एन्कोडेड हैं</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>क्या बाइट क्रम लिट्ल एंडियन है</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>क्या पिक्सेल डेटा इनकैप्सुलेटेड (संपीड़ित) है</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>क्या संपीड़न विधि लॉसी है</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>क्या सिंटैक्स डिफ्लेट संपीड़न उपयोग करता है</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>क्या ट्रांसफ़र सिंटैक्स DICOM मानक द्वारा रिटायर किया गया है</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>लॉसी संपीड़न विधि का ISO मानक पहचानकर्ता</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="लॉसी बनाम लॉसलेस संपीड़न">}}

<p>DICOM फ़ाइलों को ट्रांसकोड करते समय लॉसी और लॉसलेस संपीड़न के बीच अंतर समझना अत्यंत महत्वपूर्ण है:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>पहलू</th>
<th>लॉसलेस</th>
<th>लॉसी</th>
</tr>
</thead>
<tbody>
<tr><td>छवि गुणवत्ता</td><td>पिक्सेल-परफ़ेक्ट — मूल डेटा पूरी तरह संरक्षित</td><td>छोटी आकार प्राप्त करने के लिए कुछ डेटा स्थायी रूप से खो जाता है</td></tr>
<tr><td>संपीड़न अनुपात</td><td>आमतौर पर 2:1 से 3:1</td><td>आमतौर पर 10:1 से 30:1 या अधिक</td></tr>
<tr><td>राउंड-ट्रिप सुरक्षित</td><td>हां — डिकम्प्रेस करें और समान पिक्सेल प्राप्त करें</td><td>नहीं — प्रत्येक लॉसी री-एन्कोडिंग से गुणवत्ता और घटती है</td></tr>
<tr><td>उपयोग केस</td><td>आर्काइवल, डायग्नॉस्टिक्स, कानूनी रिकॉर्ड</td><td>प्रारंभिक समीक्षा, टेलीमेडिसिन, नेटवर्क ट्रांसमिशन</td></tr>
<tr><td>समर्थित कोडेक्स</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>महत्वपूर्ण:</strong> लॉसी- संपीड़ित फ़ाइल को लॉसलेस सिंटैक्स में ट्रांसकोड करने से खोया हुआ डेटा पुनर्स्थापित नहीं होता। मूल लॉसी संपीड़न से हुई गुणवत्ता गिरावट स्थायी होती है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शैक्षणिक संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="डॉक्यूमेंटेशन" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="सोर्स कोड" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API रेफ़रेंसेज़" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="निःशुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="सशुल्क समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
