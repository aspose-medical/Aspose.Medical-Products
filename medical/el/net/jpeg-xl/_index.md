---
title: JPEG XL για DICOM σε C# .NET | Aspose.Medical
weight: 10500

description: Αποθηκεύστε εικόνες DICOM σε JPEG XL από C#. Απώλειας-συμπίεση JPEG XL που επιστρέφει τα pixel bit για bit, σε μία ενιαία διαχειριζόμενη συναρμολόγηση χωρίς καμία γνήσια κωδικοποιητική βιβλιοθήκη για ανάπτυξη.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL για DICOM σε .NET C#" h2="Η πιο πρόσφατη συμπίεση στο πρότυπο DICOM, με τα μικρότερα αρχεία απώλειας-συμπίεσης που μετρήσαμε, υλοποιημένη σε διαχειριζόμενο C# και διανεμημένη μέσα σε μία συναρμολόγηση." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Γιατί το JPEG XL ενσωματώθηκε στο DICOM">}}

<p>Οι ιατρικές αρχειοθήκες αυξάνονται και ποτέ δεν μειώνονται. Το JPEG XL είναι ο κωδικοποιητής που σχεδίασε ο κόσμος της απεικόνισης μετά από δύο δεκαετίες εμπειρίας με JPEG και JPEG 2000, και το DICOM το πρόσθεσε ως σύνταξη μεταφοράς για τον λόγο που ενδιαφέρει τις ομάδες αποθήκευσης: για τα ίδια pixel, το αρχείο είναι μικρότερο.</p>

<p><strong>Aspose.Medical για .NET</strong> γράφει και διαβάζει JPEG XL μέσω μιας θύρας C# του libjxl που βρίσκεται μέσα στη βιβλιοθήκη. Το πακέτο διανέμει μία συναρμολόγηση, <code>Aspose.Medical.dll</code>, και κανένα γνήσιο δυαδικό δίπλα του, έτσι ένας κωδικοποιητής τόσο νέος δεν μετατρέπεται σε έργο ανάπτυξης: η ίδια συναρμολόγηση εκτελείται στα Windows, στο Linux, σε ένα agent κατασκευής και σε ένα container.</p>

<p>Δύο συνταγές μεταφοράς μεταφέρουν τα pixel:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), για διαγνωστικά δεδομένα που πρέπει να επιστραφούν αμετάβλητα.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), για τις περιπτώσεις όπου ένα μικρότερο αρχείο είναι πιο σημαντικό από ένα ακριβές αντίγραφο.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Συμπιέστε μια μελέτη, διατηρήστε κάθε pixel">}}

<p>Η μετακωδικοποίηση είναι μια κλήση, και το σύνολο δεδομένων γύρω από τα pixel ταξιδεύει μαζί του.</p>

<div class="codeblock" id="code">
 <h3>Μετακωδικοποίηση αρχείου DICOM σε JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Το μετρήσαμε σε εικόνα 1714 x 1933 16-bit από το δικό μας σύνολο δοκιμών: τα 6,3 MB χωρίς συμπίεση γίνονται 2,7 MB σε JPEG XL lossless, που είναι μικρότερα από την ίδια εικόνα σε HTJ2K lossless. Τα δικά σας νούμερα εξαρτώνται από τη μονάδα, έτσι εκτελέστε τη σύγκριση σε έναν φάκελο των αρχείων σας πριν αποφασίσετε.</p>

<p>Η λέξη lossless παίρνεται κυριολεκτικά εδώ. Μετακωδικοποιήστε σε JPEG XL και πίσω, και τα δεδομένα pixel ισούνται με τα bytes με τα οποία ξεκινήσατε, έτσι μια αρχειοθήκη μπορεί να επανασυμπιεστεί χωρίς συζήτηση για τη διαγνωστική ποιότητα.</p>

