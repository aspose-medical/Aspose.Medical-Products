---
title: C# .NET में DICOM को PNG में परिवर्तित करें | Aspose.Medical
weight: 11000
description: C# .NET में DICOM इमेजेस को पिक्सेल डेटा में रेंडर करें, बिना गुणवत्ता हानि के, विंडो/लेवल नियंत्रण और मल्टी-फ़्रेम समर्थन के साथ। Aspose.Medical API के साथ पूर्ण DICOM LUT पाइपलाइन का उपयोग करें और PNG में सहेजें।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में DICOM को PNG में परिवर्तित करें" h2="पूर्ण ग्रेस्केल पाइपलाइन समर्थन के साथ DICOM इमेजेस को कच्चे पिक्सेल डेटा में रेंडर करें। विंडो/लेवल नियंत्रण, ओवरले रेंडरिंग, और मल्टी-फ़्रेम प्रोसेसिंग के साथ इमेज गुणवत्ता बनाए रखें। किसी भी .NET इमेजिंग लाइब्रेरी का उपयोग करके बिना हानि वाला PNG सहेजें।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM से PNG रूपांतरण">}}

<p><strong>Aspose.Medical for .NET</strong> मानक DICOM ग्रेस्केल पाइपलाइन (Modality LUT, VOI LUT, Presentation LUT, Output LUT) के माध्यम से DICOM इमेजेस को रेंडर करता है ताकि सही विंडो किया हुआ BGRA32 पिक्सेल डेटा उत्पन्न हो सके। PNG एक lossless फॉर्मेट है, जो तब पसंदीदा विकल्प बनता है जब इमेज गुणवत्ता को बिना संगणन आर्टिफैक्ट के संरक्षित रखना आवश्यक हो — निदान समीक्षा, अभिलेखन, और अनुसंधान उपयोग केसों के लिए आवश्यक।</p>

<p>रेंडर किया गया आउटपुट एक <code>IRawImage</code> — एक BGRA32 पिक्सेल बफ़र है जिसमें <code>Width</code>, <code>Height</code>, और एक <code>Pixels</code> एरे होता है। आप इस पिक्सेल डेटा को किसी भी .NET इमेजिंग लाइब्रेरी जैसे <code>System.Drawing</code>, <code>SkiaSharp</code>, या <code>ImageSharp</code> का उपयोग करके PNG में सहेज सकते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# में DICOM को पिक्सेल डेटा में रेंडर करें">}}

<p>एक DICOM फ़ाइल लोड करें, इच्छित फ़्रेम को रेंडर करें, और पिक्सेल डेटा तक पहुंचें:</p>

<div class="codeblock" id="code">
 <h3>DICOM इमेज रेंडर करें - C#</h3>
 <pre><code class="cs">using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;

// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Render the first frame (index 0) through the full LUT pipeline
IRawImage image = dicomFile.RenderImage(0);

// Access the rendered pixel data
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = image.Pixels; // BGRA32 pixel buffer</code></pre>
</div>

<p><code>RenderImage</code> मेथड स्वचालित रूप से DICOM डेटासेट में संग्रहीत विंडो/लेवल मान और LUT पैरामीटर लागू करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="रेंडर किया गया इमेज PNG के रूप में सहेजें">}}

<p><code>IRawImage</code> कच्चा BGRA32 पिक्सेल डेटा प्रदान करता है जिसे किसी भी .NET इमेजिंग लाइब्रेरी का उपयोग करके lossless PNG में सहेजा जा सकता है। यहाँ <code>System.Drawing</code> का उपयोग करते हुए एक उदाहरण दिया गया है:</p>

<div class="codeblock" id="code">
 <h3>रेंडर किया गया DICOM फ्रेम PNG के रूप में सहेजें - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");
IRawImage image = dicomFile.RenderImage(0);

// Create a Bitmap from the BGRA32 pixel data
using var bitmap = new Bitmap(image.Width, image.Height, PixelFormat.Format32bppArgb);
var bitmapData = bitmap.LockBits(
    new Rectangle(0, 0, image.Width, image.Height),
    ImageLockMode.WriteOnly,
    PixelFormat.Format32bppArgb);

