---
title: Trabalhe com arquivos DICOM grandes em C# .NET | Aspose.Medical
weight: 11500

description: Abra estudos multi‑frame e imagens de lâmina inteira em C# sem carregá‑los na memória. Leia metadados sem os dados de pixel, adie elementos grandes e mova arquivos por streams e pipes.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Arquivos DICOM grandes em .NET C#" h2="Leia os metadados de um estudo multi‑frame sem os pixels, adie elementos grandes até que algo solicite‑os e mova arquivos completos por streams e pipes." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="O arquivo é grande, a questão costuma ser pequena">}}

<p>Uma imagem de lâmina inteira, uma série de CT longa ou um volume OCT tem centenas de megabytes, e a maior parte são dados de pixel. O trabalho que um aplicativo realmente realiza costuma ser muito menor: listar o que há em uma pasta, verificar um identificador de paciente, contar os quadros, decidir onde um estudo deve ser armazenado. Carregar cada byte para responder a isso é o que transforma uma tarefa simples em um problema de memória.</p>

<p><strong>Aspose.Medical for .NET</strong> permite que o chamador decida quanto de um arquivo será lido. A escolha é um argumento em <code>DicomFile.Open</code>, e se aplica a arquivos, streams e pipes igualmente.</p>

<p>Medido em um estudo de 14 MB com 128 quadros do nosso conjunto de testes, na mesma máquina e no mesmo arquivo:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Estrategia de leitura</th>
<th>Tempo para abrir</th>
<th>Memória alocada</th>
</tr>
</thead>
<tbody>
<tr><td>Tudo, o padrão</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Elementos grandes ignorados</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Elementos grandes adiados</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>O diferencial aumenta com o tamanho do arquivo. Uma pasta com 10.000 estudos é o caso em que deixa de ser uma micro‑otimização.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Leia os metadados, deixe os pixels intactos">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> exclui da leitura todo elemento acima de um limite de tamanho. O dataset retornado contém apenas as tags que um índice ou roteador necessita.</p>

<div class="codeblock" id="code">
 <h3>Leia um estudo sem seus dados de pixel - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>O limite padrão é 64 kB e aceita um valor em kilobytes, portanto um fluxo de trabalho que trata 8 kB como grande pode defini‑lo.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Adiar em vez de ignorar">}}

<p>Quando os pixels podem ser necessários, mas provavelmente mais tarde e provavelmente não todos, <code>ReadLargeOnDemand</code> é a outra metade do par. Abrir o arquivo tem o mesmo custo que ignorar, e um elemento grande é lido no momento em que o código o acessa.</p>

<div class="codeblock" id="code">
 <h3>Carregue um quadro somente quando for usado - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>A leitura adiada é um recurso licenciado; as demais estratégias também funcionam em avaliação.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Indexe uma pasta sem tocar nos pixels">}}

<p>A mesma estratégia se aplica a um stream, que é o que uma varredura de arquivo ou um armazenamento de objetos na nuvem aparenta a partir do código.</p>

<div class="codeblock" id="code">
 <h3>Varra um arquivo - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streams e pipes, de entrada e saída">}}

<p>Leitura e gravação aceitam streams, e os pontos de entrada assíncronos também aceitam tipos <code>System.IO.Pipelines</code>. Um estudo pode viajar de uma resposta de rede para o armazenamento sem que o processo tenha que manter todo o arquivo como um único array.</p>

<div class="codeblock" id="code">
 <h3>Leia e escreva via streams - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>A mesma ideia abrange as representações textuais: um documento com muitos datasets é lido um dataset de cada vez nas páginas <a href="/medical/net/json-to-dicom/">JSON para DICOM</a> e <a href="/medical/net/xml-to-dicom/">XML para DICOM</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Quadro a quadro">}}

<p>Dados multi‑frame são endereçados por quadro, portanto uma série de 500 quadros custa um quadro de cada vez, em vez de todo o elemento de dados de pixel.</p>

<div class="codeblock" id="code">
 <h3>Percorra os quadros - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Onde isso decide o design">}}

<ul>
<li>Indexação e migração de arquivos: milhões de arquivos, e somente o cabeçalho importa até que algo seja movido.</li>
<li>Roteadores e nós de armazenamento: aceitam um estudo, leem o necessário para roteá‑lo, repassam os bytes.</li>
<li>Fluxos de trabalho de IA: criam o manifesto a partir dos metadados e então extraem quadros para o subconjunto efetivamente usado no treinamento.</li>
<li>Contêineres com limite de memória: o conjunto de trabalho segue a estratégia, não o tamanho do arquivo.</li>
<li>Dados de lâmina inteira e OCT: arquivos onde ler tudo não é uma opção.</li>
</ul>

<p>O <a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">guia de gerenciamento de memória</a> explica as estratégias em detalhes, e <a href="/medical/net/dicom-networking/">a rede DICOM</a> mostra os mesmos dados chegando via DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizado" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentação" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guia do desenvolvedor" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
{{< blocks/products/pf/slr-element name="Referências de API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Suporte ao produto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Suporte gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Suporte pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Por que Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Histórias de sucesso" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
