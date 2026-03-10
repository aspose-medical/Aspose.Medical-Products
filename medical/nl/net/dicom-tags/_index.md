---
title: DICOM-tags lezen & wijzigen in C# .NET | Aspose.Medical
weight: 15000
description: Lees, voeg toe, werk bij en verwijder DICOM-tags in C# .NET. Toegang tot tag-metadata, werken met private tags, enumereren per groep en sequenties manipuleren met de Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM-tags lezen & wijzigen in .NET C#" h2="Toegang, toevoegen, bijwerken en verwijderen van DICOM-data-elementen met een type-veilig API. Werken met standaardtags, private tags, sequenties en de volledige DICOM-tagwoordenlijst." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Type-veilig DICOM-tagtoegang">}}

<p><strong>Aspose.Medical for .NET</strong> biedt een uitgebreide, type-veilige API voor het werken met DICOM-tags via de <code>Dataset</code>-klasse. Elk DICOM-data‑element wordt weergegeven door een <code>Tag</code> &mdash; een paar van groeps‑ en elementnummers die het attribuut uniek identificeren. De API biedt sterk getypeerde toegang met generieke methoden, veilige ophaalpatronen die uitzonderingen voorkomen, en ondersteuning voor enkele waarden, arrays en geïndexeerde toegang.</p>

<p>Veelvoorkomende DICOM-tags zijn beschikbaar als statische velden op de <code>Tag</code>-klasse (bijv. <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), waardoor het niet nodig is numerieke tagcodes te memoriseren. Elke tag bevat <code>TagMetadata</code> met zijn sleutelwoord, beschrijving, Value Representation (VR), waardevermenigvuldiging en verouderingsstatus.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM-tags lezen in C#">}}

<p>De <code>Dataset</code>-klasse biedt meerdere methoden om tagwaarden op te halen, afhankelijk van uw behoeften &mdash; enkele waarde, alle waarden, geïndexeerde toegang, of veilige ophaling met standaardwaarden:</p>

<div class="codeblock" id="code">
 <h3>DICOM-tagwaarden lezen - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM-tags toevoegen en bijwerken">}}

<p>Gebruik <code>AddOrUpdate</code> om tagwaarden in te stellen. De methode maakt het element aan als de tag ontbreekt, of vervangt de waarde als deze al bestaat. U kunt enkele waarden, arrays, of volledig getypeerde elementen toevoegen:</p>

<div class="codeblock" id="code">
 <h3>DICOM-tags toevoegen en bijwerken - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM-tags verwijderen">}}

<p>De <code>Remove</code>-methode ondersteunt het verwijderen van een enkele tag, meerdere tags tegelijk, of tags die voldoen aan een predicaat &mdash; handig om volledige groepen elementen te verwijderen:</p>

<div class="codeblock" id="code">
 <h3>DICOM-tags verwijderen - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Tags enumereren per groep">}}

<p>DICOM-tags zijn georganiseerd in groepen &mdash; bijvoorbeeld groep <code>0x0010</code> bevat patiëntinformatie, groep <code>0x0008</code> bevat studie‑identificatie, en groep <code>0x0028</code> bevat beeld‑pixel‑attributen. Gebruik <code>EnumerateGroup</code> om over alle elementen in een specifieke groep te itereren:</p>

<div class="codeblock" id="code">
 <h3>Elementen enumereren per groep - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Gegevensmanipulatie op elementniveau">}}

<p>Elk DICOM-element biedt methoden om individuele waarden binnen het element te lezen, toe te voegen, in te voegen, te verwijderen en te vervangen. Dit is nuttig voor meerwaarde‑tags waarbij u fijnmazige controle over de waardelijst nodig heeft:</p>

<div class="codeblock" id="code">
 <h3>Elementwaarden manipuleren - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Tagwoordenboek en metadata">}}

<p>De <code>TagDictionary</code>-klasse biedt een register van bekende DICOM-tags samen met hun metadata &mdash; sleutelwoord, beschrijving, Value Representation, waardevermenigvuldiging en verouderingsstatus. U kunt tags op sleutelwoord opzoeken of hun metadata programmatically inspecteren:</p>

<div class="codeblock" id="code">
 <h3>Het tagwoordenboek doorzoeken - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Ondersteuning voor private tags">}}

<p>DICOM staat leveranciers en organisaties toe private tags te definiëren voor eigendata. Aspose.Medical for .NET ondersteunt volledig het lezen, schrijven en registreren van private tags. U kunt aangepaste tagwoordenboeken laden uit XML‑bestanden om correcte verwerking van private elementen mogelijk te maken:</p>

<div class="codeblock" id="code">
 <h3>Werken met private tags - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Werken met DICOM-sequenties">}}

<p>DICOM-sequenties (VR‑type SQ) bevatten geneste datasets, waardoor een boomstructuur ontstaat binnen het DICOM‑bestand. De <code>Sequence</code>-klasse biedt getypeerde toegang tot sequentie‑items:</p>

<div class="codeblock" id="code">
 <h3>DICOM-sequenties lezen en maken - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Veelvoorkomende DICOM-taggroepen">}}

<p>De onderstaande tabel geeft veelgebruikte DICOM-taggroepen weer en voorbeeld‑tags die toegankelijk zijn via de <code>Tag</code>-klasse:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Groep</th>
<th>Categorie</th>
<th>Voorbeeldtags</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Bestandsmeta‑informatie</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Studie‑identificatie</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Patiëntinformatie</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Acquisitie‑parameters</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Beeldpositionering</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Beeld‑pixel‑attributen</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Pixelgegevens</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leermaterialen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Broncode" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API-referenties" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Productondersteuning" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis ondersteuning" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betaalde ondersteuning" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Waarom Aspose.Medical voor .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Klantenlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
