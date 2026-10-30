---
title: DICOMweb-Server in C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Erstellen Sie Ihren eigenen DICOMweb-Server oder PACS in C#. Aspose.Medical implementiert QIDO-RS, WADO-RS, STOW-RS und die Worklist‑Dienste auf ASP.NET Core; Sie verbinden den Speicher.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb-Server für .NET C#" h2="Fügen Sie einem ASP.NET Core‑Dienst QIDO-RS-, WADO-RS- und STOW-RS-Endpunkte hinzu. Die Bibliothek spricht das Protokoll; Ihr Code entscheidet, wo die Studien gespeichert werden." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Ihr eigener DICOMweb-Server in C#">}}

<p>Ein PACS, ein VNA, ein Cloud‑Archiv oder eine Forschungsplattform muss DICOMweb‑Anfragen beantworten: QIDO-RS‑Suchen, WADO-RS‑Downloads und STOW-RS‑Uploads sowie häufig auch die Worklist‑Dienste. Jeder von ihnen hat eigene Routen, Medientypen, Multipart‑Bodies, Matching‑Regeln und Statuscodes.</p>

<p><strong>Aspose.Medical für .NET</strong> implementiert diese Protokollseite seit Version 26.10. Es liest jede Anfrage, prüft sie gegenüber dem Standard, matcht Suchschlüssel nach den DICOM‑Regeln, paginiert die Ergebnisse, konvertiert Transfer‑Syntaxen und schreibt die Antwort. Ihr Code erledigt das, was nur Sie können: eine Instanz speichern, Kandidaten für eine Suche finden und die gespeicherten Dateien zurückgeben. Viewer, Modalitäten und andere Archive arbeiten dann mit Ihrem Server wie mit jedem anderen DICOMweb‑Server, ebenso der <a href="/medical/net/dicomweb-client/">DICOMweb‑Client</a> derselben Bibliothek.</p>

<p>Der Server ist in C# verwaltet. Er läuft überall dort, wo ASP.NET Core läuft: unter Windows, unter Linux und in einem Container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ein vollständiger Server in ASP.NET Core">}}

<p>Installieren Sie das Paket <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Es bringt Aspose.Medical mit und verbindet die DICOMweb‑Dienste mit Dependency Injection und Endpoint‑Routing. Dieses <code>Program.cs</code> ist ein funktionierender Studies‑Server; die beiden Klassen, die es registriert, werden unten gezeigt.</p>

<div class="codeblock" id="code">
 <h3>Program.cs eines DICOMweb‑Servers - C#</h3>
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

<p>Jede Studies‑Route beantwortet nun unter <code>/dicomweb</code>: <code>/dicomweb/studies</code> für Suche und Upload, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> für Abruf sowie die Routen für Metadaten, Frames, Bulk‑Daten und gerenderte Bilder. Das Capability‑Dokument an der Service‑Wurzel listet exakt die Transaktionen auf, die der Server unterstützt.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Verbinden Sie Ihren Speicher">}}

<p>Der Studies‑Dienst benötigt ein Interface mit drei Methoden: eine Instanz speichern, die gespeicherten Dateien zurückgeben und Suchkandidaten zurückgeben. Diese Implementierung hält jede Instanz als Datei in einem Ordner pro Studie und Serie.</p>

<div class="codeblock" id="code">
 <h3>Ein Ordnerarchiv hinter QIDO-RS, WADO-RS und STOW-RS - C#</h3>
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

<p>Die Bibliothek prüft die UIDs in jedem Anforderungspfad und in jeder hochgeladenen Instanz, bevor sie den Provider aufruft. Eine Instanz ohne gültige UIDs wird als fehlgeschlagen gemeldet und erreicht nie <code>StoreAsync</code>.</p>

<p>Ein Ordner reicht aus, um den Server auszuprobieren. Ein Produktionsarchiv behält dieselben drei Methoden bei und stellt einen Objektspeicher hinter <code>StoreAsync</code> und <code>RetrieveAsync</code> sowie einen Datenbank‑Index hinter <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Suche: Die Bibliothek wendet die DICOM-Matching‑Regeln an">}}

