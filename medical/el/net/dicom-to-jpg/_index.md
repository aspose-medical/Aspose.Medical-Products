---
title: Μετατροπή DICOM σε JPG σε C# .NET | Aspose.Medical
weight: 7000
description: Απόδοση εικόνων DICOM σε δεδομένα pixel σε C# .NET με έλεγχο παραθύρου/επίπεδου, απόδοση επικάλυψης και υποστήριξη πολλαπλών πλαισίων. Χρησιμοποιήστε πλήρη διαδρομή DICOM LUT με το Aspose.Medical API και αποθηκεύστε σε JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Μετατροπή DICOM σε JPG σε .NET C#" h2="Απόδοση εικόνων DICOM σε ακατέργαστα δεδομένα pixel με πλήρη υποστήριξη γραμμής εργασίας γκρι αποχρώσεων, συμπεριλαμβανομένου παραθύρου/επίπεδου, Modality LUT, VOI LUT και απόδοσης επικάλυψης. Αποθηκεύστε σε JPG χρησιμοποιώντας οποιαδήποτε βιβλιοθήκη .NET για εικόνες." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Διαδικασία Απόδοσης Εικόνας DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> αποδίδει εικόνες DICOM μέσω της τυπικής γραμμής εργασίας γκρι αποχρώσεων DICOM που ορίζεται στην προδιαγραφή DICOM. Η διαδικασία απόδοσης εφαρμόζει μια αλυσίδα Look-Up Tables (LUTs) για να μετατρέψει τις ακατέργαστες αποθηκευμένες τιμές pixel σε προβολές εικόνων:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; μετατρέπει τις αποθηκευμένες τιμές pixel σε ανεξάρτητες από τον κατασκευαστή τιμές χρησιμοποιώντας Rescale Slope/Intercept ή μια ακολουθία LUT.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; εφαρμόζει παράθυρο/επίπεδο (Window Center και Window Width) για να επιλέξει το εύρος των τιμών που θα εμφανιστούν, με υποστήριξη για μετασχηματισμούς Linear, Linear Exact και Sigmoid.</li>
<li><strong>Presentation LUT</strong> &mdash; αντιστοιχίζει τις τελικές τιμές σε προβολές P-Values, περιλαμβάνοντας υποστήριξη για INVERSE (αντιστροφή εικόνας).</li>
<li><strong>Output LUT</strong> &mdash; μετατρέπει τις τιμές γκρι σε RGB για προβολή, με προαιρετικό χρωματισμό χρησιμοποιώντας προσαρμοσμένους χάρτες χρώματος ή πίνακες Palette Color.</li>
</ol>

<p>Το παραγόμενο αποτέλεσμα είναι ένα <code>IRawImage</code> &mdash; ένας buffer pixel BGRA32 (8-bit ανά κανάλι) με <code>Width</code>, <code>Height</code> και έναν πίνακα <code>Pixels</code>. Μπορείτε στη συνέχεια να αποθηκεύσετε αυτά τα δεδομένα pixel σε JPG χρησιμοποιώντας οποιαδήποτε βιβλιοθήκη .NET για επεξεργασία εικόνας όπως <code>System.Drawing</code>, <code>SkiaSharp</code> ή <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Απόδοση DICOM σε Δεδομένα Pixel σε C#">}}

<p>Φορτώστε ένα αρχείο DICOM και αποδώστε ένα πλαίσιο σε ένα <code>IRawImage</code> που περιέχει δεδομένα pixel BGRA32:</p>

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

<p>Η μέθοδος <code>RenderImage</code> εφαρμόζει αυτόματα την πλήρη γραμμή εργασίας γκρι αποχρώσεων χρησιμοποιώντας τις τιμές παραθύρου/επίπεδου και τις παραμέτρους LUT που είναι αποθηκευμένες στο αρχείο DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Αποθήκευση Αποδομένης Εικόνας ως JPG">}}

<p>Το <code>IRawImage</code> παρέχει ακατέργαστα δεδομένα pixel BGRA32 που μπορούν να αποθηκευτούν σε JPG χρησιμοποιώντας οποιαδήποτε βιβλιοθήκη .NET για εικόνες. Εδώ είναι ένα παράδειγμα με χρήση του <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Αποθήκευση αποδοθέντος πλαισίου DICOM ως JPG - C#</h3>
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

// Save as JPG
bitmap.Save("ct_scan.jpg", ImageFormat.Jpeg);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Προσαρμοσμένο Παράθυρο/Επίπεδο (Windowing)">}}

