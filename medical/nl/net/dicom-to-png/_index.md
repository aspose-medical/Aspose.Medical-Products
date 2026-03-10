---
title: Converteer DICOM naar PNG in C# .NET | Aspose.Medical
weight: 11000
description: Render DICOM-afbeeldingen naar pixelgegevens in C# .NET met verliesvrije kwaliteit, window/level‑controle en ondersteuning voor meerdere frames. Gebruik de volledige DICOM LUT‑pipeline met de Aspose.Medical API en sla op als PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converteer DICOM naar PNG in .NET C#" h2="Render DICOM-afbeeldingen naar ruwe pixelgegevens met volledige grijswaardepijplijnondersteuning. Behoud de beeldkwaliteit met window/level‑controle, overlay‑rendering en verwerking van meerdere frames. Sla op als verliesvrije PNG met een willekeurige .NET‑beeldverwerkingsbibliotheek." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM‑naar‑PNG-conversie">}}

<p><strong>Aspose.Medical for .NET</strong> rendert DICOM‑afbeeldingen via de standaard DICOM-grijswaardepijplijn (Modality LUT, VOI LUT, Presentation LUT, Output LUT) om correct geopende BGRA32‑pixelgegevens te produceren. PNG is een verliesformaat, waardoor het de voorkeur heeft wanneer beeldkwaliteit moet worden behouden zonder compressie‑artefacten &mdash; essentieel voor diagnostische beoordeling, archivering en onderzoekstoepassingen.</p>

<p>De gerenderde output is een <code>IRawImage</code> &mdash; een BGRA32‑pixelbuffer met <code>Width</code>, <code>Height</code> en een <code>Pixels</code>-array. U kunt deze pixelgegevens opslaan als PNG met een willekeurige .NET‑beeldverwerkingsbibliotheek, zoals <code>System.Drawing</code>, <code>SkiaSharp</code> of <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render DICOM naar pixelgegevens in C#">}}

<p>Laad een DICOM‑bestand, render het gewenste frame en krijg toegang tot de pixelgegevens:</p>

<div class="codeblock" id="code">
 <h3>Render DICOM‑afbeelding - C#</h3>
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

<p>De <code>RenderImage</code>-methode past automatisch de window/level‑waarden en LUT‑parameters toe die in de DICOM‑dataset zijn opgeslagen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sla gerenderde afbeelding op als PNG">}}

<p>De <code>IRawImage</code> levert ruwe BGRA32‑pixelgegevens die met een willekeurige .NET‑beeldverwerkingsbibliotheek als verliesvrije PNG kunnen worden opgeslagen. Hieronder een voorbeeld met <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Sla gerenderd DICOM‑frame op als PNG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Aangepast window/level">}}

<p>Stuur het bereik van pixelwaarden dat wordt gerenderd door de Window Center en Window Width in te stellen. Verschillende window‑instellingen onthullen verschillende anatomische structuren uit dezelfde DICOM‑gegevens:</p>

<div class="codeblock" id="code">
 <h3>Renderen met aangepast window/level - C#</h3>
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

<p>De <code>GrayscaleRenderOptions</code>-klasse biedt ook de eigenschappen <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> en <code>BitDepth</code> voor volledige controle over de renderpipeline.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderen van multi‑frame DICOM">}}

<p>DICOM‑bestanden van CT, MRI en echografie bevatten vaak meerdere frames (slices). Render alle frames naar pixelgegevens:</p>

<div class="codeblock" id="code">
 <h3>Render alle frames van multi‑frame DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Overlay‑beheer">}}

<p>DICOM‑afbeeldingen kunnen overlay‑vlakken bevatten met annotaties, metingen of interessegebieden. Beheer de zichtbaarheid en kleur van overlays tijdens het renderen:</p>

<div class="codeblock" id="code">
 <h3>Renderen met en zonder overlays - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG voor medische beelden">}}

<p>De keuze tussen PNG en JPG hangt af van het gebruiksscenario:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Kenmerk</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Compressie</strong></td><td>Lossless</td><td>Lossy</td></tr>
<tr><td><strong>Beeldkwaliteit</strong></td><td>Pixel‑perfect &mdash; geen artefacten</td><td>Kleine compressie‑artefacten mogelijk</td></tr>
<tr><td><strong>Bestandsgrootte</strong></td><td>Groter (meestal 2&ndash;10x versus JPG)</td><td>Kleiner</td></tr>
<tr><td><strong>Transparantie</strong></td><td>Ondersteund (alpha‑kanaal)</td><td>Niet ondersteund</td></tr>
<tr><td><strong>Ideaal voor</strong></td><td>Diagnostische beoordeling, archivering, onderzoek, overlays</td><td>Web‑deling, thumbnails, previews</td></tr>
<tr><td><strong>Regelgeving</strong></td><td>Voorkeur voor kwaliteitskritieke workflows</td><td>Acceptabel voor niet‑diagnostisch gebruik</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET gebruikt dezelfde renderpipeline voor beide uitvoerformaten. De <code>IRawImage</code>-pixelgegevens zijn identiek, ongeacht het doel­formaat &mdash; de formatkeuze beïnvloedt alleen de uiteindelijke opslagnorm die door uw beeldbibliotheek wordt uitgevoerd.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leer‑bronnen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Broncode" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑referenties" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Productondersteuning" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis ondersteuning" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betaalde ondersteuning" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Waarom Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Klantenlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
