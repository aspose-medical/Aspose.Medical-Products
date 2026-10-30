---
title: Servidor DICOMweb em C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Crie seu próprio servidor DICOMweb ou PACS em C#. Aspose.Medical implementa QIDO-RS, WADO-RS, STOW-RS e os serviços de lista de trabalho em ASP.NET Core; você conecta o armazenamento.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Servidor DICOMweb para .NET C#" h2="Adicione endpoints QIDO-RS, WADO-RS e STOW-RS a um serviço ASP.NET Core. A biblioteca fala o protocolo; seu código decide onde os estudos são armazenados." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Seu próprio servidor DICOMweb em C#">}}

<p>Um PACS, um VNA, um arquivo em nuvem ou uma plataforma de pesquisa precisa responder a solicitações DICOMweb: buscas QIDO-RS, downloads WADO-RS e uploads STOW-RS, e frequentemente também os serviços de lista de trabalho. Cada um deles tem suas próprias rotas, tipos de mídia, corpos multipart, regras de correspondência e códigos de status.</p>

<p><strong>Aspose.Medical for .NET</strong> implementa esse lado do protocolo desde a versão 26.10. Ele lê cada solicitação, verifica-a contra o padrão, corresponde às chaves de busca pelas regras DICOM, pagina os resultados, converte sintaxes de transferência e grava a resposta. Seu código faz o que só você pode fazer: armazenar uma instância, encontrar candidatos para uma busca e retornar os arquivos armazenados. Visualizadores, modalidades e outros arquivos então trabalham com seu servidor como com qualquer outro servidor DICOMweb, e o <a href="/medical/net/dicomweb-client/">cliente DICOMweb</a> da mesma biblioteca também funciona.</p>

<p>O servidor é gerenciado em C#. Ele roda onde quer que o ASP.NET Core rode: no Windows, no Linux e em um contêiner.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Um servidor completo em ASP.NET Core">}}

<p>Instale o pacote <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Ele traz o Aspose.Medical e conecta os serviços DICOMweb à injeção de dependência e ao roteamento de endpoints. Este <code>Program.cs</code> é um servidor de Estudos funcional; as duas classes que ele registra são mostradas abaixo.</p>

<div class="codeblock" id="code">
 <h3>Program.cs de um servidor DICOMweb - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>Agora, cada rota de Estudos responde sob <code>/dicomweb</code>: <code>/dicomweb/studies</code> para busca e upload, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> para recuperação, e as rotas para metadados, quadros, dados em massa e imagens renderizadas. O documento de capacidades na raiz do serviço lista exatamente as transações que o servidor suporta.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Conecte seu armazenamento">}}

<p>O serviço de Estudos precisa de uma interface com três métodos: armazenar uma instância, devolver os arquivos armazenados e devolver os candidatos de busca. Esta implementação mantém cada instância como um arquivo em uma pasta por estudo e série.</p>

<div class="codeblock" id="code">
 <h3>Um arquivo de pasta por trás de QIDO-RS, WADO-RS e STOW-RS - C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>A biblioteca verifica os UIDs em cada caminho de solicitação e em cada instância enviada antes de chamar o provedor. Uma instância sem UIDs válidos é relatada como falha e nunca chega a <code>StoreAsync</code>.</p>

<p>Uma pasta é suficiente para testar o servidor. Um arquivo de produção mantém os mesmos três métodos e coloca um armazenamento de objetos atrás de <code>StoreAsync</code> e <code>RetrieveAsync</code> e um índice de banco de dados atrás de <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Busca: a biblioteca aplica as regras de correspondência DICOM">}}

<p>A correspondência QIDO-RS é mais que comparação de strings: curingas, intervalos de data e hora, listas de UIDs, nomes de pessoa difusos, valores vazios e chaves dentro de sequências. O provedor não implementa nada disso. Ele devolve candidatos em ordem estável, e pode devolver mais que o correspondido. A biblioteca aplica a correspondência DICOM exata a cada candidato, mantém os campos solicitados e aplica <code>offset</code> e <code>limit</code>. Uma resposta contém no máximo 1000 resultados por padrão, e <code>DicomWebStandardServiceOptions</code> altera esse limite.</p>

<p>Para manter as buscas rápidas em um arquivo grande, passe as chaves seletivas ao banco de dados. <code>search.Plan.Filter</code> contém as chaves de busca compiladas, com o atributo, o tipo de correspondência e os valores de cada uma:</p>

<div class="codeblock" id="code">
 <h3>Ler um Patient ID exato de uma busca - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Uma consulta ao banco de dados construída dessa forma pode devolver linhas extras, por exemplo quando ignora uma chave que não pode traduzir. A correspondência final na biblioteca as remove. Ela não deve descartar correspondências reais, pois a biblioteca não pode adicioná‑las novamente.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Armazenamento: um resultado para cada instância">}}

<p>Uma solicitação STOW-RS pode conter centenas de instâncias. A biblioteca analisa o corpo multipart e chama <code>StoreAsync</code> para cada instância sequencialmente, de modo que o upload completo nunca fica totalmente na memória de uma vez. Para cada instância, o provedor devolve <code>Stored</code>, <code>Warning</code> para uma instância armazenada com alterações, ou <code>Failed</code> com um código de razão DICOM opcional.</p>

