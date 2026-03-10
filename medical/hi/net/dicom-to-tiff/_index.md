---
title: C# .NET में DICOM को TIFF में परिवर्तित करें | Aspose.Medical
weight: 12000
description: C# .NET में DICOM छवियों को पिक्सेल डेटा में रेंडर करें और मल्टी-फ़्रेम DICOM फ़ाइलों के लिए मल्टी‑पेज समर्थन के साथ TIFF में सहेजें। Aspose.Medical API के साथ पूर्ण DICOM LUT पाइपलाइन का उपयोग करें और किसी भी .NET इमेजिंग लाइब्रेरी का प्रयोग करके TIFF में सहेजें।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में DICOM को TIFF में परिवर्तित करें" h2="पूर्ण ग्रेस्केल पाइपलाइन समर्थन के साथ DICOM छवियों को रॉ पिक्सेल डेटा में रेंडर करें। विंडो/लेवल नियंत्रण, ओवरले रेंडरिंग, और मल्टी‑फ़्रेम प्रोसेसिंग के साथ छवि गुणवत्ता बनाए रखें। किसी भी .NET इमेजिंग लाइब्रेरी का उपयोग करके TIFF में सहेजें &mdash; जिसमें मल्टी‑पेज TIFF शामिल है &mdash;।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM से TIFF रूपांतरण">}}

<p><strong>Aspose.Medical for .NET</strong> मानक DICOM ग्रेस्केल पाइपलाइन (Modality LUT, VOI LUT, Presentation LUT, Output LUT) के माध्यम से DICOM छवियों को रेंडर करता है ताकि सही ढंग से विंडो किया गया BGRA32 पिक्सेल डेटा उत्पन्न हो। TIFF एक बहुमुखी फ़ॉर्मेट है जो चिकित्सा इमेजिंग, वैज्ञानिक अनुसंधान और प्रकाशन में व्यापक रूप से उपयोग किया जाता है क्योंकि यह लॉसलैस संपीड़न, उच्च बिट डेप्थ का समर्थन करता है, और &mdash; सबसे महत्वपूर्ण रूप से &mdash; <strong>मल्टी‑पेज दस्तावेज़</strong> को सपोर्ट करता है। यह TIFF को मल्टी‑फ़्रेम DICOM फ़ाइलों (CT, MRI, अल्ट्रासाउंड) को एकल आउटपुट फ़ाइल में परिवर्तित करने के लिए स्वाभाविक विकल्प बनाता है जो सभी स्लाइस को संरक्षित रखती है।</p>

<p>रेंडर किया गया आउटपुट एक <code>IRawImage</code> है &mdash; एक BGRA32 पिक्सेल बफ़र जिसमें <code>Width</code>, <code>Height</code>, और एक <code>Pixels</code> एरे शामिल है। आप इस पिक्सेल डेटा को किसी भी .NET इमेजिंग लाइब्रेरी जैसे <code>System.Drawing</code>, <code>SkiaSharp</code>, या <code>ImageSharp</code> का उपयोग करके TIFF में सहेज सकते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# में DICOM को पिक्सेल डेटा में रेंडर करें">}}

<p>एक DICOM फ़ाइल लोड करें, इच्छित फ्रेम को रेंडर करें, और पिक्सेल डेटा तक पहुँचें:</p>

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

<p><code>RenderImage</code> मेथड स्वचालित रूप से DICOM डेटासेट में संग्रहीत विंडो/लेवल मान और LUT पैरामीटर लागू करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="रेंडर की गई छवि को TIFF के रूप में सहेजें">}}

<p><code>IRawImage</code> कच्चा BGRA32 पिक्सेल डेटा प्रदान करता है जिसे किसी भी .NET इमेजिंग लाइब्रेरी का उपयोग करके TIFF में सहेजा जा सकता है। यहाँ <code>System.Drawing</code> का उपयोग करते हुए एक उदाहरण दिया गया है:</p>

<div class="codeblock" id="code">
 <h3>रेंडर किए गए DICOM फ्रेम को TIFF के रूप में सहेजें - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("xray.dcm");
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

// Save as TIFF
bitmap.Save("xray.tiff", ImageFormat.Tiff);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="मल्टी‑फ़्रेम DICOM से मल्टी‑पेज TIFF">}}

<p>मल्टी‑फ़्रेम DICOM फ़ाइलें (जैसे CT या MRI श्रृंखलाएँ) एक ही फ़ाइल में कई इमेज स्लाइस रखती हैं। TIFF की मल्टी‑पेज क्षमता आपको सभी फ्रेम को एक आउटपुट फ़ाइल में संग्रहीत करने की अनुमति देती है, जिससे सम्पूर्ण अध्ययन को सार्वभौमिक रूप से पठनीय फ़ॉर्मेट में संरक्षित किया जा सके। <code>System.Drawing</code> TIFF एनकोडर को मल्टी‑फ़्रेम पैरामिटर्स के साथ उपयोग करें:</p>

<div class="codeblock" id="code">
 <h3>मल्टी‑फ़्रेम DICOM को मल्टी‑पेज TIFF में बदलें - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
int totalFrames = dicomFile.NumberOfFrames;

// Get the TIFF codec
var tiffCodec = ImageCodecInfo.GetImageEncoders()
    .First(c =&gt; c.FormatID == ImageFormat.Tiff.Guid);

