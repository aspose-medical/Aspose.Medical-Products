---
title: UPS-RS в C# .NET - DICOMweb Unified Procedure Step Workitems | Aspose.Medical
weight: 9400

description: Создавайте, присваивайте и завершайте DICOM UPS workitems из C#, получайте изменения состояния через WebSocket или разверните собственный сервер UPS-RS на ASP.NET Core с Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS для .NET C#" h2="Раздавайте задачи визуализации между системами с помощью DICOM Unified Procedure Step: создавайте workitems, позволяйте исполнителям присваивать и завершать их, и отслеживайте каждое изменение, выступая в роли клиента или сервера." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Очередь задач, поддерживающая DICOM">}}

<p>Модель ИИ должна обрабатывать каждый новый грудной КТ. Служба телерадиологии назначает исследования радиологам. 3D‑лаборатория получает заказы на постобработку. Каждый из этих процессов представляет собой очередь задач между системами, и DICOM определяет её как Unified Procedure Step (UPS), предоставляемый по HTTP сервисом UPS-RS DICOMweb.</p>

<p>UPS workitem определяет, что нужно сделать, для какого пациента и с каким входом. При создании он имеет статус SCHEDULED. Исполнитель присваивает его себе и переводит в статус IN PROGRESS, затем завершает как COMPLETED, либо статус меняется на CANCELED. Другие системы подписываются и получают каждое изменение состояния через WebSocket, поэтому опрос не требуется.</p>

<p><strong>Aspose.Medical for .NET</strong> реализует обе стороны на управляемом C#: типизированный клиент для любого сервера UPS-RS и серверную часть для ASP.NET Core, где библиотека проверяет правила UPS, а ваш код сохраняет workitems.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Запланировать workitem">}}

<p>Система, желающая выполнить работу, создаёт workitem. Его SOP Instance UID идентифицирует его далее; метка, метка в worklist и приоритет сообщают исполнителям, что это за задача.</p>

<div class="codeblock" id="code">
 <h3>Создать UPS workitem - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Присвоить, выполнить работу, завершить">}}

<p>Исполнитель ищет в worklist задания, которые может выполнить, и присваивает workitem, переводя его в статус IN PROGRESS с помощью сгенерированного transaction UID. С этого момента только владелец этого UID может обновлять или завершать workitem, поэтому два исполнителя никогда не обрабатывают одну и ту же задачу.</p>

<div class="codeblock" id="code">
 <h3>Найти, присвоить и завершить запланированные workitems - C#</h3>
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

<p>Для статуса COMPLETED требуется запись о работе: кто её выполнил, что было выполнено, время начала и окончания, а также результат. Исполнитель сохраняет эти данные при обновлении до изменения состояния.</p>

<div class="codeblock" id="code">
 <h3>Данные выполненной процедуры - C#</h3>
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

<p>Сервер, построенный с помощью Aspose.Medical, соблюдает эти правила. Второй исполнитель, пытающийся присвоить workitem, находящийся в состоянии IN PROGRESS, получает HTTP 409, аналогично при попытке изменить состояние уже завершённого workitem. Запрос COMPLETED без данных выполненной процедуры возвращает HTTP 400. Клиент сообщает такие ответы как <code>DicomWebClientException</code> с HTTP‑ответом.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Отслеживание worklist через WebSocket">}}

<p>Панель мониторинга, система, заказывающая работу, или исполнитель, ожидающий новые задачи, подписываются с помощью своего AE Title и открывают соединение уведомлений. Каждый отчёт о событии приходит в виде набора данных с UID workitem и его новым состоянием.</p>

<div class="codeblock" id="code">
 <h3>Получать отчёты о событиях UPS - C#</h3>
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

<p>Помимо всей worklist, клиент может подписаться на один workitem с помощью <code>SubscribeWorkitemAsync</code> или на отфильтрованную часть worklist с помощью <code>SubscribeFilteredWorklistAsync</code>, если сервер поддерживает фильтры. Отмена получения закрывает соединение; открыть новое можно с помощью <code>ConnectNotificationsAsync</code> для продолжения.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ваш собственный сервер UPS-RS">}}

<p>Пакет <code>Aspose.Medical.Dicom.Web.AspNetCore</code> добавляет UPS-RS в сервис ASP.NET Core. Библиотека обрабатывает каждый запрос: проверяет полезные данные нового workitem, переходы состояний, transaction UID исполнителя и данные, необходимые для COMPLETED, а также применяет правила сопоставления DICOM к поискам. Ваш код сохраняет workitems.</p>

<div class="codeblock" id="code">
 <h3>Program.cs сервера UPS-RS - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Хранение workitem - C#</h3>
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

<p>Каждый сохранённый workitem имеет ревизию. <code>TryReplaceAsync</code> заменяет его только если ревизия всё ещё совпадает с той, которую библиотека считала, поэтому два запроса, изменяющие один и тот же workitem одновременно, не могут перезаписать друг друга; библиотека повторяет попытку или сообщает о конфликте. Замените словарь вашей базой данных и сохраняйте эту проверку.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Уведомления на сервере">}}

<p>Чтобы отправлять отчёты о событиях, зарегистрируйте <code>IDicomWebSubscriptionProvider</code> и вызовите <code>app.UseWebSockets()</code> перед <code>MapDicomWeb</code>. Провайдер хранит подписки и одну очередь отчётов о событиях на каждого подписчика, а библиотека передаёт ему отчёт о событии каждый раз, когда workitem меняет состояние. Провайдер также сообщает библиотеке, какой AE Title у аутентифицированного вызывающего, поскольку в отчётах о событиях указывается система, изменившая состояние.</p>

<p>В <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">руководстве по провайдерам workflow</a> содержится полностью реализованный in-memory provider подписок, а <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">руководство по ASP.NET Core</a> охватывает настройки WebSocket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL и MPPS">}}

<p>UPS охватывает функции более старых Modality Worklist и MPPS между RIS и модальностью, а также добавляет исполнителей, которые не являются модальностями, позволяя присваивать задачи и получать уведомления. Многие модальности по‑прежнему используют только MWL и MPPS по DIMSE; <a href="/medical/net/modality-worklist/">страница Modality Worklist</a> демонстрирует этот рабочий процесс с той же библиотекой.</p>

<p>Связанные страницы: <a href="/medical/net/dicomweb-client/">DICOMweb client</a> для других сервисов DICOMweb, <a href="/medical/net/dicomweb-server/">DICOMweb server</a> для QIDO-RS, WADO-RS и STOW-RS на том же сервере, а также <a href="/medical/net/dicom-networking/">DICOM networking</a> для DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Обучающие ресурсы" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Руководство разработчика" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="Справочник API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
