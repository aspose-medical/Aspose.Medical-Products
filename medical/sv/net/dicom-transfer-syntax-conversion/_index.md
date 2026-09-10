---
title: DICOM‑överföringssyntaxkonvertering i C# .NET | Aspose.Medical
weight: 16000
description: Koda om DICOM‑filer mellan överföringssyntaxer i C# .NET. Stöd för JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG‑LS, RLE och okomprimerade format med Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM‑överföringssyntaxkonvertering i .NET C#" h2="Koda om DICOM‑filer mellan okomprimerade, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG‑LS och RLE‑överföringssyntaxer. Ren .NET‑bibliotek utan inhemska beroenden." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Vad är överföringssyntax?">}}

<p>En <strong>Transfer Syntax</strong> definierar hur DICOM‑data kodas för lagring och överföring. Den specificerar tre nyckelaspekter: byte‑ordning (endianness), om Value Representations är explicita eller implicita, samt komprimeringsalgoritmen som tillämpas på bilddata. Varje DICOM‑fil deklarerar sin överföringssyntax i filens meta‑informationshuvud.</p>

<p>Olika medicinska enheter, PACS‑servrar och visningsprogram stödjer olika uppsättningar av överföringssyntaxer. <strong>Aspose.Medical for .NET</strong> tillhandahåller <code>Transcode</code>-metoden för att konvertera mellan överföringssyntaxer, vilket möjliggör interoperabilitet, lagringsoptimering och kompatibilitet med bearbetningsverktyg &mdash; allt i ett rent .NET‑bibliotek utan inhemska beroenden.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Koda om en DICOM‑fil i C#">}}

<p>Metoden <code>DicomFile.Transcode</code> konverterar en DICOM‑fil från dess nuvarande överföringssyntax till någon stödjande mål‑syntax. Metoden returnerar ett nytt <code>DicomFile</code>-objekt &mdash; originalet förblir oförändrat:</p>

<div class="codeblock" id="code">
 <h3>Grundläggande DICOM‑kodning – C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Du kan också koda om på <code>Dataset</code>-nivå direkt:</p>

<div class="codeblock" id="code">
 <h3>Koda om ett Dataset – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stödda överföringssyntaxer">}}

<p>Följande tabell listar alla standard DICOM‑bilddatas‑överföringssyntaxer och deras nuvarande stödstatus i Aspose.Medical for .NET. Alla stödjade codecs är implementerade i ren C# och är helt plattformsoberoende.</p>

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
<tr><td colspan="4"><strong>Okomprimerad</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Okomprimerad</td><td>Stöds</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Okomprimerad</td><td>Stöds</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Okomprimerad (utgången)</td><td>Stöds</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Okomprimerad</td><td>Stöds ej</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Stöds</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Förlustkomprimerad, 8‑bit</td><td>Stöds</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Förlustkomprimerad, 12‑bit</td><td>Stöds ej</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Förlustfri</td><td>Stöds (endast 8‑bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Förlustfri</td><td>Stöds (endast 8‑bit)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Förlustfri</td><td>Stöds</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Nära förlustfri</td><td>Stöds</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Förlustfri</td><td>Stöds (läser 8‑bit färg och 16‑bit monokrom; skriver 16‑bit monokrom eller 8‑bit RGB)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Förlustkomprimerad eller förlustfri</td><td>Stöds (läser 8‑bit färg och 16‑bit monokrom; skriver 16‑bit monokrom eller 8‑bit RGB)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Förlustfri</td><td>Stöds ej</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Förlustkomprimerad eller förlustfri</td><td>Stöds ej</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Förlustfri</td><td>Stöds</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Förlustfri</td><td>Stöds</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Förlustfri</td><td>Stöds</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Förlustkomprimerad eller förlustfri</td><td>Stöds</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Förlustfri</td><td>Stöds</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Förlustfri</td><td>Endast dekodning (kodning kräver en JPEG‑källström, inte bilddata)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Förlustkomprimerad eller förlustfri</td><td>Stöds (förlustkomprimerat läge)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vanliga transkodningsscenarier">}}

<p>Olika arbetsflöden kräver olika transkodningsstrategier. Här är de vanligaste scenarierna:</p>

<div class="codeblock" id="code">
 <h3>Dekomprimera för bearbetning – C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Komprimera för arkiveringslagring – C#</h3>
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
 <h3>Komprimera för nätverksöverföring – C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Använd de senaste kodekarna: HTJ2K och JPEG XL – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Inspektera egenskaper för överföringssyntax">}}

<p>Klassen <code>TransferSyntax</code> exponerar egenskaper som beskriver kodningskarakteristikerna. Använd dessa för att inspektera en fils nuvarande överföringssyntax eller för att välja en lämplig mål‑syntax:</p>

<div class="codeblock" id="code">
 <h3>Läs egenskaper för överföringssyntax – C#</h3>
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
<th>Egenskap</th>
<th>Typ</th>
<th>Beskrivning</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>Den unika identifieraren för överföringssyntaxis</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Om Value Representations är explicit kodade</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Om byteordningen är little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Om bilddata är kapslad (komprimerad)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Om komprimeringsmetoden är förlustkomprimerad</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Om syntaxis använder deflate‑komprimering</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Om överföringssyntaxis har utgått enligt DICOM‑standarden</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>ISO‑standardidentifieraren för den förlustkomprimerade metoden</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Förlustkomprimering vs förlustfri komprimering">}}

<p>Att förstå skillnaden mellan förlustkomprimering och förlustfri komprimering är avgörande vid transkodning av DICOM‑filer:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Aspekt</th>
<th>Förlustfri</th>
<th>Förlustkomprimerad</th>
</tr>
</thead>
<tbody>
<tr><td>Image quality</td><td>Pixelperfekt &mdash; originaldata bevaras helt</td><td>Viss data förloras permanent för att uppnå mindre storlek</td></tr>
<tr><td>Compression ratio</td><td>Typiskt 2:1 till 3:1</td><td>Typiskt 10:1 till 30:1 eller högre</td></tr>
<tr><td>Round-trip safe</td><td>Ja &mdash; dekomprimera och få identiska pixlar</td><td>Nej &mdash; varje förlustkomprimerad återkodning försämrar kvaliteten ytterligare</td></tr>
<tr><td>Use cases</td><td>Arkivering, diagnostik, juridiska journaler</td><td>Förhandsgranskning, telemedicin, nätverksöverföring</td></tr>
<tr><td>Supported codecs</td><td>JPEG Lossless, JPEG‑LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG‑LS Near‑Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>Viktigt:</strong> Att transkoda en förlustkomprimerad fil till en förlustfri syntax återställer inte förlorade data. Kvalitetsförlusten från den ursprungliga förlustkomprimeringen är permanent.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Källkod" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑referenser" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktstöd" tabId="support" >}}
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
