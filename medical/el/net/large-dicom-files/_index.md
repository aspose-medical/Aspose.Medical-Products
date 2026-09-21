---
title: Εργασία με μεγάλα αρχεία DICOM σε C# .NET | Aspose.Medical
weight: 11500

description: Ανοίξτε μελέτες πολλαπλών καρέ και εικόνες ολόκληρων διαφανειών σε C# χωρίς να τα φορτώνετε στη μνήμη. Διαβάστε τα μεταδεδομένα χωρίς τα δεδομένα πίξελ, αναβάλετε τα μεγάλα στοιχεία και μετακινήστε τα αρχεία μέσω ροών και αγωγών.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Μεγάλα αρχεία DICOM σε .NET C#" h2="Διαβάστε τα μεταδεδομένα μιας μελέτης πολλαπλών καρέ χωρίς τα πίξελ, αναβάλετε τα μεγάλα στοιχεία μέχρι να ζητηθούν, και μετακινήστε ολόκληρα αρχεία μέσω ροών και αγωγών." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Το αρχείο είναι μεγάλο, η ερώτηση είναι συνήθως μικρή">}}

<p>Μια εικόνα ολόκληρης διαφάνειας, μια μεγάλη σειρά CT ή ένας όγκος OCT είναι εκατοντάδες megabytes, και το μεγαλύτερο μέρος αποτελείται από δεδομένα πίξελ. Η εργασία που πραγματικά κάνει μια εφαρμογή είναι συχνά πολύ μικρότερη: να απαριθμήσει τι υπάρχει σε έναν φάκελο, να ελέγξει έναν αναγνωριστικό ασθενούς, να μετρήσει τα καρέ, να αποφασίσει πού θα πρέπει να μεταφερθεί μια μελέτη. Το φορτω­μα κάθε byte για να απαντηθεί αυτό μετατρέπει μια απλή εργασία σε πρόβλημα μνήμης.</p>

<p><strong>Aspose.Medical for .NET</strong> επιτρέπει στον caller να αποφασίσει πόσο από το αρχείο θα διαβαστεί. Η επιλογή είναι ένα όρισμα στη <code>DicomFile.Open</code> και ισχύει για αρχεία, streams και pipes εξίσου.</p>

<p>Μετρήθηκε σε μια μελέτη 14 MB με 128 καρέ από το σύνολο δοκιμών μας, στην ίδια μηχανή και το ίδιο αρχείο:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Στρατηγική ανάγνωσης</th>
<th>Χρόνος ανοίγματος</th>
<th>Κατανεμημένη μνήμη</th>
</tr>
</thead>
<tbody>
<tr><td>Όλα, η προεπιλογή</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Παράλειψη μεγάλων στοιχείων</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Αναβολή μεγάλων στοιχείων</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>Το χάσμα αυξάνεται με το αρχείο. Ένας φάκελος με 10.000 μελέτες είναι η περίπτωση όπου δεν αποτελεί πλέον μικρο-βελτιστοποίηση.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Διαβάστε τα μεταδεδομένα, αφήστε τα πίξελ αμετάβλητα">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> αφήνει έξω από την ανάγνωση κάθε στοιχείο που υπερβαίνει ένα όριο μεγέθους. Το σύνολο δεδομένων που επιστρέφει περιέχει τις ετικέτες που χρειάζεται ένας δείκτης ή ένας router.</p>

<div class="codeblock" id="code">
 <h3>Διαβάστε μια μελέτη χωρίς τα δεδομένα πίξελ - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>Το όριο προεπιλογής είναι 64 kB και δέχεται τιμή σε kilobytes, ώστε μια ροή εργασίας που θεωρεί 8 kB ως μεγάλο να το ορίσει έτσι.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αναβολή αντί για παράλειψη">}}

<p>Όταν τα πίξελ μπορεί να χρειαστούν, αλλά πιθανόν αργότερα και πιθανόν όχι όλα, το <code>ReadLargeOnDemand</code> είναι το άλλο μισό του ζεύγους. Το άνοιγμα του αρχείου κοστίζει το ίδιο όσο η παράλειψη, και ένα μεγάλο στοιχείο διαβάζεται τη στιγμή που ο κώδικας το προσπερνά.</p>

