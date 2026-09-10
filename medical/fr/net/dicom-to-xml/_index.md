---
title: Convertir DICOM en XML en C# .NET | Aspose.Medical
weight: 3000
description: Sérialiser des ensembles de données DICOM au format DICOM XML standard en C# .NET. Configurez la gestion des Bulk Data, le traitement basé sur les flux et les opérations asynchrones avec l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convertir DICOM en XML en .NET C#" h2="Sérialiser les ensembles de données DICOM vers la représentation DICOM XML standard (PS3.19). Configurez les références de Bulk Data, la sortie basée sur les flux et le traitement asynchrone avec une bibliothèque pure .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Sérialisation DICOM XML basée sur les normes">}}

<p><strong>Aspose.Medical pour .NET</strong> sérialise les données DICOM en XML en suivant le <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Modèle DICOM natif PS3.19</a>. Il s'agit de la norme officielle pour représenter les ensembles de données DICOM en XML, utilisée par les services DICOMweb, les plateformes d'intégration et les systèmes qui nécessitent une représentation lisible par l'homme et validée par un schéma des métadonnées d'imagerie médicale.</p>

<p>La classe <code>DicomXmlSerializer</code> fournit des méthodes statiques pour la sérialisation et la désérialisation. Contrairement aux approches simples de vidage de balises, la sortie respecte le schéma DICOM XML où chaque élément est représenté avec son tag, son VR et des valeurs correctement formatées &mdash; permettant une conversion sans perte aller-retour entre le DICOM binaire et le XML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sérialiser DICOM en XML en C#">}}

<p>Utilisez la classe <code>DicomXmlSerializer</code> pour convertir un ensemble de données DICOM en chaîne XML. L'approche la plus simple produit un document XML conforme aux normes :</p>

<div class="codeblock" id="code">
 <h3>Convertir DICOM en XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sérialisation basée sur les flux et asynchrone">}}

<p>Pour les fichiers DICOM volumineux ou les scénarios à haut débit, sérialisez directement vers un flux afin d'éviter d'allouer de grandes chaînes en mémoire. Des méthodes synchrones et asynchrones sont disponibles :</p>

<div class="codeblock" id="code">
 <h3>Sérialisation de flux synchrone - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Sérialisation de flux asynchrone - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transmission en pipeline pour les études volumineuses">}}

<p>Les études complètes n'ont pas besoin d'être entièrement chargées en mémoire. <code>DicomXmlSerializer</code> écrit vers un <code>PipeWriter</code> et lit depuis un <code>PipeReader</code>, de sorte que le XML peut être produit et consommé au fil de son flux, et qu'une séquence d'ensembles de données peut être lue une à la fois via <code>DeserializeAsyncEnumerable</code>. Chaque méthode accepte un <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Sérialiser et désérialiser via un pipe - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Lire une séquence d'ensembles de données un à la fois - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Options de sérialisation">}}

<p>La classe <code>DicomXmlSerializerOptions</code> contrôle la façon dont les données DICOM sont représentées en XML. La configuration principale concerne la gestion du Bulk Data pour les valeurs binaires volumineuses :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Propriété</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Convertisseur personnalisé pour écrire de grandes données (p. ex., données d'image) sous forme de références URI BulkData au lieu de les incruster</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Chargeur personnalisé pour résoudre les URI BulkData lors de la désérialisation</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Instance d'options par défaut utilisée lorsqu'aucune option personnalisée n'est fournie</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Sérialiser avec des options personnalisées - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gestion du Bulk Data">}}

<p>Les valeurs binaires volumineuses (données d'image, formes d'onde, documents encapsulés) peuvent être externalisées sous forme de références URI BulkData au lieu d'être intégrées dans la sortie XML. Cela suit la spécification de l'élément <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">BulkData du DICOM PS3.19</a>.</p>

<p>Implémentez <code>IBulkDataConverter</code> pour externaliser les grandes données lors de la sérialisation, et <code>IBulkDataLoader</code> pour résoudre les URI lors de la désérialisation. Dans les cas courants, il n'est pas nécessaire d'écrire de chargeur : <code>DefaultBulkDataLoader.Instance</code> résout les URI <code>file</code>, <code>http</code> et <code>https</code>, et il implémente également <code>IAsyncBulkDataLoader</code>, de sorte que le Bulk Data est récupéré de façon asynchrone dans les chemins de flux.</p>

<div class="codeblock" id="code">
 <h3>Gestion personnalisée du Bulk Data - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
public class FileBulkDataConverter : IBulkDataConverter
{
    public string? GetBulkDataUri(IElement element)
    {
        // Externalize pixel data to a separate file
        if (element.Tag == Tag.PixelData)
            return "file:///bulk/pixeldata.raw";
        return null; // inline all other elements
    }
}

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Désérialiser XML en DICOM">}}

<p>Analysez le XML DICOM pour le reconvertir en objets Dataset. Prise en charge des entrées sous forme de chaîne, de flux et des opérations asynchrones :</p>

<div class="codeblock" id="code">
 <h3>Désérialiser XML en DICOM - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sérialisation XML vs JSON">}}

<p>Aspose.Medical prend en charge la sérialisation DICOM XML (PS3.19) et DICOM JSON (PS3.18). Les deux formats offrent une conversion sans perte en aller-retour, mais répondent à des scénarios d'intégration différents :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Fonctionnalité</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Norme</td><td>PS3.19 (Modèle DICOM natif)</td><td>PS3.18 (Modèle DICOM JSON)</td></tr>
<tr><td>Validation de schéma</td><td>XML Schema (XSD) disponible</td><td>Pas de schéma formel</td></tr>
<tr><td>Meilleur pour</td><td>Intégration d'entreprise, HL7 CDA, journaux d'audit, registres XDS</td><td>DICOMweb, API REST, FHIR ImagingStudy</td></tr>
<tr><td>Lisibilité humaine</td><td>Verbeux mais auto-descriptif</td><td>Compact et largement supporté</td></tr>
<tr><td>Données volumineuses</td><td>Élément BulkData avec URI</td><td>Propriété BulkDataURI</td></tr>
<tr><td>Classe du sérialiseur</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

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
{{< blocks/products/pf/slr-element name="Cas de succès" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
