---
title: C# .NET में DICOM JPEG 2000 संपीड़न | Aspose.Medical
weight: 2000
description: C# .NET में JPEG 2000 संपीड़न के साथ DICOM फ़ाइलों को पढ़ें, लिखें और ट्रांसकोड करें। 8-बिट रंग और 16-बिट मोनोक्रोम छवियों, लॉसलेस और लॉसी मोड्स, साथ ही Aspose.Medical API के साथ HTJ2K को सपोर्ट करता है।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में DICOM JPEG 2000 समर्थन" h2="JPEG 2000 संपीड़न के साथ DICOM फ़ाइलों को पढ़ें, लिखें और ट्रांसकोड करें। लॉसलेस और लॉसी मोड्स, 8-बिट रंग और 16-बिट मोनोक्रोम पिक्सेल डेटा, HTJ2K शामिल - सभी शुद्ध .NET में।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="मेडिकल इमेजिंग में JPEG 2000">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) मेडिकल इमेजिंग में सबसे अधिक उपयोग किया जाने वाला वेवलेट-आधारित संपीड़न मानक है। पारंपरिक JPEG के विपरीत, यह एक ही कोडेक में लॉसलेस और लॉसी दोनों संपीड़न प्रदान करता है, रुचि-क्षेत्र तक पहुँच के लिए प्रोग्रेसिव डिकोडिंग, और श्रेष्ठ संपीड़न अनुपात &mdash; जिससे यह बड़े अध्ययन को संग्रहीत करने और सीमित नेटवर्क पर छवियों को प्रसारित करने के लिए आदर्श बनता है।</p>

<p><strong>Aspose.Medical for .NET</strong> बिना किसी नेटिव निर्भरता के JPEG 2000 कोडेक का शुद्ध C# कार्यान्वयन प्रदान करता है। यह लाइब्रेरी किसी भी चार मानक JPEG 2000 ट्रांसफर सिंटैक्स से संकुचित DICOM फ़ाइलों को पढ़ सकती है, रेंडर कर सकती है और ट्रांसकोड कर सकती है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="समर्थित JPEG 2000 ट्रांसफर सिंटैक्सेस">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>मोड</th>
<th>पढ़ें</th>
<th>लिखें</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 केवल लॉसलेस</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>लॉसलेस</td><td>8-बिट RGB, 16-बिट मोनोक्रोम</td><td>16-बिट मोनोक्रोम, 8-बिट RGB</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>लॉसी या लॉसलेस</td><td>8-बिट RGB, 16-बिट मोनोक्रोम</td><td>16-बिट मोनोक्रोम, 8-बिट RGB</td></tr>
<tr><td>JPEG 2000 भाग 2 बहु‑घटक केवल लॉसलेस</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>लॉसलेस</td><td>समर्थित नहीं</td><td>समर्थित नहीं</td></tr>
<tr><td>JPEG 2000 भाग 2 बहु‑घटक</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>लॉसी या लॉसलेस</td><td>समर्थित नहीं</td><td>समर्थित नहीं</td></tr>
<tr><td>HTJ2K केवल लॉसलेस</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>लॉसलेस</td><td>मोनोक्रोम और रंग</td><td>मोनोक्रोम और रंग</td></tr>
<tr><td>HTJ2K RPCL विकल्पों के साथ केवल लॉसलेस</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>लॉसलेस</td><td>मोनोक्रोम और रंग</td><td>मोनोक्रोम और रंग</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>लॉसी या लॉसलेस</td><td>मोनोक्रोम और रंग</td><td>मोनोक्रोम और रंग</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8-बिट और 16-बिट पिक्सेल डेटा">}}

<p>मेडिकल इमेजिंग अक्सर प्रत्येक सैंपल के लिए 16 बिट्स का उपयोग करती है ताकि CT (आमतौर पर 12‑बिट 16‑बिट में संग्रहीत) और MRI जैसी मोडैलिटी की पूरी डायनामिक रेंज को कैप्चर किया जा सके। Aspose.Medical JPEG 2000 के लिए दोनों बिट‑गहराइयों को संभालता है:</p>

<ul>
<li><strong>रीडिंग (डिकम्प्रेशन)</strong>: 16‑बिट मोनोक्रोम फ़ाइलें (CT, MRI, X‑ray) और 8‑बिट त्रि‑घटक रंग फ़ाइलें (RGB, YBR_RCT, YBR_ICT)। पैलेट, CMYK, ICC‑प्रोफ़ाइल और सब‑सैंप्ल्ड रंग कोड स्ट्रीम को स्पष्ट अपवाद के साथ अस्वीकृत किया जाता है, न कि चुपचाप गलत इमेज के रूप में।</li>
<li><strong>राइटिंग (कम्प्रेशन)</strong>: 16‑बिट मोनोक्रोम और 8‑बिट RGB छवियां। 8‑बिट मोनोक्रोम और 16‑बिट रंग एन्कोडिंग उपलब्ध नहीं है; इनके लिए HTJ2K या JPEG XL का उपयोग करें, दोनों ही मोनोक्रोम और रंग को किसी भी बिट‑डेप्थ पर स्वीकार करते हैं।</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 संकुचित DICOM पढ़ें और जांचें - C#</h3>
 <pre><code class="cs">// Open a JPEG 2000 compressed DICOM file (8-bit or 16-bit)
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Check transfer syntax
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax
Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");

