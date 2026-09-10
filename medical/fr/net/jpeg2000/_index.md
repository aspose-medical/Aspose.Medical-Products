---
title: Compression DICOM JPEG 2000 en C# .NET | Aspose.Medical
weight: 2000
description: Lire, écrire et transcoder des fichiers DICOM avec compression JPEG 2000 en C# .NET. Prise en charge des images couleur 8 bits et monochromes 16 bits, modes sans perte et avec perte, plus HTJ2K avec l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Prise en charge du DICOM JPEG 2000 en .NET C#" h2="Lire, écrire et transcoder des fichiers DICOM avec compression JPEG 2000. Modes sans perte et avec perte, données de pixels couleur 8 bits et monochromes 16 bits, HTJ2K inclus – le tout en .NET pur." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 en imagerie médicale">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) est la norme de compression basée sur les ondelettes la plus utilisée en imagerie médicale. Contrairement au JPEG traditionnel, il offre à la fois la compression sans perte et avec perte dans un seul codec, un décodage progressif pour l’accès aux régions d’intérêt, et des ratios de compression supérieurs &mdash; ce qui le rend idéal pour l’archivage de grandes études et la transmission d’images sur des réseaux à capacité limitée.</p>

<p><strong>Aspose.Medical for .NET</strong> fournit une implémentation pure C# du codec JPEG 2000 sans dépendances natives. La bibliothèque peut lire, rendre et transcoder des fichiers DICOM compressés avec l’une des quatre syntaxes de transfert JPEG 2000 standards.</p>

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
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Sans perte</td><td>RGB 8 bits, monochrome 16 bits</td><td>monochrome 16 bits, RGB 8 bits</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Avec perte ou sans perte</td><td>RGB 8 bits, monochrome 16 bits</td><td>monochrome 16 bits, RGB 8 bits</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Sans perte uniquement</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Sans perte</td><td>Non pris en charge</td><td>Non pris en charge</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Avec perte ou sans perte</td><td>Non pris en charge</td><td>Non pris en charge</td></tr>
<tr><td>HTJ2K Sans perte uniquement</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Sans perte</td><td>Monochrome et couleur</td><td>Monochrome et couleur</td></tr>
<tr><td>HTJ2K avec options RPCL Sans perte uniquement</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Sans perte</td><td>Monochrome et couleur</td><td>Monochrome et couleur</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Avec perte ou sans perte</td><td>Monochrome et couleur</td><td>Monochrome et couleur</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Données de pixels 8 bits et 16 bits">}}

<p>Les images médicales utilisent souvent 16 bits par échantillon pour capturer la gamme dynamique complète de modalités telles que le CT (généralement 12 bits stockés sur 16 bits) et l'IRM. Aspose.Medical gère les deux profondeurs de bits pour le JPEG 2000 :</p>

<ul>
<li><strong>Lecture (décompression)</strong> : fichiers monochromes 16 bits (CT, IRM, radiographie) et fichiers couleur à trois composantes 8 bits (RGB, YBR_RCT, YBR_ICT). Les flux de palettes, CMYK, profils ICC et les flux couleur sous-échantillonnés sont rejetés avec une exception claire plutôt qu’une image silencieusement incorrecte.</li>
<li><strong>Écriture (compression)</strong> : images monochromes 16 bits et images RGB 8 bits. L’encodage monochrome 8 bits et couleur 16 bits n’est pas disponible ; utilisez HTJ2K ou JPEG XL pour ces cas, les deux acceptent le monochrome et la couleur à n’importe quelle profondeur de bits.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Lire et inspecter les DICOM compressés JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Transcoder en JPEG 2000">}}

<p>Utilisez la méthode <code>Transcode</code> pour compresser n’importe quel fichier DICOM en JPEG 2000 ou pour convertir entre les modes JPEG 2000 :</p>

<div class="codeblock" id="code">
 <h3>Compresser un DICOM en JPEG 2000 sans perte - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Compresser un DICOM en JPEG 2000 avec perte - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Décompresser les fichiers DICOM JPEG 2000">}}

<p>Décompressez les fichiers JPEG 2000 vers une syntaxe de transfert non compressée pour le traitement, l’analyse ou la compatibilité avec des systèmes qui ne supportent pas le JPEG 2000 :</p>

<div class="codeblock" id="code">
 <h3>Décompresser le JPEG 2000 en non compressé - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Vous pouvez également décompresser et transcoder vers d’autres formats de compression en une seule étape :</p>

<div class="codeblock" id="code">
 <h3>Transcoder entre formats de compression - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendre les images DICOM JPEG 2000">}}

<p>Les fichiers DICOM compressés JPEG 2000 peuvent être rendus en données de pixels pour affichage ou export, comme toute autre syntaxe de transfert :</p>

<div class="codeblock" id="code">
 <h3>Rendre une trame compressée JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Sans perte vs Avec perte JPEG 2000">}}

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
<tr><td>Ratio de compression</td><td>Typiquement 2:1 à 3:1</td><td>Typiquement 10:1 à 30:1 ou plus</td></tr>
<tr><td>Idéal pour</td><td>Archivage diagnostique, dossiers légaux, lecture primaire</td><td>Examen préliminaire, télémédecine, transmission réseau</td></tr>
<tr><td>Sécurité du cycle complet</td><td>Oui</td><td>Non &mdash; le réencodage dégrade davantage la qualité</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 à haut débit (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) remplace le codeur arithmétique lent du JPEG 2000 par un codeur par blocs plus rapide. Il conserve la même transformation en ondelettes, les ordres de progression et la qualité, et décode et encode plusieurs fois plus rapidement. Aspose.Medical implémente les trois syntaxes de transfert DICOM HTJ2K en .NET pur, pour les images monochromes et couleur, et transcoder entre HTJ2K et toutes les autres syntaxes prises en charge :</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; sans perte uniquement</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; sans perte avec ordre de progression RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; avec perte ou sans perte</li>
</ul>

<div class="codeblock" id="code">
 <h3>Transcoder JPEG 2000 vers HTJ2K et retour - C#</h3>
 <pre><code class="cs">// Transcode a JPEG 2000 file to HTJ2K, and back to classic JPEG 2000
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile htj2kFile = j2kFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2kFile.Save("htj2k_lossless.dcm");

// HTJ2K with RPCL progression order, lossless
DicomFile rpclFile = j2kFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
rpclFile.Save("htj2k_rpcl.dcm");

// HTJ2K lossy
DicomFile htj2kLossy = j2kFile.Transcode(TransferSyntax.HTJ2K);
htj2kLossy.Save("htj2k_lossy.dcm");

// Any HTJ2K file decodes back to an uncompressed transfer syntax
DicomFile uncompressed = htj2kFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decoded.dcm");</code></pre>
</div>

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

{{< blocks/products/pf/slr-tab tabTitle="Pourquoi Aspose.Medical pour .NET ?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Liste des clients" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Histoires de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
