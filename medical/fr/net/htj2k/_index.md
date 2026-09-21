---
title: HTJ2K en C# .NET - JPEG 2000 à haut débit pour DICOM | Aspose.Medical
weight: 10000

description: Compressez et lisez les images DICOM en JPEG 2000 à haut débit depuis C#. HTJ2K sans perte, la variante RPCL et HTJ2K avec perte, implémentés en .NET géré sans codec natif à déployer.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K en .NET C#" h2="JPEG 2000 à haut débit pour DICOM : la compression que la norme a ajoutée pour les archives rapides et la visualisation cloud, implémentée en C# géré sans aucun composant natif à installer." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Ce que HTJ2K change">}}

<p>Le JPEG 2000 à haut débit conserve l'ondelettes et la qualité d'image du JPEG 2000 tout en remplaçant la partie qui le rendait lent. Le codeur de blocs est nouveau, et le décodage est environ dix fois plus rapide, ce qui explique pourquoi la norme DICOM l’a adopté dans trois syntaxes de transfert et pourquoi les plates‑formes d’imagerie cloud l’ont adoptée.</p>

<p>Pour une équipe .NET, la question pratique est différente : qui peut réellement produire ces fichiers ? La plupart des bibliothèques accèdent à HTJ2K via une construction native d’OpenJPH, ce qui implique un binaire par plateforme, une étape de construction dans le conteneur et une dépendance qui sera examinée lors de la révision de sécurité. <strong>Aspose.Medical for .NET</strong> implémente le codec en code géré à l’intérieur du même paquet qui lit et écrit les fichiers, de sorte que HTJ2K fonctionne de la même manière sous Windows, sous Linux et dans un conteneur, sans rien installer.</p>

<p>Trois syntaxes de transfert sont prises en charge, et les trois permettent à la fois la lecture et l’écriture :</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), JPEG 2000 à haut débit sans perte.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), la variante sans perte avec l'ordre de progression RPCL.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), JPEG 2000 à haut débit.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Compresser une étude en HTJ2K">}}

<p>Un appel déplace un fichier vers la nouvelle syntaxe. Le jeu de données, les balises privées et les métadonnées du fichier l’accompagnent.</p>

<div class="codeblock" id="code">
 <h3>Transcoder un fichier DICOM en HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>Sur une image 16‑bits de 1714 × 1933 provenant de notre jeu de tests, le fichier passe de 6,3 Mo à 2,9 Mo, et les pixels reviennent bit à bit. Les chiffres varient selon la modalité et l’image, il convient donc de mesurer sur vos propres données, ce qui consiste en une boucle sur les fichiers que vous possédez déjà.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sans perte signifie sans perte">}}

<p>Les données diagnostiques ne tolèrent pas un codec qui ne soit qu’approximatif. Transcodez en HTJ2K sans perte puis revenez en arrière, et les données de pixels sont identiques aux octets d’origine, ce qui est une propriété que vous pouvez vérifier dans votre propre suite de tests avant d’accepter de recomprimer une archive.</p>

<div class="codeblock" id="code">
 <h3>Retour à une syntaxe non compressée - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, la variante conçue pour la visualisation via un réseau">}}

<p>La syntaxe 1.2.840.10008.1.2.4.202 stocke le même flux de codage sans perte dans l’ordre de progression RPCL : résolution d’abord, puis position, puis composant, puis couche. Un lecteur qui ne lit que le début du flux obtient une image complète en basse résolution, ce dont un visualiseur a besoin lorsqu’il ouvre une grande étude via un lien qu’il ne contrôle pas.</p>

<div class="codeblock" id="code">
 <h3>Compresser avec l’ordre de progression RPCL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lire ce qu’une archive vous envoie">}}

<p>L’autre moitié du travail consiste à accepter le HTJ2K provenant de systèmes qui le produisent déjà. Ouvrez le fichier, vérifiez sous quel format il est stocké, et travaillez avec les données de pixels.</p>

<div class="codeblock" id="code">
 <h3>Lire un fichier HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Les images multi‑trames sont traitées trame par trame, de sorte qu’une longue série consomme de la mémoire par trame plutôt que par étude.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Où HTJ2K trouve sa place">}}

<ul>
<li>Migration d’archives : recomprimer une étude stockée en HTJ2K sans perte, réduire l’empreinte, conserver les données diagnostiques intactes.</li>
<li>Cloud et DICOMweb : la vitesse de décodage est ce qui rend un visualiseur côté navigateur ou côté serveur réactif sur les grandes images.</li>
<li>Chaînes d’IA : les ensembles d’entraînement sont lus bien plus souvent qu’ils ne sont écrits, et le temps de décodage est le coût qui se répète.</li>
<li>Conteneurs et serverless : le codec fait partie de l’assembly, ainsi une image n’a pas besoin d’une bibliothèque native ou d’un compilateur lors de la construction.</li>
</ul>

<p>La bibliothèque fournit également JPEG XL, l’autre ajout récent à la norme, ainsi que les codecs plus anciens qu’une archive est susceptible de contenir : JPEG, JPEG‑LS, JPEG 2000 et RLE. La page <a href="/medical/net/dicom-transfer-syntax-conversion/">conversion de syntaxes de transfert</a> couvre l’ensemble complet, et la page <a href="/medical/net/jpeg2000/">JPEG 2000</a> couvre le codec à l’origine du HTJ2K.</p>

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
{{< blocks/products/pf/slr-element name="Témoignages de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
