---
title: UPS-RS σε C# .NET - DICOMweb Unified Procedure Step Workitems | Aspose.Medical
weight: 9400

description: Δημιουργήστε, αναλάβετε και ολοκληρώστε αντικείμενα εργασίας DICOM UPS από C#, λάβετε αλλαγές κατάστασης μέσω WebSocket, ή εκτελέστε τον δικό σας διακομιστή UPS‑RS στο ASP.NET Core με το Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS για .NET C#" h2="Διανείμετε την απεικονιστική εργασία μεταξύ συστημάτων με το DICOM Unified Procedure Step: δημιουργήστε αντικείμενα εργασίας, επιτρέψτε στους εργαζόμενους να τα αναλάβουν και να τα ολοκληρώσουν, και παρακολουθήστε κάθε αλλαγή, είτε ως πελάτης είτε ως διακομιστής." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Μια ουρά εργασίας που μιλάει DICOM">}}

<p>Ένα μοντέλο AI πρέπει να επεξεργάζεται κάθε νέο CT θώρακος. Μια υπηρεσία τηλεραδιολογίας αναθέτει τις μελέτες σε αναγνώστες. Ένα εργαστήριο 3D λαμβάνει εντολές μετα‑επεξεργασίας. Κάθε ένα από αυτά αποτελεί μια ουρά εργασιών μεταξύ συστημάτων, και το DICOM ορίζει μία για αυτό: το Unified Procedure Step (UPS), που προσφέρεται μέσω HTTP από την υπηρεσία UPS‑RS του DICOMweb.</p>

<p>Ένα αντικείμενο εργασίας UPS καθορίζει τι πρέπει να γίνει, για ποιον ασθενή και με ποια είσοδο. Είναι SCHEDULED όταν δημιουργείται. Ένας εργαζόμενος το αναλαμβάνει και το θέτει σε IN PROGRESS, στη συνέχεια το ολοκληρώνει ως COMPLETED, ή λήγει ως CANCELED. Άλλα συστήματα εγγράφονται και λαμβάνουν κάθε αλλαγή κατάστασης μέσω WebSocket, ώστε κανείς να μην χρειάζεται να κάνει polling.</p>

<p><strong>Aspose.Medical for .NET</strong> υλοποιεί και τις δύο πλευρές σε διαχειριζόμενη C#: έναν τυποποιημένο πελάτη για οποιονδήποτε διακομιστή UPS‑RS, και τη πλευρά του διακομιστή για το ASP.NET Core, όπου η βιβλιοθήκη ελέγχει τους κανόνες UPS και ο κώδικάς σας αποθηκεύει τα αντικείμενα εργασίας.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Προγραμματισμός αντικειμένου εργασίας">}}

<p>Το σύστημα που θέλει να εκτελεστεί η εργασία δημιουργεί το αντικείμενο εργασίας. Το SOP Instance UID του το ταυτοποιεί από εκεί και πέρα· η ετικέτα, η ετικέτα λίστας εργασίας και η προτεραιότητα ενημερώνουν τους εργαζόμενους για το τι είναι.</p>

<div class="codeblock" id="code">
 <h3>Δημιουργία αντικειμένου εργασίας UPS - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// What to do, for which patient, and with which input
Dataset input = new();
input.AddOrUpdate(Tag.StudyInstanceUID, "1.2.826.0.1.3680043.10.1.100");

Dataset workitem = new();
workitem.AddOrUpdate(Tag.SOPInstanceUID, Uid.CreateDerivedFromUuid());
workitem.AddOrUpdate(Tag.ProcedureStepState, "SCHEDULED");
workitem.AddOrUpdate(Tag.TransactionUID, string.Empty);
workitem.AddOrUpdate(Tag.InputReadinessState, "READY");
workitem.AddOrUpdate(Tag.ProcedureStepLabel, "Lung nodule detection");
workitem.AddOrUpdate(Tag.WorklistLabel, "AI");
workitem.AddOrUpdate(Tag.ScheduledProcedureStepPriority, "HIGH");
workitem.AddOrUpdate(Tag.PatientName, "DOE^JANE");
workitem.AddOrUpdate(Tag.PatientID, "P001");
workitem.AddOrUpdate(new Sequence(Tag.InputInformationSequence, [input]));

