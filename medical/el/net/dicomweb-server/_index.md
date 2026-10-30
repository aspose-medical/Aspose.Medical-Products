---
title: Διακομιστής DICOMweb σε C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Δημιουργήστε τον δικό σας διακομιστή DICOMweb ή PACS σε C#. Το Aspose.Medical υλοποιεί QIDO-RS, WADO-RS, STOW-RS και τις υπηρεσίες λιστών εργασιών στο ASP.NET Core· εσείς συνδέετε την αποθήκευση.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Διακομιστής DICOMweb για .NET C#" h2="Προσθέστε τα σημεία άκρης QIDO-RS, WADO-RS και STOW-RS σε μια υπηρεσία ASP.NET Core. Η βιβλιοθήκη μιλάει το πρωτόκολλο· ο κώδικάς σας αποφασίζει πού αποθηκεύονται οι μελέτες." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Ο δικός σας διακομιστής DICOMweb σε C#">}}

<p>Ένα PACS, ένα VNA, ένα cloud αρχείο ή μια ερευνητική πλατφόρμα πρέπει να ανταποκρίνεται σε αιτήματα DICOMweb: αναζητήσεις QIDO-RS, λήψεις WADO-RS και μεταφορτώσεις STOW-RS, καθώς συχνά και τις υπηρεσίες λιστών εργασιών. Κάθε μία έχει τις δικές της διαδρομές, τύπους μέσων, multipart σώματα, κανόνες αντιστοίχισης και κωδικούς κατάστασης.</p>

<p><strong>Aspose.Medical for .NET</strong> υλοποιεί αυτή τη πλευρά του πρωτοκόλλου από την έκδοση 26.10. Διαβάζει κάθε αίτημα, το ελέγχει με το πρότυπο, ταιριάζει τα κλειδιά αναζήτησης σύμφωνα με τους κανόνες DICOM, διαμερίζει τα αποτελέσματα, μετατρέπει τις συντακτικές μεταφορές και γράφει την απόκριση. Ο κώδικάς σας κάνει αυτό που μόνο εσείς μπορείτε: αποθηκεύει ένα στιγμιότυπο, βρίσκει υποψηφίους για αναζήτηση και επιστρέφει τα αποθηκευμένα αρχεία. Οι προβολείς, οι modalities και άλλα αρχεία τότε λειτουργούν με τον διακομιστή σας όπως με οποιονδήποτε άλλο διακομιστή DICOMweb, και το ίδιο ισχύει για τον <a href="/medical/net/dicomweb-client/">πελάτη DICOMweb</a> της ίδιας βιβλιοθήκης.</p>

<p>Ο διακομιστής είναι διαχειριζόμενος σε C#. Εκτελείται όπου εκτελείται το ASP.NET Core: στα Windows, στο Linux και σε ένα container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Πλήρης διακομιστής σε ASP.NET Core">}}

<p>Εγκαταστήστε το πακέτο <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Φέρνει το Aspose.Medical μαζί του και συνδέει τις υπηρεσίες DICOMweb με την εξάρτηση injection και το routing σημείων άκρης. Αυτό το <code>Program.cs</code> είναι ένας λειτουργικός διακομιστής Studies· οι δύο κλάσεις που καταχωρεί εμφανίζονται παρακάτω.</p>

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

<p>Κάθε δρομολόγιο Studies ανταποκρίνεται τώρα στο <code>/dicomweb</code>: <code>/dicomweb/studies</code> για αναζήτηση και μεταφόρτωση, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> για ανάκτηση, καθώς και τα δρομολόγια για μεταδεδομένα, καρέ, bulk δεδομένα και αποδομημένες εικόνες. Το έγγραφο δυνατοτήτων στη ρίζα της υπηρεσίας καταχωρίζει ακριβώς τις συναλλαγές που υποστηρίζει ο διακομιστής.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Συνδέστε την αποθήκη σας">}}

