---
title: Konvertera DICOM till JPG i C# .NET | Aspose.Medical
weight: 7000
description: Rendera DICOM-bilder till pixeldata i C# .NET med window/level-kontroll, overlay-rendering och stöd för flera ramar. Använd hela DICOM LUT-pipelinen med Aspose.Medical API och spara till JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konvertera DICOM till JPG i .NET C#" h2="Rendera DICOM-bilder till rå pixeldata med full stöd för gråskala-pipelinen inklusive window/level, modality LUT, VOI LUT och overlay-rendering. Spara till JPG med valfritt .NET-bildbibliotek." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM-bildrenderingspipeline">}}

<p><strong>Aspose.Medical for .NET</strong> renderar DICOM‑bilder genom den standardiserade DICOM‑gråskalapipelinen som definieras i DICOM‑specifikationen. Renderingsprocessen applicerar en kedja av Look‑Up‑Tables (LUTs) för att omvandla rå lagrade pixelvärden till visningsbara bilder:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; konverterar lagrade pixelvärden till tillverkarsoberoende värden med hjälp av Rescale Slope/Intercept eller en LUT‑sekvens.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; tillämpar window/level (Window Center och Window Width) för att välja intervallet av värden som ska visas, med stöd för Linear, Linear Exact och Sigmoid‑transformeringar.</li>
<li><strong>Presentation LUT</strong> &mdash; mappar de slutgiltiga värdena till visningsbara P‑Values, inklusive stöd för INVERSE (bildinversion).</li>
<li><strong>Output LUT</strong> &mdash; konverterar gråskalavärden till RGB för visning, med valfri färgsättning med hjälp av anpassade färgkartor eller Palette Color‑tabeller.</li>
</ol>

<p>Den renderade utdata är en <code>IRawImage</code> &mdash; en BGRA32‑pixelbuffert (8‑bit per kanal) med <code>Width</code>, <code>Height</code> och en <code>Pixels</code>-array. Du kan sedan spara dessa pixeldata till JPG med valfritt .NET‑bildbibliotek såsom <code>System.Drawing</code>, <code>SkiaSharp</code> eller <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendera DICOM till pixeldata i C#">}}

<p>Läs in en DICOM‑fil och rendera en bildruta till ett <code>IRawImage</code> som innehåller BGRA32‑pixeldata:</p>

<div class="codeblock" id="code">
 <h3>Rendera DICOM‑bild - C#</h3>
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

<p>Metoden <code>RenderImage</code> applicerar automatiskt hela gråskalapipelinen med hjälp av window/level‑värdena och LUT‑parametrarna som lagras i DICOM‑filen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Spara renderad bild som JPG">}}

<p><code>IRawImage</code> tillhandahåller rå BGRA32‑pixeldata som kan sparas till JPG med valfritt .NET‑bildbibliotek. Här är ett exempel som använder <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Spara renderad DICOM‑ram som JPG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Anpassad window/level (windowing)">}}

<p>Window/level (även kallad windowing eller W/L) är den viktigaste parametern för DICOM‑bildrendering. Den styr vilket intervall av pixelvärden som mappas till det synliga gråskalaintervallet. <strong>Window Center</strong> definierar mittpunkten och <strong>Window Width</strong> definierar värdeintervallet som visas:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; hög kontrast, färre gråtoner visas (användbart för mjuk vävnad).</li>
<li><strong>Wide window</strong> &mdash; lägre kontrast, fler gråtoner visas (användbart för ben eller lung).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Rendera med anpassad window/level - C#</h3>
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

<p>Vanliga CT‑window‑presets:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Vävnadstyp</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Mjuk vävnad</td><td>40</td><td>400</td></tr>
<tr><td>Lunga</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Ben</td><td>400</td><td>1800</td></tr>
<tr><td>Hjärna</td><td>40</td><td>80</td></tr>
<tr><td>Lever</td><td>60</td><td>150</td></tr>
<tr><td>Mediastinum</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Inställningar för gråskalrendering">}}

