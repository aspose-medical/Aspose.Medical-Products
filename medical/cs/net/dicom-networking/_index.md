---
title: DICOM síťová komunikace v C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Připojte svou .NET aplikaci k PACS. Ověřte pomocí C-ECHO, odesílejte obrázky pomocí C-STORE, dotazujte pomocí C-FIND a přijímejte obrázky pomocí vlastního SCP. DIMSE klient a server v čistém C#.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM síťová komunikace v .NET C#" h2="Komunikujte s PACS ze své aplikace: C-ECHO, C-STORE, C-FIND, C-MOVE a C-GET, jako klient i server, v řízeném C# bez nutnosti instalovat něco na stroj." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Připojte se k PACS ze svého kódu">}}

<p>Čtení souborů DICOM je snadná polovina medicínského zobrazování. Jakmile vaše aplikace musí spolupracovat se skutečným nemocničním systémem, musí mluvit protokolem DIMSE: otevřít asociaci s PACS, odeslat obrázky, zeptat se, jaké studie jsou k dispozici, a odpovědět, když jiný systém něco pošle zpět.</p>

<p><strong>Aspose.Medical for .NET</strong> doručuje tento protokol jako součást knihovny. <code>Aspose.Medical.Dicom.Network</code> poskytuje DIMSE klienta a DIMSE server, oba napsané v řízeném C#. Není nutné instalovat žádný nativní toolkit, žádnou službu konfigurovat a není zde žádná platformově specifická závislost, takže stejný kód běží na Windows, Linuxu i v kontejneru.</p>

<p>Tři věci pokrývají většinu integrací a každá z nich je jen několik řádků: ověřte spojení pomocí C-ECHO, nahrávejte obrázky pomocí C-STORE a zjistěte, co je na druhé straně pomocí C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Začněte s C-ECHO">}}

<p>C-ECHO je DICOM ping. Ověřuje, že hostitel, port a oba AE tituly jsou správné, dříve než se obviňuje cokoli jiného. Vytvořte klienta jednou, přiřaďte mu obslužnou rutinu, která pozoruje odpověď, a odešlete požadavek.</p>

<div class="codeblock" id="code">
 <h3>Ověřte spojení pomocí C-ECHO - C#</h3>
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

<p>Stav <code>0x0000</code> znamená úspěch. Požadavky jsou zařazeny do fronty a poté odeslány v rámci jedné asociace, takže dávka úloh neotevírá spojení pro každou položku.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Odesílejte obrázky pomocí C-STORE">}}

<p>C-STORE je to, co aplikace dělá po vytvoření nebo přijetí obrázku: odesílá instanci do archivu. Zařaďte jeden požadavek na instanci do fronty a odešlete je společně.</p>

<div class="codeblock" id="code">
 <h3>Odešlete DICOM soubor do PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Prezentační kontexty, které navrhnete, určují, co archiv přijme. Pokud požaduje komprimovaný syntakt, transkódujte před odesláním, jak ukazuje stránka <a href="/medical/net/dicom-transfer-syntax-conversion/">převod syntaxi přenosu</a>, nebo uveďte alternativy v <code>AdditionalTransferSyntaxes</code> a nechte vyjednání vybrat jednu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vyhledejte studie pomocí C-FIND">}}

<p>C-FIND odpovídá na otázku „co archiv obsahuje“. Nalezené položky přicházejí po jedné, každá s vlastním identifikačním datasetem, a závěrečná odpověď dotaz uzavře.</p>

<div class="codeblock" id="code">
 <h3>Dotaz na studie podle pacienta - C#</h3>
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

<p>Stejná továrna vytváří další úrovně dotazů: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> a <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> vytváří dotaz na pracovním seznamu modality, což je dotaz, který modality provádí před snímáním.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Přijímejte obrázky: vlastní SCP úložiště">}}

<p>Knihovna je také server. Zaregistrujte obslužnou rutinu pro službu, kterou chcete nabízet, spusťte naslouchání a vaše aplikace se stane DICOM uzlem, na který může modality nebo jiný PACS odesílat.</p>

<div class="codeblock" id="code">
 <h3>Přijímejte příchozí obrázky - C#</h3>
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
 <h3>Obslužná rutina, která ukládá příchozí data - C#</h3>
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

<p>Co obslužná rutina udělá s datasetem, je na vás: zapíše jej na disk, vloží do fronty, nejprve jej anonymizuje pomocí <a href="/medical/net/anonymization/">API anonymizace</a>, nebo jej transkóduje do syntaxi archivu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Co dalšího pokrývá síťové API">}}

<p>Výše uvedené tři služby jsou běžné. Zbytek protokolu DIMSE je také k dispozici:</p>

<ul>
<li>Retrieval: C-MOVE a C-GET, s počítadly podoperací uváděnými v odpovědích.</li>
<li>N-služby: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE a N-EVENT-REPORT, ze kterých jsou postaveny storage commitment a MPPS.</li>
<li>Řízení asociace: prezentační kontexty, role servisních tříd, rozšířené vyjednávání, okno asynchronních operací, vyjednávání identity uživatele a hák politiky, který může odmítnout asociaci voláním AE titulu.</li>
<li>TLS na obou stranách, pomocí <code>TlsInitiatorAuthenticator</code> a <code>TlsAcceptorAuthenticator</code>, s vlastní validací certifikátu, pokud ji potřebujete.</li>
<li>Časové limity pro každou fázi, od TCP připojení po uvolnění, a notifikace o životním cyklu asociace, aby dlouho běžící uzel mohl zaznamenat, co se stalo.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">Průvodce DICOM sítí</a> dokumentuje každou volbu a každou obslužnou rutinu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Čistý .NET, od socketu po pixelová data">}}

<p>Vše na této stránce je řízený kód ze stejného balíčku, který čte a zapisuje soubory. Asociace, kodeky a parser pocházejí z jedné knihovny, takže studie přijatá po síti může být anonymizována, transkódována nebo serializována bez opuštění procesu a bez nativní závislosti kdekoli v řetězci.</p>

<p>Související stránky: <a href="/medical/net/dicom-transfer-syntax-conversion/">převod syntaxi přenosu</a> pro to, co odeslat, <a href="/medical/net/anonymization/">anonymizace</a> pro to, co nejprve odstranit, a <a href="/medical/net/dicom-tags/">DICOM tagy</a> pro čtení přijatých dat.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Výukové zdroje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Průvodce vývojáře" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="Reference API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Podpora produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Bezplatná podpora" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Placená podpora" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Proč Aspose.Medical pro .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Seznam zákazníků" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Úspěšné příběhy" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
