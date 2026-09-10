---
title: DICOM JPEG 2000-komprimering i C# .NET | Aspose.Medical
weight: 2000
description: Läs, skriv och transkoda DICOM-filer med JPEG 2000-komprimering i C# .NET. Stöd för 8-bitars färg- och 16-bitars monokroma bilder, förlustfri och förlustfulla lägen, samt HTJ2K med Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM JPEG 2000-stöd i .NET C#" h2="Läs, skriv och transkoda DICOM-filer med JPEG 2000-komprimering. Förlustfri och förlustfulla lägen, 8-bitars färg och 16-bitars monokroma pixeldata, HTJ2K inkluderat – allt i ren .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 inom medicinsk bildbehandling">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) är den mest använda vågformsbaserade komprimeringsstandarden inom medicinsk bildbehandling. Till skillnad från traditionell JPEG erbjuder den både förlustfri och förlustfull komprimering i en enda codec, progressiv avkodning för region‑of‑interest‑åtkomst och överlägsna komprimeringsförhållanden — vilket gör den idealisk för arkivering av stora studier och överföring av bilder över begränsade nätverk.</p>

<p><strong>Aspose.Medical for .NET</strong> tillhandahåller en ren C#-implementation av JPEG 2000-codec utan några inhemska beroenden. Biblioteket kan läsa, rendera och transkoda DICOM-filer komprimerade med någon av de fyra standard JPEG 2000-överföringssyntaxerna.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stödda JPEG 2000-överföringssyntaxer">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Överföringssyntax</th>
<th>UID</th>
<th>Läge</th>
<th>Läsa</th>
<th>Skriva</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 endast förlustfri</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Förlustfri</td><td>8-bitars RGB, 16-bitars monokrom</td><td>16-bitars monokrom, 8-bitars RGB</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Förlustfull eller förlustfri</td><td>8-bitars RGB, 16-bitars monokrom</td><td>16-bitars monokrom, 8-bitars RGB</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-komponent (endast förlustfri)</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Förlustfri</td><td>Stöds ej</td><td>Stöds ej</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-komponent</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Förlustfull eller förlustfri</td><td>Stöds ej</td><td>Stöds ej</td></tr>
<tr><td>HTJ2K (endast förlustfri)</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Förlustfri</td><td>Monokrom och färg</td><td>Monokrom och färg</td></tr>
<tr><td>HTJ2K med RPCL-alternativ (endast förlustfri)</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Förlustfri</td><td>Monokrom och färg</td><td>Monokrom och färg</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Förlustfull eller förlustfri</td><td>Monokrom och färg</td><td>Monokrom och färg</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8-bitars och 16-bitars pixeldata">}}

<p>Medicinska bilder använder ofta 16 bitar per prov för att fånga hela dynamiska omfånget för modaliteter som CT (vanligtvis 12‑bit lagrad i 16‑bit) och MRI. Aspose.Medical hanterar båda bithålderna för JPEG 2000:</p>

<ul>
<li><strong>Läsning (dekomprimering)</strong>: 16‑bit monokroma filer (CT, MRI, röntgen) och 8‑bit tre‑komponents färgfiler (RGB, YBR_RCT, YBR_ICT). Palett, CMYK, ICC‑profil och under‑samplade färgkodströmmar avvisas med ett tydligt undantag istället för en tyst felaktig bild.</li>
<li><strong>Skrivning (komprimering)</strong>: 16‑bit monokroma och 8‑bit RGB‑bilder. 8‑bit monokrom och 16‑bit färgkodning är inte tillgängliga; använd HTJ2K eller JPEG XL för dessa, båda accepterar monokrom och färg i båda bithöjderna.</li>
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

<p>Använd <code>Transcode</code>-metoden för att komprimera någon DICOM-fil till JPEG 2000 eller för att konvertera mellan JPEG 2000-lägen:</p>

<div class="codeblock" id="code">
 <h3>Komprimera DICOM till JPEG 2000 förlustfri - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Komprimera DICOM till JPEG 2000 förlustfull - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dekomprimera JPEG 2000 DICOM-filer">}}

<p>Dekomprimera JPEG 2000-filer till en okomprimerad överföringssyntax för bearbetning, analys eller kompatibilitet med system som inte stödjer JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Dekomprimera JPEG 2000 till okomprimerat - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Rendera JPEG 2000 DICOM-bilder">}}

<p>JPEG 2000-komprimerade DICOM-filer kan renderas till pixeldata för visning eller export, precis som alla andra överföringssyntaxer:</p>

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

{{< blocks/products/pf/feature-page-section h2="Förlustfri vs förlustfull JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Aspekt</th>
<th>JPEG 2000 förlustfri</th>
<th>JPEG 2000 förlustfull</th>
</tr>
</thead>
<tbody>
<tr><td>Överföringssyntax</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Bildkvalitet</td><td>Pixelperfekt &mdash; identisk med originalen</td><td>Visuellt liknande, viss data permanent förlorad</td></tr>
<tr><td>Komprimeringsförhållande</td><td>Vanligtvis 2:1 till 3:1</td><td>Vanligtvis 10:1 till 30:1 eller högre</td></tr>
<tr><td>Lämplig för</td><td>Diagnostisk arkivering, juridiska register, primär läsning</td><td>Preliminär granskning, telemedicin, nätverkstransmission</td></tr>
<tr><td>Säker för återkonvertering</td><td>Ja</td><td>Nej &mdash; återkodning försämrar kvaliteten ytterligare</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Höggenomströmning JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) ersätter den långsamma aritmetiska kodaren i JPEG 2000 med en snabbare blockkodare. Den behåller samma vågformstransform, progressionsordning och kvalitet, och avkodar samt kodar flera gånger snabbare. Aspose.Medical implementerar alla tre DICOM HTJ2K-överföringssyntaxer i ren .NET, för monokroma och färgade bilder, och transkoderar mellan HTJ2K och alla andra stödda syntaxer:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; endast förlustfri</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; förlustfri med RPCL-förloppsordning</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; förlustfull eller förlustfri</li>
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
{{< blocks/products/pf/slr-element name="API-referenser" href="https://reference.aspose.com/medical/net/" >}}
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
