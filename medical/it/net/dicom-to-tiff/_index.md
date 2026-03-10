---
title: Converti DICOM in TIFF in C# .NET | Aspose.Medical
weight: 12000
description: Esegui il rendering delle immagini DICOM in dati pixel in C# .NET e salva in TIFF con supporto multi-pagina per file DICOM multi-frame. Utilizza l'intera pipeline LUT DICOM con l'API Aspose.Medical e salva in TIFF usando qualsiasi libreria di imaging .NET.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converti DICOM in TIFF in .NET C#" h2="Esegui il rendering delle immagini DICOM in dati pixel grezzi con supporto completo della pipeline in scala di grigi. Conserva la qualità dell'immagine con il controllo window/level, il rendering delle overlay e l'elaborazione multi-frame. Salva in TIFF &mdash; incluso TIFF multi-pagina &mdash; usando qualsiasi libreria di imaging .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Conversione da DICOM a TIFF">}}

<p><strong>Aspose.Medical per .NET</strong> esegue il rendering delle immagini DICOM attraverso la pipeline standard DICOM in scala di grigi (Modality LUT, VOI LUT, Presentation LUT, Output LUT) per produrre dati pixel BGRA32 correttamente windowed. TIFF è un formato versatile ampiamente utilizzato in imaging medico, ricerca scientifica e pubblicazione perché supporta compressione senza perdita, profondità di bit elevate e &mdash; soprattutto &mdash; <strong>documenti multi-pagina</strong>. Questo rende TIFF la scelta naturale per convertire file DICOM multi-frame (CT, MRI, ultrasuoni) in un unico file di output che conserva tutte le sezioni.</p>

<p>L'output renderizzato è un <code>IRawImage</code> &mdash; un buffer pixel BGRA32 con <code>Width</code>, <code>Height</code> e un array <code>Pixels</code>. È possibile salvare questi dati pixel in TIFF usando qualsiasi libreria di imaging .NET come <code>System.Drawing</code>, <code>SkiaSharp</code> o <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Esegui il rendering di DICOM in dati pixel in C#">}}

<p>Carica un file DICOM, esegui il rendering del frame desiderato e accedi ai dati pixel:</p>

<div class="codeblock" id="code">
 <h3>Esegui il rendering dell'immagine DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Salva l'immagine renderizzata come TIFF">}}

<p>L'<code>IRawImage</code> fornisce dati pixel grezzi BGRA32 che possono essere salvati in TIFF usando qualsiasi libreria di imaging .NET. Ecco un esempio che utilizza <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Salva il frame DICOM renderizzato come TIFF - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("xray.dcm");
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

// Save as TIFF
bitmap.Save("xray.tiff", ImageFormat.Tiff);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="TIFF multi-pagina da DICOM multi-frame">}}

<p>I file DICOM multi-frame (ad es., serie CT o MRI) contengono più sezioni d'immagine in un unico file. La capacità multi-pagina di TIFF consente di memorizzare tutti i frame in un unico file di output, preservando lo studio completo in un formato universalmente leggibile. Usa il codificatore TIFF di <code>System.Drawing</code> con parametri multi-frame:</p>

<div class="codeblock" id="code">
 <h3>Converti DICOM multi-frame in TIFF multi-pagina - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
int totalFrames = dicomFile.NumberOfFrames;

// Get the TIFF codec
var tiffCodec = ImageCodecInfo.GetImageEncoders()
    .First(c =&gt; c.FormatID == ImageFormat.Tiff.Guid);

// Multi-frame parameters
var multiFrameParams = new EncoderParameters(2);
multiFrameParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.MultiFrame);
multiFrameParams.Param[1] = new EncoderParameter(
    Encoder.Compression, (long)EncoderValue.CompressionLZW);

// Parameter for adding subsequent frames
var frameAddParams = new EncoderParameters(1);
frameAddParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.FrameDimensionPage);

// Parameter for flushing the final frame
var flushParams = new EncoderParameters(1);
flushParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.Flush);

Bitmap? tiffBitmap = null;

for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var frameBitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = frameBitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    frameBitmap.UnlockBits(bitmapData);

    if (i == 0)
    {
        // Save the first frame and start multi-page TIFF
        tiffBitmap = (Bitmap)frameBitmap.Clone();
        tiffBitmap.Save("ct_series.tiff", tiffCodec, multiFrameParams);
    }
    else
    {
        // Add subsequent frames
        tiffBitmap!.SaveAdd(frameBitmap, frameAddParams);
    }
}

// Flush and close the multi-page TIFF
tiffBitmap?.SaveAdd(flushParams);
tiffBitmap?.Dispose();</code></pre>
</div>

<p>Puoi anche salvare ogni frame come file TIFF separato quando sono necessarie sezioni individuali:</p>

<div class="codeblock" id="code">
 <h3>Converti ogni frame in un TIFF separato - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

for (int i = 0; i &lt; dicomFile.NumberOfFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var bitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = bitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    bitmap.UnlockBits(bitmapData);
    bitmap.Save($"frame_{i:D4}.tiff", ImageFormat.Tiff);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Window/Level personalizzato">}}

<p>Controlla quale intervallo di valori pixel viene renderizzato impostando il Window Center e il Window Width. Diverse impostazioni di window evidenziano differenti strutture anatomiche dallo stesso dato DICOM:</p>

<div class="codeblock" id="code">
 <h3>Esegui il rendering con window/level personalizzato - C#</h3>
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
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

<p>La classe <code>GrayscaleRenderOptions</code> espone anche le proprietà <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> e <code>BitDepth</code> per un controllo completo sulla pipeline di rendering.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendering DICOM multi-frame">}}

<p>I file DICOM da CT, MRI e ultrasuoni spesso contengono più frame (sezioni). Renderizza tutti i frame in dati pixel:</p>

<div class="codeblock" id="code">
 <h3>Renderizza tutti i frame da DICOM multi-frame - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as TIFF using your preferred imaging library
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
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG per immagini mediche">}}

<p>Ogni formato di output serve a diversi casi d'uso:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Caratteristica</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Compressione</strong></td><td>Senza perdita (LZW, ZIP) o non compresso</td><td>Senza perdita</td><td>Con perdita</td></tr>
<tr><td><strong>Multi-Pagina</strong></td><td>Supportato &mdash; tutti i frame in un unico file</td><td>Non supportato</td><td>Non supportato</td></tr>
<tr><td><strong>Qualità immagine</strong></td><td>Perfetto a livello di pixel</td><td>Perfetto a livello di pixel</td><td>Possibili artefatti di compressione</td></tr>
<tr><td><strong>Dimensione file</strong></td><td>La più grande</td><td>Media</td><td>La più piccola</td></tr>
<tr><td><strong>Trasparenza</strong></td><td>Supportata</td><td>Supportata</td><td>Non supportata</td></tr>
<tr><td><strong>Ideale per</strong></td><td>Archiviazione, serie multi-frame, stampa, ricerca</td><td>Revisione diagnostica, web di qualità</td><td>Condivisione web, miniature</td></tr>
</tbody>
</table>

<p>Aspose.Medical per .NET utilizza la stessa pipeline di rendering per tutti i target di output. I dati pixel <code>IRawImage</code> sono identici indipendentemente dal formato di destinazione — la scelta del formato influisce solo sul passaggio finale di salvataggio eseguito dalla tua libreria di imaging. TIFF è il formato preferito quando è necessario preservare uno studio DICOM completo multi-frame in un unico file o quando l'output è destinato ad archiviazione, stampa o ulteriori elaborazioni di immagine.</p>

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
