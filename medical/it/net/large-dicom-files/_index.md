---
title: Lavora con grandi file DICOM in C# .NET | Aspose.Medical
weight: 11500

description: Apri studi multi‑frame e immagini whole slide in C# senza caricarli in memoria. Leggi i metadati senza i dati dei pixel, differisci gli elementi di grandi dimensioni e trasferisci i file tramite stream e pipe.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Grandi file DICOM in .NET C#" h2="Leggi i metadati di uno studio multi‑frame senza i pixel, differisci gli elementi di grandi dimensioni finché non vengono richiesti, e trasferisci interi file tramite stream e pipe." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Il file è grande, la domanda è solitamente piccola">}}

<p>Un'immagine whole slide, una lunga serie CT o un volume OCT sono centinaia di megabyte, e la maggior parte è costituita da dati pixel. Il lavoro effettivo di un'applicazione è spesso molto più piccolo: elencare il contenuto di una cartella, verificare un identificatore paziente, contare i frame, decidere dove collocare uno studio. Caricare ogni byte per rispondere a ciò è ciò che trasforma un compito semplice in un problema di memoria.</p>

<p><strong>Aspose.Medical per .NET</strong> consente al chiamante di decidere quanto di un file leggere. La scelta è un argomento su <code>DicomFile.Open</code>, e si applica allo stesso modo a file, stream e pipe.</p>

<p>Misurato su uno studio di 14 MB con 128 frame dal nostro set di test, sulla stessa macchina e lo stesso file:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Strategia di lettura</th>
<th>Tempo di apertura</th>
<th>Memoria allocata</th>
</tr>
</thead>
<tbody>
<tr><td>Tutto, predefinito</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Elementi di grandi dimensioni ignorati</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Elementi di grandi dimensioni differiti</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>Il divario cresce con il file. Una cartella con 10.000 studi è il caso in cui non è più una micro‑ottimizzazione.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Leggi i metadati, lascia intatti i pixel">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> esclude dalla lettura ogni elemento sopra una soglia di dimensione. Il dataset restituito contiene solo i tag di cui un indice o un router hanno bisogno.</p>

<div class="codeblock" id="code">
 <h3>Leggi uno studio senza i dati pixel - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>La soglia predefinita è 64 kB e accetta un valore in kilobyte, quindi un flusso di lavoro che considera 8 kB come grandi può indicarlo.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Differire invece di ignorare">}}

<p>Quando i pixel potrebbero essere necessari, ma probabilmente più tardi e non tutti, <code>ReadLargeOnDemand</code> è l’altra metà della coppia. L’apertura del file ha lo stesso costo dell’ignorare, e un elemento di grandi dimensioni viene letto nel momento in cui il codice lo tocca.</p>

<div class="codeblock" id="code">
 <h3>Carica un frame solo quando viene usato - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>La lettura differita è una funzionalità con licenza; le altre strategie funzionano anche in valutazione.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Indicizza una cartella senza toccare i pixel">}}

<p>La stessa strategia si applica a uno stream, che è ciò che una scansione di archivio o un archivio di oggetti cloud appare dal codice.</p>

<div class="codeblock" id="code">
 <h3>Scansiona un archivio - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stream e pipe, in entrata e in uscita">}}

<p>La lettura e la scrittura accettano entrambi stream, e i punti di ingresso asincroni accettano anche tipi <code>System.IO.Pipelines</code>. Uno studio può passare da una risposta di rete a memorizzazione senza che il processo trattenga mai l’intero file come un unico array.</p>

<div class="codeblock" id="code">
 <h3>Leggi e scrivi tramite stream - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>Lo stesso concetto si applica alle rappresentazioni testuali: un documento con molti dataset viene letto un dataset alla volta nelle pagine <a href="/medical/net/json-to-dicom/">JSON to DICOM</a> e <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Frame per frame">}}

<p>I dati multi‑frame sono gestiti per frame, così una serie di 500 frame costa un frame alla volta invece dell’intero elemento dei dati pixel.</p>

<div class="codeblock" id="code">
 <h3>Scorri i frame - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dove questo decide il design">}}

<ul>
<li>Indicizzazione e migrazione di archivi: milioni di file, e solo l’intestazione è rilevante finché qualcosa non viene spostato.</li>
<li>Router e nodi di archiviazione: accettano uno studio, leggono ciò che è necessario per instradarlo, trasmettono i byte.</li>
<li>Pipeline AI: costruisci il manifesto dai metadati, poi estrai i frame per il sotto‑insieme su cui si allena effettivamente.</li>
<li>Container con limite di memoria: il set di lavoro segue la strategia, non la dimensione del file.</li>
<li>Dati whole slide e OCT: file in cui leggere tutto non è affatto un’opzione.</li>
</ul>

<p>La <a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">guida alla gestione della memoria</a> spiega le strategie in dettaglio, e <a href="/medical/net/dicom-networking/">DICOM networking</a> mostra gli stessi dati che arrivano tramite DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Risorse di apprendimento" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentazione" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guida per sviluppatori" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
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
