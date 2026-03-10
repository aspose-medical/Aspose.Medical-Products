---
title: Läs och ändra DICOM-taggar i C# .NET | Aspose.Medical
weight: 15000
description: Läs, lägg till, uppdatera och ta bort DICOM-taggar i C# .NET. Åtkomst till taggmetadata, arbete med privata taggar, enumerering efter grupp och manipulering av sekvenser med Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Läs och ändra DICOM-taggar i .NET C#" h2="Åtkomst, lägg till, uppdatera och ta bort DICOM-dataelement med ett typ‑säkert API. Arbeta med standardtaggar, privata taggar, sekvenser och hela DICOM‑taggboken." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Typ‑säker åtkomst till DICOM-taggar">}}

<p><strong>Aspose.Medical for .NET</strong> tillhandahåller ett omfattande, typ‑säkert API för att arbeta med DICOM-taggar via klassen <code>Dataset</code>. Varje DICOM‑dataelement representeras av en <code>Tag</code> &mdash; ett par av grupp‑ och elementnummer som unikt identifierar attributet. API:et erbjuder starkt typad åtkomst med generiska metoder, säkra hämtningsmönster som undviker undantag samt stöd för enstaka värden, arrayer och indexerad åtkomst.</p>

<p>Vanliga DICOM-taggar finns tillgängliga som statiska fält på <code>Tag</code>-klassen (t.ex. <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), vilket eliminerar behovet av att memorera numeriska taggkoder. Varje tagg innehåller <code>TagMetadata</code> med sitt nyckelord, beskrivning, Value Representation (VR), värdemultiplicitet och upphävningsstatus.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Läs DICOM-taggar i C#">}}

<p>Klassen <code>Dataset</code> tillhandahåller flera metoder för att hämta taggvärden beroende på dina behov &mdash; enstaka värde, alla värden, indexerad åtkomst eller säker hämtning med standardvärden:</p>

<div class="codeblock" id="code">
 <h3>Läs DICOM-taggvärden - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Lägg till och uppdatera DICOM-taggar">}}

<p>Använd <code>AddOrUpdate</code> för att sätta taggvärden. Metoden skapar elementet om taggen saknas, eller ersätter dess värde om det redan finns. Du kan sätta enstaka värden, arrayer eller lägga till fullt typade element:</p>

<div class="codeblock" id="code">
 <h3>Lägg till och uppdatera DICOM-taggar - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Ta bort DICOM-taggar">}}

<p>Metoden <code>Remove</code> stödjer att ta bort en enskild tagg, flera taggar samtidigt eller taggar som matchar ett villkor &mdash; användbart för att ta bort hela grupper av element:</p>

<div class="codeblock" id="code">
 <h3>Ta bort DICOM-taggar - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Iterera taggar efter grupp">}}

<p>DICOM-taggar är organiserade i grupper &mdash; till exempel innehåller grupp <code>0x0010</code> patientinformation, grupp <code>0x0008</code> studieidentifikation och grupp <code>0x0028</code> bildpixelattribut. Använd <code>EnumerateGroup</code> för att iterera över alla element i en specifik grupp:</p>

<div class="codeblock" id="code">
 <h3>Iterera element efter grupp - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Datamanipulation på elementnivå">}}

<p>Varje DICOM-element tillhandahåller metoder för att läsa, lägga till, infoga, ta bort och ersätta enskilda värden inom elementet. Detta är användbart för flervärda taggar där du behöver fin‑granulär kontroll över värdelistan:</p>

<div class="codeblock" id="code">
 <h3>Manipulera elementvärden - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Tagg‑ordbok och metadata">}}

<p>Klassen <code>TagDictionary</code> erbjuder ett register över kända DICOM-taggar samt deras metadata &mdash; nyckelord, beskrivning, Value Representation, värdemultiplicitet och upphävningsstatus. Du kan slå upp taggar efter nyckelord eller inspektera deras metadata programmässigt:</p>

<div class="codeblock" id="code">
 <h3>Fråga tagg‑ordboken - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Stöd för privata taggar">}}

<p>DICOM tillåter leverantörer och organisationer att definiera privata taggar för proprietär data. Aspose.Medical for .NET stödjer fullt ut läsning, skrivning och registrering av privata taggar. Du kan läsa in anpassade tagg‑ordböcker från XML‑filer för att möjliggöra korrekt hantering av privata element:</p>

<div class="codeblock" id="code">
 <h3>Arbeta med privata taggar - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Arbeta med DICOM‑sekvenser">}}

<p>DICOM‑sekvenser (VR‑typen SQ) innehåller inbäddade datasets och bildar en trädstruktur i DICOM‑filen. Klassen <code>Sequence</code> ger typad åtkomst till sekvensobjekt:</p>

<div class="codeblock" id="code">
 <h3>Läs och skapa DICOM‑sekvenser - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Vanliga DICOM‑tagggrupper">}}

<p>Följande tabell listar ofta använda DICOM‑tagggrupper och exempeltaggar som är tillgängliga via klassen <code>Tag</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Grupp</th>
<th>Kategori</th>
<th>Exempeltaggar</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Filmetadata</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Studieidentifikation</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Patientinformation</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Förvärvsparametrar</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Bildpositionering</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Bildpixelattribut</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Pixeldata</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Källkod" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑referenser" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktstöd" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betald support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blogg" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Varför Aspose.Medical för .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundlista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Framgångshistorier" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
