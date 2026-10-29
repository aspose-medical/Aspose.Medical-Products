---
title: UPS-RS w C# .NET - DICOMweb Unified Procedure Step Workitems | Aspose.Medical
weight: 9400

description: Twórz, zgłaszaj i kończ workitems DICOM UPS z C#, odbieraj zmiany stanu przez WebSocket lub uruchom własny serwer UPS-RS na ASP.NET Core z Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS dla .NET C#" h2="Rozdzielaj zadania obrazowania między systemami przy użyciu DICOM Unified Procedure Step: twórz workitems, pozwól pracownikom je zgłaszać i kończyć oraz śledź każdą zmianę, jako klient lub jako serwer." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Kolejka zadań obsługująca DICOM">}}

<p>Model AI musi przetworzyć każdy nowy tomografię komputerową klatki piersiowej. Usługa teleradiologii przydziela badania czytelnikom. Laboratorium 3D otrzymuje zamówienia na post‑procesowanie. Każde z nich jest kolejką zadań pomiędzy systemami, a DICOM definiuje taką kolejkę: Unified Procedure Step (UPS), udostępniany przez HTTP w ramach usługi UPS‑RS DICOMweb.</p>

<p>Workitem UPS określa, co zrobić, dla którego pacjenta i z jakimi danymi wejściowymi. Po utworzeniu ma status SCHEDULED. Pracownik zgłasza go i zmienia na IN PROGRESS, a następnie kończy jako COMPLETED, lub zostaje oznaczony jako CANCELED. Inne systemy subskrybują i otrzymują każdą zmianę stanu przez WebSocket, więc nikt nie musi wykonywać zapytań.</p>

<p><strong>Aspose.Medical for .NET</strong> implementuje obie strony w zarządzanym C#: typowanego klienta dla dowolnego serwera UPS‑RS oraz po stronie serwera dla ASP.NET Core, gdzie biblioteka sprawdza zasady UPS, a Twój kod przechowuje workitems.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zaplanuj workitem">}}

<p>System, który potrzebuje wykonania pracy, tworzy workitem. Jego SOP Instance UID identyfikuje go od tego momentu; etykieta, nazwa listy prac oraz priorytet informują pracowników, czym jest.</p>

<div class="codeblock" id="code">
 <h3>Utwórz workitem UPS - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// What to do, for which patient, and with which input
Dataset input = new();
input.AddOrUpdate(Tag.StudyInstanceUID, "1.2.826.0.1.3680043.10.1.100");

Dataset workitem = new();
workitem.AddOrUpdate(Tag.SOPInstanceUID, Uid.CreateDerivedFromUuid());
workitem.AddOrUpdate(Tag.ProcedureStepState, "SCHEDULED");
workitem.AddOrUpdate(Tag.TransactionUID, string.Empty);
workitem.AddOrUpdate(Tag.InputReadinessState, "READY");
workitem.AddOrUpdate(Tag.ProcedureStepLabel, "Lung nodule detection");
workitem.AddOrUpdate(Tag.WorklistLabel, "AI");
workitem.AddOrUpdate(Tag.ScheduledProcedureStepPriority, "HIGH");
workitem.AddOrUpdate(Tag.PatientName, "DOE^JANE");
workitem.AddOrUpdate(Tag.PatientID, "P001");
workitem.AddOrUpdate(new Sequence(Tag.InputInformationSequence, [input]));

DicomWebClientResult&lt;CreateWorkitemResult&gt; created = await client.Worklist.CreateAsync(workitem);
Console.WriteLine($"Scheduled {created.Value.WorkitemUid}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zgłoś go, wykonaj pracę, zakończ">}}

<p>Pracownik przeszukuje listę prac w poszukiwaniu zadań, które może wykonać, i zgłasza workitem, zmieniając jego status na IN PROGRESS, podając wygenerowany UID transakcji. Od tego momentu tylko posiadacz tego UID może aktualizować lub zakończyć workitem, więc dwóch pracowników nigdy nie przetworzy tego samego zadania.</p>

<div class="codeblock" id="code">
 <h3>Znajdź, zgłoś i zakończ zaplanowane workitems - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// Scheduled workitems on the AI worklist
DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters =
    [
        new DicomWebQueryParameter("ProcedureStepState", "SCHEDULED"),
        new DicomWebQueryParameter("WorklistLabel", "AI")
    ]
};

List&lt;string&gt; scheduled = [];
await using (DicomWebClientStream&lt;Dataset&gt; found = await client.Worklist.SearchAsync(search))
{
    await foreach (Dataset item in found)
        scheduled.Add(item.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID));
}

