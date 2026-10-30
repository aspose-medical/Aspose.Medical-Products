---
title: Kompresja DICOM JPEG 2000 w C# .NET | Aspose.Medical
weight: 2000
description: Odczyt, zapis i transkodowanie plików DICOM z kompresją JPEG 2000 w C# .NET. Obsługa obrazów monochromatycznych i kolorowych od 1 do 16 bitów, trybów bezstratnego i stratnego oraz HTJ2K z API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Obsługa DICOM JPEG 2000 w .NET C#" h2="Odczyt, zapis i transkodowanie plików DICOM z kompresją JPEG 2000. Tryby bezstratny i stratny, dane pikseli monochromatyczne i kolorowe od 1 do 16 bitów, w tym HTJ2K – wszystko w czystym .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 w obrazowaniu medycznym">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) jest najpowszechniej stosowanym standardem kompresji opartym na falach w obrazowaniu medycznym. W przeciwieństwie do tradycyjnego JPEG, oferuje zarówno bezstratną, jak i stratną kompresję w jednym kodeku, progresywne dekodowanie umożliwiające dostęp do wybranych obszarów oraz lepsze współczynniki kompresji &mdash; co czyni go idealnym do archiwizacji dużych badań i przesyłania obrazów w warunkach ograniczonej przepustowości sieci.</p>

<p><strong>Aspose.Medical for .NET</strong> zapewnia czystą implementację C# kodeku JPEG 2000 bez zależności natywnych. Biblioteka może odczytywać, renderować i transkodować pliki DICOM skompresowane przy użyciu obu składni transferu JPEG 2000 Part 1, bezstratnej i stratnej.</p>

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
<tr><td>JPEG 2000 tylko bezstratny</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Bezstratny</td><td>Monochromatyczne i kolorowe, od 1 do 16 bitów</td><td>Monochromatyczne, paletowe i RGB, od 1 do 16 bitów</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Stratny lub bezstratny</td><td>Monochromatyczne i kolorowe, od 1 do 16 bitów</td><td>Monochromatyczne i RGB, od 1 do 16 bitów</td></tr>
<tr><td>JPEG 2000 Part 2 wielokomponentowy tylko bezstratny</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Bezstratny</td><td>Nieobsługiwane</td><td>Nieobsługiwane</td></tr>
<tr><td>JPEG 2000 Part 2 wielokomponentowy</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Stratny lub bezstratny</td><td>Nieobsługiwane</td><td>Nieobsługiwane</td></tr>
<tr><td>HTJ2K tylko bezstratny</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Bezstratny</td><td>Monochromatyczne i kolorowe</td><td>Monochromatyczne i kolorowe</td></tr>
<tr><td>HTJ2K z opcjami RPCL tylko bezstratny</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Bezstratny</td><td>Monochromatyczne i kolorowe</td><td>Monochromatyczne i kolorowe</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Stratny lub bezstratny</td><td>Monochromatyczne i kolorowe</td><td>Monochromatyczne i kolorowe</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dane pikseli od 1 do 16 bitów">}}

<p>Obrazy medyczne często używają 16 bitów na próbkę, aby uchwycić pełny zakres dynamiczny modalności takich jak CT (zwykle 12‑bitowe przechowywane w 16‑bitowych) i MRI. Aspose.Medical obsługuje każdy poziom głębi od 1 do 16 bitów dla JPEG 2000, w próbkach 8‑bitowych lub 16‑bitowych:</p>

