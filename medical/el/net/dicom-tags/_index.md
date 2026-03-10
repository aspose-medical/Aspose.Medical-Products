---
title: Ανάγνωση & Τροποποίηση ετικετών DICOM σε C# .NET | Aspose.Medical
weight: 15000
description: Ανάγνωση, προσθήκη, ενημέρωση και αφαίρεση ετικετών DICOM σε C# .NET. Πρόσβαση σε μεταδεδομένα ετικετών, εργασία με ιδιωτικές ετικέτες, απαρίθμηση ανά ομάδα και διαχείριση ακολουθιών χρησιμοποιώντας το API του Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Ανάγνωση & Τροποποίηση ετικετών DICOM σε .NET C#" h2="Πρόσβαση, προσθήκη, ενημέρωση και αφαίρεση στοιχείων δεδομένων DICOM με ένα type-safe API. Εργασία με τυπικές ετικέτες, ιδιωτικές ετικέτες, ακολουθίες και το πλήρες λεξικό ετικετών DICOM." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Πρόσβαση ετικετών DICOM με type-safety">}}

<p><strong>Aspose.Medical for .NET</strong> παρέχει ένα ολοκληρωμένο, type-safe API για εργασία με ετικέτες DICOM μέσω της κλάσης <code>Dataset</code>. Κάθε στοιχείο δεδομένων DICOM αντιπροσωπεύεται από ένα <code>Tag</code> &mdash; ένα ζεύγος αριθμών ομάδας και στοιχείου που αναγνωρίζει μοναδικά το χαρακτηριστικό. Το API προσφέρει πρόσβαση ισχυρά τυποποιημένη με γενικές μεθόδους, ασφαλή μοτίβα ανάκτησης που αποφεύγουν εξαιρέσεις, και υποστήριξη για μονο-τιμές, πίνακες και προσπέλαση με δείκτη.</p>

<p>Οι κοινές ετικέτες DICOM είναι διαθέσιμες ως στατικά πεδία στην κλάση <code>Tag</code> (π.χ., <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), εξαλείφοντας την ανάγκη απομνημόνευσης των αριθμητικών κωδικών ετικετών. Κάθε ετικέτα περιλαμβάνει <code>TagMetadata</code> με τη λέξη-κλειδί, περιγραφή, Value Representation (VR), πολλαπλότητα τιμών και κατάσταση κατάργησης.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ανάγνωση ετικετών DICOM σε C#">}}

<p>Η κλάση <code>Dataset</code> παρέχει πολλαπλές μεθόδους για ανάκτηση τιμών ετικετών ανάλογα με τις ανάγκες σας &mdash; μονοτιμής, όλες οι τιμές, προσπέλαση με δείκτη ή ασφαλή ανάκτηση με προεπιλογές:</p>

<div class="codeblock" id="code">
 <h3>Ανάγνωση τιμών ετικετών DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Προσθήκη και Ενημέρωση ετικετών DICOM">}}

<p>Χρησιμοποιήστε το <code>AddOrUpdate</code> για ορισμό τιμών ετικετών. Η μέθοδος δημιουργεί το στοιχείο εάν λείπει η ετικέτα, ή αντικαθιστά την τιμή του εάν υπάρχει ήδη. Μπορείτε να ορίσετε μονοτιμής, πίνακες ή να προσθέσετε πλήρως τυποποιημένα στοιχεία:</p>

<div class="codeblock" id="code">
 <h3>Προσθήκη και ενημέρωση ετικετών DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Αφαίρεση ετικετών DICOM">}}

<p>Η μέθοδος <code>Remove</code> υποστηρίζει την αφαίρεση μιας ετικέτας, πολλαπλών ετικετών ταυτόχρονα, ή ετικετών που ταιριάζουν σε πρόβλεψη &mdash; χρήσιμη για αφαίρεση ολόκληρων ομάδων στοιχείων:</p>

<div class="codeblock" id="code">
 <h3>Αφαίρεση ετικετών DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Απαρίθμηση ετικετών ανά ομάδα">}}

