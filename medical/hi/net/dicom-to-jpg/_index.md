---
title: C# .NET में DICOM को JPG में परिवर्तित करें | Aspose.Medical
weight: 7000
description: C# .NET में विंडो/लेवल नियंत्रण, ओवरले रेंडरिंग, और मल्टी-फ़्रेम समर्थन के साथ DICOM छवियों को पिक्सेल डेटा में रेंडर करें। Aspose.Medical API के साथ पूर्ण DICOM LUT पाइपलाइन का उपयोग करें और JPG में सहेजें।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में DICOM को JPG में बदलें" h2="विंडो/लेवल, मोडालिटी LUT, VOI LUT, और ओवरले रेंडरिंग सहित पूर्ण ग्रेस्केल पाइपलाइन समर्थन के साथ DICOM छवियों को कच्चे पिक्सेल डेटा में रेंडर करें। किसी भी .NET इमेजिंग लाइब्रेरी का उपयोग करके JPG में सहेजें।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM छवि रेंडरिंग पाइपलाइन">}}

<p><strong>Aspose.Medical for .NET</strong> DICOM विनिर्देशन में परिभाषित मानक DICOM ग्रेस्केल पाइपलाइन के माध्यम से DICOM छवियों को रेंडर करता है। रेंडरिंग प्रक्रिया कच्चे संग्रहीत पिक्सेल मानों को प्रदर्शनीय छवियों में बदलने के लिए लुक‑अप टेबल्स (LUTs) की श्रृंखला लागू करती है:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; संग्रहीत पिक्सेल मानों को Rescale Slope/Intercept या LUT Sequence का उपयोग करके निर्माता‑स्वतंत्र मानों में परिवर्तित करता है।</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; डिस्प्ले करने के लिए मानों की सीमा चुनने हेतु विंडो/लेवल (Window Center और Window Width) लागू करता है, जिसमें Linear, Linear Exact, और Sigmoid परिवर्तन का समर्थन शामिल है।</li>
<li><strong>Presentation LUT</strong> &mdash; अंतिम मानों को प्रदर्शनीय P-Values में मैप करता है, जिसमें INVERSE (छवि उलटना) का समर्थन शामिल है।</li>
<li><strong>Output LUT</strong> &mdash; डिस्प्ले हेतु ग्रेस्केल मानों को RGB में परिवर्तित करता है, वैकल्पिक रूप से कस्टम कलर मैप्स या Palette Color तालिकाओं से रंगीन बनाता है।</li>
</ol>

<p>रेंडर किया गया आउटपुट एक <code>IRawImage</code> है &mdash; एक BGRA32 पिक्सेल बफ़र (प्रति चैनल 8‑bit) जिसमें <code>Width</code>, <code>Height</code>, और एक <code>Pixels</code> एरे होता है। आप इस पिक्सेल डेटा को किसी भी .NET इमेजिंग लाइब्रेरी जैसे <code>System.Drawing</code>, <code>SkiaSharp</code>, या <code>ImageSharp</code> का उपयोग करके JPG में सहेज सकते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# में DICOM को पिक्सेल डेटा में रेंडर करें">}}

<p>एक DICOM फ़ाइल लोड करें और एक फ्रेम को BGRA32 पिक्सेल डेटा वाली <code>IRawImage</code> में रेंडर करें:</p>

<div class="codeblock" id="code">
 <h3>DICOM छवि रेंडर करें - C#</h3>
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

<p><code>RenderImage</code> मेथड स्वचालित रूप से विंडो/लेवल मानों और DICOM फ़ाइल में संग्रहीत LUT पैरामीटरों का उपयोग करके पूर्ण ग्रेस्केल पाइपलाइन लागू करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="रेंडर की गई छवि को JPG के रूप में सहेजें">}}

<p><code>IRawImage</code> कच्चा BGRA32 पिक्सेल डेटा प्रदान करता है जिसे किसी भी .NET इमेजिंग लाइब्रेरी का उपयोग करके JPG में सहेजा जा सकता है। यहाँ <code>System.Drawing</code> का उपयोग करते हुए एक उदाहरण है:</p>

<div class="codeblock" id="code">
 <h3>रेंडर किया गया DICOM फ्रेम को JPG के रूप में सहेजें - C#</h3>
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

// Save as JPG
bitmap.Save("ct_scan.jpg", ImageFormat.Jpeg);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="कस्टम विंडो/लेवल (विंडोइंग)">}}