// Copy BGRA32 pixels to the bitmap
MemoryMarshal.AsBytes(image.Pixels.AsSpan())
    .CopyTo(new Span&lt;byte&gt;(
        (void*)bitmapData.Scan0,
        bitmapData.Stride * bitmapData.Height));

bitmap.UnlockBits(bitmapData);

// Save as lossless PNG
bitmap.Save("ct_scan.png", ImageFormat.Png);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="कस्टम विंडो/लेवल">}}

<p>विंडो सेंटर और विंडो विथ सेट करके यह नियंत्रित करें कि कौनसी पिक्सेल वैल्यू रेंडर होगी। विभिन्न विंडो सेटिंग्स समान DICOM डेटा से विभिन्न शारीरिक संरचनाओं को उजागर करती हैं:</p>

<div class="codeblock" id="code">
 <h3>कस्टम विंडो/लेवल के साथ रेंडर करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);</code></pre>
</div>

<p><code>GrayscaleRenderOptions</code> क्लास अतिरिक्त रूप से <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code>, और <code>BitDepth</code> प्रॉपर्टीज़ को उजागर करता है जिससे रेंडरिंग पाइपलाइन पर पूर्ण नियंत्रण मिलता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="मल्टी-फ़्रेम DICOM रेंडरिंग">}}

<p>CT, MRI, और अल्ट्रासाउंड से प्राप्त DICOM फ़ाइलें अक्सर कई फ़्रेम (स्लाइस) रखती हैं। सभी फ़्रेम्स को पिक्सेल डेटा में रेंडर करें:</p>

<div class="codeblock" id="code">
 <h3>मल्टी-फ़्रेम DICOM से सभी फ़्रेम रेंडर करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as PNG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ओवरले नियंत्रण">}}

<p>DICOM इमेजेस में एनोटेशन, माप या रुचि क्षेत्रों के साथ ओवरले प्लेन हो सकते हैं। रेंडरिंग के समय ओवरले की दृश्यमानता और रंग को नियंत्रित करें:</p>

<div class="codeblock" id="code">
 <h3>ओवरले के साथ और बिना ओवरले के रेंडर करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible (default)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions { ShowOverlays = false };
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="चिकित्सा इमेजेस के लिए PNG बनाम JPG">}}

<p>PNG और JPG के बीच चयन उपयोग केस पर निर्भर करता है:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>विशेषता</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>संपीड़न</strong></td><td>Lossless</td><td>Lossy</td></tr>
<tr><td><strong>इमेज गुणवत्ता</strong></td><td>Pixel-perfect &mdash; कोई आर्टिफैक्ट नहीं</td><td>छोटे संपीड़न आर्टिफैक्ट हो सकते हैं</td></tr>
<tr><td><strong>फ़ाइल आकार</strong></td><td>बड़ा (आम तौर पर JPG की तुलना में 2&ndash;10x)</td><td>छोटा</td></tr>
<tr><td><strong>पारदर्शिता</strong></td><td>समर्थित (alpha चैनल)</td><td>असमर्थित</td></tr>
<tr><td><strong>सबसे उपयुक्त</strong></td><td>निदान समीक्षा, अभिलेखन, अनुसंधान, ओवरले</td><td>वेब शेयरिंग, थंबनेल, प्रीव्यू</td></tr>
<tr><td><strong>नियामक</strong></td><td>गुणवत्ता-गंभीर कार्य प्रवाह के लिए पसंदीदा</td><td>गैर-निदान उपयोग के लिए स्वीकार्य</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET दोनों आउटपुट टार्गेट्स के लिए समान रेंडरिंग पाइपलाइन का उपयोग करता है। टार्गेट फॉर्मेट की परवाह किए बिना <code>IRawImage</code> पिक्सेल डेटा समान रहता है — फॉर्मेट चयन केवल आपके इमेजिंग लाइब्रेरी द्वारा किए गए अंतिम सहेजने के कदम को प्रभावित करता है।</p>

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
