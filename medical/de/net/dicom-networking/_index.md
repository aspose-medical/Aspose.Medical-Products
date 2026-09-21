---
title: DICOM-Netzwerk in C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Verbinden Sie Ihre .NET-Anwendung mit einem PACS. Verifizieren Sie mit C-ECHO, senden Sie Bilder mit C-STORE, fragen Sie mit C-FIND und empfangen Sie Bilder mit Ihrem eigenen SCP. Ein DIMSE-Client und -Server in reinem C#.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM-Netzwerk in .NET C#" h2="Kommunizieren Sie mit einem PACS aus Ihrer eigenen Anwendung: C-ECHO, C-STORE, C-FIND, C-MOVE und C-GET, sowohl als Client als auch als Server, in Managed C# ohne Installation auf dem Rechner." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Verbinden Sie sich von Ihrem eigenen Code aus mit einem PACS">}}

<p>DICOM-Dateien zu lesen ist die einfache Hälfte der medizinischen Bildgebung. Sobald Ihre Anwendung mit einem realen Krankenhaus‑System arbeiten muss, muss sie DIMSE sprechen: eine Association mit einem PACS öffnen, Bilder senden, nach vorhandenen Studien fragen und antworten, wenn ein anderes System etwas zurücksendet.</p>

<p><strong>Aspose.Medical für .NET</strong> liefert dieses Protokoll als Teil der Bibliothek. <code>Aspose.Medical.Dicom.Network</code> stellt Ihnen einen DIMSE‑Client und einen DIMSE‑Server bereit, beide in Managed C# geschrieben. Es gibt kein zu installierendes natives Toolkit, keinen zu konfigurierenden Dienst und nichts Plattformspezifisches, sodass derselbe Code unter Windows, Linux und in einem Container läuft.</p>

<p>Drei Dinge decken die meisten Integrationen ab, und jedes besteht aus wenigen Zeilen: die Verbindung mit C-ECHO prüfen, Bilder mit C-STORE senden und herausfinden, was auf der anderen Seite mit C-FIND ist.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Beginnen Sie mit C-ECHO">}}

<p>C-ECHO ist das DICOM‑Ping. Es beweist, dass Host, Port und die beiden AE‑Titel korrekt sind, bevor sonst etwas fehlschlägt. Erstellen Sie einmal einen Client, versehen Sie ihn mit einem Handler, der die Antwort beobachtet, und senden Sie die Anfrage.</p>

<div class="codeblock" id="code">
 <h3>Verifizieren Sie die Verbindung mit C-ECHO - C#</h3>
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

<p>Der Status <code>0x0000</code> bedeutet Erfolg. Anfragen werden in eine Warteschlange gestellt und dann über eine Association gesendet, sodass ein Batch von Vorgängen nicht für jedes Element eine Verbindung öffnet.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bilder mit C-STORE senden">}}

<p>C-STORE ist das, was eine Anwendung nach dem Erzeugen oder Empfangen eines Bildes tut: sie schiebt die Instanz ins Archiv. Legen Sie für jede Instanz eine Anfrage in die Warteschlange und senden Sie sie gemeinsam.</p>

<div class="codeblock" id="code">
 <h3>Senden Sie eine DICOM‑Datei an ein PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Die von Ihnen vorgeschlagenen Presentation Contexts bestimmen, was das Archiv akzeptiert. Wenn es eine komprimierte Syntax verlangt, transkodieren Sie vor dem Senden, wie die Seite <a href=\"/medical/net/dicom-transfer-syntax-conversion/\">transfer syntax conversion</a> zeigt, oder listen Sie die Alternativen in <code>AdditionalTransferSyntaxes</code> auf und lassen die Verhandlung eine auswählen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Studien mit C-FIND finden">}}

<p>C-FIND beantwortet die Frage \"was hat das Archiv\". Treffer kommen einzeln an, jeweils mit einem eigenen Identifier‑Datensatz, und eine Abschlussantwort beendet die Abfrage.</p>

<div class="codeblock" id="code">
 <h3>Studien nach Patient abfragen - C#</h3>
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

<p>Die gleiche Factory erzeugt die anderen Abfrageebenen: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> und <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> erstellt eine Modalitäts‑Worklist‑Abfrage, die eine Modalität vor einem Scan stellt.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bilder empfangen: Ihr eigener Store‑SCP">}}

<p>Die Bibliothek ist ebenfalls ein Server. Registrieren Sie einen Handler für den Service, den Sie anbieten wollen, starten Sie das Hören, und Ihre Anwendung wird zu einem DICOM‑Knoten, zu dem eine Modalität oder ein anderes PACS senden kann.</p>

<div class="codeblock" id="code">
 <h3>Eingehende Bilder akzeptieren - C#</h3>
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
 <h3>Der Handler, der Ankommendes speichert - C#</h3>
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

<p>Was der Handler mit dem Datensatz macht, liegt in Ihrer Entscheidung: auf die Festplatte schreiben, in eine Warteschlange einreihen, zuerst mit der <a href=\"/medical/net/anonymization/\">anonymization API</a> anonymisieren oder in die Syntax des Archivs transkodieren.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Was die Networking‑API noch abdeckt">}}

<p>Die oben genannten drei Dienste sind die üblichen. Der Rest von DIMSE ist ebenfalls vorhanden:</p>

<ul>
<li>Retrieval: C-MOVE und C-GET, wobei die Sub‑Operation‑Zähler in den Antworten gemeldet werden.</li>
<li>Die N-Services: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE und N-EVENT-REPORT, aus denen Storage Commitment und MPPS aufgebaut sind.</li>
<li>Association‑Steuerung: Presentation Contexts, Service‑Class‑Rollen, erweiterte Verhandlung, das Fenster für asynchrone Operationen, User‑Identity‑Verhandlung und ein Policy‑Hook, das eine Association anhand des AE‑Titles ablehnen kann.</li>
<li>TLS auf beiden Seiten, über <code>TlsInitiatorAuthenticator</code> und <code>TlsAcceptorAuthenticator</code>, mit eigener Zertifikatsvalidierung, falls gewünscht.</li>
<li>Timeouts für jede Phase, vom TCP‑Connect bis zum Release, sowie Benachrichtigungen zum Lifecycle der Association, sodass ein langfristig laufender Knoten protokollieren kann, was passiert ist.</li>
</ul>

<p>Der <a href=\"https://docs.aspose.com/medical/net/developer-guide/dicom-networking/\">DICOM‑Networking‑Leitfaden</a> dokumentiert jede Option und jeden Handler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Reines .NET, vom Socket bis zu den Pixeldaten">}}

<p>Alles auf dieser Seite ist verwalteter Code aus demselben Paket, das die Dateien liest und schreibt. Association, Codecs und Parser stammen aus einer Bibliothek, sodass eine über das Netzwerk empfangene Studie anonymisiert, transkodiert oder serialisiert werden kann, ohne den Prozess zu verlassen und ohne native Abhängigkeiten irgendwo in der Kette.</p>

<p>Verwandte Seiten: <a href=\"/medical/net/dicom-transfer-syntax-conversion/\">transfer syntax conversion</a> für das zu sendende, <a href=\"/medical/net/anonymization/\">anonymization</a> für das zuerst zu entfernende, und <a href=\"/medical/net/dicom-tags/\">DICOM tags</a> zum Lesen des Angekommenden.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Entwicklerhandbuch" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="API‑Referenzen" href="https://reference.aspose.com/medical/net/" >}}
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