<p>Οι ετικέτες DICOM οργανώνονται σε ομάδες — για παράδειγμα, η ομάδα <code>0x0010</code> περιέχει πληροφορίες ασθενούς, η ομάδα <code>0x0008</code> περιέχει ταυτοποίηση μελέτης, και η ομάδα <code>0x0028</code> περιέχει χαρακτηριστικά εικονοστοιχείου. Χρησιμοποιήστε <code>EnumerateGroup</code> για επανάληψη σε όλα τα στοιχεία μιας συγκεκριμένης ομάδας:</p>

<div class="codeblock" id="code">
 <h3>Απαρίθμηση στοιχείων ανά ομάδα - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Διαχείριση δεδομένων σε επίπεδο στοιχείου">}}

<p>Κάθε στοιχείο DICOM παρέχει μεθόδους για ανάγνωση, προσθήκη, εισαγωγή, αφαίρεση και αντικατάσταση μεμονωμένων τιμών εντός του στοιχείου. Αυτό είναι χρήσιμο για ετικέτες πολλαπλών τιμών όπου απαιτείται λεπτομερής έλεγχος της λίστας τιμών:</p>

<div class="codeblock" id="code">
 <h3>Διαχείριση τιμών στοιχείου - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Λεξικό ετικετών και μεταδεδομένα">}}

<p>Η κλάση <code>TagDictionary</code> παρέχει ένα μητρώο γνωστών ετικετών DICOM μαζί με τα μεταδεδομένα τους — λέξη-κλειδί, περιγραφή, Value Representation, πολλαπλότητα τιμών και κατάσταση κατάργησης. Μπορείτε να αναζητήσετε ετικέτες με βάση τη λέξη-κλειδί ή να ελέγξετε τα μεταδεδομένα τους προγραμματιστικά:</p>

<div class="codeblock" id="code">
 <h3>Αναζήτηση στο λεξικό ετικετών - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Υποστήριξη ιδιωτικών ετικετών">}}

<p>Το DICOM επιτρέπει σε προμηθευτές και οργανισμούς να ορίζουν ιδιωτικές ετικέτες για ιδιόκτητα δεδομένα. Το Aspose.Medical for .NET υποστηρίζει πλήρως την ανάγνωση, εγγραφή και καταχώρηση ιδιωτικών ετικετών. Μπορείτε να φορτώσετε προσαρμοσμένα λεξικά ετικετών από αρχεία XML για να ενεργοποιήσετε τη σωστή διαχείριση ιδιωτικών στοιχείων:</p>

<div class="codeblock" id="code">
 <h3>Εργασία με ιδιωτικές ετικέτες - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Εργασία με ακολουθίες DICOM">}}

<p>Οι ακολουθίες DICOM (VR τύπου SQ) περιέχουν ένθετα datasets, δημιουργώντας μια δομή δέντρου εντός του αρχείου DICOM. Η κλάση <code>Sequence</code> παρέχει τυποποιημένη πρόσβαση στα στοιχεία της ακολουθίας:</p>

<div class="codeblock" id="code">
 <h3>Ανάγνωση και δημιουργία ακολουθιών DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Κοινές ομάδες ετικετών DICOM">}}

<p>Ο παρακάτω πίνακας παραθέτει συχνά χρησιμοποιούμενες ομάδες ετικετών DICOM και παραδείγματα ετικετών προσβάσιμες μέσω της κλάσης <code>Tag</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Ομάδα</th>
<th>Κατηγορία</th>
<th>Παράδειγμα Ετικετών</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Πληροφορίες Μεταδεδομένων Αρχείου</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Ταυτοποίηση Μελέτης</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Πληροφορίες Ασθενούς</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Παράμετροι Λήψης</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Τοποθέτηση Εικόνας</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Χαρακτηριστικά Pixel Εικόνας</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Δεδομένα Pixel</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι Μάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Κώδικας Πηγής" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Αναφορές API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Υποστήριξη Προϊόντος" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Δωρεάν Υποστήριξη" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Πληρωμένη Υποστήριξη" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Ιστολόγιο" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Γιατί το Aspose.Medical for .NET;" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Λίστα Πελατών" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Ιστορίες Επιτυχίας" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