<div class="codeblock" id="code">
 <h3>Φορτώστε ένα καρέ μόνο όταν χρησιμοποιηθεί - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>Η αναβαλλόμενη ανάγνωση είναι μια αδειοδοτημένη δυνατότητα· οι άλλες στρατηγικές λειτουργούν επίσης σε αξιολόγηση.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Δεικτοδοτήστε έναν φάκελο χωρίς να αγγίξετε τα πίξελ">}}

<p>Η ίδια στρατηγική εφαρμόζεται σε μια ροή, η οποία είναι αυτό που φαίνεται ως σάρωση αρχείου ή αποθήκη αντικειμένων στο σύννεφο από τον κώδικα.</p>

<div class="codeblock" id="code">
 <h3>Σάρωση αρχείου - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ροές και αγωγοί, εισόδους και εξόδους">}}

<p>Τanto η ανάγνωση όσο και η εγγραφή δέχονται ροές, και τα ασύγχρονα entry points δέχονται επίσης τύπους <code>System.IO.Pipelines</code>. Μια μελέτη μπορεί να μεταβιβαστεί από την απάντηση δικτύου στην αποθήκευση χωρίς η διαδικασία ποτέ να κρατά ολόκληρο το αρχείο ως ένας πίνακας.</p>

<div class="codeblock" id="code">
 <h3>Ανάγνωση και εγγραφή μέσω ροών - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>Η ίδια ιδέα καλύπτει τις αναπαραστάσεις κειμένου: ένα έγγραφο με πολλά σύνολα δεδομένων διαβάζεται ένα σύνολο δεδομένων τη φορά στις σελίδες <a href="/medical/net/json-to-dicom/">JSON to DICOM</a> και <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Καρέ κατά καρέ">}}

<p>Τα δεδομένα πολλαπλών καρέ αντιμετωπίζονται ανά καρέ, έτσι μια σειρά 500 καρέ κοστίζει ένα καρέ τη φορά αντί για όλο το στοιχείο δεδομένων πίξελ.</p>

<div class="codeblock" id="code">
 <h3>Περιήγηση στα καρέ - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Όπου αυτό αποφασίζει το σχεδιασμό">}}

<ul>
<li>Δεικτοδότηση και μετάβαση αρχείου: εκατομμύρια αρχεία, και μόνο η κεφαλίδα έχει σημασία μέχρι να μεταφερθεί κάτι.</li>
<li>Routers και κόμβοι αποθήκης: δέχονται μια μελέτη, διαβάζουν ό,τι χρειάζεται για να τη δρομολογήσουν, περνούν τα bytes παρακάτω.</li>
<li>AI pipelines: δημιουργήστε το manifest από τα μεταδεδομένα, στη συνέχεια αντλήστε καρέ για το υποσύνολο που πραγματικά εκπαιδεύεται.</li>
<li>Containers με περιορισμό μνήμης: το ενεργό σύνολο ακολουθεί τη στρατηγική, όχι το μέγεθος του αρχείου.</li>
<li>Δεδομένα ολόκληρων διαφανειών και OCT: αρχεία όπου η ανάγνωση όλου δεν είναι καθόλου επιλογή.</li>
</ul>

<p>Ο <a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">οδηγός διαχείρισης μνήμης</a> εξηγεί τις στρατηγικές λεπτομερώς, και το <a href="/medical/net/dicom-networking/">DICOM networking</a> δείχνει τα ίδια δεδομένα να φτάνουν μέσω DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πηγές εκμάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Οδηγός προγραμματιστή" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
{{< blocks/products/pf/slr-element name="Αναφορές API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Υποστήριξη προϊόντος" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Δωρεάν υποστήριξη" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Πληρωμένη υποστήριξη" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Ιστολόγιο" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Γιατί Aspose.Medical για .NET;" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Λίστα πελατών" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Ιστορίες επιτυχίας" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
