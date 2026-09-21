---
title: JPEG XL for DICOM w C# .NET | Aspose.Medical
weight: 10500

description: Przechowuj obrazy DICOM w formacie JPEG XL z C#. Bezstratny JPEG XL, który zwraca piksele bit po bicie, w jednej zarządzanej assembly bez natywnego kodeka do wdrożenia.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL for DICOM w .NET C#" h2="Najnowsza kompresja w standardzie DICOM, z najmniejszymi bezstratnymi plikami, które zmierzyliśmy, zaimplementowana w zarządzanym C# i dostarczana w jednej assembly." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Dlaczego JPEG XL został przyjęty w DICOM">}}

<p>Archiwa medyczne rosną i nigdy się nie zmniejszają. JPEG XL jest kodekiem, który świat obrazowania zaprojektował po dwóch dekadach doświadczeń z JPEG i JPEG 2000, a DICOM dodał go jako transfer syntax z powodu, na którym zależy zespoły przechowywania: przy tych samych pikselach plik jest mniejszy.</p>

<p><strong>Aspose.Medical for .NET</strong> zapisuje i odczytuje JPEG XL poprzez port C# biblioteki libjxl, który znajduje się wewnątrz biblioteki. Pakiet dostarcza jedną assembly, <code>Aspose.Medical.dll</code>, i nie zawiera natywnego pliku binarnego, więc tak nowy kodek nie przekształca się w projekt wdrożeniowy: ta sama assembly działa na Windows, na Linux, na agencie build i w kontenerze.</p>

<p>Dwie transfer syntaxy przenoszą piksele:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), dla danych diagnostycznych, które muszą pozostać niezmienione.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), dla przypadków, w których mniejszy plik ma większe znaczenie niż dokładna kopia.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Skompresuj badanie, zachowaj każdy piksel">}}

<p>Transkodowanie to jedno wywołanie, a zestaw danych wokół pikseli podróżuje razem z nim.</p>

<div class="codeblock" id="code">
 <h3>Transkoduj plik DICOM do JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Zmierzono to na obrazie 1714 × 1933, 16‑bitowym, z naszego własnego zestawu testowego: 6,3 MB nieskompresowane staje się 2,7 MB w JPEG XL bezstratnym, co jest mniejsze niż ten sam obraz w HTJ2K bezstratnym. Twoje własne wyniki zależą od modalności, więc przed wyborem przeprowadź porównanie na folderze swoich plików.</p>

<p>Bezstratny to słowo, które należy brać dosłownie. Transkoduj do JPEG XL i z powrotem, a dane pikseli są identyczne z bajtami, od których rozpoczęto, więc archiwum może być ponownie skompresowane bez dyskusji o jakości diagnostycznej.</p>

<div class="codeblock" id="code">
 <h3>Powrót do nieskompresowanej składni - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Odczytaj to, co już jest zapisane jako JPEG XL">}}

<p>Plik przychodzący w formacie JPEG XL otwiera się jak każdy inny. Transfer syntax określa, co to jest, a dane pikseli są dostępne po zdekodowaniu klatki.</p>

<div class="codeblock" id="code">
 <h3>Otwórz plik JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL lub HTJ2K">}}

<p>Oba są nowoczesne, oba są bezstratne, gdy żądasz bezstratności, a biblioteka zapisuje i odczytuje oba. Odpowiadają na różne pytania.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Pytanie</th>
<th>Odpowiedź</th>
</tr>
</thead>
<tbody>
<tr><td>Który wytworzył mniejszy plik w naszym teście</td><td>JPEG XL bezstratny, o kilka procent</td></tr>
<tr><td>Który jest zaprojektowany do progresywnego podglądu przez sieć</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, szczególnie wariant RPCL</td></tr>
<tr><td>Który wszedł najpierw do standardu DICOM</td><td>HTJ2K, więc dziś więcej archiwów go akceptuje</td></tr>
<tr><td>Który wymaga natywnej zależności tutaj</td><td>Żaden, oba są kodem zarządzanym w jednej assembly</td></tr>
</tbody>
</table>

<p>Wybór zwykle wynika z drugiej strony łącza: transkoduj do składni, którą akceptuje archiwum, i zachowaj resztę pipeline taką samą.</p>

<div class="codeblock" id="code">
 <h3>Niech docelowe archiwum zdecyduje - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gdzie to się opłaca">}}

<ul>
<li>Archiwa długoterminowe: te same badania, mniej terabajtów i brak utraty, którą trzeba uzasadniać radiologowi.</li>
<li>Rachunki za przechowywanie w chmurze: oszczędności powtarzają się co miesiąc, podczas gdy transkodowanie odbywa się jednorazowo.</li>
<li>Zbiory danych do badań i AI: mniejsze kopie szybciej przemieszczają się między przechowywaniem a treningiem.</li>
<li>Wdrożenie: taki nowy kodek zwykle oznacza natywną kompilację na każdą platformę; tutaj jest częścią assembly, którą już referujesz.</li>
</ul>

<p>Biblioteka zapisuje również kodeki, które znajdują się w istniejących archiwach: JPEG, JPEG‑LS, JPEG 2000, HTJ2K i RLE. Strona <a href="/medical/net/dicom-transfer-syntax-conversion/">konwersji transfer syntax</a> obejmuje cały zestaw, <a href="/medical/net/htj2k/">HTJ2K</a> ma własną stronę, a <a href="/medical/net/jpeg2000/">JPEG 2000</a> jest źródłem obu nowych kodeków.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zasoby edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Przewodnik dla programistów" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="Odwołania API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Wsparcie produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Bezpłatne wsparcie" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Płatne wsparcie" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Dlaczego Aspose.Medical dla .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista klientów" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Historie sukcesu" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
