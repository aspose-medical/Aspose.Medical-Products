---
title: UPS-RS in C# .NET - DICOMweb Unified Procedure Step Arbeitsobjekte | Aspose.Medical
weight: 9400

description: Erstellen, beanspruchen und abschließen von DICOM UPS Workitems aus C#, Statusänderungen über WebSocket empfangen oder Ihren eigenen UPS-RS-Server auf ASP.NET Core mit Aspose.Medical betreiben.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS für .NET C#" h2="Verteilen Sie Bildgebungsaufgaben zwischen Systemen mit dem DICOM Unified Procedure Step: Erstellen Sie Workitems, lassen Sie Arbeiter sie beanspruchen und abschließen und verfolgen Sie jede Änderung, entweder als Client oder als Server." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Eine Arbeitswarteschlange, die DICOM versteht">}}

<p>Ein KI‑Modell muss jede neue Brust‑CT verarbeiten. Ein Tele‑Radiologie‑Dienst weist Studien Lesern zu. Ein 3D‑Labor erhält Nachbearbeitungsaufträge. Jeder dieser Vorgänge stellt eine Aufgabenwarteschlange zwischen Systemen dar, und DICOM definiert dafür einen: den Unified Procedure Step (UPS), der über HTTP vom UPS‑RS‑Dienst von DICOMweb bereitgestellt wird.</p>

<p>Ein UPS‑Workitem gibt an, was zu tun ist, für welchen Patienten und mit welchen Eingaben. Es ist bei Erstellung SCHEDULED. Ein Arbeiter beansprucht es und setzt es auf IN PROGRESS, beendet es anschließend als COMPLETED oder es endet als CANCELED. Andere Systeme abonnieren und erhalten jede Statusänderung über einen WebSocket, sodass niemand polling betreiben muss.</p>

<p><strong>Aspose.Medical für .NET</strong> implementiert beide Seiten in verwaltetem C#: einen typisierten Client für jeden UPS‑RS‑Server und die Serverseite für ASP.NET Core, wobei die Bibliothek die UPS‑Regeln prüft und Ihr Code die Workitems speichert.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Workitem planen">}}

<p>Das System, das die Arbeit ausführen möchte, erstellt das Workitem. Seine SOP‑Instance‑UID identifiziert es von da an; das Label, das Worklist‑Label und die Priorität informieren die Arbeiter darüber, was es ist.</p>

<div class="codeblock" id="code">
 <h3>Erstellen eines UPS‑Workitems – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Beanspruchen, Arbeit ausführen, abschließen">}}

<p>Ein Arbeiter durchsucht die Worklist nach Aufgaben, die er erledigen kann, und beansprucht ein Workitem, indem er es mit einer von ihm erzeugten Transaction‑UID auf IN PROGRESS setzt. Ab diesem Moment kann nur der Inhaber dieser UID das Workitem aktualisieren oder abschließen, sodass zwei Arbeiter niemals dieselbe Aufgabe bearbeiten.</p>

<div class="codeblock" id="code">
 <h3>Geplante Workitems finden, beanspruchen und abschließen – C#</h3>
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

<p>COMPLETED erfordert eine Aufzeichnung der Arbeit: wer sie durchgeführt hat, was durchgeführt wurde, Start‑ und Endzeit sowie das Ergebnis. Der Arbeiter speichert diese Information mit einem Update, bevor der Status geändert wird.</p>

<div class="codeblock" id="code">
 <h3>Die Daten des durchgeführten Verfahrens – C#</h3>
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

<p>Ein mit Aspose.Medical erstellter Server setzt diese Regeln durch. Ein zweiter Arbeiter, der versucht, ein bereits in Bearbeitung befindliches Workitem zu beanspruchen, erhält HTTP 409, ebenso eine Statusänderung bei einem bereits abgeschlossenen Workitem. Eine COMPLETED‑Anfrage ohne die Daten des durchgeführten Verfahrens liefert HTTP 400. Der Client meldet solche Antworten als <code>DicomWebClientException</code> mit der HTTP‑Antwort.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklist über WebSocket verfolgen">}}

