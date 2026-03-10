---
title: DICOM-Tags in C# .NET lesen & bearbeiten | Aspose.Medical
weight: 15000
description: DICOM-Tags in C# .NET lesen, hinzufügen, aktualisieren und entfernen. Tag-Metadaten zugreifen, mit privaten Tags arbeiten, nach Gruppe enumerieren und Sequenzen mit der Aspose.Medical API manipulieren.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM-Tags in .NET C# lesen & bearbeiten" h2="Zugriff, Hinzufügen, Aktualisieren und Entfernen von DICOM-Datenobjekten mit einer typsicheren API. Arbeit mit Standard-Tags, privaten Tags, Sequenzen und dem vollständigen DICOM-Tag-Wörterbuch." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Typsicherer Zugriff auf DICOM-Tags">}}

<p><strong>Aspose.Medical for .NET</strong> bietet eine umfassende, typsichere API zur Arbeit mit DICOM-Tags über die Klasse <code>Dataset</code>. Jedes DICOM-Datenlement wird durch ein <code>Tag</code> &mdash; ein Paar aus Gruppen- und Elementnummern, die das Attribut eindeutig identifizieren – dargestellt. Die API bietet stark typisierten Zugriff mit generischen Methoden, sichere Abrufmuster, die Ausnahmen vermeiden, sowie Unterstützung für Einzelwerte, Arrays und indizierten Zugriff.</p>

<p>Übliche DICOM-Tags sind als statische Felder in der Klasse <code>Tag</code> verfügbar (z. B. <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), wodurch das Auswendiglernen numerischer Tag-Codes entfällt. Jeder Tag enthält <code>TagMetadata</code> mit seinem Schlüsselwort, seiner Beschreibung, der Value Representation (VR), der Wertmultiplikität und dem Veraltungsstatus.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM-Tags in C# lesen">}}

<p>Die Klasse <code>Dataset</code> stellt mehrere Methoden zum Abrufen von Tag-Werten bereit, je nach Bedarf &mdash; Einzelwert, alle Werte, indizierter Zugriff oder sicherer Abruf mit Vorgabewerten:</p>

<div class="codeblock" id="code">
 <h3>DICOM-Tag-Werte lesen - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM-Tags hinzufügen und aktualisieren">}}

<p>Verwenden Sie <code>AddOrUpdate</code>, um Tag-Werte zu setzen. Die Methode erstellt das Element, wenn das Tag fehlt, oder ersetzt dessen Wert, falls es bereits existiert. Sie können Einzelwerte, Arrays oder vollständig typisierte Elemente hinzufügen:</p>

<div class="codeblock" id="code">
 <h3>DICOM-Tags hinzufügen und aktualisieren - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM-Tags entfernen">}}

<p>Die Methode <code>Remove</code> unterstützt das Entfernen eines einzelnen Tags, mehrerer Tags gleichzeitig oder von Tags, die einem Prädikat entsprechen &mdash; nützlich, um ganze Gruppen von Elementen zu entfernen:</p>

<div class="codeblock" id="code">
 <h3>DICOM-Tags entfernen - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Tags nach Gruppe enumerieren">}}

<p>DICOM-Tags sind in Gruppen organisiert &mdash; zum Beispiel enthält Gruppe <code>0x0010</code> Patienteninformationen, Gruppe <code>0x0008</code> Studienidentifikation und Gruppe <code>0x0028</code> Bildpixel‑Attribute. Verwenden Sie <code>EnumerateGroup</code>, um über alle Elemente einer bestimmten Gruppe zu iterieren:</p>

<div class="codeblock" id="code">
 <h3>Elemente nach Gruppe enumerieren - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Datenmanipulation auf Elementebene">}}

<p>Jedes DICOM-Element bietet Methoden zum Lesen, Hinzufügen, Einfügen, Entfernen und Ersetzen einzelner Werte innerhalb des Elements. Dies ist nützlich für mehrwertige Tags, bei denen eine feinkörnige Kontrolle der Wertliste erforderlich ist:</p>

<div class="codeblock" id="code">
 <h3>Elementwerte manipulieren - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Tag-Wörterbuch und Metadaten">}}

<p>Die Klasse <code>TagDictionary</code> stellt ein Register bekannter DICOM-Tags samt ihrer Metadaten bereit &mdash; Schlüsselwort, Beschreibung, Value Representation, Wertmultiplikität und Veraltungsstatus. Sie können Tags über das Schlüsselwort nachschlagen oder deren Metadaten programmgesteuert inspizieren:</p>

<div class="codeblock" id="code">
 <h3>Abfrage des Tag-Wörterbuchs - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Unterstützung privater Tags">}}

<p>DICOM ermöglicht Anbietern und Organisationen die Definition privater Tags für proprietäre Daten. Aspose.Medical for .NET unterstützt das Lesen, Schreiben und Registrieren privater Tags vollständig. Sie können benutzerdefinierte Tag-Wörterbücher aus XML‑Dateien laden, um eine korrekte Handhabung privater Elemente zu ermöglichen:</p>

<div class="codeblock" id="code">
 <h3>Arbeiten mit privaten Tags - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Arbeiten mit DICOM-Sequenzen">}}

<p>DICOM-Sequenzen (VR-Typ SQ) enthalten verschachtelte Datasets und bilden eine Baumstruktur innerhalb der DICOM-Datei. Die Klasse <code>Sequence</code> bietet typisierten Zugriff auf Sequenz‑Elemente:</p>

<div class="codeblock" id="code">
 <h3>DICOM-Sequenzen lesen und erstellen - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Häufige DICOM-Tag-Gruppen">}}

<p>Die folgende Tabelle listet häufig verwendete DICOM-Tag-Gruppen und Beispiel‑Tags, die über die Klasse <code>Tag</code> zugänglich sind:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Gruppe</th>
<th>Kategorie</th>
<th>Beispiel‑Tags</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Datei-Metainformationen</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Studienidentifikation</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Patienteninformationen</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Erfassungsparameter</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Bildpositionierung</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Bildpixel‑Attribute</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Pixeldaten</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Quellcode" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API-Referenzen" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktunterstützung" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Kostenloser Support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Bezahlter Support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Warum Aspose.Medical für .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundenliste" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Erfolgsgeschichten" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
