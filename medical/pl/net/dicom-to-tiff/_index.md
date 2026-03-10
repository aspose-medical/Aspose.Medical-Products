---
title: Konwertuj DICOM do TIFF w C# .NET | Aspose.Medical
weight: 12000
description: Renderuj obrazy DICOM do danych pikselowych w C# .NET i zapisz do TIFF z obsługą wielu stron dla wieloklatkowych plików DICOM. Użyj pełnego pipeline’u DICOM LUT z API Aspose.Medical i zapisz do TIFF przy użyciu dowolnej biblioteki obrazu .NET.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konwertuj DICOM do TIFF w .NET C#" h2="Renderuj obrazy DICOM do surowych danych pikselowych z pełnym wsparciem pipeline’u w skali szarości. Zachowaj jakość obrazu dzięki kontroli window/level, renderowaniu overlay oraz przetwarzaniu wieloklatkowym. Zapisz do TIFF &mdash; w tym wielostronicowego TIFF &mdash; przy użyciu dowolnej biblioteki obrazu .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Konwersja DICOM do TIFF">}}

<p><strong>Aspose.Medical for .NET</strong> renderuje obrazy DICOM poprzez standardowy pipeline DICOM w skali szarości (Modality LUT, VOI LUT, Presentation LUT, Output LUT), aby uzyskać prawidłowo wyświetlone dane pikselowe BGRA32. TIFF jest wszechstronnym formatem szeroko stosowanym w obrazowaniu medycznym, badaniach naukowych i publikacjach, ponieważ obsługuje kompresję bezstratną, wysoką głębię bitową oraz &mdash; co najważniejsze &mdash; <strong>dokumenty wielostronicowe</strong>. Dzięki temu TIFF jest naturalnym wyborem do konwertowania wieloklatkowych plików DICOM (CT, MRI, ultradźwięki) do jednego pliku wyjściowego, który zachowuje wszystkie przekroje.</p>

<p>Renderowany wynik to <code>IRawImage</code> &mdash; bufor pikseli BGRA32 zawierający <code>Width</code>, <code>Height</code> oraz tablicę <code>Pixels</code>. Możesz zapisać te dane pikselowe do TIFF przy użyciu dowolnej biblioteki obrazu .NET, takiej jak <code>System.Drawing</code>, <code>SkiaSharp</code> lub <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderuj DICOM do danych pikselowych w C#">}}

<p>Wczytaj plik DICOM, renderuj wybraną klatkę i uzyskaj dostęp do danych pikselowych:</p>

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

<p>Metoda <code>RenderImage</code> automatycznie stosuje wartości window/level oraz parametry LUT zapisane w zestawie danych DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zapisz renderowany obraz jako TIFF">}}

<p><code>IRawImage</code> dostarcza surowe dane pikselowe BGRA32, które mogą być zapisane do TIFF przy użyciu dowolnej biblioteki obrazu .NET. Oto przykład z użyciem <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Zapisz renderowaną klatkę DICOM jako TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Wielostronicowy TIFF z wieloklatkowego DICOM">}}

<p>Pliki DICOM wieloklatkowe (np. serie CT lub MRI) zawierają wiele przekrojów obrazu w jednym pliku. Możliwość wielostronicowa formatu TIFF pozwala przechowywać wszystkie klatki w jednym pliku wyjściowym, zachowując pełne badanie w uniwersalnym formacie czytelnym. Użyj enkodera TIFF z <code>System.Drawing</code> z parametrami wieloklatkowymi:</p>

<div class="codeblock" id="code">
 <h3>Konwertuj wieloklatkowy DICOM na wielostronicowy TIFF - C#</h3>
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

<p>Możesz również zapisać każdą klatkę jako oddzielny plik TIFF, gdy potrzebne są pojedyncze przekroje:</p>

<div class="codeblock" id="code">
 <h3>Konwertuj każdą klatkę na oddzielny TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Niestandardowe Window/Level">}}

<p>Kontroluj zakres renderowanych wartości pikseli, ustawiając Window Center i Window Width. Różne ustawienia okna uwidaczniają różne struktury anatomiczne w tych samych danych DICOM:</p>

<div class="codeblock" id="code">
 <h3>Renderuj z niestandardowym window/level - C#</h3>
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

<p>Klasa <code>GrayscaleRenderOptions</code> udostępnia także właściwości <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> oraz <code>BitDepth</code> umożliwiając pełną kontrolę nad pipeline’em renderowania.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderowanie wieloklatkowego DICOM">}}

<p>Pliki DICOM z CT, MRI i ultradźwięków często zawierają wiele klatek (przekrojów). Renderuj wszystkie klatki do danych pikselowych:</p>

<div class="codeblock" id="code">
 <h3>Renderuj wszystkie klatki z wieloklatkowego DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Kontrola overlay">}}

<p>Obrazy DICOM mogą zawierać płaszczyzny overlay z adnotacjami, pomiarami lub regionami zainteresowania. Kontroluj widoczność i kolor overlay podczas renderowania:</p>

<div class="codeblock" id="code">
 <h3>Renderuj z i bez overlay - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG dla obrazów medycznych">}}

<p>Każdy format wyjściowy służy innym zastosowaniom:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Cecha</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Kompresja</strong></td><td>Bezstratna (LZW, ZIP) lub nieskompresowana</td><td>Bezstratna</td><td>Stratna</td></tr>
<tr><td><strong>Wielostronicowy</strong></td><td>Obsługiwane &mdash; wszystkie klatki w jednym pliku</td><td>Nieobsługiwane</td><td>Nieobsługiwane</td></tr>
<tr><td><strong>Jakość obrazu</strong></td><td>Pixel-perfect</td><td>Pixel-perfect</td><td>Możliwe artefakty kompresji</td></tr>
<tr><td><strong>Rozmiar pliku</strong></td><td>Największy</td><td>Średni</td><td>Najmniejszy</td></tr>
<tr><td><strong>Transparentność</strong></td><td>Obsługiwane</td><td>Obsługiwane</td><td>Nieobsługiwane</td></tr>
<tr><td><strong>Najlepszy do</strong></td><td>Archiwizacja, serie wieloklatkowe, druk, badania</td><td>Przegląd diagnostyczny, sieć z wysoką jakością</td><td>Udostępnianie w sieci, miniatury</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET używa tego samego pipeline’u renderowania dla wszystkich celów wyjściowych. Dane pikselowe <code>IRawImage</code> są identyczne niezależnie od formatu docelowego &mdash; wybór formatu wpływa jedynie na końcowy krok zapisu wykonywany przez Twoją bibliotekę obrazu. TIFF jest preferowanym formatem, gdy potrzebujesz zachować pełne wieloklatkowe badanie DICOM w jednym pliku lub gdy wyjście jest przeznaczone do archiwizacji, druku lub dalszego przetwarzania obrazu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zasoby edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Kod źródłowy" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referencje API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Wsparcie produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Darmowe wsparcie" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Płatne wsparcie" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Dlaczego Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista klientów" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Historie sukcesu" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
