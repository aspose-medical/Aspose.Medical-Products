---
title: Leggi e Modifica i Tag DICOM in C# .NET | Aspose.Medical
weight: 15000
description: Leggi, aggiungi, aggiorna e rimuovi i tag DICOM in C# .NET. Accedi ai metadati dei tag, lavora con i tag privati, elenca per gruppo e manipola le sequenze usando l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Leggi e Modifica i Tag DICOM in .NET C#" h2="Accedi, aggiungi, aggiorna e rimuovi elementi di dati DICOM con un'API type-safe. Lavora con i tag standard, i tag privati, le sequenze e l'intero dizionario dei tag DICOM." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Accesso Type-Safe ai Tag DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> fornisce un'API completa e type-safe per lavorare con i tag DICOM tramite la classe <code>Dataset</code>. Ogni elemento di dati DICOM è rappresentato da un <code>Tag</code> &mdash; una coppia di numeri di gruppo e di elemento che identifica univocamente l'attributo. L'API offre accesso tipizzato con metodi generici, pattern di recupero sicuri che evitano eccezioni e supporto per valori singoli, array e accesso indicizzato.</p>

<p>I tag DICOM comuni sono disponibili come campi statici nella classe <code>Tag</code> (ad es., <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), eliminando la necessità di memorizzare i codici numerici dei tag. Ogni tag contiene <code>TagMetadata</code> con la sua keyword, descrizione, Value Representation (VR), molteplicità del valore e stato di ritiro.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Leggi i Tag DICOM in C#">}}

<p>La classe <code>Dataset</code> fornisce diversi metodi per recuperare i valori dei tag a seconda delle tue esigenze &mdash; valore singolo, tutti i valori, accesso indicizzato o recupero sicuro con valori predefiniti:</p>

<div class="codeblock" id="code">
 <h3>Leggi i valori dei tag DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Aggiungi e Aggiorna i Tag DICOM">}}

<p>Utilizza <code>AddOrUpdate</code> per impostare i valori dei tag. Il metodo crea l'elemento se il tag è mancante, oppure ne sostituisce il valore se esiste già. È possibile impostare valori singoli, array o aggiungere elementi completamente tipizzati:</p>

<div class="codeblock" id="code">
 <h3>Aggiungi e aggiorna i tag DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Rimuovi i Tag DICOM">}}

<p>Il metodo <code>Remove</code> supporta la rimozione di un singolo tag, di più tag contemporaneamente o di tag che corrispondono a un predicato &mdash; utile per eliminare interi gruppi di elementi:</p>

<div class="codeblock" id="code">
 <h3>Rimuovi i tag DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Elenca i Tag per Gruppo">}}

<p>I tag DICOM sono organizzati in gruppi &mdash; ad esempio, il gruppo <code>0x0010</code> contiene le informazioni del paziente, il gruppo <code>0x0008</code> contiene l'identificazione dello studio e il gruppo <code>0x0028</code> contiene gli attributi dei pixel dell'immagine. Usa <code>EnumerateGroup</code> per iterare su tutti gli elementi di un gruppo specifico:</p>

<div class="codeblock" id="code">
 <h3>Elenca gli elementi per gruppo - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Manipolazione dei Dati a Livello di Elemento">}}

<p>Ogni elemento DICOM fornisce metodi per leggere, aggiungere, inserire, rimuovere e sostituire valori individuali all'interno dell'elemento. Questo è utile per i tag multivalore in cui è necessario un controllo granulare sulla lista dei valori:</p>

<div class="codeblock" id="code">
 <h3>Manipola i valori dell'elemento - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Dizionario dei Tag e Metadati">}}

<p>La classe <code>TagDictionary</code> fornisce un registro dei tag DICOM noti insieme ai loro metadati &mdash; keyword, descrizione, Value Representation, molteplicità del valore e stato di ritiro. È possibile cercare i tag per keyword o ispezionare i loro metadati programmaticamente:</p>

<div class="codeblock" id="code">
 <h3>Interroga il dizionario dei tag - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Supporto ai Tag Privati">}}

<p>DICOM consente a fornitori e organizzazioni di definire tag privati per dati proprietari. Aspose.Medical per .NET supporta pienamente la lettura, scrittura e registrazione dei tag privati. È possibile caricare dizionari di tag personalizzati da file XML per abilitare una corretta gestione degli elementi privati:</p>

<div class="codeblock" id="code">
 <h3>Lavora con i tag privati - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Lavora con le Sequenze DICOM">}}

<p>Le sequenze DICOM (tipo VR SQ) contengono dataset annidati, formando una struttura ad albero all'interno del file DICOM. La classe <code>Sequence</code> fornisce accesso tipizzato agli elementi della sequenza:</p>

<div class="codeblock" id="code">
 <h3>Leggi e crea sequenze DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Gruppi di Tag DICOM comuni">}}

<p>La tabella seguente elenca i gruppi di tag DICOM più utilizzati e i tag di esempio accessibili tramite la classe <code>Tag</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Gruppo</th>
<th>Categoria</th>
<th>Tag di Esempio</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Informazioni Meta del File</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Identificazione dello Studio</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Informazioni del Paziente</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Parametri di Acquisizione</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Posizionamento dell'Immagine</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Attributi Pixel dell'Immagine</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Dati Pixel</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Risorse di Apprendimento" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentazione" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Codice Sorgente" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Riferimenti API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Supporto al Prodotto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Supporto Gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Supporto a Pagamento" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Perché Aspose.Medical per .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Elenco Clienti" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Storie di Successo" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