<p>Η υπηρεσία Studies χρειάζεται μία διεπαφή με τρεις μεθόδους: αποθήκευση ενός στιγμιότυπου, επιστροφή των αποθηκευμένων αρχείων και επιστροφή υποψηφίων αναζήτησης. Αυτή η υλοποίηση διατηρεί κάθε στιγμιότυπο ως αρχείο σε φάκελο ανά μελέτη και σειρά.</p>

<div class="codeblock" id="code">
 <h3>Αρχείο φακέλου πίσω από QIDO-RS, WADO-RS και STOW-RS - C#</h3>
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

<p>Η βιβλιοθήκη ελέγχει τα UID σε κάθε διαδρομή αιτήματος και σε κάθε μεταφορτωμένο στιγμιότυπο πριν καλέσει τον πάροχο. Ένα στιγμιότυπο χωρίς έγκυρα UID αναφέρεται ως αποτυχία και δεν φτάνει ποτέ το <code>StoreAsync</code>.</p>

<p>Ένας φάκελος είναι επαρκής για δοκιμή του διακομιστή. Ένα παραγωγικό αρχείο διατηρεί τις ίδιες τρεις μεθόδους και τοποθετεί ένα αποθηκευτικό αντικειμένων πίσω από το <code>StoreAsync</code> και το <code>RetrieveAsync</code> και ένα ευρετήριο βάσης δεδομένων πίσω από το <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αναζήτηση: η βιβλιοθήκη εφαρμόζει τους κανόνες αντιστοίχισης DICOM">}}

<p>Η αντιστοίχηση QIDO-RS είναι περισσότερη από απλή σύγκριση συμβολοσειρών: μπαλαντέρ, εύρη ημερομηνίας και ώρας, λίστες UID, ασαφή ονόματα ατόμων, κενές τιμές και κλειδιά μέσα στις ακολουθίες. Ο πάροχος δεν υλοποιεί κανένα από αυτά. Επιστρέφει υποψηφίους με σταθερή σειρά, και μπορεί να επιστρέψει πιο πολλά από τα ταιριαστά. Η βιβλιοθήκη εφαρμόζει την ακριβή αντιστοίχηση DICOM σε κάθε υποψήφιο, διατηρεί τα ζητηθέντα πεδία και εφαρμόζει <code>offset</code> και <code>limit</code>. Μια απόκριση περιέχει το πολύ 1000 αποτελέσματα εξ ορισμού, και το <code>DicomWebStandardServiceOptions</code> αλλάζει αυτό το όριο.</p>

<p>Για να διατηρηθούν οι αναζητήσεις γρήγορες σε μεγάλο αρχείο, μεταβιβάστε τα επιλεκτικά κλειδιά στη βάση δεδομένων. Το <code>search.Plan.Filter</code> περιέχει τα μεταγλωττισμένα κλειδιά αναζήτησης, με το χαρακτηριστικό, τον τύπο αντιστοίχισης και τις τιμές του καθενός:</p>

<div class="codeblock" id="code">
 <h3>Ανάγνωση ακριβούς Patient ID από αναζήτηση - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Μια ερώτηση βάσης δεδομένων που χτίζεται με αυτόν τον τρόπο μπορεί να επιστρέψει επιπλέον γραμμές, π.χ. όταν αγνοεί ένα κλειδί που δεν μπορεί να μεταφράσει. Η τελική αντιστοίχηση στη βιβλιοθήκη τα αφαιρεί. Δεν πρέπει να απορρίπτει πραγματικές αντιστοιχίες, επειδή η βιβλιοθήκη δεν μπορεί να τις προσθέσει ξανά.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αποθήκευση: ένα αποτέλεσμα για κάθε στιγμιότυπο">}}

<p>Ένα αίτημα STOW-RS μπορεί να μεταφέρει εκατοντάδες στιγμιότυπα. Η βιβλιοθήκη αναλύει το multipart σώμα και καλεί το <code>StoreAsync</code> για κάθε στιγμιότυπο διαδοχικά, ώστε η πλήρης μεταφόρτωση να μην κρατάται ποτέ εξ ολοκλήρου στη μνήμη. Για κάθε στιγμιότυπο, ο πάροχος επιστρέφει <code>Stored</code>, <code>Warning</code> για ένα στιγμιότυπο που αποθηκεύτηκε με αλλαγές, ή <code>Failed</code> με έναν προαιρετικό κωδικό αιτίας DICOM.</p>

