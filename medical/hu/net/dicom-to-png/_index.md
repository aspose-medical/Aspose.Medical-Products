---
title: DICOM konvertálása PNG-re C# .NET-ben | Aspose.Medical
weight: 11000
description: Renderelje a DICOM képeket pixeladatokra C# .NET-ben, veszteségmentes minőség, ablak/szint szabályozás és többkeretes támogatás mellett. Használja a teljes DICOM LUT csővezetékláncot az Aspose.Medical API-val, és mentse PNG formátumba.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM konvertálása PNG-re .NET C#-ban" h2="Renderelje a DICOM képeket nyers pixeladatokra a teljes szürkeárnyalatos csővezeték támogatásával. Tartsa meg a képminőséget ablak/szint szabályozással, overlay rendereléssel és többkeretes feldolgozással. Mentse veszteségmentes PNG‑ként bármely .NET képkönyvtárral." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM → PNG konverzió">}}

<p><strong>Aspose.Medical for .NET</strong> a szabványos DICOM szürkeárnyalatos csővezetéken (Modality LUT, VOI LUT, Presentation LUT, Output LUT) keresztül rendereli a DICOM képeket, hogy helyesen ablakolt BGRA32 pixeladatokat állítson elő. A PNG veszteségmentes formátum, így előnyös választás, ha a képminőséget tömörítési artefaktusok nélkül kell megőrizni &mdash; ami elengedhetetlen a diagnosztikai felülvizsgálathoz, archiváláshoz és kutatási felhasználásokhoz.</p>

<p>A renderelt kimenet egy <code>IRawImage</code> &mdash; egy BGRA32 pixelpuffer <code>Width</code>, <code>Height</code> és egy <code>Pixels</code> tömbbel. Ezt a pixeladatot PNG‑ként mentheti bármely .NET képkönyvtárral, például <code>System.Drawing</code>, <code>SkiaSharp</code> vagy <code>ImageSharp</code> segítségével.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM renderelése pixeladatokra C#-ban">}}

<p>Töltsön be egy DICOM fájlt, renderelje a kívánt képkockát, és férjen hozzá a pixeladatokhoz:</p>

<div class="codeblock" id="code">
 <h3>DICOM kép renderelése - C#</h3>
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

<p>A <code>RenderImage</code> metódus automatikusan alkalmazza a DICOM adathalmazban tárolt ablak/szint értékeket és LUT paramétereket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderelt kép mentése PNG‑ként">}}

<p>Az <code>IRawImage</code> nyers BGRA32 pixeladatot biztosít, amely bármely .NET képkönyvtár segítségével menthető veszteségmentes PNG‑ként. Íme egy példa <code>System.Drawing</code> használatával:</p>

<div class="codeblock" id="code">
 <h3>Renderelt DICOM képkocka mentése PNG‑ként - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Egyéni ablak/szint">}}

<p>Szabályozza, hogy a pixelértékek mely tartománya legyen renderelve a Window Center és Window Width beállításával. Különböző ablakbeállítások más‑más anatómiai struktúrákat tárnak fel ugyanabból a DICOM adatból:</p>

<div class="codeblock" id="code">
 <h3>Renderelés egyéni ablak/szint beállítással - C#</h3>
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

<p>A <code>GrayscaleRenderOptions</code> osztály továbbá elérhetővé teszi a <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> és <code>BitDepth</code> tulajdonságokat a renderelési csővezeték teljes irányítása érdekében.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Többkeretes DICOM renderelés">}}

<p>A CT, MRI és ultrahang DICOM fájlok gyakran több keretet (szeletet) tartalmaznak. Renderelje az összes keretet pixeladatokká:</p>

<div class="codeblock" id="code">
 <h3>Az összes keret renderelése többkeretes DICOM‑ból - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Overlay vezérlés">}}

<p>A DICOM képek tartalmazhatnak overlay síkokat annotációkkal, mérésekkel vagy érdeklődési területekkel. Szabályozza az overlay láthatóságát és színét a renderelés során:</p>

<div class="codeblock" id="code">
 <h3>Renderelés overlayvel és overlay nélkül - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG orvosi képekhez">}}

<p>A PNG és JPG közötti választás a felhasználási esettől függ:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Jellemző</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Tömörítés</strong></td><td>Veszteségmentes</td><td>Veszteséges</td></tr>
<tr><td><strong>Képminőség</strong></td><td>Pixel‑tökéletes &mdash; nincs artefakt</td><td>Kisebb tömörítési artefaktusok előfordulhatnak</td></tr>
<tr><td><strong>Fájlméret</strong></td><td>Nagyobb (általában 2&ndash;10× a JPG-hez képest)</td><td>Kisebb</td></tr>
<tr><td><strong>Átlátszóság</strong></td><td>Támogatott (alpha csatorna)</td><td>Nem támogatott</td></tr>
<tr><td><strong>Legalkalmasabb</strong></td><td>Diagnosztikai felülvizsgálat, archiválás, kutatás, overlay‑k</td><td>Webes megosztás, miniatűrök, előnézetek</td></tr>
<tr><td><strong>Szabályozási</strong></td><td>Előnyben részesített a minőségkritikus munkafolyamatoknál</td><td>Elfogadható nem diagnosztikus használatra</td></tr>
</tbody>
</table>

<p>Az Aspose.Medical for .NET ugyanazt a renderelési csővezetéket használja mindkét kimeneti célhoz. A <code>IRawImage</code> pixeladatok azonosak a célformátumtól függetlenül &mdash; a formátum választása csak a végső mentési lépést befolyásolja, amelyet az Ön képkönyvtára végez.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Forráskód" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetős támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért az Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
