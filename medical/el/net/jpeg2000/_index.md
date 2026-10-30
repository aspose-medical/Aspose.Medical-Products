---
title: Συμπίεση DICOM JPEG 2000 σε C# .NET | Aspose.Medical
weight: 2000
description: Ανάγνωση, εγγραφή και μετακωδικοποίηση αρχείων DICOM με συμπίεση JPEG 2000 σε C# .NET. Υποστήριξη για μονόχρωμες και έγχρωμες εικόνες από 1 έως 16 bit, λειτουργίες lossless και lossy, καθώς και HTJ2K με το API του Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Υποστήριξη DICOM JPEG 2000 σε .NET C#" h2="Ανάγνωση, εγγραφή και μετακωδικοποίηση αρχείων DICOM με συμπίεση JPEG 2000. Λειτουργίες lossless και lossy, μονόχρωμα και έγχρωμα δεδομένα pixel από 1 έως 16 bit, HTJ2K συμπεριλαμβανόμενο – όλα σε καθαρό .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 στην Ιατρική Απεικόνιση">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) είναι το πιο ευρέως χρησιμοποιούμενο πρότυπο συμπίεσης βάσει κυματιδίου στην ιατρική απεικόνιση. Σε αντίθεση με το παραδοσιακό JPEG, προσφέρει τόσο συμπίεση lossless όσο και lossy σε ένα μόνο codec, προοδευτική αποκωδικοποίηση για πρόσβαση σε περιοχή ενδιαφέροντος, και άριστους λόγους συμπίεσης &mdash; καθιστώντας το ιδανικό για αρχειοθέτηση μεγάλων μελετών και μετάδοση εικόνων μέσω περιορισμένων δικτύων.</p>

<p><strong>Aspose.Medical for .NET</strong> παρέχει μια καθαρή υλοποίηση C# του codec JPEG 2000 χωρίς εγγενείς εξαρτήσεις. Η βιβλιοθήκη μπορεί να διαβάζει, να αποδίδει και να μετακωδικοποιεί αρχεία DICOM συμπιεσμένα με τις δύο συντακτικές μορφές μεταφοράς JPEG 2000 Part 1, lossless και lossy.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Υποστηριζόμενες Συντακτικές Μορφές Μεταφοράς JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Σύνταξη Μεταφοράς</th>
<th>UID</th>
<th>Λειτουργία</th>
<th>Ανάγνωση</th>
<th>Εγγραφή</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 μόνο Lossless</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>Μονόχρωμο και έγχρωμο, 1 έως 16 bit</td><td>Μονόχρωμο, χρωματική παλέτα και RGB, 1 έως 16 bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy ή lossless</td><td>Μονόχρωμο και έγχρωμο, 1 έως 16 bit</td><td>Μονόχρωμο και RGB, 1 έως 16 bit</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component μόνο Lossless</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Μη υποστηριζόμενο</td><td>Μη υποστηριζόμενο</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy ή lossless</td><td>Μη υποστηριζόμενο</td><td>Μη υποστηριζόμενο</td></tr>
<tr><td>HTJ2K μόνο Lossless</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Μονόχρωμο και έγχρωμο</td><td>Μονόχρωμο και έγχρωμο</td></tr>
<tr><td>HTJ2K με επιλογές RPCL μόνο Lossless</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Μονόχρωμο και έγχρωμο</td><td>Μονόχρωμο και έγχρωμο</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy ή lossless</td><td>Μονόχρωμο και έγχρωμο</td><td>Μονόχρωμο και έγχρωμο</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Δεδομένα Pixel από 1 έως 16 Bit">}}

<p>Οι ιατρικές εικόνες συχνά χρησιμοποιούν 16 bit ανά δειγματοληψία για να καταγράψουν το πλήρες δυναμικό φάσμα των modalities όπως CT (συνήθως 12-bit αποθηκευμένα σε 16-bit) και MRI. Το Aspose.Medical διαχειρίζεται κάθε βάθος από 1 έως 16 bit για JPEG 2000, σε δείγματα 8-bit ή 16-bit:</p>

<ul>
<li><strong>Ανάγνωση (αποσυμπίεση)</strong>: μονόχρωμα αρχεία (CT, MRI, X-ray), αρχεία χρωματικής παλέτας και τριακοχρωματικά αρχεία (RGB, YBR_RCT, YBR_ICT, και άλλα χρωματικά labels όπως YBR_FULL, decoded as coded) με 1 έως 16 bit ανά δείγμα, υπογεγραμμένα ή μη υπογεγραμμένα. Τα CMYK, ICC‑profile, παλέτα JP2 και οι ροές χρωματικού κώδικα υποδειγματοληψίας απορρίπτονται με σαφή εξαίρεση αντί για σιωπηρή λανθασμένη εικόνα.</li>
<li><strong>Εγγραφή (συμπίεση)</strong>: μονόχρωμες και εικόνες RGB με 1 έως 16 bit ανά δείγμα, υπογεγραμμένες ή μη υπογεγραμμένες, και εικόνες χρωματικής παλέτας με JPEG 2000 Lossless. Οι εικόνες YBR δεν κωδικοποιούνται εκτός αν αποκωδικοποιηθούν σε RGB, όπως συμβαίνει με τα αρχεία JPEG YBR lossy.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Ανάγνωση και επιθεώρηση DICOM συμπιεσμένων με JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Μετακωδικοποίηση σε JPEG 2000">}}

