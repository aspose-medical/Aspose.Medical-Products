---
title: C# .NET में DICOM ट्रांसफ़र सिंटैक्स रूपांतरण | Aspose.Medical
weight: 16000
description: C# .NET में ट्रांसफ़र सिंटैक्स के बीच DICOM फ़ाइलों को ट्रांसकोड करें। JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, RLE और अनकम्प्रेस्ड फ़ॉर्मैट्स का समर्थन Aspose.Medical API के साथ।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में DICOM ट्रांसफ़र सिंटैक्स रूपांतरण" h2="अनकम्प्रेस्ड, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS और RLE ट्रांसफ़र सिंटैक्स के बीच DICOM फ़ाइलों को ट्रांसकोड करें। शुद्ध .NET लाइब्रेरी, जिसमें कोई नेटिव निर्भरता नहीं है।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="ट्रांसफ़र सिंटैक्स क्या है?">}}

<p>एक <strong>Transfer Syntax</strong> DICOM डेटा को संग्रहण और प्रसारण के लिये कैसे एन्कोड किया जाता है, यह निर्धारित करता है। यह तीन प्रमुख पहलुओं को निर्दिष्ट करता है: बाइट क्रम (एंडियननेस), वैल्यू रिप्रेजेंटेशन स्पष्ट है या अप्रकट, और पिक्सेल डेटा पर लागू संपीड़न एल्गोरिद्म। प्रत्येक DICOM फ़ाइल अपने ट्रांसफ़र सिंटैक्स को फ़ाइल मेटा इन्फॉर्मेशन हेडर में घोषित करती है।</p>

<p>विभिन्न चिकित्सा उपकरण, PACS सर्वर्स और व्यूइंग एप्लिकेशन्स विभिन्न ट्रांसफ़र सिंटैक्स सेटों का समर्थन करते हैं। <strong>Aspose.Medical for .NET</strong> <code>Transcode</code> मेथड प्रदान करता है जो ट्रांसफ़र सिंटैक्स के बीच रूपांतरण सक्षम करता है, जिससे इंटरऑपरेबिलिटी, संग्रहण अनुकूलन और प्रोसेसिंग टूल्स के साथ संगति प्रदान होती है &mdash; यह सभी शुद्ध .NET लाइब्रेरी में, बिना किसी नेटिव निर्भरता के।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# में DICOM फ़ाइल को ट्रांसकोड करें">}}

<p><code>DicomFile.Transcode</code> मेथड DICOM फ़ाइल को उसकी वर्तमान ट्रांसफ़र सिंटैक्स से किसी भी समर्थित लक्ष्य सिंटैक्स में रूपांतरित करता है। यह मेथड एक नया <code>DicomFile</code> इंस्टेंस लौटाता है &mdash; मूल फ़ाइल अपरिवर्तित रहती है:</p>

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

<p>निम्न तालिका सभी मानक DICOM इमेज डेटा ट्रांसफ़र सिंटैक्स और Aspose.Medical for .NET में उनके वर्तमान समर्थन स्थिति को सूचीबद्ध करती है। सभी समर्थित कोडेक शुद्ध C# में लागू किए गए हैं और पूर्णतः प्लेटफ़ॉर्म-स्वतंत्र हैं।</p>

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
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Uncompressed</td><td>Supported</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Uncompressed</td><td>Supported</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Uncompressed (retired)</td><td>Supported</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Uncompressed</td><td>Not supported</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Supported</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Lossy, 8-bit</td><td>Supported</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Lossy, 12-bit</td><td>Not supported</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Lossless</td><td>Supported (8-bit only)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Lossless</td><td>Supported (8-bit only)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Near-lossless</td><td>Supported</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>Supported (read 8-bit color and 16-bit monochrome; write 16-bit monochrome or 8-bit RGB)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy or lossless</td><td>Supported (read 8-bit color and 16-bit monochrome; write 16-bit monochrome or 8-bit RGB)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Not supported</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy or lossless</td><td>Not supported</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy or lossless</td><td>Supported</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Lossless</td><td>Supported</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Lossless</td><td>Decode only (encoding needs a JPEG source stream, not pixel data)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Lossy or lossless</td><td>Supported (lossy mode)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="सामान्य ट्रांसकोडिंग परिदृश्य">}}

