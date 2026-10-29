---
title: Klient DICOMweb w C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Wyszukuj, pobieraj i wysyłaj badania DICOM przez HTTP z .NET. Klient DICOMweb w czystym C# dla QIDO-RS, WADO-RS i STOW-RS, współpracujący z PACS, VNA i usługami obrazowania w chmurze.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Klient DICOMweb dla .NET C#" h2="Wyszukuj, pobieraj i wysyłaj badania na dowolnym serwerze DICOMweb przy użyciu QIDO-RS, WADO-RS i STOW-RS, w zarządzanym C# bez konieczności instalacji czegokolwiek na komputerze." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM przez HTTP, z własnego kodu">}}

<p>DICOMweb to forma DICOM używająca protokołu HTTP. Zamiast asocjacji DIMSE na porcie 104, Twoja aplikacja wysyła zwykłe żądania HTTPS: QIDO-RS do wyszukiwania, WADO-RS do pobierania, STOW-RS do wysyłania. Usługi obrazowania w chmurze, takie jak Azure Health Data Services DICOM service oraz Google Cloud Healthcare API, działają w ten sposób, podobnie jak archiwa takie jak Orthanc i dcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> zawiera klienta DICOMweb od wersji 26.10. <code>DicomWebClient</code> opakowuje każdą standardową transakcję w metodę typowaną, a wyniki zwracane są jako te same obiekty <code>Dataset</code> i <code>DicomFile</code>, które reszta biblioteki odczytuje i zapisuje. Serwer po drugiej stronie może być dowolną implementacją DICOMweb; nie musi być zbudowany przy użyciu Aspose.Medical.</p>

<p>Klient jest zarządzanym C# opartym na <code>HttpClient</code>. Nie ma natywnego zestawu narzędzi ani usługi proxy, więc ten sam kod działa na Windows, Linux oraz w kontenerze.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Połącz się z serwerem DICOMweb">}}

<p>Utwórz klienta z <code>HttpClient</code> i adresu URL korzenia usługi serwera. Uwierzytelnianie, ustawienia TLS, proxy i pooling połączeń pozostają w <code>HttpClient</code>, gdzie Twoja aplikacja już je konfiguruje. Klient nie jest właścicielem <code>HttpClient</code> i nie usuwa go.</p>

<div class="codeblock" id="code">
 <h3>Utwórz klienta DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Jeśli jedno wdrożenie obsługuje WADO-URI lub inną usługę z innego URL, <code>ServiceBaseUris</code> nadpisuje korzeń tylko dla tej usługi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Wyszukaj badania za pomocą QIDO-RS">}}

<p>Wyszukiwanie przyjmuje parametry dopasowania z kluczowym słowem DICOM jako nazwą, pola do zwrócenia oraz kontrolki stronicowania. Wyniki przychodzą jako strumień datasetów, odczytywany po jednym, podczas gdy odpowiedź nadal się pobiera.</p>

<div class="codeblock" id="code">
 <h3>Znajdź badania pacjenta - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> i <code>SearchInstancesAsync</code> przeszukują o poziom niżej, w całym archiwum lub wewnątrz jednego badania lub serii. Te same opcje umożliwiają także dopasowanie przybliżone, dopasowanie pustych wartości oraz offset wyników dla stronicowania.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pobierz badanie przy użyciu WADO-RS">}}

<p>Badanie może zawierać tysiące instancji. <code>RetrieveStudyAsync</code> zwraca je jako strumień, więc każdy plik jest parsowany w miarę przybywania części odpowiedzi i może być zapisany przed odczytaniem kolejnego. Zużycie pamięci nie rośnie wraz z rozmiarem badania.</p>

<div class="codeblock" id="code">
 <h3>Zapisz każdą instancję badania - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> i <code>RetrieveInstanceAsync</code> pobierają jedną serię lub jedną instancję w ten sam sposób.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pobierz miniaturkę lub wyrenderowany obraz">}}

<p>Podgląd lub ekran listy pracy często wymaga obrazu, a nie pliku DICOM. Serwer renderuje go i wysyła jako JPEG lub PNG, więc klient nie pobiera ani nie dekoduje pierwotnych danych pikseli.</p>