DicomWebClientResult&lt;CreateWorkitemResult&gt; created = await client.Worklist.CreateAsync(workitem);
Console.WriteLine($"Scheduled {created.Value.WorkitemUid}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αναλάβετε το, εκτελέστε την εργασία, ολοκληρώστε το">}}

<p>Ένας εργαζόμενος ψάχνει στη λίστα εργασιών για ό,τι μπορεί να κάνει και αναλαμβάνει ένα αντικείμενο εργασίας μετακινώντας το σε IN PROGRESS με ένα UID συναλλαγής που έχει δημιουργήσει. Από αυτή τη στιγμή μόνο ο κάτοχος αυτού του UID μπορεί να ενημερώσει ή να ολοκληρώσει το αντικείμενο εργασίας, έτσι δύο εργαζόμενοι ποτέ δεν επεξεργάζονται την ίδια εργασία.</p>

<div class="codeblock" id="code">
 <h3>Εύρεση, ανάληψη και ολοκλήρωση προγραμματισμένων αντικειμένων εργασίας - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// Scheduled workitems on the AI worklist
DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters =
    [
        new DicomWebQueryParameter("ProcedureStepState", "SCHEDULED"),
        new DicomWebQueryParameter("WorklistLabel", "AI")
    ]
};

List&lt;string&gt; scheduled = [];
await using (DicomWebClientStream&lt;Dataset&gt; found = await client.Worklist.SearchAsync(search))
{
    await foreach (Dataset item in found)
        scheduled.Add(item.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID));
}

foreach (string workitemUid in scheduled)
{
    // Claim the workitem; only the holder of this transaction UID can update or finish it
    string transactionUid = Uid.CreateDerivedFromUuid().Code;
    await client.Worklist.ChangeStateAsync(workitemUid, WorkitemState.InProgress, transactionUid);

    // ... run the job ...

    await client.Worklist.UpdateAsync(workitemUid, transactionUid, Performed());
    await client.Worklist.ChangeStateAsync(workitemUid, WorkitemState.Completed, transactionUid);
}</code></pre>
</div>

<p>Το COMPLETED απαιτεί μια καταγραφή της εργασίας: ποιος την εκτέλεσε, τι εκτελέστηκε, η ώρα έναρξης και λήξης, και το αποτέλεσμα. Ο εργαζόμενος το αποθηκεύει με μια ενημέρωση πριν αλλάξει η κατάσταση.</p>

<div class="codeblock" id="code">
 <h3>Τα δεδομένα της εκτελεσθείσας διαδικασίας - C#</h3>
 <pre><code class="cs">// What COMPLETED requires: who did the work, what was done, when, and the output (here none)
public static Dataset Performed()
{
    Dataset performed = new();
    performed.AddOrUpdate(new Sequence(Tag.PerformedStationNameCodeSequence, [Code("AI-NODE-1", "99LOCAL", "AI node 1")]));
    performed.AddOrUpdate(new Sequence(Tag.PerformedWorkitemCodeSequence, [Code("NODULE", "99LOCAL", "Lung nodule detection")]));
    performed.AddOrUpdate(Tag.PerformedProcedureStepStartDateTime, DateTimeOffset.Now.AddMinutes(-2).ToString("yyyyMMddHHmmss"));
    performed.AddOrUpdate(Tag.PerformedProcedureStepEndDateTime, DateTimeOffset.Now.ToString("yyyyMMddHHmmss"));
    performed.AddOrUpdate(new Sequence(Tag.OutputInformationSequence, []));

    Dataset update = new();
    update.AddOrUpdate(new Sequence(Tag.UnifiedProcedureStepPerformedProcedureSequence, [performed]));
    return update;
}

private static Dataset Code(string value, string scheme, string meaning)
{
    Dataset code = new();
    code.AddOrUpdate(Tag.CodeValue, value);
    code.AddOrUpdate(Tag.CodingSchemeDesignator, scheme);
    code.AddOrUpdate(Tag.CodeMeaning, meaning);
    return code;
}</code></pre>
</div>

