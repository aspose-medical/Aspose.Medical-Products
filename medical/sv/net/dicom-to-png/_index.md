---
title: Konvertera DICOM till PNG i C# .NET | Aspose.Medical
weight: 11000
description: Rendera DICOM-bilder till pixeldata i C# .NET med förlustfri kvalitet, fönster/nivå-kontroll och stöd för flerbildssekvenser. Använd hela DICOM LUT-pipelinen med Aspose.Medical API och spara till PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konvertera DICOM till PNG i .NET C#" h2="Rendera DICOM-bilder till rå pixeldata med full stöd för gråskalepipeline. Bevara bildkvaliteten med fönster/nivå-kontroll, overlay-rendering och flerbildsbehandling. Spara till förlustfri PNG med valfritt .NET-bildbibliotek." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM till PNG-konvertering">}}

<p><strong>Aspose.Medical for .NET</strong> renderar DICOM-bilder genom den standardiserade DICOM-gråskalepipen (Modality LUT, VOI LUT, Presentation LUT, Output LUT) för att producera korrekt fönsterställd BGRA32-pixeldata. PNG är ett förlustfritt format, vilket gör det till det föredragna valet när bildkvaliteten måste bevaras utan komprimeringsartefakter &mdash; viktigt för diagnostisk granskning, arkivering och forskningsändamål.</p>

<p>Det renderade resultatet är ett <code>IRawImage</code> &mdash; en BGRA32-pixelbuffert med <code>Width</code>, <code>Height</code> och en <code>Pixels</code>-array. Du kan spara dessa pixeldata till PNG med valfritt .NET-bildbibliotek såsom <code>System.Drawing</code>, <code>SkiaSharp</code> eller <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendera DICOM till pixeldata i C#">}}

<p>Läs in en DICOM-fil, rendera den önskade ramen och få åtkomst till pixeldata:</p>

<div class="codeblock" id="code">
 <h3>Rendera DICOM-bild - C#</h3>
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

<p>Metoden <code>RenderImage</code> tillämpar automatiskt fönster/nivå-värden och LUT-parametrar som lagras i DICOM-datasetet.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Spara renderad bild som PNG">}}

<p><code>IRawImage</code> ger rå BGRA32-pixeldata som kan sparas till förlustfri PNG med valfritt .NET-bildbibliotek. Här är ett exempel med <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Spara renderad DICOM-ram som PNG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Anpassat fönster/nivå">}}

<p>Styr vilket intervall av pixelvärden som renderas genom att sätta Window Center och Window Width. Olika fönsterinställningar visar olika anatomiska strukturer från samma DICOM-data:</p>

<div class="codeblock" id="code">
 <h3>Rendera med anpassat fönster/nivå - C#</h3>
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

<p>Klassen <code>GrayscaleRenderOptions</code> exponerar även egenskaperna <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> och <code>BitDepth</code> för full kontroll över renderingspipen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendering av flerbilds-DICOM">}}

<p>DICOM-filer från CT, MRI och ultraljud innehåller ofta flera ramar (skivor). Rendera alla ramar till pixeldata:</p>

<div class="codeblock" id="code">
 <h3>Rendera alla ramar från flerbilds-DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Overlay‑kontroll">}}

<p>DICOM-bilder kan innehålla overlay-plantor med anteckningar, mätningar eller intresseområden. Styr overlay‑synlighet och färg vid rendering:</p>

<div class="codeblock" id="code">
 <h3>Rendera med och utan overlay - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG för medicinska bilder">}}

<p>Valet mellan PNG och JPG beror på användningsfallet:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Egenskap</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Kompression</strong></td><td>Förlustfri</td><td>Med förlust</td></tr>
<tr><td><strong>Bildkvalitet</strong></td><td>Pixelperfekt &mdash; inga artefakter</td><td>Mindre komprimeringsartefakter möjliga</td></tr>
<tr><td><strong>Filstorlek</strong></td><td>Större (vanligtvis 2&ndash;10× vs JPG)</td><td>Mindre</td></tr>
<tr><td><strong>Transparens</strong></td><td>Stöds (alfa‑kanal)</td><td>Stöds ej</td></tr>
<tr><td><strong>Bäst för</strong></td><td>Diagnostisk granskning, arkivering, forskning, overlay</td><td>Webbdelning, miniatyrbilder, förhandsgranskningar</td></tr>
<tr><td><strong>Regulatorisk</strong></td><td>Föredras för kvalitetkritiska arbetsflöden</td><td>Acceptabelt för icke-diagnostisk användning</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET använder samma renderingspipeline för båda målformaten. <code>IRawImage</code>-pixeldata är identiska oavsett målformat &mdash; val av format påverkar endast det sista sparsteget som utförs av ditt bildbibliotek.</p>

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
