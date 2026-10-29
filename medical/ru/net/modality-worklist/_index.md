---
title: Список процедур модальности и MPPS в C# .NET — DICOM C-FIND, N-CREATE, N-SET | Aspose.Medical
weight: 9300

description: Запрашивайте список процедур DICOM модальности с помощью C‑FIND и отправляйте MPPS из C#, либо разверните собственный сервер списка процедур. DIMSE и DICOMweb в одной .NET‑библиотеке.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Список процедур модальности и MPPS для .NET C#" h2="Получайте ежедневные исследования из RIS, отмечайте каждое как начатое и завершённое, либо выступайте в роли собственного сервера списка процедур через DIMSE или DICOMweb." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="От RIS расписания к выполненному исследованию">}}

<p>Перед сканированием техник не должен вручную вводить имя пациента, идентификатор пациента и номер доступа в модальность. RIS уже запланировал исследование, и модальность запрашивает его: запрос Modality Worklist возвращает запланированные шаги процедуры для данного сканера и текущего дня. Модальность копирует данные пациента и заказа в изображения, поэтому исследование попадает в PACS уже сопоставленным с заказом.</p>

<p>Во время выполнения исследования модальность сообщает о нём через MPPS (Modality Performed Procedure Step): IN PROGRESS при начале сканирования, COMPLETED или DISCONTINUED по завершении, вместе с полученными сериями. RIS использует эти отчёты для обновления расписания, оповещения радиолога о поступлении изображений и для выставления счетов.</p>

<p><strong>Aspose.Medical for .NET</strong> охватывает обе стороны этого рабочего процесса в управляемом C#: модальность, которая делает запросы и отправляет отчёты, и сервер списка процедур, который отвечает. Оба работают через DIMSE, классический сетевой протокол DICOM, и через DICOMweb.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Запрос списка процедур с помощью C‑FIND">}}

<p>Запрос списка процедур – это запрос C‑FIND в информационной модели Modality Worklist. <code>CFindRequest.CreateWorklistQuery</code> формирует его с обычными ключами возврата: демография пациента, номер доступа, запрошенная процедура и запланированный шаг процедуры. Фильтры, которые задаёт модальность, включают её собственный AE‑title, тип модальности и дату, всё внутри запланированного шага процедуры.</p>

<div class="codeblock" id="code">
 <h3>Получить сегодняшние исследования для CT‑сканера — C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "RIS_AE",
        Calling = "CT01",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.20"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityWorklistInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddCFindHandler((request, response, cancellationToken) =&gt;
    {
        // Every scheduled exam arrives as one identifier; the final response has none
        if (response.Identifier is Dataset item)
        {
            Dataset step = item.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);
            Console.WriteLine($"{item.GetSingleValueOrDefault(Tag.AccessionNumber, string.Empty)} " +
                              $"{item.GetSingleValueOrDefault(Tag.PatientName, string.Empty)} " +
                              $"{step.GetSingleValueOrDefault(Tag.ScheduledProcedureStepDescription, string.Empty)}");
        }

        return ValueTask.CompletedTask;
    })
    .Build();

// CT exams for this scanner
CFindRequest query = CFindRequest.CreateWorklistQuery(
    patientId: null,
    patientName: null,
    stationAe: "CT01",
    stationName: null,
    modality: "CT",
    scheduledDateTime: null);

// Scheduled for today
query.Identifier.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence)
    .AddOrUpdate(Tag.ScheduledProcedureStepStartDate, DateTimeOffset.Now.ToString("yyyyMMdd"));

client.QueueRequest(query);

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Каждое совпадение представляет собой набор данных с атрибутами пациента и заказа на верхнем уровне и запланированным шагом процедуры в последовательности. Чтобы запросить дополнительные ключи возврата, добавьте их в <code>query.Identifier</code> с пустым значением до помещения запроса в очередь.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Отчёт о исследовании с помощью MPPS">}}

<p>MPPS представляет собой две нормализованные операции DIMSE над классом SOP Modality Performed Procedure Step: N‑CREATE при начале исследования и N‑SET по его завершении. Модальность выбирает UID выполненного шага и использует его для обеих операций. Атрибуты N‑CREATE указывают, кого исследовали и какой запланированный шаг был выполнен; большинство из них берутся непосредственно из элемента списка процедур.</p>

