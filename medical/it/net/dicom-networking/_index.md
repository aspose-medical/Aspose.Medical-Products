---
title: Rete DICOM in C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Collega la tua applicazione .NET a un PACS. Verifica con C-ECHO, invia immagini con C-STORE, effettua query con C-FIND e ricevi le immagini con il tuo SCP. Un client e server DIMSE in puro C#.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Rete DICOM in .NET C#" h2="Parla con un PACS dalla tua applicazione: C-ECHO, C-STORE, C-FIND, C-MOVE e C-GET, sia come client sia come server, in C# gestito senza necessità di installare nulla sulla macchina." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Connettiti a un PACS dal tuo codice">}}

<p>Leggere i file DICOM è la metà più semplice dell'imaging medico. Nel momento in cui la tua applicazione deve interagire con un vero sistema ospedaliero, deve parlare DIMSE: aprire un'associazione con un PACS, inviare immagini, chiedere quali studi sono presenti e rispondere quando un altro sistema invia qualcosa indietro.</p>

<p><strong>Aspose.Medical per .NET</strong> include quel protocollo come parte della libreria. <code>Aspose.Medical.Dicom.Network</code> fornisce un client DIMSE e un server DIMSE, entrambi scritti in C# gestito. Non è necessario installare alcun toolkit nativo, né configurare servizi, e non c'è nulla di specifico per la piattaforma, così lo stesso codice gira su Windows, Linux e in un container.</p>

<p>Tre cose coprono la maggior parte delle integrazioni, e ciascuna richiede solo poche righe: verificare la connessione con C-ECHO, inviare immagini con C-STORE e scoprire cosa c'è dall'altro lato con C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Inizia con C-ECHO">}}

<p>C-ECHO è il ping DICOM. Dimostra che host, porta e i due titoli AE sono corretti prima che qualsiasi altro errore venga segnalato. Crea un client una volta, assegnagli un gestore che osservi la risposta, e invia la richiesta.</p>

<div class="codeblock" id="code">
 <h3>Verifica la connessione con C-ECHO - C#</h3>
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

<p>Lo stato <code>0x0000</code> indica successo. Le richieste vengono messe in coda e poi inviate su un'unica associazione, così un batch di operazioni non apre una connessione per ogni elemento.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Invia immagini con C-STORE">}}

<p>C-STORE è ciò che un'applicazione fa dopo aver prodotto o ricevuto un'immagine: invia l'istanza all'archivio. Metti in coda una richiesta per ogni istanza e inviale insieme.</p>

<div class="codeblock" id="code">
 <h3>Invia un file DICOM a un PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>I contesti di presentazione che proponi determinano ciò che l'archivio accetterà. Se desidera una sintassi compressa, transcodifica prima di inviare, come mostra la pagina <a href="/medical/net/dicom-transfer-syntax-conversion/">conversione della sintassi di trasferimento</a>, oppure elenca le alternative in <code>AdditionalTransferSyntaxes</code> e lascia che la negoziazione ne scelga una.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Trova studi con C-FIND">}}

<p>C-FIND risponde alla domanda "cosa ha l'archivio". Le corrispondenze arrivano una alla volta, ognuna con il proprio dataset di identificatore, e una risposta finale chiude la query.</p>

<div class="codeblock" id="code">
 <h3>Interroga studi per paziente - C#</h3>
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

<p>La stessa factory crea gli altri livelli di query: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> e <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> genera una query della worklist della modalità, ovvero la query che una modalità richiede prima di una scansione.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ricevi immagini: il tuo SCP di archiviazione">}}

<p>La libreria è anche un server. Registra un gestore per il servizio che desideri offrire, avvia l'ascolto e la tua applicazione diventa un nodo DICOM a cui una modalità o un altro PACS può inviare dati.</p>

<div class="codeblock" id="code">
 <h3>Accetta immagini in ingresso - C#</h3>
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
 <h3>Il gestore che memorizza ciò che arriva - C#</h3>
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

<p>Ciò che il gestore fa con il dataset è una tua decisione: scriverlo su disco, inserirlo in una coda, anonimizzarlo prima con l'<a href="/medical/net/anonymization/">API di anonimizzazione</a>, o transcodificarlo nella sintassi dell'archivio.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Altre funzionalità coperte dall'API di networking">}}

<p>I tre servizi sopra sono i più comuni. Anche il resto di DIMSE è disponibile:</p>

<ul>
<li>Recupero: C-MOVE e C-GET, con i contatori delle sottooperazioni riportati nelle risposte.</li>
<li>I servizi N: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE e N-EVENT-REPORT, da cui nascono impegni di archiviazione e MPPS.</li>
<li>Controllo dell'associazione: contesti di presentazione, ruoli delle classi di servizio, negoziazione estesa, finestra di operazioni asincrone, negoziazione dell'identità utente e hook di policy che può rifiutare un'associazione chiamando l'AE title.</li>
<li>TLS su entrambi i lati, tramite <code>TlsInitiatorAuthenticator</code> e <code>TlsAcceptorAuthenticator</code>, con convalida del certificato personalizzata se necessaria.</li>
<li>Timeout per ogni fase, dalla connessione TCP al rilascio, e notifiche per il ciclo di vita dell'associazione così un nodo a lungo termine può registrare gli eventi.</li>
</ul>

<p>La <a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">guida al networking DICOM</a> documenta ogni opzione e ogni gestore.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pure .NET, dal socket ai dati pixel">}}

<p>Tutto ciò che trovi in questa pagina è codice gestito proveniente dallo stesso pacchetto che legge e scrive i file. L'associazione, i codec e il parser provengono da un'unica libreria, così uno studio ricevuto via rete può essere anonimizzato, transcodificato o serializzato senza uscire dal processo e senza dipendenze native in nessuna parte della catena.</p>

<p>Pagine correlate: <a href="/medical/net/dicom-transfer-syntax-conversion/">conversione della sintassi di trasferimento</a> per cosa inviare, <a href="/medical/net/anonymization/">anonimizzazione</a> per cosa rimuovere prima, e <a href="/medical/net/dicom-tags/">tag DICOM</a> per leggere ciò che è arrivato.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Risorse di apprendimento" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentazione" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guida per sviluppatori" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="Riferimenti API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Supporto prodotto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Supporto gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Supporto a pagamento" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Perché Aspose.Medical per .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Elenco clienti" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Storie di successo" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
