---
title: Serwer DICOMweb w C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Zbuduj własny serwer DICOMweb lub PACS w C#. Aspose.Medical implementuje QIDO-RS, WADO-RS, STOW-RS oraz usługi listy roboczej na ASP.NET Core; Ty podłączasz magazyn.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Serwer DICOMweb dla .NET C#" h2="Dodaj punkty końcowe QIDO-RS, WADO-RS i STOW-RS do usługi ASP.NET Core. Biblioteka obsługuje protokół; twój kod decyduje, gdzie przechowywane są badania." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Twój własny serwer DICOMweb w C#">}}

<p>PACS, VNA, archiwum w chmurze lub platforma badawcza muszą odpowiadać na żądania DICOMweb: wyszukiwania QIDO-RS, pobierania WADO-RS i przesyłania STOW-RS, a często także usługi listy roboczej. Każda z nich ma własne trasy, typy mediów, ciała multipart, reguły dopasowania i kody statusu.</p>

<p><strong>Aspose.Medical for .NET</strong> implementuje tę stronę protokołu od wersji 26.10. Odczytuje każde żądanie, weryfikuje je względem standardu, dopasowuje klucze wyszukiwania zgodnie z regułami DICOM, stronicuje wyniki, konwertuje syntakty transferu i zapisuje odpowiedź. Twój kod robi to, co może tylko Ty: przechowuje instancję, znajduje kandydatów do wyszukiwania i zwraca zapisane pliki. Przeglądarki, modalności i inne archiwa działają z Twoim serwerem tak samo jak z każdym innym serwerem DICOMweb, podobnie jak <a href="/medical/net/dicomweb-client/">klient DICOMweb</a> tej samej biblioteki.</p>

<p>Serwer jest zarządzany w C#. Działa wszędzie tam, gdzie działa ASP.NET Core: na Windows, na Linuxie i w kontenerze.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kompletny serwer w ASP.NET Core">}}

<p>Zainstaluj pakiet <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Dostarcza on Aspose.Medical i łączy usługi DICOMweb z wstrzykiwaniem zależności oraz routowaniem punktów końcowych. Ten <code>Program.cs</code> jest działającym serwerem Studies; poniżej pokazano dwie klasy, które rejestruje.</p>

<div class="codeblock" id="code">
 <h3>Program.cs serwera DICOMweb - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>Każda trasa Studies odpowiada teraz pod <code>/dicomweb</code>: <code>/dicomweb/studies</code> do wyszukiwania i przesyłania, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> do pobierania oraz trasy dla metadanych, klatek, danych masowych i renderowanych obrazów. Dokument możliwości w korzeniu usługi wymienia dokładnie transakcje obsługiwane przez serwer.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Połącz swój magazyn">}}

<p>Usługa Studies wymaga jednego interfejsu z trzema metodami: przechowywania instancji, zwracania przechowywanych plików oraz zwracania kandydatów do wyszukiwania. Ta implementacja trzyma każdą instancję jako plik w folderze per badanie i seria.</p>

<div class="codeblock" id="code">
 <h3>Archiwum folderowe za QIDO-RS, WADO-RS i STOW-RS - C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>Biblioteka sprawdza UIDy w każdej ścieżce żądania i w każdej przesłanej instancji przed wywołaniem dostawcy. Instancja bez prawidłowych UIDów jest zgłaszana jako niepowodzenie i nigdy nie dociera do <code>StoreAsync</code>.</p>

<p>Folder wystarczy, aby wypróbować serwer. Archiwum produkcyjne zachowuje te same trzy metody i umieszcza magazyn obiektów za <code>StoreAsync</code> i <code>RetrieveAsync</code> oraz indeks bazy danych za <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Wyszukiwanie: biblioteka stosuje reguły dopasowania DICOM">}}

<p>Dopasowanie QIDO-RS to więcej niż porównanie łańcuchów: symbole wieloznaczne, zakresy dat i czasów, listy UIDów, przybliżone nazwy osób, puste wartości i klucze wewnątrz sekwencji. Dostawca nie implementuje tego. Zwraca kandydatów w stabilnym porządku i może zwrócić więcej niż pasuje. Biblioteka stosuje dokładne dopasowanie DICOM do każdego kandydata, zachowuje żądane pola i stosuje <code>offset</code> oraz <code>limit</code>. Jedna odpowiedź zawiera domyślnie maksymalnie 1000 wyników, a <code>DicomWebStandardServiceOptions</code> zmienia ten limit.</p>

<p>Aby utrzymać szybkie wyszukiwania w dużym archiwum, przekaż selektywne klucze do bazy danych. <code>search.Plan.Filter</code> zawiera skompilowane klucze wyszukiwania, z atrybutem, typem dopasowania i wartościami każdego z nich:</p>

<div class="codeblock" id="code">
 <h3>Odczytaj dokładny Patient ID z wyszukiwania - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Zapytanie do bazy danych zbudowane w ten sposób może zwrócić dodatkowe wiersze, na przykład gdy zignoruje klucz, którego nie potrafi przetłumaczyć. Końcowe dopasowanie w bibliotece usuwa je. Nie może usuwać prawdziwych dopasowań, ponieważ biblioteka nie może ich ponownie dodać.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Przechowywanie: wynik dla każdej instancji">}}

<p>Żądanie STOW-RS może zawierać setki instancji. Biblioteka parsuje ciało multipart i kolejno wywołuje <code>StoreAsync</code> dla każdej instancji, dzięki czemu cały upload nie jest jednorazowo trzymany w pamięci. Dla każdej instancji dostawca zwraca <code>Stored</code>, <code>Warning</code> dla instancji przechowywanej z zmianami lub <code>Failed</code> z opcjonalnym kodem przyczyny DICOM.</p>

