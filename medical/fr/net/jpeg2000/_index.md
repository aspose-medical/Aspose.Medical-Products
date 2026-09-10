---
title: Compression DICOM JPEG 2000 en C# .NET | Aspose.Medical
weight: 2000
description: Lisez, écrivez et transcodez des fichiers DICOM avec compression JPEG 2000 en C# .NET. Prise en charge des images 8 bits et 16 bits, des modes sans perte et avec perte, des données multi-composants avec l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Prise en charge DICOM JPEG 2000 en .NET C#" h2="Lisez, écrivez et transcodez des fichiers DICOM avec compression JPEG 2000. Modes sans perte et avec perte, données de pixels 8 bits et 16 bits, images multi-composants — le tout en pur .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 en imagerie médicale">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) est la norme de compression à base d'ondelettes la plus largement utilisée en imagerie médicale. Contrairement au JPEG traditionnel, elle offre à la fois la compression sans perte et avec perte dans un seul codec, un décodage progressif pour l'accès à des régions d'intérêt, et des ratios de compression supérieurs &mdash; ce qui la rend idéale pour l'archivage de grandes études et la transmission d'images sur des réseaux limités.</p>

<p><strong>Aspose.Medical for .NET</strong> fournit une implémentation pure C# du codec JPEG 2000 sans dépendances natives. La bibliothèque peut lire, rendre et transcoder des fichiers DICOM compressés avec l'une des quatre syntaxes de transfert standard JPEG 2000.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Syntaxes de transfert JPEG 2000 prises en charge">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Syntaxe de transfert</th>
<th>UID</th>
<th>Mode</th>
<th>Lecture</th>
<th>Écriture</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 sans perte uniquement</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Sans perte</td><td>8 bits et 16 bits</td><td>8 bits</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Avec perte ou sans perte</td><td>8 bits et 16 bits</td><td>8 bits</td></tr>
<tr><td>JPEG 2000 Part 2 multi-composant uniquement sans perte</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Sans perte</td><td>8 bits et 16 bits</td><td>8 bits</td></tr>
<tr><td>JPEG 2000 Part 2 multi-composant</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Avec perte ou sans perte</td><td>8 bits et 16 bits</td><td>8 bits</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Données de pixels 8 bits et 16 bits">}}

<p>Les images médicales utilisent souvent 16 bits par échantillon pour capturer toute la plage dynamique des modalités telles que le CT (généralement 12 bits stockés sur 16 bits) et l'IRM. Aspose.Medical gère les deux profondeurs de bits pour le JPEG 2000 :</p>

<ul>
<li><strong>Lecture (décompression)</strong> : prise en charge complète des fichiers DICOM compressés JPEG 2000 en 8 bits et 16 bits. La bibliothèque décode correctement les données de pixels quel que soit les valeurs d'origine de Bits Allocated, Bits Stored et High Bit.</li>
<li><strong>Écriture (compression)</strong> : prise en charge actuelle des images 8 bits. La prise en charge de l'écriture 16 bits est prévue pour une prochaine version.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Lisez et inspectez les DICOM compressés JPEG 2000 - C#</h3>
 <pre><code class="cs">// Open a JPEG 2000 compressed DICOM file (8-bit or 16-bit)
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Check transfer syntax
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax
Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");

// Inspect pixel data bit depth
PixelData pixelData = PixelData.Create(dicomFile.Dataset);
Console.WriteLine($"Bits Allocated: {pixelData.BitsAllocated}");
Console.WriteLine($"Bits Stored: {pixelData.BitsStored}");
Console.WriteLine($"High Bit: {pixelData.HighBit}");
Console.WriteLine($"Samples Per Pixel: {pixelData.SamplesPerPixel}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transcoder en JPEG 2000">}}

<p>Utilisez la méthode <code>Transcode</code> pour compresser n'importe quel fichier DICOM en JPEG 2000 ou pour convertir entre les modes JPEG 2000 :</p>

<div class="codeblock" id="code">
 <h3>Compresser un DICOM en JPEG 2000 sans perte - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Compresser un DICOM en JPEG 2000 avec perte - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Décompresser les fichiers DICOM JPEG 2000">}}

<p>Décompressez les fichiers JPEG 2000 vers une syntaxe de transfert non compressée pour le traitement, l'analyse ou la compatibilité avec des systèmes qui ne prennent pas en charge le JPEG 2000 :</p>

<div class="codeblock" id="code">
 <h3>Décompresser le JPEG 2000 en non compressé - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Vous pouvez également décompresser et transcoder vers d'autres formats de compression en une seule étape :</p>

<div class="codeblock" id="code">
 <h3>Transcoder entre formats de compression - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendre les images DICOM JPEG 2000">}}

<p>Les fichiers DICOM compressés JPEG 2000 peuvent être rendus en données de pixels pour l'affichage ou l'export, comme n'importe quelle autre syntaxe de transfert :</p>

<div class="codeblock" id="code">
 <h3>Rendre une trame compressée JPEG 2000 - C#</h3>
 <pre><code class="cs">// Open JPEG 2000 DICOM file
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Render the first frame — decompression is handled automatically
using PixelImage&lt;Bgra32&gt; image = dicomFile.RenderImage(0);

// Copy the BGRA32 pixel data for display or export
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = new Bgra32[width * height];
image.CopyPixelsTo(pixels);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 sans perte vs avec perte">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Aspect</th>
<th>JPEG 2000 sans perte</th>
<th>JPEG 2000 avec perte</th>
</tr>
</thead>
<tbody>
<tr><td>Syntaxe de transfert</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Qualité d'image</td><td>Pixel-perfect &mdash; identique à l'original</td><td>Visuellement similaire, certaines données perdues de façon permanente</td></tr>
<tr><td>Ratio de compression</td><td>Typiquement 2 :1 à 3 :1</td><td>Typiquement 10 :1 à 30 :1 ou plus</td></tr>
<tr><td>Idéal pour</td><td>Archivage diagnostique, dossiers légaux, lecture principale</td><td>Révision préliminaire, télémédecine, transmission réseau</td></tr>
<tr><td>Aller-retour sûr</td><td>Oui</td><td>Non &mdash; le ré-encodage dégrade davantage la qualité</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 multi-composant">}}

<p>JPEG 2000 Part 2 (ISO/IEC 15444-2) étend le codec standard avec des capacités de transformation multi‑composants. Il est utilisé pour les images médicales en couleur et les modalités qui produisent des données multi‑canaux. Aspose.Medical prend en charge les deux syntaxes de transfert Part 2 :</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; compression sans perte avec décorrélation inter‑composants pour une compression optimale des données multi‑canaux.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; compression avec perte ou sans perte avec des transformations multi‑composants.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 à haut débit (HTJ2K) — Bientôt disponible">}}

<p>HTJ2K (ISO/IEC 15444-15) est une extension de nouvelle génération du JPEG 2000 conçue pour des vitesses d'encodage et de décodage nettement plus rapides tout en conservant la même efficacité de compression. On s'attend à ce qu'il devienne le codec privilégié pour les flux de travail d'imagerie médicale en temps réel.</p>

<p>Aspose.Medical ajoutera la prise en charge de HTJ2K dans une version future, couvrant trois syntaxes de transfert :</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; Sans perte uniquement</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; Sans perte avec ordre de progression RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; Avec perte ou sans perte</li>
</ul>

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

{{< blocks/products/pf/slr-tab tabTitle="Pourquoi choisir Aspose.Medical pour .NET ?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Liste des clients" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Histoires de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
