---
title: Αποανωνυμοποίηση Αρχείων DICOM σε C# .NET | Aspose.Medical
weight: 1000
description: Αποανωνυμοποιήστε αρχεία DICOM σε C# .NET χρησιμοποιώντας ρυθμιζόμενα προφίλ εμπιστευτικότητας. Αφαιρέστε τα προσωπικά δεδομένα (PII) του ασθενούς, διασφαλίζοντας τη συμμόρφωση με HIPAA και GDPR μέσω του API της Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Αποανωνυμοποίηση Αρχείων DICOM σε .NET C#" h2="Αφαιρέστε τα στοιχεία ταυτοποίησης του ασθενούς από αρχεία DICOM χρησιμοποιώντας τα προφίλ εμπιστευτικότητας DICOM PS 3.15. Διασφαλίστε τη συμμόρφωση με HIPAA και GDPR με μια καθαρή βιβλιοθήκη .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Αποανωνυμοποίηση DICOM Βασισμένη σε Πρότυπα">}}

<p><strong>Aspose.Medical for .NET</strong> παρέχει ένα ολοκληρωμένο API αποανωνυμοποίησης DICOM βασισμένο στα <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">Προφίλ Εμπιστευτικότητας DICOM PS 3.15</a>. Το API επιτρέπει στους προγραμματιστές υγειονομικής περίθαλψης να αφαιρέσουν ή να τροποποιήσουν τα στοιχεία ταυτοποίησης του ασθενούς (PII) από αρχεία DICOM διατηρώντας την κλινική αξία των δεδομένων απεικόνισης. Σε αντίθεση με τις απλές προσεγγίσεις αφαίρεσης ετικετών, το Aspose.Medical ακολουθεί το επίσημο πρότυπο DICOM για να εξασφαλίσει τη σωστή αποπροσωποποίηση.</p>

<p>Η κλάση <code>Anonymizer</code> λειτουργεί με αντικείμενα <code>ConfidentialityProfile</code> που ορίζουν ακριβώς πώς πρέπει να αντιμετωπίζεται κάθε χαρακτηριστικό DICOM &mdash; αν θα πρέπει να αφαιρεθεί, να αντικατασταθεί με ψεύτικη τιμή, να καθαριστεί ή να παραμείνει αμετάβλητο. Αυτή η προσέγγιση εξασφαλίζει συνεπή, επαληθεύσιμη αποανωνυμοποίηση που πληροί τις κανονιστικές απαιτήσεις.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αποανωνυμοποίηση Αρχείου DICOM σε C#">}}

<p>Ο πιο απλός τρόπος να αποανωνυμοποιήσετε ένα αρχείο DICOM είναι να χρησιμοποιήσετε το προεπιλεγμένο προφίλ εμπιστευτικότητας. Αυτό εφαρμόζει το Βασικό Προφίλ DICOM PS 3.15, το οποίο διαχειρίζεται όλα τα τυπικά χαρακτηριστικά ταυτοποίησης ασθενούς:</p>

<div class="codeblock" id="code">
 <h3>Βασική αποανωνυμοποίηση DICOM - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>Η μέθοδος <code>Anonymize</code> είναι <strong>μη καταστροφική</strong> &mdash; κλωνοποιεί το αρχικό σύνολο δεδομένων και επιστρέφει ένα νέο αποανωνυμοποιημένο αντίγραφο. Το αρχικό αρχείο DICOM παραμένει αμετάβλητο, κάτι που είναι απαραίτητο για τις αλυσίδες ελέγχου και τις ροές εργασίας ακεραιότητας δεδομένων.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Προσαρμοσμένη Αντικατάσταση Ταυτότητας Ασθενούς">}}

<p>Σε πολλές ροές εργασίας, χρειάζεται να αντικαταστήσετε τις πληροφορίες του ασθενούς με συγκεκριμένες ψευδώνυμες τιμές αντί να τις αφαιρέσετε εντελώς. Η κλάση <code>ConfidentialityProfile</code> σας επιτρέπει να ορίσετε τιμές αντικατάστασης για το όνομα του ασθενούς και το αναγνωριστικό του ασθενούς:</p>

<div class="codeblock" id="code">
 <h3>Αποανωνυμοποίηση με προσαρμοσμένη ταυτότητα ασθενούς - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create a confidentiality profile with custom replacement values
ConfidentialityProfile profile = new()
{
    PatientName = "ANONYMOUS PATIENT",
    PatientId = "00000000"
};

// Create an anonymizer with this profile
Anonymizer anonymizer = new(profile);

