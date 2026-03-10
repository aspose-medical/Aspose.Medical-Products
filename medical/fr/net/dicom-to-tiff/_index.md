---
title: Convertir DICOM en TIFF en C# .NET | Aspose.Medical
weight: 12000
description: Rendre les images DICOM en données de pixels en C# .NET et les enregistrer au format TIFF avec prise en charge multi-pages pour les fichiers DICOM multi‑frames. Utilisez la chaîne complète de LUT DICOM avec l'API Aspose.Medical et enregistrez au format TIFF à l'aide de n'importe quelle bibliothèque d'imagerie .NET.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convertir DICOM en TIFF en .NET C#" h2="Rendre les images DICOM en données de pixels brutes avec prise en charge complète de la chaîne de niveaux de gris. Préservez la qualité de l'image avec le contrôle fenêtre/niveau, le rendu des superpositions et le traitement multi‑frames. Enregistrez au format TIFF &mdash; y compris le TIFF multi‑pages &mdash; à l'aide de n'importe quelle bibliothèque d'imagerie .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Conversion DICOM vers TIFF">}}

<p><strong>Aspose.Medical for .NET</strong> rend les images DICOM via la chaîne standard de niveaux de gris DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT) pour produire des données de pixels BGRA32 correctement windowées. TIFF est un format polyvalent largement utilisé en imagerie médicale, recherche scientifique et édition parce qu'il prend en charge la compression sans perte, des profondeurs de bits élevées, et &mdash; surtout &mdash; <strong>les documents multi‑pages</strong>. Cela fait de TIFF le choix naturel pour convertir les fichiers DICOM multi‑frames (CT, MRI, ultrasound) en un fichier unique qui préserve toutes les coupes.</p>

<p>La sortie rendue est un <code>IRawImage</code> &mdash; un tampon de pixels BGRA32 avec <code>Width</code>, <code>Height</code> et un tableau <code>Pixels</code>. Vous pouvez enregistrer ces données de pixels au format TIFF à l'aide de n'importe quelle bibliothèque d'imagerie .NET telle que <code>System.Drawing</code>, <code>SkiaSharp</code> ou <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendre DICOM en données de pixels en C#">}}

<p>Chargez un fichier DICOM, rendez la trame souhaitée et accédez aux données de pixels :</p>

<div class="codeblock" id="code">
 <h3>Rendre une image DICOM – C#</h3>
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

<p>La méthode <code>RenderImage</code> applique automatiquement les valeurs fenêtre/niveau et les paramètres LUT stockés dans le jeu de données DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Enregistrer l'image rendue au format TIFF">}}

<p>L'objet <code>IRawImage</code> fournit des données de pixels brutes BGRA32 qui peuvent être enregistrées au format TIFF à l'aide de n'importe quelle bibliothèque d'imagerie .NET. Voici un exemple utilisant <code>System.Drawing</code> :</p>

<div class="codeblock" id="code">
 <h3>Enregistrer la trame DICOM rendue au format TIFF – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF multi‑pages à partir de DICOM multi‑frames">}}

<p>Les fichiers DICOM multi‑frames (p. ex., séries CT ou MRI) contiennent plusieurs coupes d'image dans un seul fichier. La capacité multi‑pages du TIFF vous permet de stocker toutes les trames dans un fichier de sortie unique, préservant l'étude complète dans un format universellement lisible. Utilisez l'encodeur TIFF de <code>System.Drawing</code> avec des paramètres multi‑frames :</p>

<div class="codeblock" id="code">
 <h3>Convertir le DICOM multi‑frame en TIFF multi‑pages – C#</h3>
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

<p>Vous pouvez également enregistrer chaque trame dans un fichier TIFF séparé lorsque des coupes individuelles sont nécessaires :</p>

<div class="codeblock" id="code">
 <h3>Convertir chaque trame en TIFF séparé – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Fenêtre/Niveau personnalisés">}}

<p>Contrôlez la plage de valeurs de pixels rendue en définissant le centre de fenêtre (Window Center) et la largeur de fenêtre (Window Width). Différents réglages de fenêtre font ressortir différentes structures anatomiques à partir des mêmes données DICOM :</p>

<div class="codeblock" id="code">
 <h3>Rendre avec fenêtre/niveau personnalisés – C#</h3>
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

<p>La classe <code>GrayscaleRenderOptions</code> expose également les propriétés <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> et <code>BitDepth</code> pour un contrôle complet de la chaîne de rendu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendu DICOM multi‑frame">}}

<p>Les fichiers DICOM issus du CT, du MRI et de l'échographie contiennent souvent plusieurs trames (coupes). Rendez toutes les trames en données de pixels :</p>

<div class="codeblock" id="code">
 <h3>Rendre toutes les trames d'un DICOM multi‑frame – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Contrôle des superpositions">}}

<p>Les images DICOM peuvent contenir des plans de superposition avec des annotations, mesures ou régions d'intérêt. Contrôlez la visibilité et la couleur des superpositions lors du rendu :</p>

<div class="codeblock" id="code">
 <h3>Rendre avec et sans superpositions – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG pour les images médicales">}}

<p>Chaque format de sortie répond à des cas d'utilisation différents :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Caractéristique</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Compression</strong></td><td>Sans perte (LZW, ZIP) ou non compressé</td><td>Sans perte</td><td>Avec perte</td></tr>
<tr><td><strong>Multi‑pages</strong></td><td>Pris en charge &mdash; toutes les trames dans un seul fichier</td><td>Non pris en charge</td><td>Non pris en charge</td></tr>
<tr><td><strong>Qualité d'image</strong></td><td>Pixel-perfect</td><td>Pixel-perfect</td><td>Artefacts de compression possibles</td></tr>
<tr><td><strong>Taille du fichier</strong></td><td>Le plus grand</td><td>Moyen</td><td>Le plus petit</td></tr>
<tr><td><strong>Transparence</strong></td><td>Pris en charge</td><td>Pris en charge</td><td>Non pris en charge</td></tr>
<tr><td><strong>Idéal pour</strong></td><td>Archivage, séries multi‑frames, impression, recherche</td><td>Révision diagnostique, web avec qualité</td><td>Partage Web, vignettes</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET utilise la même chaîne de rendu pour toutes les cibles de sortie. Les données de pixels <code>IRawImage</code> sont identiques quel que soit le format cible — le choix du format n’affecte que l’étape finale d’enregistrement effectuée par votre bibliothèque d’imagerie. Le TIFF est le format préféré lorsque vous devez préserver une étude DICOM multi‑frame complète dans un seul fichier ou lorsque la sortie est destinée à l’archivage, à l’impression ou à un traitement d’image supplémentaire.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ressources d'apprentissage" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Code source" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Références API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Support produit" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Support gratuit" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Support payant" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Pourquoi Aspose.Medical for .NET ?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Liste des clients" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Cas de succès" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
