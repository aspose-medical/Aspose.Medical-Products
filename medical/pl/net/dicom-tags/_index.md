---
title: Odczyt i modyfikacja tagów DICOM w C# .NET | Aspose.Medical
weight: 15000
description: Odczytuj, dodawaj, aktualizuj i usuwaj tagi DICOM w C# .NET. Uzyskaj dostęp do metadanych tagów, pracuj z prywatnymi tagami, wymieniaj według grupy oraz manipuluj sekwencjami przy użyciu API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Odczyt i modyfikacja tagów DICOM w .NET C#" h2="Uzyskaj dostęp, dodawaj, aktualizuj i usuwaj elementy danych DICOM przy użyciu typowo bezpiecznego API. Pracuj ze standardowymi tagami, prywatnymi tagami, sekwencjami oraz pełnym słownikiem tagów DICOM." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Typowo bezpieczny dostęp do tagów DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> zapewnia kompleksowe, typowo bezpieczne API do pracy z tagami DICOM za pośrednictwem klasy <code>Dataset</code>. Każdy element danych DICOM jest reprezentowany przez <code>Tag</code> &mdash; parę numerów grupy i elementu, które jednoznacznie identyfikują atrybut. API oferuje silnie typowy dostęp przy użyciu metod generycznych, bezpieczne wzorce pobierania unikające wyjątków oraz obsługę wartości pojedynczych, tablic i dostępu indeksowanego.</p>

<p>Popularne tagi DICOM są dostępne jako pola statyczne w klasie <code>Tag</code> (np. <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), co eliminuje konieczność zapamiętywania numerycznych kodów tagów. Każdy tag zawiera <code>TagMetadata</code> z jego słowem kluczowym, opisem, reprezentacją wartości (VR), liczebnością wartości oraz statusem wycofania.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Odczyt tagów DICOM w C#">}}

<p>Klasa <code>Dataset</code> oferuje wiele metod pobierania wartości tagów w zależności od potrzeb &mdash; pojedyncza wartość, wszystkie wartości, dostęp indeksowany lub bezpieczne pobieranie z wartościami domyślnymi:</p>

<div class="codeblock" id="code">
 <h3>Odczyt wartości tagów DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Dodawanie i aktualizacja tagów DICOM">}}

<p>Użyj <code>AddOrUpdate</code> do ustawiania wartości tagów. Metoda tworzy element, jeśli tag jest nieobecny, lub zamienia jego wartość, jeśli już istnieje. Możesz ustawiać pojedyncze wartości, tablice lub dodawać w pełni typowane elementy:</p>

<div class="codeblock" id="code">
 <h3>Dodawanie i aktualizacja tagów DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Usuwanie tagów DICOM">}}

<p>Metoda <code>Remove</code> umożliwia usunięcie pojedynczego taga, wielu tagów jednocześnie lub tagów spełniających predykat &mdash; przydatne do usuwania całych grup elementów:</p>

<div class="codeblock" id="code">
 <h3>Usuwanie tagów DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Iteracja tagów według grupy">}}

<p>Tagi DICOM są zorganizowane w grupy &mdash; na przykład grupa <code>0x0010</code> zawiera informacje o pacjencie, grupa <code>0x0008</code> zawiera identyfikację badania, a grupa <code>0x0028</code> zawiera atrybuty pikseli obrazu. Użyj <code>EnumerateGroup</code>, aby iterować po wszystkich elementach w określonej grupie:</p>

<div class="codeblock" id="code">
 <h3>Iteracja elementów według grupy - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Manipulacja danymi na poziomie elementu">}}

<p>Każdy element DICOM udostępnia metody do odczytu, dodawania, wstawiania, usuwania i zamiany poszczególnych wartości w obrębie elementu. Jest to przydatne przy tagach wielowartościowych, gdzie potrzebna jest precyzyjna kontrola nad listą wartości:</p>

<div class="codeblock" id="code">
 <h3>Manipulacja wartościami elementu - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Słownik tagów i metadane">}}

<p>Klasa <code>TagDictionary</code> dostarcza rejestru znanych tagów DICOM wraz z ich metadanymi &mdash; słowem kluczowym, opisem, reprezentacją wartości, liczebnością wartości oraz statusem wycofania. Możesz wyszukiwać tagi po słowie kluczowym lub programowo przeglądać ich metadane:</p>

<div class="codeblock" id="code">
 <h3>Zapytanie słownika tagów - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Obsługa tagów prywatnych">}}

<p>DICOM umożliwia dostawcom i organizacjom definiowanie prywatnych tagów dla danych własnościowych. Aspose.Medical for .NET w pełni obsługuje odczyt, zapis i rejestrację tagów prywatnych. Możesz wczytywać własne słowniki tagów z plików XML, aby zapewnić właściwe przetwarzanie prywatnych elementów:</p>

<div class="codeblock" id="code">
 <h3>Praca z tagami prywatnymi - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Praca z sekwencjami DICOM">}}

<p>Sekwencje DICOM (typ VR SQ) zawierają zagnieżdżone zestawy danych, tworząc strukturę drzewa w pliku DICOM. Klasa <code>Sequence</code> zapewnia typowy dostęp do elementów sekwencji:</p>

<div class="codeblock" id="code">
 <h3>Odczyt i tworzenie sekwencji DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Popularne grupy tagów DICOM">}}

<p>Poniższa tabela zawiera najczęściej używane grupy tagów DICOM oraz przykładowe tagi dostępne poprzez klasę <code>Tag</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Grupa</th>
<th>Kategoria</th>
<th>Przykładowe tagi</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Informacje meta pliku</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Identyfikacja badania</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Informacje o pacjencie</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Parametry akwizycji</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Pozycjonowanie obrazu</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Atrybuty pikseli obrazu</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Dane pikseli</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zasoby edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Kod źródłowy" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Odwołania API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Wsparcie produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Darmowe wsparcie" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Wsparcie płatne" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Dlaczego Aspose.Medical dla .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista klientów" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Historie sukcesu" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
