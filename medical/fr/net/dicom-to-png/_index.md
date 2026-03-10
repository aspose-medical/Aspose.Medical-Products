---
title: Convertir DICOM en PNG en C# .NET | Aspose.Medical
weight: 11000
description: Rendre les images DICOM en données de pixels en C# .NET avec une qualité sans perte, un contrôle window/level et la prise en charge du multi-cadre. Utilisez l’ensemble complet du pipeline LUT DICOM avec l’API Aspose.Medical et enregistrez en PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convertir DICOM en PNG en .NET C#" h2="Rendre les images DICOM en données de pixels brutes avec prise en charge complète du pipeline niveaux de gris. Préservez la qualité de l’image avec le contrôle window/level, le rendu des superpositions et le traitement multi-cadre. Enregistrez en PNG sans perte à l’aide de n’importe quelle bibliothèque d’imagerie .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Conversion DICOM vers PNG">}}

<p><strong>Aspose.Medical for .NET</strong> rend les images DICOM via le pipeline standard DICOM en niveaux de gris (Modality LUT, VOI LUT, Presentation LUT, Output LUT) pour produire des données de pixels BGRA32 correctement windowées. PNG est un format sans perte, ce qui en fait le choix privilégié lorsque la qualité de l’image doit être préservée sans artefacts de compression &mdash; essentiel pour la révision diagnostique, l’archivage et les cas d’utilisation en recherche.</p>

<p>Le rendu produit un <code>IRawImage</code> &mdash; un tampon de pixels BGRA32 contenant <code>Width</code>, <code>Height</code> et un tableau <code>Pixels</code>. Vous pouvez enregistrer ces données de pixels au format PNG à l’aide de n’importe quelle bibliothèque d’imagerie .NET telle que <code>System.Drawing</code>, <code>SkiaSharp</code> ou <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendre DICOM en données de pixels en C#">}}

<p>Chargez un fichier DICOM, rendez le cadre souhaité et accédez aux données de pixels :</p>

<div class="codeblock" id="code">
 <h3>Rendu de l'image DICOM - C#</h3>
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

<p>La méthode <code>RenderImage</code> applique automatiquement les valeurs window/level et les paramètres LUT stockés dans le jeu de données DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Enregistrer l'image rendue en PNG">}}

<p>Le <code>IRawImage</code> fournit des données de pixels BGRA32 brutes qui peuvent être enregistrées en PNG sans perte à l’aide de n’importe quelle bibliothèque d’imagerie .NET. Voici un exemple utilisant <code>System.Drawing</code> :</p>

<div class="codeblock" id="code">
 <h3>Enregistrer le cadre DICOM rendu en PNG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Window/Level personnalisé">}}

<p>Contrôlez la plage de valeurs de pixels rendue en définissant le Window Center et le Window Width. Différents réglages de window font apparaître différentes structures anatomiques à partir des mêmes données DICOM :</p>

<div class="codeblock" id="code">
 <h3>Rendu avec window/level personnalisé - C#</h3>
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

<p>La classe <code>GrayscaleRenderOptions</code> expose également les propriétés <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> et <code>BitDepth</code> pour un contrôle total du pipeline de rendu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendu DICOM multi‑cadre">}}

<p>Les fichiers DICOM provenant des CT, IRM et échographies contiennent souvent plusieurs cadres (coupes). Rendre tous les cadres en données de pixels :</p>

<div class="codeblock" id="code">
 <h3>Rendre tous les cadres d'un DICOM multi-cadre - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Contrôle des superpositions">}}

<p>Les images DICOM peuvent contenir des plans de superposition avec des annotations, mesures ou régions d’intérêt. Contrôlez la visibilité et la couleur des superpositions lors du rendu :</p>

<div class="codeblock" id="code">
 <h3>Rendu avec et sans superpositions - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG pour les images médicales">}}

<p>Le choix entre PNG et JPG dépend du cas d’utilisation :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Caractéristique</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Compression</strong></td><td>Sans perte</td><td>Avec perte</td></tr>
<tr><td><strong>Qualité d'image</strong></td><td>Pixel-perfect &mdash; aucun artefact</td><td>Petits artefacts de compression possibles</td></tr>
<tr><td><strong>Taille du fichier</strong></td><td>Plus grande (généralement 2&ndash;10× vs JPG)</td><td>Plus petite</td></tr>
<tr><td><strong>Transparence</strong></td><td>Supportée (canal alpha)</td><td>Non supportée</td></tr>
<tr><td><strong>Meilleur pour</strong></td><td>Révision diagnostique, archivage, recherche, superpositions</td><td>Partage web, vignettes, aperçus</td></tr>
<tr><td><strong>Réglementaire</strong></td><td>Préféré pour les flux de travail critiques en qualité</td><td>Acceptable pour un usage non diagnostique</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET utilise le même pipeline de rendu pour les deux cibles de sortie. Les données de pixels du <code>IRawImage</code> sont identiques quel que soit le format cible — le choix du format n’affecte que l’étape finale d’enregistrement effectuée par votre bibliothèque d’imagerie.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ressources d’apprentissage" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Code source" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Références API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Support produit" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Support gratuit" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Support payant" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Pourquoi Aspose.Medical pour .NET ?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Liste des clients" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Histoires de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
