---
title: Konwersja składni transferu DICOM w C# .NET | Aspose.Medical
weight: 16000
description: Transkoduj pliki DICOM pomiędzy składniami transferu w C# .NET. Obsługa JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, RLE oraz formatów nieskompresowanych przy użyciu API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konwersja składni transferu DICOM w .NET C#" h2="Transkoduj pliki DICOM pomiędzy nieskompresowanymi, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS i RLE składniami transferu. Czysta biblioteka .NET bez zależności natywnych." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Czym jest składnia transferu?">}}

<p><strong>Składnia transferu</strong> określa sposób kodowania danych DICOM do przechowywania i transmisji. Definiuje trzy kluczowe aspekty: kolejność bajtów (endianness), czy reprezentacje wartości są explicite czy implicite, oraz algorytm kompresji stosowany do danych pikseli. Każdy plik DICOM deklaruje swoją składnię transferu w nagłówku File Meta Information.</p>

<p>Różne urządzenia medyczne, serwery PACS i aplikacje przeglądające obsługują różne zestawy składni transferu. <strong>Aspose.Medical for .NET</strong> udostępnia metodę <code>Transcode</code> do konwersji pomiędzy składniami transferu, umożliwiając interoperacyjność, optymalizację przechowywania i kompatybilność z narzędziami przetwarzającymi — wszystko w czystej bibliotece .NET bez zależności natywnych.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transkoduj plik DICOM w C#">}}

<p>Metoda <code>DicomFile.Transcode</code> konwertuje plik DICOM z bieżącej składni transferu na dowolną obsługiwaną docelową składnię. Metoda zwraca nową instancję <code>DicomFile</code> &mdash; oryginał pozostaje niezmieniony:</p>

<div class="codeblock" id="code">
 <h3>Podstawowe transkodowanie DICOM - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Możesz również transkodować bezpośrednio na poziomie <code>Dataset</code>:</p>

<div class="codeblock" id="code">
 <h3>Transkoduj Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Obsługiwane składnie transferu">}}

<p>Poniższa tabela wymienia wszystkie standardowe składnie transferu danych obrazu DICOM oraz ich aktualny status wsparcia w Aspose.Medical for .NET. Wszystkie obsługiwane kodeki zostały zaimplementowane w czystym C# i są w pełni niezależne od platformy.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Składnia transferu</th>
<th>UID</th>
<th>Typ</th>
<th>Status</th>
</tr>
</thead>
<tbody>
<tr><td colspan=\"4\"><strong>Nieskompresowane</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Nieskompresowane</td><td>Wspierane</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Nieskompresowane</td><td>Wspierane</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Nieskompresowane (wycofane)</td><td>Wspierane</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Nieskompresowane</td><td>Nieobsługiwane</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Wspierane</td></tr>
<tr><td colspan=\"4\"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Stratny, 8-bitowy</td><td>Wspierane</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Stratny, 12-bitowy</td><td>Nieobsługiwane</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Bezstratny</td><td>Wspierane (tylko 8-bitowe)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Bezstratny</td><td>Wspierane (tylko 8-bitowe)</td></tr>
<tr><td colspan=\"4\"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Bezstratny</td><td>Wspierane</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Near-lossless</td><td>Wspierane</td></tr>
<tr><td colspan=\"4\"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Bezstratny</td><td>Wspierane (odczyt 8-bitowego koloru i 16-bitowego monochromu; zapis 16-bitowego monochromu lub 8-bitowego RGB)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Stratny lub bezstratny</td><td>Wspierane (odczyt 8-bitowego koloru i 16-bitowego monochromu; zapis 16-bitowego monochromu lub 8-bitowego RGB)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Bezstratny</td><td>Nieobsługiwane</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Stratny lub bezstratny</td><td>Nieobsługiwane</td></tr>
<tr><td colspan=\"4\"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Bezstratny</td><td>Wspierane</td></tr>
<tr><td colspan=\"4\"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Bezstratny</td><td>Wspierane</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Bezstratny</td><td>Wspierane</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Stratny lub bezstratny</td><td>Wspierane</td></tr>
<tr><td colspan=\"4\"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Bezstratny</td><td>Wspierane</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Bezstratny</td><td>Tylko dekodowanie (kodowanie wymaga strumienia źródłowego JPEG, nie danych pikseli)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Stratny lub bezstratny</td><td>Wspierane (tryb stratny)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Typowe scenariusze transkodowania">}}

<p>Różne przepływy pracy wymagają różnych strategii transkodowania. Oto najczęstsze scenariusze:</p>

<div class="codeblock" id="code">
 <h3>Dekompresja w celu przetwarzania - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Kompresja w celu archiwizacji - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Kompresja w celu transmisji sieciowej - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Użyj najnowszych kodeków: HTJ2K i JPEG XL - C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sprawdź właściwości składni transferu">}}

<p>Klasa <code>TransferSyntax</code> udostępnia właściwości opisujące cechy kodowania. Użyj ich, aby sprawdzić bieżącą składnię transferu pliku lub wybrać odpowiednią docelową składnię:</p>

<div class="codeblock" id="code">
 <h3>Odczytaj właściwości składni transferu - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>Właściwość</th>
<th>Typ</th>
<th>Opis</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>Unikalny identyfikator składni transferu</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Czy reprezentacje wartości są jawnie kodowane</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Czy kolejność bajtów jest little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Czy dane pikseli są enkapsulowane (skompresowane)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Czy metoda kompresji jest stratna</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Czy składnia używa kompresji deflate</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Czy składnia transferu została wycofana przez standard DICOM</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>Identyfikator metody kompresji stratnej w standardzie ISO</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kompresja stratna vs bezstratna">}}

<p>Zrozumienie różnicy między kompresją stratną a bezstratną jest kluczowe przy transkodowaniu plików DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Aspekt</th>
<th>Bezstratny</th>
<th>Stratny</th>
</tr>
</thead>
<tbody>
<tr><td>Jakość obrazu</td><td>Pixel-perfect &mdash; oryginalne dane w pełni zachowane</td><td>Niektóre dane trwale utracone w celu uzyskania mniejszego rozmiaru</td></tr>
<tr><td>Współczynnik kompresji</td><td>Zazwyczaj 2:1 do 3:1</td><td>Zazwyczaj 10:1 do 30:1 lub wyższy</td></tr>
<tr><td>Bezpieczny przy pełnym cyklu</td><td>Tak &mdash; dekompresja i uzyskanie identycznych pikseli</td><td>Nie &mdash; każde ponowne kodowanie stratne dodatkowo obniża jakość</td></tr>
<tr><td>Zastosowania</td><td>Archiwizacja, diagnostyka, dokumentacja prawna</td><td>Wstępna ocena, telemedycyna, transmisja sieciowa</td></tr>
<tr><td>Obsługiwane kodeki</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>Ważne:</strong> Transkodowanie pliku skompresowanego stratnie do składni bezstratnej nie przywraca utraconych danych. Pogorszenie jakości spowodowane pierwotną kompresją stratną jest trwałe.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Materiały edukacyjne" tabId="resources" >}}
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
