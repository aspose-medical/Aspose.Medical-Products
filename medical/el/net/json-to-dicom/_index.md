---
title: Μετατροπή JSON σε DICOM σε C# .NET | Aspose.Medical
weight: 6000

description: Δημιουργήστε αρχεία DICOM από το πρότυπο DICOM JSON Model (PS3.18) σε C# .NET. Διαβάστε JSON από συμβολοσειρά, ροή ή αγωγό, ραγδαία μεταδώστε μια ακολουθία συνόλων δεδομένων και επιλύστε αναφορές bulk data με το Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Μετατροπή JSON σε DICOM σε .NET C#" h2="Διαβάστε το πρότυπο DICOM JSON Model (PS3.18) ξανά σε σύνολα δεδομένων και αρχεία DICOM. Εργαστείτε από συμβολοσειρά, ροή ή αγωγό, μεταδώστε μια ακολουθία μελετών και επιλύστε αναφορές bulk data." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Από DICOM JSON σε αρχείο DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> διαβάζει το <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>, την αναπαράσταση που χρησιμοποιείται από τις υπηρεσίες DICOMweb και από συστήματα που ανταλλάσσουν μελέτες μέσω HTTP. Αυτό που φθάνει ως JSON μετατρέπεται σε <code>Dataset</code>, και ένα <code>Dataset</code> αποθηκεύεται στον δίσκο ως αρχείο DICOM.</p>

<p>Αυτή είναι η αντίστροφη κατεύθυνση της σελίδας <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>, και τα δύο χρησιμοποιούν την ίδια κλάση, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Δημιουργία αρχείου DICOM από JSON - C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Ένα σύνολο δεδομένων που δεν περιέχει File Meta Information γράφεται με την προεπιλεγμένη σύνταξη μεταφοράς, Implicit VR Little Endian, όταν εφοδιάζεται σε ένα <code>DicomFile</code>.</p>

<p>Η ανάγνωση DICOM JSON είναι λειτουργία με άδεια. Χωρίς εφαρμοσμένη άδεια on-premise, ο αναγνώστης εγείρει ένα <code>MedicalApiException</code>, επομένως εφαρμόστε πρώτα την άδεια, όπως περιγράφεται στον <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">οδηγό αδειοδότησης</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Διατήρηση του File Meta Information">}}

<p>Η <code>Deserialize</code> επιστρέφει μόνο το σύνολο δεδομένων. Όταν το έγγραφο JSON επίσης περιλαμβάνει την ομάδα File Meta Information, για παράδειγμα επειδή δημιουργήθηκε από ένα πλήρες αρχείο DICOM, η <code>DeserializeFile</code> επιστρέφει ένα <code>DicomFile</code> με εκείνη την ομάδα αμετάβλητη, συμπεριλαμβανομένης της σύνταξης μεταφοράς που δηλώνει το αρχείο.</p>

<div class="codeblock" id="code">
 <h3>Ανάγνωση πλήρους αρχείου DICOM από JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ροές, αγωγοί και ασύγχρονη λειτουργία">}}

<p>Κάθε σημείο εισόδου διαθέτει υπερφόρτωση για ροή και ασύγχρονη υπερφόρτωση, και οι ασύγχρονες επίσης δέχονται ένα <code>PipeReader</code>. Ένα έγγραφο που προέρχεται από web response ή από δίσκο διαβάζεται χωρίς να μετατραπεί πρώτα σε συμβολοσειρά, κάτι που είναι σημαντικό όταν το JSON περιέχει pixel data.</p>

<div class="codeblock" id="code">
 <h3>Ανάγνωση JSON από ροή - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ακολουθία συνόλων δεδομένων, ένα κάθε φορά">}}

<p>Μία ερώτηση DICOMweb επιστρέφει έναν πίνακα συνόλων δεδομένων, και ένα τέτοιο έγγραφο μπορεί να είναι μεγάλο. Η <code>DeserializeList</code> διαβάζει ολόκληρο τον πίνακα στη μνήμη· η <code>DeserializeAsyncEnumerable</code> αποδίδει ένα σύνολο δεδομένων τη φορά, έτσι το έγγραφο δεν κρατιέται ποτέ εξ ολοκλήρου.</p>

<div class="codeblock" id="code">
 <h3>Διαβίβαση πίνακα συνόλων δεδομένων - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αναφορές bulk data">}}

<p>Το DICOM JSON Model δεν περιλαμβάνει pixel data ενσωματωμένα. Μεγάλες τιμές αντικαθίστανται με ένα <code>BulkDataURI</code> που δείχνει στα bytes, διατηρώντας το έγγραφο JSON μικρό. Για να επιλύσετε αυτές τις αναφορές κατά την ανάγνωση, δώστε στον σειριοποιητή έναν φορτωτή bulk data. Η <code>DefaultBulkDataLoader</code> ανακτά URIs <code>file</code>, <code>http</code> και <code>https</code> χωρίς έλεγχο ταυτότητας· για ένα αρχείο που απαιτεί διαπιστευτήρια, υλοποιήστε εσείς τις <code>IBulkDataLoader</code> ή <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Επίλυση BulkDataURI κατά την ανάγνωση - C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αντίστροφη διαδικασία με DICOM σε JSON">}}

<p>Οι δύο κατευθύνσεις προορίζονται να χρησιμοποιηθούν μαζί: μια μελέτη εξέρχεται ως JSON, διασχίζει μια web υπηρεσία και επιστρέφει ως αρχείο DICOM. Τίποτα στη διαδικασία δεν εξαρτάται από native code, έτσι η ίδια αντίστροφη διαδικασία λειτουργεί σε Windows, Linux και macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM σε JSON και επιστροφή - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Για τις επιλογές που ελέγχουν την εμφάνιση του JSON, δείτε τη σελίδα <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>. Το ίδιο ζεύγος υπάρχει για XML: <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> και <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>. Ο <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">οδηγός σειριοποίησης JSON</a> καλύπτει ολόκληρο το API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι Εκμάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Οδηγός Προγραμματιστών" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="Αναφορές API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Υποστήριξη Προϊόντος" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Δωρεάν Υποστήριξη" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Πληρωμένη Υποστήριξη" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Ιστολόγιο" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Γιατί Aspose.Medical για .NET;" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Λίστα Πελατών" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Ιστορίες Επιτυχίας" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}