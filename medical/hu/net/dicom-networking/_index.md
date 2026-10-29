---
title: DICOM hálózatkezelés C# .NET-ben - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Kapcsolja .NET alkalmazását egy PACS-hoz. Ellenőrizze C-ECHO-val, küldjön képeket C-STORE-ral, kérdezzen C-FIND-del, és fogadjon képeket a saját SCP-jével. Egy DIMSE kliens és szerver tisztán C#-ban.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM hálózatkezelés .NET C#-ban" h2="Kommunikáljon egy PACS-szal saját alkalmazásából: C-ECHO, C-STORE, C-FIND, C-MOVE és C-GET, mint kliens és szerver, menedzselt C#-ban anélkül, hogy bármit telepíteni kellene a gépre." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Kapcsolódjon egy PACS-hoz saját kódjából">}}

<p>A DICOM fájlok olvasása az orvosi képalkotás könnyű fele. Amikor az alkalmazásnak valós kórházi rendszerrel kell dolgoznia, DIMSE-nek kell beszélnie: kapcsolatot nyitni egy PACS-szal, képeket küldeni, megkérdezni, milyen vizsgálatok vannak, és válaszolni, amikor egy másik rendszer valamit visszaküld.</p>

<p><strong>Aspose.Medical for .NET</strong> a protokollt a könyvtár részeként szállítja. A <code>Aspose.Medical.Dicom.Network</code> egy DIMSE klienst és egy DIMSE szervert biztosít, mindkettő menedzselt C#-ban íródott. Nincs telepítendő natív eszközkészlet, nincs konfigurálandó szolgáltatás és nincs platform-specifikus követelmény, így ugyanaz a kód Windowson, Linuxon és konténerben egyaránt fut.</p>

<p>Három dolog lefedi a legtöbb integrációt, és mindegyik csak néhány sor: ellenőrizze a kapcsolatot C-ECHO-val, küldjön képeket C-STORE-ral, és derítse ki, mi van a másik oldalon C-FIND-del.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kezdje a C-ECHO-val">}}

<p>A C-ECHO a DICOM ping. Bizonyítja, hogy a host, a port és a két AE cím helyes, mielőtt bármi más hibáztatásra kerülne. Hozzon létre egyszer egy klienst, adjon neki egy kezelőt, amely figyeli a választ, és küldje el a kérést.</p>

<div class="codeblock" id="code">
 <h3>Kapcsolat ellenőrzése C-ECHO-val - C#</h3>
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

<p>A <code>0x0000</code> állapot sikerességet jelent. A kérések sorba vannak állítva, majd egy kapcsolatban küldésre kerülnek, így egy munkacsomag nem nyit egy kapcsolatot minden egyes elemhez.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Képek küldése C-STORE-rel">}}

<p>A C-STORE azt a műveletet jelenti, amelyet egy alkalmazás egy kép előállítása vagy fogadása után végez: az instance-et a tárhelybe töltözi. Sorba állítson egy kérést minden instance-hez, és küldje el őket együtt.</p>

<div class="codeblock" id="code">
 <h3>DICOM fájl küldése egy PACS-ba - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Az Ön által javasolt prezentációs kontextusok határozzák meg, mit fogad el a tárhely. Ha tömörített szintaxist szeretne, kódolja újra a küldés előtt, ahogyan a <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> oldal mutatja, vagy sorolja fel a lehetőségeket a <code>AdditionalTransferSyntaxes</code>-ben, és hagyja, hogy a tárgyalás válasszon egyet.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vizsgálatok keresése C-FIND-del">}}

<p>A C-FIND megválaszolja a "mit tárol a tárhely" kérdést. Egyezések egyesével érkeznek, mindegyik saját azonosító adatkészlettel, és egy végső válasz lezárja a lekérdezést.</p>

<div class="codeblock" id="code">
 <h3>Vizsgálatok lekérdezése beteg szerint - C#</h3>
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

<p>Ugyanez a gyár építi a többi lekérdezési szintet: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> és <code>CreateImageQuery</code>. A <code>CreateWorklistQuery</code> egy modality worklist lekérdezést hoz létre, amelyet a modality egy szkennelés előtt kér; a <a href="/medical/net/modality-worklist/">modality worklist page</a> bemutatja azt MPPS‑sel együtt és a worklist szerver oldalát.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Képek fogadása: saját store SCP">}}

<p>A könyvtár szintén egy szerver. Regisztráljon egy kezelőt a kívánt szolgáltatáshoz, indítsa el a hallgatást, és az alkalmazása DICOM csomóponttá válik, amelyhez egy modality vagy egy másik PACS küldhet.</p>

<div class="codeblock" id="code">
 <h3>Bejövő képek elfogadása - C#</h3>
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
 <h3>A kezelés, amely tárolja a beérkező adatokat - C#</h3>
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

<p>Azt, hogy a handler mit tesz az adatkészlettel, az Ön döntése: leírja lemezre, sorba helyezi, először anonimná teszi a <a href="/medical/net/anonymization/">anonymization API</a>-val, vagy átírja a tárhely szintaxisára.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Mi másra is kiterjed a hálózati API">}}

<p>A fenti három szolgáltatás a leggyakoribbak. A DIMSE többi része is rendelkezésre áll:</p>

<ul>
<li>Letöltés: C-MOVE és C-GET, a válaszokban jelentett al-művelet számlálókkal.</li>
<li>Az N-szolgáltatások: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE és N-EVENT-REPORT, amelyekből a storage commitment és MPPS épül.</li>
<li>Kapcsolatvezérlés: prezentációs kontextusok, szolgáltatásosztály szerepek, kiterjesztett tárgyalás, aszinkron műveletekablak, felhasználói azonosító tárgyalás, és egy szabály-hook, amely AE cím hívásával elutasíthat egy kapcsolatot.</li>
<li>TLS mindkét oldalon, a <code>TlsInitiatorAuthenticator</code> és <code>TlsAcceptorAuthenticator</code> segítségével, saját tanúsítvány-ellenőrzéssel, ha szükséges.</li>
<li>Időkorlátok minden szakaszra, a TCP csatlakozástól a leválasztásig, valamint értesítések a kapcsolat életciklusáról, hogy egy hosszú ideig futó node naplózhassa, mi történt.</li>
</ul>

<p>A <a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">DICOM networking guide</a> minden opciót és kezelőt dokumentál.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tiszta .NET, a sockettől a pixel adatokig">}}

<p>Az ezen az oldalon található minden menedzselt kód ugyanabból a csomagból származik, amely a fájlok olvasását és írását végzi. A kapcsolat, a kodekek és a parser egy könyvtárból származik, így egy hálózaton keresztül érkező vizsgálat anonimná tehető, újrakódolható vagy sorosítható anélkül, hogy kilépne a folyamatból és anélkül, hogy bármelyik láncszakaszon natív függőség lenne.</p>

<p>Kapcsolódó oldalak: <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> a küldendő adatokhoz, <a href="/medical/net/anonymization/">anonymization</a> az elsőként eltávolítandóakhoz, és <a href="/medical/net/dicom-tags/">DICOM tags</a> a beérkezett olvasásához.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási erőforrások" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Fejlesztői útmutató" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetős támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért Aspose.Medical .NET-re?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