foreach (string workitemUid in scheduled)
{
    // Claim the workitem; only the holder of this transaction UID can update or finish it
    string transactionUid = Uid.CreateDerivedFromUuid().Code;
    await client.Worklist.ChangeStateAsync(workitemUid, WorkitemState.InProgress, transactionUid);

    // ... run the job ...

    await client.Worklist.UpdateAsync(workitemUid, transactionUid, Performed());
    await client.Worklist.ChangeStateAsync(workitemUid, WorkitemState.Completed, transactionUid);
}</code></pre>
</div>

<p>COMPLETED wymaga zapisu pracy: kto ją wykonał, co zostało wykonane, czas rozpoczęcia i zakończenia oraz wyjście. Pracownik przechowuje te informacje przy aktualizacji przed zmianą stanu.</p>

<div class="codeblock" id="code">
 <h3>Dane przeprowadzonej procedury - C#</h3>
 <pre><code class="cs">// What COMPLETED requires: who did the work, what was done, when, and the output (here none)
public static Dataset Performed()
{
    Dataset performed = new();
    performed.AddOrUpdate(new Sequence(Tag.PerformedStationNameCodeSequence, [Code("AI-NODE-1", "99LOCAL", "AI node 1")]));
    performed.AddOrUpdate(new Sequence(Tag.PerformedWorkitemCodeSequence, [Code("NODULE", "99LOCAL", "Lung nodule detection")]));
    performed.AddOrUpdate(Tag.PerformedProcedureStepStartDateTime, DateTimeOffset.Now.AddMinutes(-2).ToString("yyyyMMddHHmmss"));
    performed.AddOrUpdate(Tag.PerformedProcedureStepEndDateTime, DateTimeOffset.Now.ToString("yyyyMMddHHmmss"));
    performed.AddOrUpdate(new Sequence(Tag.OutputInformationSequence, []));

    Dataset update = new();
    update.AddOrUpdate(new Sequence(Tag.UnifiedProcedureStepPerformedProcedureSequence, [performed]));
    return update;
}

private static Dataset Code(string value, string scheme, string meaning)
{
    Dataset code = new();
    code.AddOrUpdate(Tag.CodeValue, value);
    code.AddOrUpdate(Tag.CodingSchemeDesignator, scheme);
    code.AddOrUpdate(Tag.CodeMeaning, meaning);
    return code;
}</code></pre>
</div>

<p>Serwer zbudowany przy użyciu Aspose.Medical egzekwuje te zasady. Drugi pracownik, który próbuje zgłosić workitem będący w toku, otrzymuje HTTP 409, podobnie zmiana stanu na workitem już zakończonym. Żądanie COMPLETED bez danych przeprowadzonej procedury zwraca HTTP 400. Klient zgłasza takie odpowiedzi jako <code>DicomWebClientException</code> z odpowiedzią HTTP.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Śledź listę prac przez WebSocket">}}

<p>Pulpit, system zamawiający pracę lub pracownik oczekujący na nowe zadania subskrybuje przy użyciu swojego tytułu AE i otwiera połączenie powiadomień. Każde zgłoszenie zdarzenia przychodzi jako zestaw danych z UID workitemu i jego nowym stanem.</p>

<div class="codeblock" id="code">
 <h3>Odbieraj raporty zdarzeń UPS - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// Every change on the worklist, pushed over a WebSocket
await client.Worklist.SubscribeWorklistAsync("DASHBOARD");
await using DicomWebNotificationConnection connection = await client.ConnectNotificationsAsync("DASHBOARD");

while (await connection.ReceiveAsync(cancellationToken) is Dataset report)
{
    Console.WriteLine($"{report.GetSingleValueOrDefault(Tag.AffectedSOPInstanceUID, string.Empty)}: " +
                      $"{report.GetSingleValueOrDefault(Tag.ProcedureStepState, string.Empty)}");
}</code></pre>
</div>

<p>Oprócz całej listy prac, klient może subskrybować pojedynczy workitem za pomocą <code>SubscribeWorkitemAsync</code> lub przefiltrowaną część listy prac za pomocą <code>SubscribeFilteredWorklistAsync</code>, jeśli serwer obsługuje filtry. Anulowanie odbioru zamyka połączenie; otwórz nowe przy użyciu <code>ConnectNotificationsAsync</code>, aby kontynuować.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Twój własny serwer UPS‑RS">}}

