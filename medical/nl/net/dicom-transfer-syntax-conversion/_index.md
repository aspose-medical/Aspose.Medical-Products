---
title: DICOM Transfer Syntax Conversie in C# .NET | Aspose.Medical
weight: 16000
description: Transcodeer DICOM‑bestanden tussen transfer syntaxes in C# .NET. Ondersteuning voor JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG‑LS, RLE en ongecomprimeerde formaten met de Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM Transfer Syntax Conversie in .NET C#" h2="Transcodeer DICOM‑bestanden tussen ongecomprimeerde, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG‑LS en RLE transfer syntaxes. Pure .NET‑bibliotheek zonder native afhankelijkheden." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Wat is Transfer Syntax?">}}

<p>Een <strong>Transfer Syntax</strong> definieert hoe DICOM‑gegevens worden gecodeerd voor opslag en overdracht. Het specificeert drie belangrijke aspecten: byte‑volgorde (endianness), of Value Representations expliciet of impliciet zijn, en het compressie‑algoritme dat op pixelgegevens wordt toegepast. Elk DICOM‑bestand geeft zijn transfer syntax op in de File Meta Information‑header.</p>

<p>Verschillende medische apparaten, PACS‑servers en weergave‑applicaties ondersteunen verschillende sets van transfer syntaxes. <strong>Aspose.Medical for .NET</strong> biedt de <code>Transcode</code>‑methode om tussen transfer syntaxes te converteren, waardoor interoperabiliteit, opslagoptimalisatie en compatibiliteit met verwerkings‑tools mogelijk zijn &mdash; alles in een pure .NET‑bibliotheek zonder native afhankelijkheden.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transcodeer een DICOM‑bestand in C#">}}

<p>De <code>DicomFile.Transcode</code>‑methode converteert een DICOM‑bestand van zijn huidige transfer syntax naar elke ondersteunde doelsyntax. De methode retourneert een nieuw <code>DicomFile</code>‑object &mdash; het origineel blijft ongewijzigd:</p>

<div class="codeblock" id="code">
 <h3>Basis DICOM‑transcoding - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>U kunt ook direct op <code>Dataset</code>-niveau transcoderen:</p>

<div class="codeblock" id="code">
 <h3>Transcode een Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ondersteunde Transfer Syntaxes">}}

<p>De onderstaande tabel toont alle standaard DICOM‑beeldgegevens‑transfer syntaxes en hun huidige ondersteuningsstatus in Aspose.Medical for .NET. Alle ondersteunde codecs zijn geïmplementeerd in pure C# en zijn volledig platform‑onafhankelijk.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Type</th>
<th>Status</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Ongecomprimeerd</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Ongecomprimeerd</td><td>Ondersteund</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Ongecomprimeerd</td><td>Ondersteund</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Ongecomprimeerd (verouderd)</td><td>Ondersteund</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Ongecomprimeerd</td><td>Niet ondersteund</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Ondersteund</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Met verlies, 8-bit</td><td>Ondersteund</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Met verlies, 12-bit</td><td>Niet ondersteund</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Lossless</td><td>Ondersteund (alleen 8-bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Lossless</td><td>Ondersteund (alleen 8-bit)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Lossless</td><td>Ondersteund</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Bijna verliesloos</td><td>Ondersteund</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>Ondersteund (lezen 8-bit kleur en 16-bit monochroom; schrijven 16-bit monochroom of 8-bit RGB)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Met verlies of verliesloos</td><td>Ondersteund (lezen 8-bit kleur en 16-bit monochroom; schrijven 16-bit monochroom of 8-bit RGB)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Niet ondersteund</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Met verlies of verliesloos</td><td>Niet ondersteund</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Lossless</td><td>Ondersteund</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Ondersteund</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Ondersteund</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Met verlies of verliesloos</td><td>Ondersteund</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Lossless</td><td>Ondersteund</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Lossless</td><td>Alleen decoderen (codering vereist een JPEG‑bronstroom, geen pixelgegevens)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Met verlies of verliesloos</td><td>Ondersteund (lossy‑modus)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Veelvoorkomende Transcoding‑scenario's">}}

<p>Verschillende workflows vereisen verschillende transcoding‑strategieën. Hieronder de meest voorkomende scenario's:</p>

<div class="codeblock" id="code">
 <h3>Decomprimeren voor verwerking - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Comprimeren voor archiefopslag - C#</h3>
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
 <h3>Comprimeren voor netwerktransmissie - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Gebruik de nieuwste codecs: HTJ2K en JPEG XL - C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Inspecteer Transfer Syntax‑eigenschappen">}}

<p>De <code>TransferSyntax</code>-klasse exposeert eigenschappen die de coderingskenmerken beschrijven. Gebruik deze om de huidige transfer syntax van een bestand te inspecteren of om een geschikte doelsyntax te selecteren:</p>

<div class="codeblock" id="code">
 <h3>Lees transfer syntax‑eigenschappen - C#</h3>
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
<th>Eigenschap</th>
<th>Type</th>
<th>Beschrijving</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>De unieke identifier van de transfer syntax</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Of Value Representations expliciet zijn gecodeerd</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Of de byte‑volgorde little endian is</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Of pixeldata is ingekapseld (gecomprimeerd)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Of de compressiemethode verliesgevend is</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Of de syntax deflate‑compressie gebruikt</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Of de transfer syntax door de DICOM‑standaard is verouderd</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>De ISO‑standaardidentifier van de verliesgevende compressiemethode</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Verliesgevend vs Verliesloos Compressie">}}

<p>Het begrijpen van het verschil tussen verliesgevende en verliesloze compressie is cruciaal bij het transcoderen van DICOM‑bestanden:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Aspect</th>
<th>Verliesloos</th>
<th>Verliesgevend</th>
</tr>
</thead>
<tbody>
<tr><td>Beeldkwaliteit</td><td>Pixel-perfect — originele gegevens volledig behouden</td><td>Sommige gegevens permanent verloren om een kleinere grootte te bereiken</td></tr>
<tr><td>Compressieverhouding</td><td>Typisch 2:1 tot 3:1</td><td>Typisch 10:1 tot 30:1 of hoger</td></tr>
<tr><td>Rondreis veilig</td><td>Ja — decompress en krijg identieke pixels</td><td>Nee — elke verliesgevende hercodering degradeert de kwaliteit verder</td></tr>
<tr><td>Toepassingsgevallen</td><td>Archivering, diagnostiek, juridische dossiers</td><td>Voorlopige beoordeling, telemedicine, netwerktransmissie</td></tr>
<tr><td>Ondersteunde codecs</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>Belangrijk:</strong> Transcoderen van een verliesgecomprimeerd bestand naar een verliesloze syntax herstelt de verloren gegevens niet. De kwaliteitsdegradatie die ontstaat door de oorspronkelijke verliesgevende compressie is permanent.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leermiddelen" tabId="resources" >}}
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