<div class="codeblock" id="code">
 <h3>Атрибуты N‑CREATE из элемента списка процедур — C#</h3>
 <pre><code class="cs">// N-CREATE attributes of a performed procedure step, filled from the worklist item the modality works on
public static Dataset InProgress(Dataset item, string stationAe)
{
    Dataset step = item.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);

    Dataset scheduled = new();
    Copy(item, scheduled, Tag.StudyInstanceUID, Tag.AccessionNumber, Tag.RequestedProcedureID, Tag.RequestedProcedureDescription);
    Copy(step, scheduled, Tag.ScheduledProcedureStepID, Tag.ScheduledProcedureStepDescription);
    scheduled.AddOrUpdate(new Sequence(Tag.ReferencedStudySequence, []));
    scheduled.AddOrUpdate(new Sequence(Tag.ScheduledProtocolCodeSequence, []));

    Dataset mpps = new();
    mpps.AddOrUpdate(new Sequence(Tag.ScheduledStepAttributesSequence, [scheduled]));
    Copy(item, mpps, Tag.PatientName, Tag.PatientID, Tag.PatientBirthDate, Tag.PatientSex, Tag.StudyID);
    Copy(step, mpps, Tag.Modality);
    mpps.AddOrUpdate(new Sequence(Tag.ReferencedPatientSequence, []));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepID, step.GetSingleValueOrDefault(Tag.ScheduledProcedureStepID, string.Empty));
    mpps.AddOrUpdate(Tag.PerformedStationAETitle, stationAe);
    mpps.AddOrUpdate(Tag.PerformedStationName, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedLocation, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedProcedureStepStartDate, DateTimeOffset.Now.ToString("yyyyMMdd"));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepStartTime, DateTimeOffset.Now.ToString("HHmmss"));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepStatus, "IN PROGRESS");
    mpps.AddOrUpdate(Tag.PerformedProcedureStepDescription, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedProcedureTypeDescription, string.Empty);
    mpps.AddOrUpdate(new Sequence(Tag.ProcedureCodeSequence, []));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepEndDate, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedProcedureStepEndTime, string.Empty);
    mpps.AddOrUpdate(new Sequence(Tag.PerformedProtocolCodeSequence, []));
    mpps.AddOrUpdate(new Sequence(Tag.PerformedSeriesSequence, []));
    return mpps;
}

