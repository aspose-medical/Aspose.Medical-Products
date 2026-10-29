---
title: Cliente DICOMweb em C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Pesquise, baixe e envie estudos DICOM via HTTP a partir do .NET. Um cliente DICOMweb em C# puro para QIDO-RS, WADO-RS e STOW-RS que funciona com PACS, VNA e serviços de imagem em nuvem.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Cliente DICOMweb para .NET C#" h2="Pesquise, baixe e envie estudos em qualquer servidor DICOMweb com QIDO-RS, WADO-RS e STOW-RS, em C# gerenciado sem necessidade de instalar nada na máquina." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM sobre HTTP, a partir do seu próprio código">}}

<p>DICOMweb é a forma HTTP do DICOM. Em vez de uma associação DIMSE na porta 104, sua aplicação envia requisições HTTPS comuns: QIDO-RS para pesquisa, WADO-RS para download, STOW-RS para envio. Serviços de imagem em nuvem como o serviço DICOM do Azure Health Data Services e a Google Cloud Healthcare API funcionam desta forma, assim como arquivos como Orthanc e dcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> inclui um cliente DICOMweb desde a versão 26.10. <code>DicomWebClient</code> encapsula cada transação padrão em um método tipado, e os resultados chegam como os mesmos objetos <code>Dataset</code> e <code>DicomFile</code> que o restante da biblioteca lê e grava. O servidor do outro lado pode ser qualquer implementação DICOMweb; não precisa ser construído com Aspose.Medical.</p>

<p>O cliente é C# gerenciado sobre <code>HttpClient</code>. Não há toolkit nativo nem serviço de proxy, de modo que o mesmo código roda no Windows, no Linux e em um contêiner.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Conectar a um servidor DICOMweb">}}

<p>Crie o cliente a partir de um <code>HttpClient</code> e da URL raiz do serviço do servidor. Autenticação, configurações TLS, proxies e pool de conexões permanecem no <code>HttpClient</code>, onde sua aplicação já os configura. O cliente não possui o <code>HttpClient</code> e não o descarta.</p>

<div class="codeblock" id="code">
 <h3>Criar um cliente DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Se uma implantação fornece WADO-URI ou outro serviço a partir de uma URL diferente, <code>ServiceBaseUris</code> substitui a raiz apenas para esse serviço.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pesquisar estudos com QIDO-RS">}}

<p>Uma pesquisa recebe parâmetros de correspondência com a palavra‑chave DICOM como nome, os campos a serem retornados e controles de paginação. As correspondências chegam como um fluxo de datasets, lidos um a um enquanto a resposta ainda está sendo baixada.</p>

<div class="codeblock" id="code">
 <h3>Encontrar os estudos de um paciente - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> e <code>SearchInstancesAsync</code> buscam um nível abaixo, em todo o arquivo ou dentro de um estudo ou série. As mesmas opções também definem correspondência difusa, correspondência de valores vazios e o deslocamento de resultados para paginação.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Baixar um estudo com WADO-RS">}}

<p>Um estudo pode conter milhares de instâncias. <code>RetrieveStudyAsync</code> as devolve como um fluxo, de modo que cada arquivo é analisado à medida que sua parte da resposta chega e pode ser salvo antes que o próximo seja lido. O uso de memória não cresce com o tamanho do estudo.</p>

<div class="codeblock" id="code">
 <h3>Salvar cada instância de um estudo - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> e <code>RetrieveInstanceAsync</code> baixam uma série ou uma instância da mesma forma.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Obter uma miniatura ou uma imagem renderizada">}}

<p>Um visualizador ou uma tela de lista de trabalhos costuma precisar de uma imagem, não de um arquivo DICOM. O servidor a renderiza e envia um JPEG ou PNG, de modo que o cliente não faz download nem decodifica os dados de pixel originais.</p>

