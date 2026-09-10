---
title: C# .NET में DICOM JPEG 2000 संपीड़न | Aspose.Medical
weight: 2000
description: C# .NET में JPEG 2000 संपीड़न के साथ DICOM फाइलें पढ़ें, लिखें और ट्रांसकोड करें। 8-बिट और 16-बिट इमेज, लॉसलैस और लॉसी मोड, मल्टी-कम्पोनेंट डेटा के लिए Aspose.Medical API का समर्थन।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM JPEG 2000 समर्थन .NET C# में" h2="JPEG 2000 संपीड़न के साथ DICOM फाइलें पढ़ें, लिखें और ट्रांसकोड करें। लॉसलैस और लॉसी मोड, 8-bit और 16-bit पिक्सेल डेटा, मल्टी-कम्पोनेंट इमेज — सभी शुद्ध .NET में।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="चिकित्सा इमेजिंग में JPEG 2000">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) चिकित्सा इमेजिंग में सबसे अधिक उपयोग किया जाने वाला वेवलेट-आधारित संपीड़न मानक है। पारंपरिक JPEG से अलग, यह एक ही कोडेक में लॉसलैस और लॉसी दोनों संपीड़न प्रदान करता है, क्षेत्र-ऑफ़-इंटरेस्ट एक्सेस के लिए प्रोग्रेसिव डिकोडिंग और श्रेष्ठ संपीड़न अनुपात — जिससे बड़े अध्ययन को संग्रहीत करने और सीमित नेटवर्क पर छवियों को ट्रांसमिट करने के लिए यह आदर्श बनता है।</p>

<p><strong>Aspose.Medical for .NET</strong> JPEG 2000 कोडेक का शुद्ध C# कार्यान्वयन प्रदान करता है जिसमें कोई नेटिव निर्भरताएँ नहीं हैं। यह लाइब्रेरी चार मानक JPEG 2000 ट्रांसफर सिंटैक्स में से किसी भी के साथ संपीड़ित DICOM फाइलें पढ़, रेंडर और ट्रांसकोड कर सकती है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="समर्थित JPEG 2000 ट्रांसफर सिंटैक्स">}}

<table class="table table-bordered">
<thead>
<tr>
<th>ट्रांसफर सिंटैक्स</th>
<th>UID</th>
<th>मोड</th>
<th>पढ़ें</th>
<th>लिखें</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 केवल लॉसलैस</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>लॉसलैस</td><td>8-bit और 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>लॉसी या लॉसलैस</td><td>8-bit और 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 भाग 2 मल्टी-कम्पोनेंट केवल लॉसलैस</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>लॉसलैस</td><td>8-bit और 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 भाग 2 मल्टी-कम्पोनेंट</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>लॉसी या लॉसलैस</td><td>8-bit और 16-bit</td><td>8-bit</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8-बिट और 16-बिट पिक्सेल डेटा">}}

<p>चिकित्सा इमेज अक्सर प्रत्येक सैंपल पर 16 बिट उपयोग करती हैं ताकि CT (आमतौर पर 12-बिट को 16-बिट में संग्रहीत) और MRI जैसी मोडालिटीज़ की पूर्ण डायनामिक रेंज को कैप्चर किया जा सके। Aspose.Medical JPEG 2000 के लिए दोनों बिट गहराइयों को संभालता है:</p>

<ul>
<li><strong>पढ़ना (डिकम्प्रेशन)</strong>: 8-bit और 16-bit JPEG 2000 संपीड़ित DICOM फाइलों के लिए पूर्ण समर्थन। लाइब्रेरी मूल Bits Allocated, Bits Stored, और High Bit मानों की परवाह किए बिना पिक्सेल डेटा को सही ढंग से डिकोड करती है।</li>
<li><strong>लिखना (संपीड़न)</strong>: वर्तमान में 8-bit इमेज का समर्थन करता है। 16-bit लिखने का समर्थन भविष्य के रिलीज़ में योजनाबद्ध है।</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 संपीड़ित DICOM को पढ़ें और निरीक्षण करें - C#</h3>
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

<p>किसी भी DICOM फ़ाइल को JPEG 2000 में संपीड़ित करने या JPEG 2000 मोड्स के बीच परिवर्तित करने के लिए <code>Transcode</code> मेथड का उपयोग करें:</p>

