---
title: Sieciowanie DICOM w C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Połącz swoją aplikację .NET z PACS. Zweryfikuj za pomocą C-ECHO, wyślij obrazy przy użyciu C-STORE, zapytaj przy użyciu C-FIND i odbierz obrazy za pomocą własnego SCP. Klient i serwer DIMSE w czystym C#.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Sieciowanie DICOM w .NET C#" h2="Komunikuj się z PACS ze swojej aplikacji: C-ECHO, C-STORE, C-FIND, C-MOVE i C-GET, jako klient i jako serwer, w zarządzanym C# bez potrzeby instalacji czegokolwiek na maszynie." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Połącz się z PACS z własnego kodu">}}

<p>Odczytywanie plików DICOM to łatwa połowa obrazowania medycznego. Gdy Twoja aplikacja musi współpracować z rzeczywistym systemem szpitalnym, musi posługiwać się protokołem DIMSE: otworzyć asocjację z PACS, wysłać obrazy, zapytać o dostępne badania oraz odpowiedzieć, gdy inny system odeśle coś z powrotem.</p>

<p><strong>Aspose.Medical for .NET</strong> udostępnia ten protokół jako część biblioteki. <code>Aspose.Medical.Dicom.Network</code> zapewnia klienta DIMSE i serwer DIMSE, oba napisane w zarządzanym C#. Nie ma potrzeby instalacji natywnego zestawu narzędzi, żadnej usługi do konfiguracji i nie ma zależności od platformy, więc ten sam kod działa pod Windows, Linux oraz w kontenerze.</p>

<p>Trzy elementy obejmują większość integracji, a każdy z nich wymaga kilku linii kodu: sprawdź połączenie za pomocą C-ECHO, przesyłaj obrazy przy użyciu C-STORE i wyszukuj zawartość po drugiej stronie przy pomocy C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zacznij od C-ECHO">}}

<p>C-ECHO to ping DICOM. Potwierdza, że host, port i dwa tytuły AE są poprawne, zanim pojawią się inne problemy. Zbuduj klienta raz, podaj mu obsługę, która obserwuje odpowiedź, i wyślij żądanie.</p>

<div class="codeblock" id="code">
 <h3>Zweryfikuj połączenie za pomocą C-ECHO - C#</h3>
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

<p>Status <code>0x0000</code> oznacza sukces. Żądania są kolejkowane i następnie wysyłane w jednej asocjacji, więc pakiet pracy nie otwiera połączenia dla każdego elementu osobno.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Wyślij obrazy przy użyciu C-STORE">}}

<p>C-STORE to czynność, którą aplikacja wykonuje po utworzeniu lub odebraniu obrazu: przesyła instancję do archiwum. Zkolejkuj jedno żądanie na każdą instancję i wyślij je razem.</p>

<div class="codeblock" id="code">
 <h3>Wyślij plik DICOM do PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Konteksty prezentacji, które proponujesz, decydują, co archiwum zaakceptuje. Jeśli wymaga skompresowanej składni, dokonaj transkodowania przed wysłaniem, jak pokazuje strona <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a>, lub wymień alternatywy w <code>AdditionalTransferSyntaxes</code> i pozwól negocjacjom wybrać jedną.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Wyszukaj badania przy użyciu C-FIND">}}

<p>C-FIND odpowiada na pytanie „co znajduje się w archiwum”. Dopasowania przychodzą pojedynczo, każde z własnym zestawem danych identyfikatora, a ostateczna odpowiedź zamyka zapytanie.</p>

<div class="codeblock" id="code">
 <h3>Zapytaj o badania według pacjenta - C#</h3>
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

<p>Ta sama fabryka tworzy pozostałe poziomy zapytań: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> i <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> tworzy zapytanie listy prac modalności, które modalność wysyła przed skanowaniem; <a href="/medical/net/modality-worklist/">strona modality worklist</a> pokazuje je wraz z MPPS i po stronie serwera listy prac.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Odbieraj obrazy: własny SCP przechowujący">}}

<p>Biblioteka jest również serwerem. Zarejestruj obsługę dla usługi, którą chcesz udostępnić, rozpocznij nasłuchiwanie i Twoja aplikacja stanie się węzłem DICOM, do którego może wysyłać modalność lub inny PACS.</p>

<div class="codeblock" id="code">
 <h3>Akceptuj przychodzące obrazy - C#</h3>
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
 <h3>Obsługa, która przechowuje przychodzące dane - C#</h3>
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

<p>Co obsługa robi z zestawem danych, zależy od Ciebie: zapisać go na dysku, umieścić w kolejce, najpierw zanonimizować przy użyciu <a href="/medical/net/anonymization/">anonymization API</a>, lub transkodować do składni archiwum.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Co jeszcze obejmuje API sieciowe">}}

<p>Powyższe trzy usługi są najczęściej używane. Reszta DIMSE jest również dostępna:</p>

<ul>
<li>Pobieranie: C-MOVE i C-GET, z licznikami podoperacji zgłaszanymi w odpowiedziach.</li>
<li>Usługi N: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE i N-EVENT-REPORT, z których budowane są zobowiązanie przechowywania i MPPS.</li>
<li>Kontrola asocjacji: konteksty prezentacji, role klas usług, rozszerzona negocjacja, okno operacji asynchronicznych, negocjacja tożsamości użytkownika oraz hak polityki, który może odrzucić asocjację po wywołaniu tytułu AE.</li>
<li>TLS po obu stronach, przy użyciu <code>TlsInitiatorAuthenticator</code> i <code>TlsAcceptorAuthenticator</code>, z własną weryfikacją certyfikatów, jeśli jest potrzebna.</li>
<li>Limity czasu dla każdego etapu, od połączenia TCP po zwolnienie, oraz powiadomienia o cyklu życia asocjacji, aby długotrwale działający węzeł mógł rejestrować zdarzenia.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">Przewodnik po sieciowaniu DICOM</a> opisuje każdą opcję i każdą obsługę.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Czysty .NET, od gniazda po dane pikseli">}}

<p>Wszystko na tej stronie to kod zarządzany z tego samego pakietu, który odczytuje i zapisuje pliki. Asocjacja, kodeki i parser pochodzą z jednej biblioteki, więc badanie otrzymane przez sieć może być anonimizowane, transkodowane lub serializowane bez opuszczania procesu i bez natywnych zależności w całym łańcuchu.</p>

<p>Powiązane strony: <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> – co wysyłać, <a href="/medical/net/anonymization/">anonymization</a> – co najpierw usunąć, oraz <a href="/medical/net/dicom-tags/">DICOM tags</a> – jak odczytywać otrzymane dane.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zasoby edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Przewodnik dewelopera" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="Referencje API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Wsparcie produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Wsparcie darmowe" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Wsparcie płatne" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Dlaczego Aspose.Medical dla .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista klientów" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Historie sukcesu" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
