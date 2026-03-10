---
title: Μετατροπή DICOM σε PNG σε C# .NET | Aspose.Medical
weight: 11000
description: Απόδοση εικόνων DICOM σε δεδομένα εικονοστοιχείων σε C# .NET με απώλειας ποιότητα, έλεγχο παραθύρου/επιπέδου και υποστήριξη πολλαπλών καρέ. Χρησιμοποιήστε ολόκληρη τη ροή LUT DICOM με το Aspose.Medical API και αποθηκεύστε σε PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Μετατροπή DICOM σε PNG σε .NET C#" h2="Απόδοση εικόνων DICOM σε ακατέργαστα δεδομένα εικονοστοιχείων με πλήρη υποστήριξη ροής γκρι κλίμακας. Διατηρήστε την ποιότητα της εικόνας με έλεγχο παραθύρου/επιπέδου, απόδοση επικάλυψης και επεξεργασία πολλαπλών καρέ. Αποθηκεύστε σε PNG χωρίς απώλειες χρησιμοποιώντας οποιαδήποτε βιβλιοθήκη .NET για επεξεργασία εικόνας." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Μετατροπή DICOM σε PNG">}}

<p><strong>Aspose.Medical for .NET</strong> αποδίδει εικόνες DICOM μέσω της τυπικής ροής γκρι κλίμακας DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT) για την παραγωγή σωστά παραθυρισμένων δεδομένων εικονοστοιχείων BGRA32. PNG είναι μορφή χωρίς απώλειες, καθιστώντας το προτιμώμενη επιλογή όταν η ποιότητα της εικόνας πρέπει να διατηρηθεί χωρίς παραμορφώσεις συμπίεσης &mdash; απαραίτητο για διαγνωστική ανασκόπηση, αρχειοθέτηση και ερευνητικές περιπτώσεις.</p>

<p>Η παραγόμενη έξοδος είναι ένα <code>IRawImage</code> &mdash; ένα buffer εικονοστοιχείων BGRA32 με <code>Width</code>, <code>Height</code> και έναν πίνακα <code>Pixels</code>. Μπορείτε να αποθηκεύσετε αυτά τα δεδομένα εικονοστοιχείων σε PNG χρησιμοποιώντας οποιαδήποτε βιβλιοθήκη .NET για εικόνες όπως <code>System.Drawing</code>, <code>SkiaSharp</code> ή <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Απόδοση DICOM σε δεδομένα εικονοστοιχείων σε C#">}}

<p>Φορτώστε ένα αρχείο DICOM, αποδώστε το επιθυμητό καρέ και αποκτήστε πρόσβαση στα δεδομένα εικονοστοιχείων:</p>

<div class="codeblock" id="code">
 <h3>Απόδοση εικόνας DICOM - C#</h3>
 <pre><code class="cs">using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;

// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Render the first frame (index 0) through the full LUT pipeline
IRawImage image = dicomFile.RenderImage(0);

// Access the rendered pixel data
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = image.Pixels; // BGRA32 pixel buffer</code></pre>
</div>

<p>Η μέθοδος <code>RenderImage</code> εφαρμόζει αυτόματα τις τιμές παραθύρου/επιπέδου και τις παραμέτρους LUT που αποθηκεύονται στο σύνολο δεδομένων DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αποθήκευση αποδιδόμενης εικόνας ως PNG">}}

<p>Το <code>IRawImage</code> παρέχει ακατέργαστα δεδομένα εικονοστοιχείων BGRA32 που μπορούν να αποθηκευτούν σε PNG χωρίς απώλειες χρησιμοποιώντας οποιαδήποτε βιβλιοθήκη .NET για εικόνες. Ακολουθεί ένα παράδειγμα με χρήση του <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Αποθήκευση αποδιδόμενου καρέ DICOM ως PNG - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");
IRawImage image = dicomFile.RenderImage(0);

// Create a Bitmap from the BGRA32 pixel data
using var bitmap = new Bitmap(image.Width, image.Height, PixelFormat.Format32bppArgb);
var bitmapData = bitmap.LockBits(
    new Rectangle(0, 0, image.Width, image.Height),
    ImageLockMode.WriteOnly,
    PixelFormat.Format32bppArgb);

// Copy BGRA32 pixels to the bitmap
MemoryMarshal.AsBytes(image.Pixels.AsSpan())
    .CopyTo(new Span&lt;byte&gt;(
        (void*)bitmapData.Scan0,
        bitmapData.Stride * bitmapData.Height));