// Anonymize the file
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("custom_anonymized.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αποανωνυμοποίηση εντός μνήμης">}}

<p>Για σενάρια όπου η αποδοτικότητα μνήμης είναι σημαντική ή δεν χρειάζεται να διατηρήσετε τα αρχικά δεδομένα, το API παρέχει αποανωνυμοποίηση εντός μνήμης που τροποποιεί το σύνολο δεδομένων άμεσα:</p>

<div class="codeblock" id="code">
 <h3>Αποανωνυμοποίηση DICOM εντός μνήμης - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Τόσο οι μέθοδοι <code>Anonymize</code> όσο και <code>AnonymizeInPlace</code> δέχονται επίσης ένα αντικείμενο <code>Dataset</code> απευθείας, παρέχοντάς σας πλήρη έλεγχο του ποίου σύνολο δεδομένων θα επεξεργαστεί.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ρυθμιζόμενες Επιλογές Προφίλ Εμπιστευτικότητας">}}

<p>Το πρότυπο DICOM PS 3.15 ορίζει ένα <strong>Βασικό Προφίλ</strong> καθώς και αρκετά προαιρετικά προφίλ που ελέγχουν πώς συγκεκριμένες κατηγορίες δεδομένων διαχειρίζονται κατά την αποανωνυμοποίηση. Μπορείτε να συνδυάσετε αυτές τις επιλογές χρησιμοποιώντας την enum <code>ConfidentialityProfileOptions</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Επιλογή</th>
<th>Περιγραφή</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Το προεπιλεγμένο Βασικό Προφίλ Εμπιστευτικότητας Επίπεδου Εφαρμογής DICOM PS 3.15</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Διατήρηση ασφαλών ιδιωτικών χαρακτηριστικών κατά την αποανωνυμοποίηση</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Διατήρηση των αρχικών DICOM UID (Study, Series, Instance) αμετάβλητων</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Διατήρηση χαρακτηριστικών αναγνώρισης συσκευής (κατασκευαστής, μοντέλο, όνομα σταθμού)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Διατήρηση χαρακτηριστικών αναγνώρισης ιδρύματος (όνομα, διεύθυνση, τμήμα)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Διατήρηση χαρακτηριστικών ασθενούς (ηλικία, φύλο, μέγεθος, βάρος) για ερευνητικούς σκοπούς</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Διατήρηση πλήρων τιμών ημερομηνίας και ώρας για διαχρονικές μελέτες</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Διατήρηση τροποποιημένων ημερομηνιών για διαχρονικές μελέτες</td></tr>
<tr><td><code>CleanDesc</code></td><td>Καθαρισμός κειμενικών περιγραφών που ενδέχεται να περιέχουν πληροφορίες ταυτοποίησης</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Καθαρισμός δομημένου περιεχομένου που ενδέχεται να περιέχει πληροφορίες ταυτοποίησης</td></tr>
<tr><td><code>CleanGraph</code></td><td>Καθαρισμός γραφικών δεδομένων που ενδέχεται να περιέχουν ενσωματωμένες πληροφορίες ασθενούς</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Αποανωνυμοποίηση με συγκεκριμένες επιλογές προφίλ - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Κωδικοί Ενέργειας για Διαχείριση Χαρακτηριστικών">}}

<p>Κάθε χαρακτηριστικό σε ένα προφίλ εμπιστευτικότητας εκχωρείται ένας κωδικός ενέργειας που ορίζει πώς πρέπει να επεξεργαστεί κατά την αποανωνυμοποίηση. Αυτοί ακολουθούν το πρότυπο DICOM PS 3.15 Πίνακας E.1-1a:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Κώδικας</th>
<th>Δράση</th>
<th>Περιγραφή</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Αντικατάσταση με ψεύτικη</td><td>Αντικατάσταση με τιμή μη μηδενικού μήκους σύμφωνη με το VR</td></tr>
<tr><td><strong>Z</strong></td><td>Μηδενικού μήκους ή ψεύτικη</td><td>Αντικατάσταση με τιμή μηδενικού μήκους ή ψεύτικη τιμή σύμφωνη με το VR</td></tr>
<tr><td><strong>X</strong></td><td>Αφαίρεση</td><td>Αφαίρεση του χαρακτηριστικού πλήρως από το σύνολο δεδομένων</td></tr>
<tr><td><strong>K</strong></td><td>Διατήρηση</td><td>Διατήρηση του χαρακτηριστικού αμετάβλητου (για μη-ακολουθίες) ή καθαρισμένο (για ακολουθίες)</td></tr>
<tr><td><strong>C</strong></td><td>Καθαρισμός</td><td>Αντικατάσταση με καθαρισμένη τιμή παρόμοιας σημασίας σύμφωνη με το VR</td></tr>
<tr><td><strong>U</strong></td><td>Αντικατάσταση UID</td><td>Αντικατάσταση με νέο UID που είναι εσωτερικά συνεπές εντός του συνόλου των περιπτώσεων</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Προσαρμοσμένα Προφίλ από CSV, JSON και XML">}}

