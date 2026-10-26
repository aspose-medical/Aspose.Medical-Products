---
title: Πελάτης DICOMweb σε C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Αναζήτηση, λήψη και ανέβασμα DICOM μελετών μέσω HTTP από .NET. Ένας πελάτης DICOMweb σε καθαρό C# για QIDO-RS, WADO-RS και STOW-RS που λειτουργεί με PACS, VNA και υπηρεσίες cloud απεικόνισης.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Πελάτης DICOMweb για .NET C#" h2="Αναζήτηση, λήψη και ανέβασμα μελετών σε οποιονδήποτε διακομιστή DICOMweb με QIDO-RS, WADO-RS και STOW-RS, σε διαχειριζόμενο C# χωρίς καμία εγκατάσταση στο μηχάνημα." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM μέσω HTTP, από τον δικό σας κώδικα">}}

<p>Το DICOMweb είναι η μορφή HTTP του DICOM. Αντί για μια σύνδεση DIMSE στη θύρα 104, η εφαρμογή σας στέλνει κανονικά αιτήματα HTTPS: QIDO-RS για αναζήτηση, WADO-RS για λήψη, STOW-RS για ανέβασμα. Υπηρεσίες cloud απεικόνισης όπως η υπηρεσία DICOM του Azure Health Data Services και το Google Cloud Healthcare API λειτουργούν με αυτόν τον τρόπο, όπως επίσης και αρχεία όπως το Orthanc και το dcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> περιλαμβάνει έναν πελάτη DICOMweb από την έκδοση 26.10. <code>DicomWebClient</code> περιβάλλει κάθε τυπική συναλλαγή σε μια τυποποιημένη μέθοδο, και τα αποτελέσματα φθάνουν ως τα ίδια αντικείμενα <code>Dataset</code> και <code>DicomFile</code> που η υπόλοιπη βιβλιοθήκη διαβάζει και γράφει. Ο διακομιστής από την άλλη πλευρά μπορεί να είναι οποιαδήποτε υλοποίηση DICOMweb· δεν χρειάζεται να δημιουργηθεί με Aspose.Medical.</p>

<p>Ο πελάτης είναι διαχειριζόμενο C# πάνω από το <code>HttpClient</code>. Δεν υπάρχει εγγενές εργαλείο και δεν υπάρχει υπηρεσία διαμεσολάβησης, έτσι ο ίδιος κώδικας εκτελείται σε Windows, σε Linux και σε ένα container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Σύνδεση σε διακομιστή DICOMweb">}}

<p>Δημιουργήστε τον πελάτη από ένα <code>HttpClient</code> και το βασικό URL της υπηρεσίας του διακομιστή. Η πιστοποίηση, οι ρυθμίσεις TLS, οι διαμεσολαβητές και η συγκέντρωση συνδέσεων παραμένουν στο <code>HttpClient</code>, όπου η εφαρμογή σας τα έχει ήδη ρυθμίσει. Ο πελάτης δεν κατέχει το <code>HttpClient</code> και δεν το διαγράφει.</p>

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

<p>Εάν μια ανάπτυξη εξυπηρετεί WADO-URI ή άλλη υπηρεσία από διαφορετικό URL, το <code>ServiceBaseUris</code> παρακάμπτει τη ρίζα μόνο για αυτή την υπηρεσία.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αναζήτηση μελετών με QIDO-RS">}}

<p>Μια αναζήτηση δέχεται παραμέτρους αντιστοίχισης με τη λέξη-κλειδί DICOM ως όνομα, τα πεδία που θα επιστραφούν και ελέγχους σελιδοποίησης. Τα αποτελέσματα φθάνουν ως ροή συνόλων δεδομένων, διαβάζονται ένα προς ένα ενώ η απόκριση εξακολουθεί να κατεβαίνει.</p>

<div class="codeblock" id="code">
 <h3>Εύρεση των μελετών ενός ασθενούς - C#</h3>
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

