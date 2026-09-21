---
title: Praca z dużymi plikami DICOM w C# .NET | Aspose.Medical
weight: 11500

description: Otwórz badania wieloklatkowe i obrazy całych przekrojów w C# bez ładowania ich do pamięci. Odczytaj metadane bez danych pikselowych, odracz duże elementy i przenoś pliki przy użyciu strumieni i potoków.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Duże pliki DICOM w .NET C#" h2="Odczytaj metadane badania wieloklatkowego bez pikseli, odracz duże elementy, aż coś o nie poprosi, i przenoś całe pliki przy użyciu strumieni i potoków." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Plik jest duży, pytanie zazwyczaj jest małe">}}

<p>Obraz całego przekroju, długa seria CT lub objętość OCT liczy setki megabajtów, a większość stanowią dane pikselowe. Praca, którą faktycznie wykonuje aplikacja, jest zazwyczaj znacznie mniejsza: wypisanie zawartości folderu, sprawdzenie identyfikatora pacjenta, policzenie klatek, decyzja, gdzie ma trafić badanie. Ładowanie każdego bajtu w celu uzyskania tych informacji zamienia proste zadanie w problem pamięciowy.</p>

<p><strong>Aspose.Medical for .NET</strong> umożliwia wywołującemu określenie, jaką część pliku odczytać. Wybór to jeden argument w <code>DicomFile.Open</code>, i dotyczy zarówno plików, strumieni, jak i potoków.</p>

<p>Zmierzono na badaniu o wielkości 14 MB i 128 klatkach z naszego zestawu testowego, na tym samym komputerze i tym samym pliku:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Strategia odczytu</th>
<th>Czas otwarcia</th>
<th>Przydzielona pamięć</th>
</tr>
</thead>
<tbody>
<tr><td>Wszystko, domyślne</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Pominięto duże elementy</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Duże elementy odroczone</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>Różnica rośnie wraz z rozmiarem pliku. Folder zawierający 10 000 badań to przypadek, w którym przestaje to być mikrooptymalizacja.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Odczytaj metadane, pozostaw piksele w spokoju">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> pomija każdy element przekraczający określony próg rozmiaru przy odczycie. Zwrócony zestaw danych zawiera tylko te tagi, które potrzebuje indeks lub router.</p>

<div class="codeblock" id="code">
 <h3>Odczytaj badanie bez danych pikselowych – C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>Domyślny próg wynosi 64 kB i przyjmuje wartość w kilobajtach, więc proces, który traktuje 8 kB jako duże, może to określić.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Odracz zamiast pomijać">}}

<p>Gdy piksele mogą być potrzebne, ale najprawdopodobniej później i nie wszystkie, <code>ReadLargeOnDemand</code> jest drugą połową pary. Otwarcie pliku kosztuje tyle samo co pomijanie, a duży element jest odczytywany w momencie, gdy kod do niego się odwoła.</p>

<div class="codeblock" id="code">
 <h3>Ładuj klatkę tylko wtedy, gdy jest używana – C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>Odczyt odroczony jest funkcją licencjonowaną; pozostałe strategie działają również w wersji ewaluacyjnej.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Indeksuj folder bez dotykania pikseli">}}

<p>Ta sama strategia ma zastosowanie do strumienia, co odpowiada skanowaniu archiwum lub magazynowi obiektów w chmurze z perspektywy kodu.</p>

<div class="codeblock" id="code">
 <h3>Skanuj archiwum – C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Strumienie i potoki, wejście i wyjście">}}

<p>Odczyt i zapis akceptują zarówno strumienie, a asynchroniczne punkty wejścia przyjmują także typy <code>System.IO.Pipelines</code>. Badanie może przemieszczać się od odpowiedzi sieciowej do przechowywania bez konieczności trzymania całego pliku w pamięci jako jednej tablicy.</p>

<div class="codeblock" id="code">
 <h3>Czytaj i zapisuj przez strumienie – C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>Ta sama koncepcja obejmuje reprezentacje tekstowe: dokument zawierający wiele zestawów danych jest odczytywany po jednym zestawie na stronach <a href="/medical/net/json-to-dicom/">JSON to DICOM</a> i <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Klatka po klatce">}}

<p>Dane wieloklatkowe są adresowane po jednej klatce, więc seria 500 klatek wymaga przetwarzania jednej klatki naraz, a nie całego elementu danych pikselowych.</p>

<div class="codeblock" id="code">
 <h3>Przeglądaj klatki – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gdzie to decyduje o projekcie">}}

<ul>
<li>Indeksowanie i migracja archiwum: miliony plików, a jedynie nagłówek ma znaczenie, dopóki coś nie zostanie przeniesione.</li>
<li>Routery i węzły magazynujące: przyjmują badanie, odczytują to, co potrzebne do jego trasowania, i przekazują bajty dalej.</li>
<li>Potoki AI: budują manifest z metadanych, a następnie pobierają klatki dla podzbioru, na którym faktycznie odbywa się trening.</li>
<li>Kontenery z limitem pamięci: zestaw roboczy podąża za strategią, nie za rozmiarem pliku.</li>
<li>Dane całych przekrojów i OCT: pliki, w których odczytanie wszystkiego nie wchodzi w grę.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">Przewodnik po zarządzaniu pamięcią</a> wyjaśnia strategie szczegółowo, a <a href="/medical/net/dicom-networking/">sieciowanie DICOM</a> pokazuje te same dane przychodzące przez DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zasoby edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Przewodnik dewelopera" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
{{< blocks/products/pf/slr-element name="Odwołania do API" href="https://reference.aspose.com/medical/net/" >}}
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