<div class="codeblock" id="code">
 <h3>DICOM को JPEG 2000 लॉसलैस में संपीड़ित करें - C#</h3>
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

<p>प्रोसेसिंग, विश्लेषण, या उन सिस्टमों के साथ संगतता के लिए जो JPEG 2000 का समर्थन नहीं करते, JPEG 2000 फाइलों को अनकम्प्रेस्ड ट्रांसफर सिंटैक्स में डिकम्प्रेस करें:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 को अनकम्प्रेस्ड में डिकम्प्रेस करें - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>आप एक ही चरण में अन्य संपीड़न फॉर्मैट में भी डिकम्प्रेस और ट्रांसकोड कर सकते हैं:</p>

<div class="codeblock" id="code">
 <h3>संपीड़न फॉर्मैट के बीच ट्रांसकोड करें - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM इमेज को रेंडर करें">}}

<p>JPEG 2000 संपीड़ित DICOM फाइलों को डिस्प्ले या एक्सपोर्ट के लिए पिक्सेल डेटा में रेंडर किया जा सकता है, ठीक किसी भी अन्य ट्रांसफर सिंटैक्स की तरह:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 संपीड़ित फ्रेम को रेंडर करें - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="लॉसलैस बनाम लॉसी JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>पहलू</th>
<th>JPEG 2000 लॉसलैस</th>
<th>JPEG 2000 लॉसी</th>
</tr>
</thead>
<tbody>
<tr><td>ट्रांसफर सिंटैक्स</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>छवि गुणवत्ता</td><td>पिक्सेल-परफेक्ट &mdash; मूल के समान</td><td>दृश्यात्मक रूप से समान, कुछ डेटा स्थायी रूप से खो गया</td></tr>
<tr><td>संपीड़न अनुपात</td><td>आम तौर पर 2:1 से 3:1</td><td>आम तौर पर 10:1 से 30:1 या अधिक</td></tr>
<tr><td>उपयुक्त</td><td>डायग्नोस्टिक अभिलेखन, कानूनी रिकॉर्ड, प्राथमिक पढ़ना</td><td>प्रारंभिक समीक्षा, टेलीमेडिसिन, नेटवर्क ट्रांसमिशन</td></tr>
<tr><td>राउंड-ट्रिप सुरक्षित</td><td>हाँ</td><td>नहीं &mdash; पुनः एन्कोडिंग से गुणवत्ता और घटती है</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 भाग 2 मल्टी-कम्पोनेंट">}}

<p>JPEG 2000 भाग 2 (ISO/IEC 15444-2) मानक कोडेक को मल्टी-कम्पोनेंट ट्रांसफ़ॉर्म क्षमताओं से बढ़ाता है। यह रंगीन चिकित्सा इमेज और मल्टी-चैनल डेटा उत्पन्न करने वाली मोडालिटीज़ के लिए उपयोग होता है। Aspose.Medical दोनों भाग 2 ट्रांसफर सिंटैक्स को समर्थन देता है:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; मल्टी-चैनल डेटा की इष्टतम संपीड़न के लिए इंटर-कम्पोनेंट डेकोरिलेशन के साथ लॉसलैस संपीड़न।</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; मल्टी-कम्पोनेंट ट्रांसफ़ॉर्म्स के साथ लॉसी या लॉसलैस संपीड़न।</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="उच्च-थ्रूपुट JPEG 2000 (HTJ2K) — जल्द ही">}}

<p>HTJ2K (ISO/IEC 15444-15) JPEG 2000 का अगली पीढ़ी का विस्तार है, जो संपीड़न दक्षता को बनाए रखते हुए एन्कोड और डिकोड स्पीड को नाटकीय रूप से तेज़ बनाता है। यह वास्तविक‑समय चिकित्सा इमेजिंग कार्यप्रवाहों के लिए पसंदीदा कोडेक बनने की उम्मीद है।</p>

<p>Aspose.Medical भविष्य के रिलीज़ में HTJ2K समर्थन जोड़ देगा, जिसमें तीन ट्रांसफर सिंटैक्स शामिल होंगे:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; केवल लॉसलैस</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; RPCL प्रोग्रेशन क्रम के साथ लॉसलैस</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; लॉसी या लॉसलैस</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="सोर्स कोड" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API संदर्भ" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="मुफ़्त समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="पेड समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical .NET के लिए?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहक सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता की कहानियां" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
