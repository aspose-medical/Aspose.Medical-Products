---
title: Modality Worklist και MPPS σε C# .NET - DICOM C-FIND, N-CREATE, N-SET | Aspose.Medical
weight: 9300

description: Εκτελέστε ερώτημα στη DICOM Modality Worklist με C-FIND και αναφέρετε MPPS από C#, ή τρέξτε τον δικό σας διακομιστή λίστας εργασιών. DIMSE και DICOMweb σε μία βιβλιοθήκη .NET.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Modality Worklist και MPPS για .NET C#" h2="Αποκτήστε τις εξετάσεις της ημέρας από το RIS, αναφέρετε καθεμία ως ξεκίνησε και ολοκληρώθηκε, ή γίνετε ο δικός σας διακομιστής λίστας εργασιών, μέσω DIMSE ή DICOMweb." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Από το πρόγραμμα του RIS στην εκτελεσθείσα εξέταση">}}

<p>Πριν από μια σάρωση, ο τεχνικός δεν πρέπει να πληκτρολογήσει το όνομα του ασθενούς, το αναγνωριστικό του ασθενούς και τον αριθμό πρόσβασης στη συσκευή. Το RIS έχει ήδη προγραμματίσει την εξέταση, και η συσκευή το ζητά: ένα ερώτημα Modality Worklist επιστρέφει τα προγραμματισμένα βήματα διαδικασίας για αυτό το σαρωτή και αυτή την ημέρα. Η συσκευή αντιγράφει τα δεδομένα του ασθενούς και της αίτησης στις εικόνες, ώστε η μελέτη να φτάνει στο PACS ήδη συσχετισμένη με την εντολή της.</p>

<p>Καθώς η εξέταση πραγματοποιείται, η συσκευή τη αναφέρει πίσω με MPPS (Modality Performed Procedure Step): IN PROGRESS όταν ξεκινά η σάρωση, COMPLETED ή DISCONTINUED όταν ολοκληρωθεί, μαζί με τις σειρές που παρήγαγε. Το RIS χρησιμοποιεί αυτές τις αναφορές για να ενημερώσει το πρόγραμμα, για να ενημερώσει τον ακτινολόγο ότι οι εικόνες έρχονται, και για χρέωση.</p>

<p><strong>Aspose.Medical for .NET</strong> καλύπτει και τις δύο πλευρές αυτής της ροής εργασίας σε διαχειριζόμενη C#: τη συσκευή που εκτελεί ερωτήματα και αναφορές, καθώς και τον διακομιστή λίστας εργασιών που απαντά. Και τα δύο λειτουργούν μέσω DIMSE, του κλασικού δικτυακού πρωτοκόλλου DICOM, και μέσω DICOMweb.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Εκτελέστε ερώτημα στη worklist με C-FIND">}}

<p>Ένα ερώτημα worklist είναι ένα αίτημα C-FIND στο μοντέλο πληροφορίας Modality Worklist. Η μέθοδος <code>CFindRequest.CreateWorklistQuery</code> το δημιουργεί με τα συνηθισμένα κλειδιά επιστροφής: δημογραφικά ασθενούς, αριθμός πρόσβασης, ζητούμενη διαδικασία και το προγραμματισμένο βήμα διαδικασίας. Τα φίλτρα που ορίζει η συσκευή είναι ο δικός της τίτλος AE, η μονάδα της και η ημερομηνία, όλα εντός του προγραμματισμένου βήματος διαδικασίας.</p>

<div class="codeblock" id="code">
 <h3>Λάβετε τις εξετάσεις της σημερινής ημέρας για έναν CT σαρωτή - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "RIS_AE",
        Calling = "CT01",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.20"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityWorklistInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddCFindHandler((request, response, cancellationToken) =&gt;
    {
        // Every scheduled exam arrives as one identifier; the final response has none
        if (response.Identifier is Dataset item)
        {
            Dataset step = item.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);
            Console.WriteLine($"{item.GetSingleValueOrDefault(Tag.AccessionNumber, string.Empty)} " +
                              $"{item.GetSingleValueOrDefault(Tag.PatientName, string.Empty)} " +
                              $"{step.GetSingleValueOrDefault(Tag.ScheduledProcedureStepDescription, string.Empty)}");
        }

        return ValueTask.CompletedTask;
    })
    .Build();

