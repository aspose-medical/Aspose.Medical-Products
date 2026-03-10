---
title: DICOM átalakítása JPG-re C# .NET-ben | Aspose.Medical
weight: 7000
description: DICOM képek renderelése pixel adatokra C# .NET-ben, ablak/szint vezérléssel, overlay rendereléssel és többkeretes támogatással. Használja a teljes DICOM LUT csővezetékét az Aspose.Medical API-val, és mentse JPG formátumba.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM konvertálása JPG-re .NET C#-ban" h2="DICOM képek renderelése nyers pixel adatokra a teljes szürkeárnyalatos csővezeték támogatásával, beleértve az ablak/szint, a modality LUT, a VOI LUT és az overlay renderelést. Mentse JPG-be bármely .NET képkönyvtár használatával." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM Kép Renderelési Csővezeték">}}

<p><strong>Aspose.Medical for .NET</strong> rendereli a DICOM képeket a DICOM specifikációban meghatározott szabványos DICOM szürkeárnyalatos csővezetékön keresztül. A renderelési folyamat egy Look-Up Table (LUT) láncot alkalmaz, amely a nyers tárolt pixel értékeket megjeleníthető képekké alakítja:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; átkonvertálja a tárolt pixel értékeket gyártófüggetlen értékekké a Rescale Slope/Intercept vagy egy LUT sorozat használatával.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; alkalmaz ablak/szint (Window Center és Window Width) beállítást a megjelenítendő értéktartomány kiválasztásához, támogatva a Linear, Linear Exact és Sigmoid transzformációkat.</li>
<li><strong>Presentation LUT</strong> &mdash; leképezi a végső értékeket megjeleníthető P-Value-kra, beleértve az INVERSE (kép invertálás) támogatását.</li>
<li><strong>Output LUT</strong> &mdash; konvertálja a szürkeárnyalatos értékeket RGB-re a megjelenítéshez, opcionális színezéssel egyedi színpályák vagy Palette Color táblák használatával.</li>
</ol>

<p>A renderelt kimenet egy <code>IRawImage</code> &mdash; egy BGRA32 pixelpuffer (8-bit csatornánként) <code>Width</code>, <code>Height</code> és egy <code>Pixels</code> tömbbel. Ezután a pixel adatokat JPG-re mentheti bármely .NET képkönyvtárral, például <code>System.Drawing</code>, <code>SkiaSharp</code> vagy <code>ImageSharp</code> segítségével.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM renderelése pixel adatokra C#-ban">}}

<p>Töltsön be egy DICOM fájlt, és rendereljen egy keretet egy <code>IRawImage</code> objektumba, amely BGRA32 pixel adatokat tartalmaz:</p>

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

<p>A <code>RenderImage</code> metódus automatikusan alkalmazza a teljes szürkeárnyalatos csővezetéket a DICOM fájlban tárolt ablak/szint értékek és LUT paraméterek felhasználásával.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderelt kép mentése JPG-ként">}}

<p>Az <code>IRawImage</code> nyers BGRA32 pixel adatokat biztosít, amelyeket bármely .NET képkönyvtárral JPG-re menthet. Itt egy példa a <code>System.Drawing</code> használatával:</p>

<div class="codeblock" id="code">
 <h3>Renderelt DICOM keret mentése JPG-ként - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Egyéni ablak/szint (Windowing)">}}

<p>Az ablak/szint (más néven windowing vagy W/L) a legfontosabb paraméter a DICOM képrenderelésnél. Ez szabályozza, hogy a pixelértékek mely tartománya legyen leképezve a látható szürkeárnyalatos tartományra. A <strong>Window Center</strong> a középpontot, a <strong>Window Width</strong> pedig a megjelenített értéktartományt határozza meg:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; magas kontraszt, kevesebb szürke szint jelenik meg (hasznos puha szövet esetén).</li>
<li><strong>Wide window</strong> &mdash; alacsonyabb kontraszt, több szürke szint jelenik meg (hasznos csont vagy tüdő esetén).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Renderelés egyéni ablak/szint használatával - C#</h3>
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

<p>Gyakori CT ablak előbeállítások:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Szövet típusa</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Puha szövet</td><td>40</td><td>400</td></tr>
<tr><td>Tüdő</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Csont</td><td>400</td><td>1800</td></tr>
<tr><td>Agy</td><td>40</td><td>80</td></tr>
<tr><td>Máj</td><td>60</td><td>150</td></tr>
<tr><td>Mediastinum</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Szürkeárnyalatos renderelési beállítások">}}

