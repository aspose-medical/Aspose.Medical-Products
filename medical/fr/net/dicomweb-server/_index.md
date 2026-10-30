---
title: Serveur DICOMweb en C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Créez votre propre serveur DICOMweb ou PACS en C#. Aspose.Medical implémente QIDO-RS, WADO-RS, STOW-RS et les services de liste de travail sur ASP.NET Core ; vous connectez le stockage.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Serveur DICOMweb pour .NET C#" h2="Ajoutez des points de terminaison QIDO-RS, WADO-RS et STOW-RS à un service ASP.NET Core. La bibliothèque parle le protocole ; votre code décide où les études sont stockées." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Votre propre serveur DICOMweb en C#">}}

<p>Un PACS, un VNA, une archive cloud ou une plateforme de recherche doit répondre aux requêtes DICOMweb : recherches QIDO-RS, téléchargements WADO-RS et téléchargements STOW-RS, et souvent aussi les services de liste de travail. Chacun d'eux possède ses propres routes, types de média, corps multipart, règles de correspondance et codes d'état.</p>

<p><strong>Aspose.Medical for .NET</strong> implémente ce côté protocolaire depuis la version 26.10. Il lit chaque requête, la vérifie par rapport à la norme, fait correspondre les clés de recherche selon les règles DICOM, pagine les résultats, convertit les syntaxes de transfert et écrit la réponse. Votre code fait ce que vous seul pouvez faire : stocker une instance, trouver des candidats pour une recherche et renvoyer les fichiers stockés. Les visionneuses, modalités et autres archives fonctionnent alors avec votre serveur comme avec n'importe quel autre serveur DICOMweb, et il en va de même pour le <a href="/medical/net/dicomweb-client/">client DICOMweb</a> de la même bibliothèque.</p>

<p>Le serveur est géré en C#. Il fonctionne partout où ASP.NET Core s'exécute : sous Windows, sous Linux et dans un conteneur.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Un serveur complet sous ASP.NET Core">}}

<p>Installez le package <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Il apporte Aspose.Medical et connecte les services DICOMweb à l’injection de dépendances et au routage des points de terminaison. Ce <code>Program.cs</code> est un serveur Studies fonctionnel ; les deux classes qu’il enregistre sont affichées ci‑dessous.</p>

<div class="codeblock" id="code">
 <h3>Program.cs d'un serveur DICOMweb - C#</h3>
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

<p>Chaque route Studies répond désormais sous <code>/dicomweb</code> : <code>/dicomweb/studies</code> pour la recherche et le téléchargement, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> pour la récupération, ainsi que les routes pour les métadonnées, les images, les données volumineuses et les images rendues. Le document de capacités à la racine du service répertorie exactement les transactions prises en charge par le serveur.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Connectez votre stockage">}}

<p>Le service Studies nécessite une interface avec trois méthodes : stocker une instance, renvoyer les fichiers stockés et renvoyer les candidats de recherche. Cette implémentation garde chaque instance comme un fichier dans un dossier par étude et série.</p>

<div class="codeblock" id="code">
 <h3>Un archive dossier derrière QIDO-RS, WADO-RS et STOW-RS - C#</h3>
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

<p>La bibliothèque vérifie les UID dans chaque chemin de requête et dans chaque instance téléchargée avant d’appeler le fournisseur. Une instance sans UID valides est signalée comme échouée et n’atteint jamais <code>StoreAsync</code>.</p>

<p>Un dossier suffit pour tester le serveur. Une archive de production conserve les mêmes trois méthodes et place un magasin d'objets derrière <code>StoreAsync</code> et <code>RetrieveAsync</code> ainsi qu’un index de base de données derrière <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Recherche : la bibliothèque applique les règles de correspondance DICOM">}}

<p>La correspondance QIDO-RS est plus qu’une comparaison de chaînes : jokers, intervalles de dates et heures, listes d’UID, noms de personnes approximatifs, valeurs vides et clés à l’intérieur de séquences. Le fournisseur ne met en œuvre aucune de ces fonctionnalités. Il renvoie les candidats dans un ordre stable, et peut renvoyer plus que la correspondance. La bibliothèque applique la correspondance DICOM exacte à chaque candidat, conserve les champs demandés et applique <code>offset</code> et <code>limit</code>. Une réponse contient par défaut au plus 1000 résultats, et <code>DicomWebStandardServiceOptions</code> modifie cette limite.</p>

<p>Pour garder les recherches rapides sur une grande archive, transmettez les clés sélectives à la base de données. <code>search.Plan.Filter</code> contient les clés de recherche compilées, avec l’attribut, le type de correspondance et les valeurs de chacune :</p>

<div class="codeblock" id="code">
 <h3>Lire un ID patient exact depuis une recherche - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Une requête de base de données construite de cette manière peut renvoyer des lignes supplémentaires, par exemple lorsqu’elle ignore une clé qu’elle ne peut traduire. La correspondance finale dans la bibliothèque les élimine. Elle ne doit pas supprimer les correspondances réelles, car la bibliothèque ne peut pas les réintégrer.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stockage : un résultat pour chaque instance">}}

<p>Une requête STOW-RS peut contenir des centaines d’instances. La bibliothèque analyse le corps multipart et appelle <code>StoreAsync</code> pour chaque instance à son tour, de sorte que le téléchargement complet n’est jamais conservé en mémoire d’un seul coup. Pour chaque instance, le fournisseur renvoie <code>Stored</code>, <code>Warning</code> pour une instance stockée avec modifications, ou <code>Failed</code> avec un code de raison DICOM optionnel.</p>