<ul>
<li><strong>Odczyt (dekompresja)</strong>: pliki monochromatyczne (CT, MRI, rentgen), pliki z paletą kolorów oraz trójskładowe pliki kolorów (RGB, YBR_RCT, YBR_ICT i inne oznaczenia kolorów takie jak YBR_FULL, dekodowane jako kodowane) z 1‑do‑16‑bitową próbkę, ze znakiem lub bez. Strumienie CMYK, profil ICC, paleta JP2 oraz podpróbkowane strumienie kodu kolorów są odrzucane z wyraźnym wyjątkiem zamiast cichego błędnego obrazu.</li>
<li><strong>Zapis (kompresja)</strong>: obrazy monochromatyczne i RGB z 1‑do‑16‑bitową próbkę, ze znakiem lub bez, oraz obrazy z paletą kolorów przy użyciu JPEG 2000 bezstratnego. Obrazy YBR nie są kodowane, chyba że dekodują się do RGB, tak jak to ma miejsce w stratnych plikach JPEG YBR.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Odczyt i inspekcja skompresowanego DICOM JPEG 2000 - C#</h3>
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
 <h3>Kompresja DICOM do JPEG 2000 bezstratnego - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Kompresja DICOM do JPEG 2000 stratnego - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dekompresja plików DICOM JPEG 2000">}}

<p>Dekompresuj pliki JPEG 2000 do nieskompresowanej składni transferu w celu przetwarzania, analizy lub zapewnienia kompatybilności z systemami, które nie obsługują JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Dekompresja JPEG 2000 do nieskompresowanego - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Możesz również zdekompresować i transkodować do innych formatów kompresji w jednym kroku:</p>

<div class="codeblock" id="code">
 <h3>Transkodowanie pomiędzy formatami kompresji - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderowanie obrazów DICOM JPEG 2000">}}

<p>Pliki DICOM skompresowane JPEG 2000 mogą być renderowane do danych pikseli w celu wyświetlenia lub eksportu, tak jak w przypadku każdej innej składni transferu:</p>

<div class="codeblock" id="code">
 <h3>Renderowanie skompresowanej klatki JPEG 2000 - C#</h3>
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
<tr><td>Jakość obrazu</td><td>Idealna piksel po pikselu &mdash; identyczna z oryginałem</td><td>Podobna wizualnie, część danych trwale utracona</td></tr>
<tr><td>Współczynnik kompresji</td><td>Zwykle 2:1 do 3:1</td><td>Zwykle 10:1 do 30:1 lub wyższy</td></tr>
<tr><td>Najlepsze dla</td><td>Archiwizacja diagnostyczna, dokumentacja prawna, pierwsza lektura</td><td>Wstępna ocena, telemedycyna, transmisja sieciowa</td></tr>
<tr><td>Bezpieczne w cyklu pełnym</td><td>Tak</td><td>Nie &mdash; ponowne kodowanie dodatkowo degraduję jakość</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Wysokowydajny JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) zastępuje wolny koder arytmetyczny JPEG 2000 szybszym kodowaniem blokowym. Zachowuje ten sam przekształcenie falkowe, kolejności progresji i jakość, a dekodowanie i kodowanie jest kilka razy szybsze. Aspose.Medical implementuje wszystkie trzy składnie transferu DICOM HTJ2K w czystym .NET, dla obrazów monochromatycznych i kolorowych, oraz transkoduje pomiędzy HTJ2K a każdą inną obsługiwaną składnią:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; tylko bezstratny</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; bezstratny z kolejnością progresji RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; stratny lub bezstratny</li>
</ul>

<div class="codeblock" id="code">
 <h3>Transkodowanie JPEG 2000 do HTJ2K i z powrotem - C#</h3>
 <pre><code class="cs">// Transcode a JPEG 2000 file to HTJ2K, and back to classic JPEG 2000
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile htj2kFile = j2kFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2kFile.Save("htj2k_lossless.dcm");

// HTJ2K with RPCL progression order, lossless
DicomFile rpclFile = j2kFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
rpclFile.Save("htj2k_rpcl.dcm");

// HTJ2K lossy
DicomFile htj2kLossy = j2kFile.Transcode(TransferSyntax.HTJ2K);
htj2kLossy.Save("htj2k_lossy.dcm");

// Any HTJ2K file decodes back to an uncompressed transfer syntax
DicomFile uncompressed = htj2kFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decoded.dcm");</code></pre>
</div>

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