<p>Ένας διακομιστής που κτίστηκε με το Aspose.Medical επιβάλλει αυτούς τους κανόνες. Ένας δεύτερος εργαζόμενος που προσπαθεί να αναλάβει ένα αντικείμενο εργασίας που βρίσκεται σε πρόοδο λαμβάνει HTTP 409, όπως και μια αλλαγή κατάστασης σε ολοκληρωμένο αντικείμενο. Ένα αίτημα COMPLETED χωρίς τα δεδομένα της εκτελεσθείσας διαδικασίας λαμβάνει HTTP 400. Ο πελάτης αναφέρει τέτοιες απαντήσεις ως <code>DicomWebClientException</code> με την HTTP απόκριση.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Παρακολουθήστε τη λίστα εργασιών μέσω WebSocket">}}

<p>Ένας πίνακας ελέγχου, το σύστημα που διέταξε την εργασία ή ένας εργαζόμενος που περιμένει νέες εργασίες εγγράφονται με τον τίτλο AE τους και ανοίγουν μια σύνδεση ειδοποιήσεων. Κάθε αναφορά συμβάντος φτάνει ως σύνολο δεδομένων με το UID του αντικειμένου εργασίας και τη νέα του κατάσταση.</p>

<div class="codeblock" id="code">
 <h3>Λήψη αναφορών συμβάντων UPS - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// Every change on the worklist, pushed over a WebSocket
await client.Worklist.SubscribeWorklistAsync("DASHBOARD");
await using DicomWebNotificationConnection connection = await client.ConnectNotificationsAsync("DASHBOARD");

while (await connection.ReceiveAsync(cancellationToken) is Dataset report)
{
    Console.WriteLine($"{report.GetSingleValueOrDefault(Tag.AffectedSOPInstanceUID, string.Empty)}: " +
                      $"{report.GetSingleValueOrDefault(Tag.ProcedureStepState, string.Empty)}");
}</code></pre>
</div>

<p>Εκτός από ολόκληρη τη λίστα εργασιών, ένας πελάτης μπορεί να εγγραφεί σε ένα αντικείμενο εργασίας με <code>SubscribeWorkitemAsync</code> ή σε ένα φιλτραρισμένο μέρος της λίστας εργασιών με <code>SubscribeFilteredWorklistAsync</code>, εάν ο διακομιστής υποστηρίζει φίλτρα. Η ακύρωση μιας λήψης κλείνει τη σύνδεση· ανοίξτε μια νέα με <code>ConnectNotificationsAsync</code> για να συνεχίσετε.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ο δικός σας διακομιστής UPS‑RS">}}

<p>Το πακέτο <code>Aspose.Medical.Dicom.Web.AspNetCore</code> προσθέτει UPS‑RS σε μια υπηρεσία ASP.NET Core. Η βιβλιοθήκη διαχειρίζεται κάθε αίτημα: ελέγχει το payload ενός νέου αντικειμένου εργασίας, τις μεταβάσεις κατάστασης, το UID συναλλαγής του εργαζομένου και τα δεδομένα που απαιτούνται για COMPLETED, και εφαρμόζει τους κανόνες αντιστοίχισης DICOM στις αναζητήσεις. Ο κώδικάς σας αποθηκεύει τα αντικείμενα εργασίας.</p>

<div class="codeblock" id="code">
 <h3>Program.cs ενός διακομιστή UPS‑RS - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Η αποθήκευση αντικειμένων εργασίας - C#</h3>
 <pre><code class="cs">// Workitems with a revision each; the library applies the UPS state rules and the search matching
