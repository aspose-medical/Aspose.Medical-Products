---
title: UPS-RS en C# .NET - Workitems d'étape de procédure unifiée DICOMweb | Aspose.Medical
weight: 9400

description: Créez, réservez et terminez des workitems UPS DICOM depuis C#, recevez les changements d'état via WebSocket, ou exécutez votre propre serveur UPS-RS sur ASP.NET Core avec Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS pour .NET C#" h2="Distribuez les travaux d'imagerie entre systèmes avec le DICOM Unified Procedure Step : créez des workitems, laissez les travailleurs les réserver et les terminer, et suivez chaque modification, en tant que client ou serveur." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Une file d'attente de travail qui parle DICOM">}}

<p>Un modèle d'IA doit traiter chaque nouveau scanner thoracique. Un service de téléradiologie attribue les études aux lecteurs. Un laboratoire 3D reçoit des ordres de post‑traitement. Chacun de ces éléments constitue une file d’attente de tâches entre systèmes, et DICOM en définit une : le Unified Procedure Step (UPS), proposé via HTTP par le service UPS‑RS de DICOMweb.</p>

<p>Un workitem UPS indique quoi faire, pour quel patient et avec quelles données d’entrée. Il est SCHEDULED lorsqu’il est créé. Un travailleur le réserve et le passe à IN PROGRESS, puis le termine en COMPLETED, ou il se termine en CANCELED. D’autres systèmes s’abonnent et reçoivent chaque changement d’état via WebSocket, ainsi personne n’a besoin de sonder.</p>

<p><strong>Aspose.Medical for .NET</strong> implémente les deux côtés en C# géré : un client typé pour tout serveur UPS‑RS, et le côté serveur pour ASP.NET Core, où la bibliothèque vérifie les règles UPS et votre code stocke les workitems.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Planifier un workitem">}}

<p>Le système qui souhaite que le travail soit effectué crée le workitem. Son SOP Instance UID l’identifie à partir de ce moment ; le libellé, le libellé de la worklist et la priorité indiquent aux travailleurs de quoi il s’agit.</p>

<div class="codeblock" id="code">
 <h3>Créer un workitem UPS - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Réservez‑le, effectuez le travail, terminez‑le">}}

<p>Un travailleur parcourt la worklist pour identifier ce qu’il peut faire et réserve un workitem en le passant à IN PROGRESS avec un UID de transaction qu’il a généré. À partir de ce moment, seul le détenteur de cet UID peut mettre à jour ou terminer le workitem, ainsi deux travailleurs ne traitent jamais la même tâche.</p>

<div class="codeblock" id="code">
 <h3>Trouver, réserver et terminer les workitems planifiés - C#</h3>
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

<p>COMPLETED nécessite un enregistrement du travail : qui l’a réalisé, ce qui a été réalisé, l’heure de début et de fin, ainsi que le résultat. Le travailleur le stocke avec une mise à jour avant que l’état ne change.</p>

<div class="codeblock" id="code">
 <h3>Les données de la procédure effectuée - C#</h3>
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

<p>Un serveur construit avec Aspose.Medical applique ces règles. Un second travailleur qui tente de réserver un workitem en cours reçoit HTTP 409, tout comme un changement d’état sur un workitem terminé. Une requête COMPLETED sans les données de la procédure effectuée reçoit HTTP 400. Le client signale ces réponses sous forme de <code>DicomWebClientException</code> avec la réponse HTTP.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Suivre la worklist via WebSocket">}}

<p>Un tableau de bord, le système qui a commandé le travail ou un travailleur en attente de nouvelles tâches s’abonne avec son titre AE et ouvre une connexion de notification. Chaque rapport d’événement arrive sous forme d’ensemble de données contenant le UID du workitem et son nouvel état.</p>

<div class="codeblock" id="code">
 <h3>Recevoir les rapports d'événement UPS - C#</h3>
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

<p>En plus de la worklist complète, un client peut s’abonner à un workitem avec <code>SubscribeWorkitemAsync</code> ou à une partie filtrée de la worklist avec <code>SubscribeFilteredWorklistAsync</code>, si le serveur prend en charge les filtres. Annuler une réception ferme la connexion ; ouvrez‑en une nouvelle avec <code>ConnectNotificationsAsync</code> pour continuer.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Votre propre serveur UPS‑RS">}}

<p>Le package <code>Aspose.Medical.Dicom.Web.AspNetCore</code> ajoute UPS‑RS à un service ASP.NET Core. La bibliothèque gère chaque requête : elle vérifie la charge utile d’un nouveau workitem, les transitions d’état, l’UID de transaction du travailleur et les données requises par COMPLETED, et elle applique les règles de correspondance DICOM aux recherches. Votre code stocke les workitems.</p>

<div class="codeblock" id="code">
 <h3>Program.cs d’un serveur UPS‑RS - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Le stockage des workitems - C#</h3>
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

<p>Chaque workitem stocké possède une révision. <code>TryReplaceAsync</code> le remplace uniquement lorsque la révision est toujours celle que la bibliothèque a lue, ainsi deux requêtes modifiant le même workitem au même moment ne peuvent pas s’écraser ; la bibliothèque réessaie ou signale un conflit. Remplacez le dictionnaire par votre base de données et conservez cette vérification.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Notifications sur le serveur">}}

<p>Pour envoyer des rapports d’événement, enregistrez un <code>IDicomWebSubscriptionProvider</code> et appelez <code>app.UseWebSockets()</code> avant <code>MapDicomWeb</code>. Le fournisseur conserve les abonnements et une file de rapports d’événement par abonné, et la bibliothèque lui transmet un rapport d’événement chaque fois qu’un workitem change. Le fournisseur indique également à la bibliothèque le titre AE d’un appelant authentifié, car les rapports d’événement nomment le système qui a modifié l’état.</p>

<p>The <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">guide des fournisseurs de workflow</a> contient un fournisseur d’abonnement en mémoire complet, et le <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">guide ASP.NET Core</a> couvre les paramètres WebSocket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL et MPPS">}}

<p>UPS couvre ce que les anciens Modality Worklist et MPPS font entre un RIS et une modalité, et ajoute des travailleurs qui ne sont pas des modalités, avec réservation et notifications. De nombreuses modalités ne parlent encore que MWL et MPPS via DIMSE ; la <a href="/medical/net/modality-worklist/">page Modality Worklist</a> montre ce flux de travail avec la même bibliothèque.</p>

<p>Pages liées : <a href="/medical/net/dicomweb-client/">client DICOMweb</a> pour les autres services DICOMweb, <a href="/medical/net/dicomweb-server/">serveur DICOMweb</a> pour QIDO-RS, WADO-RS et STOW-RS sur le même serveur, et <a href="/medical/net/dicom-networking/">mise en réseau DICOM</a> pour DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ressources d’apprentissage" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guide du développeur" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="Références API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Support produit" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Support gratuit" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Support payant" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Pourquoi Aspose.Medical pour .NET ?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Liste des clients" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Histoires à succès" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
