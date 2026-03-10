---
title: Convertir DICOM en JPG en C# .NET | Aspose.Medical
weight: 7000
description: Rendre les images DICOM en données de pixels en C# .NET avec contrôle fenêtre/niveau, rendu des superpositions et prise en charge multi‑trames. Utilisez la chaîne complète de LUT DICOM avec l'API Aspose.Medical et enregistrez en JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convertir DICOM en JPG en .NET C#" h2="Rendre les images DICOM en données de pixels brutes avec prise en charge complète de la chaîne en niveaux de gris, incluant fenêtre/niveau, Modality LUT, VOI LUT et rendu des superpositions. Enregistrez en JPG à l'aide de n'importe quelle bibliothèque d'imagerie .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Pipeline de rendu d'image DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> rend les images DICOM via la chaîne standard en niveaux de gris DICOM définie dans la spécification DICOM. Le processus de rendu applique une chaîne de tables de consultation (LUT) pour transformer les valeurs de pixels stockées brutes en images affichables :</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; convertit les valeurs de pixels stockées en valeurs indépendantes du fabricant en utilisant Rescale Slope/Intercept ou une séquence LUT.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; applique la fenêtre/niveau (Window Center et Window Width) pour sélectionner la plage de valeurs à afficher, avec prise en charge des transformations Linear, Linear Exact et Sigmoid.</li>
<li><strong>Presentation LUT</strong> &mdash; mappe les valeurs finales en P-Values affichables, incluant la prise en charge d'INVERSE (inversion d'image).</li>
<li><strong>Output LUT</strong> &mdash; convertit les valeurs en niveaux de gris en RGB pour l'affichage, avec colorisation optionnelle à l'aide de cartes de couleur personnalisées ou de tables Palette Color.</li>
</ol>

<p>Le résultat rendu est un <code>IRawImage</code> &mdash; un tampon de pixels BGRA32 (8 bits par canal) avec <code>Width</code>, <code>Height</code> et un tableau <code>Pixels</code>. Vous pouvez ensuite enregistrer ces données de pixels au format JPG en utilisant n'importe quelle bibliothèque d'imagerie .NET telle que <code>System.Drawing</code>, <code>SkiaSharp</code> ou <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendre DICOM en données de pixels en C#">}}

<p>Chargez un fichier DICOM et rendez une tranche dans un <code>IRawImage</code> contenant des données de pixels BGRA32 :</p>

<div class="codeblock" id="code">
 <h3>Rendre l'image DICOM - C#</h3>
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

<p>La méthode <code>RenderImage</code> applique automatiquement la chaîne complète en niveaux de gris en utilisant les valeurs de fenêtre/niveau et les paramètres LUT stockés dans le fichier DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Enregistrer l'image rendue au format JPG">}}

<p>L'objet <code>IRawImage</code> fournit des données de pixels BGRA32 brutes qui peuvent être enregistrées au format JPG en utilisant n'importe quelle bibliothèque d'imagerie .NET. Voici un exemple utilisant <code>System.Drawing</code> :</p>

<div class="codeblock" id="code">
 <h3>Enregistrer la trame DICOM rendue au format JPG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Fenêtre/Niveau personnalisés (Windowing)">}}

<p>La fenêtre/niveau (également appelée windowing ou W/L) est le paramètre le plus important pour le rendu d'images DICOM. Elle contrôle la plage de valeurs de pixels qui est mappée à la plage de niveaux de gris visible. Le <strong>Window Center</strong> définit le point médian et le <strong>Window Width</strong> définit la plage de valeurs affichées :</p>

<ul>
<li><strong>Narrow window</strong> &mdash; contraste élevé, moins de niveaux de gris affichés (utile pour les tissus mous).</li>
<li><strong>Wide window</strong> &mdash; contraste moindre, plus de niveaux de gris affichés (utile pour les os ou les poumons).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Rendre avec fenêtre/niveau personnalisés - C#</h3>
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

<p>Préréglages de fenêtre CT courants :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Type de tissu</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Tissu mou</td><td>40</td><td>400</td></tr>
<tr><td>Poumon</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Os</td><td>400</td><td>1800</td></tr>
<tr><td>Cerveau</td><td>40</td><td>80</td></tr>
<tr><td>Foie</td><td>60</td><td>150</td></tr>
<tr><td>Médiastin</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Options de rendu en niveaux de gris">}}

