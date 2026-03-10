---
title: Converteer DICOM naar JPG in C# .NET | Aspose.Medical
weight: 7000
description: Render DICOM‑afbeeldingen naar pixelgegevens in C# .NET met window/level‑besturing, overlay‑rendering en multi‑frame‑ondersteuning. Gebruik de volledige DICOM LUT‑pipeline met de Aspose.Medical‑API en sla op als JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converteer DICOM naar JPG in .NET C#" h2="Render DICOM‑afbeeldingen naar ruwe pixelgegevens met volledige grayscale‑pipelineondersteuning, inclusief window/level, modality LUT, VOI LUT en overlay‑rendering. Sla op als JPG met behulp van elke .NET‑afbeeldingsbibliotheek." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM‑afbeeldingsrenderingspipeline">}}

<p><strong>Aspose.Medical for .NET</strong> rendert DICOM‑afbeeldingen via de standaard DICOM grayscale‑pipeline die in de DICOM‑specificatie is gedefinieerd. Het renderproces past een keten van Look‑Up Tables (LUT’s) toe om ruwe opgeslagen pixelwaarden om te zetten in weergeefbare afbeeldingen:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; converteert opgeslagen pixelwaarden naar fabrikant-onafhankelijke waarden met behulp van Rescale Slope/Intercept of een LUT‑Sequence.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; past window/level (Window Center en Window Width) toe om het bereik van waarden te selecteren dat wordt weergegeven, met ondersteuning voor Linear, Linear Exact en Sigmoid‑transformaties.</li>
<li><strong>Presentation LUT</strong> &mdash; mappt de uiteindelijke waarden naar weergeefbare P‑Values, inclusief ondersteuning voor INVERSE (beeldinversie).</li>
<li><strong>Output LUT</strong> &mdash; converteert grayscale‑waarden naar RGB voor weergave, met optionele kleuring met aangepaste colormaps of Palette Color‑tabellen.</li>
</ol>

<p>De gerenderde output is een <code>IRawImage</code> &mdash; een BGRA32-pixelbuffer (8‑bit per kanaal) met <code>Width</code>, <code>Height</code> en een <code>Pixels</code>-array. U kunt deze pixelgegevens vervolgens opslaan als JPG met behulp van elke .NET‑afbeeldingsbibliotheek, zoals <code>System.Drawing</code>, <code>SkiaSharp</code> of <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render DICOM naar pixelgegevens in C#">}}

<p>Laad een DICOM‑bestand en render een frame naar een <code>IRawImage</code> dat BGRA32-pixelgegevens bevat:</p>

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

<p>De <code>RenderImage</code>-methode past automatisch de volledige grayscale‑pipeline toe met de window/level‑waarden en LUT‑parameters die in het DICOM‑bestand zijn opgeslagen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sla gerenderde afbeelding op als JPG">}}

<p>De <code>IRawImage</code> levert ruwe BGRA32-pixelgegevens die kunnen worden opgeslagen als JPG met behulp van elke .NET‑afbeeldingsbibliotheek. Hieronder een voorbeeld met <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Sla gerenderde DICOM‑frame op als JPG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Aangepast window/level (windowing)">}}

<p>Window/level (ook wel windowing of W/L genoemd) is de belangrijkste parameter voor DICOM‑afbeeldingsrendering. Het bepaalt welk bereik van pixelwaarden wordt gemapt naar het zichtbare grayscale‑bereik. De <strong>Window Center</strong> definieert het middelpunt en de <strong>Window Width</strong> definieert het bereik van de weergegeven waarden:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; hoog contrast, minder grijstinten weergegeven (handig voor zacht weefsel).</li>
<li><strong>Wide window</strong> &mdash; lager contrast, meer grijstinten weergegeven (handig voor bot of long).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Render met aangepast window/level - C#</h3>
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

<p>Veelvoorkomende CT‑window‑presets:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Weefseltype</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Zacht weefsel</td><td>40</td><td>400</td></tr>
<tr><td>Long</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Bot</td><td>400</td><td>1800</td></tr>
<tr><td>Hersenen</td><td>40</td><td>80</td></tr>
<tr><td>Lever</td><td>60</td><td>150</td></tr>
<tr><td>Mediastinum</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Grayscale‑renderopties">}}

