---
title: DICOM nach PNG konvertieren in C# .NET | Aspose.Medical
weight: 11000
description: DICOM‑Bilder in Pixeldaten rendern in C# .NET mit verlustfreier Qualität, Fenster/Level‑Steuerung und Multi‑Frame‑Unterstützung. Verwenden Sie die vollständige DICOM‑LUT‑Pipeline mit der Aspose.Medical‑API und speichern Sie als PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM nach PNG konvertieren in .NET C#" h2="DICOM‑Bilder in Rohpixeldaten rendern mit voller Graustufen‑Pipeline‑Unterstützung. Bildqualität mit Fenster/Level‑Steuerung, Overlay‑Rendering und Multi‑Frame‑Verarbeitung erhalten. Als verlustfreies PNG mit beliebiger .NET‑Bildbibliothek speichern." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM‑zu‑PNG‑Konvertierung">}}

<p><strong>Aspose.Medical for .NET</strong> rendert DICOM‑Bilder durch die standardmäßige DICOM‑Graustufen‑Pipeline (Modality LUT, VOI LUT, Presentation LUT, Output LUT), um korrekt gefensterte BGRA32‑Pixeldaten zu erzeugen. PNG ist ein verlustfreies Format und damit die bevorzugte Wahl, wenn die Bildqualität ohne Kompressionsartefakte erhalten bleiben muss &mdash; unerlässlich für die diagnostische Ansicht, Archivierung und Forschungsanwendungen.</p>

<p>Die gerenderte Ausgabe ist ein <code>IRawImage</code> &mdash; ein BGRA32‑Pixel‑Puffer mit <code>Width</code>, <code>Height</code> und einem <code>Pixels</code>-Array. Sie können diese Pixeldaten mit beliebiger .NET‑Bildbibliothek, z. B. <code>System.Drawing</code>, <code>SkiaSharp</code> oder <code>ImageSharp</code>, als PNG speichern.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM in Pixeldaten rendern in C#">}}

<p>Laden Sie eine DICOM‑Datei, rendern Sie das gewünschte Bild und greifen Sie auf die Pixeldaten zu:</p>

<div class="codeblock" id="code">
 <h3>DICOM‑Bild rendern - C#</h3>
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

<p>Die Methode <code>RenderImage</code> wendet automatisch Fenster/Level‑Werte und LUT‑Parameter an, die im DICOM‑Datensatz gespeichert sind.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gerendertes Bild als PNG speichern">}}

<p>Das <code>IRawImage</code> liefert rohe BGRA32‑Pixeldaten, die mit beliebiger .NET‑Bildbibliothek verlustfrei als PNG gespeichert werden können. Hier ein Beispiel mit <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Gerenderten DICOM‑Frame als PNG speichern - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Benutzerdefiniertes Fenster/Level">}}

<p>Steuern Sie den zu rendernden Wertebereich, indem Sie Window Center und Window Width festlegen. Unterschiedliche Fenstereinstellungen zeigen verschiedene anatomische Strukturen aus denselben DICOM‑Daten:</p>

<div class="codeblock" id="code">
 <h3>Rendern mit benutzerdefiniertem Fenster/Level - C#</h3>
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

<p>Die Klasse <code>GrayscaleRenderOptions</code> stellt außerdem die Eigenschaften <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> und <code>BitDepth</code> zur Verfügung, um die Rendering‑Pipeline vollständig zu steuern.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Multi‑Frame‑DICOM‑Rendering">}}

<p>DICOM‑Dateien von CT, MRT und Ultraschall enthalten häufig mehrere Frames (Scheiben). Rendern Sie alle Frames zu Pixeldaten:</p>

<div class="codeblock" id="code">
 <h3>Alle Frames aus Multi‑Frame‑DICOM rendern - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Overlay‑Steuerung">}}

<p>DICOM‑Bilder können Overlay‑Ebenen mit Anmerkungen, Messungen oder Interessensgebieten enthalten. Steuern Sie die Sichtbarkeit und Farbe von Overlays beim Rendern:</p>

<div class="codeblock" id="code">
 <h3>Rendern mit und ohne Overlays - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG für medizinische Bilder">}}

<p>Die Wahl zwischen PNG und JPG hängt vom Anwendungsfall ab:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Eigenschaft</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Kompression</strong></td><td>Verlustfrei</td><td>Verlustbehaftet</td></tr>
<tr><td><strong>Bildqualität</strong></td><td>Pixelgenau &mdash; keine Artefakte</td><td>Kleinere Kompressionsartefakte möglich</td></tr>
<tr><td><strong>Dateigröße</strong></td><td>Größer (typischerweise 2&ndash;10x gegenüber JPG)</td><td>Kleiner</td></tr>
<tr><td><strong>Transparenz</strong></td><td>Unterstützt (Alpha‑Kanal)</td><td>Nicht unterstützt</td></tr>
<tr><td><strong>Am besten geeignet für</strong></td><td>Diagnostische Prüfung, Archivierung, Forschung, Overlays</td><td>Web‑Freigabe, Thumbnails, Vorschaubilder</td></tr>
<tr><td><strong>Regulatorisch</strong></td><td>Bevorzugt für qualitätskritische Arbeitsabläufe</td><td>Akzeptabel für nicht diagnostische Anwendungen</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET verwendet dieselbe Rendering‑Pipeline für beide Ausgabemedien. Die <code>IRawImage</code>-Pixeldaten sind unabhängig vom Zielformat identisch — die Formatwahl beeinflusst nur den abschließenden Speichervorgang, der von Ihrer Bildbibliothek durchgeführt wird.</p>

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
