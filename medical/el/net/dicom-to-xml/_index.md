---
title: Μετατροπή DICOM σε XML με C# .NET | Aspose.Medical
weight: 3000
description: Σειριοποιήστε τα DICOM σύνολα δεδομένων σε τυπική μορφή DICOM XML με C# .NET. Διαμορφώστε τη διαχείριση bulk δεδομένων, την επεξεργασία βασισμένη σε ροές και τις async λειτουργίες με το Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Μετατροπή DICOM σε XML με .NET C#" h2="Σειριοποιήστε τα DICOM σύνολα δεδομένων στην τυπική αναπαράσταση DICOM XML (PS3.19). Διαμορφώστε τις αναφορές bulk δεδομένων, την έξοδο βασισμένη σε ροή και την async επεξεργασία με μια καθαρή βιβλιοθήκη .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Σειριοποιήση DICOM XML βάσει προτύπων">}}

<p><strong>Aspose.Medical for .NET</strong> σειριοποιεί δεδομένα DICOM σε XML ακολουθώντας το <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a>. Αυτό είναι το επίσημο πρότυπο για την αναπαράσταση DICOM συνόλων δεδομένων σε XML, που χρησιμοποιείται από υπηρεσίες DICOMweb, πλατφόρμες ενσωμάτωσης και συστήματα που απαιτούν ανθρώπινα αναγνώσιμη, επικυρωμένη κατά σχήμα, αναπαράσταση μεταδεδομένων ιατρικής απεικόνισης.</p>

<p>Η κλάση <code>DicomXmlSerializer</code> παρέχει στατικές μεθόδους για σειριοποιήση και αποσειριοποιήση. Σε αντίθεση με απλές προσεγγίσεις εξαγωγής ετικετών, η έξοδος συμμορφώνεται με το σχήμα DICOM XML, όπου κάθε στοιχείο αντιπροσωπεύεται με την ετικέτα του, το VR και τις σωστά μορφοποιημένες τιμές &mdash; επιτρέποντας απώλειας-μη-μετατροπής μετατροπή με κλειστό βρόχο μεταξύ δυαδικού DICOM και XML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Σειριοποιήστε DICOM σε XML με C#">}}

<p>Χρησιμοποιήστε την κλάση <code>DicomXmlSerializer</code> για να μετατρέψετε ένα σύνολο δεδομένων DICOM σε συμβολοσειρά XML. Η πιο απλή προσέγγιση παράγει ένα XML έγγραφο σύμφωνο με τα πρότυπα:</p>

<div class="codeblock" id="code">
 <h3>Μετατροπή DICOM σε XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Σειριοποιήση βασισμένη σε ροή και Async">}}

<p>Για μεγάλα αρχεία DICOM ή σενάρια υψηλής διαπερατότητας, σειριοποιήστε άμεσα σε ροή ώστε να αποφύγετε τη δέσμευση μεγάλων συμβολοσειρών στη μνήμη. Διατίθενται τόσο συγχρονικές όσο και async μέθοδοι:</p>

<div class="codeblock" id="code">
 <h3>Συγχρονική σειριοποιήση ροής - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Async σειριοποιήση ροής - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ροή αγωγού για μεγάλες μελέτες">}}

<p>Ολόκληρες μελέτες δεν χρειάζεται να διατηρούνται στη μνήμη. Η <code>DicomXmlSerializer</code> γράφει σε έναν <code>PipeWriter</code> και διαβάζει από έναν <code>PipeReader</code>, έτσι ώστε το XML να μπορεί να παράγεται και να καταναλώνεται καθώς ρέει, και μια ακολουθία συνόλων δεδομένων να διαβάζεται ένα προς ένα μέσω <code>DeserializeAsyncEnumerable</code>. Κάθε μέθοδος δέχεται ένα <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Σειριοποιήστε και αποσειριοποιήστε μέσω αγωγού - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Διαβάστε μια ακολουθία συνόλων δεδομένων ένα-ένα - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Επιλογές Σειριοποιήσης">}}