<p>A <code>GrayscaleRenderOptions</code> osztály teljes irányítást biztosít a szürkeárnyalatos renderelési csővezeték felett:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tulajdonság</th>
<th>Leírás</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>A megjelenített értéktartomány középpontja (a VOI LUT vízszintes középpontja)</td></tr>
<tr><td><code>WindowWidth</code></td><td>A megjelenített értéktartomány szélessége (a kontrasztot szabályozza)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Modality LUT átskálázási meredekség &mdash; a tárolt pixel értékekkel szorzva</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Modality LUT átskálázási eltolás &mdash; a meredekség szorzása után hozzáadva</td></tr>
<tr><td><code>Invert</code></td><td>Inverziót alkalmaz a szürkeárnyalatos képen (alkalmazza az INVERSE Presentation LUT formát)</td></tr>
<tr><td><code>BitDepth</code></td><td>Bitmélység információ: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Inverz szürkeárnyalatos renderelés - C#</h3>
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

<p>Használja a <code>RenderOptionsFactory.Create(pixelData)</code> metódust a renderelési beállítások automatikus feltöltéséhez a DICOM adathalmaz pixeladat-attribútumaiból, beleértve a bitmélységet, átskálázási paramétereket, ablak/szint és inverterezési beállításokat.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Overlay renderelés">}}

<p>A DICOM képek tartalmazhatnak overlay síkokat &mdash; a képre égetett grafikai vagy szöveges annotációkat. A <code>RenderOptions</code> osztály szabályozza, hogy az overlay-k meg legyenek renderelve, és milyen színben jelenjenek meg:</p>

<div class="codeblock" id="code">
 <h3>Renderelés overlay vezérléssel - C#</h3>
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

<p>A DICOM két overlay típust támogat: <strong>Graphics</strong> overlay-k (annotációk, mérőszámok) és <strong>Region of Interest</strong> (ROI) overlay-k. Mindkét típust a <code>ShowOverlays</code> beállítás szabályozza.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Többkeretes DICOM renderelés">}}

<p>Sok CT, MRI és ultrahang DICOM fájl több keretet (szeletet) tartalmaz. Ellenőrizheti a keretek számát, és minden egyes keretet külön-külön renderelhet:</p>

<div class="codeblock" id="code">
 <h3>Az összes keret renderelése többkeretes DICOM-ból - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="VOI LUT transzformáció típusok">}}

<p>A DICOM szabvány több VOI LUT transzformációs függvényt definiál, amelyek szabályozzák, hogy az ablak/szint leképezés hogyan történik. Az Aspose.Medical for .NET támogatja az összes szabványos típust:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Típus</th>
<th>Leírás</th>
<th>Alkalmazási példa</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Szabványos lineáris leképezés a középről eltolással</td><td>Leggyakoribb, általános CT/MR képezésnél használatos</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Lineáris leképezés középről eltolás nélkül</td><td>Amikor pontos pixelérték leképezés szükséges</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>S-görbe transzformáció a simább kontrasztátmenetekért</td><td>Puha szövet ábrázolás, mammográfia</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>Egyedi LUT, amely a DICOM adathalmazban lookup table-ként definiált</td><td>Gyártó-specifikus vagy modality-specifikus megjelenítési görbék</td></tr>
</tbody>
</table>

<p>A renderelő motor automatikusan kiválasztja a megfelelő VOI LUT transzformációt a DICOM adathalmaz attribútumai alapján. Egyedi <code>GrayscaleRenderOptions</code> használata esetén a Linear transzformáció alkalmazásra kerül alapértelmezés szerint.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Támogatott modalitások">}}

<p>Az Aspose.Medical for .NET támogatja a DICOM képek renderelését az összes gyakori orvosi képalkotó modalitásból:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modalitás</th>
<th>Leírás</th>
<th>Tipikus formátum</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Számítógépes tomográfia</td><td>Szürkeárnyalatos, többkeretes, 12–16 bit</td></tr>
<tr><td><strong>MR</strong></td><td>Mágneses rezonancia</td><td>Szürkeárnyalatos, többkeretes, 12–16 bit</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Számított / Digitális radiográfia</td><td>Szürkeárnyalatos, egykeretes, 10–16 bit</td></tr>
<tr><td><strong>US</strong></td><td>Ultrahang</td><td>Szürkeárnyalatos vagy RGB, többkeretes</td></tr>
<tr><td><strong>MG</strong></td><td>Mammográfia</td><td>Szürkeárnyalatos, nagy felbontású, 12–16 bit</td></tr>
<tr><td><strong>XA</strong></td><td>Röntgen angiográfia</td><td>Szürkeárnyalatos, többkeretes</td></tr>
<tr><td><strong>NM</strong></td><td>Nukleáris medicina</td><td>Szürkeárnyalatos, többkeretes</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Pozitron emissziós tomográfia)</td><td>Szürkeárnyalatos, többkeretes</td></tr>
</tbody>
</table>

<p>Mind a szürkeárnyalatos (MONOCHROME1/MONOCHROME2), mind a színes (RGB, YBR_FULL, PALETTE COLOR) fotometrikus értelmezések támogatottak. A renderelő motor automatikusan kezeli a megfelelő színtér átalakítást.</p>

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
{{< blocks/products/pf/slr-element name="Fizetett támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért az Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
