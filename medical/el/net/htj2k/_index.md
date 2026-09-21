---
title: HTJ2K σε C# .NET - High-Throughput JPEG 2000 για DICOM | Aspose.Medical
weight: 10000

description: Συμπιέστε και διαβάστε εικόνες DICOM σε High-Throughput JPEG 2000 από C#. Lossless HTJ2K, η παραλλαγή RPCL και lossy HTJ2K, υλοποιημένα σε managed .NET χωρίς native codec για εγκατάσταση.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K σε .NET C#" h2="High-Throughput JPEG 2000 για DICOM: η συμπίεση που πρόσθεσε το πρότυπο για γρήγορα αρχεία και προβολή στο cloud, υλοποιημένη σε managed C# χωρίς τίποτα native για εγκατάσταση." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Τι αλλάζει το HTJ2K">}}

<p>High-Throughput JPEG 2000 διατηρεί τα wavelet και την ποιότητα εικόνας του JPEG 2000 και αντικαθιστά το μέρος που το καθυστερούσε. Ο block coder είναι νέο, και η αποκωδικοποίηση είναι τάξης μεγέθους πολύ πιο γρήγορη, γι' αυτό το πρότυπο DICOM το υιοθέτησε σε τρεις transfer syntaxes και γιατί οι πλατφόρμες cloud imaging το υιοθέτησαν.</p>

<p>Για μια ομάδα .NET η πρακτική ερώτηση είναι διαφορετική: ποιος μπορεί πραγματικά να παράγει αυτά τα αρχεία. Οι περισσότεροι βιβλιοθήκες φθάνουν στο HTJ2K μέσω native OpenJPH build, κάτι που σημαίνει ένα binary ανά πλατφόρμα, ένα βήμα build στο container και μια εξάρτηση που η αξιολόγηση ασφαλείας θα ερευνήσει. <strong>Aspose.Medical for .NET</strong> υλοποιεί τον codec σε managed κώδικα μέσα στο ίδιο πακέτο που διαβάζει και γράφει τα αρχεία, έτσι το HTJ2K λειτουργεί το ίδιο σε Windows, σε Linux και σε container, χωρίς τίποτα για εγκατάσταση.</p>

<p>Υποστηρίζονται τρία transfer syntaxes, και και τα τρία μπορούν να διαβάζουν και να γράφουν:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), High-Throughput JPEG 2000 χωρίς απώλεια.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), η παραλλαγή χωρίς απώλεια με την σειρά προόδου RPCL.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Συμπιέστε μια μελέτη σε HTJ2K">}}

<p>Μία κλήση μεταφέρει ένα αρχείο στη νέα σύνταξη. Το dataset, τα ιδιωτικά tags και οι μεταπληροφορίες του αρχείου ταξιδεύουν μαζί του.</p>

<div class="codeblock" id="code">
 <h3>Μετακωδικοποίηση αρχείου DICOM σε HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>Σε εικόνα 1714x1933 16-bit από το δικό μας σύνολο δοκιμών, το αρχείο μειώνεται από 6,3 MB σε 2,9 MB, και τα pixel επιστρέφουν bit προς bit. Οι αριθμοί διαφέρουν ανά λογική και ανά εικόνα, γι’ αυτό μετρήστε στα δικά σας δεδομένα, που σημαίνει ένας βρόχος πάνω στα αρχεία που ήδη έχετε.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossless σημαίνει χωρίς απώλεια">}}

<p>Τα διαγνωστικά δεδομένα δεν ανεχτούν έναν codec που είναι σχεδόν σωστός. Μετακωδικοποιήστε σε HTJ2K χωρίς απώλεια και επιστρέψτε, και τα pixel δεδομένα είναι ταυτόσημα με τα bytes που ξεκινήσατε, μια ιδιότητα που μπορείτε να επαληθεύσετε στη δική σας σειρά δοκιμών πριν συμφωνήσετε να επανασυμπιέσετε ένα αρχείο.</p>

<div class="codeblock" id="code">
 <h3>Επιστροφή σε μη συμπιεσμένη σύνταξη - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, η παραλλαγή που δημιουργήθηκε για προβολή μέσω δικτύου">}}

<p>Η σύνταξη 1.2.840.10008.1.2.4.202 αποθηκεύει το ίδιο lossless codestream με τη σειρά προόδου RPCL: ανάλυση πρώτα, μετά θέση, μετά συστατικό, μετά στρώμα. Ένας αναγνώστης που παίρνει μόνο την αρχή του ροής λαμβάνει μια πλήρη εικόνα χαμηλής ανάλυσης, κάτι που χρειάζεται ένας viewer όταν ανοίγει μια μεγάλη μελέτη μέσω συνδέσμου που δεν ελέγχει.</p>

<div class="codeblock" id="code">
 <h3>Συμπίεση με τη σειρά προόδου RPCL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Διαβάστε τι στέλνει ένα αρχείο">}}

<p>Το άλλο μισό της εργασίας είναι η αποδοχή HTJ2K από συστήματα που ήδη το παράγουν. Ανοίξτε το αρχείο, ελέγξτε πώς είναι αποθηκευμένο, και εργαστείτε με τα pixel δεδομένα.</p>

<div class="codeblock" id="code">
 <h3>Διαβάστε ένα αρχείο HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Οι εικόνες multi-frame επεξεργάζονται καρέ-καρέ, έτσι μια μακριά σειρά καταναλώνει μνήμη ανά καρέ αντί για ανά μελέτη.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Πού το HTJ2K κερδίζει τη θέση του">}}

<ul>
<li>Μεταφορά αρχείων: επανασυμπίεση μιας αποθηκευμένης μελέτης σε HTJ2K χωρίς απώλεια, μείωση του αποτυπώματος, διατήρηση των διαγνωστικών δεδομένων άθικτα.</li>
<li>Cloud και DICOMweb: η ταχύτητα αποκωδικοποίησης είναι αυτό που κάνει έναν viewer στο πρόγραμμα περιήγησης ή στο διακομιστή να φαίνεται άμεσο σε μεγάλες εικόνες.</li>
<li>Διαδρομές AI: τα σύνολα εκπαίδευσης διαβάζονται πολύ πιο συχνά από ό,τι γράφονται, και ο χρόνος αποκωδικοποίησης είναι το κόστος που επαναλαμβάνεται.</li>
<li>Containers και serverless: ο codec είναι μέρος του assembly, έτσι μια εικόνα δεν χρειάζεται native βιβλιοθήκη ή compiler στη διαδικασία build.</li>
</ul>

<p>Η βιβλιοθήκη παρέχει επίσης JPEG XL, την άλλη πρόσφατη προσθήκη στο πρότυπο, καθώς και τους παλαιότερους codecs που πιθανόν να περιέχει ένα αρχείο: JPEG, JPEG-LS, JPEG 2000 και RLE. Η σελίδα <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> καλύπτει όλο το σύνολο, και η σελίδα <a href="/medical/net/jpeg2000/">JPEG 2000</a> καλύπτει τον codec από τον οποίο προέκυψε το HTJ2K.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι Εκμάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Οδηγός Προγραμματιστών" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
