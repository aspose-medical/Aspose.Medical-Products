---
title: Μετατροπή DICOM σε TIFF σε C# .NET | Aspose.Medical
weight: 12000
description: Απόδοση εικόνων DICOM σε δεδομένα pixel σε C# .NET και αποθήκευση σε TIFF με υποστήριξη πολλαπλών σελίδων για αρχεία DICOM πολλαπλών καρέ. Χρησιμοποιήστε το πλήρες pipeline LUT DICOM με το API Aspose.Medical και αποθηκεύστε σε TIFF χρησιμοποιώντας οποιαδήποτε βιβλιοθήκη απεικόνισης .NET.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Μετατροπή DICOM σε TIFF σε .NET C#" h2="Απόδοση εικόνων DICOM σε ακατέργαστα δεδομένα pixel με πλήρη υποστήριξη pipeline grayscale. Διατηρήστε την ποιότητα της εικόνας με έλεγχο παραθύρου/επίπεδου, απόδοση overlay και επεξεργασία πολλαπλών καρέ. Αποθήκευση σε TIFF &mdash; συμπεριλαμβανομένου του multi-page TIFF &mdash; χρησιμοποιώντας οποιαδήποτε βιβλιοθήκη απεικόνισης .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Μετατροπή DICOM σε TIFF">}}

<p><strong>Aspose.Medical for .NET</strong> αποδίδει εικόνες DICOM μέσω του τυπικού pipeline grayscale DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT) για να παραγάγει σωστά παραθυροποιημένα δεδομένα pixel BGRA32. Το TIFF είναι μια ευέλικτη μορφή που χρησιμοποιείται εκτενώς στην ιατρική απεικόνιση, την επιστημονική έρευνα και τη δημοσίευση, επειδή υποστηρίζει συμπίεση χωρίς απώλειες, υψηλά βάθη bit, και &mdash; το πιο σημαντικό &mdash; <strong>έγγραφα πολλαπλών σελίδων</strong>. Αυτό καθιστά το TIFF την φυσική επιλογή για μετατροπή αρχείων DICOM πολλαπλών καρέ (CT, MRI, ultrasound) σε ένα ενιαίο αρχείο εξόδου που διατηρεί όλες τις τομές.</p>

<p>Η αποδοθείσα έξοδος είναι ένα <code>IRawImage</code> &mdash; ένας buffer pixel BGRA32 με <code>Width</code>, <code>Height</code> και έναν πίνακα <code>Pixels</code>. Μπορείτε να αποθηκεύσετε αυτά τα δεδομένα pixel σε TIFF χρησιμοποιώντας οποιαδήποτε βιβλιοθήκη απεικόνισης .NET όπως <code>System.Drawing</code>, <code>SkiaSharp</code> ή <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Απόδοση DICOM σε δεδομένα pixel σε C#">}}

<p>Φορτώστε ένα αρχείο DICOM, αποδώστε το επιθυμητό καρέ και προσπελάστε τα δεδομένα pixel:</p>

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

<p>Η μέθοδος <code>RenderImage</code> εφαρμόζει αυτόματα τις τιμές παραθύρου/επίπεδου και τις παραμέτρους LUT που αποθηκεύονται στο σύνολο δεδομένων DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αποθήκευση αποδοθείσας εικόνας ως TIFF">}}

<p>Το <code>IRawImage</code> παρέχει ακατέργαστα δεδομένα pixel BGRA32 που μπορούν να αποθηκευτούν σε TIFF χρησιμοποιώντας οποιαδήποτε βιβλιοθήκη απεικόνισης .NET. Ακολουθεί ένα παράδειγμα με χρήση του <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Αποθήκευση αποδοθέντος καρέ DICOM ως TIFF - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("xray.dcm");
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

// Save as TIFF
bitmap.Save("xray.tiff", ImageFormat.Tiff);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Multi-Page TIFF από Multi-Frame DICOM">}}

<p>Τα αρχεία DICOM πολλαπλών καρέ (π.χ., σειρές CT ή MRI) περιέχουν πολλαπλές τομές εικόνας σε ένα ενιαίο αρχείο. Η δυνατότητα multi-page του TIFF σας επιτρέπει να αποθηκεύσετε όλα τα καρέ σε ένα αρχείο εξόδου, διατηρώντας τη πλήρης μελέτη σε μια καθολικά αναγώγιμη μορφή. Χρησιμοποιήστε τον κωδικοποιητή TIFF του <code>System.Drawing</code> με παραμέτρους multi-frame:</p>

<div class="codeblock" id="code">
 <h3>Μετατροπή DICOM πολλαπλών καρέ σε multi-page TIFF - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
int totalFrames = dicomFile.NumberOfFrames;

// Get the TIFF codec
var tiffCodec = ImageCodecInfo.GetImageEncoders()
    .First(c =&gt; c.FormatID == ImageFormat.Tiff.Guid);

// Multi-frame parameters
var multiFrameParams = new EncoderParameters(2);
multiFrameParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.MultiFrame);
multiFrameParams.Param[1] = new EncoderParameter(
    Encoder.Compression, (long)EncoderValue.CompressionLZW);

// Parameter for adding subsequent frames
var frameAddParams = new EncoderParameters(1);
frameAddParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.FrameDimensionPage);

// Parameter for flushing the final frame
var flushParams = new EncoderParameters(1);
flushParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.Flush);

