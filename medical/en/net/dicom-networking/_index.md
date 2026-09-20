---
title: DICOM Networking in C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Connect your .NET application to a PACS. Verify with C-ECHO, send images with C-STORE, query with C-FIND, and receive images with your own SCP. A DIMSE client and server in pure C#.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM Networking in .NET C#" h2="Talk to a PACS from your own application: C-ECHO, C-STORE, C-FIND, C-MOVE and C-GET, as a client and as a server, in managed C# with nothing to install on the machine." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Connect to a PACS from your own code">}}

<p>Reading DICOM files is the easy half of medical imaging. The moment your application has to work with a real hospital system, it has to speak DIMSE: open an association with a PACS, send images, ask what studies are there, and answer when another system sends something back.</p>

<p><strong>Aspose.Medical for .NET</strong> ships that protocol as part of the library. <code>Aspose.Medical.Dicom.Network</code> gives you a DIMSE client and a DIMSE server, both written in managed C#. There is no native toolkit to install, no service to configure and nothing platform specific, so the same code runs on Windows, on Linux and in a container.</p>

<p>Three things cover most integrations, and each one is a few lines: check the link with C-ECHO, push images with C-STORE, and find what is on the other side with C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Start with C-ECHO">}}

<p>C-ECHO is the DICOM ping. It proves that the host, the port and the two AE titles are right before anything else is blamed. Build a client once, give it a handler that observes the answer, and send the request.</p>

<div class="codeblock" id="code">
 <h3>Verify the connection with C-ECHO - C#</h3>
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

<p>Status <code>0x0000</code> means success. Requests are queued and then sent on one association, so a batch of work does not open a connection per item.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Send images with C-STORE">}}

<p>C-STORE is what an application does after it has produced or received an image: it pushes the instance to the archive. Queue one request per instance and send them together.</p>

<div class="codeblock" id="code">
 <h3>Send a DICOM file to a PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>The presentation contexts you propose decide what the archive will accept. If it wants a compressed syntax, transcode before sending, as the <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> page shows, or list the alternatives in <code>AdditionalTransferSyntaxes</code> and let the negotiation pick one.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Find studies with C-FIND">}}

<p>C-FIND answers the question "what does the archive have". Matches arrive one at a time, each with its own identifier dataset, and a final response closes the query.</p>

<div class="codeblock" id="code">
 <h3>Query studies by patient - C#</h3>
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

<p>The same factory builds the other query levels: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> and <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> builds a modality worklist query, which is the query a modality asks before a scan.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Receive images: your own store SCP">}}

<p>The library is a server as well. Register a handler for the service you want to offer, start listening, and your application becomes a DICOM node that a modality or another PACS can send to.</p>

<div class="codeblock" id="code">
 <h3>Accept incoming images - C#</h3>
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
 <h3>The handler that stores what arrives - C#</h3>
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

<p>What the handler does with the dataset is your decision: write it to disk, put it in a queue, anonymize it first with the <a href="/medical/net/anonymization/">anonymization API</a>, or transcode it into the archive's syntax.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="What else the networking API covers">}}

<p>The three services above are the common ones. The rest of DIMSE is there as well:</p>

<ul>
<li>Retrieval: C-MOVE and C-GET, with the sub-operation counters reported on the responses.</li>
<li>The N-services: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE and N-EVENT-REPORT, which is what storage commitment and MPPS are built from.</li>
<li>Association control: presentation contexts, service class roles, extended negotiation, the asynchronous operations window, user identity negotiation, and a policy hook that can reject an association by calling AE title.</li>
<li>TLS on both sides, through <code>TlsInitiatorAuthenticator</code> and <code>TlsAcceptorAuthenticator</code>, with your own certificate validation if you need it.</li>
<li>Timeouts for every stage, from the TCP connect to the release, and notifications for the association lifecycle so a long-running node can log what happened.</li>
</ul>

<p>The <a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">DICOM networking guide</a> documents every option and every handler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pure .NET, from the socket to the pixel data">}}

<p>Everything on this page is managed code from the same package that reads and writes the files. The association, the codecs and the parser come from one library, so a study received over the network can be anonymized, transcoded or serialized without leaving the process and without a native dependency anywhere in the chain.</p>

<p>Related pages: <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> for what to send, <a href="/medical/net/anonymization/">anonymization</a> for what to remove first, and <a href="/medical/net/dicom-tags/">DICOM tags</a> for reading what arrived.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Learning Resources" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Developer Guide" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="API References" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Product Support" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Free Support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Paid Support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Why Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Customers List" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Success Stories" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
