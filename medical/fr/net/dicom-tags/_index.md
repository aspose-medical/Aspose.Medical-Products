---
title: Lire & Modifier les tags DICOM en C# .NET | Aspose.Medical
weight: 15000
description: Lire, ajouter, mettre à jour et supprimer des tags DICOM en C# .NET. Accéder aux métadonnées des tags, travailler avec les tags privés, énumérer par groupe et manipuler les séquences à l’aide de l’API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Lire & Modifier les tags DICOM en .NET C#" h2="Accéder, ajouter, mettre à jour et supprimer des éléments de données DICOM avec une API typée. Travailler avec les tags standards, les tags privés, les séquences et le dictionnaire complet des tags DICOM." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Accès typé aux tags DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> fournit une API complète et typée pour travailler avec les tags DICOM via la classe <code>Dataset</code>. Chaque élément de données DICOM est représenté par un <code>Tag</code> &mdash; une paire de numéros de groupe et d’élément qui identifie de façon unique l’attribut. L’API offre un accès fortement typé avec des méthodes génériques, des modèles de récupération sécurisés qui évitent les exceptions, et la prise en charge des valeurs simples, des tableaux et de l’accès indexé.</p>

<p>Les tags DICOM courants sont disponibles en tant que champs statiques sur la classe <code>Tag</code> (par ex., <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), éliminant ainsi la nécessité de mémoriser les codes numériques des tags. Chaque tag possède un <code>TagMetadata</code> contenant son mot‑clé, sa description, la représentation de valeur (VR), la multiplicité des valeurs et son statut de retrait.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lire les tags DICOM en C#">}}

<p>La classe <code>Dataset</code> propose plusieurs méthodes pour récupérer les valeurs des tags en fonction de vos besoins &mdash; valeur unique, toutes les valeurs, accès indexé ou récupération sécurisée avec des valeurs par défaut :</p>

<div class="codeblock" id="code">
 <h3>Lire les valeurs des tags DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Get a single value (throws if tag is missing or multi-valued)
string patientName = dataset.GetSingleValue&lt;string&gt;(Tag.PatientName);

// Safe retrieval with a default value
string institution = dataset.GetSingleValueOrDefault&lt;string&gt;(
    Tag.InstitutionName, "Unknown");

// Get all values from a multi-valued tag
double[] pixelSpacing = dataset.GetValues&lt;double&gt;(Tag.PixelSpacing);

// Get a value by index (supports negative indexing with ^)
double firstPosition = dataset.GetValue&lt;double&gt;(Tag.ImagePositionPatient, 0);

// Check if a tag exists before accessing it
if (dataset.Contains(Tag.PatientBirthDate))
{
    var birthDate = dataset.GetSingleValue&lt;DateOnly&gt;(Tag.PatientBirthDate);
}

// TryGet pattern for safe access without exceptions
if (dataset.TryGetSingleValue&lt;string&gt;(Tag.PatientID, out var patientId))
{
    // Use patientId
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ajouter et mettre à jour les tags DICOM">}}

<p>Utilisez <code>AddOrUpdate</code> pour définir les valeurs des tags. La méthode crée l’élément si le tag est absent, ou remplace sa valeur s’il existe déjà. Vous pouvez définir des valeurs uniques, des tableaux ou ajouter des éléments entièrement typés :</p>

<div class="codeblock" id="code">
 <h3>Ajouter et mettre à jour les tags DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Set a single value
dataset.AddOrUpdate(Tag.PatientName, "John Doe");
dataset.AddOrUpdate(Tag.PatientAge, new Age { Number = 42, Units = Age.Unit.Years });

// Set multiple values on a multi-valued tag
dataset.AddOrUpdate&lt;double&gt;(Tag.PixelSpacing, [0.5, 0.5]);

// Add multiple elements at once using typed element classes
dataset.AddOrUpdateRange(new IElement[]
{
    new PersonName(Tag.PatientName, ["John Doe"]),
    new LongString(Tag.PatientID, ["64AD6A3F"]),
    new Date(Tag.PatientBirthDate, [new DateOnly(2002, 10, 10)])
});

// Save the modified file
dicomFile.Save("updated.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Supprimer les tags DICOM">}}

