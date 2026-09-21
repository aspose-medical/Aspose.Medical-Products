---
title: HTJ2K w C# .NET - High-Throughput JPEG 2000 dla DICOM | Aspose.Medical
weight: 10000

description: Kompresuj i odczytuj obrazy DICOM w formacie High-Throughput JPEG 2000 z C#. Bezstratny HTJ2K, wariant RPCL oraz stratny HTJ2K, zaimplementowane w zarządzanym .NET bez potrzeby instalacji natywnego kodeka.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K w .NET C#" h2="High-Throughput JPEG 2000 dla DICOM: kompresja dodana do standardu dla szybkich archiwów i przeglądania w chmurze, zaimplementowana w zarządzanym C# bez konieczności instalacji natywnych komponentów." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Co zmienia HTJ2K">}}

<p>High-Throughput JPEG 2000 zachowuje falę i jakość obrazu JPEG 2000, jednocześnie zastępując część, która spowalniała proces. Nowy kodator blokowy oraz dekodowanie jest o rząd wielkości szybsze, co spowodowało przyjęcie go przez standard DICOM w trzech syntaksach transferu oraz migrację platform obrazowania w chmurze.</p>

<p>Dla zespołu .NET praktyczne pytanie brzmi inaczej: kto naprawdę może generować te pliki. Większość bibliotek uzyskuje dostęp do HTJ2K przez natywną kompilację OpenJPH, co oznacza binarkę dla każdej platformy, krok budowania w kontenerze oraz zależność, o której będzie pytać przegląd bezpieczeństwa. <strong>Aspose.Medical for .NET</strong> implementuje kodek w zarządzanym kodzie w tym samym pakiecie, który odczytuje i zapisuje pliki, więc HTJ2K działa tak samo na Windows, Linux i w kontenerze, bez konieczności instalacji.</p>

<p>Obsługiwane są trzy syntakty transferu, a wszystkie trzy zarówno odczytują, jak i zapisują:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), High-Throughput JPEG 2000 bezstratny.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), bezstratny wariant z kolejnością progresji RPCL.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Skompresuj studium do HTJ2K">}}

<p>Jedno wywołanie przenosi plik do nowej syntaksy. Zestaw danych, prywatne tagi oraz informacje meta pliku podążają za nim.</p>

<div class="codeblock" id="code">
 <h3>Transkoduj plik DICOM do HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>Na obrazie 1714 × 1933, 16‑bitowym z naszego własnego zestawu testowego, plik zmniejsza się z 6,3 MB do 2,9 MB, a piksele odtwarzane są bit po bicie. Wyniki różnią się w zależności od modalności i obrazu, dlatego należy zmierzyć na własnych danych, co jest jedną pętlą po istniejących już plikach.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bezstratny oznacza bezstratność">}}

<p>Dane diagnostyczne nie tolerują kodeka, który jest jedynie prawie poprawny. Transkoduj do HTJ2K bezstratnego i z powrotem, a dane pikseli będą identyczne z bajtami, od których zaczęto, co możesz zweryfikować w własnym zestawie testów przed zgodą na ponowną kompresję archiwum.</p>

<div class="codeblock" id="code">
 <h3>Powrót do nieskompresowanej syntaksy - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, wariant przeznaczony do przeglądania w sieci">}}

<p>Syntaksa 1.2.840.10008.1.2.4.202 przechowuje ten sam bezstratny strumień kodowy w kolejności progresji RPCL: najpierw rozdzielczość, potem pozycja, komponent, a na końcu warstwa. Czytnik, który pobiera jedynie początek strumienia, otrzymuje kompletny obraz o niskiej rozdzielczości, co jest potrzebne podglądowi przy otwieraniu dużego studium przez łącze, nad którym nie ma kontroli.</p>

<div class="codeblock" id="code">
 <h3>Kompresuj z kolejnością progresji RPCL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Odczytaj, co przesyła archiwum">}}

<p>Druga część zadania to akceptowanie HTJ2K od systemów, które już go generują. Otwórz plik, sprawdź, w jakiej formie jest przechowywany, i pracuj z danymi pikseli.</p>

<div class="codeblock" id="code">
 <h3>Odczytaj plik HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Obrazy wieloklatkowe są obsługiwane klatka po klatce, więc długa seria zużywa pamięć na klatkę zamiast na całe studium.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gdzie HTJ2K znajduje swoje zastosowanie">}}

<ul>
<li>Migracja archiwum: ponowna kompresja przechowywanego studium do HTJ2K bezstratnego, zmniejszenie rozmiaru, zachowanie integralności danych diagnostycznych.</li>
<li>Chmura i DICOMweb: szybkość dekodowania to czynnik, który sprawia, że podgląd po stronie przeglądarki lub serwera jest natychmiastowy przy dużych obrazach.</li>
<li>Potoki AI: zestawy treningowe są czytane znacznie częściej niż zapisywane, a czas dekodowania jest powtarzającym się kosztem.</li>
<li>Kontenery i serverless: kodek jest częścią zestawu, więc obraz nie wymaga natywnej biblioteki ani kompilatora w procesie budowania.</li>
</ul>

<p>Biblioteka dostarcza również JPEG XL, kolejne niedawne rozszerzenie standardu, oraz starsze kodeki, które archiwum może zawierać: JPEG, JPEG‑LS, JPEG 2000 i RLE. Strona <a href="/medical/net/dicom-transfer-syntax-conversion/">konwersji syntaksów transferu</a> opisuje cały zestaw, a strona <a href="/medical/net/jpeg2000/">JPEG 2000</a> opisuje kodek, z którego wyewoluował HTJ2K.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Materiały edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Przewodnik dewelopera" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
