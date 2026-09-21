---
title: JPEG XL para DICOM em C# .NET | Aspose.Medical
weight: 10500

description: Armazene imagens DICOM em JPEG XL a partir de C#. JPEG XL sem perdas que devolve os pixels bit a bit, em uma única assembly gerenciada sem nenhum codec nativo para implantar.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL para DICOM em .NET C#" h2="A compressão mais recente no padrão DICOM, com os arquivos sem perdas menores que medimos, implementada em C# gerenciado e distribuída em uma única assembly." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Por que o JPEG XL chegou ao DICOM">}}

<p>Os arquivos médicos crescem e nunca diminuem. JPEG XL é o codec que o mundo de imagem projetou após duas décadas de experiência com JPEG e JPEG 2000, e o DICOM o adicionou como sintaxe de transferência pela razão que as equipes de armazenamento se importam: para os mesmos pixels, o arquivo é menor.</p>

<p><strong>Aspose.Medical para .NET</strong> escreve e lê JPEG XL por meio de uma porta C# de libjxl que reside dentro da biblioteca. O pacote entrega uma única assembly, <code>Aspose.Medical.dll</code>, e nenhum binário nativo ao lado, de modo que um codec tão novo não se transforma em um projeto de implantação: a mesma assembly funciona no Windows, no Linux, em um agente de compilação e em um contêiner.</p>

<p>Dois sintaxes de transferência carregam os pixels:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), para dados diagnósticos que precisam retornar inalterados.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), para os casos em que um arquivo menor importa mais do que uma cópia exata.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Compacte um estudo, mantenha cada pixel">}}

<p>Transcodificação é uma única chamada, e o conjunto de dados em torno dos pixels viaja com ela.</p>

<div class="codeblock" id="code">
 <h3>Transcodificar um arquivo DICOM para JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Medimos em uma imagem de 1714 por 1933 de 16 bits do nosso próprio conjunto de testes: 6,3 MB descomprimido se torna 2,7 MB em JPEG XL sem perdas, que é menor que a mesma imagem em HTJ2K sem perdas. Seus próprios números dependem da modalidade, então execute a comparação em uma pasta dos seus arquivos antes de escolher.</p>

<p>Sem perdas é a palavra a ser levada ao pé da letra aqui. Transcodifique para JPEG XL e volte, e os dados de pixel são iguais aos bytes com que você começou, de modo que um arquivo pode ser recomprimido sem discussão sobre a qualidade diagnóstica.</p>

<div class="codeblock" id="code">
 <h3>Voltar a uma sintaxe não comprimida - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Leia o que já está armazenado como JPEG XL">}}

<p>Um arquivo que chega em JPEG XL abre como qualquer outro. A sintaxe de transferência indica o que é, e os dados de pixel ficam disponíveis assim que o quadro é decodificado.</p>

<div class="codeblock" id="code">
 <h3>Abrir um arquivo JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL ou HTJ2K">}}

<p>Ambos são recentes, ambos são sem perdas quando você solicita sem perdas, e a biblioteca escreve e lê ambos. Eles respondem a perguntas diferentes.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Pergunta</th>
<th>Resposta</th>
</tr>
</thead>
<tbody>
<tr><td>Qual produziu o arquivo menor em nosso teste</td><td>JPEG XL sem perdas, por alguns percentuais</td></tr>
<tr><td>Qual foi criado para visualização progressiva via rede</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, especialmente a variante RPCL</td></tr>
<tr><td>Qual entrou primeiro no padrão DICOM</td><td>HTJ2K, portanto mais arquivos o aceitam hoje</td></tr>
<tr><td>Qual gera uma dependência nativa aqui</td><td>Nenhum, ambos são código gerenciado em uma única assembly</td></tr>
</tbody>
</table>

<p>A escolha geralmente vem do outro lado da cadeia: transcodifique para a sintaxe que o arquivo aceita, e mantenha o resto do pipeline igual.</p>

<div class="codeblock" id="code">
 <h3>Deixe o arquivo de destino decidir - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Onde compensa">}}

<ul>
<li>Arquivos de longo prazo: os mesmos estudos, menos terabytes, e nenhuma perda a justificar para um radiologista.</li>
<li>Faturas de armazenamento em nuvem: a economia se repete a cada mês, enquanto a transcodificação ocorre uma única vez.</li>
<li>Conjuntos de dados para pesquisa e IA: cópias menores se movem mais rápido entre armazenamento e treinamento.</li>
<li>Implantação: um codec tão novo normalmente significa uma compilação nativa por plataforma; aqui ele faz parte da assembly que você já referencia.</li>
</ul>

<p>A biblioteca também grava os codecs que um arquivo existente contém: JPEG, JPEG-LS, JPEG 2000, HTJ2K e RLE. A página de <a href="/medical/net/dicom-transfer-syntax-conversion/">conversão de sintaxe de transferência</a> cobre todo o conjunto, <a href="/medical/net/htj2k/">HTJ2K</a> tem sua própria página, e <a href="/medical/net/jpeg2000/">JPEG 2000</a> é de onde vêm ambos os novos codecs.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizado" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentação" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guia do Desenvolvedor" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