<p>विभिन्न कार्यप्रवाहों को विभिन्न ट्रांसकोडिंग रणनीतियों की आवश्यकता होती है। यहाँ सबसे सामान्य परिदृश्य प्रस्तुत हैं:</p>

<div class="codeblock" id="code">
 <h3>प्रोसेसिंग के लिए डीकम्प्रेस करें - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>आर्काइवल स्टोरेज के लिए कम्प्रेस करें - C#</h3>
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
 <h3>नेटवर्क ट्रांसमिशन के लिए कम्प्रेस करें - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>नवीनतम कोडेक्स का उपयोग करें: HTJ2K और JPEG XL - C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ट्रांसफ़र सिंटैक्स गुणों का निरीक्षण करें">}}

<p><code>TransferSyntax</code> क्लास ऐसी प्रॉपर्टीज़ प्रदान करती है जो एन्कोडिंग विशेषताओं का वर्णन करती हैं। इनका उपयोग फ़ाइल की वर्तमान ट्रांसफ़र सिंटैक्स की जाँच करने या उपयुक्त लक्ष्य सिंटैक्स चुनने के लिए करें:</p>

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
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>ट्रांसफ़र सिंटैक्स का अद्वितीय पहचानकर्ता</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>क्या वैल्यू रिप्रेजेंटेशन स्पष्ट रूप से एन्कोड किए गए हैं</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>क्या बाइट क्रम लिटिल एंडियन है</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>क्या पिक्सेल डेटा एन्कैप्सुलेटेड (कम्प्रेस्ड) है</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>क्या संपीड़न विधि लॉसी है</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>क्या सिंटैक्स डिफ्लेट संपीड़न का उपयोग करता है</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>क्या ट्रांसफ़र सिंटैक्स DICOM मानक द्वारा रिटायर्ड है</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>लॉसी संपीड़न विधि का ISO मानक पहचानकर्ता</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="लॉसी बनाम लॉसलैस संपीड़न">}}

<p>DICOM फ़ाइलों को ट्रांसकोड करते समय लॉसी और लॉसलैस संपीड़न के अंतर को समझना अत्यंत महत्वपूर्ण है:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>पहलू</th>
<th>लॉसलैस</th>
<th>लॉसी</th>
</tr>
</thead>
<tbody>
<tr><td>इमेज क्वालिटी</td><td>पिक्सेल-परफेक्ट &mdash; मूल डेटा पूरी तरह संरक्षित</td><td>छोटा आकार पाने के लिये कुछ डेटा स्थायी रूप से खो जाता है</td></tr>
<tr><td>संपीड़न अनुपात</td><td>आमतौर पर 2:1 से 3:1</td><td>आमतौर पर 10:1 से 30:1 या अधिक</td></tr>
<tr><td>राउंड-ट्रिप सुरक्षित</td><td>हाँ &mdash; डीकम्प्रेस करें और समान पिक्सेल प्राप्त करें</td><td>नहीं &mdash; प्रत्येक लॉसी री-एनकोड से गुणवत्ता और घटती है</td></tr>
<tr><td>उपयोग केस</td><td>आर्काइव, निदान, कानूनी रिकॉर्ड</td><td>प्रारंभिक समीक्षा, टेलीमेडिसिन, नेटवर्क ट्रांसमिशन</td></tr>
<tr><td>समर्थित कोडेक्स</td><td>JPEG लॉसलैस, JPEG-LS, JPEG 2000 लॉसलैस, HTJ2K लॉसलैस, JPEG XL लॉसलैस, RLE</td><td>JPEG बेसलाइन, JPEG-LS नियर-लॉसलैस, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>महत्वपूर्ण:</strong> लॉसी-भुगत फ़ाइल को लॉसलैस सिंटैक्स में ट्रांसकोड करने से खोया डेटा पुनः प्राप्त नहीं होता। मूल लॉसी संपीड़न से हुई गुणवत्ता ह्रास स्थायी है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="डॉक्यूमेंटेशन" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="सोर्स कोड" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API रेफ़रेंसेस" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="फ़्री सपोर्ट" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="पेड सपोर्ट" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता की कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
