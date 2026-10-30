---
title: Compressione DICOM JPEG 2000 in C# .NET | Aspose.Medical
weight: 2000
description: Leggi, scrivi e transcodifica file DICOM con compressione JPEG 2000 in C# .NET. Supporto per immagini in bianco e nero e a colori da 1 a 16 bit, modalità lossless e lossy, oltre a HTJ2K con l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Supporto DICOM JPEG 2000 in .NET C#" h2="Leggi, scrivi e transcodifica file DICOM con compressione JPEG 2000. Modalità lossless e lossy, dati pixel in bianco e nero e a colori da 1 a 16 bit, HTJ2K incluso – tutto in puro .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 in Imaging Medico">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) è lo standard di compressione basato su wavelet più ampiamente utilizzato nell'imaging medico. Diversamente dal JPEG tradizionale, offre sia compressione lossless che lossy in un unico codec, decodifica progressiva per l'accesso a regioni di interesse e rapporti di compressione superiori &mdash; rendendolo ideale per archiviare studi di grandi dimensioni e trasmettere immagini su reti limitate.</p>

<p><strong>Aspose.Medical for .NET</strong> fornisce un'implementazione pura in C# del codec JPEG 2000 senza dipendenze native. La libreria può leggere, rendere e transcodificare file DICOM compressi con entrambe le sintassi di trasferimento JPEG 2000 Part 1, lossless e lossy.</p>

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
<tr><td>JPEG 2000 solo lossless</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>Monocromo e colore, da 1 a 16 bit</td><td>Monocromo, colore palette e RGB, da 1 a 16 bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy o lossless</td><td>Monocromo e colore, da 1 a 16 bit</td><td>Monocromo e RGB, da 1 a 16 bit</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Solo lossless</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Non supportato</td><td>Non supportato</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy o lossless</td><td>Non supportato</td><td>Non supportato</td></tr>
<tr><td>HTJ2K Solo lossless</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Monocromo e colore</td><td>Monocromo e colore</td></tr>
<tr><td>HTJ2K con opzioni RPCL Solo lossless</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Monocromo e colore</td><td>Monocromo e colore</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy o lossless</td><td>Monocromo e colore</td><td>Monocromo e colore</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dati pixel da 1 a 16 bit">}}

<p>Le immagini mediche spesso utilizzano 16 bit per campione per catturare l'intera gamma dinamica delle modalità come CT (tipicamente 12‑bit memorizzati in 16‑bit) e MRI. Aspose.Medical gestisce ogni profondità da 1 a 16 bit per JPEG 2000, in campioni a 8‑bit o 16‑bit:</p>

<ul>
<li><strong>Lettura (decompression)</strong>: file monocromatici (CT, MRI, X‑ray), file a colori palette e file a colori a tre componenti (RGB, YBR_RCT, YBR_ICT e altre etichette colore come YBR_FULL, decodificati come codificati) con 1‑16 bit per campione, signed o unsigned. CMYK, profilo ICC, palette JP2 e flussi di codice colore sottocampionati sono rifiutati con un'eccezione chiara invece di un'immagine errata silenziosa.</li>
<li><strong>Scrittura (compressione)</strong>: immagini monocromatiche e RGB con 1‑16 bit per campione, signed o unsigned, e immagini a colori palette con JPEG 2000 Lossless. Le immagini YBR non sono codificate a meno che non vengano decodificate in RGB, come avviene per i file JPEG YBR lossy.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Leggi e ispeziona DICOM compressi JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Transcodifica a JPEG 2000">}}

<p>Utilizza il metodo <code>Transcode</code> per comprimere qualsiasi file DICOM in JPEG 2000 o per convertire tra le modalità JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Comprimi DICOM a JPEG 2000 Lossless - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Comprimi DICOM a JPEG 2000 Lossy - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Decomprimi file DICOM JPEG 2000">}}

<p>Decomprimi file JPEG 2000 in una sintassi di trasferimento non compressa per l'elaborazione, l'analisi o la compatibilità con sistemi che non supportano JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Decomprimi JPEG 2000 in non compresso - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Puoi anche decomprimere e transcodificare in altri formati di compressione in un unico passo:</p>

<div class="codeblock" id="code">
 <h3>Transcodifica tra formati di compressione - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderizza immagini DICOM JPEG 2000">}}

<p>I file DICOM compressi JPEG 2000 possono essere renderizzati in dati pixel per visualizzazione o esportazione, proprio come qualsiasi altra sintassi di trasferimento:</p>

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
<tr><td>Qualità immagine</td><td>Pixel-perfect &mdash; identica all'originale</td><td>Visivamente simile, alcuni dati persi definitivamente</td></tr>
<tr><td>Rapporto di compressione</td><td>Tipicamente 2:1 a 3:1</td><td>Tipicamente 10:1 a 30:1 o superiore</td></tr>
<tr><td>Ideale per</td><td>Archivio diagnostico, documenti legali, lettura primaria</td><td>Revisione preliminare, telemedicina, trasmissione in rete</td></tr>
<tr><td>Sicuro in round‑trip</td><td>Sì</td><td>No &mdash; la ricodifica deteriora ulteriormente la qualità</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 ad alta velocità (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) sostituisce il lento codificatore aritmetico del JPEG 2000 con un codificatore a blocchi più veloce. Mantiene la stessa trasformata wavelet, gli ordini di progressione e la qualità, e decodifica e codifica diverse volte più rapidamente. Aspose.Medical implementa tutte e tre le sintassi di trasferimento DICOM HTJ2K in puro .NET, per immagini monocromatiche e a colori, e transcodifica tra HTJ2K e tutte le altre sintassi supportate:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; solo lossless</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; lossless con ordine di progressione RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; lossy o lossless</li>
</ul>

<div class="codeblock" id="code">
 <h3>Transcodifica JPEG 2000 in HTJ2K e viceversa - C#</h3>
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