// Inspect pixel data bit depth
PixelData pixelData = PixelData.Create(dicomFile.Dataset);
Console.WriteLine($"Bits Allocated: {pixelData.BitsAllocated}");
Console.WriteLine($"Bits Stored: {pixelData.BitsStored}");
Console.WriteLine($"High Bit: {pixelData.HighBit}");
Console.WriteLine($"Samples Per Pixel: {pixelData.SamplesPerPixel}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 में ट्रांसकोड करें">}}

<p>किसी भी DICOM फ़ाइल को JPEG 2000 में संकुचित करने या JPEG 2000 मोड्स के बीच परिवर्तित करने के लिए <code>Transcode</code> मेथड का उपयोग करें:</p>

<div class="codeblock" id="code">
 <h3>DICOM को JPEG 2000 लॉसलेस में संपीड़ित करें - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>DICOM को JPEG 2000 लॉसी में संपीड़ित करें - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM फ़ाइलों को डिकम्प्रेस करें">}}

<p>प्रोसेसिंग, विश्लेषण, या उन सिस्टमों के साथ संगतता के लिए जो JPEG 2000 को सपोर्ट नहीं करते, JPEG 2000 फ़ाइलों को अनकम्प्रेस्ड ट्रांसफर सिंटैक्स में डिकम्प्रेस करें:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 को अनकम्प्रेस्ड में डिकम्प्रेस करें - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>आप एक ही चरण में डिकम्प्रेस और अन्य संपीड़न फॉर्मेट्स में ट्रांसकोड भी कर सकते हैं:</p>

<div class="codeblock" id="code">
 <h3>संपीड़न फॉर्मेट्स के बीच ट्रांसकोड - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM इमेजेज़ को रेंडर करें">}}

<p>JPEG 2000 संकुचित DICOM फ़ाइलों को पिक्सेल डेटा में रेंडर किया जा सकता है ताकि उन्हें प्रदर्शित या निर्यात किया जा सके, ठीक वैसे ही जैसे कोई अन्य ट्रांसफर सिंटैक्स हो।</p>

<div class="codeblock" id="code">
 <h3>एक JPEG 2000 संकुचित फ्रेम को रेंडर करें - C#</h3>
 <pre><code class="cs">// Open JPEG 2000 DICOM file
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Render the first frame — decompression is handled automatically
using PixelImage&lt;Bgra32&gt; image = dicomFile.RenderImage(0);

// Copy the BGRA32 pixel data for display or export
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = new Bgra32[width * height];
image.CopyPixelsTo(pixels);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="लॉसलेस बनाम लॉसी JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>पहलू</th>
<th>JPEG 2000 लॉसलेस</th>
<th>JPEG 2000 लॉसी</th>
</tr>
</thead>
<tbody>
<tr><td>ट्रांसफर सिंटैक्स</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>इमेज क्वालिटी</td><td>पिक्सेल‑परफेक्ट &mdash; मूल के समान</td><td>दृश्य रूप से समान, कुछ डेटा स्थायी रूप से खो गया</td></tr>
<tr><td>संकुचन अनुपात</td><td>आमतौर पर 2:1 से 3:1</td><td>आमतौर पर 10:1 से 30:1 या अधिक</td></tr>
<tr><td>सबसे उपयुक्त</td><td>डायग्नोस्टिक अभिलेखन, कानूनी रिकॉर्ड, प्राथमिक पठन</td><td>प्रारंभिक समीक्षा, टेलीमेडिसिन, नेटवर्क ट्रांसमिशन</td></tr>
<tr><td>राउंड‑ट्रिप सुरक्षित</td><td>हाँ</td><td>नहीं &mdash; पुनः‑एन्कोडिंग से गुणवत्ता और घटती है</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="हाई‑थ्रूपुट JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) JPEG 2000 के धीमे अरिथमैटिक कोडर को एक तेज़ ब्लॉक कोडर से बदलता है। यह वही वेवलेट ट्रांसफ़ॉर्म, प्रोग्रेशन क्रम और गुणवत्ता रखता है, और कई गुना तेज़ डिकोड और एन्कोड करता है। Aspose.Medical शुद्ध .NET में सभी तीन DICOM HTJ2K ट्रांसफर सिंटैक्स को लागू करता है, मोनोक्रोम और रंग छवियों के लिए, और HTJ2K तथा अन्य सभी समर्थित सिंटैक्स के बीच ट्रांसकोड करता है:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; केवल लॉसलेस</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; RPCL प्रोग्रेशन ऑर्डर के साथ लॉसलेस</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; लॉसी या लॉसलेस</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 को HTJ2K में और वापस ट्रांसकोड करें - C#</h3>
 <pre><code class="cs">// Transcode a JPEG 2000 file to HTJ2K, and back to classic JPEG 2000
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile htj2kFile = j2kFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2kFile.Save("htj2k_lossless.dcm");

// HTJ2K with RPCL progression order, lossless
DicomFile rpclFile = j2kFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
rpclFile.Save("htj2k_rpcl.dcm");

// HTJ2K lossy
DicomFile htj2kLossy = j2kFile.Transcode(TransferSyntax.HTJ2K);
htj2kLossy.Save("htj2k_lossy.dcm");

// Any HTJ2K file decodes back to an uncompressed transfer syntax
DicomFile uncompressed = htj2kFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decoded.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="प्रलेखन" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="सोर्स कोड" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API संदर्भ" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="निःशुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="सशुल्क समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहक सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता कहानियां" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
