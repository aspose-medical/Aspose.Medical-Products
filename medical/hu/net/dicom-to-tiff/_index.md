---
title: DICOM konvertálása TIFF-be C# .NET-ben | Aspose.Medical
weight: 12000
description: DICOM képek renderelése pixeladatokká C# .NET-ben, és mentés TIFF-be többoldalas támogatással többkeretes DICOM fájlok esetén. Használja a teljes DICOM LUT csővezetéket az Aspose.Medical API-val, és mentse TIFF-be bármely .NET képkönyvtár segítségével.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM konvertálása TIFF-be .NET C#-ban" h2="DICOM képek renderelése nyers pixeladatokká a teljes szürkeárnyalatos csővezeték támogatásával. A képek minőségének megőrzése ablak/szint (window/level) vezérléssel, overlay rendereléssel és többkeretes feldolgozással. Mentés TIFF-be &mdash; beleértve a többoldalas TIFF-et &mdash; bármely .NET képkönyvtár használatával." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM átalakítása TIFF-be">}}

<p><strong>Aspose.Medical for .NET</strong> rendereli a DICOM képeket a szabványos DICOM szürkeárnyalatos csővezetéken (Modality LUT, VOI LUT, Presentation LUT, Output LUT) keresztül, hogy helyesen ablakolt BGRA32 pixeladatokat állítson elő. A TIFF egy sokoldalú formátum, amely széles körben használt az orvosi képalkotásban, a tudományos kutatásban és a kiadványszerkesztésben, mivel támogatja a veszteségmentes tömörítést, a nagy bitmélységet, és &mdash; legfőképp &mdash; <strong>többoldalas dokumentumokat</strong>. Ez teszi a TIFF-et a természetes választássá a többkeretes DICOM fájlok (CT, MRI, ultrahang) egyetlen kimeneti fájlba való konvertálásához, amely megőrzi az összes szeletet.</p>

<p>A renderelt kimenet egy <code>IRawImage</code> &mdash; egy BGRA32 pixel puffer <code>Width</code>, <code>Height</code> és egy <code>Pixels</code> tömbbel. Ezt a pixeladatot menthet TIFF-be bármely .NET képkönyvtár használatával, például <code>System.Drawing</code>, <code>SkiaSharp</code> vagy <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM renderelése pixeladatokba C#-ban">}}

<p>Töltsön be egy DICOM fájlt, renderelje a kívánt keretet, és érje el a pixeladatokat:</p>

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

{{< blocks/products/pf/feature-page-section h2="Renderelt kép mentése TIFF-be">}}

<p>A <code>IRawImage</code> nyers BGRA32 pixel adatokat biztosít, amelyeket bármely .NET képkönyvtárral menthet TIFF-be. Íme egy példa a <code>System.Drawing</code> használatával:</p>

<div class="codeblock" id="code">
 <h3>Renderelt DICOM keret mentése TIFF-be - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Többoldalas TIFF többkeretes DICOM-ból">}}

<p>A többkeretes DICOM fájlok (pl. CT vagy MRI sorozatok) egyetlen fájlban több képszeletet tartalmaznak. A TIFF többoldalas képessége lehetővé teszi, hogy az összes keretet egy kimeneti fájlba tárolja, megőrizve a teljes vizsgálatot egy mindenki által olvasható formátumban. Használja a <code>System.Drawing</code> TIFF enkódert többkeretes paraméterekkel:</p>

<div class="codeblock" id="code">
 <h3>Többkeretes DICOM átalakítása többoldalas TIFF-be - C#</h3>
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

<p>Emellett minden keretet külön TIFF fájlként is menthet, ha egyedi szeletekre van szükség:</p>

<div class="codeblock" id="code">
 <h3>Minden keret átalakítása külön TIFF-be - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Egyedi ablak/szint">}}

<p>Állítsa be, hogy a pixelértékek mely tartománya legyen renderelve az Ablak Közép (Window Center) és Ablak Szélesség (Window Width) megadásával. Különböző ablakbeállítások különböző anatómiai struktúrákat tárnak fel ugyanabból a DICOM adathalmazból:</p>

<div class="codeblock" id="code">
 <h3>Renderelés egyedi ablak/szint - C#</h3>
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

<p>A <code>GrayscaleRenderOptions</code> osztály szintén elérhetővé teszi a <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> és <code>BitDepth</code> tulajdonságokat a renderelési csővezeték teljes irányítása érdekében.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Többkeretes DICOM renderelés">}}

<p>A CT, MRI és ultrahang DICOM fájlok gyakran több keretet (szeletet) tartalmaznak. Renderelje az összes keretet pixeladatokká:</p>

<div class="codeblock" id="code">
 <h3>Az összes keret renderelése többkeretes DICOM-ból - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Overlay vezérlés">}}

<p>A DICOM képek tartalmazhatnak overlay síkokat megjegyzésekkel, mérésekkel vagy érdeklődési területekkel. Vezérelje az overlay láthatóságát és színét a renderelés során:</p>

<div class="codeblock" id="code">
 <h3>Renderelés overlay-vel és overlay nélkül - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG orvosi képekhez">}}

<p>Minden kimeneti formátum különböző felhasználási esetekhez szolgál:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Jellemző</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Tömörítés</strong></td><td>Veszteségmentes (LZW, ZIP) vagy tömörítetlen</td><td>Veszteségmentes</td><td>Veszteséges</td></tr>
<tr><td><strong>Többoldalas</strong></td><td>Támogatott &mdash; minden keret egy fájlban</td><td>Nem támogatott</td><td>Nem támogatott</td></tr>
<tr><td><strong>Képminőség</strong></td><td>Pixel-pontos</td><td>Pixel-pontos</td><td>Tömörítési hibák lehetségesek</td></tr>
<tr><td><strong>Fájlméret</strong></td><td>Legnagyobb</td><td>Közepes</td><td>Legkisebb</td></tr>
<tr><td><strong>Átlátszóság</strong></td><td>Támogatott</td><td>Támogatott</td><td>Nem támogatott</td></tr>
<tr><td><strong>Leginkább alkalmas</strong></td><td>Archiválás, többkeretes sorozatok, nyomtatás, kutatás</td><td>Diagnosztikai áttekintés, minőségi web</td><td>Webes megosztás, bélyegképek</td></tr>
</tbody>
</table>

<p>Az Aspose.Medical for .NET minden kimeneti célhoz ugyanazt a renderelési csővezetéket használja. A <code>IRawImage</code> pixel adatok azonosak a célformátumtól függetlenül &mdash; a formátum választása csak a végső mentési lépést befolyásolja, amelyet az Ön képkönyvtára hajt végre. A TIFF a preferált formátum, ha egy teljes többkeretes DICOM vizsgálatot kell egyetlen fájlban megőrizni, vagy ha a kimenetet archiválásra, nyomtatásra vagy további képfeldolgozásra szánják.</p>

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