bitmap.UnlockBits(bitmapData);

// Save as lossless PNG
bitmap.Save("ct_scan.png", ImageFormat.Png);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Προσαρμοσμένο Παράθυρο/Επίπεδο">}}

<p>Ελέγξτε ποιο εύρος τιμών εικονοστοιχείων αποδίδεται ορίζοντας το Κέντρο Παραθύρου (Window Center) και το Πλάτος Παραθύρου (Window Width). Διαφορετικές ρυθμίσεις παραθύρου αποκαλύπτουν διαφορετικές ανατομικές δομές από τα ίδια δεδομένα DICOM:</p>

<div class="codeblock" id="code">
 <h3>Απόδοση με προσαρμοσμένο παράθυρο/επίπεδο - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);</code></pre>
</div>

<p>Η κλάση <code>GrayscaleRenderOptions</code> εκθέτει επίσης τις ιδιότητες <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> και <code>BitDepth</code> για πλήρη έλεγχο της ροής απόδοσης.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Απόδοση DICOM πολλαπλών καρέ">}}

<p>Αρχεία DICOM από CT, MRI και υπερηχογραφία συχνά περιέχουν πολλαπλά καρέ (φασικές). Αποδώστε όλα τα καρέ σε δεδομένα εικονοστοιχείων:</p>

<div class="codeblock" id="code">
 <h3>Απόδοση όλων των καρέ από DICOM πολλαπλών καρέ - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as PNG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Έλεγχος Επικάλυψης">}}

<p>Οι εικόνες DICOM μπορεί να περιέχουν επίπεδα επικάλυψης με σημειώσεις, μετρήσεις ή περιοχές ενδιαφέροντος. Ελέγξτε την ορατότητα και το χρώμα της επικάλυψης κατά την απόδοση:</p>

<div class="codeblock" id="code">
 <h3>Απόδοση με και χωρίς επικάλυψη - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible (default)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions { ShowOverlays = false };
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG για Ιατρικές Εικόνες">}}

<p>Η επιλογή μεταξύ PNG και JPG εξαρτάται από τη χρήση:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Χαρακτηριστικό</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Συμπίεση</strong></td><td>Lossless</td><td>Lossy</td></tr>
<tr><td><strong>Ποιότητα εικόνας</strong></td><td>Pixel-perfect &mdash; χωρίς τεχνουργήματα</td><td>Πιθανά μικρά τεχνουργήματα συμπίεσης</td></tr>
<tr><td><strong>Μέγεθος αρχείου</strong></td><td>Μεγαλύτερο (συνήθως 2&ndash;10x σε σχέση με JPG)</td><td>Μικρότερο</td></tr>
<tr><td><strong>Διαφάνεια</strong></td><td>Υποστηρίζεται (κανάλι άλφα)</td><td>Δεν υποστηρίζεται</td></tr>
<tr><td><strong>Καλύτερο για</strong></td><td>Διαγνωστική ανασκόπηση, αρχειοθέτηση, έρευνα, επικάλυψη</td><td>Διαμοίραση στο web, μικρογραφίες, προεπισκοπήσεις</td></tr>
<tr><td><strong>Κανονιστικό</strong></td><td>Προτιμάται για ροές εργασίας κρίσιμες στην ποιότητα</td><td>Αποδεκτό για μη διαγνωστική χρήση</td></tr>
</tbody>
</table>

<p>Το Aspose.Medical for .NET χρησιμοποιεί την ίδια ροή απόδοσης για και τις δύο εξόδους. Τα δεδομένα εικονοστοιχείων του <code>IRawImage</code> είναι τα ίδια ανεξάρτητα από τη μορφή προορισμού — η επιλογή μορφής επηρεάζει μόνο το τελικό βήμα αποθήκευσης που εκτελεί η βιβλιοθήκη εικόνων σας.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πηγές Μάθησης" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Τεκμηρίωση" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Κώδικας Πηγής" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Αναφορές API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Υποστήριξη Προϊόντος" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Δωρεάν Υποστήριξη" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Πληρωτέο Υποστήριξη" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Ιστολόγιο" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Γιατί Aspose.Medical for .NET;" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Λίστα Πελατών" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Ιστορίες Επιτυχίας" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
