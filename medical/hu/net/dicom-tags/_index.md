---
title: DICOM címkék olvasása és módosítása C# .NET-ben | Aspose.Medical
weight: 15000
description: Olvassa, adja hozzá, frissítse és távolítsa el a DICOM címkéket C# .NET-ben. Hozzáférhet a címke metaadataihoz, dolgozhat privát címkékkel, csoportonként felsorolhat, és sorozatokat manipulálhat az Aspose.Medical API-val.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM címkék olvasása és módosítása .NET C#-ban" h2="Hozzáférhet, hozzáadhat, frissíthet és eltávolíthat DICOM adat‑elemeket egy típusbiztos API-val. Dolgozhat szabványos címkékkel, privát címkékkel, sorozatokkal és a teljes DICOM címke‑gyűjteménnyel." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Típusbiztos DICOM címkehozzáférés">}}

<p><strong>Aspose.Medical for .NET</strong> átfogó, típusbiztos API-t biztosít a DICOM címkékkel való munkához a <code>Dataset</code> osztályon keresztül. Minden DICOM adat‑elemet egy <code>Tag</code> &mdash; egy csoport‑ és elem‑számok párosa, amely egyedileg azonosítja a tulajdonságot – reprezentál. Az API erősen típusos hozzáférést kínál generikus metódusokkal, biztonságos lekérdezési mintákkal, amelyek elkerülik a kivételeket, valamint támogatja az egyszeri értékeket, tömböket és indexelt hozzáférést.</p>

<p>A gyakori DICOM címkék statikus mezőkként érhetők el a <code>Tag</code> osztályban (például <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), ezáltal nincs szükség a numerikus címkekódok memorizálására. Minden címke <code>TagMetadata</code>-t tartalmaz, amely a kulcsszót, leírást, Value Representation‑t (VR), értékmultiplicitást és a visszavonási állapotot tartalmazza.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM címkék olvasása C#-ban">}}

<p>A <code>Dataset</code> osztály több módszert kínál a címkeértékek lekérdezésére az igényeitől függően &mdash; egyetlen érték, összes érték, indexelt hozzáférés vagy biztonságos lekérés alapértelmezésekkel:</p>

<div class="codeblock" id="code">
 <h3>DICOM címkeértékek olvasása - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM címkék hozzáadása és frissítése">}}

<p>Használja a <code>AddOrUpdate</code> metódust a címkeértékek beállításához. A metódus létrehozza az elemet, ha a címke hiányzik, vagy lecseréli annak értékét, ha már létezik. Beállíthat egyetlen értékeket, tömböket, vagy teljesen típusos elemeket adhat hozzá:</p>

<div class="codeblock" id="code">
 <h3>DICOM címkék hozzáadása és frissítése - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM címkék eltávolítása">}}

<p>A <code>Remove</code> metódus támogatja egyetlen címke, több címke egyszerre, vagy egy feltételnek megfelelő címkék eltávolítását &mdash; hasznos a teljes elemcsoportok levágásához:</p>

<div class="codeblock" id="code">
 <h3>DICOM címkék eltávolítása - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Címkék felsorolása csoport szerint">}}

<p>A DICOM címkék csoportokba vannak rendezve — például a <code>0x0010</code> csoport a beteg információkat, a <code>0x0008</code> csoport a vizsgálati azonosítást, a <code>0x0028</code> csoport pedig a kép pixel attribútumait tartalmazza. Használja a <code>EnumerateGroup</code> metódust a megadott csoport összes elemének bejárásához:</p>

<div class="codeblock" id="code">
 <h3>Elemek felsorolása csoport szerint - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Elem-szintű adatmanipuláció">}}

<p>Minden DICOM elem metódusokat kínál az elem belső egyedi értékeinek olvasására, hozzáadására, beszúrására, eltávolítására és cseréjére. Ez hasznos többértékű címkéknél, ahol finomhangolt vezérlésre van szükség az értéklista felett:</p>

<div class="codeblock" id="code">
 <h3>Elemértékek manipulálása - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Címke-gyűjtemény és metaadatok">}}

<p>A <code>TagDictionary</code> osztály egy regisztert biztosít az ismert DICOM címkékről és azok metaadatairól — kulcsszó, leírás, Value Representation, értékmultiplicitás és visszavonási állapot. Címkéket kereshet kulcsszó alapján vagy programozottan ellenőrizheti a metaadataikat:</p>

<div class="codeblock" id="code">
 <h3>Címke-gyűjtemény lekérdezése - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Privát címkék támogatása">}}

<p>A DICOM lehetővé teszi a gyártók és szervezetek számára, hogy privát címkéket definiáljanak saját adataikhoz. Az Aspose.Medical for .NET teljes mértékben támogatja a privát címkék olvasását, írását és regisztrálását. Betölthet egyéni címke-gyűjteményeket XML fájlokból a privát elemek megfelelő kezeléséhez:</p>

<div class="codeblock" id="code">
 <h3>Munka privát címkékkel - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Munka DICOM sorozatokkal">}}

<p>A DICOM sorozatok (VR típus SQ) egymásba ágyazott dataset-eket tartalmaznak, amely fastruktúrát hoz létre a DICOM fájlban. A <code>Sequence</code> osztály típusos hozzáférést kínál a sorozatelemekhez:</p>

<div class="codeblock" id="code">
 <h3>DICOM sorozatok olvasása és létrehozása - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Gyakori DICOM címkecsoportok">}}

<p>Az alábbi táblázat felsorolja a gyakran használt DICOM címkecsoportokat és példacímkéket, amelyek a <code>Tag</code> osztályon keresztül érhetők el:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Csoport</th>
<th>Kategória</th>
<th>Példa címkék</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Fájl meta információ</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Vizsgálat azonosítása</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Beteg információi</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Beszerzési paraméterek</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Kép pozicionálás</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Kép pixel attribútumok</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Pixel adatok</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Forráskód" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetett támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért az Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
