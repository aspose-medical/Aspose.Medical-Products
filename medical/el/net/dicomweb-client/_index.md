---
title: Πελάτης DICOMweb σε C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Αναζήτηση, λήψη και αποστολή μελετών DICOM μέσω HTTP από .NET. Ένας πελάτης DICOMweb σε καθαρό C# για QIDO-RS, WADO-RS και STOW-RS που λειτουργεί με PACS, VNA και υπηρεσίες απεικόνισης στο cloud.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Πελάτης DICOMweb για .NET C#" h2="Αναζήτηση, λήψη και αποστολή μελετών σε οποιονδήποτε διακομιστή DICOMweb με QIDO-RS, WADO-RS και STOW-RS, σε διαχειριζόμενο C# χωρίς εγκατάσταση στη μηχανή." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM μέσω HTTP, από τον δικό σας κώδικα">}}

<p>Το DICOMweb είναι η μορφή HTTP του DICOM. Αντί σε μια σύνδεση DIMSE στη θύρα 104, η εφαρμογή σας στέλνει απλά HTTPS αιτήματα: QIDO-RS για αναζήτηση, WADO-RS για λήψη, STOW-RS για αποστολή. Υπηρεσίες απεικόνισης στο cloud όπως η υπηρεσία DICOM του Azure Health Data Services και το Google Cloud Healthcare API λειτουργούν με αυτόν τον τρόπο, όπως και αρχείο όπως το Orthanc και το dcm4chee.</p>

<p><strong>Aspose.Medical για .NET</strong> περιλαμβάνει έναν πελάτη DICOMweb από την έκδοση 26.10. <code>DicomWebClient</code> περιτυλίγει κάθε τυπική συναλλαγή σε μια τυποποιημένη μέθοδο, και τα αποτελέσματα φθάνουν ως τα ίδια αντικείμενα <code>Dataset</code> και <code>DicomFile</code> που η υπόλοιπη βιβλιοθήκη διαβάζει και γράφει. Ο διακομιστής στην άλλη πλευρά μπορεί να είναι οποιαδήποτε υλοποίηση DICOMweb· δεν χρειάζεται να έχει δημιουργηθεί με Aspose.Medical.</p>

<p>Ο πελάτης είναι διαχειριζόμενο C# πάνω από το <code>HttpClient</code>. Δεν υπάρχει εγγενές εργαλείο και δεν υπάρχει υπηρεσία proxy, έτσι ο ίδιος κώδικας εκτελείται σε Windows, σε Linux και σε κοντέινερ.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Σύνδεση σε διακομιστή DICOMweb">}}

<p>Δημιουργήστε τον πελάτη από ένα <code>HttpClient</code> και τη ριζική URL της υπηρεσίας του διακομιστή. Η αυθεντικοποίηση, οι ρυθμίσεις TLS, τα proxies και η συγκέντρωση συνδέσεων παραμένουν στο <code>HttpClient</code>, όπου η εφαρμογή σας τα ρυθμίζει ήδη. Ο πελάτης δεν κατέχει το <code>HttpClient</code> και δεν το διαγράφει.</p>

<div class="codeblock" id="code">
 <h3>Δημιουργία πελάτη DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Αν μια υλοποίηση εξυπηρετεί WADO-URI ή άλλη υπηρεσία από διαφορετική URL, το <code>ServiceBaseUris</code> αντικαθιστά τη ρίζα μόνο για εκείνη την υπηρεσία.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αναζήτηση μελετών με QIDO-RS">}}

<p>Μια αναζήτηση παίρνει παραμέτρους αντιστοίχισης με τη λέξη-κλειδί DICOM ως όνομα, τα πεδία προς επιστροφή και ελέγχους σελιδοποίησης. Τα αποτελέσματα φτάνουν ως ροή από datasets, διαβάζονται ένα τη φορά ενώ η απόκριση εξακολουθεί να κατεβαίνει.</p>

