---
title: Διακομιστής DICOMweb σε C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Δημιουργήστε το δικό σας διακομιστή DICOMweb ή PACS σε C#. Το Aspose.Medical υλοποιεί QIDO-RS, WADO-RS, STOW-RS και τις υπηρεσίες λιστών εργασίας στο ASP.NET Core· εσείς συνδέετε την αποθήκευση.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Διακομιστής DICOMweb για .NET C#" h2="Προσθέστε τα σημεία τελικής πρόσβασης QIDO-RS, WADO-RS και STOW-RS σε μια υπηρεσία ASP.NET Core. Η βιβλιοθήκη μιλά το πρωτόκολλο· ο κώδικάς σας αποφασίζει πού θα αποθηκεύονται οι μελέτες." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Ο δικός σας διακομιστής DICOMweb σε C#">}}

<p>Ένα PACS, ένα VNA, ένα cloud αρχείο ή μια ερευνητική πλατφόρμα πρέπει να απαντούν σε αιτήματα DICOMweb: αναζητήσεις QIDO‑RS, λήψεις WADO‑RS και μεταφορτώσεις STOW‑RS, καθώς και συχνά τις υπηρεσίες λιστών εργασίας. Κάθε μία έχει τις δικές της διαδρομές, τύπους media, πολυμέρη σώματα, κανόνες αντιστοίχισης και κωδικούς κατάστασης.</p>

<p><strong>Aspose.Medical for .NET</strong> υλοποιεί αυτή τη πλευρά του πρωτοκόλλου από την έκδοση 26.10. Διαβάζει κάθε αίτημα, το ελέγχει κατά το πρότυπο, αντιστοιχεί τα κλειδιά αναζήτησης σύμφωνα με τους κανόνες DICOM, σελιδοποιεί τα αποτελέσματα, μετατρέπει τις συντακτικές μεταφορές και γράφει την απόκριση. Ο κώδικάς σας κάνει ό,τι μόνο εσείς μπορείτε: αποθηκεύει ένα instance, βρίσκει υποψηφίους για μια αναζήτηση και επιστρέφει τα αποθηκευμένα αρχεία. Οι προβολείς, οι μορφολογίες και άλλα αρχεία τότε λειτουργούν με τον διακομιστή σας όπως με οποιονδήποτε άλλο διακομιστή DICOMweb, και το ίδιο ισχύει για τον <a href="/medical/net/dicomweb-client/">πελάτη DICOMweb</a> της ίδιας βιβλιοθήκης.</p>

<p>Ο διακομιστής διαχειρίζεται σε C#. Τρέχει όπου τρέχει το ASP.NET Core: σε Windows, σε Linux και σε κοντέινερ.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ένας πλήρης διακομιστής σε ASP.NET Core">}}

<p>Εγκαταστήστε το πακέτο <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Φέρνει το Aspose.Medical μαζί του και συνδέει τις υπηρεσίες DICOMweb με την εξάρτηση injection και το endpoint routing. Αυτό το <code>Program.cs</code> είναι ένας λειτουργικός διακομιστής Studies· οι δύο κλάσεις που καταχωρεί εμφανίζονται παρακάτω.</p>

<div class="codeblock" id="code">
 <h3>Program.cs ενός διακομιστή DICOMweb - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>Κάθε διαδρομή του Studies τώρα ανταποκρίνεται κάτω από <code>/dicomweb</code>: <code>/dicomweb/studies</code> για αναζήτηση και μεταφόρτωση, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> για ανάκτηση, καθώς και οι διαδρομές για μεταδεδομένα, καρέ, μαζικά δεδομένα και αποδομένες εικόνες. Το έγγραφο δυνατοτήτων στη ρίζα της υπηρεσίας καταγράφει ακριβώς τις συναλλαγές που υποστηρίζει ο διακομιστής.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Συνδέστε την αποθήκευσή σας">}}

