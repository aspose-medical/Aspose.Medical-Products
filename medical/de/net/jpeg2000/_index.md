---
title: DICOM JPEG 2000-Kompression in C# .NET | Aspose.Medical
weight: 2000
description: Lesen, schreiben und transkodieren Sie DICOM-Dateien mit JPEG 2000-Kompression in C# .NET. Unterstützung für 8‑Bit‑Farbbilder und 16‑Bit‑Monochrom‑Bilder, verlustfreie und verlustbehaftete Modi sowie HTJ2K mit der Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM JPEG 2000-Unterstützung in .NET C#" h2="Lesen, schreiben und transkodieren Sie DICOM-Dateien mit JPEG 2000-Kompression. Verlustfreie und verlustbehaftete Modi, 8‑Bit‑Farbe und 16‑Bit‑Monochrom‑Pixeldaten, HTJ2K enthalten – alles in reinem .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 in der medizinischen Bildgebung">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) ist der am weitesten verbreitete wavelet-basierte Kompressionsstandard in der medizinischen Bildgebung. Im Gegensatz zu herkömmlichem JPEG bietet es sowohl verlustfreie als auch verlustbehaftete Kompression in einem einzigen Codec, progressives Dekodieren für Region‑of‑Interest‑Zugriff und überlegene Kompressionsverhältnisse &mdash; was es ideal für das Archivieren großer Studien und das Übertragen von Bildern über begrenzte Netzwerke macht.</p>

<p><strong>Aspose.Medical für .NET</strong> bietet eine reine C#‑Implementierung des JPEG 2000‑Codecs ohne native Abhängigkeiten. Die Bibliothek kann DICOM-Dateien lesen, rendern und transkodieren, die mit einer der vier Standard‑JPEG‑2000‑Transfer‑Syntaxen komprimiert wurden.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Unterstützte JPEG 2000 Transfersyntaxen">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Transfersyntax</th>
<th>UID</th>
<th>Modus</th>
<th>Lesen</th>
<th>Schreiben</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 nur verlustfrei</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Verlustfrei</td><td>8‑Bit‑RGB, 16‑Bit‑Monochrom</td><td>16‑Bit‑Monochrom, 8‑Bit‑RGB</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Verlustbehaftet oder verlustfrei</td><td>8‑Bit‑RGB, 16‑Bit‑Monochrom</td><td>16‑Bit‑Monochrom, 8‑Bit‑RGB</td></tr>
<tr><td>JPEG 2000 Teil 2 Mehrkomponenten nur verlustfrei</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Verlustfrei</td><td>Nicht unterstützt</td><td>Nicht unterstützt</td></tr>
<tr><td>JPEG 2000 Teil 2 Mehrkomponenten</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Verlustbehaftet oder verlustfrei</td><td>Nicht unterstützt</td><td>Nicht unterstützt</td></tr>
<tr><td>HTJ2K nur verlustfrei</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Verlustfrei</td><td>Monochrom und Farbe</td><td>Monochrom und Farbe</td></tr>
<tr><td>HTJ2K mit RPCL-Optionen nur verlustfrei</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Verlustfrei</td><td>Monochrom und Farbe</td><td>Monochrom und Farbe</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Verlustbehaftet oder verlustfrei</td><td>Monochrom und Farbe</td><td>Monochrom und Farbe</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8‑Bit‑ und 16‑Bit‑Pixeldaten">}}

<p>Medizinische Bilder verwenden häufig 16 Bit pro Sample, um den vollen Dynamikbereich von Modalitäten wie CT (typischerweise 12‑Bit in 16‑Bit gespeichert) und MRI abzubilden. Aspose.Medical unterstützt beide Bittiefen für JPEG 2000:</p>

<ul>
<li><strong>Lesen (Dekompression)</strong>: 16‑Bit‑Monochrom‑Dateien (CT, MRI, Röntgen) und 8‑Bit‑Drei‑Komponenten‑Farbdaten (RGB, YBR_RCT, YBR_ICT). Paletten, CMYK, ICC‑Profile und unterabgetastete Farbcodestreams werden mit einer klaren Ausnahme abgewiesen, anstatt ein stillschweigend falsches Bild zu erzeugen.</li>
<li><strong>Schreiben (Kompression)</strong>: 16‑Bit‑Monochrom‑ und 8‑Bit‑RGB‑Bilder. 8‑Bit‑Monochrom‑ und 16‑Bit‑Farbkodierung sind nicht verfügbar; verwenden Sie dafür HTJ2K oder JPEG XL, beide akzeptieren Monochrom und Farbe in beiden Bittiefen.</li>
</ul>

<div class="codeblock" id="code">
 <h3>DICOM mit JPEG 2000 komprimiert lesen und inspizieren – C#</h3>
 <pre><code class="cs">// Open a JPEG 2000 compressed DICOM file (8-bit or 16-bit)
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Check transfer syntax
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax
Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");