<p>विंडो/लेवल (जिसे विंडोइंग या W/L भी कहा जाता है) DICOM छवि रेंडरिंग के लिए सबसे महत्वपूर्ण पैरामीटर है। यह निर्धारित करता है कि कौन सी पिक्सेल मानों की सीमा दृश्य ग्रेस्केल सीमा में मैप की जाएगी। <strong>Window Center</strong> मध्य बिंदु को परिभाषित करता है और <strong>Window Width</strong> प्रदर्शित मानों की सीमा को निर्धारित करता है:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; उच्च कंट्रास्ट, कम ग्रे लेवल दर्शाए जाते हैं (सॉफ्ट टिश्यू के लिए उपयोगी)।</li>
<li><strong>Wide window</strong> &mdash; निचला कंट्रास्ट, अधिक ग्रे लेवल दर्शाए जाते हैं (हड्डी या फेफड़े के लिए उपयोगी)।</li>
</ul>

<div class="codeblock" id="code">
 <h3>कस्टम विंडो/लेवल के साथ रेंडर करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window: center=40, width=400
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window: center=400, width=1800
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Lung window: center=-600, width=1500
var lung = new GrayscaleRenderOptions
{
    WindowCenter = -600,
    WindowWidth = 1500
};
IRawImage lungImage = dicomFile.RenderImage(lung, 0);</code></pre>
</div>

<p>सामान्य CT विंडो प्रीसेट्स:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>टिश्यू प्रकार</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>सॉफ्ट टिश्यू</td><td>40</td><td>400</td></tr>
<tr><td>फेफड़ा</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>हड्डी</td><td>400</td><td>1800</td></tr>
<tr><td>मस्तिष्क</td><td>40</td><td>80</td></tr>
<tr><td>जिगर</td><td>60</td><td>150</td></tr>
<tr><td>Mediastinum</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ग्रेस्केल रेंडर विकल्प">}}

<p><code>GrayscaleRenderOptions</code> क्लास ग्रेस्केल रेंडरिंग पाइपलाइन पर पूर्ण नियंत्रण प्रदान करती है:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>प्रॉपर्टी</th>
<th>विवरण</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>प्रदर्शित मान सीमा का मध्य (VOI LUT का क्षैतिज मध्य बिंदु)</td></tr>
<tr><td><code>WindowWidth</code></td><td>प्रदर्शित मान सीमा की चौड़ाई (कॉन्ट्रास्ट को नियंत्रित करता है)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Modality LUT रिस्केल स्लोप — संग्रहीत पिक्सेल मानों से गुणा किया जाता है</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Modality LUT रिस्केल इंटरसेप्ट — स्लोप गुणन के बाद जोड़ा जाता है</td></tr>
<tr><td><code>Invert</code></td><td>ग्रेस्केल छवि को उलट देता है (INVERSE Presentation LUT आकार लागू करता है)</td></tr>
<tr><td><code>BitDepth</code></td><td>बिट डेप्थ जानकारी: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>उलटा ग्रेस्केल रेंडरिंग - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray.dcm");

// Render with inverted grayscale (white-on-black becomes black-on-white)
var options = new GrayscaleRenderOptions
{
    WindowCenter = 2048,
    WindowWidth = 4096,
    Invert = true
};

IRawImage image = dicomFile.RenderImage(options, 0);</code></pre>
</div>

<p>DICOM डेटासेट के पिक्सेल डेटा एट्रिब्यूट्स से, बिट डेप्थ, रिस्केल पैरामीटर, विंडो/लेवल, और इनवर्जन सेटिंग्स सहित, रेंडर विकल्पों को स्वचालित रूप से भरने के लिए <code>RenderOptionsFactory.Create(pixelData)</code> का उपयोग करें।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ओवरले रेंडरिंग">}}

<p>DICOM छवियों में ओवरले प्लेन हो सकते हैं — ग्राफिक्स या टेक्स्ट एनोटेशन जो छवि में एम्बेडेड होते हैं। <code>RenderOptions</code> क्लास यह नियंत्रित करती है कि ओवरले रेंडर किए जाएँ या नहीं और उनका रंग क्या रहेगा:</p>

<div class="codeblock" id="code">
 <h3>ओवरले नियंत्रण के साथ रेंडर करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible in white (default behavior)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions
{
    ShowOverlays = false
};
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

<p>DICOM दो प्रकार के ओवरले का समर्थन करता है: <strong>Graphics</strong> ओवरले (एनोटेशन, माप) और <strong>Region of Interest</strong> (ROI) ओवरले। दोनों प्रकार <code>ShowOverlays</code> सेटिंग द्वारा नियंत्रित होते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="मल्टी-फ़्रेम DICOM रेंडरिंग">}}

