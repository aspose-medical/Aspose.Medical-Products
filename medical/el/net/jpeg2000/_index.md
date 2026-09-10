---
title: Συμπίεση DICOM JPEG 2000 σε C# .NET | Aspose.Medical
weight: 2000
description: Ανάγνωση, εγγραφή και διακωδικοποίηση αρχείων DICOM με συμπίεση JPEG 2000 σε C# .NET. Υποστήριξη για εικόνες 8-bit και 16-bit, λειτουργίες χωρίς απώλειες και με απώλειες, πολυ-στοιχείων δεδομένων με το API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Υποστήριξη DICOM JPEG 2000 σε .NET C#" h2="Ανάγνωση, εγγραφή και διακωδικοποίηση αρχείων DICOM με συμπίεση JPEG 2000. Λειτουργίες χωρίς απώλειες και με απώλειες, δεδομένα pixel 8-bit και 16-bit, εικόνες πολυ-στοιχείων — όλα σε καθαρό .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 στην Ιατρική Απεικόνιση">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) είναι το πιο ευρέως χρησιμοποιούμενο πρότυπο συμπίεσης βασισμένο σε κυματισμούς στην ιατρική απεικόνιση. Σε αντίθεση με το παραδοσιακό JPEG, προσφέρει τόσο συμπίεση χωρίς απώλειες όσο και με απώλειες σε ένα ενιαίο codec, προοδευτική αποκωδικοποίηση για πρόσβαση σε περιοχές ενδιαφέροντος, και ανώτερες αναλογίες συμπίεσης &mdash; καθιστώντας το ιδανικό για αρχειοθέτηση μεγάλων μελετών και μετάδοση εικόνων μέσω περιορισμένων δικτύων.</p>

<p><strong>Aspose.Medical for .NET</strong> παρέχει μια καθαρή υλοποίηση C# του codec JPEG 2000 χωρίς εγγενείς εξαρτήσεις. Η βιβλιοθήκη μπορεί να διαβάζει, αποδίδει και διακωδικοποιεί αρχεία DICOM συμπιεσμένα με οποιαδήποτε από τις τέσσερις τυπικές συντακτικές μορφές μεταφοράς JPEG 2000.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Υποστηριζόμενες Συντακτικές Μορφές Μεταφοράς JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Συντακτική Μορφή Μεταφοράς</th>
<th>UID</th>
<th>Λειτουργία</th>
<th>Ανάγνωση</th>
<th>Εγγραφή</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 μόνο χωρίς απώλειες</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Χωρίς απώλειες</td><td>8-bit και 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Με ή χωρίς απώλειες</td><td>8-bit και 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 Part 2 Πολυ‑Συστατικό μόνο χωρίς απώλειες</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Χωρίς απώλειες</td><td>8-bit και 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 Part 2 Πολυ‑Συστατικό</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Με ή χωρίς απώλειες</td><td>8-bit και 16-bit</td><td>8-bit</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Δεδομένα Pixel 8-bit και 16-bit">}}

<p>Οι ιατρικές εικόνες συχνά χρησιμοποιούν 16 bit ανά δείγμα για την καταγραφή του πλήρους δυναμικού εύρους των μεθόδων όπως CT (συνήθως 12-bit αποθηκευμένα σε 16-bit) και MRI. Το Aspose.Medical διαχειρίζεται και τις δύο βάθους bit για JPEG 2000:</p>

<ul>
<li><strong>Ανάγνωση (αποσυμπίεση)</strong>: Πλήρης υποστήριξη για αρχεία DICOM συμπιεσμένα με JPEG 2000 8-bit και 16-bit. Η βιβλιοθήκη αποκωδικοποιεί σωστά τα δεδομένα pixel ανεξαρτήτως των αρχικών τιμών Bits Allocated, Bits Stored και High Bit.</li>
<li><strong>Εγγραφή (συμπίεση)</strong>: Προς το παρόν υποστηρίζει εικόνες 8-bit. Η υποστήριξη εγγραφής 16-bit προγραμματίζεται για μελλοντική έκδοση.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Ανάγνωση και επιθεώρηση DICOM με συμπίεση JPEG 2000 - C#</h3>
 <pre><code class="cs">// Open a JPEG 2000 compressed DICOM file (8-bit or 16-bit)
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Check transfer syntax
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax
Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");

// Inspect pixel data bit depth
PixelData pixelData = PixelData.Create(dicomFile.Dataset);
Console.WriteLine($"Bits Allocated: {pixelData.BitsAllocated}");
Console.WriteLine($"Bits Stored: {pixelData.BitsStored}");
Console.WriteLine($"High Bit: {pixelData.HighBit}");
Console.WriteLine($"Samples Per Pixel: {pixelData.SamplesPerPixel}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Διακωδικοποίηση σε JPEG 2000">}}

