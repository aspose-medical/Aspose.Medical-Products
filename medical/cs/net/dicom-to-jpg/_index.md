---
title: Převod DICOM na JPG v C# .NET | Aspose.Medical
weight: 7000
description: Vykreslete DICOM obrázky do pixelových dat v C# .NET s řízením okna/úrovně, vykreslováním překryvů a podporou více snímků. Použijte kompletní DICOM LUT pipeline s API Aspose.Medical a uložte jako JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Převod DICOM na JPG v .NET C#" h2="Vykreslete DICOM obrázky do surových pixelových dat s kompletní podporou grayscale pipeline, včetně okna/úrovně, modality LUT, VOI LUT a vykreslování překryvu. Uložte jako JPG pomocí libovolné .NET knihovny pro zpracování obrazu." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Pipeline vykreslování DICOM obrázků">}}

<p><strong>Aspose.Medical pro .NET</strong> vykresluje DICOM obrázky pomocí standardní DICOM grayscale pipeline definované v DICOM specifikaci. Proces vykreslování použije řetězec Look-Up Tables (LUT) k transformaci surových uložených pixelových hodnot na zobrazitelné obrazy:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; převádí uložené pixelové hodnoty na hodnoty nezávislé na výrobci pomocí Rescale Slope/Intercept nebo sekvence LUT.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; aplikuje okno/úroveň (Window Center a Window Width) k výběru rozsahu hodnot k zobrazení, s podporou Linear, Linear Exact a Sigmoid transformací.</li>
<li><strong>Presentation LUT</strong> &mdash; mapuje konečné hodnoty na zobrazitelné P-hodnoty, včetně podpory INVERSE (inverze obrazu).</li>
<li><strong>Output LUT</strong> &mdash; převádí grayscale hodnoty na RGB pro zobrazení, s volitelnou kolorací pomocí vlastních colormap nebo tabulek Palette Color.</li>
</ol>

<p>Výstup po vykreslení je <code>IRawImage</code> &mdash; BGRA32 pixelový buffer (8‑bit na kanál) s <code>Width</code>, <code>Height</code> a polem <code>Pixels</code>. Tento pixelový data pak můžete uložit jako JPG pomocí libovolné .NET knihovny pro práci s obrazy, např. <code>System.Drawing</code>, <code>SkiaSharp</code> nebo <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vykreslení DICOM na pixelová data v C#">}}

<p>Načtěte DICOM soubor a vykreslete snímek do <code>IRawImage</code> obsahujícího BGRA32 pixelová data:</p>

<div class="codeblock" id="code">
 <h3>Vykreslení DICOM obrazu - C#</h3>
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

<p>Metoda <code>RenderImage</code> automaticky aplikuje kompletní grayscale pipeline s využitím hodnot okna/úrovně a parametrů LUT uložených v DICOM souboru.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Uložit vykreslený obraz jako JPG">}}

<p><code>IRawImage</code> poskytuje surová BGRA32 pixelová data, která lze uložit jako JPG pomocí libovolné .NET knihovny pro zpracování obrazu. Zde je příklad s využitím <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Uložení vykresleného DICOM snímku jako JPG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Vlastní okno/úroveň (windowing)">}}

<p>Window/level (také nazývaný windowing nebo W/L) je nejdůležitějším parametrem pro vykreslování DICOM obrázků. Řídí, který rozsah pixelových hodnot je mapován do viditelného grayscale rozsahu. <strong>Window Center</strong> určuje střed a <strong>Window Width</strong> určuje rozsah zobrazovaných hodnot:</p>

<ul>
<li><strong>Úzké okno</strong> &mdash; vysoký kontrast, méně zobrazovaných stupňů šedi (užitečné pro měkké tkáně).</li>
<li><strong>Široké okno</strong> &mdash; nižší kontrast, více zobrazovaných stupňů šedi (užitečné pro kost nebo plíce).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Vykreslení s vlastním oknem/úrovní - C#</h3>
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

<p>Časté přednastavené CT okna:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Typ tkáně</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Měkká tkáň</td><td>40</td><td>400</td></tr>
<tr><td>Plíce</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Kost</td><td>400</td><td>1800</td></tr>
<tr><td>Mozek</td><td>40</td><td>80</td></tr>
<tr><td>Játra</td><td>60</td><td>150</td></tr>
<tr><td>Mediastinum</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Možnosti grayscale vykreslování">}}

