---
title: Konwertuj DICOM na JPG w C# .NET | Aspose.Medical
weight: 7000
description: Renderuj obrazy DICOM do danych pikseli w C# .NET z kontrolą okna/poziomu, renderowaniem nakładek i obsługą wieloklatkową. Użyj pełnego potoku DICOM LUT z API Aspose.Medical i zapisz jako JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konwertuj DICOM na JPG w .NET C#" h2="Renderuj obrazy DICOM do surowych danych pikseli z pełnym wsparciem potoku odcieni szarości, w tym okna/poziomu, Modality LUT, VOI LUT oraz renderowaniem nakładek. Zapisz jako JPG używając dowolnej biblioteki .NET do obrazowania." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Potok renderowania obrazu DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> renderuje obrazy DICOM poprzez standardowy potok odcieni szarości DICOM określony w specyfikacji DICOM. Proces renderowania stosuje łańcuch tabel Look‑Up Tables (LUT), aby przekształcić surowe przechowywane wartości pikseli w obrazy wyświetlane:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; konwertuje przechowywane wartości pikseli na wartości niezależne od producenta, używając Rescale Slope/Intercept lub sekwencji LUT.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; stosuje okno/poziom (Window Center i Window Width) w celu wybrania zakresu wartości do wyświetlenia, z obsługą transformacji Linear, Linear Exact i Sigmoid.</li>
<li><strong>Presentation LUT</strong> &mdash; mapuje końcowe wartości na wyświetlane P‑Values, w tym obsługę INVERSE (odwrócenie obrazu).</li>
<li><strong>Output LUT</strong> &mdash; konwertuje wartości odcieni szarości na RGB do wyświetlenia, z opcjonalną koloryzacją przy użyciu niestandardowych map kolorów lub tabel Palette Color.</li>
</ol>

<p>Wynik renderowania to <code>IRawImage</code> &mdash; bufor pikseli BGRA32 (8‑bit na kanał) z <code>Width</code>, <code>Height</code> oraz tablicą <code>Pixels</code>. Następnie możesz zapisać te dane pikseli jako JPG używając dowolnej biblioteki .NET do obrazowania, takiej jak <code>System.Drawing</code>, <code>SkiaSharp</code> lub <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderuj DICOM do danych pikseli w C#">}}

<p>Wczytaj plik DICOM i renderuj klatkę do <code>IRawImage</code> zawierającego dane pikseli BGRA32:</p>

<div class="codeblock" id="code">
 <h3>Renderuj obraz DICOM - C#</h3>
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

<p>Metoda <code>RenderImage</code> automatycznie stosuje pełny potok odcieni szarości wykorzystując wartości okna/poziomu oraz parametry LUT zapisane w pliku DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zapisz renderowany obraz jako JPG">}}

<p><code>IRawImage</code> dostarcza surowe dane pikseli BGRA32, które można zapisać jako JPG używając dowolnej biblioteki .NET do obrazowania. Oto przykład z użyciem <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Zapisz renderowaną klatkę DICOM jako JPG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Niestandardowe okno/poziom (windowing)">}}

<p>Window/level (zwany także windowing lub W/L) jest najważniejszym parametrem renderowania obrazu DICOM. Kontroluje, który zakres wartości pikseli jest mapowany na widoczny zakres odcieni szarości. <strong>Window Center</strong> definiuje punkt środkowy, a <strong>Window Width</strong> określa zakres wyświetlanych wartości:</p>

<ul>
<li><strong>Wąskie okno</strong> &mdash; wysokie kontrasty, wyświetlane mniej poziomów szarości (przydatne dla miękkich tkanek).</li>
<li><strong>Szerokie okno</strong> &mdash; niższy kontrast, wyświetlane więcej poziomów szarości (przydatne dla kości lub płuc).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Renderuj z niestandardowym oknem/poziomem - C#</h3>
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

<p>Typowe ustawienia predefiniowanych okien CT:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Typ tkanki</th>
<th>Środek okna</th>
<th>Szerokość okna</th>
</tr>
</thead>
<tbody>
<tr><td>Miękka tkanka</td><td>40</td><td>400</td></tr>
<tr><td>Płuco</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Kość</td><td>400</td><td>1800</td></tr>
<tr><td>Mózg</td><td>40</td><td>80</td></tr>
<tr><td>Wątroba</td><td>60</td><td>150</td></tr>
<tr><td>Śródpiersie</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opcje renderowania odcieni szarości">}}