<p>Χρησιμοποιήστε τη μέθοδο <code>Transcode</code> για να συμπιέσετε οποιοδήποτε αρχείο DICOM σε JPEG 2000 ή για να μετατρέψετε μεταξύ λειτουργιών JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Συμπίεση DICOM σε JPEG 2000 χωρίς απώλειες - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Συμπίεση DICOM σε JPEG 2000 με απώλειες - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αποσυμπίεση αρχείων DICOM JPEG 2000">}}

<p>Αποσυμπίεση αρχείων JPEG 2000 σε ανυπόστατη (uncompressed) συντακτική μορφή μεταφοράς για επεξεργασία, ανάλυση ή συμβατότητα με συστήματα που δεν υποστηρίζουν JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Αποσυμπίεση JPEG 2000 σε ανυπόστατη μορφή - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Μπορείτε επίσης να αποσυμπιέσετε και να διακωδικοποιήσετε σε άλλες μορφές συμπίεσης σε ένα μόνο βήμα:</p>

<div class="codeblock" id="code">
 <h3>Διακωδικοποίηση μεταξύ μορφών συμπίεσης - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Απόδοση εικόνων DICOM JPEG 2000">}}

<p>Τα αρχεία DICOM συμπιεσμένα με JPEG 2000 μπορούν να αποδοθούν σε δεδομένα pixel για προβολή ή εξαγωγή, όπως κάθε άλλη συντακτική μορφή μεταφοράς:</p>

<div class="codeblock" id="code">
 <h3>Απόδοση ενός καρέ συμπιεσμένου με JPEG 2000 - C#</h3>
 <pre><code class="cs">// Open JPEG 2000 DICOM file
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Render the first frame — decompression is handled automatically
using PixelImage&lt;Bgra32&gt; image = dicomFile.RenderImage(0);

// Copy the BGRA32 pixel data for display or export
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = new Bgra32[width * height];
image.CopyPixelsTo(pixels);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Χωρίς απώλειες vs Με απώλειες JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Πτυχή</th>
<th>JPEG 2000 χωρίς απώλειες</th>
<th>JPEG 2000 με απώλειες</th>
</tr>
</thead>
<tbody>
<tr><td>Συντακτική Μορφή</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Ποιότητα εικόνας</td><td>Ακριβής pixel &mdash; ταυτόσημη με την αρχική</td><td>Οπτικά παρόμοια, κάποια δεδομένα χάνονται μόνιμα</td></tr>
<tr><td>Αναλογία συμπίεσης</td><td>Συνήθως 2:1 έως 3:1</td><td>Συνήθως 10:1 έως 30:1 ή περισσότερο</td></tr>
<tr><td>Καλύτερο για</td><td>Διαγνωστική αρχειοθέτηση, νομικά αρχεία, πρωτεύουσα ανάγνωση</td><td>Προκαταρκτική αξιολόγηση, τηλεϊατρική, μετάδοση μέσω δικτύου</td></tr>
<tr><td>Ασφαλές σε κυκλική διαδρομή</td><td>Ναι</td><td>Όχι &mdash; η επανακωδικοποίηση επιδεινώνει περαιτέρω την ποιότητα</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 Πολυ‑Συστατικό">}}

<p>Το JPEG 2000 Part 2 (ISO/IEC 15444-2) επεκτείνει το τυπικό codec με δυνατότητες μετασχηματισμού πολυ‑συστατικών. Αυτό χρησιμοποιείται για χρωματιστές ιατρικές εικόνες και μεθόδους που παράγουν δεδομένα πολλαπλών καναλιών. Το Aspose.Medical υποστηρίζει και τις δύο συντακτικές μορφές μεταφοράς Part 2:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; συμπίεση χωρίς απώλειες με αφαίρεση διασυστατικού συσχέτισης για βέλτιστη συμπίεση δεδομένων πολλαπλών καναλιών.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; συμπίεση με ή χωρίς απώλειες με μετασχηματισμούς πολυ‑συστατικού.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K) — Έρχεται Σύντομα">}}

<p>Το HTJ2K (ISO/IEC 15444-15) είναι μια επόμενης γενιάς επέκταση του JPEG 2000 σχεδιασμένη για δραματικά ταχύτερη κωδικοποίηση και αποκωδικοποίηση, διατηρώντας την ίδια απόδοση συμπίεσης. Αναμένεται να γίνει το προτιμώμενο codec για ροές εργασίας ιατρικής απεικόνισης σε πραγματικό χρόνο.</p>

<p>Το Aspose.Medical θα προσθέσει υποστήριξη HTJ2K σε μελλοντική έκδοση, καλύπτοντας τρεις συντακτικές μορφές μεταφοράς:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; Μόνο χωρίς απώλειες</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; Χωρίς απώλειες με σειρά προόδου RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; Με ή χωρίς απώλειες</li>
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