<p>CT, MRI, और अल्ट्रासाउंड के कई DICOM फ़ाइलों में कई फ़्रेम (स्लाइस) होते हैं। आप फ़्रेम की संख्या जांच सकते हैं और प्रत्येक को अलग‑अलग रेंडर कर सकते हैं:</p>

<div class="codeblock" id="code">
 <h3>मल्टी-फ़्रेम DICOM से सभी फ़्रेम रेंडर करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;
Console.WriteLine($"Total frames: {totalFrames}");

// Render all frames to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as JPG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="VOI LUT ट्रांसफ़ॉर्मेशन प्रकार">}}

<p>DICOM मानक कई VOI LUT ट्रांसफ़ॉर्मेशन फ़ंक्शन परिभाषित करता है जो विंडो/लेवल मैपिंग के लागू होने को नियंत्रित करते हैं। Aspose.Medical for .NET सभी मानक प्रकारों को समर्थन देता है:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>प्रकार</th>
<th>विवरण</th>
<th>उपयोग केस</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>केंद्र से ऑफसेट के साथ मानक रैखिक मैपिंग</td><td>सबसे सामान्य, सामान्य CT/MR इमेजिंग के लिए उपयोग किया जाता है</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>केंद्र से बिना ऑफसेट के रैखिक मैपिंग</td><td>जब सटीक पिक्सेल मान मैपिंग आवश्यक हो</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>समतल कॉन्ट्रास्ट संक्रमण के लिए S-क्रव परिवर्तन</td><td>सॉफ्ट टिश्यू विज़ुअलाइज़ेशन, मैमोग्राफी</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>DICOM डेटासेट में लुक‑अप टेबल के रूप में परिभाषित कस्टम LUT</td><td>वेंडर‑विशिष्ट या मोडालिटी‑विशिष्ट डिस्प्ले कर्व्स</td></tr>
</tbody>
</table>

<p>रेंडरिंग इंजन DICOM डेटासेट एट्रिब्यूट्स के आधार पर उपयुक्त VOI LUT ट्रांसफ़ॉर्मेशन को स्वचालित रूप से चुनता है। कस्टम <code>GrayscaleRenderOptions</code> का उपयोग करने पर, डिफ़ॉल्ट रूप से Linear ट्रांसफ़ॉर्मेशन लागू किया जाता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="समर्थित मोडालिटीज़">}}

<p>Aspose.Medical for .NET सभी सामान्य मेडिकल इमेजिंग मोडालिटीज़ से DICOM छवियों को रेंडर करने का समर्थन करता है:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>मोडालिटी</th>
<th>विवरण</th>
<th>सामान्य फ़ॉर्मेट</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>कम्प्यूटेड टॉमोग्राफी</td><td>ग्रेस्केल, मल्टी‑फ़्रेम, 12–16 बिट</td></tr>
<tr><td><strong>MR</strong></td><td>मैग्नेटिक रेज़ोनेंस</td><td>ग्रेस्केल, मल्टी‑फ़्रेम, 12–16 बिट</td></tr>
<tr><td><strong>CR / DX</strong></td><td>कम्प्यूटेड / डिजिटल रेडियोग्राफी</td><td>ग्रेस्केल, सिंगल फ्रेम, 10–16 बिट</td></tr>
<tr><td><strong>US</strong></td><td>अल्ट्रासाउंड</td><td>ग्रेस्केल या RGB, मल्टी‑फ़्रेम</td></tr>
<tr><td><strong>MG</strong></td><td>मैमोग्राफी</td><td>ग्रेस्केल, हाई रिज़ॉल्यूशन, 12–16 बिट</td></tr>
<tr><td><strong>XA</strong></td><td>एक्स‑रे एंजियोग्राफी</td><td>ग्रेस्केल, मल्टी‑फ़्रेम</td></tr>
<tr><td><strong>NM</strong></td><td>न्यूक्लियर मेडिसिन</td><td>ग्रेस्केल, मल्टी‑फ़्रेम</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Positron Emission Tomography)</td><td>ग्रेस्केल, मल्टी‑फ़्रेम</td></tr>
</tbody>
</table>

<p>ग्रेस्केल (MONOCHROME1/MONOCHROME2) और रंग (RGB, YBR_FULL, PALETTE COLOR) दोनों फोटोमेट्रिक व्याख्याओं का समर्थन किया जाता है। रेंडरिंग इंजन स्वचालित रूप से उपयुक्त कलर स्पेस परिवर्तन को संभालता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="स्रोत कोड" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API संदर्भ" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="नि:शुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="भुगतान समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Aspose.Medical for .NET क्यों?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता कहानियां" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