<div class="codeblock" id="code">
 <h3>Πίσω σε σύνταξη χωρίς συμπίεση - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Διαβάστε τι είναι ήδη αποθηκευμένο ως JPEG XL">}}

<p>Ένα αρχείο που φθάνει σε JPEG XL ανοίγει όπως οποιοδήποτε άλλο. Η σύνταξη μεταφοράς δηλώνει τι είναι, και τα δεδομένα pixel είναι διαθέσιμα μόλις αποκωδικοποιηθεί το πλαίσιο.</p>

<div class="codeblock" id="code">
 <h3>Άνοιγμα αρχείου JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL ή HTJ2K">}}

<p>Και τα δύο είναι πρόσφατα, και τα δύο είναι lossless όταν ζητήσετε lossless, και η βιβλιοθήκη γράφει και διαβάζει και τα δύο. Απαντούν σε διαφορετικά ερωτήματα.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Ερώτηση</th>
<th>Απάντηση</th>
</tr>
</thead>
<tbody>
<tr><td>Ποιο παρήγαγε το μικρότερο αρχείο στη δοκιμή μας</td><td>JPEG XL lossless, κατά μερικά ποσοστά</td></tr>
<tr><td>Ποιο έχει σχεδιαστεί για προοδευτική προβολή μέσω δικτύου</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, ειδικά η παραλλαγή RPCL</td></tr>
<tr><td>Ποιο εισήχθη πρώτα στο πρότυπο DICOM</td><td>HTJ2K, έτσι περισσότερες αρχειοθήκες το αποδέχονται σήμερα</td></tr>
<tr><td>Ποιο απαιτεί γνήσια εξάρτηση εδώ</td><td>Κανένα, και τα δύο είναι managed code σε μία συναρμολόγηση</td></tr>
</tbody>
</table>

<p>Η επιλογή συνήθως προέρχεται από την άλλη πλευρά του συνδέσμου: μετακωδικοποιήστε στη σύνταξη που αποδέχεται η αρχειοθήκη, και διατηρήστε το υπόλοιπο της αλυσίδας επεξεργασίας όπως είναι.</p>

<div class="codeblock" id="code">
 <h3>Αφήστε την στοχευμένη αρχειοθήκη να αποφασίσει - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Πού αποδίδει">}}

<ul>
<li>Μακροπρόθεσμες αρχειοθήκες: οι ίδιες μελέτες, λιγότερα terabytes, και καμία απώλεια για να δικαιολογηθεί σε ραδιολόγο.</li>
<li>Τιμολόγια αποθήκευσης στο σύννεφο: η εξοικονόμηση επαναλαμβάνεται κάθε μήνα, ενώ η μετακωδικοποίηση εκτελείται μία φορά.</li>
<li>Σύνολα δεδομένων για έρευνα και AI: μικρότερα αντίγραφα μετακινούνται γρηγορότερα μεταξύ αποθήκευσης και εκπαίδευσης.</li>
<li>Διάθεση: ένας κωδικοποιητής τόσο νέος συνήθως σημαίνει γνήσια κατασκευή ανά πλατφόρμα· εδώ είναι μέρος της συναρμολόγησης που ήδη αναφέρετε.</li>
</ul>

<p>Η βιβλιοθήκη γράφει επίσης τους κωδικοποιητές που περιέχει μια υπάρχουσα αρχειοθήκη: JPEG, JPEG-LS, JPEG 2000, HTJ2K και RLE. Η σελίδα <a href="/medical/net/dicom-transfer-syntax-conversion/">μετατροπής σύνταξης μεταφοράς</a> καλύπτει ολόκληρο το σύνολο, το <a href="/medical/net/htj2k/">HTJ2K</a> έχει τη δική του σελίδα, και το <a href="/medical/net/jpeg2000/">JPEG 2000</a> είναι από όπου προέρχονται και οι δύο νέοι κωδικοποιητές.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι εκμάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Οδηγός προγραμματιστή" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