<p>Pakiet <code>Aspose.Medical.Dicom.Web.AspNetCore</code> dodaje UPS‑RS do usługi ASP.NET Core. Biblioteka obsługuje każde żądanie: sprawdza ładunek nowego workitemu, przejścia stanów, UID transakcji pracownika oraz dane wymagane przez COMPLETED, a także stosuje reguły dopasowania DICOM w wyszukiwaniach. Twój kod przechowuje workitems.</p>

<div class="codeblock" id="code">
 <h3>Program.cs serwera UPS‑RS - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Przechowywanie workitemów - C#</h3>
 <pre><code class="cs">// Workitems with a revision each; the library applies the UPS state rules and the search matching
public sealed class WorkitemStore : IDicomWebWorkitemProvider
{
    private readonly ConcurrentDictionary&lt;string, DicomWebVersionedDataset&gt; _items = new();

    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string uid in _items.Keys.Order(StringComparer.Ordinal))
            if (_items.TryGetValue(uid, out DicomWebVersionedDataset? item))
                yield return new Dataset(item.Dataset);
        await Task.CompletedTask;
    }

    public ValueTask&lt;DicomWebVersionedDataset?&gt; GetAsync(string workitemUid, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryGetValue(workitemUid, out DicomWebVersionedDataset? item) ? Copy(item) : null);

    public ValueTask&lt;bool&gt; TryCreateAsync(string workitemUid, Dataset workitem, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryAdd(workitemUid, NewRevision(workitem)));

    // Replaces the workitem only if nobody changed it since it was read
    public ValueTask&lt;bool&gt; TryReplaceAsync(string workitemUid, string expectedRevision, Dataset workitem,
        CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryGetValue(workitemUid, out DicomWebVersionedDataset? current)
                             &amp;&amp; current.Revision == expectedRevision
                             &amp;&amp; _items.TryUpdate(workitemUid, NewRevision(workitem), current));

    private static DicomWebVersionedDataset NewRevision(Dataset dataset) =&gt;
        new() { Dataset = new Dataset(dataset), Revision = Guid.NewGuid().ToString("N") };

    private static DicomWebVersionedDataset Copy(DicomWebVersionedDataset item) =&gt;
        new() { Dataset = new Dataset(item.Dataset), Revision = item.Revision };
}</code></pre>
</div>

<p>Każdy przechowywany workitem ma wersję (revision). <code>TryReplaceAsync</code> zastępuje go tylko wtedy, gdy wersja jest nadal tą, którą biblioteka odczytała, więc dwa żądania zmieniające ten sam workitem w tym samym momencie nie mogą się nadpisywać; biblioteka ponawia próbę lub zgłasza konflikt. Zastąp słownik własną bazą danych i zachowaj to sprawdzenie.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Powiadomienia na serwerze">}}

<p>Aby wysyłać raporty zdarzeń, zarejestruj <code>IDicomWebSubscriptionProvider</code> i wywołaj <code>app.UseWebSockets()</code> przed <code>MapDicomWeb</code>. Dostawca przechowuje subskrypcje oraz jedną kolejkę raportów zdarzeń dla każdego subskrybenta, a biblioteka przekazuje mu raport zdarzenia przy każdej zmianie workitemu. Dostawca informuje także bibliotekę, jaki tytuł AE ma uwierzytelniony wywołujący, ponieważ raporty zdarzeń podają system, który zmienił stan.</p>

<p>Przewodnik <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">workflow providers guide</a> zawiera kompletny dostawcę subskrypcji w pamięci, a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">przewodnik ASP.NET Core</a> omawia ustawienia WebSocket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL i MPPS">}}

<p>UPS obejmuje to, co starsze Modality Worklist i MPPS robią pomiędzy RIS a urządzeniem, i dodaje pracowników, którzy nie są modalnościami, umożliwiając zgłaszanie i powiadomienia. Wiele urządzeń wciąż komunikuje się wyłącznie za pomocą MWL i MPPS przez DIMSE; <a href="/medical/net/modality-worklist/">strona modality worklist</a> prezentuje ten przepływ pracy przy użyciu tej samej biblioteki.</p>

<p>Powiązane strony: <a href="/medical/net/dicomweb-client/">klient DICOMweb</a> dla pozostałych usług DICOMweb, <a href="/medical/net/dicomweb-server/">serwer DICOMweb</a> dla QIDO-RS, WADO-RS i STOW-RS na tym samym serwerze oraz <a href="/medical/net/dicom-networking/">sieciowanie DICOM</a> dla DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zasoby edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Przewodnik dewelopera" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="Referencje API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
