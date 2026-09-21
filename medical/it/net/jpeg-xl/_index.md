---
title: JPEG XL per DICOM in C# .NET | Aspose.Medical
weight: 10500

description: Memorizza le immagini DICOM in JPEG XL da C#. JPEG XL lossless che restituisce i pixel bit per bit, in un'unica assembly gestita senza codec nativo da distribuire.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL per DICOM in .NET C#" h2="La compressione più recente nello standard DICOM, con i file lossless più piccoli che abbiamo misurato, implementata in C# gestito e fornita all'interno di un'unica assembly." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Perché JPEG XL è stato introdotto in DICOM">}}

<p>Gli archivi medici crescono e non si riducono mai. JPEG XL è il codec che il mondo dell'imaging ha progettato dopo due decenni di esperienza con JPEG e JPEG 2000, e DICOM lo ha aggiunto come transfer syntax per il motivo a cui le squadre di archiviazione tengono: per gli stessi pixel, il file è più piccolo.</p>

<p><strong>Aspose.Medical per .NET</strong> scrive e legge JPEG XL attraverso una porta C# di libjxl che vive all'interno della libreria. Il pacchetto fornisce un'unica assembly, <code>Aspose.Medical.dll</code>, e nessun binario nativo accanto, così un codec così nuovo non si trasforma in un progetto di distribuzione: la stessa assembly funziona su Windows, su Linux, su un agente di build e in un container.</p>

<p>Due transfer syntax trasportano i pixel:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), per i dati diagnostici che devono ritornare invariati.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), per i casi in cui un file più piccolo conta più di una copia esatta.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Comprimi uno studio, conserva ogni pixel">}}

<p>Il transcodifica avviene con una singola chiamata, e il dataset intorno ai pixel viaggia con esso.</p>

<div class="codeblock" id="code">
 <h3>Transcodifica un file DICOM in JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Lo abbiamo misurato su un'immagine 16‑bit di 1714 × 1933 dal nostro set di test: 6,3 MB non compressi diventano 2,7 MB in JPEG XL lossless, più piccolo della stessa immagine in HTJ2K lossless. I tuoi valori dipendono dalla modalità, quindi esegui il confronto su una cartella dei tuoi file prima di scegliere.</p>

<p>Lossless è un termine da prendere alla lettera qui. Transcodifica in JPEG XL e ritorna indietro, e i dati dei pixel corrispondono ai byte con cui hai iniziato, così un archivio può essere ricompresso senza discussioni sulla qualità diagnostica.</p>

<div class="codeblock" id="code">
 <h3>Torna a una sintassi non compressa - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Leggi ciò che è già memorizzato come JPEG XL">}}

<p>Un file che arriva in JPEG XL si apre come qualsiasi altro. La transfer syntax indica di cosa si tratta, e i dati dei pixel sono disponibili una volta decodificato il frame.</p>

<div class="codeblock" id="code">
 <h3>Apri un file JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL o HTJ2K">}}

<p>Entrambi sono recenti, entrambi sono lossless quando chiedi lossless, e la libreria scrive e legge entrambi. Rispondono a domande diverse.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Domanda</th>
<th>Risposta</th>
</tr>
</thead>
<tbody>
<tr><td>Quale ha prodotto il file più piccolo nel nostro test</td><td>JPEG XL lossless, di qualche percento</td></tr>
<tr><td>Quale è concepito per la visualizzazione progressiva su rete</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, in particolare la variante RPCL</td></tr>
<tr><td>Quale è entrato per primo nello standard DICOM</td><td>HTJ2K, quindi più archivi lo accettano oggi</td></tr>
<tr><td>Quale richiede una dipendenza nativa qui</td><td>Nessuno, entrambi sono codice gestito in un'unica assembly</td></tr>
</tbody>
</table>

<p>La scelta di solito dipende dall'altro lato del collegamento: transcodifica nella sintassi accettata dall'archivio, mantenendo invariato il resto della pipeline.</p>

<div class="codeblock" id="code">
 <h3>Lascia decidere l'archivio di destinazione - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dove conviene">}}

<ul>
<li>Archivi a lungo termine: gli stessi studi, meno terabyte, e nessuna perdita da giustificare a un radiologo.</li>
<li>Fatture di storage cloud: il risparmio si ripete ogni mese, mentre la transcodifica avviene una sola volta.</li>
<li>Set di dati per ricerca e IA: copie più piccole si spostano più velocemente tra storage e addestramento.</li>
<li>Distribuzione: un codec così nuovo di solito implica una build nativa per piattaforma; qui è parte dell'assembly già referenziato.</li>
</ul>

<p>La libreria scrive anche i codec di cui è pieno un archivio esistente: JPEG, JPEG‑LS, JPEG 2000, HTJ2K e RLE. La pagina di <a href="/medical/net/dicom-transfer-syntax-conversion/">conversione della transfer syntax</a> copre l'intero set, <a href="/medical/net/htj2k/">HTJ2K</a> ha la sua pagina, e <a href="/medical/net/jpeg2000/">JPEG 2000</a> è da dove provengono entrambi i nuovi codec.</p>

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
