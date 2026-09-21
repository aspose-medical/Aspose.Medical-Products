---
title: Rede DICOM em C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Conecte sua aplicação .NET a um PACS. Verifique com C-ECHO, envie imagens com C-STORE, consulte com C-FIND e receba imagens com seu próprio SCP. Um cliente e servidor DIMSE em puro C#.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Rede DICOM em .NET C#" h2="Comunique-se com um PACS a partir da sua própria aplicação: C-ECHO, C-STORE, C-FIND, C-MOVE e C-GET, como cliente e como servidor, em C# gerenciado sem necessidade de instalar nada na máquina." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Conecte-se a um PACS a partir do seu próprio código">}}

<p>Ler arquivos DICOM é a metade fácil da imagem médica. No momento em que sua aplicação precisa trabalhar com um sistema hospitalar real, ela deve falar DIMSE: abrir uma associação com um PACS, enviar imagens, perguntar quais estudos existem e responder quando outro sistema envia algo de volta.</p>

<p><strong>Aspose.Medical for .NET</strong> inclui esse protocolo como parte da biblioteca. <code>Aspose.Medical.Dicom.Network</code> fornece um cliente DIMSE e um servidor DIMSE, ambos escritos em C# gerenciado. Não há toolkit nativo para instalar, nenhum serviço para configurar e nada específico de plataforma, portanto o mesmo código roda no Windows, no Linux e em um contêiner.</p>

<p>Três coisas cobrem a maioria das integrações, e cada uma requer poucas linhas: verificar a conexão com C-ECHO, enviar imagens com C-STORE e encontrar o que está do outro lado com C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Comece com C-ECHO">}}

<p>C-ECHO é o ping DICOM. Ele comprova que o host, a porta e os dois títulos AE estão corretos antes que qualquer outra coisa seja questionada. Crie um cliente uma vez, forneça-lhe um manipulador que observe a resposta e envie a solicitação.</p>

<div class="codeblock" id="code">
 <h3>Verificar a conexão com C-ECHO - C#</h3>
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

<p>Status <code>0x0000</code> significa sucesso. As solicitações são enfileiradas e então enviadas em uma única associação, de modo que um lote de trabalho não abre uma conexão por item.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Enviar imagens com C-STORE">}}

<p>C-STORE é o que uma aplicação faz depois de produzir ou receber uma imagem: envia a instância para o arquivo. Enfileire uma solicitação por instância e envie-as juntas.</p>

<div class="codeblock" id="code">
 <h3>Enviar um arquivo DICOM para um PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Os contextos de apresentação que você propõe decidem o que o arquivo aceitará. Se ele quiser uma sintaxe comprimida, transcoda antes de enviar, como mostra a página de <a href="/medical/net/dicom-transfer-syntax-conversion/">conversão de sintaxe de transferência</a>, ou liste as alternativas em <code>AdditionalTransferSyntaxes</code> e deixe a negociação escolher uma.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Encontrar estudos com C-FIND">}}

<p>C-FIND responde à pergunta "o que o arquivo tem". As correspondências chegam uma a uma, cada uma com seu próprio conjunto de dados de identificador, e uma resposta final fecha a consulta.</p>

<div class="codeblock" id="code">
 <h3>Consultar estudos por paciente - C#</h3>
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

<p>A mesma fábrica cria os outros níveis de consulta: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> e <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> constrói uma consulta de lista de trabalho de modalidade, que é a consulta que uma modalidade faz antes de um exame.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Receber imagens: seu próprio SCP de armazenamento">}}

<p>A biblioteca também funciona como servidor. Registre um manipulador para o serviço que deseja oferecer, inicie a escuta, e sua aplicação se torna um nó DICOM para o qual uma modalidade ou outro PACS pode enviar.</p>

<div class="codeblock" id="code">
 <h3>Aceitar imagens recebidas - C#</h3>
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
 <h3>O manipulador que armazena o que chega - C#</h3>
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

<p>O que o manipulador faz com o conjunto de dados é sua decisão: gravá‑lo em disco, colocá‑lo em uma fila, anonimiza‑lo primeiro com a <a href="/medical/net/anonymization/">API de anonimização</a>, ou transcodificá‑lo para a sintaxe do arquivo.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="O que mais a API de rede cobre">}}

<p>Os três serviços acima são os mais comuns. O resto do DIMSE também está disponível:</p>

<ul>
<li>Recuperação: C-MOVE e C-GET, com os contadores de sub‑operações relatados nas respostas.</li>
<li>Os serviços N: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE e N-EVENT-REPORT, que são a base para commitment de armazenamento e MPPS.</li>
<li>Controle de associação: contextos de apresentação, papéis da classe de serviço, negociação estendida, janela de operações assíncronas, negociação de identidade de usuário e um hook de política que pode rejeitar uma associação ao chamar o título AE.</li>
<li>TLS em ambos os lados, através de <code>TlsInitiatorAuthenticator</code> e <code>TlsAcceptorAuthenticator</code>, com sua própria validação de certificado, se necessário.</li>
<li>Timeouts para cada etapa, desde a conexão TCP até o release, e notificações para o ciclo de vida da associação, permitindo que um nó de longa duração registre o que aconteceu.</li>
</ul>

<p>O <a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">guia de rede DICOM</a> documenta cada opção e cada manipulador.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Puro .NET, do socket aos dados de pixel">}}

<p>Tudo nesta página é código gerenciado do mesmo pacote que lê e grava os arquivos. A associação, os codecs e o analisador provêm de uma única biblioteca, de modo que um estudo recebido pela rede pode ser anonimizado, transcodificado ou serializado sem sair do processo e sem dependência nativa em qualquer ponto da cadeia.</p>

<p>Páginas relacionadas: <a href="/medical/net/dicom-transfer-syntax-conversion/">conversão de sintaxe de transferência</a> para o que enviar, <a href="/medical/net/anonymization/">anonimização</a> para o que remover primeiro, e <a href="/medical/net/dicom-tags/">tags DICOM</a> para ler o que chegou.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizado" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentação" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guia do Desenvolvedor" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="Referências de API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Suporte ao Produto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Suporte Gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Suporte Pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Por que Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de Clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Casos de Sucesso" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
