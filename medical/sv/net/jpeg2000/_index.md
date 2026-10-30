---
title: DICOM JPEG 2000-komprimering i C# .NET | Aspose.Medical
weight: 2000
description: Läs, skriv och transkoda DICOM-filer med JPEG 2000-komprimering i C# .NET. Stöd för monokroma och färgade bilder från 1 till 16 bitar, förlustfri och förlustig läge, samt HTJ2K med Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM JPEG 2000-stöd i .NET C#" h2="Läs, skriv och transkoda DICOM-filer med JPEG 2000-komprimering. Förlustfri och förlustig läge, monokroma och färgade pixeldata från 1 till 16 bitar, HTJ2K inkluderat – allt i ren .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 inom medicinsk avbildning">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) är den mest använda vågformsbaserade komprimeringsstandarden inom medicinsk avbildning. Till skillnad från traditionell JPEG erbjuder den både förlustfri och förlustig komprimering i en enda codec, progressiv avkodning för region‑of‑interest‑åtkomst, och överlägsna komprimeringsförhållanden &mdash; vilket gör den idealisk för arkivering av stora studier och överföring av bilder över begränsade nätverk.</p>

<p><strong>Aspose.Medical for .NET</strong> erbjuder en ren C#-implementation av JPEG 2000-codec utan några inhemska beroenden. Biblioteket kan läsa, rendera och transkoda DICOM-filer komprimerade med både JPEG 2000 Part 1 transfer syntaxer, förlustfri och förlustig.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stödda JPEG 2000-transfer syntaxer">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Mode</th>
<th>Read</th>
<th>Write</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>Monokrom och färg, 1 till 16 bitar</td><td>Monokrom, palettfärg och RGB, 1 till 16 bitar</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy or lossless</td><td>Monokrom och färg, 1 till 16 bitar</td><td>Monokrom och RGB, 1 till 16 bitar</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Ej stöd</td><td>Ej stöd</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy or lossless</td><td>Ej stöd</td><td>Ej stöd</td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Monokrom och färg</td><td>Monokrom och färg</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Monokrom och färg</td><td>Monokrom och färg</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy or lossless</td><td>Monokrom och färg</td><td>Monokrom och färg</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pixeldata från 1 till 16 bitar">}}

<p>Medicinska bilder använder ofta 16 bitar per prov för att fånga hela det dynamiska omfånget hos modaliteter som CT (vanligtvis 12‑bit lagrad i 16‑bit) och MR. Aspose.Medical hanterar varje djup från 1 till 16 bitar för JPEG 2000, i 8‑bit eller 16‑bit prover:</p>

<ul>
<li><strong>Läsning (dekomprimering)</strong>: monokroma filer (CT, MRI, röntgen), palettfärg-filer och trekomponentfärg-filer (RGB, YBR_RCT, YBR_ICT, och andra färgetiketter såsom YBR_FULL, avkodade som kodade) med 1 till 16 bitar per prov, signerade eller osignerade. CMYK, ICC-profil, JP2-palette och underprovsade färgkodströmmar avvisas med ett tydligt undantag istället för en tyst felaktig bild.</li>
<li><strong>Skrivning (komprimering)</strong>: monokroma och RGB-bilder med 1 till 16 bitar per prov, signerade eller osignerade, samt palettfärg-bilder med JPEG 2000 Lossless. YBR-bilder kodas inte om de inte avkodas till RGB, som förlustiga JPEG YBR-filer gör.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Läs och inspektera JPEG 2000-komprimerad DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Transkoda till JPEG 2000">}}

<p>Använd metoden <code>Transcode</code> för att komprimera vilken DICOM-fil som helst till JPEG 2000 eller för att konvertera mellan JPEG 2000‑lägen:</p>

<div class="codeblock" id="code">
 <h3>Komprimera DICOM till JPEG 2000 Lossless - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Komprimera DICOM till JPEG 2000 Lossy - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dekomprimera JPEG 2000 DICOM-filer">}}

<p>Dekomprimera JPEG 2000-filer till en okomprimerad transfer syntax för bearbetning, analys eller kompatibilitet med system som inte stödjer JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Dekomprimera JPEG 2000 till okomprimerad - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Du kan även dekomprimera och transkoda till andra komprimeringsformat i ett enda steg:</p>

<div class="codeblock" id="code">
 <h3>Transkoda mellan komprimeringsformat - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendera JPEG 2000 DICOM‑bilder">}}

<p>JPEG 2000-komprimerade DICOM-filer kan renderas till pixeldata för visning eller export, precis som vilken annan transfer syntax som helst:</p>

<div class="codeblock" id="code">
 <h3>Rendera en JPEG 2000-komprimerad ram - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Lossless vs Lossy JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Aspekt</th>
<th>JPEG 2000 Lossless</th>
<th>JPEG 2000 Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Transfer Syntax</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Bildkvalitet</td><td>Pixelperfekt &mdash; identisk med originalen</td><td>Visuellt liknande, viss data permanent förlorad</td></tr>
<tr><td>Komprimeringsförhållande</td><td>Typiskt 2:1 till 3:1</td><td>Typiskt 10:1 till 30:1 eller högre</td></tr>
<tr><td>Bäst för</td><td>Diagnostisk arkivering, juridiska register, primär läsning</td><td>Preliminär granskning, telemedicin, nätverkstransmission</td></tr>
<tr><td>Round‑trip‑säker</td><td>Ja</td><td>Nej &mdash; omkodning försämrar kvaliteten ytterligare</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) ersätter den långsamma aritmetiska kodaren i JPEG 2000 med en snabbare blockkodare. Den behåller samma vågtransformation, progresionsordningar och kvalitet, och avkodar och kodar flera gånger snabbare. Aspose.Medical implementerar alla tre DICOM HTJ2K-transfer syntaxer i ren .NET, för monokroma och färgade bilder, och transkoderar mellan HTJ2K och alla andra stödda syntaxer:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; endast lossless</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; lossless med RPCL-progresionsordning</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; lossy eller lossless</li>
</ul>

<div class="codeblock" id="code">
 <h3>Transkoda JPEG 2000 till HTJ2K och tillbaka - C#</h3>
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