<p>Klassen <code>GrayscaleRenderOptions</code> ger full kontroll över gråskalrenderingspipeline:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Egenskap</th>
<th>Beskrivning</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Centrum för det visade värdeintervallet (horisontell mittpunkt för VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Bredden på det visade värdeintervallet (kontrollerar kontrast)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Modality LUT‑rescale‑slope — multiplicerat med lagrade pixelvärden</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Modality LUT‑rescale‑intercept — adderas efter slope‑multiplikation</td></tr>
<tr><td><code>Invert</code></td><td>Inverterar gråskalabilden (tillämpa INVERSE Presentation LUT‑form)</td></tr>
<tr><td><code>BitDepth</code></td><td>Bitdjupsinformation: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Inverterad gråskalrendering - C#</h3>
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

<p>Använd <code>RenderOptionsFactory.Create(pixelData)</code> för att automatiskt fylla renderingsalternativ från DICOM‑datasetets pixeldata‑attribut, inklusive bitdjup, rescale‑parametrar, window/level och inverteringsinställningar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Overlay‑rendering">}}

<p>DICOM‑bilder kan innehålla overlay‑plan — grafik eller textannotationer inbrända i bilden. Klassen <code>RenderOptions</code> styr om overlay‑lager renderas och i vilken färg de visas:</p>

<div class="codeblock" id="code">
 <h3>Rendera med overlay‑kontroll - C#</h3>
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

<p>DICOM stödjer två overlay‑typer: <strong>Graphics</strong>-overlay (annotationer, mätningar) och <strong>Region of Interest</strong> (ROI)-overlay. Båda typerna styrs av inställningen <code>ShowOverlays</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderning av multi‑frame DICOM">}}

<p>Många DICOM‑filer från CT, MR och ultraljud innehåller flera ramar (skivor). Du kan kontrollera antalet ramar och rendera varje enskilt:</p>

<div class="codeblock" id="code">
 <h3>Rendera alla ramar från multi‑frame DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="VOI LUT‑transformationstyper">}}

<p>DICOM‑standarden definierar flera VOI LUT‑transformationsfunktioner som styr hur window/level‑mappning tillämpas. Aspose.Medical för .NET stödjer alla standardtyper:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Typ</th>
<th>Beskrivning</th>
<th>Användningsfall</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Standard linjär mappning med offset från centrum</td><td>Mest vanlig, används för allmän CT/MR‑avbildning</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Linjär mappning utan offset från centrum</td><td>När exakt pixelvärdesmappning krävs</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>S‑kurva‑transformation för mjukare kontrastövergångar</td><td>Visualisering av mjuk vävnad, mammografi</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>Anpassad LUT definierad som en lookup‑tabell i DICOM‑datasetet</td><td>Leverantörsspecifika eller modalitiespecifika displaykurvor</td></tr>
</tbody>
</table>

<p>Renderingsmotorn väljer automatiskt lämplig VOI LUT‑transformation baserat på DICOM‑datasetets attribut. Vid användning av anpassade <code>GrayscaleRenderOptions</code> tillämpas Linear‑transformationen som standard.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stödda modaliteter">}}

<p>Aspose.Medical för .NET stödjer rendering av DICOM‑bilder från alla vanliga medicinska bildmodaliteter:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modalitet</th>
<th>Beskrivning</th>
<th>Typiskt format</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Computed Tomography</td><td>Grayscale, multi-frame, 12–16 bit</td></tr>
<tr><td><strong>MR</strong></td><td>Magnetic Resonance</td><td>Grayscale, multi-frame, 12–16 bit</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Computed / Digital Radiography</td><td>Grayscale, single frame, 10–16 bit</td></tr>
<tr><td><strong>US</strong></td><td>Ultrasound</td><td>Grayscale or RGB, multi-frame</td></tr>
<tr><td><strong>MG</strong></td><td>Mammography</td><td>Grayscale, high resolution, 12–16 bit</td></tr>
<tr><td><strong>XA</strong></td><td>X-Ray Angiography</td><td>Grayscale, multi-frame</td></tr>
<tr><td><strong>NM</strong></td><td>Nuclear Medicine</td><td>Grayscale, multi-frame</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Positron Emission Tomography)</td><td>Grayscale, multi-frame</td></tr>
</tbody>
</table>

<p>Både gråskala (MONOCHROME1/MONOCHROME2) och färg (RGB, YBR_FULL, PALETTE COLOR) fotometriska tolkningar stödjs. Renderingsmotorn hanterar automatiskt korrekt färgrymdskonvertering.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Källkod" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑referenser" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktsupport" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betald support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blogg" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Varför Aspose.Medical för .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundlista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Framgångshistorier" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