<p>La méthode <code>Remove</code> permet de supprimer un seul tag, plusieurs tags en une fois, ou des tags correspondant à un prédicat &mdash; utile pour éliminer des groupes entiers d’éléments :</p>

<div class="codeblock" id="code">
 <h3>Supprimer les tags DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Remove a single tag
dataset.Remove(Tag.PatientAddress);

// Remove multiple tags at once
dataset.Remove(Tag.PatientName, Tag.PatientID, Tag.PatientBirthDate);

// Remove all tags in a specific group (e.g., group 0x0002 - File Meta Information)
dataset.Remove(element => element.Tag.Group == 0x0002);

dicomFile.Save("cleaned.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Énumérer les tags par groupe">}}

<p>Les tags DICOM sont organisés en groupes &mdash; par exemple, le groupe <code>0x0010</code> contient les informations du patient, le groupe <code>0x0008</code> contient l’identification de l’étude, et le groupe <code>0x0028</code> contient les attributs des pixels d’image. Utilisez <code>EnumerateGroup</code> pour itérer sur tous les éléments d’un groupe spécifique :</p>

<div class="codeblock" id="code">
 <h3>Énumérer les éléments par groupe - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Enumerate all patient-related tags (group 0x0010)
foreach (IElement element in dataset.EnumerateGroup(0x0010))
{
    Tag tag = element.Tag;
    string keyword = tag.Metadata.Keyword;
    string vr = element.ValueRepresentation.ToString();

    Console.WriteLine($"({tag.Group:X4},{tag.Element:X4}) {keyword} [{vr}]");
}