<p>Biblioteka tworzy raport STOW-RS z tych wyników i wybiera status: 200, gdy każda instancja jest przechowywana, 202, gdy niektóre mają ostrzeżenia lub niepowodzenia, 409, gdy wszystkie nie powiodły się. Instancje w żądaniu jednego nazwanego badania, które należą do innego badania, są odrzucane przed wywołaniem dostawcy.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pobieranie: syntaksy transferu, klatki i renderowane obrazy">}}

<p>Dostawca zwraca przechowywane pliki, a biblioteka dostarcza je w formie żądanej przez klienta. Klient WADO-RS może żądać syntaksy transferu w nagłówku <code>Accept</code>, a biblioteka konwertuje każdy plik przed wysłaniem, zawierając własne kodeki, w tym HTJ2K i JPEG XL. Metadane w DICOM JSON lub XML, dane masowe oraz pojedyncze klatki pochodzą od tego samego dostawcy, bez dodatkowego kodu.</p>

<p>Renderowane obrazy potrzebują jeszcze jednej klasy, ponieważ tylko Ty decydujesz, które obrazy serwer może wygenerować. Ta klasa oferuje JPEG dla renderowanych instancji, renderowanych klatek i miniatur, a biblioteka je renderuje:</p>

<div class="codeblock" id="code">
 <h3>Udostępnij renderowane obrazy JPEG i miniatury - C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>Wbudowany renderer tworzy JPEG z jednopłytkowej instancji lub z klatek wybranych w żądaniu. Renderowana wieloklatkowa instancja jako całość, obraz szerszy lub wyższy niż 65500 pikseli (miniatury pomijają takie obrazy), inne formaty obrazów oraz parametry viewportu, okna i profilu ICC wymagają Twojej własnej metody <code>RetrieveAsync</code> w tej klasie.</p>

<p>Miniatury są obsługiwane z dbałością o prywatność pacjenta. Wbudowany renderer tworzy miniaturkę tylko z instancji, której atrybuty Burned In Annotation oraz Recognizable Visual Features mają wartość <code>NO</code>, co oznacza, że obraz sam deklaruje brak danych pacjenta. Wiele obrazów nie posiada tych atrybutów i w takim przypadku serwer zwraca 406. Jeśli Twoje archiwum potrzebuje miniatur takich obrazów, wygeneruj je w własnej metodzie <code>RetrieveAsync</code> i upewnij się, że ukrywają wszelkie dane pacjenta.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Listy robocze, kroki procedury i inne usługi">}}

<p>Pozostałe usługi DICOMweb działają według tego samego modelu: rejestrują usługę, implementują jej interfejs magazynowania.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> listy robocze z <code>AddStandardWorklistService</code>: tworzenie, wyszukiwanie i aktualizacja elementów pracy oraz zmiana ich stanu, przy czym reguły stanu są sprawdzane przez bibliotekę, a powiadomienia zdarzeń wysyłane przez WebSocket.</li>
<li><a href="/medical/net/modality-worklist/">Lista robocza modalności</a> (MWL-RS) z <code>AddStandardModalityWorklistService</code>, aby modalności mogły zapytać o zaplanowane kroki procedury w Twoim RIS.</li>
<li>Kroki procedury wykonane przez modalność (MPPS-RS), zobowiązanie do przechowywania oraz instancje niepacjentowe, takie jak palety kolorów i protokoły zawieszania.</li>
<li>Pobieranie WADO-URI dla starszych przeglądarek, obsługiwane przez usługę Studies.</li>
</ul>

<p>Gdy standardowe zachowanie jednej transakcji nie pasuje, zarejestruj własny obsługujący ją handler za pomocą <code>AddHandler</code>. Biblioteka nadal routuje i sprawdza żądanie, a pozostałe transakcje zachowują swoje standardowe handler’y.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bezpieczeństwo i hosting pozostają w Twoich rękach">}}

<p>Punkty końcowe DICOMweb są zwykłymi punktami końcowymi ASP.NET Core. Uwierzytelnianie, autoryzacja, CORS, TLS, przekazane nagłówki i limity rozmiaru żądania są konfigurowane tak samo jak w pozostałej części Twojej usługi:</p>

<div class="codeblock" id="code">
 <h3>Wymagaj uwierzytelnionego użytkownika - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Część protokołu nie zależy od ASP.NET Core. <code>DicomWebApplication</code> zamienia <code>DicomWebRequest</code> na odpowiedź, więc te same usługi mogą działać za innym serwerem HTTP lub w teście bez sieci:</p>

<div class="codeblock" id="code">
 <h3>Ten sam serwer bez ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Klient i serwer z jednej biblioteki">}}

<p><a href="/medical/net/dicomweb-client/">Klient DICOMweb</a> Aspose.Medical testuje Twój serwer z testu jednostkowego lub programu konsolowego, a każdy plik otrzymany przez serwer jest zwykłym <code>DicomFile</code>, który może być anonimowy, transkodowany lub renderowany w tym samym procesie.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">Przewodnik dewelopera DICOMweb</a> opisuje każdą usługę i interfejs magazynowania, a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">przewodnik ASP.NET Core</a> obejmuje mapowanie punktów końcowych i WebSockets.</p>

<p>Powiązane strony: <a href="/medical/net/dicom-networking/">Sieciowanie DICOM</a> dla systemów wysyłających badania przez DIMSE zamiast HTTP, <a href="/medical/net/anonymization/">anonimizacja</a> przychodzących badań, <a href="/medical/net/dicom-transfer-syntax-conversion/">konwersja syntaksu transferu</a> oraz <a href="/medical/net/dicom-to-json/">DICOM do JSON</a> dla formatu wyników QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zasoby edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Przewodnik dewelopera" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
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