<div class="codeblock" id="code">
 <h3>Βρείτε τις μελέτες ενός ασθενούς - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p>Οι μέθοδοι <code>SearchSeriesAsync</code> και <code>SearchInstancesAsync</code> αναζητούν ένα επίπεδο πιο κάτω, σε όλο το αρχείο ή μέσα σε μια μελέτη ή σειρά. Οι ίδιες επιλογές επίσης ορίζουν ασαφή αντιστοίχιση, αντιστοίχιση κενών τιμών και το offset του αποτελέσματος για σελιδοποίηση.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Λήψη μελέτης με WADO-RS">}}

<p>Μια μελέτη μπορεί να περιέχει χιλιάδες instances. Η <code>RetrieveStudyAsync</code> τα επιστρέφει ως ροή, ώστε κάθε αρχείο να αναλύεται καθώς φθάνει το αντίστοιχο τμήμα της απόκρισης και μπορεί να αποθηκευτεί πριν διαβαστεί το επόμενο. Η χρήση μνήμης δεν αυξάνεται με το μέγεθος της μελέτης.</p>

<div class="codeblock" id="code">
 <h3>Αποθήκευση κάθε instance μιας μελέτης - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p>Οι μέθοδοι <code>RetrieveSeriesAsync</code> και <code>RetrieveInstanceAsync</code> κατεβάζουν μια σειρά ή ένα instance με τον ίδιο τρόπο.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Λήψη μικρογραφίας ή αποδοθείσας εικόνας">}}

<p>Ένας προβολέας ή μια οθόνη λίστας εργασιών συχνά χρειάζεται μια εικόνα, όχι ένα αρχείο DICOM. Ο διακομιστής την αποδίδει και στέλνει ένα JPEG ή PNG, ώστε ο πελάτης να μην κατεβάζει ή αποκωδικοποιεί τα αρχικά δεδομένα pixel.</p>

<div class="codeblock" id="code">
 <h3>Αποθήκευση μικρογραφίας JPEG ενός instance - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p>Οι μέθοδοι <code>RetrieveRenderedInstanceAsync</code> και <code>RetrieveRenderedFramesAsync</code> επιστρέφουν αντί αυτού μια εικόνα πλήρους μεγέθους. Το <code>DicomWebRenderOptions</code> μεταβιβάζει τα τυπικά στοιχεία απόδοσης στον διακομιστή: ποιότητα εικόνας, μέγεθος οπτικής περιοχής, παράθυρο και προφίλ ICC.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αποστολή instances με STOW-RS">}}

<p>Το STOW-RS στέλνει οποιονδήποτε αριθμό instances σε ένα αίτημα. Ο διακομιστής απαντά με μια αναφορά που απαριθμεί, instance κατά instance, όσα αποθήκευσε και όσα απορρέασε.</p>

<div class="codeblock" id="code">
 <h3>Αποστολή φακέλου DICOM αρχείων - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>Κάποιοι διακομιστές απαντούν με σφάλμα HTTP, όπως 409, όταν ένα ή περισσότερα instances αποτύχουν. Η κλήση τότε πετάει <code>DicomWebClientException</code>, όπως δείχνει η επόμενη ενότητα, και η ίδια αναφορά βρίσκεται στο σώμα της απόκρισης της εξαίρεσης.</p>

<p>Η <code>StoreStudyAsync</code> αποστέλλει σε μια ονομασμένη μελέτη. Και οι δύο μέθοδοι δέχονται επίσης ένα <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, ώστε τα αρχεία να μπορούν να δημιουργούνται ενώ το αίτημα αποστέλλεται.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Διαχείριση σφαλμάτων διακομιστή">}}

<p>Κάθε τυποποιημένη λειτουργία ελέγχει την κατάσταση HTTP πριν επιστρέψει. Όταν ο διακομιστής απορρίπτει ένα αίτημα, η κλήση πετάει <code>DicomWebClientException</code>, το οποίο μεταφέρει τον κωδικό κατάστασης, τις κεφαλίδες και την αρχή του σώματος της απάντησης για το αρχείο καταγραφής.</p>

