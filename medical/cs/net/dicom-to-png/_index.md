---
title: Převod DICOM na PNG v C# .NET | Aspose.Medical
weight: 11000
description: Vykreslete DICOM obrazy do pixelových dat v C# .NET s bezztrátovou kvalitou, řízením okna/úrovně a podporou více snímků. Použijte kompletní DICOM LUT pipeline s Aspose.Medical API a uložte do PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Převod DICOM na PNG v .NET C#" h2="Vykreslete DICOM obrazy do surových pixelových dat s plnou podporou šedotónové pipeline. Zachovejte kvalitu obrazu s řízením okna/úrovně, vykreslováním překryvů a zpracováním více snímků. Uložte do bezztrátového PNG pomocí libovolné .NET knihovny pro zpracování obrazu." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Převod DICOM na PNG">}}

<p><strong>Aspose.Medical for .NET</strong> vykresluje DICOM obrazy prostřednictvím standardní DICOM šedotónové pipeline (Modality LUT, VOI LUT, Presentation LUT, Output LUT) a vytváří správně okenované BGRA32 pixelové údaje. PNG je bezztrátový formát, což z něj činí preferovanou volbu, když je třeba zachovat kvalitu obrazu bez kompresních artefaktů &mdash; nezbytné pro diagnostické posouzení, archivaci a výzkumné případy.</p>

<p>Vykreslený výstup je <code>IRawImage</code> &mdash; BGRA32 pixelová vyrovnávací paměť s <code>Width</code>, <code>Height</code> a polem <code>Pixels</code>. Tento pixelový data můžete uložit do PNG pomocí libovolné .NET knihovny pro zpracování obrazu, jako je <code>System.Drawing</code>, <code>SkiaSharp</code> nebo <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vykreslit DICOM do pixelových dat v C#">}}

<p>Načtěte DICOM soubor, vykreslete požadovaný snímek a získáte pixelová data:</p>

<div class="codeblock" id="code">
 <h3>Vykreslit DICOM obraz - C#</h3>
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

<p>Metoda <code>RenderImage</code> automaticky aplikuje hodnoty okna/úrovně a parametry LUT uložené v DICOM datasetu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Uložit vykreslený obraz jako PNG">}}

<p><code>IRawImage</code> poskytuje surová BGRA32 pixelová data, která mohou být uložena do bezztrátového PNG pomocí libovolné .NET knihovny pro zpracování obrazu. Zde je příklad s <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Uložit vykreslený DICOM snímek jako PNG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Vlastní okno/úroveň">}}

<p>Ovládejte, jaký rozsah pixelových hodnot je vykreslen nastavením středu okna (Window Center) a šířky okna (Window Width). Různá nastavení okna odhalují různé anatomické struktury ze stejných DICOM dat:</p>

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
IRawImage boneImage = dicomFile.RenderImage(bone, 0);</code></pre>
</div>

<p>Třída <code>GrayscaleRenderOptions</code> také poskytuje vlastnosti <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> a <code>BitDepth</code> pro plnou kontrolu nad rendering pipeline.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vykreslování DICOM s více snímky">}}

<p>DICOM soubory z CT, MRI a ultrazvuku často obsahují více snímků (plátků). Vykreslete všechny snímky do pixelových dat:</p>

<div class="codeblock" id="code">
 <h3>Vykreslit všechny snímky z více-snímkového DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Řízení překryvu">}}

<p>DICOM obrazy mohou obsahovat překryvné roviny s anotacemi, měřeními nebo oblastmi zájmu. Ovládejte viditelnost a barvu překryvu při vykreslování:</p>

<div class="codeblock" id="code">
 <h3>Vykreslit s a bez překryvů - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG pro medicínské obrazy">}}

<p>Volba mezi PNG a JPG závisí na konkrétním případu použití:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Charakteristika</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Komprese</strong></td><td>Bezztrátová</td><td>Ztrátová</td></tr>
<tr><td><strong>Kvalita obrazu</strong></td><td>Pixelově dokonalá &mdash; bez artefaktů</td><td>Možné menší kompresní artefakty</td></tr>
<tr><td><strong>Velikost souboru</strong></td><td>Větší (obvykle 2&ndash;10x oproti JPG)</td><td>Menší</td></tr>
<tr><td><strong>Průhlednost</strong></td><td>Podporována (alpha kanál)</td><td>Není podporována</td></tr>
<tr><td><strong>Nejvhodnější pro</strong></td><td>Diagnostické posouzení, archivace, výzkum, překryvy</td><td>Webové sdílení, miniatury, náhledy</td></tr>
<tr><td><strong>Regulační</strong></td><td>Preferováno pro workflow s kritickou kvalitou</td><td>Přijatelné pro ne-diagnostické použití</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET používá stejnou rendering pipeline pro oba výstupní cíle. Pixelová data <code>IRawImage</code> jsou identická bez ohledu na cílový formát — volba formátu ovlivní jen poslední krok uložení prováděný vaší knihovnou pro zpracování obrazu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Výukové zdroje" tabId="resources" >}}
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