<p>De klasse <code>GrayscaleRenderOptions</code> biedt volledige controle over de grayscale‑rendering‑pipeline:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Eigenschap</th>
<th>Beschrijving</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Middelpunt van het weergegeven waardenbereik (horizontaal middelpunt van de VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Breedte van het weergegeven waardenbereik (reguleert contrast)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Modality LUT rescale‑slope &mdash; vermenigvuldigd met opgeslagen pixelwaarden</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Modality LUT rescale‑intercept &mdash; toegevoegd na vermenigvuldiging met de slope</td></tr>
<tr><td><code>Invert</code></td><td>Inverteert het grayscale‑beeld (past INVERSE Presentation LUT‑vorm toe)</td></tr>
<tr><td><code>BitDepth</code></td><td>Bitdiepte‑informatie: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Invers grayscale renderen - C#</h3>
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

<p>Gebruik <code>RenderOptionsFactory.Create(pixelData)</code> om renderopties automatisch te vullen vanuit de pixeldata‑attributen van de DICOM‑dataset, inclusief bitdiepte, rescale‑parameters, window/level en inversie‑instellingen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Overlay‑rendering">}}

<p>DICOM‑afbeeldingen kunnen overlay‑vlakken bevatten &mdash; grafische of tekstannotaties die in het beeld zijn ingebrand. De klasse <code>RenderOptions</code> bepaalt of overlays worden gerenderd en in welke kleur ze verschijnen:</p>

<div class="codeblock" id="code">
 <h3>Render met overlay‑controle - C#</h3>
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

<p>DICOM ondersteunt twee overlay‑types: <strong>Graphics</strong>-overlays (annotaties, metingen) en <strong>Region of Interest</strong> (ROI)-overlays. Beide types worden geregeld door de <code>ShowOverlays</code>-instelling.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Multi‑frame DICOM‑rendering">}}

<p>Veel DICOM‑bestanden van CT, MRI en echografie bevatten meerdere frames (slice). U kunt het aantal frames controleren en elk frame afzonderlijk renderen:</p>

<div class="codeblock" id="code">
 <h3>Render alle frames van multi‑frame DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="VOI LUT‑transformatietypes">}}

<p>De DICOM‑standaard definieert verschillende VOI LUT‑transformatiefuncties die bepalen hoe de window/level‑mapping wordt toegepast. Aspose.Medical for .NET ondersteunt alle standaardtypes:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Type</th>
<th>Beschrijving</th>
<th>Gebruikssituatie</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Standaard lineaire mapping met offset vanaf het centrum</td><td>Meest gangbaar, gebruikt voor algemene CT/MR‑beeldvorming</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Lineaire mapping zonder offset vanaf het centrum</td><td>Wanneer een nauwkeurige pixelwaarde‑mapping vereist is</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>S-curve‑transformatie voor soepelere contrastovergangen</td><td>Zacht weefsel visualisatie, mammografie</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>Aangepaste LUT gedefinieerd als een lookup‑table in de DICOM‑dataset</td><td>Leverancier‑specifieke of modality‑specifieke weergavecurves</td></tr>
</tbody>
</table>

<p>De renderengine selecteert automatisch de juiste VOI LUT‑transformatie op basis van de DICOM‑dataset‑attributen. Bij gebruik van aangepaste <code>GrayscaleRenderOptions</code> wordt standaard de Linear‑transformatie toegepast.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ondersteunde modaliteiten">}}

<p>Aspose.Medical for .NET ondersteunt het renderen van DICOM‑afbeeldingen uit alle gangbare medische beeldvormingsmodaliteiten:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modaliteit</th>
<th>Beschrijving</th>
<th>Typisch formaat</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Computertomografie</td><td>Grayscale, multi-frame, 12&ndash;16 bit</td></tr>
<tr><td><strong>MR</strong></td><td>Magnetische resonantie</td><td>Grayscale, multi-frame, 12&ndash;16 bit</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Computertomografie / Digitale radiografie</td><td>Grayscale, single frame, 10&ndash;16 bit</td></tr>
<tr><td><strong>US</strong></td><td>Echografie</td><td>Grayscale of RGB, multi-frame</td></tr>
<tr><td><strong>MG</strong></td><td>Mammografie</td><td>Grayscale, hoge resolutie, 12&ndash;16 bit</td></tr>
<tr><td><strong>XA</strong></td><td>Röntgenangiografie</td><td>Grayscale, multi-frame</td></tr>
<tr><td><strong>NM</strong></td><td>Nucleaire geneeskunde</td><td>Grayscale, multi-frame</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Positronemissietomografie)</td><td>Grayscale, multi-frame</td></tr>
</tbody>
</table>

<p>Zowel grayscale (MONOCHROME1/MONOCHROME2) als kleur (RGB, YBR_FULL, PALETTE COLOR) fotometrische interpretaties worden ondersteund. De renderengine verwerkt automatisch de juiste kleurenspace‑conversie.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leermaterialen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Broncode" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑referenties" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Productondersteuning" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis ondersteuning" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betaalde ondersteuning" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Waarom Aspose.Medical voor .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Klantlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