// CT exams for this scanner
CFindRequest query = CFindRequest.CreateWorklistQuery(
    patientId: null,
    patientName: null,
    stationAe: "CT01",
    stationName: null,
    modality: "CT",
    scheduledDateTime: null);

// Scheduled for today
query.Identifier.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence)
    .AddOrUpdate(Tag.ScheduledProcedureStepStartDate, DateTimeOffset.Now.ToString("yyyyMMdd"));

client.QueueRequest(query);

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Κάθε αντιστοιχία είναι ένα σύνολο δεδομένων (dataset) με τα χαρακτηριστικά του ασθενούς και της αίτησης στο ανώτερο επίπεδο και το προγραμματισμένο βήμα διαδικασίας σε μια ακολουθία. Για να ζητήσετε περισσότερα κλειδιά επιστροφής, προσθέστε τα στο <code>query.Identifier</code> με κενή τιμή πριν την αίτηση τοποθετηθεί στην ουρά.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αναφέρετε την εξέταση με MPPS">}}

<p>MPPS αποτελείται από δύο τυποποιημένες λειτουργίες DIMSE στην κλάση SOP Modality Performed Procedure Step: N-CREATE όταν η εξέταση ξεκινά και N-SET όταν ολοκληρώνεται. Η συσκευή επιλέγει το UID του εκτελεσθέντος βήματος και το χρησιμοποιεί και στις δύο. Τα χαρακτηριστικά N-CREATE δηλώνουν ποιος εξετάστηκε και ποιο προγραμματισμένο βήμα εκτελέστηκε· τα περισσότερα προέρχονται απευθείας από το στοιχείο worklist.</p>

<div class="codeblock" id="code">
 <h3>Τα χαρακτηριστικά N-CREATE από ένα στοιχείο worklist - C#</h3>
 <pre><code class="cs">// N-CREATE attributes of a performed procedure step, filled from the worklist item the modality works on
public static Dataset InProgress(Dataset item, string stationAe)
{
    Dataset step = item.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);

    Dataset scheduled = new();
    Copy(item, scheduled, Tag.StudyInstanceUID, Tag.AccessionNumber, Tag.RequestedProcedureID, Tag.RequestedProcedureDescription);
    Copy(step, scheduled, Tag.ScheduledProcedureStepID, Tag.ScheduledProcedureStepDescription);
    scheduled.AddOrUpdate(new Sequence(Tag.ReferencedStudySequence, []));
    scheduled.AddOrUpdate(new Sequence(Tag.ScheduledProtocolCodeSequence, []));

    Dataset mpps = new();
    mpps.AddOrUpdate(new Sequence(Tag.ScheduledStepAttributesSequence, [scheduled]));
    Copy(item, mpps, Tag.PatientName, Tag.PatientID, Tag.PatientBirthDate, Tag.PatientSex, Tag.StudyID);
    Copy(step, mpps, Tag.Modality);
    mpps.AddOrUpdate(new Sequence(Tag.ReferencedPatientSequence, []));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepID, step.GetSingleValueOrDefault(Tag.ScheduledProcedureStepID, string.Empty));
    mpps.AddOrUpdate(Tag.PerformedStationAETitle, stationAe);
    mpps.AddOrUpdate(Tag.PerformedStationName, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedLocation, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedProcedureStepStartDate, DateTimeOffset.Now.ToString("yyyyMMdd"));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepStartTime, DateTimeOffset.Now.ToString("HHmmss"));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepStatus, "IN PROGRESS");
    mpps.AddOrUpdate(Tag.PerformedProcedureStepDescription, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedProcedureTypeDescription, string.Empty);
    mpps.AddOrUpdate(new Sequence(Tag.ProcedureCodeSequence, []));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepEndDate, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedProcedureStepEndTime, string.Empty);
    mpps.AddOrUpdate(new Sequence(Tag.PerformedProtocolCodeSequence, []));
    mpps.AddOrUpdate(new Sequence(Tag.PerformedSeriesSequence, []));
    return mpps;
}

