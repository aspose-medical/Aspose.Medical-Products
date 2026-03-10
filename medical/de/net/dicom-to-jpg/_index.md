---
title: DICOM in JPG konvertieren in C# .NET | Aspose.Medical
weight: 7000
description: DICOM‑Bilder in Pixeldaten rendern in C# .NET mit Fenster/Level‑Steuerung, Overlay‑Rendering und Mehrfach‑Frame‑Unterstützung. Verwenden Sie die komplette DICOM‑LUT‑Pipeline mit der Aspose.Medical‑API und speichern Sie als JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM in JPG konvertieren in .NET C#" h2="DICOM‑Bilder in Roh‑Pixeldaten rendern mit voller Graustufen‑Pipeline‑Unterstützung, einschließlich Fenster/Level, Modality LUT, VOI LUT und Overlay‑Rendering. Als JPG speichern mit jeder .NET‑Bildbibliothek." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM‑Bildrendering‑Pipeline">}}

<p><strong>Aspose.Medical for .NET</strong> rendert DICOM‑Bilder über die standardisierte DICOM‑Graustufen‑Pipeline, die in der DICOM‑Spezifikation definiert ist. Der Rendering‑Process wendet eine Kette von Look‑Up‑Tables (LUTs) an, um rohe gespeicherte Pixelwerte in darstellbare Bilder zu transformieren:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; konvertiert gespeicherte Pixelwerte in herstellerunabhängige Werte mittels Rescale Slope/Intercept oder einer LUT‑Sequenz.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; wendet Fenster/Level (Window Center und Window Width) an, um den darzustellenden Wertebereich zu bestimmen, mit Unterstützung für Linear, Linear Exact und Sigmoid‑Transformationen.</li>
<li><strong>Presentation LUT</strong> &mdash; mappt die Endwerte zu darstellbaren P‑Values, einschließlich Unterstützung für INVERSE (Bildinversion).</li>
<li><strong>Output LUT</strong> &mdash; konvertiert Graustufenwerte zu RGB für die Anzeige, mit optionaler Kolorierung mittels benutzerdefinierter Farbkarten oder Palette‑Color‑Tabellen.</li>
</ol>

<p>Der gerenderte Output ist ein <code>IRawImage</code> &mdash; ein BGRA32‑Pixelpuffer (8‑Bit pro Kanal) mit <code>Width</code>, <code>Height</code> und einem <code>Pixels</code>-Array. Sie können diese Pixeldaten anschließend mit jeder .NET‑Bildbibliothek, z. B. <code>System.Drawing</code>, <code>SkiaSharp</code> oder <code>ImageSharp</code>, als JPG speichern.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM in Pixeldaten rendern in C#">}}

<p>Laden Sie eine DICOM‑Datei und rendern Sie ein Frame zu einem <code>IRawImage</code>, das BGRA32‑Pixeldaten enthält:</p>

<div class="codeblock" id="code">
 <h3>DICOM‑Bild rendern – C#</h3>
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

<p>Die Methode <code>RenderImage</code> wendet automatisch die komplette Graustufen‑Pipeline unter Verwendung der im DICOM‑File gespeicherten Fenster/Level‑Werte und LUT‑Parameter an.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gerendertes Bild als JPG speichern">}}

<p>Das <code>IRawImage</code> liefert rohe BGRA32‑Pixeldaten, die mit jeder .NET‑Bildbibliothek als JPG gespeichert werden können. Hier ein Beispiel mit <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Gerendertes DICOM‑Frame als JPG speichern – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Benutzerdefiniertes Fenster/Level (Windowing)">}}

<p>Window/Level (auch als Windowing oder W/L bezeichnet) ist der wichtigste Parameter für das Rendering von DICOM‑Bildern. Er steuert, welcher Bereich von Pixelwerten auf den sichtbaren Graustufenbereich abgebildet wird. Der <strong>Window Center</strong> definiert den Mittelpunkt und die <strong>Window Width</strong> definiert den Bereich der angezeigten Werte:</p>

<ul>
<li><strong>Schmales Fenster</strong> &mdash; hoher Kontrast, weniger Graustufen angezeigt (nützlich für Weichteile).</li>
<li><strong>Breites Fenster</strong> &mdash; geringerer Kontrast, mehr Graustufen angezeigt (nützlich für Knochen oder Lunge).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Rendern mit benutzerdefiniertem Fenster/Level – C#</h3>
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

<p>Übliche CT‑Fenster‑Voreinstellungen:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Gewebetyp</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Weichgewebe</td><td>40</td><td>400</td></tr>
<tr><td>Lunge</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Knochen</td><td>400</td><td>1800</td></tr>
<tr><td>Gehirn</td><td>40</td><td>80</td></tr>
<tr><td>Leber</td><td>60</td><td>150</td></tr>
<tr><td>Mediastinum</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Graustufen‑Renderoptionen">}}

