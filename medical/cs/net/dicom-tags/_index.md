---
title: Číst a upravovat DICOM značky v C# .NET | Aspose.Medical
weight: 15000
description: Čtěte, přidávejte, aktualizujte a odstraňujte DICOM značky v C# .NET. Přístup k metadatům značek, práce s privátními značkami, výčtování podle skupiny a manipulace se sekvencemi pomocí Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Číst a upravovat DICOM značky v .NET C#" h2="Přístup, přidávání, aktualizace a odstraňování DICOM datových elementů pomocí typově bezpečného API. Práce se standardními značkami, privátními značkami, sekvencemi a kompletním slovníkem DICOM značek." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Typově bezpečný přístup ke DICOM značkám">}}

<p><strong>Aspose.Medical for .NET</strong> poskytuje komplexní, typově bezpečné API pro práci se DICOM značkami prostřednictvím třídy <code>Dataset</code>. Každý DICOM datový element je reprezentován <code>Tag</code> &mdash; dvojicí čísla skupiny a elementu, která jedinečně identifikuje atribut. API nabízí silně typovaný přístup pomocí generických metod, bezpečné vzory načítání zabraňující výjimkám a podporu pro jednotlivé hodnoty, pole i indexovaný přístup.</p>

<p>Časté DICOM značky jsou dostupné jako statické pole ve třídě <code>Tag</code> (např. <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), což eliminuje nutnost pamatovat si číselné kódy značek. Každá značka obsahuje <code>TagMetadata</code> se svým klíčovým slovem, popisem, Value Representation (VR), početnostmi hodnot a stavem vyřazení.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Číst DICOM značky v C#">}}

<p>Třída <code>Dataset</code> poskytuje více metod pro získání hodnot značek podle vašich potřeb &mdash; jedna hodnota, všechny hodnoty, indexovaný přístup nebo bezpečné načtení s výchozími hodnotami:</p>

<div class="codeblock" id="code">
 <h3>Číst hodnoty DICOM značek – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Přidávat a aktualizovat DICOM značky">}}

<p>Použijte <code>AddOrUpdate</code> k nastavení hodnot značek. Metoda vytvoří element, pokud značka chybí, nebo nahradí její hodnotu, pokud již existuje. Můžete nastavit jednotlivé hodnoty, pole nebo přidat plně typované elementy:</p>

<div class="codeblock" id="code">
 <h3>Přidávat a aktualizovat DICOM značky – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Odstraňovat DICOM značky">}}

<p>Metoda <code>Remove</code> podporuje odstraňování jedné značky, více značek najednou nebo značek odpovídajících predikátu &mdash; užitečné pro odstraňování celých skupin elementů:</p>

<div class="codeblock" id="code">
 <h3>Odstraňovat DICOM značky – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Vyjmenovat značky podle skupiny">}}

<p>DICOM značky jsou uspořádány do skupin — například skupina <code>0x0010</code> obsahuje informace o pacientovi, skupina <code>0x0008</code> obsahuje identifikaci studie a skupina <code>0x0028</code> obsahuje atributy pixelů obrazu. Použijte <code>EnumerateGroup</code> k iteraci přes všechny elementy v konkrétní skupině:</p>

<div class="codeblock" id="code">
 <h3>Vyjmenovat elementy podle skupiny – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Manipulace s daty na úrovni elementu">}}

<p>Každý DICOM element poskytuje metody pro čtení, přidávání, vkládání, odstraňování a nahrazování jednotlivých hodnot v rámci elementu. To je užitečné pro vícehodnotové značky, kde potřebujete jemnozrnnou kontrolu nad seznamem hodnot:</p>

<div class="codeblock" id="code">
 <h3>Manipulovat hodnoty elementu – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Slovník značek a metadata">}}

<p>Třída <code>TagDictionary</code> poskytuje registr známých DICOM značek spolu s jejich metadaty — klíčové slovo, popis, Value Representation, početnost hodnot a stav vyřazení. Můžete vyhledávat značky podle klíčového slova nebo programově zkoumat jejich metadata:</p>

<div class="codeblock" id="code">
 <h3>Dotázat se na slovník značek – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Podpora privátních značek">}}

<p>DICOM umožňuje výrobcům a organizacím definovat privátní značky pro proprietární data. Aspose.Medical pro .NET plně podporuje čtení, zápis a registraci privátních značek. Můžete načíst vlastní slovníky značek z XML souborů, aby bylo zajištěno správné zpracování privátních elementů:</p>

<div class="codeblock" id="code">
 <h3>Práce s privátními značkami – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Práce s DICOM sekvencemi">}}

<p>DICOM sekvence (typ VR SQ) obsahují vnořené datasety, tvořící stromovou strukturu uvnitř DICOM souboru. Třída <code>Sequence</code> poskytuje typově bezpečný přístup k položkám sekvence:</p>

<div class="codeblock" id="code">
 <h3>Číst a vytvářet DICOM sekvence – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Běžné skupiny DICOM značek">}}

<p>Následující tabulka uvádí často používané skupiny DICOM značek a příkladové značky dostupné přes třídu <code>Tag</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Skupina</th>
<th>Kategorie</th>
<th>Příklad značek</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Informace o souboru</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Identifikace studie</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Informace o pacientovi</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Parametry akvizice</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Umístění obrazu</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Atributy pixelu obrazu</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Pixelová data</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Vzdělávací zdroje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Zdrojový kód" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Odkazy na API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Podpora produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Bezplatná podpora" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Placená podpora" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Proč Aspose.Medical pro .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Seznam zákazníků" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Úspěšné příběhy" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