// Copies the attributes the source has; a missing one is sent empty
private static void Copy(Dataset source, Dataset target, params Tag[] tags)
{
    foreach (Tag tag in tags)
    {
        if (source.GetOrDefault(tag) is IElement element)
            target.AddOrUpdate(element);
        else
            target.AddOrUpdate(tag, string.Empty);
    }
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Τα χαρακτηριστικά N-SET που ολοκληρώνουν το βήμα - C#</h3>
 <pre><code class="cs">// N-SET attributes that close the step and list the series the modality produced
public static Dataset Completed(string seriesInstanceUid, string protocolName)
{
    Dataset series = new();
    series.AddOrUpdate(Tag.SeriesInstanceUID, seriesInstanceUid);
    series.AddOrUpdate(Tag.ProtocolName, protocolName);
    series.AddOrUpdate(Tag.SeriesDescription, string.Empty);
    series.AddOrUpdate(Tag.PerformingPhysicianName, string.Empty);
    series.AddOrUpdate(Tag.OperatorsName, string.Empty);
    series.AddOrUpdate(Tag.RetrieveAETitle, string.Empty);
    series.AddOrUpdate(new Sequence(Tag.ReferencedImageSequence, []));
    series.AddOrUpdate(new Sequence(Tag.ReferencedNonImageCompositeSOPInstanceSequence, []));

    Dataset completed = new();
    completed.AddOrUpdate(Tag.PerformedProcedureStepStatus, "COMPLETED");
    completed.AddOrUpdate(Tag.PerformedProcedureStepEndDate, DateTimeOffset.Now.ToString("yyyyMMdd"));
    completed.AddOrUpdate(Tag.PerformedProcedureStepEndTime, DateTimeOffset.Now.ToString("HHmmss"));
    completed.AddOrUpdate(new Sequence(Tag.PerformedSeriesSequence, [series]));
    return completed;
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Αποστέλλει IN PROGRESS, έπειτα COMPLETED - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "RIS_AE",
        Calling = "CT01",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.20"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityPerformedProcedureStepSOPClass,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddNCreateHandler((request, response, cancellationToken) =&gt;
    {
        Console.WriteLine($"N-CREATE: 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .AddNSetHandler((request, response, cancellationToken) =&gt;
    {
        Console.WriteLine($"N-SET: 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .Build();

// The modality chooses the UID of the performed step; N-SET refers to it later
Uid mppsUid = Uid.CreateDerivedFromUuid();

client.QueueRequest(new NCreateRequest(Uid.ModalityPerformedProcedureStepSOPClass, mppsUid, InProgress(item, "CT01")));
await client.SendAsync(CancellationToken.None);

// ... the scan runs ...

client.QueueRequest(new NSetRequest(Uid.ModalityPerformedProcedureStepSOPClass, mppsUid, Completed("1.2.826.0.1.3680043.10.1.1", "CHEST")));
await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Μια εξέταση που διακόπτεται πρόωρα λήγει ως DISCONTINUED αντί για COMPLETED, και το N-SET τότε περιλαμβάνει τον λόγο στη Performed Procedure Step Discontinuation Reason Code Sequence.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Η ίδια ροή εργασίας μέσω DICOMweb">}}

<p>Νεότερα RIS και πλατφόρμες cloud προσφέρουν τη worklist και το MPPS μέσω HTTP. Ο <a href="/medical/net/dicomweb-client/">πελάτης DICOMweb</a> του Aspose.Medical αναζητά τα προγραμματισμένα βήματα διαδικασίας και αναφέρει το εκτελεσθέν βήμα με τα ίδια σύνολα δεδομένων όπως παραπάνω. Ένα κλειδί εντός του προγραμματισμένου βήματος διαδικασίας γράφεται ως διαδρομή, όπως <code>ScheduledProcedureStepSequence.Modality</code>.</p>

<div class="codeblock" id="code">
 <h3>Εκτελέστε ερώτημα στη worklist και αναφέρετε MPPS μέσω DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://ris.example.org/dicomweb/")
});

// The same keys as the C-FIND query, as query parameters
DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters =
    [
        new DicomWebQueryParameter("ScheduledProcedureStepSequence.Modality", "CT"),
        new DicomWebQueryParameter("ScheduledProcedureStepSequence.ScheduledStationAETitle", "CT01"),
        new DicomWebQueryParameter("ScheduledProcedureStepSequence.ScheduledProcedureStepStartDate", DateTimeOffset.Now.ToString("yyyyMMdd"))
    ]
};

List&lt;Dataset&gt; worklist = [];
await using (DicomWebClientStream&lt;Dataset&gt; steps = await client.ModalityWorklist.SearchAsync(search))
{
    await foreach (Dataset step in steps)
        worklist.Add(step);
}

// The same MPPS attributes go to the procedure step resource
Dataset item = worklist[0];
string mppsUid = Uid.CreateDerivedFromUuid().Code;
await client.ModalityPerformedProcedureSteps.CreateAsync(mppsUid, InProgress(item, "CT01"));
await client.ModalityPerformedProcedureSteps.UpdateAsync(mppsUid, Completed("1.2.826.0.1.3680043.10.1.2", "CHEST"));</code></pre>
</div>

<p>Ο πελάτης εκτελείται στο δικό σας <code>HttpClient</code>, έτσι ο έλεγχος ταυτότητας και το TLS διαμορφώνονται εκεί.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ο δικός σας διακομιστής worklist μέσω DIMSE">}}

<p>Ένας διαμεσολαβητής worklist, ένα μικρό RIS ή ένα σύστημα δοκιμής για ενσωμάτωση συσκευής πρέπει να απαντήσει στην άλλη πλευρά. Καταχωρήστε έναν χειριστή C-FIND για τη worklist και χειριστές N-CREATE και N-SET για το MPPS, και η εφαρμογή γίνεται SCP worklist και SCP MPPS.</p>

<div class="codeblock" id="code">
 <h3>Ξεκινήστε έναν διακομιστή worklist και MPPS - C#</h3>
 <pre><code class="cs">// One handler instance answers N-CREATE and N-SET, so both see the same steps
MppsHandler mpps = new MppsHandler();

DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Any, 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityWorklistInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityPerformedProcedureStepSOPClass,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddSingletonCFindHandler(new WorklistHandler("worklist"))
    .AddSingletonNCreateHandler(mpps)
    .AddSingletonNSetHandler(mpps)
    .Build();

await server.StartAsync(CancellationToken.None);</code></pre>
</div>

<p>Αυτός ο χειριστής διαβάζει τις προγραμματισμένες εξετάσεις από αρχεία <code>.wl</code>, ένα σύνολο δεδομένων ανά αρχείο, και συγκρίνει τα τρία κλειδιά που συνήθως στέλνει η συσκευή. Στο DIMSE ο χειριστής διαχειρίζεται την αντιστοίχηση: μπαλαντέρ, εύρη ημερομηνιών και η επιλογή κλειδιών επιστροφής γράφονται σε αυτόν.</p>

<div class="codeblock" id="code">
 <h3>Ο χειριστής C-FIND της worklist - C#</h3>
 <pre><code class="cs">// Answers worklist queries from the .wl files in a folder, one scheduled exam per file
public sealed class WorklistHandler(string folder) : ICFindRequestHandler
{
    public async IAsyncEnumerable&lt;CFindResponse&gt; Handle(CFindRequest request,
        [EnumeratorCancellation] CancellationToken cancellationToken)
    {
        // A modality filters inside the scheduled procedure step: its AE title, its modality, the date
        Dataset keys = request.Identifier.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);
        Tag[] filters = [Tag.ScheduledStationAETitle, Tag.Modality, Tag.ScheduledProcedureStepStartDate];

        foreach (string path in Directory.EnumerateFiles(folder, "*.wl"))
        {
            DicomFile file = await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
            Dataset step = file.Dataset.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);
            if (filters.All(tag =&gt; Matches(keys, step, tag)))
                yield return WithStatus(new CFindResponse(file.Dataset), 0xFF00);
        }

        yield return WithStatus(new CFindResponse(null), 0x0000);
    }

    // Single value matching: an empty key matches every value
    private static bool Matches(Dataset keys, Dataset step, Tag tag)
    {
        string key = keys.GetSingleValueOrDefault(tag, string.Empty)!;
        return key.Length == 0 || key == step.GetSingleValueOrDefault(tag, string.Empty);
    }

    private static CFindResponse WithStatus(CFindResponse response, ushort status)
    {
        response.Command.AddOrUpdate(Tag.Status, status);
        return response;
    }
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Ο χειριστής N-CREATE και N-SET του MPPS - C#</h3>
 <pre><code class="cs">// Receives the performed procedure steps the modalities report
public sealed class MppsHandler : INCreateRequestHandler, INSetRequestHandler
{
    private readonly ConcurrentDictionary&lt;string, Dataset&gt; _steps = new();

    // N-CREATE: the exam has started
    public ValueTask&lt;NCreateResponse&gt; Handle(NCreateRequest request, CancellationToken cancellationToken)
    {
        bool created = _steps.TryAdd(request.AffectedSopInstanceUid.Code, request.Attributes ?? new Dataset());

        NCreateResponse response = new(null);
        response.Command.AddOrUpdate(Tag.Status, created ? (ushort)0x0000 : (ushort)0x0111); // 0x0111: duplicate instance
        return ValueTask.FromResult(response);
    }

    // N-SET: the exam is COMPLETED or DISCONTINUED
    public ValueTask&lt;NSetResponse&gt; Handle(NSetRequest request, CancellationToken cancellationToken)
    {
        NSetResponse response = new(null);
        if (!_steps.TryGetValue(request.RequestedSopInstanceUid.Code, out Dataset? step))
        {
            response.Command.AddOrUpdate(Tag.Status, (ushort)0x0112); // 0x0112: no such instance
            return ValueTask.FromResult(response);
        }

        step.AddOrUpdateRange(request.ModificationList);
        Console.WriteLine($"{step.GetSingleValueOrDefault(Tag.PatientID, string.Empty)}: " +
                          $"{step.GetSingleValueOrDefault(Tag.PerformedProcedureStepStatus, string.Empty)}");

        response.Command.AddOrUpdate(Tag.Status, (ushort)0x0000);
        return ValueTask.FromResult(response);
    }
}</code></pre>
</div>

<p>Εδώ τα βήματα διατηρούνται στη μνήμη και η κατάσταση εκτυπώνεται. Ένα πραγματικό MPPS SCP αποθηκεύει το βήμα, απορρίπτει αλλαγές σε βήμα που είναι ήδη COMPLETED ή DISCONTINUED, και παραδίδει το αποτέλεσμα στο RIS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ο δικός σας διακομιστής worklist μέσω DICOMweb">}}

<p>Στο DICOMweb η βιβλιοθήκη εκτελεί περισσότερη εργασία διακομιστή. Για μια αναζήτηση worklist εφαρμόζει τους κανόνες αντιστοίχισης DICOM, συμπεριλαμβανομένων των μπαλαντέρ και των εύρους ημερομηνιών, και σελιδοποιεί τα αποτελέσματα. Για το MPPS ελέγχει τα απαιτούμενα χαρακτηριστικά και τις μεταβάσεις κατάστασης. Οι κλάσεις σας παρέχουν τα δεδομένα.</p>

<div class="codeblock" id="code">
 <h3>Program.cs ενός διακομιστή DICOMweb worklist και MPPS - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// The scheduled exams and the reported procedure steps
builder.Services.AddSingleton&lt;IDicomWebModalityWorklistProvider&gt;(new FolderWorklist("worklist"));
builder.Services.AddSingleton&lt;IDicomWebMppsProvider, MppsStore&gt;();
builder.Services.AddDicomWeb()
    .AddStandardModalityWorklistService()
    .AddStandardModalityPerformedProcedureStepService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Η αποθήκευση worklist και MPPS - C#</h3>
 <pre><code class="cs">// The scheduled exams for DICOMweb searches; the library applies the matching rules and the paging
public sealed class FolderWorklist(string folder) : IDicomWebModalityWorklistProvider
{
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Directory.EnumerateFiles(folder, "*.wl").Order(StringComparer.Ordinal))
            yield return (await DicomFile.OpenAsync(path, cancellationToken: cancellationToken)).Dataset;
    }
}

