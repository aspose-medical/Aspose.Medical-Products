---
title: DICOMweb-Client in C# .NET – QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Suchen, herunterladen und hochladen von DICOM-Studien über HTTP aus .NET. Ein DICOMweb-Client in reinem C# für QIDO-RS, WADO-RS und STOW-RS, der mit PACS, VNA und Cloud-Imaging-Diensten funktioniert.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb-Client für .NET C#" h2="Suchen, herunterladen und hochladen von Studien auf jedem DICOMweb-Server mit QIDO-RS, WADO-RS und STOW-RS, in verwaltetem C# ohne Installation auf dem Rechner." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM über HTTP, aus eigenem Code">}}

<p>DICOMweb ist die HTTP‑Form von DICOM. Anstatt einer DIMSE‑Verbindung auf Port 104 sendet Ihre Anwendung gewöhnliche HTTPS‑Anfragen: QIDO‑RS zum Suchen, WADO‑RS zum Herunterladen, STOW‑RS zum Hochladen. Cloud‑Imaging‑Dienste wie der Azure Health Data Services DICOM‑Dienst und die Google Cloud Healthcare API funktionieren auf diese Weise, ebenso wie Archive wie Orthanc und dcm4chee.</p>

<p><strong>Aspose.Medical für .NET</strong> enthält seit Version 26.10 einen DICOMweb‑Client. <code>DicomWebClient</code> kapselt jede Standardtransaktion in einer typisierten Methode, und die Ergebnisse kommen als dieselben <code>Dataset</code>- und <code>DicomFile</code>-Objekte, die der Rest der Bibliothek liest und schreibt. Der Server auf der anderen Seite kann jede DICOMweb‑Implementierung sein; er muss nicht mit Aspose.Medical gebaut sein.</p>

<p>Der Client ist verwaltetes C# auf Basis von <code>HttpClient</code>. Es gibt kein natives Toolkit und keinen Proxy‑Dienst, sodass derselbe Code unter Windows, Linux und in einem Container läuft.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Verbindung zu einem DICOMweb-Server herstellen">}}

<p>Erstellen Sie den Client aus einem <code>HttpClient</code> und der Service‑Root‑URL des Servers. Authentifizierung, TLS‑Einstellungen, Proxys und Connection‑Pooling verbleiben im <code>HttpClient</code>, wo Ihre Anwendung sie bereits konfiguriert. Der Client besitzt den <code>HttpClient</code> nicht und gibt ihn nicht frei.</p>

<div class="codeblock" id="code">
 <h3>Erstellen eines DICOMweb-Clients – C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Wenn eine Bereitstellung WADO‑URI oder einen anderen Dienst von einer anderen URL aus bereitstellt, überschreibt <code>ServiceBaseUris</code> die Basis‑URL nur für diesen Dienst.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Studien mit QIDO‑RS suchen">}}

<p>Eine Suche verwendet Abgleichparameter, wobei das DICOM‑Schlüsselwort als Name dient, die zurückzugebenden Felder und Paging‑Steuerungen. Die Treffer kommen als Strom von Datasets, die einzeln gelesen werden, während die Antwort noch heruntergeladen wird.</p>

<div class="codeblock" id="code">
 <h3>Finden der Studien eines Patienten – C#</h3>
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

<p><code>SearchSeriesAsync</code> und <code>SearchInstancesAsync</code> suchen eine Ebene tiefer, im gesamten Archiv oder innerhalb einer Studie oder Serie. Die gleichen Optionen setzen auch unscharfe Treffer, leere Werte‑Abgleich und das Ergebnis‑Offset für das Paging.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Eine Studie mit WADO‑RS herunterladen">}}

<p>Eine Studie kann Tausende von Instanzen enthalten. <code>RetrieveStudyAsync</code> liefert sie als Strom, sodass jede Datei geparst wird, sobald ihr Teil der Antwort eintrifft, und gespeichert werden kann, bevor die nächste gelesen wird. Der Speicherverbrauch wächst nicht mit der Größe der Studie.</p>

<div class="codeblock" id="code">
 <h3>Jede Instanz einer Studie speichern – C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> und <code>RetrieveInstanceAsync</code> laden eine Serie bzw. eine Instanz auf dieselbe Weise herunter.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ein Thumbnail oder ein gerendertes Bild erhalten">}}

<p>Ein Betrachter oder ein Worklist‑Bildschirm benötigt oft ein Bild, nicht eine DICOM‑Datei. Der Server rendert es und sendet ein JPEG oder PNG, sodass der Client die ursprünglichen Pixeldaten nicht herunterladen oder dekodieren muss.</p>