<p>QIDO-RS‑Matching ist mehr als ein String‑Vergleich: Wildcards, Datums‑ und Zeitbereiche, Listen von UIDs, unscharfe Personennamen, leere Werte und Schlüssel innerhalb von Sequenzen. Der Provider implementiert dies nicht. Er liefert Kandidaten in stabiler Reihenfolge und kann mehr als die Übereinstimmung zurückgeben. Die Bibliothek wendet das exakte DICOM‑Matching auf jeden Kandidaten an, behält die angeforderten Felder und verwendet <code>offset</code> und <code>limit</code>. Eine Antwort enthält standardmäßig höchstens 1000 Ergebnisse, und <code>DicomWebStandardServiceOptions</code> ändert dieses Limit.</p>

<p>Um Suchvorgänge in einem großen Archiv schnell zu halten, übergeben Sie die selektiven Schlüssel an die Datenbank. <code>search.Plan.Filter</code> enthält die kompilierten Suchschlüssel mit dem Attribut, der Art des Matchings und den jeweiligen Werten:</p>

<div class="codeblock" id="code">
 <h3>Exakte Patient‑ID aus einer Suche auslesen - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Eine auf diese Weise erstellte Datenbankabfrage kann zusätzliche Zeilen zurückliefern, zum Beispiel wenn sie einen Schlüssel ignoriert, den sie nicht übersetzen kann. Das abschließende Matching in der Bibliothek entfernt diese. Es darf echte Treffer nicht verwerfen, weil die Bibliothek sie nicht zurücksetzen kann.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Speichern: ein Ergebnis für jede Instanz">}}

<p>Eine STOW-RS‑Anfrage kann Hunderte von Instanzen transportieren. Die Bibliothek parsed den Multipart‑Body und ruft nacheinander <code>StoreAsync</code> für jede Instanz auf, sodass der gesamte Upload nie gleichzeitig im Speicher gehalten wird. Für jede Instanz liefert der Provider <code>Stored</code>, <code>Warning</code> für eine mit Änderungen gespeicherte Instanz oder <code>Failed</code> mit einem optionalen DICOM‑Fehlercode zurück.</p>

<p>Die Bibliothek erstellt den STOW-RS‑Report aus diesen Ergebnissen und wählt den Status: 200, wenn jede Instanz gespeichert ist, 202, wenn einige Warnungen haben oder fehlgeschlagen sind, 409, wenn alle fehlgeschlagen sind. Instanzen in einer Anfrage für ein benanntes Study, die zu einem anderen Study gehören, werden zurückgewiesen, bevor der Provider aufgerufen wird.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Abrufen: Transfer‑Syntaxen, Frames und gerenderte Bilder">}}

<p>Der Provider liefert gespeicherte Dateien zurück, und die Bibliothek übergibt sie in der vom Client gewünschten Form. Ein WADO-RS‑Client kann im <code>Accept</code>-Header nach einer Transfer‑Syntax fragen, und die Bibliothek konvertiert jede Datei vor dem Senden, einschließlich eigener Codecs, HTJ2K und JPEG XL. Metadaten in DICOM JSON oder XML, Bulk‑Daten und Einzel‑Frames stammen vom selben Provider, ohne zusätzlichen Code.</p>

<p>Gerenderte Bilder benötigen eine weitere Klasse, weil nur Sie entscheiden, welche Bilder der Server erzeugen darf. Diese Klasse bietet JPEG für gerenderte Instanzen, gerenderte Frames und Thumbnails an, und die Bibliothek rendert sie:</p>

<div class="codeblock" id="code">
 <h3>Gerenderte JPEG‑Bilder und Thumbnails anbieten – C#</h3>
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

<p>Der eingebaute Renderer erzeugt JPEG aus einer Einzel‑Frame‑Instanz oder aus den vom Request ausgewählten Frames. Ein gerendertes Multi‑Frame‑Instanz‑Gesamtbild, ein Bild breiter oder höher als 65 500 Pixel (Thumbnails überschreiten solche Bilder), andere Bildformate sowie die Parameter Viewport, Window und ICC‑Profil benötigen Ihre eigene <code>RetrieveAsync</code> in dieser Klasse.</p>