<p>Η κλάση <code>DicomXmlSerializerOptions</code> ελέγχει πώς τα δεδομένα DICOM αντιπροσωπεύονται σε XML. Η κύρια ρύθμιση αφορά τη διαχείριση bulk δεδομένων για μεγάλες δυαδικές τιμές:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Ιδιότητα</th>
<th>Τύπος</th>
<th>Περιγραφή</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Προσαρμοσμένος μετατροπέας για την εγγραφή μεγάλων δεδομένων (π.χ., δεδομένα pixel) ως BulkData URI αναφορές αντί για ενσωμάτωση</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Προσαρμοσμένο φορτωτής για την επίλυση BulkData URI κατά την αποσειριοποιήση</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Προεπιλεγμένο αντικείμενο επιλογών που χρησιμοποιείται όταν δεν παρέχονται προσαρμοσμένες επιλογές</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Σειριοποιήστε με προσαρμοσμένες επιλογές - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Διαχείριση Bulk Δεδομένων">}}

<p>Μεγάλες δυαδικές τιμές (δεδομένα pixel, κυματομορφές, ενσωματωμένα έγγραφα) μπορούν να εξωτερικευτούν ως BulkData URI αναφορές αντί να ενσωματωθούν στην έξοδο XML. Αυτό ακολουθεί την προδιαγραφή <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">στοιχείο DICOM PS3.19 BulkData</a>.</p>

<p>Εφαρμόστε το <code>IBulkDataConverter</code> για την εξωτερικοποίηση μεγάλων δεδομένων κατά τη σειριοποιήση, και το <code>IBulkDataLoader</code> για την επίλυση URI κατά την αποσειριοποιήση. Για τις κοινές περιπτώσεις δεν απαιτείται καν η δημιουργία φορτωτή: το <code>DefaultBulkDataLoader.Instance</code> επιλύει URI <code>file</code>, <code>http</code> και <code>https</code>, και υλοποιεί επίσης το <code>IAsyncBulkDataLoader</code>, έτσι τα bulk δεδομένα ανακτώνται ασύγχρονα στις διαδρομές ροής.</p>

<div class="codeblock" id="code">
 <h3>Προσαρμοσμένη διαχείριση bulk δεδομένων - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
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

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αποσειριοποιήστε XML σε DICOM">}}

<p>Αναλύστε το DICOM XML ξανά σε αντικείμενα Dataset. Υποστηρίζει είσοδο συμβολοσειράς, είσοδο ροής και async λειτουργίες:</p>

<div class="codeblock" id="code">
 <h3>Αποσειριοποιήστε XML σε DICOM - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Σειριοποιήση XML vs JSON">}}

<p>Το Aspose.Medical υποστηρίζει τόσο τη σειριοποιήση DICOM XML (PS3.19) όσο και DICOM JSON (PS3.18). Και οι δύο μορφές προσφέρουν απώλειας-μη-μετατροπής μετατροπή με κλειστό βρόχο, αλλά εξυπηρετούν διαφορετικά σενάρια ενσωμάτωσης:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Χαρακτηριστικό</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Πρότυπο</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>Έλεγχος σχήματος</td><td>XML Schema (XSD) available</td><td>No formal schema</td></tr>
<tr><td>Καλύτερο για</td><td>Enterprise integration, HL7 CDA, audit logs, XDS registries</td><td>DICOMweb, REST APIs, FHIR ImagingStudy</td></tr>
<tr><td>Ανθρωπική αναγνωσιμότητα</td><td>Verbose but self-describing</td><td>Compact and widely supported</td></tr>
<tr><td>Bulk δεδομένα</td><td>BulkData element with URI</td><td>BulkDataURI property</td></tr>
<tr><td>Κλάση Σειριοποιητή</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πηγές Μάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Πηγαίος Κώδικας" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Αναφορές API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Υποστήριξη Προϊόντος" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Δωρεάν Υποστήριξη" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Πληρωμένη Υποστήριξη" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Γιατί Aspose.Medical για .NET;" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Λίστα Πελατών" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Ιστορίες Επιτυχίας" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