<div class="codeblock" id="code">
 <h3>Ein JPEG‑Thumbnail einer Instanz speichern – C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> und <code>RetrieveRenderedFramesAsync</code> geben stattdessen ein Bild in Vollgröße zurück. <code>DicomWebRenderOptions</code> übergibt die standardmäßigen Render‑Steuerungen an den Server: Bildqualität, Ansichtgröße, Fenster‑ und ICC‑Profil.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Instanzen mit STOW‑RS hochladen">}}

<p>STOW‑RS sendet eine beliebige Anzahl von Instanzen in einer Anfrage. Der Server antwortet mit einem Bericht, der Instanz für Instanz auflistet, was gespeichert und was abgelehnt wurde.</p>

<div class="codeblock" id="code">
 <h3>Einen Ordner mit DICOM‑Dateien hochladen – C#</h3>
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

<p>Einige Server antworten mit einem HTTP‑Fehler, z. B. 409, wenn eine oder mehrere Instanzen fehlschlagen. Der Aufruf wirft dann <code>DicomWebClientException</code>, wie im nächsten Abschnitt gezeigt, und derselbe Bericht befindet sich im Antwort‑Body der Ausnahme.</p>

<p><code>StoreStudyAsync</code> lädt in eine benannte Studie hoch. Beide Methoden akzeptieren außerdem ein <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, sodass Dateien erzeugt werden können, während die Anfrage gesendet wird.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serverfehler behandeln">}}

<p>Jede typisierte Operation prüft den HTTP‑Status, bevor sie zurückkehrt. Wenn der Server eine Anfrage ablehnt, wirft der Aufruf <code>DicomWebClientException</code>, das den Statuscode, die Header und den Beginn des Antwort‑Bodys für das Protokoll enthält.</p>

<div class="codeblock" id="code">
 <h3>Einen fehlgeschlagenen Request melden – C#</h3>
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

<p>Methoden, die eine rohe Antwort zurückgeben, wie das obige Thumbnail, überlassen die Statusprüfung Ihnen über <code>IsSuccessStatusCode</code> oder <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Was der Client sonst noch abdeckt">}}

<p>Suchen, Herunterladen und Hochladen sind die üblichen Aufrufe. Der Client verfügt zudem über eine Operationsgruppe für jede der anderen DICOMweb‑Dienste:</p>

<ul>
<li>Bulk‑Daten, Pixeldaten und ausgewählte Frames einer Instanz, nach eins‑basiertem Frame‑Nummer, wie sie vom Server gesendet werden.</li>
<li>WADO‑URI, das ältere Single‑URL‑Abrufverfahren, das viele Betrachter und Archive noch nutzen, für DICOM‑Dateien und gerenderte Bilder.</li>
<li><a href="/medical/net/ups-rs/">UPS-RS</a>-Arbeitslisten: Erstellen, Suchen, Aktualisieren und Beanspruchen von Workitems, deren Zustand ändern, Abbruch anfordern und Events abonnieren, die über einen WebSocket eintreffen.</li>
<li>Die <a href="/medical/net/modality-worklist/">Modality Worklist</a> (MWL‑RS), durch Modalität durchgeführte Verfahrensschritte (MPPS‑RS) und Storage Commitment.</li>
<li>Nicht‑Patient‑Instanzen wie Farbpaletten und Hanging‑Protokolle sowie das Fähigkeits‑Dokument des Servers.</li>
<li><code>SendAsync</code> für jede Standardtransaktion mit eigenen Routendefinitionen, Abfrageparametern, Headern und Body, wenn eine typisierte Methode nicht passt.</li>
</ul>

<p>Der <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb-Client-Leitfaden</a> dokumentiert jede Operation und Option, und die <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">Client-Operations-Referenz</a> listet jede Methode auf.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Eine Bibliothek vom HTTP-Antwort bis zu den Pixeldaten">}}

<p>Eine mit dem Client heruntergeladene Datei ist ein reguläres <code>DicomFile</code>. Sie kann im selben Prozess anonymisiert, transkodiert, gerendert oder serialisiert werden, ohne einen Konvertierungsschritt und ohne eine zweite Bibliothek in der Kette.</p>

<p>Verwandte Seiten: <a href="/medical/net/dicomweb-server/">DICOMweb-Server</a> für die Gegenseite des Protokolls in Ihrem eigenen ASP.NET-Core-Dienst, <a href="/medical/net/dicom-networking/">DICOM-Netzwerk</a> für Archive, die DIMSE statt HTTP sprechen, <a href="/medical/net/anonymization/">Anonymisierung</a> bevor eine Studie das Krankenhaus verlässt, <a href="/medical/net/dicom-transfer-syntax-conversion/">Transfer-Syntax-Konvertierung</a> für das, was der Server speichert, und <a href="/medical/net/dicom-to-json/">DICOM zu JSON</a> für das Format, das QIDO‑RS-Suchergebnisse verwenden.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Entwicklerhandbuch" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="API‑Referenzen" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktsupport" tabId="support" >}}
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
