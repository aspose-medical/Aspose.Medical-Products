---
title: Convertir XML en DICOM en C# .NET | Aspose.Medical
weight: 5000

description: Générez des fichiers DICOM à partir du XML du modèle DICOM natif PS3.19 en C# .NET. Lisez le XML depuis une chaîne, un flux ou un pipe, diffusez des documents consécutifs et résolvez les références de données volumineuses avec l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convertir XML en DICOM en .NET C#" h2="Lisez le XML du modèle DICOM natif PS3.19 pour le reconvertir en jeux de données et fichiers DICOM. Travaillez à partir d’une chaîne, d’un flux ou d’un pipe, diffusez des documents consécutifs et résolvez les références de données volumineuses." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="XML du modèle DICOM natif standard">}}

<p><strong>Aspose.Medical for .NET</strong> lit le <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">modèle DICOM natif</a> défini dans le DICOM PS3.19. Il s’agit de la représentation XML inscrite dans la norme elle‑même, et non d’un format inventé par Aspose, ce qui la rend utile pour l’intégration : un système qui échange déjà le DICOM sous forme XML génère des documents que cette bibliothèque accepte.</p>

<p>L'élément racine du document est <code>NativeDicomModel</code>, et chaque attribut est un élément <code>DicomAttribute</code> contenant son tag, sa représentation de valeur et son mot‑clé :</p>

<div class="codeblock" id="code">
 <h3>Format du modèle DICOM natif</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>Cette page traite de la direction inverse de <a href="/medical/net/dicom-to-xml/">DICOM vers XML</a>, et les deux utilisent la même classe, <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Créer un fichier DICOM à partir d'XML en C#">}}

<p><code>Deserialize</code> transforme un document en <code>Dataset</code>, et un jeu de données est écrit sur le disque sous forme de fichier DICOM.</p>

<div class="codeblock" id="code">
 <h3>Créer un fichier DICOM à partir d'XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Le modèle DICOM natif ne comporte pas de groupe File Meta Information, de sorte que la syntaxe de transfert ne fait pas partie du document. Un jeu de données encapsulé dans un <code>DicomFile</code> est écrit avec la syntaxe de transfert par défaut, Implicit VR Little Endian. Pour stocker le fichier avec une autre syntaxe, transcodez‑le, comme le montre la page <a href="/medical/net/dicom-transfer-syntax-conversion/">conversion de syntaxe de transfert</a>.</p>

<p>La lecture du XML DICOM est une fonctionnalité sous licence. Sans licence locale appliquée, le lecteur génère une <code>MedicalApiException</code>, il faut donc appliquer la licence d’abord, comme le décrit le <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">guide de licence</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Flux, canaux et asynchrone">}}

<p>Chaque point d’entrée possède une surcharge de flux et une surcharge asynchrone, et les versions asynchrones acceptent également un <code>PipeReader</code>. Un document provenant d’une réponse web est analysé au fur et à mesure de sa lecture, sans être d’abord converti en chaîne.</p>

<div class="codeblock" id="code">
 <h3>Lire le XML depuis un flux - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Documents consécutifs dans un même flux">}}

<p>Une exportation d’un autre système contient souvent un élément <code>NativeDicomModel</code> après l’autre dans un seul flux. <code>DeserializeAsyncEnumerable</code> produit un jeu de données par élément, dans l’ordre d’entrée, de sorte que le flux est traité sans être chargé en mémoire. Les éléments se suivent directement : une déclaration XML n’est autorisée qu’au tout début, comme dans tout flux XML.</p>

<div class="codeblock" id="code">
 <h3>Diffuser des documents consécutifs - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Références de données volumineuses">}}

<p>Les valeurs volumineuses comme les données de pixels ne sont pas écrites en ligne. Elles apparaissent sous forme d’un élément <code>BulkData</code> avec une URI pointant vers les octets, ce qui garde le document léger. Pour résoudre ces références lors de la lecture, fournissez au sérialiseur un chargeur de données volumineuses. <code>DefaultBulkDataLoader</code> récupère les URI <code>file</code>, <code>http</code> et <code>https</code> sans authentification ; pour une archive nécessitant des informations d’identification, implémentez vous‑même <code>IBulkDataLoader</code> ou <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Résoudre les données volumineuses lors de la lecture - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Aller‑retour avec DICOM vers XML">}}

<p>Les deux directions sont conçues pour être utilisées ensemble : une étude part sous forme XML, traverse un système qui comprend le XML, puis revient sous forme de fichier DICOM. Tout est géré en .NET, ainsi le même aller‑retour fonctionne sous Windows, Linux et macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM vers XML et retour - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Pour les options qui contrôlent la forme du XML, consultez la page <a href="/medical/net/dicom-to-xml/">DICOM vers XML</a>. La même paire existe pour JSON : <a href="/medical/net/dicom-to-json/">DICOM vers JSON</a> et <a href="/medical/net/json-to-dicom/">JSON vers DICOM</a>. Le <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">guide de sérialisation</a> couvre l’ensemble de l’API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ressources d’apprentissage" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guide du développeur" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="Références API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Assistance produit" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Assistance gratuite" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Assistance payante" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Pourquoi choisir Aspose.Medical pour .NET ?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Liste des clients" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Histoires de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}