---
title: Compressione DICOM JPEG 2000 in C# .NET | Aspose.Medical
weight: 2000
description: Leggi, scrivi e transcodifica file DICOM con compressione JPEG 2000 in C# .NET. Supporto per immagini a 8-bit e 16-bit, modalità lossless e lossy, dati multicomponente con l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Supporto DICOM JPEG 2000 in .NET C#" h2="Leggi, scrivi e transcodifica file DICOM con compressione JPEG 2000. Modalità lossless e lossy, dati pixel a 8-bit e 16-bit, immagini multicomponente — tutto in puro .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 nell'imaging medico">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) è lo standard di compressione basato su wavelet più utilizzato nell'imaging medico. A differenza del JPEG tradizionale, offre compressione sia lossless che lossy in un unico codec, decodifica progressiva per l&rsquo;accesso a regioni di interesse e rapporti di compressione superiori &mdash; rendendolo ideale per l'archiviazione di studi di grandi dimensioni e la trasmissione di immagini su reti a larghezza limitata.</p>

<p><strong>Aspose.Medical for .NET</strong> fornisce un'implementazione pura in C# del codec JPEG 2000 senza dipendenze native. La libreria può leggere, renderizzare e transcodificare file DICOM compressi con una qualsiasi delle quattro sintassi di trasferimento standard JPEG 2000.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sintassi di trasferimento JPEG 2000 supportate">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Sintassi di trasferimento</th>
<th>UID</th>
<th>Modalità</th>
<th>Lettura</th>
<th>Scrittura</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>8-bit e 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy or lossless</td><td>8-bit e 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>8-bit e 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy or lossless</td><td>8-bit e 16-bit</td><td>8-bit</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dati pixel a 8-bit e 16-bit">}}

<p>Le immagini medicali spesso usano 16 bit per campione per catturare l'intera gamma dinamica delle modalità come CT (tipicamente 12-bit memorizzati in 16-bit) e MRI. Aspose.Medical gestisce entrambe le profondità di bit per JPEG 2000:</p>

<ul>
<li><strong>Lettura (decompressione)</strong>: Supporto completo per file DICOM compressi JPEG 2000 a 8-bit e 16-bit. La libreria decodifica correttamente i dati pixel indipendentemente dai valori originali di Bits Allocated, Bits Stored e High Bit.</li>
<li><strong>Scrittura (compressione)</strong>: Attualmente supporta immagini a 8-bit. Il supporto per la scrittura a 16-bit è previsto in una futura release.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Leggi e ispeziona DICOM compresso JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Transcodifica in JPEG 2000">}}

<p>Utilizza il metodo <code>Transcode</code> per comprimere qualsiasi file DICOM in JPEG 2000 o per convertire tra le modalità JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Comprimi DICOM in JPEG 2000 Lossless - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Comprimi DICOM in JPEG 2000 Lossy - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Decomprimi file DICOM JPEG 2000">}}

<p>Decomprimi i file JPEG 2000 in una sintassi di trasferimento non compressa per l'elaborazione, l'analisi o la compatibilità con sistemi che non supportano JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Decomprimi JPEG 2000 in non compresso - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Puoi anche decomprimere e transcodificare in altri formati di compressione in un unico passaggio:</p>

<div class="codeblock" id="code">
 <h3>Transcodifica tra formati di compressione - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderizza immagini DICOM JPEG 2000">}}

<p>I file DICOM compressi JPEG 2000 possono essere renderizzati in dati pixel per la visualizzazione o l'esportazione, proprio come qualsiasi altra sintassi di trasferimento:</p>

<div class="codeblock" id="code">
 <h3>Renderizza un frame compresso JPEG 2000 - C#</h3>
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
<th>Aspetto</th>
<th>JPEG 2000 Lossless</th>
<th>JPEG 2000 Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Sintassi di trasferimento</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Qualità immagine</td><td>Pixel-perfect &mdash; identico all'originale</td><td>Visivamente simile, alcuni dati persi permanentemente</td></tr>
<tr><td>Rapporto di compressione</td><td>Tipicamente 2:1 a 3:1</td><td>Tipicamente 10:1 a 30:1 o superiore</td></tr>
<tr><td>Ideale per</td><td>Archiviazione diagnostica, documentazione legale, lettura primaria</td><td>Revisione preliminare, telemedicina, trasmissione in rete</td></tr>
<tr><td>Sicuro round-trip</td><td>Sì</td><td>No &mdash; la ricodifica degrada ulteriormente la qualità</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 Multi-Component">}}

<p>JPEG 2000 Part 2 (ISO/IEC 15444-2) estende il codec standard con capacità di trasformazione multicomponente. Viene utilizzato per immagini medicali a colori e modalità che producono dati multicanale. Aspose.Medical supporta entrambe le sintassi di trasferimento Part 2:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; compressione lossless con decorrelazione inter-componenti per una compressione ottimale dei dati multicanale.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; compressione lossy o lossless con trasformazioni multicomponente.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K) — Prossimamente">}}

<p>HTJ2K (ISO/IEC 15444-15) è un'estensione di nuova generazione di JPEG 2000 progettata per velocità di codifica e decodifica drasticamente più rapide mantenendo la stessa efficienza di compressione. Si prevede che diventerà il codec preferito per i flussi di lavoro di imaging medico in tempo reale.</p>

<p>Aspose.Medical aggiungerà il supporto HTJ2K in una futura release, coprendo tre sintassi di trasferimento:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; Solo lossless</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; Lossless con ordine di progressione RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; lossy o lossless</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Risorse di apprendimento" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentazione" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Codice sorgente" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Riferimenti API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Supporto prodotto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Supporto gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Supporto a pagamento" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Perché Aspose.Medical per .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Elenco clienti" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Storie di successo" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
