---
title: DICOM hálózat C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Csatlakoztassa .NET alkalmazását egy PACS-hoz. Ellenőrizze C-ECHO-val, küldjön képeket C-STORE-val, kérdezzen C-FIND-del, és fogadjon képeket saját SCP-jével. Egy DIMSE kliens és szerver tiszta C#-ban.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM hálózat .NET C#" h2="Kommunikáljon egy PACS-szal saját alkalmazásából: C-ECHO, C-STORE, C-FIND, C-MOVE és C-GET, kliensként és szerverként, menedzselt C#-ban, anélkül, hogy bármit telepítenének a gépre." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Kapcsolódás egy PACS-hoz a saját kódból">}}

<p>A DICOM fájlok olvasása az orvosi képalkotás könnyű fele. Amint az alkalmazásnak valós kórházi rendszerrel kell működnie, DIMSE-t kell használnia: kapcsolatot nyit egy PACS-szal, képeket küld, megkérdezi, milyen vizsgálatok vannak, és válaszol, amikor egy másik rendszer valamit visszaküld.</p>

<p><strong>Aspose.Medical for .NET</strong> tartalmazza ezt a protokollt a könyvtár részeként. A <code>Aspose.Medical.Dicom.Network</code> egy DIMSE klienst és egy DIMSE szervert biztosít, mindkettő menedzselt C#-ban íródott. Nincs natív eszköztár a telepítéshez, nincs konfigurálandó szolgáltatás és semmi platformspecifikus, így ugyanaz a kód fut Windows-on, Linux-on és konténerben.</p>

<p>Három dolog lefedi a legtöbb integrációt, és mindegyik csak néhány sor: ellenőrizze a kapcsolatot C-ECHO-val, küldje a képeket C-STORE-val, és keresse meg, mi van a másik oldalon C-FIND-del.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kezdje C-ECHO-val">}}

<p>A C-ECHO a DICOM ping. Bizonyítja, hogy a host, a port és a két AE cím helyes, mielőtt bármi mást hibáztatnának. Építsen egy klienst egyszer, adjon neki egy kezelőt, amely figyeli a választ, és küldje el a kérést.</p>

<div class="codeblock" id="code">
 <h3>A kapcsolat ellenőrzése C-ECHO-val - C#</h3>
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

<p>A <code>0x0000</code> állapot sikeres. A kérések sorba állnak, majd egy kapcsolaton keresztül elküldésre kerülnek, így egy munkacsoport nem nyit kapcsolatot elemenként.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Képek küldése C-STORE-val">}}

<p>A C-STORE az, amit egy alkalmazás tesz egy kép előállítása vagy fogadása után: a példányt az archívumba továbbítja. Sorba állítson egy kérést példányonként, és küldje el őket együtt.</p>

<div class="codeblock" id="code">
 <h3>DICOM fájl küldése egy PACS-nek - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Az Ön által javasolt prezentációs kontextusok határozzák meg, mit fogad el az archívum. Ha tömörített szintaxist igényel, transzkódoljon küldés előtt, ahogyan a <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> oldal mutatja, vagy sorolja fel a lehetőségeket a <code>AdditionalTransferSyntaxes</code>‑ben, és hagyja, hogy a tárgyalás válasszon.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vizsgálatok keresése C-FIND-del">}}

<p>A C-FIND megválaszolja a „mit tartalmaz az archívum” kérdést. A találatok egyenként érkeznek, mindegyik saját azonosító adatkészlettel, és egy végső válasz zárja le a lekérdezést.</p>

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

<p>Ugyanaz a gyár építi a többi lekérdezési szintet: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> és <code>CreateImageQuery</code>. A <code>CreateWorklistQuery</code> egy modality worklist lekérdezést hoz létre, amelyet a modalitás kér egy szkennelés előtt.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Képek fogadása: saját store SCP">}}

<p>A könyvtár szerver is egyben. Regisztráljon egy kezelőt a kínálni kívánt szolgáltatáshoz, indítsa el a hallgatást, és az alkalmazása DICOM csomópóttá válik, amelyhez egy modalitás vagy egy másik PACS küldhet.</p>

<div class="codeblock" id="code">
 <h3>Bejövő képek fogadása - C#</h3>
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
 <h3>A kezelő, amely tárolja a beérkezett adatokat - C#</h3>
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

<p>Hogy a kezelő mit tesz az adatkészlettel, az az Ön döntése: leírja lemezre, egy sorba helyezi, először anonimizálja a <a href="/medical/net/anonymization/">anonymization API</a>-val, vagy transzkódolja az archívum szintaxisába.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Milyen további területeket fed le a hálózati API">}}

<p>A fenti három szolgáltatás a leggyakoribbak. A DIMSE többi része is elérhető:</p>

<ul>
<li>Letöltés: C-MOVE és C-GET, a válaszokban jelentett al-műveleti számlálókkal.</li>
<li>Az N-szolgáltatások: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE és N-EVENT-REPORT, amelyekből a storage commitment és MPPS épül.</li>
<li>Kapcsolatvezérlés: prezentációs kontextusok, szolgáltatásosztály szerepek, kiterjesztett tárgyalás, aszinkron műveleti ablak, felhasználó-azonosító tárgyalás, és egy szabály hook, amely AE cím hívásával elutasíthatja a kapcsolatot.</li>
<li>TLS mindkét oldalon, a <code>TlsInitiatorAuthenticator</code> és <code>TlsAcceptorAuthenticator</code> segítségével, saját tanúsítvány-ellenőrzéssel, ha szükséges.</li>
<li>Időkorlátok minden szakaszra, a TCP csatlakozástól a feloldásig, valamint értesítések a kapcsolat életciklusáról, hogy egy hosszú futású csomópont naplózhassa az eseményeket.</li>
</ul>

<p>A <a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">DICOM networking guide</a> minden opciót és kezelőt dokumentál.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tiszta .NET, a sockettől a pixeladatokig">}}

<p>Az oldalon található minden menedzselt kód ugyanabból a csomagból származik, amely olvas és ír fájlokat. A kapcsolat, a kodekek és a parser egy könyvtárból jönnek, így egy hálózaton keresztül fogadott vizsgálat anonimizálható, transzkódolható vagy sorosítható anélkül, hogy elhagyná a folyamatot és natív függőség nélkül a láncban.</p>

<p>Kapcsolódó oldalak: <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> a küldendő dolgokhoz, <a href="/medical/net/anonymization/">anonymization</a> a először eltávolítandókhoz, és <a href="/medical/net/dicom-tags/">DICOM tags</a> a beérkezett olvasásához.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Fejlesztői útmutató" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetett támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
