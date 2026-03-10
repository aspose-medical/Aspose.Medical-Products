---
title: Converti DICOM in PNG in C# .NET | Aspose.Medical
weight: 11000
description: Esegui il rendering delle immagini DICOM in dati pixel in C# .NET con qualità lossless, controllo window/level e supporto multiframe. Usa l'intera pipeline DICOM LUT con l'API Aspose.Medical e salva in PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converti DICOM in PNG in .NET C#" h2="Esegui il rendering delle immagini DICOM in dati pixel grezzi con supporto completo della pipeline in scala di grigi. Preserva la qualità dell'immagine con controllo window/level, rendering delle overlay e elaborazione multiframe. Salva in PNG lossless usando qualsiasi libreria di imaging .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Conversione DICOM in PNG">}}

<p><strong>Aspose.Medical per .NET</strong> renderizza le immagini DICOM attraverso la pipeline standard DICOM in scala di grigi (Modality LUT, VOI LUT, Presentation LUT, Output LUT) per produrre dati pixel BGRA32 correttamente windowed. PNG è un formato lossless, rendendolo la scelta preferita quando la qualità dell'immagine deve essere preservata senza artefatti di compressione &mdash; essenziale per la revisione diagnostica, l'archiviazione e i casi d'uso di ricerca.</p>

<p>L'output renderizzato è un <code>IRawImage</code> &mdash; un buffer di pixel BGRA32 con <code>Width</code>, <code>Height</code> e un array <code>Pixels</code>. È possibile salvare questi dati pixel in PNG usando qualsiasi libreria di imaging .NET come <code>System.Drawing</code>, <code>SkiaSharp</code> o <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderizza DICOM in dati pixel in C#">}}

<p>Carica un file DICOM, renderizza il frame desiderato e accedi ai dati pixel:</p>

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

<p>Il metodo <code>RenderImage</code> applica automaticamente i valori window/level e i parametri LUT memorizzati nel dataset DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Salva immagine renderizzata come PNG">}}

<p>L'<code>IRawImage</code> fornisce dati pixel BGRA32 grezzi che possono essere salvati in PNG lossless usando qualsiasi libreria di imaging .NET. Ecco un esempio che utilizza <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Salva frame DICOM renderizzato come PNG - C#</h3>
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

// Save as lossless PNG
bitmap.Save("ct_scan.png", ImageFormat.Png);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Window/Level personalizzato">}}

<p>Controlla l'intervallo di valori pixel da renderizzare impostando Window Center e Window Width. Diverse impostazioni di window mostrano differenti strutture anatomiche dallo stesso dato DICOM:</p>

<div class="codeblock" id="code">
 <h3>Renderizza con window/level personalizzato - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);</code></pre>
</div>

<p>La classe <code>GrayscaleRenderOptions</code> espone anche le proprietà <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> e <code>BitDepth</code> per un controllo completo sulla pipeline di rendering.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendering DICOM multi-frame">}}

<p>I file DICOM provenienti da CT, MRI e ultrasuoni contengono spesso più frame (slice). Renderizza tutti i frame in dati pixel:</p>

<div class="codeblock" id="code">
 <h3>Renderizza tutti i frame da DICOM multi-frame - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as PNG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Controllo overlay">}}

<p>Le immagini DICOM possono contenere piani overlay con annotazioni, misurazioni o regioni di interesse. Controlla la visibilità e il colore dell'overlay durante il rendering:</p>

<div class="codeblock" id="code">
 <h3>Renderizza con e senza overlay - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible (default)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions { ShowOverlays = false };
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG per immagini medicali">}}

<p>Scegliere tra PNG e JPG dipende dal caso d'uso:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Caratteristica</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Compressione</strong></td><td>Lossless</td><td>Lossy</td></tr>
<tr><td><strong>Qualità immagine</strong></td><td>Pixel-perfect &mdash; nessun artefatto</td><td>Possibili piccoli artefatti di compressione</td></tr>
<tr><td><strong>Dimensione file</strong></td><td>Più grande (tipicamente 2&ndash;10x rispetto a JPG)</td><td>Più piccolo</td></tr>
<tr><td><strong>Trasparenza</strong></td><td>Supportata (canale alpha)</td><td>Non supportata</td></tr>
<tr><td><strong>Ideale per</strong></td><td>Revisione diagnostica, archiviazione, ricerca, overlay</td><td>Condivisione web, miniature, anteprime</td></tr>
<tr><td><strong>Regolamentare</strong></td><td>Preferito per flussi di lavoro critici sulla qualità</td><td>Accettabile per uso non diagnostico</td></tr>
</tbody>
</table>

<p>Aspose.Medical per .NET utilizza la stessa pipeline di rendering per entrambi i target di output. I dati pixel del <code>IRawImage</code> sono identici indipendentemente dal formato di destinazione &mdash; la scelta del formato influisce solo sul passaggio finale di salvataggio eseguito dalla tua libreria di imaging.</p>

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