<p>Τα <code>SearchSeriesAsync</code> και <code>SearchInstancesAsync</code> αναζητούν ένα επίπεδο πιο κάτω, σε ολόκληρο το αρχείο ή μέσα σε μία μελέτη ή σειρά. Οι ίδιες επιλογές ρυθμίζουν επίσης ασαφή αντιστοίχιση, αντιστοίχιση κενής τιμής και την μετατόπιση αποτελεσμάτων για σελιδοποίηση.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Λήψη μελέτης με WADO-RS">}}

<p>Μία μελέτη μπορεί να περιέχει χιλιάδες στιγμιότυπα. Το <code>RetrieveStudyAsync</code> τα επιστρέφει ως ροή, ώστε κάθε αρχείο να αναλύεται καθώς φτάνει το τμήμα της απόκρισης και να μπορεί να αποθηκευτεί πριν διαβαστεί το επόμενο. Η χρήση μνήμης δεν αυξάνεται με το μέγεθος της μελέτης.</p>

<div class="codeblock" id="code">
 <h3>Αποθήκευση κάθε στιγμιότυπου μιας μελέτης - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p>Τα <code>RetrieveSeriesAsync</code> και <code>RetrieveInstanceAsync</code> κατεβάζουν μία σειρά ή ένα στιγμιότυπο με τον ίδιο τρόπο.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Λήψη μικρογραφίας ή αποδιδόμενης εικόνας">}}

<p>Ένας προεξόπτης ή μια οθόνη λίστας εργασιών συχνά χρειάζεται μια εικόνα, όχι ένα αρχείο DICOM. Ο διακομιστής την αποδίδει και στέλνει ένα JPEG ή PNG, έτσι ο πελάτης δεν κατεβάζει ή αποκρυπτογραφεί τα αρχικά δεδομένα pixel.</p>

<div class="codeblock" id="code">
 <h3>Αποθήκευση μικρογραφίας JPEG ενός στιγμιότυπου - C#</h3>
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

<p>Τα <code>RetrieveRenderedInstanceAsync</code> και <code>RetrieveRenderedFramesAsync</code> επιστρέφουν αντ' αυτού μια εικόνα πλήρους μεγέθους. Το <code>DicomWebRenderOptions</code> περνά τα τυπικά ελέγχους απόδοσης στον διακομιστή: ποιότητα εικόνας, μέγεθος παραθύρου προβολής, παράθυρο και προφίλ ICC.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ανέβασμα στιγμιοτύπων με STOW-RS">}}

<p>Το STOW-RS στέλνει οποιονδήποτε αριθμό στιγμιοτύπων σε ένα αίτημα. Ο διακομιστής απαντά με μια έκθεση που καταγράφει, στιγμιότυπο προς στιγμιότυπο, τι αποθήκευσε και τι απέρριψε.</p>

<div class="codeblock" id="code">
 <h3>Ανέβασμα φακέλου με DICOM αρχεία - C#</h3>
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

<p>Ορισμένοι διακομιστές απαντούν με σφάλμα HTTP, όπως 409, όταν ένα ή περισσότερα στιγμιότυπα αποτύχουν. Η κλήση τότε πετάει <code>DicomWebClientException</code>, όπως φαίνεται στην επόμενη ενότητα, και η ίδια έκθεση βρίσκεται στο σώμα της απόκρισης της εξαίρεσης.</p>

<p>Το <code>StoreStudyAsync</code> ανεβάζει σε μία ονομαστική μελέτη. Και οι δύο μέθοδοι δέχονται επίσης ένα <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, ώστε τα αρχεία να μπορούν να παράγονται ενώ το αίτημα αποστέλλεται.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Διαχείριση σφαλμάτων διακομιστή">}}

<p>Κάθε τυποποιημένη λειτουργία ελέγχει την κατάσταση HTTP πριν επιστρέψει. Όταν ο διακομιστής αρνείται ένα αίτημα, η κλήση πετάει <code>DicomWebClientException</code>, το οποίο μεταφέρει τον κωδικό κατάστασης, τις κεφαλίδες και την αρχή του σώματος της απόκρισης για το αρχείο καταγραφής.</p>

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