// Copies the attributes the source has; a missing one is sent empty
private static void Copy(Dataset source, Dataset target, params Tag[] tags)
{
    foreach (Tag tag in tags)
    {
        if (source.GetOrDefault(tag) is IElement element)
            target.AddOrUpdate(element);
        else
            target.AddOrUpdate(tag, string.Empty);
    }
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Атрибуты N‑SET, завершающие шаг — C#</h3>
 <pre><code class="cs">// N-SET attributes that close the step and list the series the modality produced
public static Dataset Completed(string seriesInstanceUid, string protocolName)
{
    Dataset series = new();
    series.AddOrUpdate(Tag.SeriesInstanceUID, seriesInstanceUid);
    series.AddOrUpdate(Tag.ProtocolName, protocolName);
    series.AddOrUpdate(Tag.SeriesDescription, string.Empty);
    series.AddOrUpdate(Tag.PerformingPhysicianName, string.Empty);
    series.AddOrUpdate(Tag.OperatorsName, string.Empty);
    series.AddOrUpdate(Tag.RetrieveAETitle, string.Empty);
    series.AddOrUpdate(new Sequence(Tag.ReferencedImageSequence, []));
    series.AddOrUpdate(new Sequence(Tag.ReferencedNonImageCompositeSOPInstanceSequence, []));

    Dataset completed = new();
    completed.AddOrUpdate(Tag.PerformedProcedureStepStatus, "COMPLETED");
    completed.AddOrUpdate(Tag.PerformedProcedureStepEndDate, DateTimeOffset.Now.ToString("yyyyMMdd"));
    completed.AddOrUpdate(Tag.PerformedProcedureStepEndTime, DateTimeOffset.Now.ToString("HHmmss"));
    completed.AddOrUpdate(new Sequence(Tag.PerformedSeriesSequence, [series]));
    return completed;
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Отправить IN PROGRESS, затем COMPLETED — C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "RIS_AE",
        Calling = "CT01",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.20"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityPerformedProcedureStepSOPClass,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddNCreateHandler((request, response, cancellationToken) =&gt;
    {
        Console.WriteLine($"N-CREATE: 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .AddNSetHandler((request, response, cancellationToken) =&gt;
    {
        Console.WriteLine($"N-SET: 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .Build();

// The modality chooses the UID of the performed step; N-SET refers to it later
Uid mppsUid = Uid.CreateDerivedFromUuid();

client.QueueRequest(new NCreateRequest(Uid.ModalityPerformedProcedureStepSOPClass, mppsUid, InProgress(item, "CT01")));
await client.SendAsync(CancellationToken.None);

// ... the scan runs ...

client.QueueRequest(new NSetRequest(Uid.ModalityPerformedProcedureStepSOPClass, mppsUid, Completed("1.2.826.0.1.3680043.10.1.1", "CHEST")));
await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Исследование, прерванное досрочно, завершается статусом DISCONTINUED вместо COMPLETED, и N‑SET передаёт причину в последовательность Performed Procedure Step Discontinuation Reason Code Sequence.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Тот же рабочий процесс через DICOMweb">}}

<p>Современные RIS и облачные платформы предоставляют список процедур и MPPS через HTTP. <a href="/medical/net/dicomweb-client/">DICOMweb client</a> библиотеки Aspose.Medical ищет запланированные шаги процедуры и сообщает о выполненном шаге с теми же наборами данных, что указаны выше. Ключ внутри запланированного шага процедуры записывается как путь, например <code>ScheduledProcedureStepSequence.Modality</code>.</p>

<div class="codeblock" id="code">
 <h3>Запрос списка процедур и отправка MPPS через DICOMweb — C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://ris.example.org/dicomweb/")
});

// The same keys as the C-FIND query, as query parameters
DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters =
    [
        new DicomWebQueryParameter("ScheduledProcedureStepSequence.Modality", "CT"),
        new DicomWebQueryParameter("ScheduledProcedureStepSequence.ScheduledStationAETitle", "CT01"),
        new DicomWebQueryParameter("ScheduledProcedureStepSequence.ScheduledProcedureStepStartDate", DateTimeOffset.Now.ToString("yyyyMMdd"))
    ]
};

List&lt;Dataset&gt; worklist = [];
await using (DicomWebClientStream&lt;Dataset&gt; steps = await client.ModalityWorklist.SearchAsync(search))
{
    await foreach (Dataset step in steps)
        worklist.Add(step);
}

// The same MPPS attributes go to the procedure step resource
Dataset item = worklist[0];
string mppsUid = Uid.CreateDerivedFromUuid().Code;
await client.ModalityPerformedProcedureSteps.CreateAsync(mppsUid, InProgress(item, "CT01"));
await client.ModalityPerformedProcedureSteps.UpdateAsync(mppsUid, Completed("1.2.826.0.1.3680043.10.1.2", "CHEST"));</code></pre>
</div>

<p>Клиент работает на вашем собственном <code>HttpClient</code>, поэтому аутентификация и TLS настраиваются там.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ваш собственный сервер списка процедур через DIMSE">}}

<p>Брокер списка процедур, небольшой RIS или тестовая система для интеграции модальностей должны отвечать на запросы другой стороны. Зарегистрируйте обработчик C‑FIND для списка процедур и обработчики N‑CREATE и N‑SET для MPPS, после чего приложение станет SCP списка процедур и SCP MPPS.</p>

<div class="codeblock" id="code">
 <h3>Запуск сервера списка процедур и MPPS — C#</h3>
 <pre><code class="cs">// One handler instance answers N-CREATE and N-SET, so both see the same steps
MppsHandler mpps = new MppsHandler();

DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Any, 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityWorklistInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityPerformedProcedureStepSOPClass,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddSingletonCFindHandler(new WorklistHandler("worklist"))
    .AddSingletonNCreateHandler(mpps)
    .AddSingletonNSetHandler(mpps)
    .Build();

await server.StartAsync(CancellationToken.None);</code></pre>
</div>

<p>Этот обработчик читает запланированные исследования из файлов <code>.wl</code>, по одному набору данных на файл, и сравнивает три ключа, обычно отправляемые модальностью. Через DIMSE обработчик управляет сопоставлением: шаблоны, диапазоны дат и набор возвращаемых ключей задаются в нём.</p>

<div class="codeblock" id="code">
 <h3>Обработчик C‑FIND списка процедур — C#</h3>
 <pre><code class="cs">// Answers worklist queries from the .wl files in a folder, one scheduled exam per file
public sealed class WorklistHandler(string folder) : ICFindRequestHandler
{
    public async IAsyncEnumerable&lt;CFindResponse&gt; Handle(CFindRequest request,
        [EnumeratorCancellation] CancellationToken cancellationToken)
    {
        // A modality filters inside the scheduled procedure step: its AE title, its modality, the date
        Dataset keys = request.Identifier.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);
        Tag[] filters = [Tag.ScheduledStationAETitle, Tag.Modality, Tag.ScheduledProcedureStepStartDate];

        foreach (string path in Directory.EnumerateFiles(folder, "*.wl"))
        {
            DicomFile file = await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
            Dataset step = file.Dataset.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);
            if (filters.All(tag =&gt; Matches(keys, step, tag)))
                yield return WithStatus(new CFindResponse(file.Dataset), 0xFF00);
        }

        yield return WithStatus(new CFindResponse(null), 0x0000);
    }

    // Single value matching: an empty key matches every value
    private static bool Matches(Dataset keys, Dataset step, Tag tag)
    {
        string key = keys.GetSingleValueOrDefault(tag, string.Empty)!;
        return key.Length == 0 || key == step.GetSingleValueOrDefault(tag, string.Empty);
    }

    private static CFindResponse WithStatus(CFindResponse response, ushort status)
    {
        response.Command.AddOrUpdate(Tag.Status, status);
        return response;
    }
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Обработчик MPPS N‑CREATE и N‑SET — C#</h3>
 <pre><code class="cs">// Receives the performed procedure steps the modalities report
public sealed class MppsHandler : INCreateRequestHandler, INSetRequestHandler
{
    private readonly ConcurrentDictionary&lt;string, Dataset&gt; _steps = new();

    // N-CREATE: the exam has started
    public ValueTask&lt;NCreateResponse&gt; Handle(NCreateRequest request, CancellationToken cancellationToken)
    {
        bool created = _steps.TryAdd(request.AffectedSopInstanceUid.Code, request.Attributes ?? new Dataset());

        NCreateResponse response = new(null);
        response.Command.AddOrUpdate(Tag.Status, created ? (ushort)0x0000 : (ushort)0x0111); // 0x0111: duplicate instance
        return ValueTask.FromResult(response);
    }

    // N-SET: the exam is COMPLETED or DISCONTINUED
    public ValueTask&lt;NSetResponse&gt; Handle(NSetRequest request, CancellationToken cancellationToken)
    {
        NSetResponse response = new(null);
        if (!_steps.TryGetValue(request.RequestedSopInstanceUid.Code, out Dataset? step))
        {
            response.Command.AddOrUpdate(Tag.Status, (ushort)0x0112); // 0x0112: no such instance
            return ValueTask.FromResult(response);
        }

        step.AddOrUpdateRange(request.ModificationList);
        Console.WriteLine($"{step.GetSingleValueOrDefault(Tag.PatientID, string.Empty)}: " +
                          $"{step.GetSingleValueOrDefault(Tag.PerformedProcedureStepStatus, string.Empty)}");

        response.Command.AddOrUpdate(Tag.Status, (ushort)0x0000);
        return ValueTask.FromResult(response);
    }
}</code></pre>
</div>

<p>Здесь шаги хранятся в памяти, а состояние выводится. Реальный MPPS SCP сохраняет шаг, отклоняет изменения для шага, уже находящегося в статусе COMPLETED или DISCONTINUED, и передаёт результат в RIS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ваш собственный сервер списка процедур через DICOMweb">}}

<p>При работе через DICOMweb библиотека берёт на себя большую часть серверной логики. Для поиска списка процедур она применяет правила сопоставления DICOM, включая шаблоны и диапазоны дат, и постранично выводит результаты. Для MPPS она проверяет обязательные атрибуты и переходы состояний. Ваши классы предоставляют данные.</p>

<div class="codeblock" id="code">
 <h3>Program.cs DICOMweb‑сервера списка процедур и MPPS — C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// The scheduled exams and the reported procedure steps
builder.Services.AddSingleton&lt;IDicomWebModalityWorklistProvider&gt;(new FolderWorklist("worklist"));
builder.Services.AddSingleton&lt;IDicomWebMppsProvider, MppsStore&gt;();
builder.Services.AddDicomWeb()
    .AddStandardModalityWorklistService()
    .AddStandardModalityPerformedProcedureStepService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Хранилище списка процедур и MPPS — C#</h3>
 <pre><code class="cs">// The scheduled exams for DICOMweb searches; the library applies the matching rules and the paging
public sealed class FolderWorklist(string folder) : IDicomWebModalityWorklistProvider
{
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Directory.EnumerateFiles(folder, "*.wl").Order(StringComparer.Ordinal))
            yield return (await DicomFile.OpenAsync(path, cancellationToken: cancellationToken)).Dataset;
    }
}

