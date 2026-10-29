---
title: DICOM-nätverk i C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Anslut din .NET‑applikation till ett PACS. Verifiera med C-ECHO, skicka bilder med C-STORE, fråga med C-FIND och ta emot bilder med din egen SCP. En DIMSE‑klient och server i ren C#.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM‑nätverk i .NET C#" h2="Kommunicera med ett PACS från din egen applikation: C-ECHO, C-STORE, C-FIND, C-MOVE och C-GET, som klient och som server, i hanterad C# utan att behöva installera något på maskinen." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Anslut till ett PACS från din egen kod">}}

<p>Att läsa DICOM‑filer är den enkla halvan av medicinsk bildbehandling. Så fort din applikation måste arbeta med ett riktigt sjukhussystem, måste den tala DIMSE: öppna en association med ett PACS, skicka bilder, fråga vilka studier som finns, och svara när ett annat system skickar tillbaka något.</p>

<p><strong>Aspose.Medical for .NET</strong> levereras med det protokollet som en del av biblioteket. <code>Aspose.Medical.Dicom.Network</code> ger dig en DIMSE‑klient och en DIMSE‑server, båda skrivna i hanterad C#. Det finns inget inbyggt verktyg att installera, ingen tjänst att konfigurera och inget plattformsberoende, så samma kod körs på Windows, på Linux och i en container.</p>

<p>Tre saker täcker de flesta integrationer, och var och en är bara några rader kod: kontrollera förbindelsen med C-ECHO, skicka bilder med C-STORE och hitta vad som finns på andra sidan med C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Börja med C-ECHO">}}

<p>C-ECHO är DICOM‑pingen. Det visar att värden, porten och de två AE‑titlarna är korrekta innan något annat kan kritiseras. Skapa en klient en gång, ge den en hanterare som observerar svaret, och skicka förfrågan.</p>

<div class="codeblock" id="code">
 <h3>Verifiera anslutningen med C-ECHO - C#</h3>
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

<p>Status <code>0x0000</code> betyder framgång. Begäran köas och skickas sedan på en enda association, så en batch av arbete öppnar inte en anslutning per objekt.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Skicka bilder med C-STORE">}}

<p>C-STORE är vad en applikation gör efter att den har producerat eller mottagit en bild: den skickar instansen till arkivet. Köa en begäran per instans och skicka dem tillsammans.</p>

<div class="codeblock" id="code">
 <h3>Skicka en DICOM‑fil till ett PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>De presentationskontexter du föreslår bestämmer vad arkivet kommer att acceptera. Om det vill ha en komprimerad syntax, transkoda före sändning, som sidan <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> visar, eller lista alternativen i <code>AdditionalTransferSyntaxes</code> och låt förhandlingen välja en.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Hitta studier med C-FIND">}}

<p>C-FIND svarar på frågan "vad har arkivet". Träffar kommer en i taget, var och en med sitt eget identifierings‑dataset, och ett sista svar avslutar frågan.</p>

<div class="codeblock" id="code">
 <h3>Fråga studier per patient - C#</h3>
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

<p>Samma fabrik bygger de andra frågenivåerna: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> och <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> bygger en modalities arbetslistfråga, vilket är den fråga en modality ställer före en skanning; <a href="/medical/net/modality-worklist/">modality worklist page</a> visar den tillsammans med MPPS och arbetslistans serverdel.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ta emot bilder: din egen store‑SCP">}}

<p>Biblioteket är också en server. Registrera en hanterare för den tjänst du vill erbjuda, börja lyssna, och din applikation blir en DICOM‑nod som en modality eller ett annat PACS kan skicka till.</p>

<div class="codeblock" id="code">
 <h3>Acceptera inkommande bilder - C#</h3>
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
 <h3>Hanteraren som lagrar det som anländer - C#</h3>
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

<p>Vad hanteraren gör med datasetet är upp till dig: skriv det till disk, lägg det i en kö, anonymisera det först med <a href="/medical/net/anonymization/">anonymization API</a>, eller transkoda det till arkivets syntax.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vad mer nätverks‑API:t täcker">}}

<p>De tre tjänsterna ovan är de vanligaste. Resten av DIMSE finns också:</p>

<ul>
<li>Hämtning: C-MOVE och C-GET, med sub‑operationsräknare rapporterade i svaren.</li>
<li>N‑tjänsterna: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE och N-EVENT-REPORT, vilka är grunden för lagringsåtagande och MPPS.</li>
<li>Associationskontroll: presentationskontexter, serviceklassroller, utökad förhandling, fönstret för asynkrona operationer, förhandling av användaridentitet, och en policy‑hook som kan avvisa en association genom att anropa AE‑titel.</li>
<li>TLS på båda sidor, via <code>TlsInitiatorAuthenticator</code> och <code>TlsAcceptorAuthenticator</code>, med egen certifikatvalidering om du behöver det.</li>
<li>Timeouts för varje steg, från TCP‑anslutning till frigöring, samt notifikationer för associationens livscykel så att en långvarig nod kan logga vad som hände.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">DICOM‑nätverksguiden</a> dokumenterar varje alternativ och varje hanterare.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ren .NET, från socket till pixeldata">}}

<p>Allt på denna sida är hanterad kod från samma paket som läser och skriver filerna. Associationen, codec‑arna och parsern kommer från ett enda bibliotek, så en studie mottagen över nätverket kan anonymiseras, transkodas eller serialiseras utan att lämna processen och utan någon inbyggd beroende någonstans i kedjan.</p>

<p>Relaterade sidor: <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> för vad som ska skickas, <a href="/medical/net/anonymization/">anonymization</a> för vad som ska tas bort först, och <a href="/medical/net/dicom-tags/">DICOM tags</a> för att läsa vad som anlände.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Utvecklarguide" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="API‑referenser" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktstöd" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betald support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blogg" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Varför Aspose.Medical för .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundlista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Framgångshistorier" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
