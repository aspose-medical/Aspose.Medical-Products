---
title: Converter DICOM para PNG em C# .NET | Aspose.Medical
weight: 11000
description: Renderize imagens DICOM para dados de pixel em C# .NET com qualidade lossless, controle de janela/nível e suporte a multi‑frames. Use o pipeline completo de LUT DICOM com a API Aspose.Medical e salve em PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converter DICOM para PNG em .NET C#" h2="Renderize imagens DICOM para dados de pixel brutos com suporte completo ao pipeline grayscale. Preserve a qualidade da imagem com controle de janela/nível, renderização de sobreposições e processamento de multi‑frames. Salve em PNG lossless usando qualquer biblioteca de imagens .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Conversão de DICOM para PNG">}}

<p><strong>Aspose.Medical for .NET</strong> renderiza imagens DICOM através do pipeline padrão de grayscale DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT) para produzir dados de pixel BGRA32 corretamente windowed. PNG é um formato lossless, tornando‑se a escolha preferida quando a qualidade da imagem deve ser preservada sem artefatos de compressão &mdash; essencial para revisão diagnóstica, arquivamento e casos de uso em pesquisa.</p>

<p>A saída renderizada é um <code>IRawImage</code> &mdash; um buffer de pixels BGRA32 com <code>Width</code>, <code>Height</code> e um array <code>Pixels</code>. Você pode salvar esses dados de pixel em PNG usando qualquer biblioteca de imagens .NET como <code>System.Drawing</code>, <code>SkiaSharp</code> ou <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderizar DICOM para Dados de Pixel em C#">}}

<p>Carregue um arquivo DICOM, renderize o frame desejado e acesse os dados de pixel:</p>

<div class="codeblock" id="code">
 <h3>Renderizar imagem DICOM - C#</h3>
 <pre><code class="cs">using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;

// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Render the first frame (index 0) through the full LUT pipeline
IRawImage image = dicomFile.RenderImage(0);

// Access the rendered pixel data
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = image.Pixels; // BGRA32 pixel buffer</code></pre>
</div>

<p>O método <code>RenderImage</code> aplica automaticamente os valores de window/level e os parâmetros LUT armazenados no dataset DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Salvar Imagem Renderizada como PNG">}}

<p>O <code>IRawImage</code> fornece dados de pixel BGRA32 brutos que podem ser salvos em PNG lossless usando qualquer biblioteca de imagens .NET. Aqui está um exemplo usando <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Salvar frame DICOM renderizado como PNG - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");
IRawImage image = dicomFile.RenderImage(0);

// Create a Bitmap from the BGRA32 pixel data
using var bitmap = new Bitmap(image.Width, image.Height, PixelFormat.Format32bppArgb);
var bitmapData = bitmap.LockBits(
    new Rectangle(0, 0, image.Width, image.Height),
    ImageLockMode.WriteOnly,
    PixelFormat.Format32bppArgb);

// Copy BGRA32 pixels to the bitmap
MemoryMarshal.AsBytes(image.Pixels.AsSpan())
    .CopyTo(new Span&lt;byte&gt;(
        (void*)bitmapData.Scan0,
        bitmapData.Stride * bitmapData.Height));

bitmap.UnlockBits(bitmapData);

// Save as lossless PNG
bitmap.Save("ct_scan.png", ImageFormat.Png);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Janela/Nível Personalizados">}}

<p>Controle qual intervalo de valores de pixel é renderizado definindo o Window Center e o Window Width. Configurações de janela diferentes revelam diferentes estruturas anatômicas a partir dos mesmos dados DICOM:</p>

<div class="codeblock" id="code">
 <h3>Renderizar com janela/nível personalizado - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);</code></pre>
</div>

<p>A classe <code>GrayscaleRenderOptions</code> também expõe as propriedades <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> e <code>BitDepth</code> para controle total do pipeline de renderização.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderização DICOM Multi‑Frame">}}

<p>Arquivos DICOM de TC, RM e ultrassom frequentemente contêm múltiplos frames (fatias). Renderize todos os frames para dados de pixel:</p>

<div class="codeblock" id="code">
 <h3>Renderizar todos os frames de DICOM multi‑frame - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as PNG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Controle de Sobreposição">}}

<p>Imagens DICOM podem conter planos de sobreposição com anotações, medições ou regiões de interesse. Controle a visibilidade e a cor da sobreposição ao renderizar:</p>

<div class="codeblock" id="code">
 <h3>Renderizar com e sem sobreposições - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible (default)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions { ShowOverlays = false };
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG para Imagens Médicas">}}

<p>Escolher entre PNG e JPG depende do caso de uso:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Característica</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Compressão</strong></td><td>Lossless</td><td>Lossy</td></tr>
<tr><td><strong>Qualidade da Imagem</strong></td><td>Pixel-perfeito &mdash; sem artefatos</td><td>Possíveis pequenos artefatos de compressão</td></tr>
<tr><td><strong>Tamanho do Arquivo</strong></td><td>Maior (tipicamente 2&ndash;10x vs JPG)</td><td>Menor</td></tr>
<tr><td><strong>Transparência</strong></td><td>Suportado (canal alfa)</td><td>Não suportado</td></tr>
<tr><td><strong>Ideal para</strong></td><td>Revisão diagnóstica, arquivamento, pesquisa, sobreposições</td><td>Compartilhamento web, miniaturas, pré‑visualizações</td></tr>
<tr><td><strong>Regulamentação</strong></td><td>Preferido para fluxos de trabalho críticos de qualidade</td><td>Aceitável para uso não diagnóstico</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET usa o mesmo pipeline de renderização para ambos os destinos de saída. Os dados de pixel do <code>IRawImage</code> são idênticos independentemente do formato de destino &mdash; a escolha do formato afeta apenas a etapa final de salvamento executada pela sua biblioteca de imagens.</p>

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

{{< blocks/products/pf/slr-tab tabTitle="Por que Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de Clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Casos de Sucesso" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