<p>Η υπηρεσία Studies χρειάζεται μία διεπαφή με τρεις μεθόδους: αποθήκευση ενός instance, επιστροφή των αποθηκευμένων αρχείων και επιστροφή υποψηφίων αναζήτησης. Αυτή η υλοποίηση διατηρεί κάθε instance ως αρχείο σε φάκελο ανά μελέτη και series.</p>

<div class="codeblock" id="code">
 <h3>Φάκελος αποθήκης πίσω από QIDO‑RS, WADO‑RS και STOW‑RS - C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>Η βιβλιοθήκη ελέγχει τα UID σε κάθε διαδρομή αιτήματος και σε κάθε ανεβασμένο instance πριν καλέσει τον provider. Ένα instance χωρίς έγκυρα UID καταχωρείται ως αποτυχία και δεν φθάνει ποτέ στο <code>StoreAsync</code>.</p>

<p>Ένας φάκελος είναι αρκετός για να δοκιμάσετε τον διακομιστή. Ένα παραγωγικό αποθετήριο διατηρεί τις ίδιες τρεις μεθόδους και τοποθετεί ένα αποθηκευτικό αντικειμένων πίσω από το <code>StoreAsync</code> και <code>RetrieveAsync</code> και ένα ευρετήριο βάσης δεδομένων πίσω από το <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αναζήτηση: η βιβλιοθήκη εφαρμόζει τους κανόνες αντιστοίχισης DICOM">}}

<p>Η αντιστοίχηση QIDO‑RS είναι κάτι παραπάνω από σύγκριση συμβολοσειρών: μπαλαντέρ, εύρη ημερομηνίας και ώρας, λίστες UID, ασαφή ονόματα ατόμων, κενές τιμές και κλειδιά μέσα σε ακολουθίες. Ο provider δεν υλοποιεί κανένα από αυτά. Επιστρέφει υποψηφίους με σταθερή σειρά και μπορεί να επιστρέψει περισσότερα από τα ταιριαστά. Η βιβλιοθήκη εφαρμόζει την ακριβή αντιστοίχηση DICOM σε κάθε υποψήφιο, διατηρεί τα ζητούμενα πεδία και εφαρμόζει <code>offset</code> και <code>limit</code>. Μία απόκριση περιέχει το πολύ 1000 αποτελέσματα εξ ορισμού, και το <code>DicomWebStandardServiceOptions</code> αλλάζει αυτό το όριο.</p>

<p>Για να διατηρηθούν οι αναζητήσεις γρήγορες σε μεγάλο αποθετήριο, μεταβιβάστε τα επιλεκτικά κλειδιά στη βάση δεδομένων. Το <code>search.Plan.Filter</code> κρατά τα μεταγλωττισμένα κλειδιά αναζήτησης, με το χαρακτηριστικό, τον τύπο αντιστοίχισης και τις τιμές του καθενός:</p>

<div class="codeblock" id="code">
 <h3>Ανάγνωση ακριβούς Patient ID από αναζήτηση - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Ένα ερώτημα βάσης δεδομένων που χτίζεται έτσι μπορεί να επιστρέφει επιπλέον γραμμές, για παράδειγμα όταν αγνοεί ένα κλειδί που δεν μπορεί να μεταφράσει. Η τελική αντιστοίχηση στη βιβλιοθήκη τις αφαιρεί. Δεν πρέπει να αφαιρέσει πραγματικές αντιστοιχίες, επειδή η βιβλιοθήκη δεν μπορεί να τις προσθέσει ξανά.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αποθήκευση: αποτέλεσμα για κάθε instance">}}

<p>Ένα αίτημα STOW‑RS μπορεί να μεταφέρει εκατοντάδες instances. Η βιβλιοθήκη αναλύει το multipart σώμα και καλεί το <code>StoreAsync</code> για κάθε instance διαδοχικά, ώστε η συνολική μεταφόρτωση να μην κρατιέται ποτέ ολόκληρη στη μνήμη. Για κάθε instance ο provider επιστρέφει <code>Stored</code>, <code>Warning</code> για ένα instance αποθηκευμένο με αλλαγές ή <code>Failed</code> με προαιρετικό κωδικό αιτίας DICOM.</p>

