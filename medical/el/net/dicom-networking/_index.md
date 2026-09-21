---
title: Δικτύωση DICOM σε C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Συνδέστε την εφαρμογή .NET σας με ένα PACS. Επαληθεύστε με C-ECHO, στείλτε εικόνες με C-STORE, κάντε ερώτημα με C-FIND και λάβετε εικόνες με το δικό σας SCP. Ένας πελάτης και διακομιστής DIMSE σε καθαρό C#.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Δικτύωση DICOM σε .NET C#" h2="Επικοινωνήστε με ένα PACS από τη δική σας εφαρμογή: C-ECHO, C-STORE, C-FIND, C-MOVE και C-GET, ως πελάτης και ως διακομιστής, σε διαχειριζόμενο C# χωρίς να χρειάζεται εγκατάσταση σε μηχάνημα." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Συνδεθείτε με ένα PACS από τον δικό σας κώδικα">}}

<p>Η ανάγνωση αρχείων DICOM είναι το εύκολο ήμισυ της ιατρικής απεικόνισης. Μόλις η εφαρμογή σας πρέπει να συνεργαστεί με ένα πραγματικό σύστημα νοσοκομείου, πρέπει να μιλήσει DIMSE: να ανοίξει μια σύνδεση με ένα PACS, να στείλει εικόνες, να ρωτήσει ποιές μελέτες υπάρχουν και να απαντήσει όταν ένα άλλο σύστημα στείλει κάτι πίσω.</p>

<p><strong>Aspose.Medical for .NET</strong> προσφέρει αυτό το πρωτόκολλο ως μέρος της βιβλιοθήκης. <code>Aspose.Medical.Dicom.Network</code> σας παρέχει έναν πελάτη DIMSE και έναν διακομιστή DIMSE, και οι δύο γραμμένοι σε διαχειριζόμενο C#. Δεν υπάρχει εγγενές εργαλείο για εγκατάσταση, δεν χρειάζεται ρύθμιση υπηρεσίας και δεν υπάρχει κάτι εξειδικευμένο για πλατφόρμα, έτσι ο ίδιος κώδικας εκτελείται σε Windows, σε Linux και σε κοντέινερ.</p>

<p>Τρία στοιχεία καλύπτουν τις περισσότερες ενσωματώσεις, και το καθένα είναι μερικές γραμμές: ελέγξτε τη σύνδεση με C-ECHO, προωθήστε εικόνες με C-STORE και βρείτε τι υπάρχει στην άλλη πλευρά με C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ξεκινήστε με C-ECHO">}}

<p>C-ECHO είναι το ping του DICOM. Δείχνει ότι ο κεντρικός υπολογιστής, η θύρα και οι δύο τίτλοι AE είναι σωστοί πριν κατηγορηθεί οτιδήποτε άλλο. Δημιουργήστε έναν πελάτη μία φορά, δώστε του έναν χειριστή που παρακολουθεί την απάντηση, και στείλτε το αίτημα.</p>

<div class="codeblock" id="code">
 <h3>Επαλήθευση της σύνδεσης με C-ECHO - C#</h3>
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

<p>Η κατάσταση <code>0x0000</code> σημαίνει επιτυχία. Τα αιτήματα τοποθετούνται σε ουρά και στη συνέχεια αποστέλλονται σε μια σύνδεση, ώστε μια παρτίδα εργασίας να μην ανοίγει σύνδεση ανά αντικείμενο.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αποστολή εικόνων με C-STORE">}}

<p>C-STORE είναι αυτό που κάνει μια εφαρμογή αφού έχει παραγάγει ή λάβει μια εικόνα: σπρώχνει το instance στο αρχείο. Τοποθετήστε ένα αίτημα ανά instance και στείλτε τα μαζί.</p>

<div class="codeblock" id="code">
 <h3>Αποστολή αρχείου DICOM σε PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Τα presentation contexts που προτείνετε καθορίζουν τι θα αποδεχτεί το αρχείο. Αν απαιτεί μια συμπιεσμένη σύνταξη, κάντε μετατροπή πριν την αποστολή, όπως δείχνει η σελίδα <a href="/medical/net/dicom-transfer-syntax-conversion/">μετατροπής συντακτικού μεταφοράς</a>, ή απαριθμίστε τις εναλλακτικές στο <code>AdditionalTransferSyntaxes</code> και αφήστε τη διαπραγμάτευση να επιλέξει μία.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Εύρεση μελετών με C-FIND">}}