Bitmap? tiffBitmap = null;

for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var frameBitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = frameBitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    frameBitmap.UnlockBits(bitmapData);

    if (i == 0)
    {
        // Save the first frame and start multi-page TIFF
        tiffBitmap = (Bitmap)frameBitmap.Clone();
        tiffBitmap.Save("ct_series.tiff", tiffCodec, multiFrameParams);
    }
    else
    {
        // Add subsequent frames
        tiffBitmap!.SaveAdd(frameBitmap, frameAddParams);
    }
}

// Flush and close the multi-page TIFF
tiffBitmap?.SaveAdd(flushParams);
tiffBitmap?.Dispose();</code></pre>
</div>

<p>Μπορείτε επίσης να αποθηκεύσετε κάθε καρέ ως ξεχωριστό αρχείο TIFF όταν απαιτούνται μεμονωμένες τομές:</p>

<div class="codeblock" id="code">
 <h3>Μετατροπή κάθε καρέ σε ξεχωριστό TIFF - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

for (int i = 0; i &lt; dicomFile.NumberOfFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var bitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = bitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    bitmap.UnlockBits(bitmapData);
    bitmap.Save($"frame_{i:D4}.tiff", ImageFormat.Tiff);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Προσαρμοσμένο Window/Level">}}

<p>Ελέγξτε ποιο εύρος τιμών pixel αποδίδεται ρυθμίζοντας το Window Center και το Window Width. Διαφορετικές ρυθμίσεις παραθύρου αποκαλύπτουν διαφορετικές ανατομικές δομές από τα ίδια δεδομένα DICOM:</p>

<div class="codeblock" id="code">
 <h3>Απόδοση με προσαρμοσμένο window/level - C#</h3>
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
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

<p>Η κλάση <code>GrayscaleRenderOptions</code> εκθέτει επίσης τις ιδιότητες <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> και <code>BitDepth</code> για πλήρη έλεγχο του pipeline απόδοσης.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Απόδοση Multi-Frame DICOM">}}

<p>Τα αρχεία DICOM από CT, MRI και υπερηχογραφία συχνά περιέχουν πολλαπλά καρέ (τομές). Αποδώστε όλα τα καρέ σε δεδομένα pixel:</p>

<div class="codeblock" id="code">
 <h3>Απόδοση όλων των καρέ από multi-frame DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as TIFF using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Έλεγχος Overlay">}}

<p>Οι εικόνες DICOM μπορεί να περιέχουν επίπεδα overlay με σημειώσεις, μετρήσεις ή περιοχές ενδιαφέροντος. Ελέγξτε την ορατότητα και το χρώμα του overlay κατά την απόδοση:</p>

<div class="codeblock" id="code">
 <h3>Απόδοση με και χωρίς overlays - C#</h3>
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
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG για Ιατρικές Εικόνες">}}

<p>Κάθε μορφή εξόδου εξυπηρετεί διαφορετικές περιπτώσεις χρήσης:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Χαρακτηριστικό</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Συμπίεση</strong></td><td>Χωρίς απώλειες (LZW, ZIP) ή χωρίς συμπίεση</td><td>Χωρίς απώλειες</td><td>Με απώλειες</td></tr>
<tr><td><strong>Multi-Page</strong></td><td>Υποστηρίζεται &mdash; όλα τα καρέ σε ένα αρχείο</td><td>Δεν υποστηρίζεται</td><td>Δεν υποστηρίζεται</td></tr>
<tr><td><strong>Ποιότητα Εικόνας</strong></td><td>Pixel-perfect</td><td>Pixel-perfect</td><td>Πιθανές εικονοπαραμορφώσεις από τη συμπίεση</td></tr>
<tr><td><strong>Μέγεθος Αρχείου</strong></td><td>Μεγαλύτερο</td><td>Μεσαίο</td><td>Μικρότερο</td></tr>
<tr><td><strong>Διαφάνεια</strong></td><td>Υποστηρίζεται</td><td>Υποστηρίζεται</td><td>Δεν υποστηρίζεται</td></tr>
<tr><td><strong>Καλύτερο για</strong></td><td>Αρχειοθέτηση, σειρές multi-frame, εκτύπωση, έρευνα</td><td>Διαγνωστική ανασκόπηση, ιστός με ποιότητα</td><td>Κοινή χρήση στο διαδίκτυο, μικρογραφίες</td></tr>
</tbody>
</table>

<p>Το Aspose.Medical for .NET χρησιμοποιεί το ίδιο pipeline απόδοσης για όλους τους στόχους εξόδου. Τα δεδομένα pixel του <code>IRawImage</code> είναι τα ίδια ανεξαρτήτως μορφής στόχου &mdash; η επιλογή μορφής επηρεάζει μόνο το τελικό βήμα αποθήκευσης που εκτελεί η βιβλιοθήκη απεικόνισης. Το TIFF είναι η προτιμώμενη μορφή όταν χρειάζεται να διατηρήσετε μια πλήρη μελέτη DICOM πολλαπλών καρέ σε ένα αρχείο ή όταν η έξοδος προορίζεται για αρχειοθέτηση, εκτύπωση ή περαιτέρω επεξεργασία εικόνας.</p>

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

{{< blocks/products/pf/slr-tab tabTitle="Γιατί Aspose.Medical for .NET;" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Λίστα Πελατών" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Ιστορίες Επιτυχίας" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
