---
title: Travailler avec de gros fichiers DICOM en C# .NET | Aspose.Medical
weight: 11500

description: Ouvrez des études multi‑cadres et des images de lames entières en C# sans les charger en mémoire. Lisez les métadonnées sans les données d’image, différerez les gros éléments et déplacez les fichiers via des flux et des pipelines.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Gros fichiers DICOM en .NET C#" h2="Lisez les métadonnées d’une étude multi‑cadre sans les pixels, différez les gros éléments jusqu’à ce qu’une requête les sollicite, et déplacez les fichiers entiers via des flux et des pipelines." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Le fichier est volumineux, la question est généralement petite">}}

<p>Une image de lame entière, une série CT longue ou un volume OCT font plusieurs centaines de mégaoctets, et la majeure partie est constituée de données de pixels. Le travail réel d’une application est souvent bien plus petit : lister le contenu d’un dossier, vérifier un identifiant patient, compter les cadres, décider où placer une étude. Charger chaque octet pour répondre à cela transforme une tâche simple en problème de mémoire.</p>

<p><strong>Aspose.Medical for .NET</strong> permet à l’appelant de décider quelle partie d’un fichier est lue. Le choix se fait via un argument de <code>DicomFile.Open</code>, et il s’applique de la même façon aux fichiers, flux et pipelines.</p>

<p>Mesuré sur une étude de 14 Mo contenant 128 cadres de notre jeu de tests, sur la même machine et le même fichier :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Stratégie de lecture</th>
<th>Temps d’ouverture</th>
<th>Mémoire allouée</th>
</tr>
</thead>
<tbody>
<tr><td>Tout, par défaut</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Éléments volumineux ignorés</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Éléments volumineux différés</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>L’écart augmente avec la taille du fichier. Un dossier contenant 10 000 études représente le cas où cela cesse d’être une micro‑optimisation.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lire les métadonnées, laisser les pixels intacts">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> exclut de la lecture chaque élément dépassant un seuil de taille. L’ensemble de données retourné ne contient que les tags nécessaires à un index ou à un routeur.</p>

<div class="codeblock" id="code">
 <h3>Lire une étude sans ses données de pixels - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>Le seuil vaut par défaut 64 kB et accepte une valeur en kilooctets, ainsi un workflow qui considère 8 kB comme grand peut le spécifier.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Différer au lieu d’ignorer">}}

<p>Lorsque les pixels peuvent être nécessaires, mais probablement plus tard et peut‑être pas tous, <code>ReadLargeOnDemand</code> constitue l’autre moitié de la paire. L’ouverture du fichier coûte autant qu’en ignorant, et un élément volumineux est lu au moment où le code y accède.</p>

<div class="codeblock" id="code">
 <h3>Charger un cadre uniquement lorsqu’il est utilisé - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>La lecture différée est une fonctionnalité sous licence ; les autres stratégies fonctionnent également en mode d’évaluation.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Indexer un dossier sans toucher aux pixels">}}

<p>La même stratégie s’applique à un flux, qui correspond à ce qu’une analyse d’archive ou un stockage d’objets cloud ressemble du point de vue du code.</p>

<div class="codeblock" id="code">
 <h3>Analyser une archive - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Flux et pipelines, entrées et sorties">}}

<p>La lecture et l’écriture acceptent toutes deux des flux, et les points d’entrée asynchrones acceptent également les types <code>System.IO.Pipelines</code>. Une étude peut circuler d’une réponse réseau au stockage sans que le processus ne détienne jamais le fichier complet sous forme d’un seul tableau.</p>

<div class="codeblock" id="code">
 <h3>Lire et écrire via des flux - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>Le même principe s’applique aux représentations textuelles : un document contenant de nombreux jeux de données est lu un jeu de données à la fois sur les pages <a href="/medical/net/json-to-dicom/">JSON vers DICOM</a> et <a href="/medical/net/xml-to-dicom/">XML vers DICOM</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Cadre par cadre">}}

<p>Les données multi‑cadres sont adressées cadre par cadre, ainsi une série de 500 cadres est traitée un cadre à la fois plutôt que l’intégralité de l’élément de données de pixels.</p>

<div class="codeblock" id="code">
 <h3>Parcourir les cadres - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Où cela détermine la conception">}}

<ul>
<li>Indexation et migration d’archives : des millions de fichiers, et seul l’en‑tête compte tant qu’aucun fichier n’est déplacé.</li>
<li>Routeurs et nœuds de stockage : accepter une étude, lire ce qui est nécessaire pour l’acheminer, transmettre les octets.</li>
<li>Chaînes d’IA : construire le manifeste à partir des métadonnées, puis extraire les cadres pour le sous‑ensemble réellement utilisé pour l’entraînement.</li>
<li>Conteneurs avec une limite de mémoire : le jeu de travail suit la stratégie, pas la taille du fichier.</li>
<li>Données de lames entières et OCT : fichiers pour lesquels la lecture totale n’est pas du tout envisageable.</li>
</ul>

<p>Le <a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">guide de gestion de mémoire</a> explique les stratégies en détail, et le <a href="/medical/net/dicom-networking/">réseau DICOM</a> montre les mêmes données arrivant via DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ressources d’apprentissage" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guide du développeur" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
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
