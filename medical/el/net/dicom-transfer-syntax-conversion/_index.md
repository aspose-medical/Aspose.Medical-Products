---
title: Μετατροπή Συντακτικού Μεταφοράς DICOM σε C# .NET | Aspose.Medical
weight: 16000
description: Κωδικοποίηση DICOM αρχείων μεταξύ συντακτικών μεταφοράς σε C# .NET. Υποστήριξη για JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, RLE και ασυμπίεστη μορφές με το API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Μετατροπή Συντακτικού Μεταφοράς DICOM σε .NET C#" h2="Κωδικοποίηση DICOM αρχείων μεταξύ ασυμπίεστων, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS και RLE συντακτικών μεταφοράς. Καθαρή .NET βιβλιοθήκη χωρίς εγγενείς εξαρτήσεις." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Τι είναι το Συντακτικό Μεταφοράς;">}}

<p>Ένα <strong>Transfer Syntax</strong> ορίζει πώς κωδικοποιούνται τα δεδομένα DICOM για αποθήκευση και μετάδοση. Καθορίζει τρία βασικά στοιχεία: τη διάταξη των bytes (endianness), εάν τα Value Representations είναι ρητά ή έμμεσα, και τον αλγόριθμο συμπίεσης που εφαρμόζεται στα δεδομένα εικονοστοιχείων. Κάθε αρχείο DICOM δηλώνει το συντακτικό μεταφοράς του στην κεφαλίδα File Meta Information.</p>

<p>Διάφορες ιατρικές συσκευές, διακομιστές PACS και εφαρμογές προβολής υποστηρίζουν διαφορετικά σύνολα συντακτικών μεταφοράς. <strong>Aspose.Medical for .NET</strong> παρέχει τη μέθοδο <code>Transcode</code> για μετατροπή μεταξύ συντακτικών μεταφοράς, επιτρέποντας τη διαλειτουργικότητα, τη βελτιστοποίηση αποθήκευσης και τη συμβατότητα με εργαλεία επεξεργασίας &mdash; όλα σε μια καθαρή .NET βιβλιοθήκη χωρίς εγγενείς εξαρτήσεις.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Κωδικοποίηση DICOM αρχείου σε C#">}}

<p>Η μέθοδος <code>DicomFile.Transcode</code> μετατρέπει ένα αρχείο DICOM από το τρέχον συντακτικό μεταφοράς του σε οποιοδήποτε υποστηριζόμενο συντακτικό-στόχο. Η μέθοδος επιστρέφει ένα νέο αντικείμενο <code>DicomFile</code> &mdash; το αρχικό παραμένει αμετάβλητο:</p>

<div class="codeblock" id="code">
 <h3>Βασική κωδικοποίηση DICOM - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Μπορείτε επίσης να κωδικοποιήσετε άμεσα σε επίπεδο <code>Dataset</code>:</p>

<div class="codeblock" id="code">
 <h3>Κωδικοποίηση Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Υποστηριζόμενα Συντακτικά Μεταφοράς">}}