<p>Η βιβλιοθήκη κατασκευάζει την αναφορά STOW‑RS από αυτά τα αποτελέσματα και επιλέγει την κατάσταση: 200 όταν κάθε instance είναι αποθηκευμένο, 202 όταν κάποια έχουν προειδοποιήσεις ή αποτυχίες, 409 όταν όλα απέτυχαν. Τα instances σε αίτημα για μια ονομασμένη μελέτη που ανήκουν σε άλλη μελέτη απορρίπτονται πριν κληθεί ο provider.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ανάκτηση: συντακτικές μεταφορές, καρέ και αποδομένες εικόνες">}}

<p>Ο provider επιστρέφει αποθηκευμένα αρχεία, και η βιβλιοθήκη τα παραδίδει στη μορφή που ζήτησε ο πελάτης. Ένας πελάτης WADO‑RS μπορεί να ζητήσει μια συντακτική μεταφορά στο header <code>Accept</code>, και η βιβλιοθήκη μετατρέπει κάθε αρχείο πριν το στείλει, με τους δικούς της κωδικοποιητές, συμπεριλαμβανομένων των HTJ2K και JPEG XL. Τα μεταδεδομένα σε DICOM JSON ή XML, τα μαζικά δεδομένα και τα μεμονωμένα καρέ προέρχονται από τον ίδιο provider, χωρίς πρόσθετο κώδικα.</p>

<p>Οι αποδομένες εικόνες απαιτούν μία ακόμη κλάση, επειδή μόνο εσείς αποφασίζετε ποιες εικόνες μπορεί να παραγάγει ο διακομιστής. Αυτή προσφέρει JPEG για render‑αρισμένα instances, render‑αρισμένα καρέ και μικρογραφίες, και η βιβλιοθήκη τις αποδίδει:</p>

<div class="codeblock" id="code">
 <h3>Προσφορά αποδομένων εικόνων JPEG και μικρογραφιών - C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>Ο ενσωματωμένος renderer παράγει JPEG από ένα instance μονής λήψης ή από τα καρέ που επιλέγει το αίτημα. Ένα render‑αρισμένο multi‑frame instance στο σύνολό του, άλλες μορφές εικόνας, καθώς και οι παράμετροι viewport, window και ICC profile απαιτούν το δικό σας <code>RetrieveAsync</code> σε αυτήν την κλάση.</p>

<p>Οι μικρογραφίες χειρίζονται με προσοχή για την ιδιωτικότητα του ασθενούς. Ο ενσωματωμένος renderer δημιουργεί μικρογραφία μόνο από ένα instance του οποίου τα χαρακτηριστικά Burned In Annotation και Recognizable Visual Features είναι και τα δύο <code>NO</code>, που σημαίνει ότι η εικόνα δηλώνει ότι δεν εμφανίζει δεδομένα ασθενούς. Πολλές εικόνες δεν διαθέτουν αυτά τα χαρακτηριστικά, και για αυτές ο διακομιστής απαντά 406. Εάν το αποθετήριό σας χρειάζεται μικρογραφίες τέτοιων εικόνων, δημιουργήστε τις στο δικό σας <code>RetrieveAsync</code> και βεβαιωθείτε ότι κρύβουν τυχόν δεδομένα ασθενούς.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Λίστες εργασίας, βήματα διαδικασίας και οι άλλες υπηρεσίες">}}

<p>Οι άλλες υπηρεσίες DICOMweb ακολουθούν το ίδιο μοντέλο: καταχωρήστε την υπηρεσία, υλοποιήστε τη διεπαφή αποθήκευσής της.</p>