// The performed procedure steps; the library checks the attributes and the state changes
public sealed class MppsStore : IDicomWebMppsProvider
{
    private readonly ConcurrentDictionary&lt;string, DicomWebVersionedDataset&gt; _steps = new();

    public ValueTask&lt;DicomWebVersionedDataset?&gt; GetAsync(string mppsUid, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_steps.TryGetValue(mppsUid, out DicomWebVersionedDataset? step) ? Copy(step) : null);

    public ValueTask&lt;bool&gt; TryCreateAsync(string mppsUid, Dataset procedureStep, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_steps.TryAdd(mppsUid, NewRevision(procedureStep)));

    // Replaces the step only if nobody changed it since it was read
    public ValueTask&lt;bool&gt; TryReplaceAsync(string mppsUid, string expectedRevision, Dataset procedureStep,
        CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_steps.TryGetValue(mppsUid, out DicomWebVersionedDataset? current)
                             &amp;&amp; current.Revision == expectedRevision
                             &amp;&amp; _steps.TryUpdate(mppsUid, NewRevision(procedureStep), current));

    private static DicomWebVersionedDataset NewRevision(Dataset dataset) =&gt;
        new() { Dataset = new Dataset(dataset), Revision = Guid.NewGuid().ToString("N") };

    private static DicomWebVersionedDataset Copy(DicomWebVersionedDataset step) =&gt;
        new() { Dataset = new Dataset(step.Dataset), Revision = step.Revision };
}</code></pre>
</div>

