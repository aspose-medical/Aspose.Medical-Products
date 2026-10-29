---
title: UPS-RS em C# .NET - Workitems de Etapa de Procedimento Unificado DICOMweb | Aspose.Medical
weight: 9400

description: Crie, reivindique e conclua workitems UPS DICOM a partir de C#, receba mudanças de estado via WebSocket ou execute seu próprio servidor UPS-RS no ASP.NET Core com Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS para .NET C#" h2="Distribua trabalho de imagem entre sistemas com o DICOM Unified Procedure Step: crie workitems, permita que os workers os reivindiquem e concluam, e acompanhe cada alteração, como cliente ou como servidor." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Uma fila de trabalho que fala DICOM">}}

<p>Um modelo de IA precisa processar cada novo tomografia torácica. Um serviço de teleradiologia atribui estudos aos leitores. Um laboratório 3D recebe ordens de pós‑processamento. Cada um desses é uma fila de tarefas entre sistemas, e o DICOM define uma para isso: o Unified Procedure Step (UPS), oferecido via HTTP pelo serviço UPS‑RS do DICOMweb.</p>

<p>Um workitem UPS indica o que fazer, para qual paciente e com qual entrada. Ele está SCHEDULED quando criado. Um worker o reivindica e o coloca IN PROGRESS, depois o finaliza como COMPLETED, ou ele termina como CANCELED. Outros sistemas se inscrevem e recebem cada mudança de estado via WebSocket, de modo que ninguém precise fazer polling.</p>

<p><strong>Aspose.Medical para .NET</strong> implementa ambos os lados em C# gerenciado: um cliente tipado para qualquer servidor UPS‑RS e o lado do servidor para ASP.NET Core, onde a biblioteca verifica as regras UPS e seu código armazena os workitems.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Agendar um workitem">}}

<p>O sistema que deseja que o trabalho seja realizado cria o workitem. Seu SOP Instance UID o identifica a partir de então; o rótulo, o rótulo da lista de trabalho e a prioridade informam aos workers o que ele é.</p>

<div class="codeblock" id="code">
 <h3>Criar um workitem UPS - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Reivindique-o, faça o trabalho, conclua-o">}}

<p>Um worker pesquisa a lista de trabalho para o que pode fazer e reivindica um workitem movendo‑o para IN PROGRESS com um UID de transação que ele gerou. A partir desse momento, somente o detentor desse UID pode atualizar ou finalizar o workitem, de modo que dois workers nunca processem a mesma tarefa.</p>

<div class="codeblock" id="code">
 <h3>Localizar, reivindicar e concluir workitems agendados - C#</h3>
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

<p>COMPLETED requer um registro do trabalho: quem o executou, o que foi executado, o horário de início e término, e a saída. O worker o armazena com uma atualização antes de mudar o estado.</p>

<div class="codeblock" id="code">
 <h3>Dados do procedimento realizado - C#</h3>
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

<p>Um servidor construído com Aspose.Medical impõe essas regras. Um segundo worker que tenta reivindicar um workitem em progresso recebe HTTP 409, e o mesmo ocorre com uma mudança de estado em um workitem concluído. Uma requisição COMPLETED sem os dados do procedimento realizado recebe HTTP 400. O cliente relata tais respostas como uma <code>DicomWebClientException</code> com a resposta HTTP.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Acompanhe a lista de trabalho via WebSocket">}}

<p>Um painel, o sistema que solicitou o trabalho ou um worker aguardando novas tarefas se inscreve com seu título AE e abre uma conexão de notificação. Cada relatório de evento chega como um dataset com o UID do workitem e seu novo estado.</p>

<div class="codeblock" id="code">
 <h3>Receber relatórios de eventos UPS - C#</h3>
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

<p>Além da lista de trabalho completa, um cliente pode inscrever‑se a um único workitem usando <code>SubscribeWorkitemAsync</code> ou a uma parte filtrada da lista usando <code>SubscribeFilteredWorklistAsync</code>, se o servidor suportar filtros. Cancelar uma recepção fecha a conexão; abra uma nova com <code>ConnectNotificationsAsync</code> para continuar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Seu próprio servidor UPS‑RS">}}

<p>O pacote <code>Aspose.Medical.Dicom.Web.AspNetCore</code> adiciona UPS‑RS a um serviço ASP.NET Core. A biblioteca trata cada requisição: verifica a carga útil de um novo workitem, as transições de estado, o UID de transação do worker e os dados exigidos por COMPLETED, e aplica as regras de correspondência DICOM às pesquisas. Seu código armazena os workitems.</p>

<div class="codeblock" id="code">
 <h3>Program.cs de um servidor UPS‑RS - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>O armazenamento de workitems - C#</h3>
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

<p>Cada workitem armazenado possui uma revisão. <code>TryReplaceAsync</code> o substitui apenas quando a revisão ainda é a que a biblioteca leu, de modo que duas requisições que alterem o mesmo workitem ao mesmo tempo não possam sobrescrevê‑lo; a biblioteca tenta novamente ou relata um conflito. Substitua o dicionário pelo seu banco de dados e mantenha essa verificação.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Notificações no servidor">}}

<p>Para enviar relatórios de eventos, registre um <code>IDicomWebSubscriptionProvider</code> e chame <code>app.UseWebSockets()</code> antes de <code>MapDicomWeb</code>. O provedor mantém as inscrições e uma fila de relatórios de eventos por assinante, e a biblioteca entrega a ele um relatório sempre que um workitem muda. O provedor também informa à biblioteca qual título AE um chamador autenticado possui, pois os relatórios de eventos nomeiam o sistema que alterou o estado.</p>

<p>O <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">guia de provedores de fluxo de trabalho</a> contém um provedor de assinatura em memória completo, e o <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">guia ASP.NET Core</a> cobre as configurações de WebSocket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL e MPPS">}}

<p>UPS cobre o que o antigo Modality Worklist e MPPS fazem entre um RIS e uma modalidade, e adiciona workers que não são modalidades, com reivindicação e notificações. Muitas modalidades ainda utilizam apenas MWL e MPPS sobre DIMSE; a <a href="/medical/net/modality-worklist/">página de lista de trabalho de modalidade</a> demonstra esse fluxo de trabalho com a mesma biblioteca.</p>

<p>Páginas relacionadas: <a href="/medical/net/dicomweb-client/">cliente DICOMweb</a> para os outros serviços DICOMweb, <a href="/medical/net/dicomweb-server/">servidor DICOMweb</a> para QIDO-RS, WADO-RS e STOW-RS no mesmo servidor, e <a href="/medical/net/dicom-networking/">rede DICOM</a> para DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizado" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentação" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guia do Desenvolvedor" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="Referências da API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Suporte ao Produto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Suporte Gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Suporte Pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Por que Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de Clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Casos de Sucesso" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