<ul>
<li>Λίστες εργασίας UPS‑RS με <code>AddStandardWorklistService</code>: δημιουργία, αναζήτηση και ενημέρωση workitems και αλλαγή της κατάστασής τους, με τους κανόνες κατάστασης να ελέγχονται από τη βιβλιοθήκη και ειδοποιήσεις συμβάντων να αποστέλλονται μέσω WebSocket.</li>
<li>Η λίστα εργασίας μορφολογίας (MWL‑RS) με <code>AddStandardModalityWorklistService</code>, ώστε οι μορφολογίες να μπορούν να αναζητούν τα προγραμματισμένα βήματα διαδικασίας του RIS σας.</li>
<li>Βήματα διαδικασίας που εκτελέστηκαν από τη μορφολογία (MPPS‑RS), δέσμευση αποθήκευσης και instances χωρίς ασθενή όπως παλέτες χρωμάτων και hanging protocols.</li>
<li>Ανάκτηση WADO‑URI για παλαιότερους προβολείς, εξυπηρετείται από την υπηρεσία Studies.</li>
</ul>

<p>Όταν η τυπική συμπεριφορά μιας συναλλαγής δεν ταιριάζει, καταχωρήστε το δικό σας handler γι' αυτήν με το <code>AddHandler</code>. Η βιβλιοθήκη συνεχίζει να δρομολογεί και να ελέγχει το αίτημα, και οι άλλες συναλλαγές διατηρούν τους τυπικούς τους handlers.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Η ασφάλεια και η φιλοξενία παραμένουν στα χέρια σας">}}

<p>Τα σημεία τελικής πρόσβασης DICOMweb είναι απλά σημεία τελικής πρόσβασης ASP.NET Core. Η αυθεντικοποίηση, εξουσιοδότηση, CORS, TLS, προωθημένα headers και όρια μεγέθους αιτήματος διαμορφώνονται με τον ίδιο τρόπο όπως για το υπόλοιπο της υπηρεσίας σας:</p>

<div class="codeblock" id="code">
 <h3>Απαιτείται αυθεντικοποιημένος χρήστης - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Το μέρος του πρωτοκόλλου δεν εξαρτάται από το ASP.NET Core. Το <code>DicomWebApplication</code> μετατρέπει ένα <code>DicomWebRequest</code> σε απόκριση, ώστε οι ίδιες υπηρεσίες να μπορούν να τρέχουν πίσω από έναν άλλο HTTP server ή σε δοκιμή χωρίς δίκτυο:</p>

<div class="codeblock" id="code">
 <h3>Ο ίδιος διακομιστής χωρίς ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Πελάτης και διακομιστής από μία βιβλιοθήκη">}}

<p>Ο <a href="/medical/net/dicomweb-client/">πελάτης DICOMweb</a> του Aspose.Medical ελέγχει το διακομιστή σας από μια μονάδα δοκιμής ή ένα πρόγραμμα κονσόλας, και κάθε αρχείο που λαμβάνει ο διακομιστής είναι ένα κανονικό <code>DicomFile</code> που μπορεί να ανωνυμοποιηθεί, να μετακωδικοποιηθεί ή να αποδοθεί στην ίδια διαδικασία.</p>

<p>Ο <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">οδηγός προγραμματιστή DICOMweb</a> καλύπτει κάθε υπηρεσία και διεπαφή αποθήκευσης, και ο <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">οδηγός ASP.NET Core</a> καλύπτει την αντιστοίχηση σημείων τελικής πρόσβασης και τα WebSockets.</p>

<p>Σχετικές σελίδες: <a href="/medical/net/dicom-networking/">Δικτύωση DICOM</a> για συστήματα που στέλνουν μελέτες μέσω DIMSE αντί για HTTP, <a href="/medical/net/anonymization/">ανωνυμοποίηση</a> εισερχόμενων μελετών, <a href="/medical/net/dicom-transfer-syntax-conversion/">μετατροπή συντακτικής μεταφοράς</a>, και <a href="/medical/net/dicom-to-json/">DICOM σε JSON</a> για τη μορφή που χρησιμοποιούν τα αποτελέσματα QIDO‑RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι μάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Οδηγός προγραμματιστή" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
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
