---
title: Δικτύωση DICOM σε C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Συνδέστε την εφαρμογή .NET με ένα PACS. Επαληθεύστε με C-ECHO, στείλτε εικόνες με C-STORE, κάντε ερώτημα με C-FIND και λάβετε εικόνες με το δικό σας SCP. Ένα πρόγραμμα-πελάτης και διακομιστής DIMSE σε καθαρό C#.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Δικτύωση DICOM σε .NET C#" h2="Επικοινωνήστε με ένα PACS από τη δική σας εφαρμογή: C-ECHO, C-STORE, C-FIND, C-MOVE και C-GET, ως πελάτης και ως διακομιστής, σε διαχειριζόμενο C# χωρίς ανάγκη εγκατάστασης στο σύστημα." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Συνδεθείτε με ένα PACS από τον κώδικά σας">}}

<p>Η ανάγνωση αρχείων DICOM είναι το εύκολο μισό της ιατρικής απεικόνισης. Μόλις η εφαρμογή σας χρειάζεται να λειτουργήσει με ένα πραγματικό σύστημα νοσοκομείου, πρέπει να μιλάει DIMSE: ανοίγει μια σύνδεση με ένα PACS, στέλνει εικόνες, ρωτά ποιες μελέτες υπάρχουν, και απαντά όταν ένα άλλο σύστημα στέλνει κάτι πίσω.</p>

<p><strong>Aspose.Medical for .NET</strong> προσφέρει αυτό το πρωτόκολλο ως μέρος της βιβλιοθήκης. <code>Aspose.Medical.Dicom.Network</code> σας παρέχει έναν πελάτη DIMSE και έναν διακομιστή DIMSE, και οι δύο γραμμένοι σε διαχειριζόμενο C#. Δεν υπάρχει εγγενές εργαλειοθήκη για εγκατάσταση, καμία υπηρεσία για ρύθμιση και τίποτα εξαρτημένο από την πλατφόρμα, έτσι ο ίδιος κώδικας εκτελείται σε Windows, σε Linux και σε container.</p>

<p>Τρία πράγματα καλύπτουν τις περισσότερες ενσωματώσεις, και το καθένα απαιτεί λίγες γραμμές κώδικα: ελέγξτε τη σύνδεση με C-ECHO, σπρώξτε εικόνες με C-STORE, και βρείτε τι υπάρχει στην άλλη πλευρά με C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ξεκινήστε με C-ECHO">}}

<p>C-ECHO είναι το ping του DICOM. Επιβεβαιώνει ότι ο κεντρικός υπολογιστής, η θύρα και οι δύο AE τίτλοι είναι σωστοί πριν κατηγορηθεί οτιδήποτε άλλο. Δημιουργήστε έναν πελάτη μία φορά, δώστε του έναν χειριστή που παρακολουθεί την απάντηση, και στείλτε το αίτημα.</p>

<div class="codeblock" id="code">
 <h3>Επαληθεύστε τη σύνδεση με C-ECHO - C#</h3>
 <pre><code class="cs">AssociationNegotiationOptions negotiation = new AssociationNegotiationOptions()
    .WithPresentationContext(new PresentationContext
    {
        AbstractSyntax = Uid.Verification,
        Role = null,
        TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
    });

DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = negotiation
    })
    .AddCEchoHandler((request, response, cancellationToken) =>
    {
        Console.WriteLine($"C-ECHO answered with status 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(new CEchoRequest());

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Η κατάσταση <code>0x0000</code> σημαίνει επιτυχία. Τα αιτήματα τοποθετούνται σε ουρά και στη συνέχεια αποστέλλονται σε μία σύνδεση, ώστε μια δέσμη εργασίας να μην ανοίγει σύνδεση ανά αντικείμενο.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Στείλτε εικόνες με C-STORE">}}

<p>C-STORE είναι αυτό που κάνει μια εφαρμογή αφού έχει παράγει ή λάβει μια εικόνα: σπρώχνει το στιγμιότυπο στο αρχείο. Τοποθετήστε ένα αίτημα ανά στιγμιότυπο στην ουρά και στείλτε τα μαζί.</p>

<div class="codeblock" id="code">
 <h3>Στείλτε ένα αρχείο DICOM σε ένα PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Τα presentation contexts που προτείνετε αποφασίζουν τι θα αποδεχτεί το αρχείο. Αν θέλει συμπιεσμένη σύνταξη, κάντε μετακωδικοποίηση πριν την αποστολή, όπως δείχνει η σελίδα <a href="/medical/net/dicom-transfer-syntax-conversion/">μετατροπής σύνταξης μεταφοράς</a>, ή παραθέστε τις εναλλακτικές στο <code>AdditionalTransferSyntaxes</code> και αφήστε τη διαπραγμάτευση να διαλέξει μία.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Βρείτε μελέτες με C-FIND">}}

<p>C-FIND απαντά στην ερώτηση "τι έχει το αρχείο". Τα αποτελέσματα έρχονται ένα προς ένα, το καθένα με το δικό του σύνολο δεδομένων αναγνώρισης, και μια τελική απάντηση κλείνει το ερώτημα.</p>

<div class="codeblock" id="code">
 <h3>Ερώτημα μελετών ανά ασθενή - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.StudyRootQueryRetrieveInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddCFindHandler((request, response, cancellationToken) =>
    {
        // A match arrives with an identifier; the final response carries the status only
        if (response.Identifier is not null)
            Console.WriteLine(response.Identifier.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty));

        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(CFindRequest.CreateStudyQuery(
    patientId: "PATIENT-001",
    patientName: null,
    studyDateTime: null,
    accession: null,
    studyId: null,
    modalitiesInStudy: null,
    studyInstanceUid: null,
    priority: DimsePriority.Medium));

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Η ίδια factory δημιουργεί τα άλλα επίπεδα ερωτημάτων: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> και <code>CreateImageQuery</code>. Το <code>CreateWorklistQuery</code> δημιουργεί ένα ερώτημα λίστας εργασιών μοντέλου, που είναι το ερώτημα που το μοντέλο κάνει πριν από τη σάρωση· η <a href="/medical/net/modality-worklist/">σελίδα λίστας εργασιών μοντέλου</a> το παρουσιάζει μαζί με το MPPS και την πλευρά του διακομιστή λίστας εργασιών.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Λήψη εικόνων: το δικό σας store SCP">}}

<p>Η βιβλιοθήκη λειτουργεί επίσης ως διακομιστής. Καταχωρίστε έναν χειριστή για την υπηρεσία που θέλετε να προσφέρετε, ξεκινήστε την αναμονή, και η εφαρμογή σας γίνεται κόμβος DICOM στον οποίο μια μονάδα ή ένα άλλο PACS μπορεί να στείλει.</p>

<div class="codeblock" id="code">
 <h3>Αποδοχή εισερχόμενων εικόνων - C#</h3>
 <pre><code class="cs">DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Any, 11112)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.SecondaryCaptureImageStorage,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddSingletonCStoreHandler(new StoreHandler())
    .Build();

await server.StartAsync(CancellationToken.None);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Ο χειριστής που αποθηκεύει ό,τι φθάνει - C#</h3>
 <pre><code class="cs">public sealed class StoreHandler : ICStoreRequestHandler
{
    public ValueTask&lt;CStoreResponse&gt; Handle(CStoreRequest request, CancellationToken cancellationToken)
    {
        // Write what arrived, then answer Success
        new DicomFile(request.Dataset).Save($"{request.AffectedSopInstanceUid}.dcm");

        CStoreResponse response = new();
        response.Command.AddOrUpdate(Tag.Status, (ushort)0x0000);
        return ValueTask.FromResult(response);
    }
}</code></pre>
</div>

<p>Το τι κάνει ο χειριστής με το σύνολο δεδομένων είναι δική σας απόφαση: να το γράψετε σε δίσκο, να το βάλτε σε ουρά, να το ανωνυμοποιήσετε πρώτα με το <a href="/medical/net/anonymization/">API ανωνυμοποίησης</a>, ή να το μετακωδικοποιήσετε στη σύνταξη του αρχείου.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Τι άλλο καλύπτει το API δικτύωσης">}}

<p>Οι τρεις παραπάνω υπηρεσίες είναι οι κοινές. Το υπόλοιπο του DIMSE υπάρχει επίσης:</p>

<ul>
<li>Ανάκτηση: C-MOVE και C-GET, με τους μετρητές υπο-λειτουργιών που αναφέρονται στις απαντήσεις.</li>
<li>Οι N-υπηρεσίες: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE και N-EVENT-REPORT, που είναι βάση για τη δέσμευση αποθήκευσης και το MPPS.</li>
<li>Έλεγχος συσχέτισης (association): presentation contexts, ρόλοι κλάσεων υπηρεσιών, εκτεταμένη διαπραγμάτευση, το παράθυρο ασύγχρονων λειτουργιών, διαπραγμάτευση ταυτότητας χρήστη, και μια γάντζο πολιτικής που μπορεί να απορρίψει μια συσχέτιση καλώντας τον AE τίτλο.</li>
<li>TLS και στα δύο άκρα, μέσω <code>TlsInitiatorAuthenticator</code> και <code>TlsAcceptorAuthenticator</code>, με τη δική σας επαλήθευση πιστοποιητικού αν το χρειάζεστε.</li>
<li>Χρόνοι λήξης (timeouts) για κάθε στάδιο, από τη σύνδεση TCP μέχρι την απελευθέρωση, και ειδοποιήσεις για τον κύκλο ζωής της συσχέτισης ώστε ένας μακροχρόνιος κόμβος να καταγράφει τι συνέβη.</li>
</ul>

<p>Ο <a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">οδηγός δικτύωσης DICOM</a> τεκμηριώνει κάθε επιλογή και κάθε χειριστή.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Καθαρό .NET, από το socket μέχρι τα δεδομένα pixel">}}

<p>Όλα σε αυτή τη σελίδα είναι διαχειριζόμενος κώδικας από το ίδιο πακέτο που διαβάζει και γράφει τα αρχεία. Η σύνδεση, οι κωδικοποιητές (codecs) και ο αναλυτής (parser) προέρχονται από μία βιβλιοθήκη, έτσι μια μελέτη που λαμβάνεται μέσω δικτύου μπορεί να ανωνυμοποιηθεί, μετακωδικοποιηθεί ή σειριοποιηθεί χωρίς να εξέλθει από τη διαδικασία και χωρίς εγγενή εξάρτηση οπουδήποτε στην αλυσίδα.</p>

<p>Σχετικές σελίδες: <a href="/medical/net/dicom-transfer-syntax-conversion/">μετατροπή σύνταξης μεταφοράς</a> για το τι να στέλνετε, <a href="/medical/net/anonymization/">ανωνυμοποίηση</a> για το τι να αφαιρείτε πρώτα, και <a href="/medical/net/dicom-tags/">ετικέτες DICOM</a> για την ανάγνωση του τι έφτασε.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πηγές μάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Οδηγός προγραμματιστή" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
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