<p>Thumbnails werden unter Berücksichtigung des Patientenschutzes behandelt. Der eingebaute Renderer erzeugt ein Thumbnail nur aus einer Instanz, deren Attribute „Burned In Annotation“ und „Recognizable Visual Features“ beide <code>NO</code> sind, was bedeutet, dass das Bild selbst erklärt, keine Patientendaten anzuzeigen. Viele Bilder besitzen diese Attribute nicht, und dafür liefert der Server den Status 406. Wenn Ihr Archiv Thumbnails solcher Bilder benötigt, erzeugen Sie sie in Ihrer eigenen <code>RetrieveAsync</code> und stellen Sie sicher, dass sie jegliche Patientendaten verbergen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklists, Verfahrensschritte und die anderen Dienste">}}

<p>Die anderen DICOMweb‑Dienste folgen demselben Modell: Service registrieren, sein Storage‑Interface implementieren.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a>-Worklists mit <code>AddStandardWorklistService</code>: Workitems erstellen, suchen und aktualisieren sowie ihren Status ändern, wobei die Statusregeln von der Bibliothek geprüft und Ereignis‑Benachrichtigungen über einen WebSocket gesendet werden.</li>
<li>Die <a href="/medical/net/modality-worklist/">Modality‑Worklist</a> (MWL-RS) mit <code>AddStandardModalityWorklistService</code>, damit Modalitäten die geplanten Verfahrensschritte Ihres RIS abfragen können.</li>
<li>Modality Performed Procedure Steps (MPPS-RS), Storage Commitment und nicht‑patientenbezogene Instanzen wie Farbpaletten und Hanging‑Protokolle.</li>
<li>WADO-URI‑Abruf für ältere Viewer, bereitgestellt vom Studies‑Dienst.</li>
</ul>

<p>Wenn das Standardverhalten einer Transaktion nicht passt, registrieren Sie einen eigenen Handler dafür mit <code>AddHandler</code>. Die Bibliothek leitet die Anfrage weiterhin weiter und prüft sie, und die anderen Transaktionen behalten ihre Standard‑Handler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sicherheit und Hosting bleiben in Ihrer Hand">}}

<p>Die DICOMweb‑Endpunkte sind gewöhnliche ASP.NET Core‑Endpunkte. Authentifizierung, Autorisierung, CORS, TLS, weitergeleitete Header und Anfragegrößen‑Limits werden genauso konfiguriert wie für den Rest Ihres Dienstes:</p>

<div class="codeblock" id="code">
 <h3>Authentifizierten Benutzer verlangen – C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Der Protokollteil ist unabhängig von ASP.NET Core. <code>DicomWebApplication</code> wandelt einen <code>DicomWebRequest</code> in eine Antwort um, sodass dieselben Dienste hinter einem anderen HTTP‑Server oder in einem Test ohne Netzwerk laufen können:</p>

<div class="codeblock" id="code">
 <h3>Der gleiche Server ohne ASP.NET Core – C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Client und Server aus einer Bibliothek">}}

<p>Der <a href="/medical/net/dicomweb-client/">DICOMweb‑Client</a> von Aspose.Medical testet Ihren Server aus einem Unit‑Test oder einem Konsolenprogramm, und jede vom Server empfangene Datei ist ein reguläres <code>DicomFile</code>, das im selben Prozess anonymisiert, transkodiert oder gerendert werden kann.</p>

<p>Der <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb-Entwicklerleitfaden</a> behandelt jeden Service und das Storage‑Interface, und der <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core-Leitfaden</a> behandelt Endpoint‑Mapping und WebSockets.</p>

<p>Verwandte Seiten: <a href="/medical/net/dicom-networking/">DICOM‑Networking</a> für Systeme, die Studien über DIMSE statt HTTP senden, <a href="/medical/net/anonymization/">Anonymisierung</a> eingehender Studien, <a href="/medical/net/dicom-transfer-syntax-conversion/">Transfer‑Syntax‑Konvertierung</a> und <a href="/medical/net/dicom-to-json/">DICOM zu JSON</a> für das von QIDO-RS‑Ergebnissen genutzte Format.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Entwicklerhandbuch" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
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
