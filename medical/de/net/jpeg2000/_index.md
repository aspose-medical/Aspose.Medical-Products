---
title: DICOM JPEG 2000 Kompression in C# .NET | Aspose.Medical
weight: 2000
description: Lesen, schreiben und transkodieren Sie DICOM‑Dateien mit JPEG‑2000‑Kompression in C# .NET. Unterstützung für 8‑Bit‑ und 16‑Bit‑Bilder, verlustfreie und verlustbehaftete Modi, Mehrkomponentendaten mit der Aspose.Medical‑API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM JPEG 2000 Unterstützung in .NET C#" h2="Lesen, schreiben und transkodieren Sie DICOM‑Dateien mit JPEG‑2000‑Kompression. Verlustfreie und verlustbehaftete Modi, 8‑Bit‑ und 16‑Bit‑Pixel‑Daten, Mehrkomponenten‑Bilder — alles in reinem .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 in der medizinischen Bildgebung">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) ist der am weitesten verbreitete wellenbasierte Kompressionsstandard in der medizinischen Bildgebung. Im Gegensatz zu herkömmlichem JPEG bietet es sowohl verlustfreie als auch verlustbehaftete Kompression in einem einzigen Codec, progressives Dekodieren für den Zugriff auf Region‑of‑Interest und überlegene Kompressionsraten &mdash; was es ideal für die Archivierung großer Studien und die Übertragung von Bildern über eingeschränkte Netzwerke macht.</p>

<p><strong>Aspose.Medical für .NET</strong> bietet eine reine C#‑Implementierung des JPEG‑2000‑Codecs ohne native Abhängigkeiten. Die Bibliothek kann DICOM‑Dateien lesen, rendern und transkodieren, die mit einer der vier Standard‑JPEG‑2000‑Transfer‑Syntaxen komprimiert sind.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Unterstützte JPEG‑2000‑Transfer‑Syntaxen">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer‑Syntax</th>
<th>UID</th>
<th>Modus</th>
<th>Lesen</th>
<th>Schreiben</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 nur verlustfrei</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Verlustfrei</td><td>8‑Bit und 16‑Bit</td><td>8‑Bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Verlustbehaftet oder verlustfrei</td><td>8‑Bit und 16‑Bit</td><td>8‑Bit</td></tr>
<tr><td>JPEG 2000 Part 2 Mehrkomponent – nur verlustfrei</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Verlustfrei</td><td>8‑Bit und 16‑Bit</td><td>8‑Bit</td></tr>
<tr><td>JPEG 2000 Part 2 Mehrkomponent</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Verlustbehaftet oder verlustfrei</td><td>8‑Bit und 16‑Bit</td><td>8‑Bit</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8‑Bit‑ und 16‑Bit‑Pixel‑Daten">}}

<p>Medizinische Bilder verwenden häufig 16 Bit pro Probe, um den vollen Dynamikbereich von Modalitäten wie CT (typischerweise 12‑Bit in 16‑Bit gespeichert) und MRT abzubilden. Aspose.Medical unterstützt beide Bittiefen für JPEG‑2000:</p>

<ul>
<li><strong>Lesen (Dekompression)</strong>: Vollständige Unterstützung für sowohl 8‑Bit‑ als auch 16‑Bit‑JPEG‑2000‑komprimierte DICOM‑Dateien. Die Bibliothek dekodiert Pixel‑Daten korrekt, unabhängig von den ursprünglichen Bits Allocated, Bits Stored und High Bit‑Werten.</li>
<li><strong>Schreiben (Kompression)</strong>: Derzeit werden 8‑Bit‑Bilder unterstützt. Die Unterstützung für 16‑Bit‑Schreiben ist für ein zukünftiges Release geplant.</li>
</ul>

<div class="codeblock" id="code">
 <h3>DICOM mit JPEG‑2000‑Kompression lesen und prüfen – C#</h3>
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

<p>Verwenden Sie die Methode <code>Transcode</code>, um jede DICOM‑Datei zu JPEG‑2000 zu komprimieren oder zwischen JPEG‑2000‑Modi zu konvertieren:</p>

<div class="codeblock" id="code">
 <h3>DICOM verlustfrei zu JPEG 2000 komprimieren – C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>DICOM verlustbehaftet zu JPEG 2000 komprimieren – C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM‑Dateien dekomprimieren">}}

<p>Dekomprimieren Sie JPEG‑2000‑Dateien zu einer unkomprimierten Transfer‑Syntax für Verarbeitung, Analyse oder Kompatibilität mit Systemen, die JPEG‑2000 nicht unterstützen:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 zu unkomprimiert dekomprimieren – C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Sie können auch in einem Schritt dekomprimieren und zu anderen Kompressionsformaten transkodieren:</p>

<div class="codeblock" id="code">
 <h3>Zwischen Kompressionsformaten transkodieren – C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM‑Bilder rendern">}}

<p>JPEG‑2000‑komprimierte DICOM‑Dateien können in Pixeldaten gerendert werden, um sie anzuzeigen oder zu exportieren, genau wie bei jeder anderen Transfer‑Syntax:</p>

<div class="codeblock" id="code">
 <h3>Einen JPEG 2000‑komprimierten Frame rendern – C#</h3>
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
<tr><td>Transfer‑Syntax</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Bildqualität</td><td>Pixelgenau &mdash; identisch zum Original</td><td>Visuell ähnlich, einige Daten dauerhaft verloren</td></tr>
<tr><td>Kompressionsrate</td><td>Typischerweise 2:1 bis 3:1</td><td>Typischerweise 10:1 bis 30:1 oder höher</td></tr>
<tr><td>Ideal für</td><td>Diagnostische Archivierung, Rechtsdokumente, primäre Befundung</td><td>Vorläufige Begutachtung, Telemedizin, Netzwerkübertragung</td></tr>
<tr><td>Rücktransformation sicher</td><td>Ja</td><td>Nein &mdash; erneutes Kodieren verschlechtert die Qualität weiter</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 Mehrkomponente">}}

<p>JPEG 2000 Part 2 (ISO/IEC 15444-2) erweitert den Standard‑Codec um Mehrkomponenten‑Transformationsfähigkeiten. Dies wird für farbige medizinische Bilder und Modalitäten, die Mehrkanaldaten erzeugen, verwendet. Aspose.Medical unterstützt beide Part‑2‑Transfer‑Syntaxen:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; verlustfreie Kompression mit interkomponentaler Dekorrelation für optimale Kompression von Mehrkanaldaten.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; verlustbehaftete oder verlustfreie Kompression mit Mehrkomponenten‑Transformationen.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K) – Demnächst">}}

<p>HTJ2K (ISO/IEC 15444-15) ist eine nächste‑Generation‑Erweiterung von JPEG 2000, entwickelt für dramatisch schnellere Kodier‑ und Dekodiergeschwindigkeiten bei gleichbleibender Kompressionseffizienz. Es wird voraussichtlich der bevorzugte Codec für Echtzeit‑Arbeitsabläufe in der medizinischen Bildgebung werden.</p>

<p>Aspose.Medical wird in einem zukünftigen Release HTJ2K‑Unterstützung hinzufügen und drei Transfer‑Syntaxen abdecken:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; nur verlustfrei</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; verlustfrei mit RPCL‑Progressionsreihenfolge</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; verlustbehaftet oder verlustfrei</li>
</ul>

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
