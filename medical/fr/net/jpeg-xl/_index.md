---
title: JPEG XL pour DICOM en C# .NET | Aspose.Medical
weight: 10500

description: Stockez des images DICOM au format JPEG XL depuis C#. JPEG XL sans perte qui restitue les pixels bit à bit, dans une seule assembly gérée sans codec natif à déployer.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL pour DICOM en .NET C#" h2="La compression la plus récente du standard DICOM, avec les plus petits fichiers sans perte que nous ayons mesurés, implémentée en C# géré et fournie dans une seule assembly." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Pourquoi JPEG XL a atteint le DICOM">}}

<p>Les archives médicales croissent et ne rétrécissent jamais. JPEG XL est le codec que le monde de l'imagerie a conçu après deux décennies d'expérience avec JPEG et JPEG 2000, et le DICOM l'a ajouté comme syntaxe de transfert pour la raison qui importe aux équipes de stockage : pour les mêmes pixels, le fichier est plus petit.</p>

<p><strong>Aspose.Medical for .NET</strong> écrit et lit le JPEG XL via un port C# de libjxl intégré à la bibliothèque. Le package fournit une seule assembly, <code>Aspose.Medical.dll</code>, et aucun binaire natif à côté, de sorte qu'un codec aussi récent ne se transforme pas en projet de déploiement : la même assembly fonctionne sous Windows, Linux, sur un agent de build et dans un conteneur.</p>

<p>Deux syntaxes de transfert transportent les pixels :</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), pour les données diagnostiques qui doivent revenir inchangées.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), pour les cas où un fichier plus petit a plus d'importance qu'une copie exacte.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Compressez une étude, conservez chaque pixel">}}

<p>Le transcodage se fait en un seul appel, et l’ensemble de données autour des pixels l’accompagne.</p>

<div class="codeblock" id="code">
 <h3>Transcoder un fichier DICOM vers JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Nous l’avons mesuré sur une image 16 bits de 1714 × 1933 provenant de notre propre jeu de tests : 6,3 Mo non compressé devient 2,7 Mo en JPEG XL sans perte, ce qui est plus petit que la même image en HTJ2K sans perte. Vos propres chiffres dépendent de la modalité, il faut donc effectuer la comparaison sur un dossier de vos fichiers avant de choisir.</p>

<p>« Sans perte » doit être pris au pied de la lettre ici. Transcodez en JPEG XL puis revenez en arrière, et les données de pixels sont identiques aux octets de départ, ainsi une archive peut être recompressée sans discussion sur la qualité diagnostique.</p>

<div class="codeblock" id="code">
 <h3>Retour à une syntaxe non compressée - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lisez ce qui est déjà stocké en JPEG XL">}}

<p>Un fichier arrivé en JPEG XL s’ouvre comme tout autre. La syntaxe de transfert indique ce qu’il est, et les données de pixels sont disponibles dès que la trame est décodée.</p>

<div class="codeblock" id="code">
 <h3>Ouvrir un fichier JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL ou HTJ2K">}}

<p>Les deux sont récents, les deux offrent le mode sans perte lorsqu’il est demandé, et la bibliothèque les écrit et les lit tous les deux. Ils répondent à des questions différentes.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Question</th>
<th>Réponse</th>
</tr>
</thead>
<tbody>
<tr><td>Quel a produit le fichier le plus petit dans notre test</td><td>JPEG XL sans perte, de quelques pourcents</td></tr>
<tr><td>Quel est conçu pour la visualisation progressive sur un réseau</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, notamment la variante RPCL</td></tr>
<tr><td>Quel a été intégré en premier dans le standard DICOM</td><td>HTJ2K, donc davantage d’archives l’acceptent aujourd’hui</td></tr>
<tr><td>Lequel implique une dépendance native ici</td><td>Aucun, les deux sont du code géré dans une seule assembly</td></tr>
</tbody>
</table>

<p>Le choix provient généralement de l’autre côté du lien : transcodez vers la syntaxe acceptée par l’archive, et maintenez le reste du pipeline identique.</p>

<div class="codeblock" id="code">
 <h3>Laisser l’archive cible décider - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Où cela rapporte">}}

<ul>
<li>Archives à long terme : les mêmes études, moins de téraoctets, et aucune perte à justifier auprès d’un radiologue.</li>
<li>Factures de stockage cloud : l’économie se répète chaque mois, alors que le transcodage ne s’exécute qu’une fois.</li>
<li>Jeux de données pour la recherche et l’IA : les copies plus petites se déplacent plus rapidement entre le stockage et l’entraînement.</li>
<li>Déploiement : un codec aussi récent implique normalement une construction native par plate-forme ; ici il fait partie de l’assembly que vous référencez déjà.</li>
</ul>

<p>La bibliothèque écrit également les codecs dont une archive existante est remplie : JPEG, JPEG‑LS, JPEG 2000, HTJ2K et RLE. La page <a href="/medical/net/dicom-transfer-syntax-conversion/">conversion de syntaxe de transfert</a> couvre l’ensemble, <a href="/medical/net/htj2k/">HTJ2K</a> possède sa propre page, et <a href="/medical/net/jpeg2000/">JPEG 2000</a> est la source des deux nouveaux codecs.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ressources d’apprentissage" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guide du développeur" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="Références API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Support produit" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Support gratuit" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Support payant" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Pourquoi Aspose.Medical pour .NET ?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Liste des clients" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Histoires de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