<p>Χρησιμοποιήστε τη μέθοδο <code>Transcode</code> για να συμπιέσετε οποιοδήποτε αρχείο DICOM σε JPEG 2000 ή για να μετατρέψετε μεταξύ των λειτουργιών JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Συμπίεση DICOM σε JPEG 2000 Lossless - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Συμπίεση DICOM σε JPEG 2000 Lossy - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αποσυμπίεση αρχείων DICOM JPEG 2000">}}

<p>Αποσυμπιέστε τα αρχεία JPEG 2000 σε μια μη συμπιεσμένη σύνταξη μεταφοράς για επεξεργασία, ανάλυση ή συμβατότητα με συστήματα που δεν υποστηρίζουν JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Αποσυμπίεση JPEG 2000 σε μη συμπιεσμένο - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Μπορείτε επίσης να αποσυμπιέσετε και να μετακωδικοποιήσετε σε άλλες μορφές συμπίεσης σε ένα μόνο βήμα:</p>

<div class="codeblock" id="code">
 <h3>Μετακωδικοποίηση μεταξύ μορφών συμπίεσης - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Απόδοση εικόνων DICOM JPEG 2000">}}

<p>Τα αρχεία DICOM συμπιεσμένα με JPEG 2000 μπορούν να αποδοθούν σε δεδομένα pixel για προβολή ή εξαγωγή, όπως οποιαδήποτε άλλη σύνταξη μεταφοράς:</p>

<div class="codeblock" id="code">
 <h3>Απόδοση ενός JPEG 2000 συμπιεσμένου πλαισίου - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Lossless vs Lossy JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Πτυχή</th>
<th>JPEG 2000 Lossless</th>
<th>JPEG 2000 Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Σύνταξη Μεταφοράς</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Ποιότητα εικόνας</td><td>Ακριβής pixel &mdash; ταυτοτική με το αρχικό</td><td>Οπτικά παρόμοια, κάποια δεδομένα μόνιμα χαμένα</td></tr>
<tr><td>Λόγος συμπίεσης</td><td>Συνήθως 2:1 έως 3:1</td><td>Συνήθως 10:1 έως 30:1 ή περισσότερο</td></tr>
<tr><td>Καλύτερο για</td><td>Διάγνωση, αρχειοθέτηση, νομικά αρχεία, πρωτογενής ανάγνωση</td><td>Προκαταρκτική ανασκόπηση, τηλεϊατρική, μετάδοση δικτύου</td></tr>
<tr><td>Ασφάλεια κυκλικής μετατροπής</td><td>Ναι</td><td>Όχι &mdash; η επανακωδικοποίηση επιδεινώνει περαιτέρω την ποιότητα</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Υψηλής Απόδοσης (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) αντικαθιστά τον αργό αλγορίθμο αριθμητικής κωδικοποίησης του JPEG 2000 με έναν ταχύτερο μπλοκ κωδικοποιητή. Διατηρεί τον ίδιο μετασχηματισμό κυματιδίου, τις σειρές προόδου και την ποιότητα, και αποκωδικοποιεί και κωδικοποιεί αρκετές φορές πιο γρήγορα. Το Aspose.Medical υλοποιεί όλες τις τρεις συντακτικές μορφές μεταφοράς DICOM HTJ2K σε καθαρό .NET, για μονόχρωμες και έγχρωμες εικόνες, και μετακωδικοποιεί μεταξύ HTJ2K και κάθε άλλης υποστηριζόμενης σύνταξης:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; μόνο lossless</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; lossless με σειρά προόδου RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; lossy ή lossless</li>
</ul>

<div class="codeblock" id="code">
 <h3>Μετακωδικοποίηση JPEG 2000 σε HTJ2K και αντίστροφα - C#</h3>
 <pre><code class="cs">// Transcode a JPEG 2000 file to HTJ2K, and back to classic JPEG 2000
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile htj2kFile = j2kFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2kFile.Save("htj2k_lossless.dcm");

// HTJ2K with RPCL progression order, lossless
DicomFile rpclFile = j2kFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
rpclFile.Save("htj2k_rpcl.dcm");

// HTJ2K lossy
DicomFile htj2kLossy = j2kFile.Transcode(TransferSyntax.HTJ2K);
htj2kLossy.Save("htj2k_lossy.dcm");

// Any HTJ2K file decodes back to an uncompressed transfer syntax
DicomFile uncompressed = htj2kFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decoded.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι Μάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Πηγαίος Κώδικας" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Αναφορές API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Υποστήριξη Προϊόντος" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Δωρεάν Υποστήριξη" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Πληρωμένη Υποστήριξη" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Ιστολόγιο" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Γιατί το Aspose.Medical για .NET;" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Λίστα Πελατών" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Ιστορίες Επιτυχίας" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