<div class="codeblock" id="code">
 <h3>Αναφορά αποτυχημένου αιτήματος - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>Οι μέθοδοι που επιστρέφουν ακατέργαστη απόκριση, όπως η παραπάνω μικρογραφία, αφήνουν τον έλεγχο κατάστασης σε εσάς μέσω του <code>IsSuccessStatusCode</code> ή του <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Τι άλλο καλύπτει ο πελάτης">}}

<p>Η αναζήτηση, η λήψη και η αποστολή είναι οι κοινές κλήσεις. Ο πελάτης διαθέτει μια ομάδα λειτουργιών για καθένα από τις άλλες υπηρεσίες DICOMweb επίσης:</p>

<ul>
<li>Μαζικά δεδομένα, δεδομένα pixel και επιλεγμένα πλαίσια ενός instance, με βάση τον αριθμό πλαισίου 1-based, όπως τα έστειλε ο διακομιστής.</li>
<li>WADO-URI, η παλαιότερη ανάκτηση με μοναδικό URL που πολλοί προβολείς και αρχεία εξακολουθούν να χρησιμοποιούν, για αρχεία DICOM και αποδιδόμενες εικόνες.</li>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> λίστες εργασιών: δημιουργία, αναζήτηση, ενημέρωση και ανάληψη αντικειμένων εργασίας, αλλαγή της κατάστασής τους, αίτημα ακύρωσης και εγγραφή σε συμβάντα που φθάνουν μέσω WebSocket.</li>
<li>Η <a href="/medical/net/modality-worklist/">λίστα εργασιών συσκευής</a> (MWL-RS), τα βήματα εκτέλεσης διαδικασίας συσκευής (MPPS-RS) και η δέσμευση αποθήκευσης.</li>
<li>Instances που δεν αφορούν ασθενή, όπως παλέτες χρωμάτων και hanging protocols, καθώς και το έγγραφο δυνατοτήτων του διακομιστή.</li>
<li><code>SendAsync</code> για οποιαδήποτε τυπική συναλλαγή με τις δικές σας τιμές διαδρομής, παραμέτρους ερωτήματος, κεφαλίδες και σώμα, όταν μια τυποποιημένη μέθοδος δεν ταιριάζει.</li>
</ul>

<p>Ο <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">οδηγός πελάτη DICOMweb</a> τεκμηριώνει κάθε λειτουργία και επιλογή, και η <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">αναφορά λειτουργιών πελάτη</a> καταγράφει κάθε μέθοδο.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Μία βιβλιοθήκη από την απόκριση HTTP μέχρι τα δεδομένα pixel">}}

<p>Ένα αρχείο που κατεβάζεται με τον πελάτη είναι ένα κανονικό <code>DicomFile</code>. Μπορεί να ανωνυμοποιηθεί, να μετακωδικοποιηθεί, να αποδοθεί ή να σειριοποιηθεί στην ίδια διαδικασία, χωρίς βήμα μετατροπής και χωρίς δεύτερη βιβλιοθήκη στην αλυσίδα.</p>

<p>Σχετικές σελίδες: <a href="/medical/net/dicomweb-server/">Διακομιστής DICOMweb</a> για την άλλη πλευρά του πρωτοκόλλου στην δική σας υπηρεσία ASP.NET Core, <a href="/medical/net/dicom-networking/">δικτύωση DICOM</a> για αρχεία που χρησιμοποιούν DIMSE αντί για HTTP, <a href="/medical/net/anonymization/">ανωνυμοποίηση</a> πριν μια μελέτη φύγει από το νοσοκομείο, <a href="/medical/net/dicom-transfer-syntax-conversion/">μετατροπή συντακτικού μεταφοράς</a> για ό,τι αποθηκεύει ο διακομιστής, και <a href="/medical/net/dicom-to-json/">DICOM σε JSON</a> για τη μορφή που χρησιμοποιούν τα αποτελέσματα αναζήτησης QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πηγές Μάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Οδηγός Προγραμματιστή" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="Αναφορές API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Υποστήριξη Προϊόντος" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Δωρεάν Υποστήριξη" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Πληρωμένη Υποστήριξη" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Ιστολόγιο" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Γιατί Aspose.Medical για .NET;" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Κατάλογος Πελατών" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Ιστορίες Επιτυχίας" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
