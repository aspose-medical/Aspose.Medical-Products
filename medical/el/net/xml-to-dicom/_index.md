---
title: Μετατροπή XML σε DICOM σε C# .NET | Aspose.Medical
weight: 5000

description: Δημιουργήστε αρχεία DICOM από το Native DICOM Model XML του PS3.19 σε C# .NET. Διαβάστε XML από συμβολοσειρά, ροή ή αγωγό, μεταδώστε διαδοχικά έγγραφα και λύστε τις αναφορές bulk data με το Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Μετατροπή XML σε DICOM σε .NET C#" h2="Διαβάστε το Native DICOM Model XML του PS3.19 ξανά σε σύνολα δεδομένων και αρχεία DICOM. Εργαστείτε από συμβολοσειρά, ροή ή αγωγό, μεταδώστε διαδοχικά έγγραφα και λύστε τις αναφορές bulk data." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Τυπικό Native DICOM Model XML">}}

<p><strong>Aspose.Medical for .NET</strong> διαβάζει το <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a> που ορίζεται στο DICOM PS3.19. Αυτή είναι η αναπαράσταση XML που είναι ενσωματωμένη στο πρότυπο, όχι μια μορφή που δημιούργησε η Aspose, κάτι που την καθιστά χρήσιμη για ενσωμάτωση: ένα σύστημα που ήδη ανταλλάσσει DICOM ως XML παράγει έγγραφα που αυτή η βιβλιοθήκη αποδέχεται.</p>

<p>Η ρίζα του εγγράφου είναι <code>NativeDicomModel</code>, και κάθε attribute είναι ένα στοιχείο <code>DicomAttribute</code> που μεταφέρει το tag, την αναπαράσταση τιμής και τη λέξη-κλειδί:</p>

<div class="codeblock" id="code">
 <h3>Μορφή Native DICOM Model</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>Αυτή η σελίδα είναι η αντίστροφη κατεύθυνση του <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>, και και οι δύο χρησιμοποιούν την ίδια κλάση, <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Δημιουργία αρχείου DICOM από XML σε C#">}}

<p><code>Deserialize</code> μετατρέπει ένα έγγραφο σε <code>Dataset</code>, και ένα dataset γράφεται στο δίσκο ως αρχείο DICOM.</p>

<div class="codeblock" id="code">
 <h3>Δημιουργία αρχείου DICOM από XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Το Native DICOM Model δεν έχει ομάδα File Meta Information, επομένως η συντακτική μεταφορά (transfer syntax) δεν αποτελεί μέρος του εγγράφου. Ένα dataset τυλιγμένο σε <code>DicomFile</code> γράφεται με την προεπιλεγμένη συντακτική μεταφορά, Implicit VR Little Endian. Για να αποθηκεύσετε το αρχείο με διαφορετική, μετακωδικοποιήστε το, όπως δείχνει η σελίδα <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a>.</p>

<p>Η ανάγνωση DICOM XML είναι δυνατότητα με άδεια. Χωρίς εφαρμοσμένη άδεια on-premise, ο αναγνώστης ρίχνει ένα <code>MedicalApiException</code>, οπότε εφαρμόστε πρώτα την άδεια, όπως περιγράφει ο <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">οδηγός αδειοδότησης</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ροές, αγωγοί και ασύγχρονα">}}

<p>Κάθε σημείο εισόδου διαθέτει υπερφόρτωση stream και ασύγχρονη υπερφόρτωση, και οι ασύγχρονες δέχονται επίσης ένα <code>PipeReader</code>. Ένα έγγραφο που προέρχεται από απάντηση ιστού αναλύεται καθώς διαβάζεται, χωρίς να μετατραπεί πρώτα σε συμβολοσειρά.</p>

<div class="codeblock" id="code">
 <h3>Ανάγνωση XML από ροή - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Διαδοχικά έγγραφα σε μία ροή">}}

<p>Μια εξαγωγή από άλλη σύστημα συχνά περιέχει ένα στοιχείο <code>NativeDicomModel</code> μετά το άλλο σε μία ενιαία ροή. Το <code>DeserializeAsyncEnumerable</code> παράγει ένα dataset ανά στοιχείο, με τη σειρά εισόδου, ώστε η ροή να επεξεργάζεται χωρίς να κρατιέται στη μνήμη. Τα στοιχεία ακολουθούν το ένα το άλλο άμεσα: μια δήλωση XML επιτρέπεται μόνο στην πολύ αρχή, όπως σε οποιαδήποτε είσοδο XML.</p>

<div class="codeblock" id="code">
 <h3>Ροή διαδοχικών εγγράφων - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αναφορές bulk data">}}

<p>Μεγάλες τιμές όπως τα δεδομένα pixel δεν γράφονται ενσωματωμένα. Εμφανίζονται ως στοιχείο <code>BulkData</code> με URI που δείχνει στα bytes, διατηρώντας το έγγραφο μικρό. Για την επίλυση αυτών των αναφορών κατά την ανάγνωση, δώστε στον σειριοποιητή ένα bulk data loader. Το <code>DefaultBulkDataLoader</code> ανακτά URIs <code>file</code>, <code>http</code> και <code>https</code> χωρίς πιστοποίηση· για ένα αρχείο που απαιτεί διαπιστευτήρια, υλοποιήστε εσείς το <code>IBulkDataLoader</code> ή το <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Επίλυση bulk data κατά την ανάγνωση - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αντίστροφη διαδικασία με DICOM σε XML">}}

<p>Οι δύο κατευθύνσεις προορίζονται να χρησιμοποιηθούν μαζί: μια μελέτη εξάγεται ως XML, περνάει από σύστημα που επικοινωνεί σε XML και επιστρέφει ως αρχείο DICOM. Όλα διαχειρίζονται από .NET, έτσι η ίδια αντίστροφη διαδικασία λειτουργεί σε Windows, Linux και macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM σε XML και πίσω - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Για τις επιλογές που ελέγχουν την εμφάνιση του XML, δείτε τη σελίδα <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>. Το ίδιο ζεύγος υπάρχει για JSON: <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> και <a href="/medical/net/json-to-dicom/">JSON to DICOM</a>. Ο <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">οδηγός σειριοποίησης</a> καλύπτει ολόκληρο το API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι εκμάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Οδηγός προγραμματιστή" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="Αναφορές API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Υποστήριξη προϊόντος" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Δωρεάν υποστήριξη" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Πληρωμένη υποστήριξη" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Ιστολόγιο" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Γιατί Aspose.Medical για .NET;" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Κατάλογος πελατών" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Ιστορίες επιτυχίας" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}