// The performed procedure steps; the library checks the attributes and the state changes
public sealed class MppsStore : IDicomWebMppsProvider
{
    private readonly ConcurrentDictionary&lt;string, DicomWebVersionedDataset&gt; _steps = new();

    public ValueTask&lt;DicomWebVersionedDataset?&gt; GetAsync(string mppsUid, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_steps.TryGetValue(mppsUid, out DicomWebVersionedDataset? step) ? Copy(step) : null);

    public ValueTask&lt;bool&gt; TryCreateAsync(string mppsUid, Dataset procedureStep, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_steps.TryAdd(mppsUid, NewRevision(procedureStep)));

    // Replaces the step only if nobody changed it since it was read
    public ValueTask&lt;bool&gt; TryReplaceAsync(string mppsUid, string expectedRevision, Dataset procedureStep,
        CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_steps.TryGetValue(mppsUid, out DicomWebVersionedDataset? current)
                             &amp;&amp; current.Revision == expectedRevision
                             &amp;&amp; _steps.TryUpdate(mppsUid, NewRevision(procedureStep), current));

    private static DicomWebVersionedDataset NewRevision(Dataset dataset) =&gt;
        new() { Dataset = new Dataset(dataset), Revision = Guid.NewGuid().ToString("N") };

    private static DicomWebVersionedDataset Copy(DicomWebVersionedDataset step) =&gt;
        new() { Dataset = new Dataset(step.Dataset), Revision = step.Revision };
}</code></pre>
</div>

