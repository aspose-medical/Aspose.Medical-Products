---
title: Converteer DICOM naar TIFF in C# .NET | Aspose.Medical
weight: 12000
description: Render DICOM-afbeeldingen naar pixelgegevens in C# .NET en sla op als TIFF met multi-page-ondersteuning voor multi-frame DICOM-bestanden. Gebruik de volledige DICOM LUT-pijplijn met de Aspose.Medical API en sla op als TIFF met een willekeurige .NET imaging library.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converteer DICOM naar TIFF in .NET C#" h2="Render DICOM-afbeeldingen naar ruwe pixelgegevens met volledige grijstinten-pijplijnondersteuning. Behoud de beeldkwaliteit met window/level-controle, overlay-rendering en multi-frame verwerking. Sla op als TIFF &mdash; inclusief multi-page TIFF &mdash; met een willekeurige .NET imaging library." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM naar TIFF-conversie">}}

<p><strong>Aspose.Medical for .NET</strong> rendert DICOM-afbeeldingen via de standaard DICOM grijstinten-pijplijn (Modality LUT, VOI LUT, Presentation LUT, Output LUT) om correct windowed BGRA32 pixelgegevens te produceren. TIFF is een veelzijdig formaat dat veel wordt gebruikt in medische beeldvorming, wetenschappelijk onderzoek en publicatie omdat het lossless compressie, hoge bitdieptes ondersteunt en &mdash; vooral &mdash; <strong>meervoudige paginadocumenten</strong>. Dit maakt TIFF de natuurlijke keuze voor het converteren van multi-frame DICOM-bestanden (CT, MRI, echografie) naar één uitvoerbestand dat alle slices behoudt.</p>

<p>De gerenderde output is een <code>IRawImage</code> &mdash; een BGRA32 pixelbuffer met <code>Width</code>, <code>Height</code> en een <code>Pixels</code>-array. U kunt deze pixelgegevens opslaan als TIFF met een willekeurige .NET imaging library zoals <code>System.Drawing</code>, <code>SkiaSharp</code> of <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render DICOM naar pixelgegevens in C#">}}

<p>Laad een DICOM-bestand, render het gewenste frame en krijg toegang tot de pixelgegevens:</p>

<div class="codeblock" id="code">
 <h3>Render DICOM-afbeelding - C#</h3>
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

<p>De <code>RenderImage</code>-methode past automatisch window/level-waarden en LUT-parameters toe die zijn opgeslagen in de DICOM-dataset.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sla gerenderde afbeelding op als TIFF">}}

<p>De <code>IRawImage</code> levert ruwe BGRA32 pixelgegevens die kunnen worden opgeslagen als TIFF met een willekeurige .NET imaging library. Hieronder een voorbeeld met <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Sla gerenderd DICOM-frame op als TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Multi-page TIFF van multi-frame DICOM">}}

<p>Multi-frame DICOM-bestanden (bijv. CT- of MRI-series) bevatten meerdere beeldslices in één bestand. De multi-page mogelijkheid van TIFF stelt u in staat alle frames op te slaan in één uitvoerbestand, waarmee de volledige studie behouden blijft in een universeel leesbaar formaat. Gebruik de <code>System.Drawing</code> TIFF-encoder met multi-frame parameters:</p>

<div class="codeblock" id="code">
 <h3>Converteer multi-frame DICOM naar multi-page TIFF - C#</h3>
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

<p>U kunt ook elk frame opslaan als een afzonderlijk TIFF-bestand wanneer individuele slices nodig zijn:</p>

<div class="codeblock" id="code">
 <h3>Converteer elk frame naar een afzonderlijk TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Aangepast window/level">}}

<p>Stuur het bereik van pixelwaarden dat gerenderd wordt door het instellen van Window Center en Window Width. Verschillende window-instellingen onthullen verschillende anatomische structuren uit dezelfde DICOM-gegevens:</p>

<div class="codeblock" id="code">
 <h3>Render met aangepast window/level - C#</h3>
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

<p>De <code>GrayscaleRenderOptions</code>-klasse biedt ook de eigenschappen <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> en <code>BitDepth</code> voor volledige controle over de renderpijplijn.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderen van multi-frame DICOM">}}

<p>DICOM-bestanden van CT, MRI en echografie bevatten vaak meerdere frames (slices). Render alle frames naar pixelgegevens:</p>

<div class="codeblock" id="code">
 <h3>Render alle frames van multi-frame DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Overlay‑controle">}}

<p>DICOM-afbeeldingen kunnen overlay‑vlakken bevatten met annotaties, metingen of interessegebieden. Stuur de zichtbaarheid en kleur van overlays tijdens het renderen:</p>

<div class="codeblock" id="code">
 <h3>Render met en zonder overlays - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG voor medische beelden">}}

<p>Elk uitvoerformaat dient verschillende use‑cases:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Kenmerk</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Compressie</strong></td><td>Lossless (LZW, ZIP) of ongecomprimeerd</td><td>Lossless</td><td>Lossy</td></tr>
<tr><td><strong>Multi-Page</strong></td><td>Ondersteund &mdash; alle frames in één bestand</td><td>Niet ondersteund</td><td>Niet ondersteund</td></tr>
<tr><td><strong>Beeldkwaliteit</strong></td><td>Pixel-perfect</td><td>Pixel-perfect</td><td>Compressie‑artefacten mogelijk</td></tr>
<tr><td><strong>Bestandsgrootte</strong></td><td>Grootst</td><td>Gemiddeld</td><td>Kleinste</td></tr>
<tr><td><strong>Transparantie</strong></td><td>Ondersteund</td><td>Ondersteund</td><td>Niet ondersteund</td></tr>
<tr><td><strong>Beste voor</strong></td><td>Archivering, multi-frame series, printen, onderzoek</td><td>Diagnostische beoordeling, web met kwaliteit</td><td>Webdeling, miniaturen</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET gebruikt dezelfde renderpijplijn voor alle uitvoerdoelen. De <code>IRawImage</code>-pixelgegevens zijn identiek, ongeacht het doelformaat — de keuze van formaat beïnvloedt alleen de uiteindelijke opslagnorm uitgevoerd door uw imaging library. TIFF is het voorkeursformaat wanneer u een volledige multi-frame DICOM-studie in één bestand moet behouden of wanneer de output bedoeld is voor archivering, afdrukken of verdere beeldverwerking.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leerbronnen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Broncode" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API-referenties" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Productondersteuning" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis ondersteuning" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betaalde ondersteuning" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Waarom Aspose.Medical voor .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Klantenlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
