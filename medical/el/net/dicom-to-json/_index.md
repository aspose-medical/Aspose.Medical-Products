---
title: Μετατροπή DICOM σε JSON σε C# .NET | Aspose.Medical
weight: 4000
description: Σειριοποίηση αρχείων DICOM σε τυπική μορφή DICOM JSON σε C# .NET. Διαμορφώστε τη διαχείριση αριθμών, τις bulk data URIs, την έξοδο λέξεων-κλειδιών και την ασύγχρονη επεξεργασία ροής με το API του Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Μετατροπή DICOM σε JSON σε .NET C#" h2="Σειριοποίηση συνόλων δεδομένων DICOM και αρχείων σε τυπικό Μοντέλο DICOM JSON (PS3.18). Διαμορφώστε τη διαχείριση αριθμών, τις bulk data references, την έξοδο λέξεων-κλειδιών και την επεξεργασία ροής-βασισμένη async." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Τυπικό Μοντέλο DICOM JSON">}}

<p><strong>Aspose.Medical for .NET</strong> σειριοποιεί δεδομένα DICOM σε JSON ακολουθώντας το <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>. Αυτό είναι το επίσημο πρότυπο για την αναπαράσταση συνόλων δεδομένων DICOM σε JSON, που χρησιμοποιείται από τις υπηρεσίες DICOMweb, τους πόρους FHIR ImagingStudy και τα σύγχρονα APIs υγειονομικής περίθαλψης.</p>

<p>Στο DICOM JSON Model, κάθε χαρακτηριστικό αντιπροσωπεύεται από την ετικέτα του ως κλειδί JSON (π.χ., <code>\"00100010\"</code> για το Patient Name), με την Value Representation και τον πίνακα τιμών ως ένθετες ιδιότητες:</p>

<div class="codeblock" id="code">
 <h3>Μορφή DICOM JSON Model</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Σειριοποίηση DICOM σε JSON σε C#">}}

<p>Χρησιμοποιήστε την κλάση <code>DicomJsonSerializer</code> για να μετατρέψετε ένα αρχείο DICOM ή σύνολο δεδομένων σε JSON. Η πιο απλή προσέγγιση παράγει συμπαγής έξοδο που συμμορφώνεται με τα πρότυπα:</p>

<div class="codeblock" id="code">
 <h3>Μετατροπή DICOM σε JSON - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p>Μπορείτε να σειριοποιήσετε ένα <code>Dataset</code>, ένα πλήρες <code>DicomFile</code> (συμπεριλαμβανομένων των File Meta Information), ή έναν πίνακα από σύνολα δεδομένων:</p>

<div class="codeblock" id="code">
 <h3>Σειριοποίηση DicomFile και πινάκων συνόλων δεδομένων - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Σειριοποίηση βάσει ροής και ασύγχρονη">}}

<p>Για μεγάλα αρχεία DICOM ή σενάρια υψηλής διαπερατότητας, σειριοποιήστε απευθείας σε ροή UTF-8 ώστε να αποφύγετε την εκχώρηση μεγάλων συμβολοσειρών στη μνήμη. Διατίθενται τόσο συγχρονικές όσο και async μέθοδοι:</p>

<div class="codeblock" id="code">
 <h3>Ροή και ασύγχρονη σειριοποίηση - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Επιλογές Σειριοποίησης">}}

<p>Η κλάση <code>DicomJsonSerializerOptions</code> ελέγχει πώς τα δεδομένα DICOM αναπαρίσονται σε JSON:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Ιδιότητα</th>
<th>Τύπος</th>
<th>Περιγραφή</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Ελέγχει αν οι αριθμητικές VR (IS, DS, SV, UV) γράφονται ως αριθμοί JSON ή ως συμβολοσειρές</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Χρησιμοποιεί τις λέξεις-κλειδιά DICOM (π.χ., <code>\"PatientName\"</code>) αντί για ετικέτες (<code>\"00100010\"</code>) ως κλειδιά JSON. Σημείωση: μη‑τυπικό</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Συμπεριλαμβάνει τη λέξη‑κλειδί DICOM ως επιπλέον ιδιότητα JSON</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Συμπεριλαμβάνει το όνομα ετικέτας DICOM ως επιπλέον ιδιότητα JSON</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Προσαρμοσμένος μετατροπέας για τη γραφή μεγάλων δεδομένων (π.χ., pixel data) ως παραπομπές URI</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Προσαρμοσμένος φορτωτής για την επίλυση των URI bulk data κατά την απο-σειριοποίηση</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Διαμόρφωση επιλογών σειριοποίησης - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Διαχείριση Αριθμών">}}

