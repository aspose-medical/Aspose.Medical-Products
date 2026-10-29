---
title: DICOM-netwerken in C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Verbind uw .NET-toepassing met een PACS. Verifieer met C-ECHO, verzend afbeeldingen met C-STORE, query met C-FIND, en ontvang afbeeldingen met uw eigen SCP. Een DIMSE-client en -server in puur C#.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM-netwerken in .NET C#" h2="Praat met een PACS vanuit uw eigen applicatie: C-ECHO, C-STORE, C-FIND, C-MOVE en C-GET, als client en als server, in managed C# zonder iets te installeren op de machine." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Verbind met een PACS vanuit uw eigen code">}}

<p>Het lezen van DICOM-bestanden is de makkelijke helft van medische beeldvorming. Op het moment dat uw applicatie met een echt ziekenhuissysteem moet werken, moet het DIMSE spreken: een associatie openen met een PACS, afbeeldingen verzenden, vragen welke studies er zijn, en antwoorden wanneer een ander systeem iets terugstuurt.</p>

<p><strong>Aspose.Medical for .NET</strong> levert dat protocol als onderdeel van de bibliotheek. <code>Aspose.Medical.Dicom.Network</code> geeft u een DIMSE-client en een DIMSE-server, beide geschreven in managed C#. Er is geen native toolkit om te installeren, geen service om te configureren en niets platformspecifiek, zodat dezelfde code draait op Windows, op Linux en in een container.</p>

<p>Drie zaken dekken de meeste integraties, en elk ervan bestaat uit enkele regels code: controleer de verbinding met C-ECHO, verzend afbeeldingen met C-STORE, en vind wat zich aan de andere kant bevindt met C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Begin met C-ECHO">}}

<p>C-ECHO is de DICOM-ping. Het bewijst dat de host, de poort en de twee AE-titels correct zijn voordat er iets anders wordt bekritiseerd. Bouw één keer een client, geef deze een handler die het antwoord observeert, en verstuur het verzoek.</p>

<div class="codeblock" id="code">
 <h3>Verifieer de verbinding met C-ECHO - C#</h3>
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

<p>Status <code>0x0000</code> betekent succes. Verzoeken worden in de wachtrij geplaatst en vervolgens verzonden binnen één associatie, zodat een batch werk niet per item een verbinding opent.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Verzend afbeeldingen met C-STORE">}}

<p>C-STORE is wat een applicatie doet nadat het een afbeelding heeft geproduceerd of ontvangen: het duwt de instantie naar het archief. Plaats één verzoek per instantie in de wachtrij en verzend ze samen.</p>

<div class="codeblock" id="code">
 <h3>Verzend een DICOM-bestand naar een PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>De presentatietypen die u voorstelt bepalen wat het archief accepteert. Als het een gecomprimeerde syntax wil, transcodeer dan vóór het verzenden, zoals de <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> pagina laat zien, of vermeld de alternatieven in <code>AdditionalTransferSyntaxes</code> en laat de onderhandeling er één kiezen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zoek studies met C-FIND">}}

<p>C-FIND beantwoordt de vraag "wat heeft het archief". Overeenkomsten komen één voor één binnen, elk met zijn eigen identifier-dataset, en een eindrespons sluit de query.</p>

<div class="codeblock" id="code">
 <h3>Zoek studies op patiënt - C#</h3>
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

<p>Dezelfde factory bouwt de andere query-niveaus: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> en <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> bouwt een modality worklist query, dat is de query die een modaliteit stelt vóór een scan; de <a href="/medical/net/modality-worklist/">modality worklist page</a> toont deze samen met MPPS en de worklist-serverside.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ontvang afbeeldingen: uw eigen store SCP">}}

<p>De bibliotheek is ook een server. Registreer een handler voor de service die u wilt aanbieden, begin met luisteren, en uw applicatie wordt een DICOM-node waar een modaliteit of een andere PACS naartoe kan sturen.</p>

<div class="codeblock" id="code">
 <h3>Accepteer inkomende afbeeldingen - C#</h3>
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
 <h3>De handler die opslaat wat aankomt - C#</h3>
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

<p>Wat de handler met de dataset doet, is uw keuze: schrijf het naar schijf, plaats het in een wachtrij, anonimiseer het eerst met de <a href="/medical/net/anonymization/">anonymization API</a>, of transcodeer het naar de syntax van het archief.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Wat de networking‑API nog meer dekt">}}

<p>De drie bovenstaande services zijn de gangbare. De rest van DIMSE is er ook:</p>

<ul>
<li>Ophalen: C-MOVE en C-GET, met de sub‑operatie‑tellers gerapporteerd in de antwoorden.</li>
<li>De N-services: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE en N-EVENT-REPORT, waarop storage commitment en MPPS gebaseerd zijn.</li>
<li>Associatie‑controle: presentatietypen, service‑class‑rollen, uitgebreide onderhandeling, het asynchrone‑operaties‑venster, gebruikersidentiteitsonderhandeling, en een beleids‑hook die een associatie kan afwijzen door de AE‑title te bellen.</li>
<li>TLS aan beide kanten, via <code>TlsInitiatorAuthenticator</code> en <code>TlsAcceptorAuthenticator</code>, met uw eigen certificaatauthenticatie indien nodig.</li>
<li>Timeouts voor elke fase, van de TCP‑connectie tot de release, en meldingen voor de levenscyclus van de associatie zodat een langlopende node kan loggen wat er gebeurde.</li>
</ul>

<p>De <a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">DICOM networking guide</a> documenteert elke optie en elke handler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pure .NET, van de socket tot de pixeldata">}}

<p>Alles op deze pagina is managed code uit hetzelfde pakket dat de bestanden leest en schrijft. De associatie, de codecs en de parser komen uit één bibliotheek, zodat een studie die via het netwerk wordt ontvangen geanonimiseerd, getranscodeerd of geserialiseerd kan worden zonder het proces te verlaten en zonder native afhankelijkheden ergens in de keten.</p>

<p>Gerelateerde pagina's: <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> voor wat te verzenden, <a href="/medical/net/anonymization/">anonymization</a> voor wat eerst te verwijderen, en <a href="/medical/net/dicom-tags/">DICOM tags</a> voor het lezen van wat is aangekomen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leerbronnen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Developergids" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="API-referenties" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Productondersteuning" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis ondersteuning" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betaalde ondersteuning" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Waarom Aspose.Medical voor .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Klantlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