// Inspect pixel data bit depth
PixelData pixelData = PixelData.Create(dicomFile.Dataset);
Console.WriteLine($"Bits Allocated: {pixelData.BitsAllocated}");
Console.WriteLine($"Bits Stored: {pixelData.BitsStored}");
Console.WriteLine($"High Bit: {pixelData.HighBit}");
Console.WriteLine($"Samples Per Pixel: {pixelData.SamplesPerPixel}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="In JPEG 2000 transkodieren">}}

<p>Verwenden Sie die <code>Transcode</code>-Methode, um eine beliebige DICOM-Datei in JPEG 2000 zu komprimieren oder zwischen JPEG 2000‑Modi zu konvertieren:</p>

<div class="codeblock" id="code">
 <h3>DICOM verlustfrei in JPEG 2000 komprimieren – C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>DICOM verlustbehaftet in JPEG 2000 komprimieren – C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM‑Dateien dekomprimieren">}}

<p>Dekodieren Sie JPEG 2000‑Dateien in eine unkomprimierte Transfersyntax für Verarbeitung, Analyse oder Kompatibilität mit Systemen, die JPEG 2000 nicht unterstützen:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 in unkomprimiert dekomprimieren – C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Sie können außerdem in einem Schritt dekomprimieren und in andere Kompressionsformate transkodieren:</p>

<div class="codeblock" id="code">
 <h3>Zwischen Kompressionsformaten transkodieren – C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM‑Bilder rendern">}}

<p>JPEG 2000‑komprimierte DICOM‑Dateien können in Pixeldaten gerendert werden für Anzeige oder Export, genau wie jede andere Transfersyntax:</p>

<div class="codeblock" id="code">
 <h3>Ein JPEG 2000‑komprimiertes Frame rendern – C#</h3>
 <pre><code class="cs">// Open JPEG 2000 DICOM file
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Render the first frame — decompression is handled automatically
using PixelImage&lt;Bgra32&gt; image = dicomFile.RenderImage(0);

// Copy the BGRA32 pixel data for display or export
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = new Bgra32[width * height];
image.CopyPixelsTo(pixels);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Verlustfrei vs. verlustbehaftet JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Aspekt</th>
<th>JPEG 2000 verlustfrei</th>
<th>JPEG 2000 verlustbehaftet</th>
</tr>
</thead>
<tbody>
<tr><td>Transfersyntax</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Bildqualität</td><td>Pixelgenau &mdash; identisch zum Original</td><td>Visuell ähnlich, einige Daten dauerhaft verloren</td></tr>
<tr><td>Kompressionsverhältnis</td><td>Typischerweise 2:1 bis 3:1</td><td>Typischerweise 10:1 bis 30:1 oder höher</td></tr>
<tr><td>Am besten für</td><td>Diagnostische Archivierung, rechtliche Aufzeichnungen, primäre Befundung</td><td>Vorläufige Durchsicht, Telemedizin, Netzwerkübertragung</td></tr>
<tr><td>Rundlauf sicher</td><td>Ja</td><td>Nein &mdash; erneutes Kodieren verschlechtert die Qualität weiter</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High‑Throughput JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) ersetzt den langsamen arithmetischen Kodierer von JPEG 2000 durch einen schnelleren Blockkodierer. Es behält dieselbe Wavelet‑Transformation, Fortschrittsreihenfolgen und Qualität bei und dekodiert sowie kodiert mehrere Male schneller. Aspose.Medical implementiert alle drei DICOM‑HTJ2K‑Transfersyntaxen in reinem .NET, für Monochrom‑ und Farbbilder, und transkodiert zwischen HTJ2K und allen anderen unterstützten Syntaxen:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; nur verlustfrei</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; verlustfrei mit RPCL‑Fortschrittsreihenfolge</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; verlustbehaftet oder verlustfrei</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 nach HTJ2K und zurück transkodieren – C#</h3>
 <pre><code class="cs">// Transcode a JPEG 2000 file to HTJ2K, and back to classic JPEG 2000
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile htj2kFile = j2kFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2kFile.Save("htj2k_lossless.dcm");

// HTJ2K with RPCL progression order, lossless
DicomFile rpclFile = j2kFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
rpclFile.Save("htj2k_rpcl.dcm");

// HTJ2K lossy
DicomFile htj2kLossy = j2kFile.Transcode(TransferSyntax.HTJ2K);
htj2kLossy.Save("htj2k_lossy.dcm");

// Any HTJ2K file decodes back to an uncompressed transfer syntax
DicomFile uncompressed = htj2kFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decoded.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Quellcode" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑Referenzen" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktunterstützung" tabId="support" >}}
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
