---
title: Client DICOMweb en C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Recherchez, téléchargez et téléversez des études DICOM via HTTP depuis .NET. Un client DICOMweb en C# pur pour QIDO-RS, WADO-RS et STOW-RS qui fonctionne avec les PACS, VNA et les services d'imagerie cloud.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Client DICOMweb pour .NET C#" h2="Recherchez, téléchargez et téléversez des études sur n'importe quel serveur DICOMweb avec QIDO-RS, WADO-RS et STOW-RS, en C# géré sans rien à installer sur la machine." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM sur HTTP, depuis votre propre code">}}

<p>DICOMweb est la forme HTTP de DICOM. Au lieu d'une association DIMSE sur le port 104, votre application envoie des requêtes HTTPS ordinaires : QIDO‑RS pour la recherche, WADO‑RS pour le téléchargement, STOW‑RS pour le téléversement. Les services d’imagerie cloud tels que le service DICOM d’Azure Health Data Services et l’API Google Cloud Healthcare fonctionnent de cette façon, tout comme les archives telles qu’Orthanc et dcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> inclut un client DICOMweb depuis la version 26.10. <code>DicomWebClient</code> encapsule chaque transaction standard dans une méthode typée, et les résultats arrivent sous les mêmes objets <code>Dataset</code> et <code>DicomFile</code> que le reste de la bibliothèque lit et écrit. Le serveur de l'autre côté peut être n'importe quelle implémentation DICOMweb ; il n’est pas nécessaire qu’il soit construit avec Aspose.Medical.</p>

<p>Le client est en C# géré au-dessus de <code>HttpClient</code>. Il n’y a aucun kit d’outils natif ni service proxy, de sorte que le même code fonctionne sous Windows, Linux et dans un conteneur.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Se connecter à un serveur DICOMweb">}}

<p>Créez le client à partir d’un <code>HttpClient</code> et de l’URL racine du service du serveur. L’authentification, les paramètres TLS, les proxys et le pool de connexions restent sur le <code>HttpClient</code>, où votre application les configure déjà. Le client ne possède pas le <code>HttpClient</code> et ne le libère pas.</p>

<div class="codeblock" id="code">
 <h3>Créer un client DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Si un déploiement sert WADO-URI ou un autre service à partir d’une URL différente, <code>ServiceBaseUris</code> surcharge la racine uniquement pour ce service.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rechercher des études avec QIDO-RS">}}

<p>Une recherche prend des paramètres de correspondance avec le mot‑clé DICOM comme nom, les champs à renvoyer, et les contrôles de pagination. Les correspondances arrivent sous forme d’un flux de jeux de données, lus un à un pendant que la réponse se télécharge encore.</p>

<div class="codeblock" id="code">
 <h3>Trouver les études d’un patient - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> et <code>SearchInstancesAsync</code> recherchent un niveau plus bas, sur tout l’archive ou à l’intérieur d’une étude ou d’une série. Les mêmes options permettent également de définir la correspondance floue, la correspondance de valeur vide et le décalage du résultat pour la pagination.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Télécharger une étude avec WADO-RS">}}

<p>Une étude peut contenir des milliers d’instances. <code>RetrieveStudyAsync</code> les renvoie sous forme d’un flux, de sorte que chaque fichier est analysé dès que sa partie de la réponse arrive et peut être enregistré avant que le suivant ne soit lu. L’utilisation de la mémoire ne croît pas avec la taille de l’étude.</p>

<div class="codeblock" id="code">
 <h3>Enregistrer chaque instance d’une étude - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> et <code>RetrieveInstanceAsync</code> téléchargent une série ou une instance de la même manière.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Obtenir une miniature ou une image rendue">}}

<p>Un visualiseur ou un écran de liste de travail a souvent besoin d’une image, pas d’un fichier DICOM. Le serveur la rend et envoie un JPEG ou PNG, de sorte que le client ne télécharge pas ou ne décode pas les données de pixels d’origine.</p>