<p>Η βιβλιοθήκη δημιουργεί την αναφορά STOW-RS από αυτά τα αποτελέσματα και επιλέγει την κατάσταση: 200 όταν κάθε στιγμιότυπο αποθηκεύεται, 202 όταν κάποια έχουν προειδοποιήσεις ή αποτύχουν, 409 όταν όλα αποτυγχάνουν. Στιγμιότυπα σε αίτημα για μια ονομασμένη μελέτη που ανήκουν σε άλλη μελέτη απορρίπτονται πριν κληθεί ο πάροχος.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ανάκτηση: συντακτικές μεταφορές, καρέ και αποδομημένες εικόνες">}}

<p>Ο πάροχος επιστρέφει τα αποθηκευμένα αρχεία και η βιβλιοθήκη τα παραδίδει στη μορφή που ζήτησε ο πελάτης. Ένας πελάτης WADO-RS μπορεί να ζητήσει μια συντακτική μεταφορά στο header <code>Accept</code>, και η βιβλιοθήκη μετατρέπει κάθε αρχείο πριν το στείλει, με τους δικούς της κωδικοποιητές, συμπεριλαμβανομένων HTJ2K και JPEG XL. Τα μεταδεδομένα σε DICOM JSON ή XML, τα bulk δεδομένα και τα μεμονωμένα καρέ προέρχονται από τον ίδιο πάροχο, χωρίς επιπλέον κώδικα.</p>

<p>Οι αποδομημένες εικόνες χρειάζονται μια επιπλέον κλάση, επειδή μόνο εσείς αποφασίζετε ποιες εικόνες μπορεί να παράγει ο διακομιστής. Αυτή προσφέρει JPEG για αποδομημένα στιγμιότυπα, αποδομημένα καρέ και μικρογραφίες, και η βιβλιοθήκη τα αποδίδει:</p>

<div class="codeblock" id="code">
 <h3>Προσφορά αποδομημένων JPEG εικόνων και μικρογραφιών - C#</h3>
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

<p>Ο ενσωματωμένος αποσυνιστωτής δημιουργεί JPEG από ένα στιγμιότυπο μονού-καρέ ή από τα καρέ που επιλέγει ένα αίτημα. Ένα αποδομημένο πολλαπλό-καρέ στιγμιότυπο ως σύνολο, μια εικόνα ευρύτερη ή υψηλότερη από 65500 pixel (οι μικρογραφίες περνούν πάνω από τέτοιες εικόνες), άλλες μορφές εικόνας, και οι παράμετροι viewport, window και ICC profile απαιτούν το δικό σας <code>RetrieveAsync</code> σε αυτήν την κλάση.</p>

<p>Οι μικρογραφίες διαχειρίζονται με προσοχή για το απόρρητο του ασθενούς. Ο ενσωματωμένος αποσυνιστωτής δημιουργεί μικρογραφία μόνο από ένα στιγμιότυπο του οποίου τα χαρακτηριστικά Burned In Annotation και Recognizable Visual Features έχουν και τα δύο τιμή <code>NO</code>, που σημαίνει ότι η εικόνα δηλώνει ότι δεν εμφανίζει δεδομένα ασθενούς. Πολλές εικόνες δεν διαθέτουν αυτά τα χαρακτηριστικά, και για αυτές ο διακομιστής απαντά 406. Εάν το αρχείο σας χρειάζεται μικρογραφίες τέτοιων εικόνων, δημιουργήστε τις στο δικό σας <code>RetrieveAsync</code> και βεβαιωθείτε ότι κρύβουν τυχόν δεδομένα ασθενούς.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Λίστες εργασιών, βήματα διαδικασίας και οι άλλες υπηρεσίες">}}

