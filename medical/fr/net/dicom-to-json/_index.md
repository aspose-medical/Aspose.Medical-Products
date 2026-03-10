---
title: Convertir DICOM en JSON en C# .NET | Aspose.Medical
weight: 4000
description: Sérialisez des fichiers DICOM au format DICOM JSON standard en C# .NET. Configurez la gestion des nombres, les URI de données volumineuses, la sortie des mots-clés et le traitement asynchrone des flux avec l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convertir DICOM en JSON en .NET C#" h2="Sérialisez des ensembles de données DICOM et des fichiers au Modèle DICOM JSON standard (PS3.18). Configurez la gestion des nombres, les références de données volumineuses, la sortie des mots-clés et le traitement asynchrone basé sur les flux." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Modèle DICOM JSON standard">}}

<p><strong>Aspose.Medical for .NET</strong> sérialise les données DICOM en JSON conformément au <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">Modèle DICOM PS3.18 JSON</a>. Il s'agit de la norme officielle pour représenter les ensembles de données DICOM en JSON, utilisée par les services DICOMweb, les ressources FHIR ImagingStudy et les API de santé modernes.</p>

<p>Dans le Modèle DICOM JSON, chaque attribut est représenté par son tag comme clé JSON (par ex., <code>\"00100010\"</code> pour Patient Name), avec la Value Representation et le tableau de valeurs comme propriétés imbriquées :</p>

<div class="codeblock" id="code">
 <h3>Format du modèle DICOM JSON</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sérialiser DICOM en JSON en C#">}}

<p>Utilisez la classe <code>DicomJsonSerializer</code> pour convertir un fichier DICOM ou un ensemble de données en JSON. L'approche la plus simple produit une sortie compacte et conforme aux normes :</p>

<div class="codeblock" id="code">
 <h3>Convertir DICOM en JSON - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p>Vous pouvez sérialiser un <code>Dataset</code>, un <code>DicomFile</code> complet (y compris les informations méta du fichier), ou un tableau d'ensembles de données :</p>

<div class="codeblock" id="code">
 <h3>Sérialiser DicomFile et les tableaux d'ensembles de données - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sérialisation basée sur les flux et asynchrone">}}

<p>Pour les gros fichiers DICOM ou les scénarios à haut débit, sérialisez directement vers un flux UTF-8 afin d'éviter d'allouer de grandes chaînes en mémoire. Des méthodes synchrones et asynchrones sont disponibles :</p>

<div class="codeblock" id="code">
 <h3>Sérialisation en flux et asynchrone - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Options de sérialisation">}}

<p>La classe <code>DicomJsonSerializerOptions</code> contrôle la manière dont les données DICOM sont représentées en JSON :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Propriété</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Contrôle si les VR numériques (IS, DS, SV, UV) sont écrits comme nombres JSON ou chaînes</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Utilise les mots‑clés DICOM (par ex., <code>\"PatientName\"</code>) au lieu des tags (<code>\"00100010\"</code>) comme clés JSON. Remarque : non standard</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Inclut le mot‑clé DICOM comme attribut JSON supplémentaire</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Inclut le nom du tag DICOM comme attribut JSON supplémentaire</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Convertisseur personnalisé pour écrire de grandes données (par ex., données d'image) sous forme de références URI</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Chargeur personnalisé pour résoudre les URI de données volumineuses lors de la désérialisation</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Configurer les options de sérialisation - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gestion des nombres">}}

<p>Les Value Representations numériques DICOM (IS, DS, SV, UV) peuvent être sérialisées comme nombres JSON ou chaînes JSON. C'est important pour l'interopérabilité, car certaines implémentations DICOM stockent les nombres avec des espaces de tête ou un format non standard :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Mode</th>
<th>Sortie JSON</th>
<th>Cas d'utilisation</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>\"00180086\":{\"vr\":\"IS\",\"Value\":[42]}</code></td><td>Conforme à la norme, idéal pour le calcul. Lève une exception si la valeur ne peut pas être analysée.</td></tr>
<tr><td><code>AsString</code></td><td><code>\"00180086\":{\"vr\":\"IS\",\"Value\":[\"42\"]}</code></td><td>Conserve la représentation chaîne d'origine. Plus sûr pour le round‑trip de données non standard.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gestion des données volumineuses">}}

<p>Les valeurs binaires volumineuses (données d'image, formes d'onde, documents encapsulés) peuvent être gérées sous forme de références de données volumineuses plutôt que d'être incorporées dans la sortie JSON. Cela suit la spécification <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>.</p>

<p>Implémentez <code>IBulkDataConverter</code> pour externaliser les grandes données lors de la sérialisation, et <code>IBulkDataLoader</code> pour résoudre les URI lors de la désérialisation :</p>

<div class="codeblock" id="code">
 <h3>Gestion personnalisée des données volumineuses - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Désérialiser JSON en DICOM">}}

<p>Analysez le JSON DICOM pour le reconvertir en objets Dataset ou DicomFile. Prend en charge les entrées sous forme de chaîne, de flux, et les opérations de flux asynchrones :</p>

<div class="codeblock" id="code">
 <h3>Désérialiser JSON en DICOM - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Intégration System.Text.Json">}}

<p>Aspose.Medical fournit <code>DatasetJsonConverter</code> et <code>DicomFileJsonConverter</code> pour l'intégration avec le pipeline de sérialisation standard <code>System.Text.Json</code>. Cela permet de sérialiser les objets DICOM aux côtés d'autres types .NET dans votre code de traitement JSON existant :</p>

<div class="codeblock" id="code">
 <h3>Intégration System.Text.Json - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
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
{{< blocks/products/pf/slr-element name="Cas de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
