---
title: Converti DICOM in JPG in C# .NET | Aspose.Medical
weight: 7000
description: Esegui il rendering delle immagini DICOM in dati pixel in C# .NET con controllo window/level, rendering di overlay e supporto multiframe. Utilizza l'intera pipeline LUT DICOM con l'API Aspose.Medical e salva in JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converti DICOM in JPG in .NET C#" h2="Esegui il rendering delle immagini DICOM in dati pixel grezzi con supporto completo della pipeline in scala di grigi, includendo window/level, Modality LUT, VOI LUT e rendering di overlay. Salva in JPG usando qualsiasi libreria di imaging .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Pipeline di Rendering Immagine DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> esegue il rendering delle immagini DICOM mediante la pipeline standard in scala di grigi DICOM definita nella specifica DICOM. Il processo di rendering applica una catena di Look‑Up Tables (LUT) per trasformare i valori pixel grezzi memorizzati in immagini visualizzabili:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; converte i valori pixel memorizzati in valori indipendenti dal costruttore mediante Rescale Slope/Intercept o una LUT Sequence.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; applica window/level (Window Center e Window Width) per selezionare l'intervallo di valori da visualizzare, con supporto per trasformazioni Linear, Linear Exact e Sigmoid.</li>
<li><strong>Presentation LUT</strong> &mdash; mappa i valori finali in P‑Values visualizzabili, includendo il supporto per INVERSE (inversione dell’immagine).</li>
<li><strong>Output LUT</strong> &mdash; converte i valori in scala di grigi in RGB per la visualizzazione, con colorazione opzionale mediante mappe di colore personalizzate o tabelle Palette Color.</li>
</ol>

<p>L'output renderizzato è un <code>IRawImage</code> &mdash; un buffer di pixel BGRA32 (8 bit per canale) con <code>Width</code>, <code>Height</code> e un array <code>Pixels</code>. È quindi possibile salvare questi dati pixel in JPG utilizzando qualsiasi libreria di imaging .NET come <code>System.Drawing</code>, <code>SkiaSharp</code> o <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Esegui il rendering di DICOM in dati pixel in C#">}}

<p>Carica un file DICOM ed esegui il rendering di un frame in un <code>IRawImage</code> contenente dati pixel BGRA32:</p>

<div class="codeblock" id="code">
 <h3>Renderizza immagine DICOM - C#</h3>
 <pre><code class="cs">using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;

// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Render the first frame (index 0) through the full LUT pipeline
IRawImage image = dicomFile.RenderImage(0);

// Access the rendered pixel data
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = image.Pixels; // BGRA32 pixel buffer</code></pre>
</div>

<p>Il metodo <code>RenderImage</code> applica automaticamente l'intera pipeline in scala di grigi usando i valori window/level e i parametri LUT memorizzati nel file DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Salva immagine renderizzata come JPG">}}

<p>L'<code>IRawImage</code> fornisce dati pixel grezzi BGRA32 che possono essere salvati in JPG usando qualsiasi libreria di imaging .NET. Ecco un esempio che utilizza <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Salva frame DICOM renderizzato come JPG - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");
IRawImage image = dicomFile.RenderImage(0);

// Create a Bitmap from the BGRA32 pixel data
using var bitmap = new Bitmap(image.Width, image.Height, PixelFormat.Format32bppArgb);
var bitmapData = bitmap.LockBits(
    new Rectangle(0, 0, image.Width, image.Height),
    ImageLockMode.WriteOnly,
    PixelFormat.Format32bppArgb);

// Copy BGRA32 pixels to the bitmap
MemoryMarshal.AsBytes(image.Pixels.AsSpan())
    .CopyTo(new Span&lt;byte&gt;(
        (void*)bitmapData.Scan0,
        bitmapData.Stride * bitmapData.Height));

bitmap.UnlockBits(bitmapData);

// Save as JPG
bitmap.Save("ct_scan.jpg", ImageFormat.Jpeg);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Window/Level personalizzato (Windowing)">}}

