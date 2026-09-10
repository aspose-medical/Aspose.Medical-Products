---
title: Kompresja DICOM JPEG 2000 w C# .NET | Aspose.Medical
weight: 2000
description: Odczyt, zapis i transkodowanie plików DICOM z kompresją JPEG 2000 w C# .NET. Obsługa obrazów 8‑bitowych i 16‑bitowych, trybów bezstratnych i stratnych, danych wielokomponentowych przy użyciu API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Obsługa DICOM JPEG 2000 w .NET C#" h2="Odczyt, zapis i transkodowanie plików DICOM z kompresją JPEG 2000. Tryby bezstratny i stratny, dane pikseli 8‑bitowe i 16‑bitowe, obrazy wielokomponentowe — wszystko w czystym .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 w obrazowaniu medycznym">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) jest najpowszechniej stosowanym standardem kompresji falowej w obrazowaniu medycznym. W przeciwieństwie do tradycyjnego JPEG, oferuje zarówno kompresję bezstratną, jak i stratną w jednym kodeku, progresywne dekodowanie umożliwiające dostęp do regionu zainteresowania oraz lepsze współczynniki kompresji &mdash; co czyni go idealnym do archiwizacji dużych badań i przesyłania obrazów w ograniczonych sieciach.</p>

<p><strong>Aspose.Medical for .NET</strong> zapewnia czystą implementację C# kodeka JPEG 2000 bez zależności natywnych. Biblioteka może odczytywać, renderować i transkodować pliki DICOM skompresowane dowolną z czterech standardowych składni transferu JPEG 2000.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Obsługiwane składnie transferu JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Składnia transferu</th>
<th>UID</th>
<th>Tryb</th>
<th>Odczyt</th>
<th>Zapis</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 tylko bezstratny</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Bezstratny</td><td>8‑bitowy i 16‑bitowy</td><td>8‑bitowy</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Stratny lub bezstratny</td><td>8‑bitowy i 16‑bitowy</td><td>8‑bitowy</td></tr>
<tr><td>JPEG 2000 Part 2 wielokomponentowy tylko bezstratny</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Bezstratny</td><td>8‑bitowy i 16‑bitowy</td><td>8‑bitowy</td></tr>
<tr><td>JPEG 2000 Part 2 wielokomponentowy</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Stratny lub bezstratny</td><td>8‑bitowy i 16‑bitowy</td><td>8‑bitowy</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dane pikseli 8‑bitowe i 16‑bitowe">}}

<p>Obrazy medyczne często używają 16 bitów na próbkę, aby uchwycić pełny zakres dynamiki modalności takich jak CT (zazwyczaj 12‑bitowy przechowywany w 16‑bitowym) i MRI. Aspose.Medical obsługuje oba rozdzielczości bitowe dla JPEG 2000:</p>

<ul>
<li><strong>Odczyt (dekompresja)</strong>: Pełna obsługa zarówno 8‑bitowych, jak i 16‑bitowych plików DICOM skompresowanych JPEG 2000. Biblioteka poprawnie dekoduje dane pikseli niezależnie od pierwotnych wartości Bits Allocated, Bits Stored i High Bit.</li>
<li><strong>Zapis (kompresja)</strong>: Obecnie obsługuje obrazy 8‑bitowe. Obsługa zapisu 16‑bitowego jest planowana w przyszłym wydaniu.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Odczytaj i zbadaj skompresowany JPEG 2000 DICOM - C#</h3>
 <pre><code class="cs">// Open a JPEG 2000 compressed DICOM file (8-bit or 16-bit)
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Check transfer syntax
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax
Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");

// Inspect pixel data bit depth
PixelData pixelData = PixelData.Create(dicomFile.Dataset);
Console.WriteLine($"Bits Allocated: {pixelData.BitsAllocated}");
Console.WriteLine($"Bits Stored: {pixelData.BitsStored}");
Console.WriteLine($"High Bit: {pixelData.HighBit}");
Console.WriteLine($"Samples Per Pixel: {pixelData.SamplesPerPixel}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transkoduj do JPEG 2000">}}

