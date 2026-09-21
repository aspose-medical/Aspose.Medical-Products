---
title: HTJ2K em C# .NET - High-Throughput JPEG 2000 para DICOM | Aspose.Medical
weight: 10000

description: Compacte e leia imagens DICOM em High-Throughput JPEG 2000 a partir de C#. HTJ2K lossless, a variante RPCL e HTJ2K com perda, implementados em .NET gerenciado sem necessidade de codec nativo para implantação.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K em .NET C#" h2="High-Throughput JPEG 2000 para DICOM: a compressão que o padrão adicionou para arquivos rápidos e visualização em nuvem, implementada em C# gerenciado sem necessidade de componentes nativos." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="O que o HTJ2K altera">}}

<p>High-Throughput JPEG 2000 mantém a wavelet e a qualidade de imagem do JPEG 2000 e substitui a parte que o tornava lento. O codificador de blocos é novo, e a decodificação é uma ordem de magnitude mais rápida, o que fez com que o padrão DICOM o adotasse em três syntaxes de transferência e que plataformas de imagem em nuvem migrassem para ele.</p>

<p>Para uma equipe .NET a questão prática é diferente: quem realmente pode gerar esses arquivos. A maioria das bibliotecas alcança HTJ2K através de uma compilação nativa do OpenJPH, o que implica um binário por plataforma, uma etapa de build no contêiner e uma dependência que a revisão de segurança questionará. <strong>Aspose.Medical for .NET</strong> implementa o codec em código gerenciado dentro do mesmo pacote que lê e grava os arquivos, portanto o HTJ2K funciona da mesma forma no Windows, no Linux e em um contêiner, sem necessidade de instalação.</p>

<p>Três syntaxes de transferência são suportadas, e todas as três tanto leem quanto gravam:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), High-Throughput JPEG 2000 sem perdas.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), a variante sem perdas com ordem de progressão RPCL.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Compactar um estudo em HTJ2K">}}

<p>Uma única chamada move o arquivo para a nova syntax. O dataset, as tags privadas e as informações de metadados do arquivo acompanham a mudança.</p>

<div class="codeblock" id="code">
 <h3>Transcodificar um arquivo DICOM para HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>Em uma imagem de 1714 por 1933 pixels com 16 bits do nosso conjunto de testes, o arquivo passa de 6,3 MB para 2,9 MB, e os pixels retornam bit a bit. Os números variam por modalidade e por imagem, portanto meça nos seus próprios dados, o que equivale a um loop sobre os arquivos que você já possui.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossless significa sem perdas">}}

<p>Dados diagnósticos não toleram um codec que seja quase correto. Transcodifique para HTJ2K sem perdas e de volta, e os dados de pixel são idênticos aos bytes originais, o que é uma propriedade que você pode validar em sua própria suíte de testes antes de aceitar recomprimir um arquivo.</p>

<div class="codeblock" id="code">
 <h3>Voltar a uma syntax não compactada - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, a variante feita para visualização em rede">}}

<p>A syntax 1.2.840.10008.1.2.4.202 armazena o mesmo codestream sem perdas na ordem de progressão RPCL: resolução primeiro, depois posição, depois componente, depois camada. Um leitor que captura apenas o início do fluxo obtém uma imagem de baixa resolução completa, que é o que um visualizador precisa ao abrir um estudo grande por um link que não controla.</p>

<div class="codeblock" id="code">
 <h3>Compactar com a ordem de progressão RPCL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Leia o que um arquivo envia">}}

<p>A outra metade do trabalho é aceitar HTJ2K de sistemas que já o geram. Abra o arquivo, verifique em que formato está armazenado e trabalhe com os dados de pixel.</p>

<div class="codeblock" id="code">
 <h3>Ler um arquivo HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Imagens multi-frame são tratadas quadro a quadro, portanto uma série longa consome memória por quadro em vez de por estudo.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Onde o HTJ2K ganha seu espaço">}}

<ul>
<li>Migração de arquivos: recomprimir um estudo armazenado em HTJ2K sem perdas, reduzir a pegada, manter os dados diagnósticos intactos.</li>
<li>Nuvem e DICOMweb: a velocidade de decodificação é o que faz um visualizador no navegador ou no servidor parecer imediato em imagens grandes.</li>
<li>Pipeline de IA: conjuntos de treinamento são lidos muito mais vezes do que são escritos, e o tempo de decodificação é o custo recorrente.</li>
<li>Contêineres e serverless: o codec faz parte do assembly, portanto a imagem não necessita de biblioteca nativa ou compilador na construção.</li>
</ul>

<p>A biblioteca também inclui JPEG XL, a outra adição recente ao padrão, e os codecs mais antigos que um arquivo pode conter: JPEG, JPEG-LS, JPEG 2000 e RLE. A página <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> cobre todo o conjunto, e a página <a href="/medical/net/jpeg2000/">JPEG 2000</a> aborda o codec do qual o HTJ2K evoluiu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizagem" tabId="resources" >}}
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