<p>La bibliothèque crée le rapport STOW-RS à partir de ces résultats et sélectionne le statut : 200 lorsque chaque instance est stockée, 202 lorsque certaines ont des avertissements ou ont échoué, 409 lorsque toutes ont échoué. Les instances dans une requête pour une étude nommée qui appartiennent à une autre étude sont refusées avant que le fournisseur ne soit appelé.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Récupération : syntaxes de transfert, cadres et images rendues">}}

<p>Le fournisseur renvoie les fichiers stockés, et la bibliothèque les délivre sous la forme demandée par le client. Un client WADO-RS peut demander une syntaxe de transfert dans l’en-tête <code>Accept</code>, et la bibliothèque convertit chaque fichier avant de l’envoyer, avec ses propres codecs, HTJ2K et JPEG XL inclus. Les métadonnées en DICOM JSON ou XML, les données volumineuses et les cadres uniques proviennent du même fournisseur, sans code supplémentaire.</p>

<p>Les images rendues nécessitent une classe supplémentaire, car vous seul décidez quelles images le serveur peut produire. Celle-ci propose JPEG pour les instances rendues, les cadres rendus et les vignettes, et la bibliothèque les rend :</p>

<div class="codeblock" id="code">
 <h3>Proposer des images JPEG rendues et des vignettes - C#</h3>
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

<p>Le rendu intégré produit du JPEG à partir d’une instance à image unique ou des cadres sélectionnés par une requête. Une instance multi-cadre rendue dans son ensemble, une image plus large ou plus haute que 65500 pixels (les vignettes passent au-dessus de ces images), d’autres formats d’image, ainsi que les paramètres de fenêtre d’affichage, de niveau et de profil ICC nécessitent votre propre <code>RetrieveAsync</code> dans cette classe.</p>

<p>Les vignettes sont gérées avec soin pour la confidentialité du patient. Le rendu intégré crée une vignette uniquement à partir d’une instance dont les attributs Burned In Annotation et Recognizable Visual Features sont tous deux <code>NO</code>, ce qui signifie que l’image indique qu’elle ne montre aucune donnée patient. De nombreuses images ne possèdent pas ces attributs, et pour celles‑ci le serveur répond 406. Si votre archive a besoin de vignettes de telles images, générez‑les dans votre propre <code>RetrieveAsync</code> et assurez‑vous qu’elles masquent toutes les données patient.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Listes de travail, étapes de procédure et les autres services">}}

<p>Les autres services DICOMweb suivent le même modèle : enregistrer le service, implémenter son interface de stockage.</p>

<ul>
<li>Listes de travail <a href="/medical/net/ups-rs/">UPS-RS</a> avec <code>AddStandardWorklistService</code> : créer, rechercher et mettre à jour des éléments de travail et changer leur état, les règles d’état étant vérifiées par la bibliothèque et les notifications d’événement envoyées via un WebSocket.</li>
<li>La <a href="/medical/net/modality-worklist/">liste de travail des modalités</a> (MWL-RS) avec <code>AddStandardModalityWorklistService</code>, afin que les modalités puissent interroger les étapes de procédure planifiées de votre RIS.</li>
<li>Étapes de procédure réalisées par la modalité (MPPS-RS), engagement de stockage et instances non patient comme les palettes de couleur et les protocoles de suspension.</li>
<li>Récupération WADO-URI pour les visionneuses anciennes, fournie par le service Studies.</li>
</ul>

<p>Lorsque le comportement standard d’une transaction ne convient pas, enregistrez votre propre gestionnaire avec <code>AddHandler</code>. La bibliothèque continue de router et de vérifier la requête, et les autres transactions conservent leurs gestionnaires standards.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sécurité et hébergement restent entre vos mains">}}

<p>Les points de terminaison DICOMweb sont des points de terminaison ASP.NET Core ordinaires. L’authentification, l’autorisation, le CORS, TLS, les en‑têtes transmis et les limites de taille des requêtes sont configurés de la même façon que pour le reste de votre service :</p>

<div class="codeblock" id="code">
 <h3>Exiger un utilisateur authentifié - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>La partie protocolaire ne dépend pas d’ASP.NET Core. <code>DicomWebApplication</code> transforme une <code>DicomWebRequest</code> en réponse, de sorte que les mêmes services peuvent fonctionner derrière un autre serveur HTTP ou dans un test sans réseau :</p>

<div class="codeblock" id="code">
 <h3>Le même serveur sans ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Client et serveur à partir d’une même bibliothèque">}}

<p>Le <a href="/medical/net/dicomweb-client/">client DICOMweb</a> d’Aspose.Medical teste votre serveur depuis un test unitaire ou un programme console, et chaque fichier reçu par le serveur est un <code>DicomFile</code> standard pouvant être anonymisé, transcodé ou rendu dans le même processus.</p>

<p>Le <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">guide développeur DICOMweb</a> couvre chaque service et interface de stockage, et le <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">guide ASP.NET Core</a> traite du mappage des points de terminaison et des WebSockets.</p>

<p>Pages associées : <a href="/medical/net/dicom-networking/">Réseau DICOM</a> pour les systèmes qui envoient des études via DIMSE plutôt que HTTP, <a href="/medical/net/anonymization/">anonymisation</a> des études entrantes, <a href="/medical/net/dicom-transfer-syntax-conversion/">conversion de syntaxe de transfert</a>, et <a href="/medical/net/dicom-to-json/">DICOM vers JSON</a> pour le format utilisé par les résultats QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ressources d’apprentissage" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guide du développeur" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="Références API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Assistance produit" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Assistance gratuite" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Assistance payante" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Pourquoi Aspose.Medical pour .NET ?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Liste des clients" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Histoires de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