<p>Ein Dashboard, das System, das die Arbeit bestellt hat, oder ein Arbeiter, der auf neue Aufgaben wartet, abonniert mit seinem AE‑Titel und öffnet eine Benachrichtigungsverbindung. Jeder Ereignisbericht kommt als Datensatz mit der Workitem‑UID und dem neuen Status.</p>

<div class="codeblock" id="code">
 <h3>UPS‑Ereignisberichte empfangen – C#</h3>
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

<p>Neben der gesamten Worklist kann ein Client ein einzelnes Workitem mit <code>SubscribeWorkitemAsync</code> abonnieren oder einen gefilterten Teil der Worklist mit <code>SubscribeFilteredWorklistAsync</code>, sofern der Server Filter unterstützt. Das Abbrechen einer Empfangssitzung schließt die Verbindung; öffnen Sie eine neue mit <code>ConnectNotificationsAsync</code>, um fortzufahren.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ihr eigener UPS‑RS‑Server">}}

<p>Das Paket <code>Aspose.Medical.Dicom.Web.AspNetCore</code> fügt einem ASP.NET Core‑Dienst UPS‑RS hinzu. Die Bibliothek verarbeitet jede Anfrage: Sie prüft die Nutzdaten eines neuen Workitems, die Zustandsübergänge, die Transaction‑UID des Arbeiters und die von COMPLETED benötigten Daten und wendet die DICOM‑Abgleichsregeln auf Suchvorgänge an. Ihr Code speichert die Workitems.</p>

<div class="codeblock" id="code">
 <h3>Program.cs eines UPS‑RS‑Servers – C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Der Workitem‑Speicher – C#</h3>
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

<p>Jedes gespeicherte Workitem hat eine Revision. <code>TryReplaceAsync</code> ersetzt es nur, wenn die Revision noch der von der Bibliothek gelesenen entspricht, sodass zwei Anfragen, die das gleiche Workitem gleichzeitig ändern, einander nicht überschreiben können; die Bibliothek versucht es erneut oder meldet einen Konflikt. Ersetzen Sie das Wörterbuch durch Ihre Datenbank und behalten Sie diese Prüfung bei.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Benachrichtigungen auf dem Server">}}

<p>Um Ereignisberichte zu senden, registrieren Sie einen <code>IDicomWebSubscriptionProvider</code> und rufen <code>app.UseWebSockets()</code> vor <code>MapDicomWeb</code> auf. Der Provider verwaltet die Abonnements und je einen Warteschlangen für Ereignisberichte pro Abonnenten, und die Bibliothek übergibt ihm einen Ereignisbericht, sobald sich ein Workitem ändert. Der Provider teilt der Bibliothek außerdem mit, welchen AE‑Titel ein authentifizierter Aufrufer hat, da die Ereignisberichte das System benennen, das den Status geändert hat.</p>

<p>Der <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">Leitfaden zu Workflow‑Providern</a> enthält einen vollständigen In‑Memory‑Abonnement‑Provider, und der <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET‑Core‑Leitfaden</a> behandelt die WebSocket‑Einstellungen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL und MPPS">}}

<p>UPS deckt das ab, was die ältere Modality Worklist und MPPS zwischen einem RIS und einer Modalität leisten, und fügt Arbeiter hinzu, die keine Modalitäten sind, einschließlich Beanspruchung und Benachrichtigungen. Viele Modalitäten unterstützen noch nur MWL und MPPS über DIMSE; die <a href="/medical/net/modality-worklist/">Modality‑Worklist‑Seite</a> zeigt diesen Workflow mit derselben Bibliothek.</p>

<p>Verwandte Seiten: <a href="/medical/net/dicomweb-client/">DICOMweb‑Client</a> für die anderen DICOMweb‑Dienste, <a href="/medical/net/dicomweb-server/">DICOMweb‑Server</a> für QIDO‑RS, WADO‑RS und STOW‑RS auf demselben Server, sowie <a href="/medical/net/dicom-networking/">DICOM‑Networking</a> für DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Entwicklerhandbuch" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="API‑Referenzen" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktunterstützung" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Kostenloser Support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Kostenpflichtiger Support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Warum Aspose.Medical für .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundenliste" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Erfolgsgeschichten" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