<p>Το παράθυρο/επίπεδο (επίσης καλούμενο windowing ή W/L) είναι η πιο σημαντική παράμετρος για την απόδοση εικόνας DICOM. Ελέγχει ποιο εύρος τιμών pixel αντιστοιχίζεται στο ορατό εύρος γκρι αποχρώσεων. Το <strong>Window Center</strong> ορίζει το μεσαίο σημείο και το <strong>Window Width</strong> ορίζει το εύρος των εμφανιζόμενων τιμών:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; υψηλή αντίθεση, λιγότερα επίπεδα γκρι εμφανίζονται (χρήσιμο για ήπιο ιστό).</li>
<li><strong>Wide window</strong> &mdash; χαμηλότερη αντίθεση, περισσότερα επίπεδα γκρι εμφανίζονται (χρήσιμο για οστό ή πνεύμονα).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Απόδοση με προσαρμοσμένο παράθυρο/επίπεδο - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window: center=40, width=400
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window: center=400, width=1800
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Lung window: center=-600, width=1500
var lung = new GrayscaleRenderOptions
{
    WindowCenter = -600,
    WindowWidth = 1500
};
IRawImage lungImage = dicomFile.RenderImage(lung, 0);</code></pre>
</div>

<p>Κοινές προεπιλογές παραθύρου CT:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Τύπος Ιστού</th>
<th>Κέντρο Παραθύρου</th>
<th>Πλάτος Παραθύρου</th>
</tr>
</thead>
<tbody>
<tr><td>Ήπιος Ιστός</td><td>40</td><td>400</td></tr>
<tr><td>Πνεύμονας</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Οστό</td><td>400</td><td>1800</td></tr>
<tr><td>Εγκέφαλος</td><td>40</td><td>80</td></tr>
<tr><td>Σίτεγγος</td><td>60</td><td>150</td></tr>
<tr><td>Μεσοθωράκιο</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Επιλογές Απόδοσης Γκρι Αποχρώσεων">}}