// Multi-frame parameters
var multiFrameParams = new EncoderParameters(2);
multiFrameParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.MultiFrame);
multiFrameParams.Param[1] = new EncoderParameter(
    Encoder.Compression, (long)EncoderValue.CompressionLZW);

// Parameter for adding subsequent frames
var frameAddParams = new EncoderParameters(1);
frameAddParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.FrameDimensionPage);

// Parameter for flushing the final frame
var flushParams = new EncoderParameters(1);
flushParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.Flush);

Bitmap? tiffBitmap = null;

for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var frameBitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = frameBitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    frameBitmap.UnlockBits(bitmapData);

    if (i == 0)
    {
        // Save the first frame and start multi-page TIFF
        tiffBitmap = (Bitmap)frameBitmap.Clone();
        tiffBitmap.Save("ct_series.tiff", tiffCodec, multiFrameParams);
    }
    else
    {
        // Add subsequent frames
        tiffBitmap!.SaveAdd(frameBitmap, frameAddParams);
    }
}

// Flush and close the multi-page TIFF
tiffBitmap?.SaveAdd(flushParams);
tiffBitmap?.Dispose();</code></pre>
</div>

<p>जब व्यक्तिगत स्लाइस की आवश्यकता हो तो आप प्रत्येक फ्रेम को अलग TIFF फ़ाइल के रूप में भी सहेज सकते हैं:</p>

<div class="codeblock" id="code">
 <h3>प्रत्येक फ्रेम को अलग TIFF में बदलें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

for (int i = 0; i &lt; dicomFile.NumberOfFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var bitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = bitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    bitmap.UnlockBits(bitmapData);
    bitmap.Save($"frame_{i:D4}.tiff", ImageFormat.Tiff);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="कस्टम विंडो/लेवल">}}

<p>Window Center और Window Width सेट करके यह नियंत्रित करें कि किस पिक्सेल मूल्य रेंज को रेंडर किया जाए। विभिन्न विंडो सेटिंग्स समान DICOM डेटा से विभिन्न शारीरिक संरचनाओं को उजागर करती हैं:</p>

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
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

<p><code>GrayscaleRenderOptions</code> क्लास additionally <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code>, और <code>BitDepth</code> प्रॉपर्टीज़ को उजागर करता है, जिससे रेंडरिंग पाइपलाइन पर पूर्ण नियंत्रण मिलता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="मल्टी‑फ़्रेम DICOM रेंडरिंग">}}

<p>CT, MRI, और अल्ट्रासाउंड से प्राप्त DICOM फ़ाइलों में अक्सर कई फ्रेम (स्लाइस) होते हैं। सभी फ्रेम को पिक्सेल डेटा में रेंडर करें:</p>

<div class="codeblock" id="code">
 <h3>मल्टी‑फ़्रेम DICOM से सभी फ्रेम रेंडर करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as TIFF using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ओवरले नियंत्रण">}}

<p>DICOM छवियों में एनोटेशन, माप या रुचि क्षेत्रों के साथ ओवरले प्लेन हो सकते हैं। रेंडर करते समय ओवरले की दृश्यता और रंग को नियंत्रित करें:</p>

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
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="चिकित्सा छवियों के लिए TIFF बनाम PNG बनाम JPG">}}

<p>प्रत्येक आउटपुट फ़ॉर्मेट विभिन्न उपयोग मामलों के लिए उपयुक्त है:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>विशेषता</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>संकुचन</strong></td><td>लॉसलैस (LZW, ZIP) या अनकम्प्रेस्ड</td><td>लॉसलैस</td><td>लॉसी</td></tr>
<tr><td><strong>मल्टी‑पेज</strong></td><td>समर्थित &mdash; सभी फ्रेम एक फ़ाइल में</td><td>असमर्थित</td><td>असमर्थित</td></tr>
<tr><td><strong>छवि गुणवत्ता</strong></td><td>पिक्सेल‑परफेक्ट</td><td>पिक्सेल‑परफेक्ट</td><td>संपीड़न आर्टिफैक्ट संभव</td></tr>
<tr><td><strong>फ़ाइल आकार</strong></td><td>सबसे बड़ी</td><td>मध्यम</td><td>सबसे छोटी</td></tr>
<tr><td><strong>पारदर्शिता</strong></td><td>समर्थित</td><td>समर्थित</td><td>असमर्थित</td></tr>
<tr><td><strong>सबसे उपयुक्त</strong></td><td>आर्काइविंग, मल्टी‑फ़्रेम श्रृंखला, प्रिंट, अनुसंधान</td><td>डायग्नोस्टिक समीक्षा, गुणवत्ता के साथ वेब</td><td>वेब शेयरिंग, थंबनेल</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET सभी आउटपुट टार्गेट के लिए समान रेंडरिंग पाइपलाइन का उपयोग करता है। <code>IRawImage</code> पिक्सेल डेटा टार्गेट फ़ॉर्मेट के बावजूद समान रहता है — फ़ॉर्मेट चयन केवल आपकी इमेजिंग लाइब्रेरी द्वारा अंतिम सहेजने के चरण को प्रभावित करता है। जब आपको एकल फ़ाइल में पूर्ण मल्टी‑फ़्रेम DICOM अध्ययन को संरक्षित रखना हो या आउटपुट आर्काइविंग, प्रिंटिंग, या आगे की इमेज प्रोसेसिंग के लिए हो, तब TIFF पसंदीदा फ़ॉर्मेट है।</p>

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
{{< blocks/products/pf/slr-element name="भुगतान समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Aspose.Medical for .NET क्यों?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
