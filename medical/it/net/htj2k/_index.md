---
title: HTJ2K in C# .NET - JPEG 2000 ad alta velocità per DICOM | Aspose.Medical
weight: 10000

description: Comprimi e leggi immagini DICOM in JPEG 2000 ad alta velocità da C#. HTJ2K lossless, la variante RPCL e HTJ2K con perdita, implementati in .NET gestito senza codec nativo da distribuire.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K in .NET C#" h2="JPEG 2000 ad alta velocità per DICOM: la compressione che lo standard ha aggiunto per archivi rapidi e visualizzazione cloud, implementata in C# gestito senza nulla di nativo da installare." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Cosa cambia HTJ2K">}}

<p>Il JPEG 2000 ad alta velocità mantiene la wavelet e la qualità dell’immagine del JPEG 2000 e sostituisce la parte che lo rendeva lento. Il codificatore a blocchi è nuovo e la decodifica è di un ordine di grandezza più veloce, ed è per questo che lo standard DICOM lo ha adottato in tre sintassi di trasferimento e perché le piattaforme di imaging cloud lo hanno scelto.</p>

<p>Per un team .NET la domanda pratica è diversa: chi può realmente produrre quei file. La maggior parte delle librerie accede a HTJ2K tramite una build nativa di OpenJPH, il che comporta un binario per piattaforma, un passaggio di build nel container e una dipendenza su cui il controllo di sicurezza interrogherà. <strong>Aspose.Medical for .NET</strong> implementa il codec in codice gestito all’interno dello stesso pacchetto che legge e scrive i file, quindi HTJ2K funziona allo stesso modo su Windows, su Linux e in un container, senza necessità di installazione.</p>

<p>Sono supportate tre sintassi di trasferimento, e tutte e tre consentono sia lettura che scrittura:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), JPEG 2000 ad alta velocità lossless.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), la variante lossless con ordine di progressione RPCL.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), JPEG 2000 ad alta velocità.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Comprimi uno studio in HTJ2K">}}

<p>Una singola chiamata sposta un file nella nuova sintassi. Il dataset, i tag privati e le informazioni meta del file viaggiano con esso.</p>

<div class="codeblock" id="code">
 <h3>Transcodifica un file DICOM in HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>Su un’immagine 16‑bit di 1714 × 1933 dal nostro set di test, il file passa da 6,3 MB a 2,9 MB, e i pixel tornano identici bit per bit. I valori variano a seconda della modalità e dell’immagine, quindi misura sui tuoi dati, eseguendo un ciclo sui file che già possiedi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossless significa lossless">}}

<p>I dati diagnostici non tollerano un codec che sia solo quasi corretto. Transcodifica in HTJ2K lossless e ritorna indietro, e i dati pixel sono identici ai byte di partenza, proprietà che puoi verificare nella tua suite di test prima di accettare di ricomprimere un archivio.</p>

<div class="codeblock" id="code">
 <h3>Ritorna a una sintassi non compressa - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, la variante creata per la visualizzazione su rete">}}

<p>La sintassi 1.2.840.10008.1.2.4.202 memorizza lo stesso codestream lossless nell’ordine di progressione RPCL: prima la risoluzione, poi la posizione, quindi il componente e infine lo strato. Un lettore che prende solo l’inizio del flusso ottiene un’immagine a bassa risoluzione completa, ciò che un visualizzatore necessita quando apre un grande studio tramite un link non controllato.</p>

<div class="codeblock" id="code">
 <h3>Comprimi con l'ordine di progressione RPCL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Leggi quello che un archivio ti invia">}}

<p>L'altra metà del lavoro consiste nell’accettare HTJ2K da sistemi che lo producono già. Apri il file, verifica come è memorizzato e lavora con i dati pixel.</p>

<div class="codeblock" id="code">
 <h3>Leggi un file HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Le immagini multi‑frame vengono gestite frame per frame, così una lunga serie richiede memoria per frame anziché per studio.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dove HTJ2K guadagna il suo posto">}}

<ul>
<li>Migrazione archivio: ricomprimi uno studio memorizzato in HTJ2K lossless, riduci l’ingombro, mantieni intatti i dati diagnostici.</li>
<li>Cloud e DICOMweb: la velocità di decodifica è ciò che rende un visualizzatore lato browser o server immediato su immagini di grandi dimensioni.</li>
<li>Pipeline AI: i set di addestramento sono letti molto più spesso di quanto vengano scritti, e il tempo di decodifica è il costo ricorrente.</li>
<li>Container e serverless: il codec fa parte dell’assembly, quindi un’immagine non ha bisogno di una libreria nativa o di un compilatore nella build.</li>
</ul>

<p>La libreria include anche JPEG XL, l’altra recente aggiunta allo standard, e i codec più vecchi che un archivio potrebbe contenere: JPEG, JPEG‑LS, JPEG 2000 e RLE. La pagina di <a href="/medical/net/dicom-transfer-syntax-conversion/">conversione della sintassi di trasferimento</a> copre l’intero set, e la pagina <a href="/medical/net/jpeg2000/">JPEG 2000</a> descrive il codec da cui è nato HTJ2K.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Risorse di apprendimento" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentazione" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guida per sviluppatori" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