<p>La classe <code>GrayscaleRenderOptions</code> offre un contrôle complet sur la chaîne de rendu en niveaux de gris :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Propriété</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Centre de la plage de valeurs affichées (point médian horizontal du VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Largeur de la plage de valeurs affichées (contrôle le contraste)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Modality LUT rescale slope &mdash; multipliée aux valeurs de pixels stockées</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Modality LUT rescale intercept &mdash; ajouté après la multiplication par la pente</td></tr>
<tr><td><code>Invert</code></td><td>Inverse l'image en niveaux de gris (applique la forme INVERSE du Presentation LUT)</td></tr>
<tr><td><code>BitDepth</code></td><td>Informations de profondeur de bits : BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Rendu en niveaux de gris inversé - C#</h3>
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

<p>Utilisez <code>RenderOptionsFactory.Create(pixelData)</code> pour remplir automatiquement les options de rendu à partir des attributs de données de pixels du jeu de données DICOM, incluant la profondeur de bits, les paramètres de rééchelle, la fenêtre/niveau et les réglages d'inversion.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendu des superpositions">}}

<p>Les images DICOM peuvent contenir des plans de superposition &mdash; graphiques ou annotations textuelles incorporées dans l'image. La classe <code>RenderOptions</code> contrôle si les superpositions sont rendues et de quelle couleur elles apparaissent :</p>

<div class="codeblock" id="code">
 <h3>Rendre avec contrôle des superpositions - C#</h3>
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

<p>DICOM prend en charge deux types de superpositions : les superpositions <strong>Graphics</strong> (annotations, mesures) et les superpositions <strong>Region of Interest</strong> (ROI). Les deux types sont contrôlés par le paramètre <code>ShowOverlays</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendu DICOM multi‑trames">}}

<p>De nombreux fichiers DICOM provenant de CT, MRI et échographie contiennent plusieurs trames (coupes). Vous pouvez vérifier le nombre de trames et rendre chacune individuellement :</p>

<div class="codeblock" id="code">
 <h3>Rendre toutes les trames d'un DICOM multi‑trames - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Types de transformation du VOI LUT">}}

<p>La norme DICOM définit plusieurs fonctions de transformation du VOI LUT qui contrôlent la façon dont le mappage fenêtre/niveau est appliqué. Aspose.Medical for .NET prend en charge tous les types standards :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Type</th>
<th>Description</th>
<th>Cas d'utilisation</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Mappage linéaire standard avec décalage depuis le centre</td><td>Le plus courant, utilisé pour l'imagerie CT/MR générale</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Mappage linéaire sans décalage depuis le centre</td><td>Lorsque un mappage précis des valeurs de pixels est requis</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>Transformation en courbe en S pour des transitions de contraste plus douces</td><td>Visualisation des tissus mous, mammographie</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>LUT personnalisé défini comme table de consultation dans le jeu de données DICOM</td><td>Courbes d'affichage spécifiques au fabricant ou à la modalité</td></tr>
</tbody>
</table>

<p>Le moteur de rendu sélectionne automatiquement la transformation VOI LUT appropriée en fonction des attributs du jeu de données DICOM. Lors de l'utilisation de <code>GrayscaleRenderOptions</code> personnalisées, la transformation Linear est appliquée par défaut.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Modalités prises en charge">}}

<p>Aspose.Medical for .NET prend en charge le rendu d'images DICOM provenant de toutes les modalités d'imagerie médicale courantes :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modalité</th>
<th>Description</th>
<th>Format typique</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Tomodensitométrie</td><td>Grayscale, multi-frame, 12&ndash;16 bit</td></tr>
<tr><td><strong>MR</strong></td><td>Résonance magnétique</td><td>Grayscale, multi-frame, 12&ndash;16 bit</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Radiographie Computée / Numérique</td><td>Grayscale, single frame, 10&ndash;16 bit</td></tr>
<tr><td><strong>US</strong></td><td>Échographie</td><td>Grayscale or RGB, multi-frame</td></tr>
<tr><td><strong>MG</strong></td><td>Mammographie</td><td>Grayscale, high resolution, 12&ndash;16 bit</td></tr>
<tr><td><strong>XA</strong></td><td>Angiographie X‑Ray</td><td>Grayscale, multi-frame</td></tr>
<tr><td><strong>NM</strong></td><td>Médecine nucléaire</td><td>Grayscale, multi-frame</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Tomographie par Émission de Positrons)</td><td>Grayscale, multi-frame</td></tr>
</tbody>
</table>

<p>Les deux interprétations photométriques en niveaux de gris (MONOCHROME1/MONOCHROME2) et en couleur (RGB, YBR_FULL, PALETTE COLOR) sont prises en charge. Le moteur de rendu gère automatiquement la conversion appropriée de l'espace couleur.</p>

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

{{< blocks/products/pf/slr-tab tabTitle="Pourquoi Aspose.Medical pour .NET ?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Liste des clients" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Histoires de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