<p>Użyj metody <code>Transcode</code>, aby skompresować dowolny plik DICOM do JPEG 2000 lub przekonwertować pomiędzy trybami JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Kompresuj DICOM do JPEG 2000 bezstratny - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Kompresuj DICOM do JPEG 2000 stratny - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dekompresja plików DICOM JPEG 2000">}}

<p>Dekompresuj pliki JPEG 2000 do nieskompresowanej składni transferu w celu przetwarzania, analizy lub zapewnienia kompatybilności z systemami nieobsługującymi JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Dekompresuj JPEG 2000 do nieskompresowanego – C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Możesz także zdekompresować i transkodować do innych formatów kompresji w jednym kroku:</p>

<div class="codeblock" id="code">
 <h3>Transkoduj pomiędzy formatami kompresji - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderowanie obrazów DICOM JPEG 2000">}}

<p>Pliki DICOM skompresowane JPEG 2000 mogą być renderowane do danych pikseli w celu wyświetlenia lub eksportu, tak jak każda inna składnia transferu:</p>

<div class="codeblock" id="code">
 <h3>Renderuj skompresowaną ramkę JPEG 2000 - C#</h3>
 <pre><code class="cs">// Open JPEG 2000 DICOM file
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Render the first frame — decompression is handled automatically
using PixelImage&lt;Bgra32&gt; image = dicomFile.RenderImage(0);

// Copy the BGRA32 pixel data for display or export
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = new Bgra32[width * height];
image.CopyPixelsTo(pixels);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bezstratny vs stratny JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Aspekt</th>
<th>JPEG 2000 bezstratny</th>
<th>JPEG 2000 stratny</th>
</tr>
</thead>
<tbody>
<tr><td>Składnia transferu</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Jakość obrazu</td><td>Pixel-perfect &mdash; identyczna z oryginałem</td><td>Podobna wizualnie, pewne dane trwale utracone</td></tr>
<tr><td>Współczynnik kompresji</td><td>Zazwyczaj 2:1 do 3:1</td><td>Zazwyczaj 10:1 do 30:1 lub wyższy</td></tr>
<tr><td>Najlepszy dla</td><td>Archiwizacja diagnostyczna, dokumentacja prawna, wstępna diagnostyka</td><td>Wstępny przegląd, telemedycyna, przesyłanie w sieci</td></tr>
<tr><td>Bezpieczny przy dwukierunkowym przetwarzaniu</td><td>Tak</td><td>Nie &mdash; ponowne kodowanie dodatkowo pogarsza jakość</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 wielokomponentowy">}}

<p>JPEG 2000 Part 2 (ISO/IEC 15444-2) rozszerza standardowy kodek o możliwości transformacji wielokomponentowej. Jest używany w kolorowych obrazach medycznych oraz modalnościach generujących dane wielokanałowe. Aspose.Medical obsługuje obie składnie transferu Part 2:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; bezstratna kompresja z dekorelacją między komponentami dla optymalnej kompresji danych wielokanałowych.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; kompresja stratna lub bezstratna z transformacjami wielokomponentowymi.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K) — wkrótce">}}

<p>HTJ2K (ISO/IEC 15444-15) jest rozszerzeniem nowej generacji JPEG 2000 zaprojektowanym do znacznie szybszego kodowania i dekodowania przy zachowaniu tej samej efektywności kompresji. Oczekuje się, że stanie się preferowanym kodekiem w przepływach pracy obrazowania medycznego w czasie rzeczywistym.</p>

<p>Aspose.Medical doda wsparcie HTJ2K w przyszłym wydaniu, obejmujące trzy składnie transferu:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; tylko bezstratny</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; bezstratny z kolejnością progresji RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; stratny lub bezstratny</li>
</ul>

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

{{< blocks/products/pf/slr-tab tabTitle="Dlaczego Aspose.Medical dla .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista klientów" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Historie sukcesu" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