<div class="codeblock" id="code">
 <h3>Salvar uma miniatura JPEG de uma instância - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p><code>RetrieveRenderedInstanceAsync</code> e <code>RetrieveRenderedFramesAsync</code> retornam, em vez disso, uma imagem em tamanho real. <code>DicomWebRenderOptions</code> repassa os controles de renderização padrão ao servidor: qualidade da imagem, tamanho da viewport, janela e perfil ICC.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Enviar instâncias com STOW-RS">}}

<p>STOW-RS envia qualquer número de instâncias em uma única requisição. O servidor responde com um relatório que lista, instância por instância, o que foi armazenado e o que foi recusado.</p>

<div class="codeblock" id="code">
 <h3>Enviar uma pasta de arquivos DICOM - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>Alguns servidores respondem com um erro HTTP, como 409, quando uma ou mais instâncias falham. A chamada então lança <code>DicomWebClientException</code>, como mostra a seção seguinte, e o mesmo relatório está no corpo da resposta da exceção.</p>

<p><code>StoreStudyAsync</code> envia para um estudo nomeado. Ambos os métodos também aceitam um <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, de modo que os arquivos podem ser produzidos enquanto a requisição está sendo enviada.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lidar com erros do servidor">}}

<p>Cada operação tipada verifica o status HTTP antes de retornar. Quando o servidor recusa uma requisição, a chamada lança <code>DicomWebClientException</code>, que contém o código de status, os cabeçalhos e o início do corpo da resposta para o log.</p>

<div class="codeblock" id="code">
 <h3>Relatar uma requisição falhada - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>Métodos que retornam uma resposta bruta, como a miniatura acima, deixam a verificação de status a seu cargo via <code>IsSuccessStatusCode</code> ou <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="O que mais o cliente cobre">}}

<p>Pesquisa, download e envio são as chamadas comuns. O cliente também possui um grupo de operações para cada um dos outros serviços DICOMweb:</p>

<ul>
<li>Dados em massa, dados de pixel e quadros selecionados de uma instância, por número de quadro baseado em 1, conforme enviados pelo servidor.</li>
<li>WADO-URI, a recuperação de URL única mais antiga que muitos visualizadores e arquivos ainda utilizam, para arquivos DICOM e imagens renderizadas.</li>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> listas de trabalho: criar, pesquisar, atualizar e reivindicar itens de trabalho, alterar seu estado, solicitar cancelamento e inscrever‑se em eventos que chegam via WebSocket.</li>
<li>A <a href="/medical/net/modality-worklist/">lista de modalidade</a> (MWL-RS), etapas de procedimento realizadas por modalidade (MPPS-RS) e compromisso de armazenamento.</li>
<li>Instâncias não‑paciente como paletas de cores e protocolos pendentes, e o documento de capacidades do servidor.</li>
<li><code>SendAsync</code> para qualquer transação padrão com seus próprios valores de rota, parâmetros de consulta, cabeçalhos e corpo, quando um método tipado não for adequado.</li>
</ul>

<p>O <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">guia do cliente DICOMweb</a> documenta cada operação e opção, e a <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">referência de operações do cliente</a> lista cada método.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Uma biblioteca desde a resposta HTTP até os dados de pixel">}}

<p>Um arquivo baixado com o cliente é um <code>DicomFile</code> regular. Ele pode ser anonimizado, transcodificado, renderizado ou serializado no mesmo processo, sem etapa de conversão e sem segunda biblioteca na cadeia.</p>

<p>Páginas relacionadas: <a href="/medical/net/dicomweb-server/">servidor DICOMweb</a> para o outro lado do protocolo em seu próprio serviço ASP.NET Core, <a href="/medical/net/dicom-networking/">rede DICOM</a> para arquivos que utilizam DIMSE em vez de HTTP, <a href="/medical/net/anonymization/">anonimização</a> antes que um estudo deixe o hospital, <a href="/medical/net/dicom-transfer-syntax-conversion/">conversão de sintaxe de transferência</a> para o que o servidor armazena, e <a href="/medical/net/dicom-to-json/">DICOM para JSON</a> para o formato usado pelos resultados de pesquisa QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizado" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentação" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guia do Desenvolvedor" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="Referências da API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
