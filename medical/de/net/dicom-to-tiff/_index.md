---
title: DICOM in TIFF konvertieren in C# .NET | Aspose.Medical
weight: 12000
description: DICOM-Bilder in Pixeldaten rendern in C# .NET und als TIFF mit Multi‑Page‑Unterstützung für Multi‑Frame‑DICOM‑Dateien speichern. Verwenden Sie die vollständige DICOM‑LUT‑Pipeline mit der Aspose.Medical‑API und speichern Sie als TIFF mit einer beliebigen .NET‑Imaging‑Bibliothek.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM in TIFF konvertieren in .NET C#" h2="DICOM-Bilder in Roh-Pixeldaten rendern mit vollumfassender Graustufen‑Pipeline‑Unterstützung. Bildqualität mit Fenster-/Level‑Steuerung, Overlay‑Rendering und Multi‑Frame‑Verarbeitung erhalten. Als TIFF speichern &mdash; einschließlich Multi‑Page‑TIFF &mdash; mit einer beliebigen .NET‑Imaging‑Bibliothek." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM zu TIFF Konvertierung">}}

<p><strong>Aspose.Medical for .NET</strong> rendert DICOM‑Bilder über die standardmäßige DICOM‑Graustufen‑Pipeline (Modality LUT, VOI LUT, Presentation LUT, Output LUT), um korrekt gefensterte BGRA32‑Pixeldaten zu erzeugen. TIFF ist ein vielseitiges Format, das in der medizinischen Bildgebung, wissenschaftlichen Forschung und im Verlagswesen weit verbreitet ist, weil es verlustfreie Kompression, hohe Bit‑Tiefen unterstützt und &mdash; am wichtigsten &mdash; <strong>Multi‑Page‑Dokumente</strong>. Damit ist TIFF die natürliche Wahl für die Konvertierung von Multi‑Frame‑DICOM‑Dateien (CT, MRI, Ultraschall) in eine einzelne Ausgabedatei, die alle Schichten erhält.</p>

<p>Der gerenderte Output ist ein <code>IRawImage</code> &mdash; ein BGRA32‑Pixelpuffer mit <code>Width</code>, <code>Height</code> und einem <code>Pixels</code>-Array. Sie können diese Pixeldaten mit einer beliebigen .NET‑Imaging‑Bibliothek wie <code>System.Drawing</code>, <code>SkiaSharp</code> oder <code>ImageSharp</code> als TIFF speichern.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM in Pixeldaten rendern in C#">}}

<p>Laden Sie eine DICOM‑Datei, rendern Sie den gewünschten Frame und greifen Sie auf die Pixeldaten zu:</p>

<div class="codeblock" id="code">
 <h3>Render DICOM-Bild - C#</h3>
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

<p>Die Methode <code>RenderImage</code> wendet automatisch die im DICOM‑Datensatz gespeicherten Fenster‑/Level‑Werte und LUT‑Parameter an.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gerendertes Bild als TIFF speichern">}}

<p>Das <code>IRawImage</code> liefert rohe BGRA32‑Pixeldaten, die mit einer beliebigen .NET‑Imaging‑Bibliothek als TIFF gespeichert werden können. Hier ein Beispiel mit <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Gerenderten DICOM-Frame als TIFF speichern - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Multi‑Page‑TIFF von Multi‑Frame‑DICOM">}}

<p>Multi‑Frame‑DICOM‑Dateien (z. b. CT‑ oder MRI‑Serien) enthalten mehrere Bildschichten in einer einzigen Datei. Die Multi‑Page‑Fähigkeit von TIFF ermöglicht es, alle Frames in einer Ausgabedatei zu speichern und die komplette Untersuchung in einem universell lesbaren Format zu erhalten. Verwenden Sie den <code>System.Drawing</code> TIFF‑Encoder mit Multi‑Frame‑Parametern:</p>

<div class="codeblock" id="code">
 <h3>Multi‑Frame‑DICOM in Multi‑Page‑TIFF konvertieren - C#</h3>
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

<p>Sie können auch jeden Frame als separate TIFF‑Datei speichern, wenn einzelne Schichten benötigt werden:</p>

<div class="codeblock" id="code">
 <h3>Jeden Frame in eine separate TIFF konvertieren - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Benutzerdefiniertes Fenster/Level">}}

<p>Steuern Sie, welcher Bereich von Pixelwerten gerendert wird, indem Sie das Window Center und Window Width einstellen. Unterschiedliche Fenstereinstellungen zeigen unterschiedliche anatomische Strukturen aus denselben DICOM‑Daten:</p>

<div class="codeblock" id="code">
 <h3>Mit benutzerdefiniertem Fenster/Level rendern - C#</h3>
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

<p>Die Klasse <code>GrayscaleRenderOptions</code> stellt zudem die Eigenschaften <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> und <code>BitDepth</code> bereit, um die Rendering‑Pipeline vollständig zu steuern.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Multi‑Frame‑DICOM‑Rendering">}}

<p>DICOM‑Dateien von CT, MRI und Ultraschall enthalten häufig mehrere Frames (Schichten). Rendern Sie alle Frames zu Pixeldaten:</p>

<div class="codeblock" id="code">
 <h3>Alle Frames von Multi‑Frame‑DICOM rendern - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Overlay‑Steuerung">}}

<p>DICOM‑Bilder können Overlay‑Ebenen mit Anmerkungen, Messungen oder Interessensbereichen enthalten. Steuern Sie die Sichtbarkeit und Farbe des Overlays beim Rendern:</p>

<div class="codeblock" id="code">
 <h3>Mit und ohne Overlays rendern - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG für medizinische Bilder">}}

<p>Jedes Ausgabeformat dient unterschiedlichen Anwendungsfällen:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Merkmal</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Kompression</strong></td><td>Lossless (LZW, ZIP) or uncompressed</td><td>Lossless</td><td>Lossy</td></tr>
<tr><td><strong>Multi-Page</strong></td><td>Unterstützt &mdash; alle Frames in einer Datei</td><td>Nicht unterstützt</td><td>Nicht unterstützt</td></tr>
<tr><td><strong>Bildqualität</strong></td><td>Pixel-perfect</td><td>Pixel-perfect</td><td>Kompressionsartefakte möglich</td></tr>
<tr><td><strong>Dateigröße</strong></td><td>Größte</td><td>Mittel</td><td>Kleinste</td></tr>
<tr><td><strong>Transparenz</strong></td><td>Unterstützt</td><td>Unterstützt</td><td>Nicht unterstützt</td></tr>
<tr><td><strong>Am besten geeignet für</strong></td><td>Archivierung, Multi‑Frame‑Serien, Druck, Forschung</td><td>Diagnostische Prüfung, Web mit Qualität</td><td>Web‑Freigabe, Thumbnails</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET verwendet für alle Ausgabeziele dieselbe Rendering‑Pipeline. Die <code>IRawImage</code>-Pixeldaten sind unabhängig vom Zielformat identisch — die Wahl des Formats beeinflusst nur den abschließenden Speichervorgang Ihrer Imaging‑Bibliothek. TIFF ist das bevorzugte Format, wenn Sie eine komplette Multi‑Frame‑DICOM‑Studie in einer einzigen Datei erhalten müssen oder wenn die Ausgabe für Archivierung, Druck oder weitere Bildverarbeitung vorgesehen ist.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Quellcode" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑Referenzen" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktsupport" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Kostenloser Support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Kostenpflichtiger Support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Warum Aspose.Medical für .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundenliste" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Erfolgsgeschichten" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
