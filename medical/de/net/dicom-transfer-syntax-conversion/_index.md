---
title: DICOM-Transfer-Syntax-Konvertierung in C# .NET | Aspose.Medical
weight: 16000
description: Transkodieren Sie DICOM-Dateien zwischen Transfer-Syntaxen in C# .NET. Unterstützung für JPEG, JPEG 2000, JPEG‑LS, RLE und unkomprimierte Formate mit der Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM-Transfer-Syntax-Konvertierung in .NET C#" h2="Transkodieren Sie DICOM-Dateien zwischen unkomprimierten, JPEG-, JPEG 2000-, JPEG‑LS- und RLE-Transfer-Syntaxen. Reine .NET-Bibliothek ohne native Abhängigkeiten." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Was ist eine Transfer‑Syntax?">}}

<p>Eine <strong>Transfer Syntax</strong> definiert, wie DICOM-Daten für Speicherung und Übertragung codiert werden. Sie legt drei zentrale Aspekte fest: Byte‑Reihenfolge (Endianness), ob Value Representations explizit oder implizit sind, und der auf die Pixel-Daten angewandte Kompressionsalgorithmus. Jede DICOM-Datei gibt ihre Transfer-Syntax im File Meta Information-Header an.</p>

<p>Verschiedene medizinische Geräte, PACS-Server und Anzeigesoftware unterstützen unterschiedliche Sätze von Transfer-Syntaxen. <strong>Aspose.Medical for .NET</strong> stellt die Methode <code>Transcode</code> bereit, um zwischen Transfer-Syntaxen zu konvertieren, wodurch Interoperabilität, Speicheroptimierung und Kompatibilität mit Verarbeitungswerkzeugen ermöglicht werden &mdash; alles in einer reinen .NET-Bibliothek ohne native Abhängigkeiten.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transkodieren einer DICOM-Datei in C#">}}

<p>Die Methode <code>DicomFile.Transcode</code> konvertiert eine DICOM-Datei von ihrer aktuellen Transfer-Syntax in eine beliebige unterstützte Ziel-Syntax. Die Methode gibt eine neue <code>DicomFile</code>-Instanz zurück &mdash; das Original bleibt unverändert:</p>

<div class="codeblock" id="code">
 <h3>Grundlegende DICOM-Transkodierung - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Sie können auch direkt auf <code>Dataset</code>-Ebene transkodieren:</p>

<div class="codeblock" id="code">
 <h3>Transkodieren eines Datasets - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Unterstützte Transfer‑Syntaxen">}}

<p>Die nachfolgende Tabelle listet alle standardisierten DICOM-Bilddaten-Transfer-Syntaxen und deren aktuellen Unterstützungsstatus in Aspose.Medical for .NET auf. Alle unterstützten Codec-Implementierungen erfolgen in reinem C# und sind vollständig plattformunabhängig.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Typ</th>
<th>Status</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Unkomprimiert</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Unkomprimiert</td><td>Unterstützt</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Unkomprimiert</td><td>Unterstützt</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Unkomprimiert</td><td>Unterstützt</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Unterstützt</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Verlustbehaftet, 8‑Bit</td><td>Unterstützt</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Verlustbehaftet, 12‑Bit</td><td>Nicht unterstützt</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Verlustfrei</td><td>Unterstützt (nur 8‑Bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Verlustfrei</td><td>Unterstützt (nur 8‑Bit)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Verlustfrei</td><td>Unterstützt</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Fast verlustfrei</td><td>Unterstützt</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Verlustfrei</td><td>Unterstützt (Lesen 8/16‑Bit, Schreiben 8‑Bit)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Verlustbehaftet oder verlustfrei</td><td>Unterstützt (Lesen 8/16‑Bit, Schreiben 8‑Bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Verlustfrei</td><td>Unterstützt (Lesen 8/16‑Bit, Schreiben 8‑Bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Verlustbehaftet oder verlustfrei</td><td>Unterstützt (Lesen 8/16‑Bit, Schreiben 8‑Bit)</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Verlustfrei</td><td>Unterstützt</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Verlustfrei</td><td>Demnächst</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Verlustfrei</td><td>Demnächst</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Verlustbehaftet oder verlustfrei</td><td>Demnächst</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Verlustfrei</td><td>Demnächst</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Verlustfrei</td><td>Demnächst</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Verlustbehaftet oder verlustfrei</td><td>Demnächst</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Häufige Transkodierungsszenarien">}}

<p>Verschiedene Arbeitsabläufe erfordern unterschiedliche Transkodierungsstrategien. Hier sind die gängigsten Szenarien:</p>

<div class="codeblock" id="code">
 <h3>Dekomprimieren für Verarbeitung - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Komprimieren für Archivspeicherung - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Komprimieren für Netzwerkübertragung - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transfer‑Syntax-Eigenschaften inspizieren">}}

<p>Die Klasse <code>TransferSyntax</code> stellt Eigenschaften bereit, die die Kodierungsmerkmale beschreiben. Verwenden Sie diese, um die aktuelle Transfer‑Syntax einer Datei zu inspizieren oder eine passende Ziel‑Syntax auszuwählen:</p>

<div class="codeblock" id="code">
 <h3>Transfer‑Syntax‑Eigenschaften auslesen - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>Eigenschaft</th>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>Der eindeutige Bezeichner der Transfer‑Syntax</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Ob Value Representations explizit kodiert sind</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Ob die Byte‑Reihenfolge little endian ist</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Ob Pixel‑Daten gekapselt (komprimiert) sind</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Ob das Kompressionsverfahren verlustbehaftet ist</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Ob die Syntax Deflate‑Kompression verwendet</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Ob die Transfer‑Syntax vom DICOM‑Standard zurückgezogen wurde</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>Der ISO‑Standard‑Bezeichner der verlustbehafteten Kompressionsmethode</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Verlustbehaftete vs. verlustfreie Kompression">}}

<p>Das Verständnis des Unterschieds zwischen verlustbehafteter und verlustfreier Kompression ist beim Transkodieren von DICOM-Dateien entscheidend:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Aspekt</th>
<th>Verlustfrei</th>
<th>Verlustbehaftet</th>
</tr>
</thead>
<tbody>
<tr><td>Bildqualität</td><td>Pixelgenau &mdash; Originaldaten vollständig erhalten</td><td>Einige Daten dauerhaft verloren, um kleinere Größe zu erreichen</td></tr>
<tr><td>Kompressionsrate</td><td>Typisch 2:1 bis 3:1</td><td>Typisch 10:1 bis 30:1 oder höher</td></tr>
<tr><td>Rundreise‑sicher</td><td>Ja &mdash; dekomprimieren und identische Pixel erhalten</td><td>Nein &mdash; jedes verlustbehaftete erneute Kodieren verschlechtert die Qualität weiter</td></tr>
<tr><td>Anwendungsfälle</td><td>Archivierung, Diagnostik, Rechtsdokumente</td><td>Erste Begutachtung, Telemedizin, Netzwerkübertragung</td></tr>
<tr><td>Unterstützte Codecs</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near‑Lossless, JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>Wichtig:</strong> Das Transkodieren einer verlustbehaftet komprimierten Datei in eine verlustfreie Syntax stellt verlorene Daten nicht wieder her. Der Qualitätsverlust der ursprünglichen verlustbehafteten Kompression ist dauerhaft.</p>

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