<div class="codeblock" id="code">
 <h3>Zapisz miniaturkę JPEG instancji - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p><code>RetrieveRenderedInstanceAsync</code> i <code>RetrieveRenderedFramesAsync</code> zwracają zamiast tego pełnowymiarowy obraz. <code>DicomWebRenderOptions</code> przekazuje standardowe ustawienia renderowania do serwera: jakość obrazu, rozmiar okna widoku, okno (window) i profil ICC.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Wysyłaj instancje przy użyciu STOW-RS">}}

<p>STOW-RS wysyła dowolną liczbę instancji w jednym żądaniu. Serwer odpowiada raportem, który wymienia, instancja po instancji, co zostało zapisane, a co odrzucone.</p>

<div class="codeblock" id="code">
 <h3>Wgraj folder plików DICOM - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>Niektóre serwery odpowiadają błędem HTTP, np. 409, gdy jedna lub więcej instancji nie powiodą się. W takim przypadku wywołanie rzuca <code>DicomWebClientException</code>, jak pokazuje kolejna sekcja, a ten sam raport znajduje się w treści odpowiedzi wyjątku.</p>

<p><code>StoreStudyAsync</code> wgrywa do jednego nazwanych badań. Obie metody akceptują także <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, dzięki czemu pliki mogą być generowane w trakcie wysyłania żądania.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Obsłuż błędy serwera">}}

<p>Każda typowana operacja sprawdza status HTTP przed zwróceniem wyniku. Gdy serwer odrzuca żądanie, wywołanie rzuca <code>DicomWebClientException</code>, który zawiera kod statusu, nagłówki oraz początek treści odpowiedzi do logu.</p>

<div class="codeblock" id="code">
 <h3>Zgłoś nieudane żądanie - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>Metody zwracające surową odpowiedź, takie jak powyższa miniaturka, pozostawiają sprawdzenie statusu tobie za pomocą <code>IsSuccessStatusCode</code> lub <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Co jeszcze obsługuje klient">}}

<p>Wyszukiwanie, pobieranie i wysyłanie to podstawowe wywołania. Klient posiada także grupę operacji dla każdej z pozostałych usług DICOMweb:</p>

<ul>
<li>Dane masowe, dane pikseli i wybrane klatki instancji, według numeru klatki zaczynającego się od 1, tak jak serwer je przesyła.</li>
<li>WADO-URI, starsze pobieranie z pojedynczego URL, nadal używane przez wiele przeglądarek i archiwów, dla plików DICOM i renderowanych obrazów.</li>
<li>Listy prac UPS-RS: tworzenie, wyszukiwanie, aktualizacja i przyjmowanie elementów pracy, zmiana ich stanu, żądanie anulowania oraz subskrypcja zdarzeń przychodzących przez WebSocket.</li>
<li>Lista pracowni modalnej (MWL-RS), wykonane kroki procedury modalności (MPPS-RS) oraz zobowiązanie przechowywania.</li>
<li>Instancje nie dotyczące pacjenta, takie jak palety kolorów i protokoły zawieszania, oraz dokument możliwości serwera.</li>
<li><code>SendAsync</code> dla dowolnej standardowej transakcji z własnymi wartościami trasy, parametrami zapytania, nagłówkami i ciałem, gdy typowana metoda nie pasuje.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">Przewodnik po kliencie DICOMweb</a> dokumentuje każdą operację i opcję, a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">referencja operacji klienta</a> wymienia wszystkie metody.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Jedna biblioteka od odpowiedzi HTTP po dane pikseli">}}

<p>Plik pobrany przy pomocy klienta jest zwykłym <code>DicomFile</code>. Może być anonimowy, transkodowany, renderowany lub serializowany w tym samym procesie, bez kroków konwersji i bez dodatkowej biblioteki w łańcuchu.</p>

<p>Powiązane strony: <a href="/medical/net/dicomweb-server/">serwer DICOMweb</a> dla drugiej strony protokołu w Twojej własnej usłudze ASP.NET Core, <a href="/medical/net/dicom-networking/">sieciowanie DICOM</a> dla archiwów używających DIMSE zamiast HTTP, <a href="/medical/net/anonymization/">anonimizację</a> przed opuszczeniem badania szpitala, <a href="/medical/net/dicom-transfer-syntax-conversion/">konwersję składni transferu</a> dla tego, co serwer przechowuje, oraz <a href="/medical/net/dicom-to-json/">DICOM do JSON</a> w formacie używanym w wynikach wyszukiwania QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zasoby edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Przewodnik dla programistów" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="Referencje API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
