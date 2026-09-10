---
title: Conversione della Transfer Syntax DICOM in C# .NET | Aspose.Medical
weight: 16000
description: Transcodifica file DICOM tra transfer syntax in C# .NET. Supporto per JPEG, JPEG 2000, JPEG-LS, RLE e formati non compressi con l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Conversione della Transfer Syntax DICOM in .NET C#" h2="Transcodifica file DICOM tra transfer syntax non compressi, JPEG, JPEG 2000, JPEG-LS e RLE. Libreria .NET pura senza dipendenze native." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Che cos'è la Transfer Syntax?">}}

<p>Una <strong>Transfer Syntax</strong> definisce come i dati DICOM sono codificati per l'archiviazione e la trasmissione. Specifica tre aspetti chiave: l'ordine dei byte (endianness), se le Value Representation sono esplicite o implicite, e l'algoritmo di compressione applicato ai dati dei pixel. Ogni file DICOM dichiara la sua transfer syntax nell'intestazione File Meta Information.</p>

<p>Diversi dispositivi medici, server PACS e applicazioni di visualizzazione supportano differenti insiemi di transfer syntax. <strong>Aspose.Medical for .NET</strong> offre il metodo <code>Transcode</code> per convertire tra transfer syntax, consentendo l'interoperabilità, l'ottimizzazione dell'archiviazione e la compatibilità con gli strumenti di elaborazione &mdash; il tutto in una libreria .NET pura senza dipendenze native.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transcodifica un file DICOM in C#">}}

<p>Il metodo <code>DicomFile.Transcode</code> converte un file DICOM dalla sua transfer syntax corrente a qualsiasi sintassi di destinazione supportata. Il metodo restituisce una nuova istanza di <code>DicomFile</code> &mdash; l'originale rimane invariato:</p>

<div class="codeblock" id="code">
 <h3>Trascode DICOM di base - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>È possibile anche transcodificare direttamente a livello di <code>Dataset</code>:</p>

<div class="codeblock" id="code">
 <h3>Transcodifica un Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transfer Syntax supportate">}}

<p>La tabella seguente elenca tutte le transfer syntax standard per i dati immagine DICOM e il loro attuale stato di supporto in Aspose.Medical for .NET. Tutti i codec supportati sono implementati interamente in C# e sono completamente indipendenti dalla piattaforma.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Tipo</th>
<th>Stato</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Non compresso</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Non compresso</td><td>Supportato</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Non compresso</td><td>Supportato</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Non compresso</td><td>Supportato</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Supportato</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Con perdita, 8-bit</td><td>Supportato</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Con perdita, 12-bit</td><td>Non supportato</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Lossless</td><td>Supportato (solo 8-bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Lossless</td><td>Supportato (solo 8-bit)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Lossless</td><td>Supportato</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Near-lossless</td><td>Supportato</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>Supportato (lettura 8/16-bit, scrittura 8-bit)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Con perdita o lossless</td><td>Supportato (lettura 8/16-bit, scrittura 8-bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Supportato (lettura 8/16-bit, scrittura 8-bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Con perdita o lossless</td><td>Supportato (lettura 8/16-bit, scrittura 8-bit)</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Lossless</td><td>Supportato</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Prossimamente</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Prossimamente</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Con perdita o lossless</td><td>Prossimamente</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Lossless</td><td>Prossimamente</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Lossless</td><td>Prossimamente</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Con perdita o lossless</td><td>Prossimamente</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Scenari comuni di transcodifica">}}

<p>Diversi flussi di lavoro richiedono diverse strategie di transcodifica. Ecco gli scenari più comuni:</p>

<div class="codeblock" id="code">
 <h3>Decomprimere per l'elaborazione - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Comprimere per l'archiviazione - C#</h3>
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
 <h3>Comprimere per la trasmissione in rete - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ispeziona le proprietà della Transfer Syntax">}}

<p>La classe <code>TransferSyntax</code> espone proprietà che descrivono le caratteristiche di codifica. Utilizzale per ispezionare la transfer syntax corrente di un file o per selezionare una sintassi di destinazione appropriata:</p>

<div class="codeblock" id="code">
 <h3>Leggi le proprietà della transfer syntax - C#</h3>
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
<th>Proprietà</th>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>L'identificatore unico della transfer syntax</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Indica se le Value Representation sono codificate esplicitamente</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Indica se l'ordine dei byte è little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Indica se i dati pixel sono incapsulati (compressi)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Indica se il metodo di compressione è con perdita</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Indica se la sintassi utilizza la compressione deflate</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Indica se la transfer syntax è ritirata dallo standard DICOM</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>L'identificatore ISO del metodo di compressione con perdita</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Compressione con perdita vs lossless">}}

<p>Comprendere la differenza tra compressione con perdita e lossless è fondamentale durante la transcodifica dei file DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Aspetto</th>
<th>Lossless</th>
<th>Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Qualità dell'immagine</td><td>Pixel-perfect &mdash; dati originali completamente preservati</td><td>Alcuni dati persi definitivamente per ottenere una dimensione più piccola</td></tr>
<tr><td>Rapporto di compressione</td><td>Tipicamente 2:1 a 3:1</td><td>Tipicamente 10:1 a 30:1 o superiore</td></tr>
<tr><td>Sicuro per round-trip</td><td>Sì &mdash; decomprimere e ottenere pixel identici</td><td>No &mdash; ogni nuova compressione con perdita degrada ulteriormente la qualità</td></tr>
<tr><td>Casi d'uso</td><td>Archiviazione, diagnostica, registri legali</td><td>Revisione preliminare, telemedicina, trasmissione in rete</td></tr>
<tr><td>Codec supportati</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>Importante:</strong> La transcodifica da un file compresso con perdita a una sintassi lossless non ripristina i dati persi. Il degrado di qualità dovuto alla compressione originale con perdita è permanente.</p>

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
