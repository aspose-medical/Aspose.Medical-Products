---
title: Konwertuj DICOM do PNG w C# .NET | Aspose.Medical
weight: 11000
description: Renderuj obrazy DICOM do danych pikselowych w C# .NET z bezstratną jakością, kontrolą okna/poziomu i obsługą wieloklatkową. Użyj pełnego potoku DICOM LUT z API Aspose.Medical i zapisz jako PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konwertuj DICOM do PNG w .NET C#" h2="Renderuj obrazy DICOM do surowych danych pikselowych z pełnym wsparciem potoku skali szarości. Zachowaj jakość obrazu dzięki kontroli okna/poziomu, renderowaniu nakładek i przetwarzaniu wieloklatkowym. Zapisz jako bezstratny PNG przy użyciu dowolnej biblioteki obrazowania .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Konwersja DICOM do PNG">}}

<p><strong>Aspose.Medical for .NET</strong> renderuje obrazy DICOM poprzez standardowy potok skali szarości DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT), aby uzyskać prawidłowo okienkowane dane pikselowe BGRA32. PNG jest formatem bezstratnym, co czyni go preferowanym wyborem, gdy jakość obrazu musi być zachowana bez artefaktów kompresji &mdash; istotne dla przeglądu diagnostycznego, archiwizacji i badań.</p>

<p>Renderowany wynik jest obiektem <code>IRawImage</code> &mdash; buforem pikseli BGRA32 z <code>Width</code>, <code>Height</code> oraz tablicą <code>Pixels</code>. Możesz zapisać te dane pikselowe jako PNG przy użyciu dowolnej biblioteki obrazowania .NET, takiej jak <code>System.Drawing</code>, <code>SkiaSharp</code> lub <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderuj DICOM do danych pikselowych w C#">}}

<p>Wczytaj plik DICOM, wyrenderuj żądaną klatkę i uzyskaj dostęp do danych pikselowych:</p>

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

<p>Metoda <code>RenderImage</code> automatycznie stosuje wartości okna/poziomu oraz parametry LUT zapisane w zestawie danych DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zapisz wyrenderowany obraz jako PNG">}}

<p>Obiekt <code>IRawImage</code> dostarcza surowe dane pikselowe BGRA32, które mogą zostać zapisane jako bezstratny PNG przy użyciu dowolnej biblioteki obrazowania .NET. Oto przykład z użyciem <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Zapisz wyrenderowaną klatkę DICOM jako PNG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Niestandardowe okno/poziom">}}

<p>Kontroluj zakres wartości pikseli renderowanych poprzez ustawienie Window Center i Window Width. Różne ustawienia okna ujawniają różne struktury anatomiczne z tych samych danych DICOM:</p>

<div class="codeblock" id="code">
 <h3>Renderuj z niestandardowym oknem/poziomem - C#</h3>
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

<p>Klasa <code>GrayscaleRenderOptions</code> udostępnia również właściwości <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> oraz <code>BitDepth</code> umożliwiając pełną kontrolę nad potokiem renderowania.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderowanie wieloklatkowych DICOM">}}

<p>Pliki DICOM pochodzące z CT, MRI i ultradźwięków często zawierają wiele klatek (przekrojów). Renderuj wszystkie klatki do danych pikselowych:</p>

<div class="codeblock" id="code">
 <h3>Renderuj wszystkie klatki z wieloklatkowego DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Kontrola nakładek">}}

<p>Obrazy DICOM mogą zawierać płaszczyzny nakładek z adnotacjami, pomiarami lub obszarami zainteresowania. Kontroluj widoczność i kolor nakładek podczas renderowania:</p>

<div class="codeblock" id="code">
 <h3>Renderuj z i bez nakładek - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG w obrazach medycznych">}}

<p>Wybór pomiędzy PNG a JPG zależy od scenariusza użycia:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Charakterystyka</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Kompresja</strong></td><td>Bezstratna</td><td>Stratna</td></tr>
<tr><td><strong>Jakość obrazu</strong></td><td>Idealna pod względem pikseli &mdash; bez artefaktów</td><td>Możliwe drobne artefakty kompresji</td></tr>
<tr><td><strong>Rozmiar pliku</strong></td><td>Większy (zazwyczaj 2&ndash;10 razy więcej niż JPG)</td><td>Mniejszy</td></tr>
<tr><td><strong>Przezroczystość</strong></td><td>Obsługiwana (kanał alfa)</td><td>Nieobsługiwana</td></tr>
<tr><td><strong>Najlepszy dla</strong></td><td>Przegląd diagnostyczny, archiwizacja, badania, nakładki</td><td>Udostępnianie w sieci, miniatury, podglądy</td></tr>
<tr><td><strong>Regulacje</strong></td><td>Preferowany w przepływach o krytycznej jakości</td><td>Akceptowalny w zastosowaniach nie-diagnostycznych</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET używa tego samego potoku renderowania dla obu formatów wyjściowych. Dane pikselowe <code>IRawImage</code> są identyczne niezależnie od wybranego formatu &mdash; wybór formatu wpływa jedynie na końcowy krok zapisu wykonywany przez używaną bibliotekę obrazowania.</p>

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
