---
title: Réseau DICOM en C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Connectez votre application .NET à un PACS. Vérifiez avec C-ECHO, envoyez des images avec C-STORE, interrogez avec C-FIND, et recevez des images avec votre propre SCP. Un client et serveur DIMSE en C# pur.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Réseau DICOM en .NET C#" h2="Communiquez avec un PACS depuis votre propre application : C-ECHO, C-STORE, C-FIND, C-MOVE et C-GET, en tant que client et serveur, en C# géré sans rien à installer sur la machine." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Connectez-vous à un PACS depuis votre propre code">}}

<p>Lire des fichiers DICOM est la moitié facile de l’imagerie médicale. Dès que votre application doit fonctionner avec un véritable système hospitalier, elle doit parler DIMSE : ouvrir une association avec un PACS, envoyer des images, demander quels examens existent, et répondre lorsqu’un autre système renvoie quelque chose.</p>

<p><strong>Aspose.Medical pour .NET</strong> intègre ce protocole dans la bibliothèque. <code>Aspose.Medical.Dicom.Network</code> vous fournit un client DIMSE et un serveur DIMSE, tous deux écrits en C# géré. Aucun kit natif à installer, aucun service à configurer et rien de spécifique à la plateforme, de sorte que le même code s’exécute sous Windows, Linux et dans un conteneur.</p>

<p>Trois éléments couvrent la plupart des intégrations, et chacun ne nécessite que quelques lignes : vérifier la connexion avec C-ECHO, pousser des images avec C-STORE, et rechercher ce qui se trouve de l’autre côté avec C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Commencez par C-ECHO">}}

<p>C-ECHO est le ping DICOM. Il confirme que l’hôte, le port et les deux titres AE sont corrects avant que tout autre problème ne soit signalé. Créez un client une fois, associez‑lui un gestionnaire qui observe la réponse, puis envoyez la requête.</p>

<div class="codeblock" id="code">
 <h3>Vérifier la connexion avec C-ECHO - C#</h3>
 <pre><code class="cs">AssociationNegotiationOptions negotiation = new AssociationNegotiationOptions()
    .WithPresentationContext(new PresentationContext
    {
        AbstractSyntax = Uid.Verification,
        Role = null,
        TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
    });

DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = negotiation
    })
    .AddCEchoHandler((request, response, cancellationToken) =>
    {
        Console.WriteLine($"C-ECHO answered with status 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(new CEchoRequest());

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Le statut <code>0x0000</code> signifie succès. Les requêtes sont mises en file d’attente puis envoyées sur une seule association, de sorte qu’un lot de travail n’ouvre pas de connexion par élément.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Envoyer des images avec C-STORE">}}

<p>C-STORE correspond à ce qu’une application fait après avoir produit ou reçu une image : elle pousse l’instance vers l’archive. Mettez une requête en file par instance et envoyez‑les ensemble.</p>

<div class="codeblock" id="code">
 <h3>Envoyer un fichier DICOM à un PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Les contextes de présentation que vous proposez décident de ce que l’archive acceptera. Si elle souhaite une syntaxe compressée, transcoder avant l’envoi, comme le montre la page <a href="/medical/net/dicom-transfer-syntax-conversion/">conversion de syntaxe de transfert</a>, ou lister les alternatives dans <code>AdditionalTransferSyntaxes</code> et laisser la négociation en choisir une.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rechercher des études avec C-FIND">}}

<p>C-FIND répond à la question « quelles études possède l’archive ». Les correspondances arrivent une à une, chacune avec son jeu de données d’identifiant, et une réponse finale ferme la requête.</p>

<div class="codeblock" id="code">
 <h3>Interroger les études par patient - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.StudyRootQueryRetrieveInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddCFindHandler((request, response, cancellationToken) =>
    {
        // A match arrives with an identifier; the final response carries the status only
        if (response.Identifier is not null)
            Console.WriteLine(response.Identifier.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty));

        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(CFindRequest.CreateStudyQuery(
    patientId: "PATIENT-001",
    patientName: null,
    studyDateTime: null,
    accession: null,
    studyId: null,
    modalitiesInStudy: null,
    studyInstanceUid: null,
    priority: DimsePriority.Medium));

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>La même fabrique construit les autres niveaux de requête : <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> et <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> crée une requête de liste de travail de modalité, qui est la requête qu’une modalité émet avant un examen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Recevoir des images : votre propre SCP de stockage">}}

<p>La bibliothèque fonctionne également comme serveur. Enregistrez un gestionnaire pour le service que vous souhaitez offrir, démarrez l’écoute, et votre application devient un nœud DICOM auquel une modalité ou un autre PACS peut envoyer des données.</p>

<div class="codeblock" id="code">
 <h3>Accepter les images entrantes - C#</h3>
 <pre><code class="cs">DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Any, 11112)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.SecondaryCaptureImageStorage,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddSingletonCStoreHandler(new StoreHandler())
    .Build();

await server.StartAsync(CancellationToken.None);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Le gestionnaire qui stocke ce qui arrive - C#</h3>
 <pre><code class="cs">public sealed class StoreHandler : ICStoreRequestHandler
{
    public ValueTask&lt;CStoreResponse&gt; Handle(CStoreRequest request, CancellationToken cancellationToken)
    {
        // Write what arrived, then answer Success
        new DicomFile(request.Dataset).Save($"{request.AffectedSopInstanceUid}.dcm");

        CStoreResponse response = new();
        response.Command.AddOrUpdate(Tag.Status, (ushort)0x0000);
        return ValueTask.FromResult(response);
    }
}</code></pre>
</div>

<p>Ce que fait le gestionnaire avec le jeu de données dépend de vous : l’écrire sur disque, le placer dans une file, l’anonymiser d’abord avec l’<a href="/medical/net/anonymization/">API d’anonymisation</a>, ou le transcoder dans la syntaxe de l’archive.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ce que couvre encore l’API réseau">}}

<p>Les trois services ci‑dessus sont les plus courants. Le reste du DIMSE est également disponible :</p>

<ul>
<li>Récupération : C-MOVE et C-GET, avec les compteurs de sous‑opérations rapportés dans les réponses.</li>
<li>Les services N : N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE et N-EVENT-REPORT, qui constituent la base de l’engagement de stockage et du MPPS.</li>
<li>Contrôle d’association : contextes de présentation, rôles de classe de service, négociation étendue, fenêtre d’opérations asynchrones, négociation d’identité utilisateur, et un point d’extensibilité de politique pouvant rejeter une association en invoquant le titre AE.</li>
<li>TLS des deux côtés, via <code>TlsInitiatorAuthenticator</code> et <code>TlsAcceptorAuthenticator</code>, avec votre propre validation de certificat si nécessaire.</li>
<li>Délais d’attente à chaque étape, de la connexion TCP à la libération, et notifications du cycle de vie de l’association afin qu’un nœud de longue durée puisse journaliser les événements.</li>
</ul>

<p>Le <a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">guide de mise en réseau DICOM</a> documente chaque option et chaque gestionnaire.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2=".NET pur, du socket aux données de pixel">}}

<p>Tous les éléments de cette page sont du code géré provenant du même package qui lit et écrit les fichiers. L’association, les codecs et l’analyseur proviennent d’une seule bibliothèque, de sorte qu’une étude reçue via le réseau peut être anonymisée, transcodée ou sérialisée sans quitter le processus et sans dépendance native à aucune étape de la chaîne.</p>

<p>Pages associées : <a href="/medical/net/dicom-transfer-syntax-conversion/">conversion de syntaxe de transfert</a> pour ce qu’il faut envoyer, <a href="/medical/net/anonymization/">anonymisation</a> pour ce qu’il faut retirer d’abord, et <a href="/medical/net/dicom-tags/">tags DICOM</a> pour lire ce qui est arrivé.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ressources d’apprentissage" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guide du développeur" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="Références API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Support produit" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Support gratuit" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Support payant" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Pourquoi Aspose.Medical pour .NET ?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Liste des clients" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Histoires de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