<p>Die Klasse <code>GrayscaleRenderOptions</code> bietet vollständige Kontrolle über die Graustufen‑Rendering‑Pipeline:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Eigenschaft</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Mitte des angezeigten Wertebereichs (horizontaler Mittelpunkt des VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Breite des angezeigten Wertebereichs (steuert den Kontrast)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Modality LUT Reskalen‑Steigung &mdash; multipliziert mit gespeicherten Pixelwerten</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Modality LUT Reskalen‑Intercept &mdash; addiert nach der Steigungsmultiplikation</td></tr>
<tr><td><code>Invert</code></td><td>Invertiert das Graustufenbild (wendet INVERSE Presentation LUT-Form an)</td></tr>
<tr><td><code>BitDepth</code></td><td>Bit‑Tiefe-Informationen: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Invertiertes Graustufen‑Rendering – C#</h3>
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

<p>Verwenden Sie <code>RenderOptionsFactory.Create(pixelData)</code>, um die Renderoptionen automatisch aus den Pixeldaten‑Attributen des DICOM‑Datensatzes zu befüllen, einschließlich Bit‑Tiefe, Reskalen‑Parameter, Fenster/Level und Inversions‑Einstellungen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Overlay‑Rendering">}}

<p>DICOM‑Bilder können Overlay‑Ebene enthalten &mdash; Grafiken oder Textannotationen, die ins Bild eingebrannt sind. Die Klasse <code>RenderOptions</code> steuert, ob Overlays gerendert werden und in welcher Farbe sie erscheinen:</p>

<div class="codeblock" id="code">
 <h3>Rendern mit Overlay‑Steuerung – C#</h3>
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

<p>DICOM unterstützt zwei Overlay‑Typen: <strong>Graphics</strong>-Overlays (Annotationen, Messungen) und <strong>Region of Interest</strong>- (ROI‑)Overlays. Beide Typen werden über die Einstellung <code>ShowOverlays</code> gesteuert.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Mehrfach‑Frame‑DICOM‑Rendering">}}

<p>Viele DICOM‑Dateien von CT, MRT und Ultraschall enthalten mehrere Frames (Schichten). Sie können die Anzahl der Frames prüfen und jeden einzeln rendern:</p>

<div class="codeblock" id="code">
 <h3>Alle Frames eines Multi‑Frame‑DICOM rendern – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="VOI LUT‑Transformationstypen">}}

<p>Der DICOM‑Standard definiert mehrere VOI‑LUT‑Transformationsfunktionen, die steuern, wie die Fenster/Level‑Abbildung angewendet wird. Aspose.Medical für .NET unterstützt alle Standardtypen:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Typ</th>
<th>Beschreibung</th>
<th>Anwendungsfall</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Standardmäßige lineare Abbildung mit Offset vom Zentrum</td><td>Am häufigsten, verwendet für allgemeine CT/MR‑Bildgebung</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Lineare Abbildung ohne Offset vom Zentrum</td><td>Wenn eine präzise Pixelwertzuordnung erforderlich ist</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>S‑Kurven‑Transformation für weichere Kontrastübergänge</td><td>Weichteilsvisualisierung, Mammographie</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>Benutzerdefinierte LUT, definiert als Lookup‑Table im DICOM‑Datensatz</td><td>Hersteller‑ oder modalitiespezifische Anzeige‑Kurven</td></tr>
</tbody>
</table>

<p>Die Rendering‑Engine wählt automatisch die passende VOI‑LUT‑Transformation basierend auf den Attributen des DICOM‑Datensatzes aus. Beim Einsatz benutzerdefinierter <code>GrayscaleRenderOptions</code> wird standardmäßig die lineare Transformation angewendet.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Unterstützte Modalitäten">}}

<p>Aspose.Medical für .NET unterstützt das Rendering von DICOM‑Bildern aus allen gängigen medizinischen Bildgebungsmodalitäten:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modalität</th>
<th>Beschreibung</th>
<th>Typisches Format</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Computertomografie</td><td>Graustufen, Mehrfach‑Frame, 12&ndash;16 Bit</td></tr>
<tr><td><strong>MR</strong></td><td>Magnetresonanz</td><td>Graustufen, Mehrfach‑Frame, 12&ndash;16 Bit</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Computerradiographie / Digitale Radiographie</td><td>Graustufen, Einzel‑Frame, 10&ndash;16 Bit</td></tr>
<tr><td><strong>US</strong></td><td>Ultraschall</td><td>Graustufen oder RGB, Mehrfach‑Frame</td></tr>
<tr><td><strong>MG</strong></td><td>Mammographie</td><td>Graustufen, hohe Auflösung, 12&ndash;16 Bit</td></tr>
<tr><td><strong>XA</strong></td><td>Röntgen‑Angiographie</td><td>Graustufen, Mehrfach‑Frame</td></tr>
<tr><td><strong>NM</strong></td><td>Nuklearmedizin</td><td>Graustufen, Mehrfach‑Frame</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Positronen‑Emissions‑Tomographie)</td><td>Graustufen, Mehrfach‑Frame</td></tr>
</tbody>
</table>

<p>Sowohl Graustufen (MONOCHROME1/MONOCHROME2) als auch Farb‑Interpretationen (RGB, YBR_FULL, PALETTE COLOR) werden unterstützt. Die Rendering‑Engine führt automatisch die passende Farbraumkonvertierung durch.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Quellcode" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑Referenzen" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produkt‑Support" tabId="support" >}}
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