<p>Για προχωρημένες περιπτώσεις χρήσης, μπορείτε να ορίσετε προσαρμοσμένα προφίλ αποανωνυμοποίησης φορτώνοντας κανόνες από εξωτερικά αρχεία CSV, JSON ή XML. Αυτό σας επιτρέπει να προσαρμόσετε τη συμπεριφορά αποανωνυμοποίησης σύμφωνα με τις συγκεκριμένες οργανωτικές πολιτικές ή τις κανονιστικές απαιτήσεις σας:</p>

<div class="codeblock" id="code">
 <h3>Φόρτωση προφίλ αποανωνυμοποίησης από CSV - C#</h3>
 <pre><code class="cs">// CSV format: TagPattern;Action
// 0010,0010;Z   (Anonymize PatientName)
// 0010,0020;D   (Replace PatientID with dummy)
// 0020,000D;U   (Replace StudyInstanceUID)

var profile = ConfidentialityProfile.LoadFromCsvFile(
    "anonymization_rules.csv",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Φόρτωση προφίλ αποανωνυμοποίησης από JSON - C#</h3>
 <pre><code class="cs">// JSON format:
// [
//     { "Tag": "0010,0010", "Action": "Z" },
//     { "Tag": "0010,0020", "Action": "D" },
//     { "Tag": "0020,000D", "Action": "U" }
// ]

var profile = ConfidentialityProfile.LoadFromJsonFile(
    "anonymization_rules.json",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<p>Τα προσαρμοσμένα προφίλ μπορούν επίσης να φορτωθούν από αρχεία XML (<code>LoadFromXmlFile</code>), ροές (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>), ή απευθείας από συμβολοσειρές (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Συμμόρφωση με HIPAA και GDPR">}}

<p>Τα αρχεία DICOM περιέχουν συχνά Πληροφορίες Προστατευμένης Υγείας (PHI) όπως ορίζεται από το HIPAA και προσωπικά δεδομένα όπως ορίζεται από το GDPR. Το Aspose.Medical for .NET σας βοηθά να πληροίτε τις κανονιστικές απαιτήσεις παρέχοντας:</p>

<ul>
<li><strong>Συμμόρφωση με DICOM PS 3.15</strong>: Η μηχανή αποανωνυμοποίησης ακολουθεί το επίσημο πρότυπο DICOM για την αποπροσωποποίηση, εξασφαλίζοντας ότι εφαρμόζονται οι αναγνωρισμένες βέλτιστες πρακτικές σε όλα τα σχετικά χαρακτηριστικά.</li>
<li><strong>Ολοκληρωμένη αφαίρεση PII</strong>: Το όνομα του ασθενούς, το ID, η ημερομηνία γέννησης, η διεύθυνση και άλλα αναγνωριστικά διαχειρίζονται σύμφωνα με το ρυθμιζόμενο προφίλ εμπιστευτικότητας.</li>
<li><strong>Αντικατάσταση UID</strong>: Τα UID Μελέτης, Σειράς και SOP Instance μπορούν να αντικατασταθούν με νέα, εσωτερικά συνεπή UID ώστε να αποτραπεί η επαναταυτοποίηση μέσω διασταυρούμενων αναφορών.</li>
<li><strong>Ρυθμιζόμενη διατήρηση</strong>: Όταν οι ερευνητικές ή κλινικές ανάγκες το απαιτούν, συγκεκριμένες κατηγορίες δεδομένων (χαρακτηριστικά ασθενούς, ημερομηνίες, πληροφορίες συσκευής) μπορούν να διατηρηθούν επιλεκτικά χρησιμοποιώντας τα προφίλ επιλογών του προτύπου.</li>
<li><strong>Διαδικασία με δυνατότητα ελέγχου</strong>: Η προσέγγιση βασισμένη σε πρότυπα με καθορισμένους κωδικούς ενέργειας παρέχει μια σαφή, τεκμηριώσιμη διαδικασία αποανωνυμοποίησης για κανονιστικούς ελέγχους.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι Μάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Κώδικας Πηγής" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
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
