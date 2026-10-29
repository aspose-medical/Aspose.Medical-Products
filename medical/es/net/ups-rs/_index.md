---
title: UPS-RS en C# .NET - Workitems del DICOMweb Unified Procedure Step | Aspose.Medical
weight: 9400

description: Cree, reclame y complete workitems UPS de DICOM desde C#, reciba cambios de estado mediante WebSocket, o ejecute su propio servidor UPS-RS en ASP.NET Core con Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS para .NET C#" h2="Distribuya trabajo de imágenes entre sistemas con el DICOM Unified Procedure Step: cree workitems, permita que los workers los reclamen y los completen, y siga cada cambio, ya sea como cliente o como servidor." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Una cola de trabajo que habla DICOM">}}

<p>Un modelo de IA debe procesar cada nueva tomografía axial computarizada de tórax. Un servicio de teleradiología asigna estudios a los lectores. Un laboratorio 3D recibe pedidos de post‑procesamiento. Cada uno de estos es una cola de tareas entre sistemas, y DICOM define una para ello: el Unified Procedure Step (UPS), ofrecido mediante HTTP por el servicio UPS-RS de DICOMweb.</p>

<p>Un workitem UPS indica qué hacer, para qué paciente y con qué entrada. Está SCHEDULED cuando se crea. Un worker lo reclama y lo pone IN PROGRESS, luego lo finaliza como COMPLETED, o termina como CANCELED. Otros sistemas se suscriben y reciben cada cambio de estado mediante un WebSocket, de modo que nadie tiene que hacer polling.</p>

<p><strong>Aspose.Medical para .NET</strong> implementa ambos lados en C# administrado: un cliente tipado para cualquier servidor UPS-RS, y el lado del servidor para ASP.NET Core, donde la biblioteca verifica las reglas UPS y su código almacena los workitems.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Programar un workitem">}}

<p>El sistema que requiere que se realice el trabajo crea el workitem. Su SOP Instance UID lo identifica a partir de ese momento; la etiqueta, la etiqueta de la lista de trabajo y la prioridad indican a los workers qué es.</p>

<div class="codeblock" id="code">
 <h3>Crear un workitem UPS - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Reclámalo, realiza el trabajo, complétalo">}}

<p>Un worker busca en la lista de trabajo lo que puede hacer y reclama un workitem moviéndolo a IN PROGRESS con un UID de transacción que ha generado. Desde ese momento solo el poseedor de este UID puede actualizar o finalizar el workitem, por lo que dos workers nunca procesan la misma tarea.</p>

<div class="codeblock" id="code">
 <h3>Buscar, reclamar y completar workitems programados - C#</h3>
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

<p>COMPLETED necesita un registro del trabajo: quién lo realizó, qué se realizó, la hora de inicio y fin, y el resultado. El worker lo almacena con una actualización antes de que cambie el estado.</p>

<div class="codeblock" id="code">
 <h3>Los datos del procedimiento realizado - C#</h3>
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

<p>Un servidor construido con Aspose.Medical hace cumplir estas reglas. Un segundo worker que intenta reclamar un workitem en progreso recibe HTTP 409, al igual que un cambio de estado sobre uno completado. Una solicitud COMPLETED sin los datos del procedimiento realizado recibe HTTP 400. El cliente informa esas respuestas como una <code>DicomWebClientException</code> con la respuesta HTTP.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Siga la lista de trabajo mediante WebSocket">}}

<p>Un panel, el sistema que ordenó el trabajo o un worker que espera nuevas tareas se suscribe con su título AE y abre una conexión de notificaciones. Cada informe de evento llega como un conjunto de datos con el UID del workitem y su nuevo estado.</p>

<div class="codeblock" id="code">
 <h3>Recibir informes de eventos UPS - C#</h3>
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

<p>Además de la lista completa, un cliente puede suscribirse a un workitem con <code>SubscribeWorkitemAsync</code> o a una parte filtrada de la lista con <code>SubscribeFilteredWorklistAsync</code>, si el servidor admite filtros. Cancelar una recepción cierra la conexión; abra una nueva con <code>ConnectNotificationsAsync</code> para continuar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Su propio servidor UPS-RS">}}

<p>El paquete <code>Aspose.Medical.Dicom.Web.AspNetCore</code> agrega UPS-RS a un servicio ASP.NET Core. La biblioteca maneja cada solicitud: verifica la carga útil de un nuevo workitem, las transiciones de estado, el UID de transacción del worker y los datos que COMPLETED requiere, y aplica las reglas de coincidencia DICOM a las búsquedas. Su código almacena los workitems.</p>

<div class="codeblock" id="code">
 <h3>Program.cs de un servidor UPS-RS - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>El almacenamiento de workitems - C#</h3>
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

<p>Cada workitem almacenado tiene una revisión. <code>TryReplaceAsync</code> lo reemplaza solo cuando la revisión sigue siendo la que la biblioteca leyó, por lo que dos solicitudes que modifican el mismo workitem al mismo tiempo no pueden sobrescribirse; la biblioteca vuelve a intentar o informa un conflicto. Reemplace el diccionario con su base de datos y mantenga esa verificación.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Notificaciones en el servidor">}}

<p>Para enviar informes de eventos, registre un <code>IDicomWebSubscriptionProvider</code> y llame a <code>app.UseWebSockets()</code> antes de <code>MapDicomWeb</code>. El proveedor mantiene las suscripciones y una cola de informes de eventos por suscriptor, y la biblioteca le entrega un informe de evento cada vez que un workitem cambia. El proveedor también informa a la biblioteca qué título AE tiene un llamador autenticado, porque los informes de eventos nombran el sistema que cambió el estado.</p>

<p>The <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">guía de proveedores de flujo de trabajo</a> contiene un proveedor de suscripción en memoria completo, y la <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">guía de ASP.NET Core</a> cubre la configuración de WebSocket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL y MPPS">}}

<p>UPS cubre lo que hacen el antiguo Modality Worklist y MPPS entre un RIS y una modalidad, y agrega workers que no son modalidades, reclamando y enviando notificaciones. Muchas modalidades aún solo usan MWL y MPPS sobre DIMSE; la <a href="/medical/net/modality-worklist/">página del modality worklist</a> muestra ese flujo de trabajo con la misma biblioteca.</p>

<p>Páginas relacionadas: <a href="/medical/net/dicomweb-client/">cliente DICOMweb</a> para los otros servicios DICOMweb, <a href="/medical/net/dicomweb-server/">servidor DICOMweb</a> para QIDO-RS, WADO-RS y STOW-RS en el mismo servidor, y <a href="/medical/net/dicom-networking/">red DICOM</a> para DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guía del desarrollador" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="Referencias de API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Soporte del producto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Soporte gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Soporte de pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="¿Por qué Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Casos de éxito" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