<p>Klasa <code>GrayscaleRenderOptions</code> zapewnia pełną kontrolę nad potokiem renderowania odcieni szarości:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Właściwość</th>
<th>Opis</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Środek wyświetlanego zakresu wartości (poziomy punkt środkowy VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Szerokość wyświetlanego zakresu wartości (kontroluje kontrast)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Współczynnik skali Modality LUT &mdash; mnożony przez przechowywane wartości pikseli</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Wyraz wolny skali Modality LUT &mdash; dodawany po mnożeniu współczynnika</td></tr>
<tr><td><code>Invert</code></td><td>Odwraca obraz odcieni szarości (stosuje kształt INVERSE Presentation LUT)</td></tr>
<tr><td><code>BitDepth</code></td><td>Informacje o głębokości bitowej: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Renderowanie odwróconych odcieni szarości - C#</h3>
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

<p>Użyj <code>RenderOptionsFactory.Create(pixelData)</code>, aby automatycznie wypełnić opcje renderowania na podstawie atrybutów danych pikseli zestawu danych DICOM, w tym głębokości bitowej, parametrów skali, okna/poziomu oraz ustawień odwrócenia.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderowanie nakładek">}}

<p>Obrazy DICOM mogą zawierać płaszczyzny nakładek &mdash; grafiki lub adnotacje tekstowe wtopione w obraz. Klasa <code>RenderOptions</code> kontroluje, czy nakładki są renderowane i w jakim kolorze są wyświetlane:</p>

<div class="codeblock" id="code">
 <h3>Renderuj z kontrolą nakładek - C#</h3>
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

<p>DICOM obsługuje dwa typy nakładek: nakładki <strong>Graphics</strong> (adnotacje, pomiary) oraz nakładki <strong>Region of Interest</strong> (ROI). Oba typy są sterowane ustawieniem <code>ShowOverlays</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderowanie DICOM wieloklatkowego">}}

<p>Wiele plików DICOM z CT, MRI i ultrasonografii zawiera wiele klatek (przekrojów). Możesz sprawdzić liczbę klatek i renderować każdą z osobna:</p>

<div class="codeblock" id="code">
 <h3>Renderuj wszystkie klatki z wieloklatkowego DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Typy transformacji VOI LUT">}}

<p>Standard DICOM definiuje kilka funkcji transformacji VOI LUT, które kontrolują, jak stosowane jest mapowanie okna/poziomu. Aspose.Medical for .NET obsługuje wszystkie standardowe typy:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Typ</th>
<th>Opis</th>
<th>Zastosowanie</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Standardowe mapowanie liniowe z przesunięciem od środka</td><td>Najczęstsze, używane w typowej tomografii CT/MR</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Mapowanie liniowe bez przesunięcia od środka</td><td>Gdy wymagana jest precyzyjna konwersja wartości pikseli</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>Transformacja krzywej S dla łagodniejszych przejść kontrastu</td><td>Wizualizacja tkanek miękkich, mammografia</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>Niestandardowy LUT zdefiniowany jako tabela lookup w zestawie danych DICOM</td><td>Krzywe wyświetlania specyficzne dla producenta lub modalności</td></tr>
</tbody>
</table>

<p>Silnik renderujący automatycznie wybiera odpowiednią transformację VOI LUT na podstawie atrybutów zestawu danych DICOM. Przy użyciu niestandardowych <code>GrayscaleRenderOptions</code>, domyślnie stosowana jest transformacja Linear.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Obsługiwane modalności">}}

<p>Aspose.Medical for .NET obsługuje renderowanie obrazów DICOM ze wszystkich typowych modalności medycznego obrazowania:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modalność</th>
<th>Opis</th>
<th>Typowy format</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Tomografia komputerowa</td><td>Odcienie szarości, wieloklatkowe, 12&ndash;16 bit</td></tr>
<tr><td><strong>MR</strong></td><td>Rezonans magnetyczny</td><td>Odcienie szarości, wieloklatkowe, 12&ndash;16 bit</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Radiografia komputerowa / cyfrowa</td><td>Odcienie szarości, pojedyncza klatka, 10&ndash;16 bit</td></tr>
<tr><td><strong>US</strong></td><td>Ultrasonografia</td><td>Odcienie szarości lub RGB, wieloklatkowe</td></tr>
<tr><td><strong>MG</strong></td><td>Mammografia</td><td>Odcienie szarości, wysoka rozdzielczość, 12&ndash;16 bit</td></tr>
<tr><td><strong>XA</strong></td><td>Angiografia rentgenowska</td><td>Odcienie szarości, wieloklatkowe</td></tr>
<tr><td><strong>NM</strong></td><td>Medycyna nuklearna</td><td>Odcienie szarości, wieloklatkowe</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Pozytonowa Tomografia Emisyjna)</td><td>Odcienie szarości, wieloklatkowe</td></tr>
</tbody>
</table>

<p>Obsługiwane są zarówno odcienie szarości (MONOCHROME1/MONOCHROME2), jak i interpretacje kolorystyczne (RGB, YBR_FULL, PALETTE COLOR). Silnik renderujący automatycznie obsługuje odpowiednią konwersję przestrzeni barw.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zasoby edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Kod źródłowy" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referencje API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Wsparcie produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Wsparcie darmowe" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Wsparcie płatne" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Dlaczego Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista klientów" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Historie sukcesu" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