public sealed class WorkitemStore : IDicomWebWorkitemProvider
{
    private readonly ConcurrentDictionary&lt;string, DicomWebVersionedDataset&gt; _items = new();

    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string uid in _items.Keys.Order(StringComparer.Ordinal))
            if (_items.TryGetValue(uid, out DicomWebVersionedDataset? item))
                yield return new Dataset(item.Dataset);
        await Task.CompletedTask;
    }

    public ValueTask&lt;DicomWebVersionedDataset?&gt; GetAsync(string workitemUid, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryGetValue(workitemUid, out DicomWebVersionedDataset? item) ? Copy(item) : null);

    public ValueTask&lt;bool&gt; TryCreateAsync(string workitemUid, Dataset workitem, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryAdd(workitemUid, NewRevision(workitem)));

    // Replaces the workitem only if nobody changed it since it was read
    public ValueTask&lt;bool&gt; TryReplaceAsync(string workitemUid, string expectedRevision, Dataset workitem,
        CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryGetValue(workitemUid, out DicomWebVersionedDataset? current)
                             &amp;&amp; current.Revision == expectedRevision
                             &amp;&amp; _items.TryUpdate(workitemUid, NewRevision(workitem), current));

    private static DicomWebVersionedDataset NewRevision(Dataset dataset) =&gt;
        new() { Dataset = new Dataset(dataset), Revision = Guid.NewGuid().ToString("N") };

    private static DicomWebVersionedDataset Copy(DicomWebVersionedDataset item) =&gt;
        new() { Dataset = new Dataset(item.Dataset), Revision = item.Revision };
}</code></pre>
</div>

<p>Κάθε αποθηκευμένο αντικείμενο εργασίας έχει μια αναθεώρηση. Το <code>TryReplaceAsync</code> το αντικαθιστά μόνο όταν η αναθεώρηση είναι ακόμη αυτή που διάβασε η βιβλιοθήκη, ώστε δύο αιτήματα που αλλάζουν το ίδιο αντικείμενο εργασίας ταυτόχρονα να μην μπορούν να αντικαταστήσουν το ένα το άλλο· η βιβλιοθήκη κάνει επαναπροσπάθεια ή αναφέρει σύγκρουση. Αντικαταστήστε το λεξικό με τη βάση δεδομένων σας και διατηρήστε αυτόν τον έλεγχο.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ειδοποιήσεις στον διακομιστή">}}

<p>Για την αποστολή αναφορών συμβάντων, καταχωρίστε ένα <code>IDicomWebSubscriptionProvider</code> και καλέστε <code>app.UseWebSockets()</code> πριν από το <code>MapDicomWeb</code>. Ο πάροχος διατηρεί τις εγγραφές και μία ουρά αναφορών συμβάντων ανά συνδρομητή, και η βιβλιοθήκη του παρέχει μια αναφορά συμβάντος όποτε αλλάζει ένα αντικείμενο εργασίας. Ο πάροχος επίσης ενημερώνει τη βιβλιοθήκη για το ποιον τίτλο AE διαθέτει ο αυθεντικοποιημένος κάλεσας, επειδή οι αναφορές συμβάντων ονομάζουν το σύστημα που άλλαξε την κατάσταση.</p>

<p>Ο <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">οδηγός παρόχων ροής εργασίας</a> περιέχει έναν πλήρη παροχέα συνδρομής στη μνήμη, και ο <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">οδηγός ASP.NET Core</a> καλύπτει τις ρυθμίσεις WebSocket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL και MPPS">}}

<p>Το UPS καλύπτει ό,τι κάνουν οι παλαιότερες Modality Worklist και MPPS μεταξύ ενός RIS και μιας συσκευής, και προσθέτει εργαζόμενους που δεν είναι συσκευές, με ανάληψη και ειδοποιήσεις. Πολλές συσκευές εξακολουθούν να χρησιμοποιούν μόνο MWL και MPPS μέσω DIMSE· η <a href="/medical/net/modality-worklist/">σελίδα λίστας εργασιών συσκευής</a> δείχνει αυτή τη ροή εργασίας με την ίδια βιβλιοθήκη.</p>

<p>Σχετικές σελίδες: <a href="/medical/net/dicomweb-client/">πρόγραμμα-πελάτης DICOMweb</a> για τις άλλες υπηρεσίες DICOMweb, <a href="/medical/net/dicomweb-server/">διακομιστής DICOMweb</a> για QIDO‑RS, WADO‑RS και STOW‑RS στον ίδιο διακομιστή, και <a href="/medical/net/dicom-networking/">δικτύωση DICOM</a> για DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι εκμάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Οδηγός προγραμματιστών" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="Αναφορές API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
