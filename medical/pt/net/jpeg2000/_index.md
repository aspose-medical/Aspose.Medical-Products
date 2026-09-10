---
title: Compressão DICOM JPEG 2000 em C# .NET | Aspose.Medical
weight: 2000
description: Leia, escreva e transcodifique arquivos DICOM com compressão JPEG 2000 em C# .NET. Suporte para imagens de 8 bits e 16 bits, modos lossless e lossy, dados multi‑componente com a API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Suporte a DICOM JPEG 2000 em .NET C#" h2="Leia, escreva e transcodifique arquivos DICOM com compressão JPEG 2000. Modos lossless e lossy, dados de pixel de 8 bits e 16 bits, imagens multi‑componente — tudo em .NET puro." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 em Imagem Médica">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) é o padrão de compressão baseado em wavelet mais amplamente utilizado em imagem médica. Diferente do JPEG tradicional, ele oferece compressão lossless e lossy em um único codec, decodificação progressiva para acesso a região de interesse e razões de compressão superiores &mdash; tornando‑o ideal para arquivar estudos grandes e transmitir imagens em redes com largura de banda limitada.</p>

<p><strong>Aspose.Medical for .NET</strong> oferece uma implementação pura em C# do codec JPEG 2000 sem dependências nativas. A biblioteca pode ler, renderizar e transcodificar arquivos DICOM comprimidos com qualquer uma das quatro sintaxes de transferência padrão JPEG 2000.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sintaxes de Transferência JPEG 2000 Compatíveis">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Sintaxe de Transferência</th>
<th>UID</th>
<th>Modo</th>
<th>Leitura</th>
<th>Gravação</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 somente Lossless</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>8 bits e 16 bits</td><td>8 bits</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy ou lossless</td><td>8 bits e 16 bits</td><td>8 bits</td></tr>
<tr><td>JPEG 2000 Parte 2 Multi‑componente somente Lossless</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>8 bits e 16 bits</td><td>8 bits</td></tr>
<tr><td>JPEG 2000 Parte 2 Multi‑componente</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy ou lossless</td><td>8 bits e 16 bits</td><td>8 bits</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dados de Pixel de 8 Bits e 16 Bits">}}

<p>Imagens médicas frequentemente utilizam 16 bits por amostra para capturar toda a faixa dinâmica de modalidades como CT (tipicamente 12 bits armazenados em 16 bits) e MRI. Aspose.Medical manipula ambas as profundidades de bits para JPEG 2000:</p>

<ul>
<li><strong>Leitura (descompressão)</strong>: Suporte completo para arquivos DICOM comprimidos JPEG 2000 de 8 bits e 16 bits. A biblioteca decodifica corretamente os dados de pixel independentemente dos valores originais de Bits Allocated, Bits Stored e High Bit.</li>
<li><strong>Gravação (compressão)</strong>: Atualmente suporta imagens de 8 bits. O suporte a gravação de 16 bits está planejado para uma versão futura.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Leia e inspecione DICOM comprimido JPEG 2000 - C#</h3>
 <pre><code class="cs">// Open a JPEG 2000 compressed DICOM file (8-bit or 16-bit)
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Check transfer syntax
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax
Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");

// Inspect pixel data bit depth
PixelData pixelData = PixelData.Create(dicomFile.Dataset);
Console.WriteLine($"Bits Allocated: {pixelData.BitsAllocated}");
Console.WriteLine($"Bits Stored: {pixelData.BitsStored}");
Console.WriteLine($"High Bit: {pixelData.HighBit}");
Console.WriteLine($"Samples Per Pixel: {pixelData.SamplesPerPixel}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transcodificar para JPEG 2000">}}

<p>Use o método <code>Transcode</code> para comprimir qualquer arquivo DICOM para JPEG 2000 ou converter entre modos JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Comprimir DICOM para JPEG 2000 Lossless - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Comprimir DICOM para JPEG 2000 Lossy - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Descomprimir Arquivos DICOM JPEG 2000">}}

<p>Descomprima arquivos JPEG 2000 para uma sintaxe de transferência não comprimida para processamento, análise ou compatibilidade com sistemas que não suportam JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Descomprimir JPEG 2000 para não comprimido - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Você também pode descomprimir e transcodificar para outros formatos de compressão em um único passo:</p>

<div class="codeblock" id="code">
 <h3>Transcodificar entre formatos de compressão - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderizar Imagens DICOM JPEG 2000">}}

<p>Arquivos DICOM comprimidos JPEG 2000 podem ser renderizados para dados de pixel para exibição ou exportação, assim como qualquer outra sintaxe de transferência:</p>

<div class="codeblock" id="code">
 <h3>Renderizar um quadro comprimido JPEG 2000 - C#</h3>
 <pre><code class="cs">// Open JPEG 2000 DICOM file
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Render the first frame — decompression is handled automatically
using PixelImage&lt;Bgra32&gt; image = dicomFile.RenderImage(0);

// Copy the BGRA32 pixel data for display or export
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = new Bgra32[width * height];
image.CopyPixelsTo(pixels);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossless vs Lossy JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Aspecto</th>
<th>JPEG 2000 Lossless</th>
<th>JPEG 2000 Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Sintaxe de Transferência</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Qualidade da imagem</td><td>Pixel perfeito &mdash; idêntico ao original</td><td>Visivelmente semelhante, alguns dados perdidos permanentemente</td></tr>
<tr><td>Taxa de compressão</td><td>Tipicamente 2:1 a 3:1</td><td>Tipicamente 10:1 a 30:1 ou mais</td></tr>
<tr><td>Ideal para</td><td>Arquivo diagnóstico, registros legais, leitura primária</td><td>Revisão preliminar, telemedicina, transmissão em rede</td></tr>
<tr><td>Segurança em ida e volta</td><td>Sim</td><td>Não &mdash; recompactação degrada ainda mais a qualidade</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Parte 2 Multi‑Componente">}}

<p>JPEG 2000 Parte 2 (ISO/IEC 15444-2) estende o codec padrão com capacidades de transformação multi‑componente. Isso é usado para imagens médicas coloridas e modalidades que produzem dados de múltiplos canais. Aspose.Medical suporta ambas as sintaxes de transferência da Parte 2:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; compressão lossless com decorrelação inter‑componente para compressão ótima de dados multicanal.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; compressão lossy ou lossless com transformações multi‑componente.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 de Alta Taxa (HTJ2K) — Em Breve">}}

<p>HTJ2K (ISO/IEC 15444-15) é uma extensão de próxima geração do JPEG 2000 projetada para velocidades de codificação e decodificação drasticamente mais rápidas, mantendo a mesma eficiência de compressão. Espera‑se que se torne o codec preferido para fluxos de trabalho de imagem médica em tempo real.</p>

<p>Aspose.Medical adicionará suporte a HTJ2K em uma versão futura, abrangendo três sintaxes de transferência:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; somente Lossless</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; Lossless com ordem de progressão RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; Lossy ou lossless</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizado" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentação" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Código Fonte" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referências da API" href="https://reference.aspose.com/medical/net/" >}}
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
