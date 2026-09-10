---
title: DICOM JPEG 2000-compressie in C# .NET | Aspose.Medical
weight: 2000
description: Lees, schrijf en transcodeer DICOM‑bestanden met JPEG 2000-compressie in C# .NET. Ondersteuning voor 8‑bit‑ en 16‑bit‑afbeeldingen, verliesvrije en verlieslatende modi, multi‑componentgegevens met de Aspose.Medical‑API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM JPEG 2000-ondersteuning in .NET C#" h2="Lees, schrijf en transcodeer DICOM‑bestanden met JPEG 2000-compressie. Verliesvrije en verlieslatende modi, 8‑bit‑ en 16‑bit‑pixeldata, multi‑component‑afbeeldingen — allemaal in pure .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 in medische beeldvorming">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) is de meest gebruikte wavelet‑gebaseerde compressiestandaard in medische beeldvorming. In tegenstelling tot traditionele JPEG biedt het zowel verliesvrije als verlieslatende compressie in één codec, progressieve decoding voor region‑of‑interest‑toegang, en superieure compressieverhoudingen &mdash; waardoor het ideaal is voor het archiveren van grote studies en het verzenden van beelden via beperkte netwerken.</p>

<p><strong>Aspose.Medical for .NET</strong> levert een pure C#‑implementatie van de JPEG 2000‑codec zonder native afhankelijkheden. De bibliotheek kan DICOM‑bestanden lezen, renderen en transcoderen die gecomprimeerd zijn met een van de vier standaard JPEG 2000‑transfer syntaxisen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ondersteunde JPEG 2000‑transfer syntaxisen">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Transfersyntaxis</th>
<th>UID</th>
<th>Modus</th>
<th>Lezen</th>
<th>Schrijven</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 alleen verliesvrij</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Verliesvrij</td><td>8‑bit en 16‑bit</td><td>8‑bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Verlieslatend of verliesvrij</td><td>8‑bit en 16‑bit</td><td>8‑bit</td></tr>
<tr><td>JPEG 2000 Part 2 multi‑component alleen verliesvrij</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Verliesvrij</td><td>8‑bit en 16‑bit</td><td>8‑bit</td></tr>
<tr><td>JPEG 2000 Part 2 multi‑component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Verlieslatend of verliesvrij</td><td>8‑bit en 16‑bit</td><td>8‑bit</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8‑bit‑ en 16‑bit‑pixeldata">}}

<p>Medische beelden gebruiken vaak 16 bits per sample om het volledige dynamisch bereik van modaliteiten zoals CT (doorgaans 12‑bit opgeslagen in 16‑bit) en MRI vast te leggen. Aspose.Medical verwerkt beide bitdieptes voor JPEG 2000:</p>

<ul>
<li><strong>Lezen (decompressie)</strong>: Volledige ondersteuning voor zowel 8‑bit‑ als 16‑bit‑JPEG 2000‑gecomprimeerde DICOM‑bestanden. De bibliotheek decodeert pixeldata correct, ongeacht de oorspronkelijke Bits Allocated, Bits Stored en High Bit‑waarden.</li>
<li><strong>Schrijven (compressie)</strong>: Ondersteunt momenteel 8‑bit‑afbeeldingen. Ondersteuning voor 16‑bit‑schrijven is gepland voor een toekomstige release.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Lees en inspecteer JPEG 2000‑gecomprimeerde DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Transcodeer naar JPEG 2000">}}

<p>Gebruik de <code>Transcode</code>-methode om elk DICOM‑bestand te comprimeren naar JPEG 2000 of om te converteren tussen JPEG 2000‑modi:</p>

<div class="codeblock" id="code">
 <h3>Comprimeer DICOM naar JPEG 2000 verliesvrij - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Comprimeer DICOM naar JPEG 2000 verlieslatend - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Decomprimeer JPEG 2000 DICOM‑bestanden">}}

<p>Decompress JPEG 2000‑bestanden naar een ongecomprimeerde transfer syntaxis voor verwerking, analyse of compatibiliteit met systemen die JPEG 2000 niet ondersteunen:</p>

<div class="codeblock" id="code">
 <h3>Decompress JPEG 2000 naar ongecomprimeerd - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>U kunt ook decompressen en transcoderen naar andere compressieformaten in één stap:</p>

<div class="codeblock" id="code">
 <h3>Transcodeer tussen compressieformaten - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render JPEG 2000 DICOM‑afbeeldingen">}}

<p>JPEG 2000‑gecomprimeerde DICOM‑bestanden kunnen worden gerenderd naar pixeldata voor weergave of export, net als elke andere transfer syntaxis:</p>

<div class="codeblock" id="code">
 <h3>Render een JPEG 2000‑gecomprimeerd frame - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Verliesvrij vs verlieslatend JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Aspect</th>
<th>JPEG 2000 verliesvrij</th>
<th>JPEG 2000 verlieslatend</th>
</tr>
</thead>
<tbody>
<tr><td>Transfersyntaxis</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Beeldkwaliteit</td><td>Pixel‑perfect &mdash; identiek aan origineel</td><td>Visueel gelijk, sommige gegevens permanent verloren</td></tr>
<tr><td>Compressieverhouding</td><td>Typisch 2:1 tot 3:1</td><td>Typisch 10:1 tot 30:1 of hoger</td></tr>
<tr><td>Ideaal voor</td><td>Diagnostische archivering, juridische dossiers, primaire beoordeling</td><td>Voorlopige beoordeling, telemedicine, netwerkoverdracht</td></tr>
<tr><td>Rondreis veilig</td><td>Ja</td><td>Nee &mdash; hercodering vermindert de kwaliteit verder</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 multi‑component">}}

<p>JPEG 2000 Part 2 (ISO/IEC 15444-2) breidt de standaardcodec uit met multi‑component‑transformatiemogelijkheden. Dit wordt gebruikt voor gekleurde medische beelden en modaliteiten die meerkanaals data produceren. Aspose.Medical ondersteunt beide Part 2‑transfer syntaxisen:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; verliesvrije compressie met inter‑component decorrelatie voor optimale compressie van multi‑kanaal data.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; verlieslatende of verliesvrije compressie met multi‑component transformaties.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High‑Throughput JPEG 2000 (HTJ2K) — Binnenkort beschikbaar">}}

<p>HTJ2K (ISO/IEC 15444-15) is een next‑generation uitbreiding van JPEG 2000, ontworpen voor aanzienlijk snellere encode‑ en decode‑snelheden terwijl dezelfde compressie‑efficiëntie behouden blijft. Het wordt verwacht de voorkeurscodec te worden voor realtime medische‑beeldverwerkingsworkflows.</p>

<p>Aspose.Medical zal HTJ2K‑ondersteuning toevoegen in een toekomstige release, met drie transfer syntaxisen:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; Alleen verliesvrij</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; Verliesvrij met RPCL‑progressie‑volgorde</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; Verlieslatend of verliesvrij</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leerbronnen" tabId="resources" >}}
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
{{< blocks/products/pf/slr-element name="Klantenlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
