---
title: Převést DICOM na TIFF v C# .NET | Aspose.Medical
weight: 12000
description: Vykreslete DICOM obrázky do pixelových dat v C# .NET a uložte do TIFF s podporou více stránek pro soubory DICOM s více snímky. Použijte kompletní DICOM LUT pipeline s Aspose.Medical API a uložte do TIFF pomocí libovolné .NET knihovny pro práci s obrazem.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Převést DICOM na TIFF v .NET C#" h2="Vykreslete DICOM obrázky do surových pixelových dat s plnou podporou grayscale pipeline. Zachovejte kvalitu obrazu pomocí nastavení okna/úrovně, vykreslování overlay a zpracování více snímků. Uložte do TIFF &mdash; včetně více‑stránkového TIFFu &mdash; pomocí libovolné .NET knihovny pro práci s obrazem." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Převod DICOM na TIFF">}}

<p><strong>Aspose.Medical for .NET</strong> vykresluje DICOM obrázky pomocí standardní DICOM grayscale pipeline (Modality LUT, VOI LUT, Presentation LUT, Output LUT) a vytváří správně okno‑nastavená BGRA32 pixelová data. TIFF je všestranný formát široce používán v lékařském zobrazování, vědeckém výzkumu a publikování, protože podporuje bezztrátovou kompresi, vysokou bitovou hloubku a &mdash; co je nejdůležitější &mdash; <strong>více‑stránkové dokumenty</strong>. To činí TIFF přirozenou volbou pro převod více‑snímkových DICOM souborů (CT, MRI, ultrazvuk) do jediného výstupního souboru, který zachová všechny řezy.</p>

<p>Vykreslený výstup je <code>IRawImage</code> &mdash; buffer BGRA32 pixelů s <code>Width</code>, <code>Height</code> a polem <code>Pixels</code>. Tento pixelový data můžete uložit do TIFF pomocí libovolné .NET knihovny pro práci s obrazem, jako je <code>System.Drawing</code>, <code>SkiaSharp</code> nebo <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vykreslit DICOM na pixelová data v C#">}}

<p>Načtěte DICOM soubor, vykreslete požadovaný snímek a získejte přístup k pixelovým datům:</p>

<div class="codeblock" id="code">
 <h3>Vykreslit DICOM obrázek - C#</h3>
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

<p>Metoda <code>RenderImage</code> automaticky použije hodnoty okna/úrovně a parametry LUT uložené v DICOM datasetu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Uložit vykreslený obrázek jako TIFF">}}

<p><code>IRawImage</code> poskytuje surová BGRA32 pixelová data, která lze uložit do TIFF pomocí libovolné .NET knihovny pro práci s obrazem. Zde je příklad s využitím <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Uložit vykreslený DICOM snímek jako TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Více‑stránkový TIFF z více‑snímkového DICOM">}}

<p>Více‑snímkové DICOM soubory (např. série CT nebo MRI) obsahují několik obrazových řezů v jednom souboru. Možnost více stránek v TIFF umožňuje uložit všechny snímky do jediného výstupního souboru, čímž zachová celý studijní záznam v univerzálně čitelném formátu. Použijte TIFF enkodér <code>System.Drawing</code> s parametry pro více snímků:</p>

<div class="codeblock" id="code">
 <h3>Převést více‑snímkový DICOM na více‑stránkový TIFF - C#</h3>
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

<p>Můžete také uložit každý snímek jako samostatný TIFF soubor, pokud jsou potřeba jednotlivé řezy:</p>

<div class="codeblock" id="code">
 <h3>Převést každý snímek na samostatný TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Vlastní okno/úroveň">}}

<p>Ovládejte, jaké rozpětí pixelových hodnot se vykreslí nastavením středu okna (Window Center) a šířky okna (Window Width). Různá nastavení okna odhalí různé anatomické struktury ze stejných DICOM dat:</p>

<div class="codeblock" id="code">
 <h3>Vykreslit s vlastním oknem/úrovní - C#</h3>
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

<p>Třída <code>GrayscaleRenderOptions</code> také poskytuje vlastnosti <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> a <code>BitDepth</code> pro úplnou kontrolu nad vykreslovací pipeline.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vykreslování více‑snímkového DICOM">}}

<p>DICOM soubory z CT, MRI a ultrazvuku často obsahují více snímků (řezů). Vykreslete všechny snímky do pixelových dat:</p>

<div class="codeblock" id="code">
 <h3>Vykreslit všechny snímky z více‑snímkového DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Ovládání overlay">}}

<p>DICOM obrázky mohou obsahovat overlay roviny s anotacemi, měřeními nebo oblastmi zájmu. Ovládejte viditelnost a barvu overlay při vykreslování:</p>

<div class="codeblock" id="code">
 <h3>Vykreslit s a bez overlay - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG pro lékařské snímky">}}

<p>Každý výstupní formát slouží různým účelům:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Charakteristika</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Komprese</strong></td><td>Bezztrátová (LZW, ZIP) nebo nekomprimovaná</td><td>Bezztrátová</td><td>Ztrátová</td></tr>
<tr><td><strong>Více‑stránkový</strong></td><td>Podporováno &mdash; všechny snímky v jednom souboru</td><td>Není podporováno</td><td>Není podporováno</td></tr>
<tr><td><strong>Kvalita obrazu</strong></td><td>Pixel‑dokonalá</td><td>Pixel‑dokonalá</td><td>Možné artefakty komprese</td></tr>
<tr><td><strong>Velikost souboru</strong></td><td>Největší</td><td>Střední</td><td>Nejmenší</td></tr>
<tr><td><strong>Průhlednost</strong></td><td>Podporováno</td><td>Podporováno</td><td>Není podporováno</td></tr>
<tr><td><strong>Ideální pro</strong></td><td>Archivace, série s více snímky, tisk, výzkum</td><td>Diagnostické prohlížení, web s kvalitou</td><td>Webové sdílení, miniatury</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET používá stejnou renderovací pipeline pro všechna výstupní cíle. Pixelová data <code>IRawImage</code> jsou identická bez ohledu na cílový formát — volba formátu ovlivní pouze poslední krok ukládání provedený vaší knihovnou pro práci s obrazem. TIFF je preferovaný formát, když potřebujete zachovat kompletní více‑snímkovou DICOM studii v jednom souboru nebo když je výstup určen pro archivaci, tisk či další zpracování obrazu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Vzdělávací zdroje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Zdrojový kód" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Reference API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Podpora produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Bezplatná podpora" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Placená podpora" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Proč Aspose.Medical pro .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Seznam zákazníků" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Úspěšné příběhy" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