<p>Ο διακομιστής απαιτεί το πακέτο <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Η <a href="/medical/net/dicomweb-server/">σελίδα του DICOMweb server</a> δείχνει πώς να προσθέσετε τις υπηρεσίες Studies στην ίδια εφαρμογή, και ο <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">οδηγός παρόχων ροής εργασίας</a> εξηγεί το συμβόλαιο αναθεώρησης του <code>TryReplaceAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Λίστες εργασιών πέρα από τη συσκευή">}}

<p>MWL και MPPS συνδέουν μια συσκευή με ένα RIS. Για εργασία που μεταβαίνει μεταξύ άλλων συστημάτων, όπως επεξεργασία AI, εκχώρηση ανάγνωσης ή post-processing, το DICOM ορίζει το Unified Procedure Step. Η <a href="/medical/net/ups-rs/">σελίδα UPS-RS</a> δείχνει πώς να δημιουργήσετε, να διεκδικήσετε και να ολοκληρώσετε τέτοια workitems με την ίδια βιβλιοθήκη.</p>

<p>Σχετικές σελίδες: <a href="/medical/net/dicom-networking/">Δικτύωση DICOM</a> για C-ECHO, C-STORE και τις άλλες υπηρεσίες DIMSE, <a href="/medical/net/dicomweb-client/">πελάτης DICOMweb</a> και <a href="/medical/net/dicomweb-server/">διακομιστής DICOMweb</a> για μελέτες μέσω HTTP, και <a href="/medical/net/dicom-tags/">ετικέτες DICOM</a> για να διαβάσετε τι περιέχει ένα στοιχείο worklist.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι Μάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Οδηγός Προγραμματιστή" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
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
