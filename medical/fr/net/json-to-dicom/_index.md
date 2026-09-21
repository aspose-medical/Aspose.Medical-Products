---
title: Convertir JSON en DICOM en C# .NET | Aspose.Medical
weight: 6000

description: Générez des fichiers DICOM à partir du modèle JSON DICOM standard (PS3.18) en C# .NET. Lisez le JSON depuis une chaîne, un flux ou un pipe, diffusez une séquence de jeux de données et résolvez les références de données volumineuses avec l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convertir JSON en DICOM en .NET C#" h2="Lisez le modèle JSON DICOM standard (PS3.18) pour le reconvertir en jeux de données et fichiers DICOM. Travaillez depuis une chaîne, un flux ou un pipe, diffusez une séquence d'études et résolvez les références de données volumineuses." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Du JSON DICOM vers un fichier DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> lit le <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">modèle JSON DICOM PS3.18</a>, la représentation utilisée par les services DICOMweb et par les systèmes qui échangent des études via HTTP. Ce qui arrive sous forme de JSON devient un <code>Dataset</code>, et un <code>Dataset</code> est écrit sur le disque en tant que fichier DICOM.</p>

<p>Ceci est la direction inverse de la page <a href="/medical/net/dicom-to-json/">DICOM vers JSON</a>, et les deux utilisent la même classe, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Créer un fichier DICOM à partir de JSON - C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Un jeu de données qui ne porte aucune Information Méta de Fichier est écrit avec la syntaxe de transfert par défaut, Implicit VR Little Endian, lorsqu'il est encapsulé dans un <code>DicomFile</code>.</p>

<p>La lecture du JSON DICOM est une fonctionnalité sous licence. Sans licence on-premise appliquée, le lecteur lève une <code>MedicalApiException</code>, il faut donc appliquer la licence d’abord, comme le décrit le <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">guide de licence</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Conserver les Informations Méta de Fichier">}}

<p><code>Deserialize</code> renvoie uniquement le jeu de données. Lorsque le document JSON porte également le groupe d'Informations Méta de Fichier, par exemple parce qu’il a été généré à partir d’un fichier DICOM complet, <code>DeserializeFile</code> renvoie un <code>DicomFile</code> avec ce groupe intact, y compris la syntaxe de transfert déclarée par le fichier.</p>

<div class="codeblock" id="code">
 <h3>Lire un fichier DICOM complet depuis JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Flux, pipes et asynchrone">}}

<p>Chaque point d’entrée possède une surcharge de flux et une surcharge asynchrone, et les versions asynchrones acceptent également un <code>PipeReader</code>. Un document provenant d’une réponse web ou du disque est lu sans être d’abord converti en chaîne, ce qui est essentiel dès que le JSON contient des données de pixels.</p>

<div class="codeblock" id="code">
 <h3>Lire le JSON depuis un flux - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Une séquence de jeux de données, un à la fois">}}

<p>Une requête DICOMweb renvoie un tableau de jeux de données, et un tel document peut être volumineux. <code>DeserializeList</code> lit l’ensemble du tableau en mémoire ; <code>DeserializeAsyncEnumerable</code> fournit un jeu de données à la fois, de sorte que le document n’est jamais chargé en entier.</p>

<div class="codeblock" id="code">
 <h3>Diffuser un tableau de jeux de données - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Références de données volumineuses">}}

<p>Le modèle JSON DICOM ne transporte pas les données de pixels en ligne. Les valeurs volumineuses sont remplacées par un <code>BulkDataURI</code> qui pointe vers les octets, ce qui maintient le document JSON petit. Pour résoudre ces références lors de la lecture, fournissez au sérialiseur un chargeur de données volumineuses. <code>DefaultBulkDataLoader</code> récupère les URI <code>file</code>, <code>http</code> et <code>https</code> sans authentification ; pour une archive nécessitant des identifiants, implémentez vous‑même <code>IBulkDataLoader</code> ou <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Résoudre BulkDataURI lors de la lecture - C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Aller-retour avec DICOM vers JSON">}}

<p>Les deux directions sont conçues pour être utilisées ensemble : une étude part sous forme de JSON, transite par un service web, puis revient sous forme de fichier DICOM. Aucun élément du processus ne dépend de code natif, ainsi le même aller‑retour fonctionne sous Windows, Linux et macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM vers JSON et retour - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Pour les options qui contrôlent la forme du JSON, consultez la page <a href="/medical/net/dicom-to-json/">DICOM vers JSON</a>. La même paire existe pour XML : <a href="/medical/net/dicom-to-xml/">DICOM vers XML</a> et <a href="/medical/net/xml-to-dicom/">XML vers DICOM</a>. Le <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">guide de sérialisation JSON</a> couvre l’ensemble de l’API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ressources d’apprentissage" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guide du développeur" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="Références API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Support produit" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Support gratuit" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Support payant" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Pourquoi Aspose.Medical pour .NET ?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Liste des clients" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Cas de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}