<p>Οι αριθμητικές Value Representations του DICOM (IS, DS, SV, UV) μπορούν να σειριοποιηθούν ως αριθμοί JSON ή ως συμβολοσειρές JSON. Αυτό είναι σημαντικό για τη διαλειτουργικότητα, καθώς ορισμένες υλοποιήσεις DICOM αποθηκεύουν αριθμούς με προκαθορισμένα κενά ή μη‑τυπική μορφοποίηση:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Λειτουργία</th>
<th>Έξοδος JSON</th>
<th>Περίπτωση Χρήσης</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>\"00180086\":{\"vr\":\"IS\",\"Value\":[42]}</code></td><td>Συμμορφώνεται με το πρότυπο, ιδανικό για υπολογισμούς. Παράγει εξαίρεση εάν η τιμή δεν μπορεί να αναλυθεί.</td></tr>
<tr><td><code>AsString</code></td><td><code>\"00180086\":{\"vr\":\"IS\",\"Value\":[\"42\"]}</code></td><td>Διατηρεί την αρχική αναπαράσταση συμβολοσειράς. Πιο ασφαλές για αναστρέψιμη μεταφορά μη‑τυπικών δεδομένων.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Διαχείριση Bulk Data">}}

<p>Μεγάλες δυαδικές τιμές (pixel data, waveforms, encapsulated documents) μπορούν να διαχειριστούν ως αναφορές bulk data αντί να ενσωματωθούν στην έξοδο JSON. Αυτό ακολουθεί την προδιαγραφή <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>.</p>

<p>Υλοποιήστε το <code>IBulkDataConverter</code> για την εξωτερική αποθήκευση μεγάλων δεδομένων κατά τη σειριοποίηση, και το <code>IBulkDataLoader</code> για την επίλυση των URI κατά την απο-σειριοποίηση:</p>

<div class="codeblock" id="code">
 <h3>Προσαρμοσμένη διαχείριση bulk data - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Απο-σειριοποίηση JSON σε DICOM">}}

<p>Αναλύει το DICOM JSON ξανά σε αντικείμενα Dataset ή DicomFile. Υποστηρίζει είσοδο συμβολοσειράς, είσοδο ροής και async λειτουργίες ροής:</p>

<div class="codeblock" id="code">
 <h3>Απο-σειριοποίηση JSON σε DICOM - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ενσωμάτωση System.Text.Json">}}

<p>Το Aspose.Medical παρέχει τα <code>DatasetJsonConverter</code> και <code>DicomFileJsonConverter</code> για ενσωμάτωση με το τυπικό pipeline σειριοποίησης του <code>System.Text.Json</code>. Αυτό επιτρέπει στα αντικείμενα DICOM να σειριοποιούνται μαζί με άλλους τύπους .NET στον υπάρχοντα κώδικα επεξεργασίας JSON σας:</p>

<div class="codeblock" id="code">
 <h3>Ενσωμάτωση System.Text.Json - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
</div>

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

{{< blocks/products/pf/slr-tab tabTitle="Γιατί το Aspose.Medical για .NET;" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Λίστα Πελατών" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Ιστορίες Επιτυχίας" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