<p>Серверу требуется пакет <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. На <a href="/medical/net/dicomweb-server/">странице DICOMweb server</a> показано, как добавить сервисы Studies в то же приложение, а руководство <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">по провайдерам workflow</a> объясняет контракт ревизии метода <code>TryReplaceAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Списки процедур за пределами модальности">}}

<p>MWL и MPPS соединяют модальность с RIS. Для задач, перемещающихся между другими системами, такими как обработка ИИ, распределение чтения или постобработка, DICOM определяет Unified Procedure Step. На <a href="/medical/net/ups-rs/">странице UPS‑RS</a> показано, как создавать, брать в работу и завершать такие элементы работы с помощью той же библиотеки.</p>

<p>Сопутствующие страницы: <a href="/medical/net/dicom-networking/">DICOM networking</a> для C‑ECHO, C‑STORE и других сервисов DIMSE, <a href="/medical/net/dicomweb-client/">DICOMweb client</a> и <a href="/medical/net/dicomweb-server/">DICOMweb server</a> для исследований по HTTP, а также <a href="/medical/net/dicom-tags/">DICOM tags</a> для просмотра содержимого элемента списка процедур.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Обучающие ресурсы" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Руководство разработчика" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="Справочники API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Поддержка продукта" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Бесплатная поддержка" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Платная поддержка" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Блог" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Почему Aspose.Medical для .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Список клиентов" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Истории успеха" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