<p>Η κλάση <code>GrayscaleRenderOptions</code> παρέχει πλήρη έλεγχο της διαδικασίας απόδοσης γκρι αποχρώσεων:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Ιδιότητα</th>
<th>Περιγραφή</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Κέντρο του εμφανιζόμενου εύρους τιμών (Οριζόντιο μεσαίο σημείο του VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Πλάτος του εμφανιζόμενου εύρος τιμών (ελέγχει την αντίθεση)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Κλίση κλίμακας Modality LUT — πολλαπλασιάζεται με τις αποθηκευμένες τιμές pixel</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Απόσπαση κλίμακας Modality LUT — προστίθεται μετά τον πολλαπλασιασμό με την κλίση</td></tr>
<tr><td><code>Invert</code></td><td>Αντιστρέφει την εικόνα γκρι αποχρώσεων (εφαρμόζει σχήμα INVERSE Presentation LUT)</td></tr>
<tr><td><code>BitDepth</code></td><td>Πληροφορίες βάθους bit: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Αντεστραμμένη απόδοση γκρι αποχρώσεων - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray.dcm");

// Render with inverted grayscale (white-on-black becomes black-on-white)
var options = new GrayscaleRenderOptions
{
    WindowCenter = 2048,
    WindowWidth = 4096,
    Invert = true
};

IRawImage image = dicomFile.RenderImage(options, 0);</code></pre>
</div>

<p>Χρησιμοποιήστε <code>RenderOptionsFactory.Create(pixelData)</code> για να γεμίσετε αυτόματα τις επιλογές απόδοσης από τα χαρακτηριστικά δεδομένων pixel του συνόλου δεδομένων DICOM, συμπεριλαμβανομένου του βάθους bit, των παραμέτρων κλίμακας, του παραθύρου/επίπεδου και των ρυθμίσεων αντιστροφής.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Απόδοση Επικάλυψης">}}

<p>Οι εικόνες DICOM μπορούν να περιέχουν επίπεδα επικάλυψης — γραφικά ή κειμενικές σημειώσεις που ενσωματώνονται στην εικόνα. Η κλάση <code>RenderOptions</code> ελέγχει εάν οι επικαλύψεις θα αποδοθούν και σε ποιο χρώμα θα εμφανιστούν:</p>

<div class="codeblock" id="code">
 <h3>Απόδοση με έλεγχο επικάλυψης - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible in white (default behavior)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions
{
    ShowOverlays = false
};
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

<p>Το DICOM υποστηρίζει δύο τύπους επικάλυψης: επικαλύψεις <strong>Graphics</strong> (σημειώσεις, μετρήσεις) και επικαλύψεις <strong>Region of Interest</strong> (ROI). Και οι δύο τύποι ελέγχονται από την ρύθμιση <code>ShowOverlays</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Απόδοση Πολλαπλών Πλαισίων DICOM">}}

<p>Πολλά αρχεία DICOM από CT, MRI και υπέρηχους περιέχουν πολλαπλά πλαίσια (τομές). Μπορείτε να ελέγξετε τον αριθμό των πλαισίων και να αποδώσετε το καθένα ξεχωριστά:</p>

<div class="codeblock" id="code">
 <h3>Απόδοση όλων των πλαισίων από DICOM πολλαπλών πλαισίων - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;
Console.WriteLine($"Total frames: {totalFrames}");

// Render all frames to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as JPG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Τύποι Μετασχηματισμού VOI LUT">}}

<p>Το πρότυπο DICOM ορίζει πολλές λειτουργίες μετασχηματισμού VOI LUT που ελέγχουν πώς εφαρμόζεται η αντιστοίχιση παράθυρου/επίπεδου. Το Aspose.Medical for .NET υποστηρίζει όλους τους τυπικούς τύπους:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Τύπος</th>
<th>Περιγραφή</th>
<th>Περίπτωση Χρήσης</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Τυπική γραμμική αντιστοίχιση με μετατόπιση από το κέντρο</td><td>Κοινότερο, χρησιμοποιείται για γενική απεικόνιση CT/MR</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Γραμμική αντιστοίχιση χωρίς μετατόπιση από το κέντρο</td><td>Όταν απαιτείται ακριβής αντιστοίχιση τιμών pixel</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>Μετασχηματισμός S-καμπύλης για πιο ομαλές μεταβάσεις αντίθεσης</td><td>Οπτική ήπιου ιστού, μαστογραφία</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>Προσαρμοσμένο LUT ορισμένο ως πίνακας αναζήτησης στο σύνολο δεδομένων DICOM</td><td>Καμπύλες απεικόνισης ειδικές προμηθευτή ή ειδικές για τη λειτουργία</td></tr>
</tbody>
</table>

<p>Η μηχανή απόδοσης επιλέγει αυτόματα τον κατάλληλο μετασχηματισμό VOI LUT βάσει των χαρακτηριστικών του συνόλου δεδομένων DICOM. Όταν χρησιμοποιείτε προσαρμοσμένα <code>GrayscaleRenderOptions</code>, ο Linear μετασχηματισμός εφαρμόζεται εξ ορισμού.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Υποστηριζόμενες Λειτουργίες">}}

<p>Το Aspose.Medical for .NET υποστηρίζει την απόδοση εικόνων DICOM από όλες τις κοινές ιατρικές απεικονιστικές λειτουργίες:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Λειτουργία</th>
<th>Περιγραφή</th>
<th>Τυπική Μορφή</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Υπολογιστική Τομογραφία</td><td>Γκρι αποχρώσεων, πολλαπλά πλαίσια, 12–16 bit</td></tr>
<tr><td><strong>MR</strong></td><td>Μαγγαντική Συσπορά</td><td>Γκρι αποχρώσεων, πολλαπλά πλαίσια, 12–16 bit</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Υπολογιστική / Ψηφιακή Ακτινογραφία</td><td>Γκρι αποχρώσεων, μονοπλαίσιο, 10–16 bit</td></tr>
<tr><td><strong>US</strong></td><td>Υπέρηχος</td><td>Γκρι αποχρώσεων ή RGB, πολλαπλά πλαίσια</td></tr>
<tr><td><strong>MG</strong></td><td>Μαστογραφία</td><td>Γκρι αποχρώσεων, υψηλή ανάλυση, 12–16 bit</td></tr>
<tr><td><strong>XA</strong></td><td>Αγγειογραφία Ακτινών Χ</td><td>Γκρι αποχρώσεων, πολλαπλά πλαίσια</td></tr>
<tr><td><strong>NM</strong></td><td>Πυρηνική Ιατρική</td><td>Γκρι αποχρώσεων, πολλαπλά πλαίσια</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Τομογραφία Εκπομπής Θετικών Σωματιδίων)</td><td>Γκρι αποχρώσεων, πολλαπλά πλαίσια</td></tr>
</tbody>
</table>

<p>Υποστηρίζονται τόσο οι ερμηνείες φωτομετρίας γκρι (MONOCHROME1/MONOCHROME2) όσο και χρώματος (RGB, YBR_FULL, PALETTE COLOR). Η μηχανή απόδοσης διαχειρίζεται αυτόματα τη σωστή μετατροπή χρωματικού χώρου.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Πόροι Εκμάθησης" tabId="resources" >}}
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