// Iterate over all elements in the dataset
foreach (IElement element in dataset)
{
    // Process each element
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Manipulation des données au niveau de l’élément">}}

<p>Chaque élément DICOM fournit des méthodes pour lire, ajouter, insérer, supprimer et remplacer des valeurs individuelles au sein de l’élément. Ceci est utile pour les tags à valeurs multiples où un contrôle granulaire de la liste de valeurs est nécessaire :</p>

<div class="codeblock" id="code">
 <h3>Manipuler les valeurs d’élément - C#</h3>
 <pre><code class="cs">// Create an element with multiple values
var element = new FloatingPointDouble(
    Tag.TableOfYBreakPoints, [50.1, 100.2, 150.3]);

// Access values by index (supports ^ for indexing from end)
double first = element.Get&lt;double&gt;(0);
double last = element.Get&lt;double&gt;(^1);

// Safe access with default
double safe = element.GetOrDefault&lt;double&gt;(10); // returns 0.0 if out of range

// Get all values or a range
double[] all = element.GetValues&lt;double&gt;();
double[] subset = element.GetValues&lt;double&gt;(1..3);

// Modify element values
element.Add(200.4);
element.AddRange([300.5, 400.6]);
element.Insert(2, 75.0);
element.RemoveAt(0);
element.Replace([1.0, 2.0, 3.0]);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dictionnaire des tags et métadonnées">}}

<p>La classe <code>TagDictionary</code> fournit un registre des tags DICOM connus ainsi que leurs métadonnées &mdash; mot‑clé, description, représentation de valeur (VR), multiplicité des valeurs et statut de retrait. Vous pouvez rechercher des tags par mot‑clé ou inspecter leurs métadonnées par programmation :</p>

<div class="codeblock" id="code">
 <h3>Interroger le dictionnaire des tags - C#</h3>
 <pre><code class="cs">// Look up a tag by its keyword
Tag? tag = TagDictionary.Default.FindByKeyword("PatientName");

// Access tag metadata from the dictionary
TagMetadata meta = TagDictionary.Default[Tag.PatientName];
Console.WriteLine($"Keyword: {meta.Keyword}");
Console.WriteLine($"Name: {meta.Name}");
Console.WriteLine($"VR: {string.Join("/", meta.ValueRepresentations)}");
Console.WriteLine($"VM: {meta.Multiplicity}");
Console.WriteLine($"Retired: {meta.Retired}");

// Access metadata directly from a tag instance
TagMetadata info = Tag.Rows.Metadata;

// Parse a tag from its string representation
Tag parsed = Tag.Parse("0010,0010");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Prise en charge des tags privés">}}

<p>DICOM autorise les fournisseurs et les organisations à définir des tags privés pour des données propriétaires. Aspose.Medical for .NET prend en charge totalement la lecture, l’écriture et l’enregistrement des tags privés. Vous pouvez charger des dictionnaires de tags personnalisés à partir de fichiers XML afin de gérer correctement les éléments privés :</p>

<div class="codeblock" id="code">
 <h3>Travailler avec les tags privés - C#</h3>
 <pre><code class="cs">// Load a custom private tag dictionary from XML
// XML format:
// &lt;dictionaries&gt;
//   &lt;dictionary creator="MY ORGANIZATION"&gt;
//     &lt;tag group="3011" element="xx00" vr="SL" vm="1"&gt;Custom Value&lt;/tag&gt;
//   &lt;/dictionary&gt;
// &lt;/dictionaries&gt;
TagDictionary.Default.Load("custom-tag-dictionary.xml");

// Check if a tag is private
Tag tag = Tag.GetOrCreate(0x3011, 0x0010);
bool isPrivate = tag.IsPrivate;

// Get a valid private tag (creates Private Creator element if needed)
Dataset dataset = dicomFile.Dataset;
Tag privateTag = dataset.GetPrivateTag(tag);

// Register a private creator programmatically
PrivateCreator creator = new("MY ORGANIZATION");
TagDictionary privateDictionary =
    TagDictionary.Default.GetPrivateDictionary(creator);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Travailler avec les séquences DICOM">}}

<p>Les séquences DICOM (type VR SQ) contiennent des ensembles de données imbriqués, formant une structure arborescente dans le fichier DICOM. La classe <code>Sequence</code> fournit un accès typé aux éléments de la séquence :</p>

<div class="codeblock" id="code">
 <h3>Lire et créer des séquences DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Read a sequence element
IElement seqElement = dataset.GetOrDefault(Tag.ReferencedStudySequence);
if (seqElement is Sequence sequence)
{
    // Iterate over sequence items (each item is a Dataset)
    foreach (Dataset item in sequence.Items)
    {
        string uid = item.GetSingleValueOrDefault&lt;string&gt;(
            Tag.ReferencedSOPInstanceUID, "");
    }

    // Access an item by index
    Dataset firstItem = sequence.Get(0);
}

// Create a new sequence
var items = new List&lt;Dataset&gt;
{
    new(new IElement[]
    {
        new UniqueIdentifier(Tag.ReferencedSOPClassUID, ["1.2.840.10008.5.1.4.1.1.2"]),
        new UniqueIdentifier(Tag.ReferencedSOPInstanceUID, ["1.2.3.4.5.6.7.8.9"])
    })
};
dataset.Add(new Sequence(Tag.ReferencedStudySequence, items));</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Groupes de tags DICOM courants">}}

<p>Le tableau suivant répertorie les groupes de tags DICOM fréquemment utilisés et des tags d’exemple accessibles via la classe <code>Tag</code> :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Groupe</th>
<th>Catégorie</th>
<th>Tags d’exemple</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Informations de méta-fichier</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Identification de l’étude</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Informations du patient</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Paramètres d’acquisition</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Positionnement de l’image</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Attributs des pixels d’image</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Données de pixels</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ressources d’apprentissage" tabId="resources" >}}
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
{{< blocks/products/pf/slr-element name="Cas de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