<p>Μέθοδοι που επιστρέφουν ακατέργαστη απόκριση, όπως η μικρογραφία παραπάνω, αφήνουν τον έλεγχο κατάστασης σε εσάς μέσω των <code>IsSuccessStatusCode</code> ή <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Τι άλλο καλύπτει ο πελάτης">}}

<p>Αναζήτηση, λήψη και ανέβασμα είναι οι κοινές κλήσεις. Ο πελάτης διαθέτει ομάδα λειτουργιών για κάθε μία από τις άλλες υπηρεσίες DICOMweb επίσης:</p>

<ul>
<li>Μαζικά δεδομένα, δεδομένα pixel και επιλεγμένα πλαίσια ενός στιγμιότυπου, με βάση τον αριθμό πλαισίου που αρχίζει από το 1, όπως τα έστειλε ο διακομιστής.</li>
<li>WADO-URI, η παλαιότερη ανάκτηση μοναδικού URL που πολλοί προεξοπίστεις και αρχεία εξακολουθούν να χρησιμοποιούν, για DICOM αρχεία και αποδιδόμενες εικόνες.</li>
<li>Λίστες εργασιών UPS-RS: δημιουργία, αναζήτηση, ενημέρωση και ανάληψη στοιχείων εργασίας, αλλαγή της κατάστασής τους, αίτημα ακύρωσης και εγγραφή σε γεγονότα που φτάνουν μέσω WebSocket.</li>
<li>Η λίστα εργασιών απεικόνισης (MWL-RS), τα βήματα διαδικασιών που εκτελέστηκαν από την απεικόνιση (MPPS-RS) και η δέσμευση αποθήκευσης.</li>
<li>Στιγμιότυπα που δεν αφορούν ασθενή, όπως χρωματικές παλέτες και πρωτόκολλα τοποθέτησης, καθώς και το έγγραφο δυνατοτήτων του διακομιστή.</li>
<li><code>SendAsync</code> για οποιαδήποτε τυπική συναλλαγή με τις δικές σας τιμές διαδρομής, παραμέτρους ερωτήματος, κεφαλίδες και σώμα, όταν μια τυποποιημένη μέθοδος δεν ταιριάζει.</li>
</ul>

<p>Ο <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">οδηγός πελάτη DICOMweb</a> τεκμηριώνει κάθε λειτουργία και επιλογή, και η <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">αναφορά λειτουργιών πελάτη</a> καταγράφει κάθε μέθοδο.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Μία βιβλιοθήκη από την απόκριση HTTP μέχρι τα δεδομένα pixel">}}

<p>Ένα αρχείο που λήφθηκε με τον πελάτη είναι ένα κανονικό <code>DicomFile</code>. Μπορεί να ανωνυμοποιηθεί, να μετατραπεί, να αποδοθεί ή να σειριοποιηθεί στην ίδια διαδικασία, χωρίς βήμα μετατροπής και χωρίς δεύτερη βιβλιοθήκη στην αλυσίδα.</p>

<p>Σχετικές σελίδες: <a href="/medical/net/dicom-networking/">Δικτύωση DICOM</a> για αρχεία που χρησιμοποιούν DIMSE αντί για HTTP, <a href="/medical/net/anonymization/">ανωνυμοποίηση</a> πριν μια μελέτη φύγει από το νοσοκομείο, <a href="/medical/net/dicom-transfer-syntax-conversion/">μετατροπή σύνταξης μεταφοράς</a> για ό,τι αποθηκεύει ο διακομιστής, και <a href="/medical/net/dicom-to-json/">DICOM σε JSON</a> για τη μορφή που χρησιμοποιούν τα αποτελέσματα αναζήτησης QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι Μάθησης" tabId="resources" >}}
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
{{< blocks/products/pf/slr-element name="Λίστα Πελατών" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Ιστορίες Επιτυχίας" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