<p>Ο παρακάτω πίνακας παραθέτει όλα τα τυπικά DICOM συντακτικά μεταφοράς δεδομένων εικόνας και την τρέχουσα κατάσταση υποστήριξής τους στο Aspose.Medical for .NET. Όλοι οι υποστηριζόμενοι κωδικοποιητές υλοποιούνται σε καθαρό C# και είναι πλήρως ανεξάρτητοι από την πλατφόρμα.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Συντακτικό Μεταφοράς</th>
<th>UID</th>
<th>Τύπος</th>
<th>Κατάσταση</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Ασυμπίεστο</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Ασυμπίεστο</td><td>Υποστηρίζεται</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Ασυμπίεστο</td><td>Υποστηρίζεται</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Ασυμπίεστο (παρωχημένο)</td><td>Υποστηρίζεται</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Ασυμπίεστο</td><td>Δεν υποστηρίζεται</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Υποστηρίζεται</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Με απώλειες, 8-bit</td><td>Υποστηρίζεται</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Με απώλειες, 12-bit</td><td>Δεν υποστηρίζεται</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Χωρίς απώλειες</td><td>Υποστηρίζεται (μόνο 8-bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Χωρίς απώλειες</td><td>Υποστηρίζεται (μόνο 8-bit)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Χωρίς απώλειες</td><td>Υποστηρίζεται</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Σχεδόν χωρίς απώλειες</td><td>Υποστηρίζεται</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Χωρίς απώλειες</td><td>Υποστηρίζεται (ανάγνωση χρώματος 8-bit και μονοχρωματικού 16-bit· εγγραφή 16-bit μονοχρωματικού ή 8-bit RGB)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Με απώλειες ή χωρίς απώλειες</td><td>Υποστηρίζεται (ανάγνωση χρώματος 8-bit και μονοχρωματικού 16-bit· εγγραφή 16-bit μονοχρωματικού ή 8-bit RGB)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Χωρίς απώλειες</td><td>Δεν υποστηρίζεται</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Με απώλειες ή χωρίς απώλειες</td><td>Δεν υποστηρίζεται</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Χωρίς απώλειες</td><td>Υποστηρίζεται</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Χωρίς απώλειες</td><td>Υποστηρίζεται</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Χωρίς απώλειες</td><td>Υποστηρίζεται</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Με απώλειες ή χωρίς απώλειες</td><td>Υποστηρίζεται</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Χωρίς απώλειες</td><td>Υποστηρίζεται</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Χωρίς απώλειες</td><td>Μόνο αποκωδικοποίηση (η κωδικοποίηση απαιτεί ροή πηγής JPEG, όχι δεδομένα εικονοστοιχείων)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Με απώλειες ή χωρίς απώλειες</td><td>Υποστηρίζεται (λειτουργία με απώλειες)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Συνηθισμένα Σενάρια Κωδικοποίησης">}}

<p>Διαφορετικές ροές εργασίας απαιτούν διαφορετικές στρατηγικές κωδικοποίησης. Ακολουθούν τα πιο συνηθισμένα σενάρια:</p>

<div class="codeblock" id="code">
 <h3>Αποσυμπίεση για επεξεργασία - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Συμπίεση για αρχειοθέτηση - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Συμπίεση για μετάδοση μέσω δικτύου - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Χρήση των πιο πρόσφατων κωδικοποιητών: HTJ2K και JPEG XL - C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Επιθεώρηση Ιδιοτήτων Συντακτικού Μεταφοράς">}}

<p>Η κλάση <code>TransferSyntax</code> εκθέτει ιδιότητες που περιγράφουν τα χαρακτηριστικά κωδικοποίησης. Χρησιμοποιήστε τις για να ελέγξετε το τρέχον συντακτικό μεταφοράς ενός αρχείου ή να επιλέξετε κατάλληλο συντακτικό-στόχο:</p>

<div class="codeblock" id="code">
 <h3>Ανάγνωση ιδιοτήτων συντακτικού μεταφοράς - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>Ιδιότητα</th>
<th>Τύπος</th>
<th>Περιγραφή</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>Το μοναδικό αναγνωριστικό του συντακτικού μεταφοράς</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Εάν τα Value Representations κωδικοποιούνται ρητά</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Εάν η διάταξη των bytes είναι little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Εάν τα δεδομένα εικονοστοιχείων είναι ενσωματωμένα (συμπιεσμένα)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Εάν η μέθοδος συμπίεσης είναι με απώλειες</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Εάν το συντακτικό χρησιμοποιεί συμπίεση deflate</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Εάν το συντακτικό μεταφοράς έχει αποσυρθεί από το πρότυπο DICOM</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>Το αναγνωριστικό ISO του τρόπου συμπίεσης με απώλειες</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Συμπίεση με Απώλειες vs Χωρίς Απώλειες">}}

<p>Η κατανόηση της διαφοράς μεταξύ συμπίεσης με απώλειες και χωρίς απώλειες είναι κρίσιμη όταν κωδικοποιείτε αρχεία DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Πτυχή</th>
<th>Χωρίς Απώλειες</th>
<th>Με Απώλειες</th>
</tr>
</thead>
<tbody>
<tr><td>Ποιότητα εικόνας</td><td>Ακριβής εικονοστοιχείο — τα αρχικά δεδομένα διατηρούνται πλήρως</td><td>Ορισμένα δεδομένα χάνονται μόνιμα για να επιτευχθεί μικρότερο μέγεθος</td></tr>
<tr><td>Λόγος συμπίεσης</td><td>Συνήθως 2:1 έως 3:1</td><td>Συνήθως 10:1 έως 30:1 ή υψηλότερος</td></tr>
<tr><td>Ασφάλεια επαναφοράς</td><td>Ναι — αποσυμπίεση και διατήρηση ταυτόσημων εικονοστοιχείων</td><td>Όχι — κάθε επανακωδικοποίηση με απώλειες υποβαθμίζει περαιτέρω την ποιότητα</td></tr>
<tr><td>Περιστατικά χρήσης</td><td>Αρχειοθέτηση, διαγνωστικά, νομικά αρχεία</td><td>Προκαταρκτική αξιολόγηση, τηλεϊατρική, μετάδοση μέσω δικτύου</td></tr>
<tr><td>Υποστηριζόμενοι κωδικοποιητές</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>Σημαντικό:</strong> Η κωδικοποίηση από αρχείο συμπιεσμένο με απώλειες σε συντακτικό χωρίς απώλειες δεν αποκαθιστά τα χαμένα δεδομένα. Η υποβάθμιση ποιότητας από την αρχική συμπίεση με απώλειες είναι μόνιμη.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πηγές Μάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Πηγαίος Κώδικας" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
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