<p>Οι άλλες υπηρεσίες DICOMweb ακολουθούν το ίδιο μοντέλο: καταχωρίστε την υπηρεσία, υλοποιήστε τη διεπαφή αποθήκευσής της.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> λίστες εργασιών με <code>AddStandardWorklistService</code>: δημιουργία, αναζήτηση και ενημέρωση αντικειμένων εργασίας και αλλαγή της κατάστασής τους, με τους κανόνες κατάστασης να ελέγχονται από τη βιβλιοθήκη και ειδοποιήσεις συμβάντων να αποστέλλονται μέσω WebSocket.</li>
<li>Η <a href="/medical/net/modality-worklist/">λίστα εργασιών modality</a> (MWL-RS) με <code>AddStandardModalityWorklistService</code>, ώστε οι modalities να μπορούν να ερωτούν τα προγραμματισμένα βήματα διαδικασίας του RIS σας.</li>
<li>Βήματα διαδικασίας που εκτελέστηκαν από modality (MPPS-RS), δέσμευση αποθήκευσης και στιγμιότυπα χωρίς ασθενή όπως χρωματικές παλέτες και hanging protocols.</li>
<li>Ανάκτηση WADO-URI για παλαιότερους προβολείς, εξυπηρετείται από την υπηρεσία Studies.</li>
</ul>

<p>Όταν η τυπική συμπεριφορά μιας συναλλαγής δεν ταιριάζει, καταχωρίστε το δικό σας χειριστή για αυτήν με <code>AddHandler</code>. Η βιβλιοθήκη εξακολουθεί να δρομολογεί και να ελέγχει το αίτημα, ενώ οι άλλες συναλλαγές διατηρούν τους τυπικούς χειριστές τους.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Η ασφάλεια και η φιλοξενία παραμένουν στα χέρια σας">}}

<p>Τα σημεία άκρης DICOMweb είναι απλά σημεία άκρης ASP.NET Core. Η ταυτοποίηση, η εξουσιοδότηση, CORS, TLS, προωθημένες κεφαλίδες και τα όρια μεγέθους αιτήματος διαμορφώνονται με τον ίδιο τρόπο όπως για την υπόλοιπη υπηρεσία σας:</p>

<div class="codeblock" id="code">
 <h3>Απαιτείται αυθεντικοποιημένος χρήστης - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Το τμήμα του πρωτοκόλλου δεν εξαρτάται από το ASP.NET Core. Το <code>DicomWebApplication</code> μετατρέπει ένα <code>DicomWebRequest</code> σε απόκριση, ώστε οι ίδιες υπηρεσίες να μπορούν να τρέχουν πίσω από άλλον διακομιστή HTTP ή σε δοκιμή χωρίς δίκτυο:</p>

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

<p>Ο <a href="/medical/net/dicomweb-client/">πελάτης DICOMweb</a> του Aspose.Medical δοκιμάζει τον διακομιστή σας από μια μονάδα δοκιμής ή ένα πρόγραμμα κονσόλας, και κάθε αρχείο που λαμβάνει ο διακομιστής είναι ένα κανονικό <code>DicomFile</code> που μπορεί να ανωνυμοποιηθεί, μετακωδικοποιηθεί ή αποδοθεί στην ίδια διαδικασία.</p>

<p>Ο <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">οδηγός προγραμματιστή DICOMweb</a> καλύπτει κάθε υπηρεσία και διεπαφή αποθήκευσης, και ο <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">οδηγός ASP.NET Core</a> καλύπτει τη χαρτογράφηση σημείων άκρης και WebSockets.</p>

<p>Σχετικές σελίδες: <a href="/medical/net/dicom-networking/">DICOM networking</a> για συστήματα που στέλνουν μελέτες μέσω DIMSE αντί για HTTP, <a href="/medical/net/anonymization/">ανωνυμοποίηση</a> εισερχόμενων μελετών, <a href="/medical/net/dicom-transfer-syntax-conversion/">μετατροπή συντακτικής μεταφοράς</a>, και <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> για τη μορφή που χρησιμοποιούν τα αποτελέσματα QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι Μάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Οδηγός Προγραμματιστή" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
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