<div class="codeblock" id="code">
 <h3>Enregistrer une miniature JPEG d’une instance - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p><code>RetrieveRenderedInstanceAsync</code> et <code>RetrieveRenderedFramesAsync</code> renvoient à la place une image en taille réelle. <code>DicomWebRenderOptions</code> transmet les contrôles de rendu standard au serveur : qualité d’image, taille du viewport, fenêtre et profil ICC.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Téléverser des instances avec STOW-RS">}}

<p>STOW-RS envoie un nombre quelconque d’instances en une seule requête. Le serveur répond avec un rapport qui répertorie, instance par instance, ce qu’il a stocké et ce qu’il a refusé.</p>

<div class="codeblock" id="code">
 <h3>Téléverser un dossier de fichiers DICOM - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>Certains serveurs répondent avec une erreur HTTP, comme 409, lorsqu’une ou plusieurs instances échouent. L’appel lance alors <code>DicomWebClientException</code>, comme le montre la section suivante, et le même rapport se trouve dans le corps de la réponse de l’exception.</p>

<p><code>StoreStudyAsync</code> téléverse dans une étude nommée. Les deux méthodes acceptent également un <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, de sorte que les fichiers puissent être produits pendant l’envoi de la requête.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gérer les erreurs du serveur">}}

<p>Chaque opération typée vérifie le statut HTTP avant de renvoyer. Lorsque le serveur refuse une requête, l’appel lance <code>DicomWebClientException</code>, qui contient le code d’état, les en‐têtes et le début du corps de la réponse pour le journal.</p>

<div class="codeblock" id="code">
 <h3>Signaler une requête échouée - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>Les méthodes qui renvoient une réponse brute, comme la miniature ci‐dessus, laissent le contrôle du statut à vous via <code>IsSuccessStatusCode</code> ou <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ce que le client couvre d’autre">}}

<p>Recherche, téléchargement et téléversement sont les appels courants. Le client possède également un groupe d’opérations pour chacun des autres services DICOMweb :</p>

<ul>
<li>Données volumineuses, données de pixels et trames sélectionnées d’une instance, par numéro de trame basé à 1, telles que le serveur les a envoyées.</li>
<li>WADO-URI, la récupération à URL unique plus ancienne que de nombreux visualiseurs et archives utilisent encore, pour les fichiers DICOM et les images rendues.</li>
<li>Listes de travail UPS‑RS : créer, rechercher, mettre à jour et réclamer des éléments de travail, changer leur état, demander l’annulation, et s’abonner aux événements qui arrivent via un WebSocket.</li>
<li>La liste de travail de modalité (MWL‑RS), les étapes de procédure réalisées par la modalité (MPPS‑RS) et l’engagement de stockage.</li>
<li>Instances non‑patient comme les palettes de couleurs et les protocoles d’accrochage, ainsi que le document de capacités du serveur.</li>
<li><code>SendAsync</code> pour toute transaction standard avec vos propres valeurs de route, paramètres de requête, en‐têtes et corps, lorsqu’une méthode typée ne convient pas.</li>
</ul>

<p>Le <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">guide du client DICOMweb</a> documente chaque opération et option, et la <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">référence des opérations du client</a> répertorie chaque méthode.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Une bibliothèque de la réponse HTTP aux données de pixels">}}

<p>Un fichier téléchargé avec le client est un <code>DicomFile</code> standard. Il peut être anonymisé, transcodé, rendu ou sérialisé dans le même processus, sans étape de conversion et sans deuxième bibliothèque dans la chaîne.</p>

<p>Pages associées : <a href="/medical/net/dicomweb-server/">Serveur DICOMweb</a> pour l’autre côté du protocole dans votre propre service ASP.NET Core, <a href="/medical/net/dicom-networking/">Réseau DICOM</a> pour les archives qui utilisent DIMSE au lieu de HTTP, <a href="/medical/net/anonymization/">Anonymisation</a> avant qu’une étude ne quitte l’hôpital, <a href="/medical/net/dicom-transfer-syntax-conversion/">Conversion de syntaxe de transfert</a> pour ce que le serveur stocke, et <a href="/medical/net/dicom-to-json/">DICOM vers JSON</a> pour le format utilisé par les résultats de recherche QIDO‑RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ressources d’apprentissage" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guide du développeur" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="Références API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