<p>C-FIND απαντά στην ερώτηση «τι έχει το αρχείο». Τα αποτελέσματα φθάνουν ένα-ένα, το καθένα με το δικό του σύνολο δεδομένων ταυτοποίησης, και μια τελική απάντηση κλείνει το ερώτημα.</p>

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

<p>Η ίδια εργοστασιακή μονάδα δημιουργεί τα άλλα επίπεδα ερωτήματος: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> και <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> δημιουργεί ένα ερώτημα λίστας εργασιών συσκευής, το οποίο μια συσκευή ζητά πριν από μια σάρωση.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Λήψη εικόνων: το δικό σας store SCP">}}

<p>Η βιβλιοθήκη λειτουργεί επίσης ως διακομιστής. Καταχωρίστε έναν χειριστή για την υπηρεσία που θέλετε να προσφέρετε, ξεκινήστε την ακρόαση και η εφαρμογή σας γίνεται κόμβος DICOM που μια συσκευή ή άλλο PACS μπορεί να στείλει.</p>

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

<p>Το τι κάνει ο χειριστής με το σύνολο δεδομένων είναι απόφαση σας: γράψτε το στο δίσκο, τοποθετήστε το σε ουρά, ανωνυμοποιήστε το πρώτα με το <a href="/medical/net/anonymization/">API ανωνυμοποίησης</a>, ή μετατρέψτε το στη σύνταξη του αρχείου.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Τι άλλο καλύπτει το API δικτύωσης">}}

<p>Οι τρεις παραπάνω υπηρεσίες είναι οι κοινές. Το υπόλοιπο του DIMSE είναι επίσης διαθέσιμο:</p>

<ul>
<li>Ανάκτηση: C-MOVE και C-GET, με τους μετρητές υπο-λειτουργιών να αναφέρονται στις απαντήσεις.</li>
<li>Οι N-υπηρεσίες: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE και N-EVENT-REPORT, από τις οποίες χτίζονται η δέσμευση αποθήκευσης και το MPPS.</li>
<li>Έλεγχος σύνδεσης: presentation contexts, ρόλοι κλάσεων υπηρεσίας, εκτεταμένη διαπραγμάτευση, παράθυρο ασύγχρονων λειτουργιών, διαπραγμάτευση ταυτότητας χρήστη, και μια άγκυρα πολιτικής που μπορεί να απορρίψει μια σύνδεση καλώντας τον τίτλο AE.</li>
<li>TLS και στα δύο μέρη, μέσω <code>TlsInitiatorAuthenticator</code> και <code>TlsAcceptorAuthenticator</code>, με δική σας επικύρωση πιστοποιητικού αν το χρειάζεστε.</li>
<li>Χρονικά όρια για κάθε στάδιο, από τη σύνδεση TCP μέχρι την απελευθέρωση, και ειδοποιήσεις για τον κύκλο ζωής της σύνδεσης ώστε ένας μακράς διάρκειας κόμβος να μπορεί να καταγράψει τι συνέβη.</li>
</ul>

<p>Ο <a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">οδηγός δικτύωσης DICOM</a> τεκμηριώνει κάθε επιλογή και κάθε χειριστή.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Καθαρό .NET, από το socket μέχρι τα δεδομένα pixel">}}

<p>Ό,τι υπάρχει σε αυτή τη σελίδα είναι διαχειριζόμενος κώδικας από το ίδιο πακέτο που διαβάει και γράφει τα αρχεία. Η σύνδεση, οι κωδικοποιητές και ο αναλυτής προέρχονται από μία βιβλιοθήκη, έτσι μια μελέτη που λαμβάνεται μέσω δικτύου μπορεί να ανωνυμοποιηθεί, μετατραπεί ή σειριοποιηθεί χωρίς να βγει από τη διαδικασία και χωρίς εγγενή εξάρτηση οπουδήποτε στην αλυσίδα.</p>

<p>Σχετικές σελίδες: <a href="/medical/net/dicom-transfer-syntax-conversion/">μετατροπή συντακτικού μεταφοράς</a> για το τι να στείλετε, <a href="/medical/net/anonymization/">ανωνυμοποίηση</a> για το τι να αφαιρέσετε πρώτα, και <a href="/medical/net/dicom-tags/">ετικέτες DICOM</a> για την ανάγνωση του τι έφτασε.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι εκμάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Οδηγός Προγραμματιστή" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="Αναφορές API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Υποστήριξη Προϊόντος" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Δωρεάν Υποστήριξη" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Εξαμειβόμενη Υποστήριξη" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Ιστολόγιο" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Γιατί Aspose.Medical για .NET;" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Λίστα Πελατών" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Ιστορίες Επιτυχίας" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