<p>A biblioteca constrói o relatório STOW-RS a partir desses resultados e seleciona o status: 200 quando todas as instâncias são armazenadas, 202 quando algumas têm avisos ou falharam, 409 quando todas falharam. Instâncias em uma solicitação para um estudo nomeado que pertencem a outro estudo são recusadas antes de chamar o provedor.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Recuperação: sintaxes de transferência, quadros e imagens renderizadas">}}

<p>O provedor devolve os arquivos armazenados, e a biblioteca os entrega na forma solicitada pelo cliente. Um cliente WADO-RS pode solicitar uma sintaxe de transferência no cabeçalho <code>Accept</code>, e a biblioteca converte cada arquivo antes de enviá‑lo, com seus próprios codecs, incluindo HTJ2K e JPEG XL. Metadados em DICOM JSON ou XML, dados em massa e quadros individuais vêm do mesmo provedor, sem código extra.</p>

<p>Imagens renderizadas precisam de mais uma classe, porque só você decide quais imagens o servidor pode produzir. Esta oferece JPEG para instâncias renderizadas, quadros renderizados e miniaturas, e a biblioteca os renderiza:</p>

<div class="codeblock" id="code">
 <h3>Oferecer imagens JPEG renderizadas e miniaturas - C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>O renderizador embutido produz JPEG a partir de uma instância de quadro único ou dos quadros selecionados por uma solicitação. Uma instância multi‑quadro renderizada como um todo, uma imagem mais larga ou mais alta que 65500 pixels (miniaturas contornam tais imagens), outros formatos de imagem, e os parâmetros de viewport, janela e perfil ICC precisam do seu próprio <code>RetrieveAsync</code> nesta classe.</p>

<p>Miniaturas são tratadas com cuidado para a privacidade do paciente. O renderizador embutido cria uma miniatura apenas a partir de uma instância cujos atributos Burned In Annotation e Recognizable Visual Features são ambos <code>NO</code>, o que significa que a própria imagem declara que não exibe dados do paciente. Muitas imagens não possuem esses atributos, e para elas o servidor responde 406. Se seu arquivo precisar de miniaturas dessas imagens, produza‑as no seu próprio <code>RetrieveAsync</code> e assegure que ocultem quaisquer dados do paciente.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Listas de trabalho, etapas de procedimento e os outros serviços">}}

<p>Os outros serviços DICOMweb seguem o mesmo modelo: registrar o serviço, implementar sua interface de armazenamento.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> listas de trabalho com <code>AddStandardWorklistService</code>: criar, buscar e atualizar itens de trabalho e mudar seu estado, com as regras de estado verificadas pela biblioteca e notificações de eventos enviadas via WebSocket.</li>
<li>O <a href="/medical/net/modality-worklist/">modality worklist</a> (MWL-RS) com <code>AddStandardModalityWorklistService</code>, para que as modalidades possam consultar as etapas de procedimento agendadas do seu RIS.</li>
<li>Etapas de procedimento realizadas pela modalidade (MPPS-RS), compromisso de armazenamento e instâncias não‑paciente como paletas de cores e protocolos de hanging.</li>
<li>Recuperação WADO-URI para visualizadores antigos, atendida pelo serviço de Estudos.</li>
</ul>

<p>Quando o comportamento padrão de uma transação não se adequa, registre seu próprio manipulador para ela com <code>AddHandler</code>. A biblioteca ainda roteia e verifica a solicitação, e as demais transações mantêm seus manipuladores padrão.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Segurança e hospedagem permanecem sob seu controle">}}

<p>Os endpoints DICOMweb são endpoints padrão do ASP.NET Core. Autenticação, autorização, CORS, TLS, cabeçalhos encaminhados e limites de tamanho de requisição são configurados da mesma forma que o restante do seu serviço:</p>

<div class="codeblock" id="code">
 <h3>Exigir um usuário autenticado - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>A parte do protocolo não depende do ASP.NET Core. <code>DicomWebApplication</code> transforma uma <code>DicomWebRequest</code> em uma resposta, de modo que os mesmos serviços podem rodar atrás de outro servidor HTTP ou em um teste sem rede:</p>

<div class="codeblock" id="code">
 <h3>O mesmo servidor sem ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Cliente e servidor da mesma biblioteca">}}

<p>O <a href="/medical/net/dicomweb-client/">cliente DICOMweb</a> da Aspose.Medical testa seu servidor a partir de um teste unitário ou de um programa de console, e cada arquivo que o servidor recebe é um <code>DicomFile</code> regular que pode ser anonimizado, transcodificado ou renderizado no mesmo processo.</p>

<p>O <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">guia do desenvolvedor DICOMweb</a> cobre todos os serviços e interfaces de armazenamento, e o <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">guia ASP.NET Core</a> cobre o mapeamento de endpoints e WebSockets.</p>

<p>Páginas relacionadas: <a href="/medical/net/dicom-networking/">DICOM networking</a> para sistemas que enviam estudos via DIMSE ao invés de HTTP, <a href="/medical/net/anonymization/">anonymization</a> de estudos recebidos, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a>, e <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> para o formato usado pelos resultados QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizado" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentação" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guia do desenvolvedor" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="Referências da API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Suporte ao produto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Suporte gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Suporte pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Por que Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Casos de sucesso" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