<p>Window/level (detto anche windowing o W/L) è il parametro più importante per il rendering delle immagini DICOM. Controlla quale intervallo di valori pixel viene mappato nell'intervallo di scala di grigi visibile. Il <strong>Window Center</strong> definisce il punto centrale e il <strong>Window Width</strong> definisce l'intervallo di valori visualizzati:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; alto contrasto, meno livelli di grigio visualizzati (utile per i tessuti molli).</li>
<li><strong>Wide window</strong> &mdash; basso contrasto, più livelli di grigio visualizzati (utile per l'osso o i polmoni).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Renderizza con window/level personalizzato - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window: center=40, width=400
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window: center=400, width=1800
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Lung window: center=-600, width=1500
var lung = new GrayscaleRenderOptions
{
    WindowCenter = -600,
    WindowWidth = 1500
};
IRawImage lungImage = dicomFile.RenderImage(lung, 0);</code></pre>
</div>

<p>Predefiniti comuni di window per CT:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tipo di tessuto</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Tessuto molle</td><td>40</td><td>400</td></tr>
<tr><td>Polmone</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Osso</td><td>400</td><td>1800</td></tr>
<tr><td>Cervello</td><td>40</td><td>80</td></tr>
<tr><td>Fegato</td><td>60</td><td>150</td></tr>
<tr><td>Mediastino</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opzioni di Rendering in Scala di Grigi">}}

<p>La classe <code>GrayscaleRenderOptions</code> fornisce il controllo completo sulla pipeline di rendering in scala di grigi:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Proprietà</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Centro dell'intervallo di valori visualizzati (punto medio orizzontale del VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Larghezza dell'intervallo di valori visualizzati (controlla il contrasto)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>coefficiente di riscalatura del Modality LUT &mdash; moltiplicato per i valori pixel memorizzati</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>intercetta di riscalatura del Modality LUT &mdash; aggiunta dopo la moltiplicazione del coefficiente</td></tr>
<tr><td><code>Invert</code></td><td>Inverte l'immagine in scala di grigi (applica la forma INVERSE del Presentation LUT)</td></tr>
<tr><td><code>BitDepth</code></td><td>Informazioni sulla profondità di bit: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Rendering in scala di grigi invertita - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray.dcm");

// Render with inverted grayscale (white-on-black becomes black-on-white)
var options = new GrayscaleRenderOptions
{
    WindowCenter = 2048,
    WindowWidth = 4096,
    Invert = true
};

IRawImage image = dicomFile.RenderImage(options, 0);</code></pre>
</div>

<p>Utilizza <code>RenderOptionsFactory.Create(pixelData)</code> per popolare automaticamente le opzioni di rendering dagli attributi dei dati pixel del dataset DICOM, includendo profondità di bit, parametri di riscalatura, window/level e impostazioni di inversione.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendering di Overlay">}}

<p>Le immagini DICOM possono contenere piani di overlay &mdash; grafiche o annotazioni testuali incorporate nell'immagine. La classe <code>RenderOptions</code> controlla se gli overlay vengono renderizzati e di che colore appaiono:</p>

<div class="codeblock" id="code">
 <h3>Renderizza con controllo overlay - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible in white (default behavior)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions
{
    ShowOverlays = false
};
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

<p>DICOM supporta due tipi di overlay: overlay <strong>Graphics</strong> (annotazioni, misurazioni) e overlay <strong>Region of Interest</strong> (ROI). Entrambi i tipi sono controllati dall'impostazione <code>ShowOverlays</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendering DICOM Multiframe">}}

<p>Molti file DICOM provenienti da CT, MRI e ultrasuoni contengono più frame (fette). È possibile verificare il numero di frame e renderizzare ciascuno individualmente:</p>

<div class="codeblock" id="code">
 <h3>Renderizza tutti i frame da DICOM multiframe - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;
Console.WriteLine($"Total frames: {totalFrames}");

// Render all frames to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as JPG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tipi di trasformazione VOI LUT">}}

<p>Lo standard DICOM definisce diverse funzioni di trasformazione VOI LUT che controllano come viene applicata la mappatura window/level. Aspose.Medical per .NET supporta tutti i tipi standard:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
<th>Caso d'uso</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Mappatura lineare standard con offset dal centro</td><td>Più comune, usato per imaging CT/MR generico</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Mappatura lineare senza offset dal centro</td><td>Quando è richiesta una mappatura precisa dei valori pixel</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>Trasformazione a curva S per transizioni di contrasto più fluide</td><td>Visualizzazione di tessuti molli, mammografia</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>LUT personalizzata definita come tabella di lookup nel dataset DICOM</td><td>Curve di visualizzazione specifiche del fornitore o della modalità</td></tr>
</tbody>
</table>

<p>Il motore di rendering seleziona automaticamente la trasformazione VOI LUT appropriata in base agli attributi del dataset DICOM. Quando si utilizzano <code>GrayscaleRenderOptions</code> personalizzate, la trasformazione Linear viene applicata per impostazione predefinita.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Modalità supportate">}}

<p>Aspose.Medical per .NET supporta il rendering di immagini DICOM provenienti da tutte le comuni modalità di imaging medico:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modalità</th>
<th>Descrizione</th>
<th>Formato tipico</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Tomografia computerizzata</td><td>Grayscale, multi-frame, 12&ndash;16 bit</td></tr>
<tr><td><strong>MR</strong></td><td>Risonanza magnetica</td><td>Grayscale, multi-frame, 12&ndash;16 bit</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Radiografia computerizzata / digitale</td><td>Grayscale, single frame, 10&ndash;16 bit</td></tr>
<tr><td><strong>US</strong></td><td>Ecografia</td><td>Grayscale or RGB, multi-frame</td></tr>
<tr><td><strong>MG</strong></td><td>Mammografia</td><td>Grayscale, high resolution, 12&ndash;16 bit</td></tr>
<tr><td><strong>XA</strong></td><td>Angiografia a raggi X</td><td>Grayscale, multi-frame</td></tr>
<tr><td><strong>NM</strong></td><td>Medicina nucleare</td><td>Grayscale, multi-frame</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Tomografia a Emissione di Positroni)</td><td>Grayscale, multi-frame</td></tr>
</tbody>
</table>

<p>Sia le interpretazioni fotometriche in scala di grigi (MONOCHROME1/MONOCHROME2) sia quelle a colori (RGB, YBR_FULL, PALETTE COLOR) sono supportate. Il motore di rendering gestisce automaticamente la conversione appropriata dello spazio colore.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Risorse didattiche" tabId="resources" >}}
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
