---
title: Conversão de Sintaxe de Transferência DICOM em C# .NET | Aspose.Medical
weight: 16000
description: Transcodifique arquivos DICOM entre sintaxes de transferência em C# .NET. Suporte a JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, RLE e formatos não comprimidos com a API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Conversão de Sintaxe de Transferência DICOM em .NET C#" h2="Transcodifique arquivos DICOM entre sintaxes de transferência não comprimidas, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS e RLE. Biblioteca .NET pura sem dependências nativas." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="O que é Sintaxe de Transferência?">}}

<p>Uma <strong>Transfer Syntax</strong> define como os dados DICOM são codificados para armazenamento e transmissão. Ela especifica três aspectos principais: ordem dos bytes (endianness), se as Representações de Valor são explícitas ou implícitas, e o algoritmo de compressão aplicado aos dados de pixel. Cada arquivo DICOM declara sua sintaxe de transferência no cabeçalho de Informações Meta do Arquivo.</p>

<p>Diversos dispositivos médicos, servidores PACS e aplicativos de visualização suportam diferentes conjuntos de sintaxes de transferência. <strong>Aspose.Medical for .NET</strong> fornece o método <code>Transcode</code> para converter entre sintaxes de transferência, possibilitando interoperabilidade, otimização de armazenamento e compatibilidade com ferramentas de processamento &mdash; tudo em uma biblioteca .NET pura sem dependências nativas.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transcodificar um Arquivo DICOM em C#">}}

<p>O método <code>DicomFile.Transcode</code> converte um arquivo DICOM da sua sintaxe de transferência atual para qualquer sintaxe de destino suportada. O método retorna uma nova instância <code>DicomFile</code> &mdash; o original permanece inalterado:</p>

<div class="codeblock" id="code">
 <h3>Transcodificação básica DICOM - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Você também pode transcodificar diretamente no nível <code>Dataset</code>:</p>

<div class="codeblock" id="code">
 <h3>Transcodificar um Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sintaxes de Transferência Compatíveis">}}

<p>A tabela a seguir lista todas as sintaxes padrão de transferência de dados de imagem DICOM e seu status de suporte atual no Aspose.Medical for .NET. Todos os codecs suportados são implementados em C# puro e são totalmente independentes de plataforma.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Sintaxe de Transferência</th>
<th>UID</th>
<th>Tipo</th>
<th>Status</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Não comprimido</strong></td></tr>
<tr><td>Implícito VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Não comprimido</td><td>Suportado</td></tr>
<tr><td>Explícito VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Não comprimido</td><td>Suportado</td></tr>
<tr><td>Explícito VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Não comprimido (descontinuado)</td><td>Suportado</td></tr>
<tr><td>Encapsulado Não comprimido Explícito VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Não comprimido</td><td>Não suportado</td></tr>
<tr><td>Deflacionado Explícito VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflacionado</td><td>Suportado</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Com perdas, 8‑bits</td><td>Suportado</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Com perdas, 12‑bits</td><td>Não suportado</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Sem perdas</td><td>Suportado (somente 8‑bits)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Sem perdas</td><td>Suportado (somente 8‑bits)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Sem perdas</td><td>Suportado</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Quase sem perdas</td><td>Suportado</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Sem perdas</td><td>Suportado (leitura 8‑bits colorido e 16‑bits monocromático; escrita 16‑bits monocromático ou 8‑bits RGB)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Com perdas ou sem perdas</td><td>Suportado (leitura 8‑bits colorido e 16‑bits monocromático; escrita 16‑bits monocromático ou 8‑bits RGB)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Sem perdas</td><td>Não suportado</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Com perdas ou sem perdas</td><td>Não suportado</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Sem perdas</td><td>Suportado</td></tr>
<tr><td colspan="4"><strong>JPEG 2000 de Alta Vazão (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Sem perdas</td><td>Suportado</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Sem perdas</td><td>Suportado</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Com perdas ou sem perdas</td><td>Suportado</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Sem perdas</td><td>Suportado</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Sem perdas</td><td>Somente decodificação (a codificação requer um fluxo JPEG de origem, não os dados de pixel)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Com perdas ou sem perdas</td><td>Suportado (modo com perdas)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Cenários Comuns de Transcodificação">}}

<p>Diferentes fluxos de trabalho requerem estratégias de transcodificação distintas. Aqui estão os cenários mais comuns:</p>

<div class="codeblock" id="code">
 <h3>Descompressão para processamento - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Compressão para armazenamento de arquivo - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Compressão para transmissão em rede - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Utilizar os codecs mais recentes: HTJ2K e JPEG XL - C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Inspecionar Propriedades da Sintaxe de Transferência">}}

<p>A classe <code>TransferSyntax</code> expõe propriedades que descrevem as características da codificação. Use-as para inspecionar a sintaxe de transferência atual de um arquivo ou para selecionar uma sintaxe de destino apropriada:</p>

<div class="codeblock" id="code">
 <h3>Ler propriedades da sintaxe de transferência - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>Propriedade</th>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>O identificador único da sintaxe de transferência</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Indica se as Representações de Valor são codificadas explicitamente</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Indica se a ordem dos bytes é little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Indica se os dados de pixel são encapsulados (comprimidos)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Indica se o método de compressão é com perdas</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Indica se a sintaxe usa compressão deflate</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Indica se a sintaxe de transferência foi descontinuada pelo padrão DICOM</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>O identificador padrão ISO do método de compressão com perdas</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Compressão com Perdas vs Sem Perdas">}}

<p>Compreender a diferença entre compressão com perdas e sem perdas é fundamental ao transcodificar arquivos DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Aspecto</th>
<th>Sem perdas</th>
<th>Com perdas</th>
</tr>
</thead>
<tbody>
<tr><td>Qualidade da imagem</td><td>Pixel-perfeito &mdash; dados originais totalmente preservados</td><td>Alguns dados perdidos permanentemente para obter tamanho menor</td></tr>
<tr><td>Taxa de compressão</td><td>Geralmente 2:1 a 3:1</td><td>Geralmente 10:1 a 30:1 ou mais</td></tr>
<tr><td>Segura em ida e volta</td><td>Sim &mdash; descompactar e obter pixels idênticos</td><td>Não &mdash; cada recompactação com perdas degrada ainda mais a qualidade</td></tr>
<tr><td>Casos de uso</td><td>Arquivamento, diagnóstico, registros legais</td><td>Revisão preliminar, telemedicina, transmissão em rede</td></tr>
<tr><td>Codecs suportados</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>Importante:</strong> Transcodificar de um arquivo comprimido com perdas para uma sintaxe sem perdas não restaura os dados perdidos. A degradação de qualidade da compressão original com perdas é permanente.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizado" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentação" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Código Fonte" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
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