<p>Třída <code>GrayscaleRenderOptions</code> poskytuje plnou kontrolu nad grayscale pipeline vykreslování:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Vlastnost</th>
<th>Popis</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Střed zobrazovaného rozsahu hodnot (horizontální střed VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Šířka zobrazovaného rozsahu hodnot (řídí kontrast)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Modality LUT rescale slope &mdash; násobí uložené pixelové hodnoty</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Modality LUT rescale intercept &mdash; přidává se po násobení slope</td></tr>
<tr><td><code>Invert</code></td><td>Inverte grayscale obraz (aplikuje INVERSE tvar Presentation LUT)</td></tr>
<tr><td><code>BitDepth</code></td><td>Informace o bitové hloubce: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Inverzní grayscale vykreslování - C#</h3>
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

<p>Použijte <code>RenderOptionsFactory.Create(pixelData)</code> k automatickému naplnění možností vykreslování z atributů pixelových dat DICOM datasetu, včetně bitové hloubky, parametrů rescale, window/level a nastavení inverze.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vykreslování překryvů">}}

<p>DICOM obrázky mohou obsahovat overlay vrstvy &mdash; grafiku nebo textové anotace vypálené do obrazu. Třída <code>RenderOptions</code> řídí, zda jsou overlay vrstvy vykresleny a v jaké barvě se zobrazí:</p>

<div class="codeblock" id="code">
 <h3>Vykreslení s řízením overlay - C#</h3>
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

<p>DICOM podporuje dva typy overlay: <strong>Graphics</strong> overlay (anotace, měření) a <strong>Region of Interest</strong> (ROI) overlay. Obě typy jsou řízeny nastavením <code>ShowOverlays</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vykreslování multi‑frame DICOM">}}

<p>Mnoho DICOM souborů z CT, MRI a ultrazvuku obsahuje více snímků (řezů). Můžete zjistit počet snímků a vykreslit každý samostatně:</p>

<div class="codeblock" id="code">
 <h3>Vykreslení všech snímků z multi‑frame DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Typy VOI LUT transformací">}}

<p>Standard DICOM definuje několik VOI LUT transformačních funkcí, které řídí aplikaci mapování window/level. Aspose.Medical pro .NET podporuje všechny standardní typy:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Typ</th>
<th>Popis</th>
<th>Případ použití</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Standardní lineární mapování s offsetem od středu</td><td>Nejčastější, používá se pro obecné CT/MR snímkování</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Lineární mapování bez offsetu od středu</td><td>Když je vyžadováno přesné mapování pixelových hodnot</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>S‑křivková transformace pro plynulejší přechody kontrastu</td><td>Vizualizace měkkých tkání, mamografie</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>Vlastní LUT definovaný jako lookup table v DICOM datasetu</td><td>Specifické display křivky pro výrobce nebo modalitu</td></tr>
</tbody>
</table>

<p>Renderovací engine automaticky vybere odpovídající VOI LUT transformaci na základě atributů DICOM datasetu. Při použití vlastních <code>GrayscaleRenderOptions</code> je výchozí aplikována Linear transformace.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Podporované modality">}}

<p>Aspose.Medical pro .NET podporuje vykreslování DICOM obrázků ze všech běžných medicínských zobrazovacích modalit:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modalita</th>
<th>Popis</th>
<th>Typický formát</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Počítačová tomografie</td><td>Grayscale, multi‑frame, 12&ndash;16 bit</td></tr>
<tr><td><strong>MR</strong></td><td>Magnetická rezonance</td><td>Grayscale, multi‑frame, 12&ndash;16 bit</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Computed / Digitální radiografie</td><td>Grayscale, single frame, 10&ndash;16 bit</td></tr>
<tr><td><strong>US</strong></td><td>Ultrazvuk</td><td>Grayscale nebo RGB, multi‑frame</td></tr>
<tr><td><strong>MG</strong></td><td>Mamografie</td><td>Grayscale, vysoké rozlišení, 12&ndash;16 bit</td></tr>
<tr><td><strong>XA</strong></td><td>X‑Ray angiografie</td><td>Grayscale, multi‑frame</td></tr>
<tr><td><strong>NM</strong></td><td>Jaderná medicína</td><td>Grayscale, multi‑frame</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Pozitronová emisní tomografie)</td><td>Grayscale, multi‑frame</td></tr>
</tbody>
</table>

<p>Jsou podporovány jak grayscale (MONOCHROME1/MONOCHROME2), tak i barevné (RGB, YBR_FULL, PALETTE COLOR) fotometrické interpretace. Renderovací engine automaticky provádí příslušnou konverzi barevného prostoru.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Výukové materiály" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Zdrojový kód" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referenční dokumentace API" href="https://reference.aspose.com/medical/net/" >}}
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
