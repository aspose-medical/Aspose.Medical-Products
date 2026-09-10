---
title: Conversion de la syntaxe de transfert DICOM en C# .NET | Aspose.Medical
weight: 16000
description: Transcoder des fichiers DICOM entre les syntaxes de transfert en C# .NET. Prise en charge du JPEG, JPEG 2000, JPEG-LS, RLE et des formats non compressés avec l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Conversion de la syntaxe de transfert DICOM en .NET C#" h2="Transcoder des fichiers DICOM entre les syntaxes de transfert non compressées, JPEG, JPEG 2000, JPEG-LS et RLE. Bibliothèque .NET pure sans dépendances natives." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Qu'est‑ce qu'une syntaxe de transfert ?">}}

<p>Une <strong>syntaxe de transfert</strong> définit la façon dont les données DICOM sont encodées pour le stockage et la transmission. Elle spécifie trois aspects clés : l'ordre des octets (endianness), si les représentations de valeurs sont explicites ou implicites, et l'algorithme de compression appliqué aux données d'image. Chaque fichier DICOM déclare sa syntaxe de transfert dans l'en-tête des métadonnées du fichier.</p>

<p>Différents appareils médicaux, serveurs PACS et applications de visualisation prennent en charge différents ensembles de syntaxes de transfert. <strong>Aspose.Medical for .NET</strong> fournit la méthode <code>Transcode</code> pour convertir entre les syntaxes de transfert, permettant l'interopérabilité, l'optimisation du stockage et la compatibilité avec les outils de traitement — le tout dans une bibliothèque .NET pure sans dépendances natives.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transcoder un fichier DICOM en C#">}}

<p>La méthode <code>DicomFile.Transcode</code> convertit un fichier DICOM de sa syntaxe de transfert actuelle vers n'importe quelle syntaxe cible prise en charge. La méthode renvoie une nouvelle instance <code>DicomFile</code> — l'original reste inchangé :</p>

<div class="codeblock" id="code">
 <h3>Transcodage DICOM de base - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Vous pouvez également transcoder directement au niveau du <code>Dataset</code> :</p>

<div class="codeblock" id="code">
 <h3>Transcoder un Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Syntaxes de transfert prises en charge">}}

<p>Le tableau ci‑dessous répertorie toutes les syntaxes de transfert d'images DICOM standard ainsi que leur état de prise en charge actuel dans Aspose.Medical pour .NET. Tous les codecs pris en charge sont implémentés en pur C# et sont totalement indépendants de la plateforme.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Syntaxe de transfert</th>
<th>UID</th>
<th>Type</th>
<th>Statut</th>
</tr>
</thead>
<tbody>
<tr><td colspan=\"4\"><strong>Non compressé</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Non compressé</td><td>Pris en charge</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Non compressé</td><td>Pris en charge</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Non compressé</td><td>Pris en charge</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Déflé</td><td>Pris en charge</td></tr>
<tr><td colspan=\"4\"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Avec perte, 8 bits</td><td>Pris en charge</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Avec perte, 12 bits</td><td>Non pris en charge</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Sans perte</td><td>Pris en charge (8 bits uniquement)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Sans perte</td><td>Pris en charge (8 bits uniquement)</td></tr>
<tr><td colspan=\"4\"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Sans perte</td><td>Pris en charge</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Quasi‑sans perte</td><td>Pris en charge</td></tr>
<tr><td colspan=\"4\"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Sans perte</td><td>Pris en charge (lecture 8/16 bits, écriture 8 bits)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Avec perte ou sans perte</td><td>Pris en charge (lecture 122 bits, écriture 8 bits)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Sans perte</td><td>Pris en charge (lecture 8/16 bits, écriture 8 bits)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Avec perte ou sans perte</td><td>Pris en charge (lecture 8/16 bits, écriture 8 bits)</td></tr>
<tr><td colspan=\"4\"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Sans perte</td><td>Pris en charge</td></tr>
<tr><td colspan=\"4\"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Sans perte</td><td>Bientôt disponible</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Sans perte</td><td>Bientôt disponible</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Avec perte ou sans perte</td><td>Bientôt disponible</td></tr>
<tr><td colspan=\"4\"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Sans perte</td><td>Bientôt disponible</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Sans perte</td><td>Bientôt disponible</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Avec perte ou sans perte</td><td>Bientôt disponible</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Scénarios courants de transcodage">}}

<p>Différents flux de travail nécessitent différentes stratégies de transcodage. Voici les scénarios les plus courants :</p>

<div class="codeblock" id="code">
 <h3>Déscompresser pour le traitement - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Compresser pour l'archivage - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Compresser pour la transmission réseau - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Inspecter les propriétés de la syntaxe de transfert">}}

<p>La classe <code>TransferSyntax</code> expose des propriétés qui décrivent les caractéristiques d'encodage. Utilisez‑les pour inspecter la syntaxe de transfert actuelle d'un fichier ou pour sélectionner une syntaxe cible appropriée :</p>

<div class="codeblock" id="code">
 <h3>Lire les propriétés de la syntaxe de transfert - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>Propriété</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>L’identifiant unique de la syntaxe de transfert</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Indique si les représentations de valeur sont encodées explicitement</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Indique si l'ordre des octets est little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Indique si les données d'image sont encapsulées (compressées)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Indique si la méthode de compression est avec perte</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Indique si la syntaxe utilise la compression deflate</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Indique si la syntaxe de transfert est retirée par la norme DICOM</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>L’identifiant selon la norme ISO de la méthode de compression avec perte</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Compression avec perte vs sans perte">}}

<p>Comprendre la différence entre compression avec perte et sans perte est essentiel lors du transcodage de fichiers DICOM :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Aspect</th>
<th>Sans perte</th>
<th>Avec perte</th>
</tr>
</thead>
<tbody>
<tr><td>Qualité d'image</td><td>Pixel‑perfect &mdash; données originales complètement préservées</td><td>Données perdues de façon permanente afin de réduire la taille</td></tr>
<tr><td>Ratio de compression</td><td>Typiquement 2 : 1 à 3 : 1</td><td>Typiquement 10 : 1 à 30 : 1 ou plus</td></tr>
<tr><td>Sécurité en aller‑retour</td><td>Oui &mdash; décompression restitue des pixels identiques</td><td>Non &mdash; chaque ré‑encodage avec perte dégrade davantage la qualité</td></tr>
<tr><td>Cas d'utilisation</td><td>Archivage, diagnostics, dossiers juridiques</td><td>Examen préliminaire, télémédecine, transmission réseau</td></tr>
<tr><td>Codecs pris en charge</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>Important :</strong> Le transcodage d’un fichier compressé avec perte vers une syntaxe sans perte ne restaure pas les données perdues. La dégradation de qualité provenant de la compression d'origine avec perte est permanente.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ressources d'apprentissage" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Code source" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Références API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Assistance produit" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Assistance gratuite" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Assistance payante" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Pourquoi Aspose.Medical pour .NET ?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Liste des clients" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Histoires de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
