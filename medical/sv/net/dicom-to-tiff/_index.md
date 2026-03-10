---
title: Konvertera DICOM till TIFF i C# .NET | Aspose.Medical
weight: 12000
description: Rendera DICOM‑bilder till pixeldatat i C# .NET och spara till TIFF med stöd för flera sidor för multi‑frame DICOM‑filer. Använd den fullständiga DICOM‑LUT‑pipelineen med Aspose.Medical API och spara till TIFF med valfritt .NET‑bildbibliotek.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konvertera DICOM till TIFF i .NET C#" h2="Rendera DICOM‑bilder till råa pixeldata med fullt stöd för gråskalepipeline. Bevara bildkvaliteten med fönster/nivå‑kontroll, overlay‑rendering och multi‑frame‑behandling. Spara till TIFF &mdash; inklusive multi‑page TIFF &mdash; med valfritt .NET‑bildbibliotek." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Konvertering av DICOM till TIFF">}}

<p><strong>Aspose.Medical for .NET</strong> renderar DICOM‑bilder genom den standardiserade DICOM‑gråskalepipelinen (Modality LUT, VOI LUT, Presentation LUT, Output LUT) för att producera korrekt fönsterställd BGRA32‑pixeldata. TIFF är ett mångsidigt format som är allmänt använt inom medicinsk bildbehandling, vetenskaplig forskning och publicering eftersom det stödjer förlustfri komprimering, hög bitdjup och &mdash; viktigast av allt &mdash; <strong>multisidiga dokument</strong>. Detta gör TIFF till det naturliga valet för att konvertera multi‑frame DICOM‑filer (CT, MRI, ultraljud) till en enda utdatafil som bevarar alla skivor.</p>

<p>Det renderade resultatet är ett <code>IRawImage</code> &mdash; en BGRA32‑pixelbuffert med <code>Width</code>, <code>Height</code> och en <code>Pixels</code>-array. Du kan spara denna pixeldata till TIFF med valfritt .NET‑bildbibliotek såsom <code>System.Drawing</code>, <code>SkiaSharp</code> eller <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendera DICOM till pixeldata i C#">}}

<p>Läs in en DICOM‑fil, rendera den önskade ramen och få åtkomst till pixeldatan:</p>

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

<p>Metoden <code>RenderImage</code> tillämpar automatiskt fönster/nivå‑värden och LUT‑parametrar som lagras i DICOM‑datasetet.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Spara renderad bild som TIFF">}}

<p><code>IRawImage</code> tillhandahåller rå BGRA32‑pixeldata som kan sparas till TIFF med valfritt .NET‑bildbibliotek. Här är ett exempel med <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Spara renderad DICOM‑ram som TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Multi‑page TIFF från Multi‑frame DICOM">}}

<p>Multi‑frame DICOM‑filer (t.ex. CT‑ eller MRI‑serier) innehåller flera bildskivor i en enda fil. TIFF:s multi‑page‑funktionalitet låter dig lagra alla ramar i en utdatafil, vilket bevarar hela studien i ett universellt läsbart format. Använd <code>System.Drawing</code> TIFF‑kodaren med multi‑frame‑parametrar:</p>

<div class="codeblock" id="code">
 <h3>Konvertera multi‑frame DICOM till multi‑page TIFF - C#</h3>
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

<p>Du kan också spara varje ram som en separat TIFF‑fil när enskilda skivor behövs:</p>

<div class="codeblock" id="code">
 <h3>Konvertera varje ram till en separat TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Anpassat fönster/nivå">}}

<p>Styr vilket intervall av pixelvärden som renderas genom att sätta Window Center och Window Width. Olika fönsterinställningar avslöjar olika anatomiska strukturer från samma DICOM‑data:</p>

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
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

<p>Klassen <code>GrayscaleRenderOptions</code> exponerar även egenskaperna <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> och <code>BitDepth</code> för fullständig kontroll över renderingspipen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderning av multi‑frame DICOM">}}

<p>DICOM‑filer från CT, MRI och ultraljud innehåller ofta flera ramar (skivor). Rendera alla ramar till pixeldata:</p>

<div class="codeblock" id="code">
 <h3>Rendera alla ramar från multi‑frame DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Overlay‑kontroll">}}

<p>DICOM‑bilder kan innehålla overlay‑plan med anteckningar, mätningar eller intresseområden. Styr overlay‑synlighet och färg vid rendering:</p>

<div class="codeblock" id="code">
 <h3>Rendera med och utan overlays - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG för medicinska bilder">}}

<p>Varje utdataformat tjänar olika användningsfall:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Egenskap</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Kompression</strong></td><td>Förlustfri (LZW, ZIP) eller okomprimerad</td><td>Förlustfri</td><td>Med förlust</td></tr>
<tr><td><strong>Multi-Page</strong></td><td>Stöds &mdash; alla ramar i en fil</td><td>Stöds ej</td><td>Stöds ej</td></tr>
<tr><td><strong>Bildkvalitet</strong></td><td>Pixelperfekt</td><td>Pixelperfekt</td><td>Kompressionsartefakter möjliga</td></tr>
<tr><td><strong>Filstorlek</strong></td><td>Störst</td><td>Mellan</td><td>Minst</td></tr>
<tr><td><strong>Transparens</strong></td><td>Stöds</td><td>Stöds</td><td>Stöds ej</td></tr>
<tr><td><strong>Bäst för</strong></td><td>Arkivering, multi‑frame‑serier, utskrift, forskning</td><td>Diagnostisk granskning, webb med kvalitet</td><td>Webbdelning, miniatyrbilder</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET använder samma renderingspipeline för alla utdata mål. <code>IRawImage</code>-pixeldatan är identisk oavsett målformat — formatvalet påverkar bara det sista sparsteget som utförs av ditt bildbibliotek. TIFF är det föredragna formatet när du behöver bevara en komplett multi‑frame DICOM‑studie i en enda fil eller när utdata är avsedd för arkivering, utskrift eller vidare bildbehandling.</p>

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
