---
title: Converter DICOM para TIFF em C# .NET | Aspose.Medical
weight: 12000
description: Renderize imagens DICOM para dados de pixels em C# .NET e salve em TIFF com suporte a múltiplas páginas para arquivos DICOM multiquadro. Use o pipeline completo de LUT DICOM com a API Aspose.Medical e salve em TIFF usando qualquer biblioteca de imagens .NET.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converter DICOM para TIFF em .NET C#" h2="Renderize imagens DICOM para dados brutos de pixels com suporte total ao pipeline de escala de cinza. Preserve a qualidade da imagem com controle de janela/nível, renderização de sobreposições e processamento de múltiplos quadros. Salve em TIFF &mdash; incluindo TIFF de múltiplas páginas &mdash; usando qualquer biblioteca de imagens .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Conversão de DICOM para TIFF">}}

<p><strong>Aspose.Medical for .NET</strong> renderiza imagens DICOM através do pipeline padrão de escala de cinza DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT) para produzir dados de pixel BGRA32 corretamente ajustados por janela. TIFF é um formato versátil amplamente usado em imagens médicas, pesquisa científica e publicação porque suporta compressão sem perdas, alta profundidade de bits e &mdash; mais importante &mdash; <strong>documentos multi‑página</strong>. Isso torna o TIFF a escolha natural para converter arquivos DICOM multi‑quadro (CT, MRI, ultrassom) em um único arquivo de saída que preserva todas as fatias.</p>

<p>A saída renderizada é um <code>IRawImage</code> &mdash; um buffer de pixels BGRA32 com <code>Width</code>, <code>Height</code> e um array <code>Pixels</code>. Você pode salvar esses dados de pixel em TIFF usando qualquer biblioteca de imagens .NET, como <code>System.Drawing</code>, <code>SkiaSharp</code> ou <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderizar DICOM para Dados de Pixels em C#">}}

<p>Carregue um arquivo DICOM, renderize o quadro desejado e acesse os dados de pixel:</p>

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

<p>O método <code>RenderImage</code> aplica automaticamente os valores de janela/nível e os parâmetros de LUT armazenados no conjunto de dados DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Salvar Imagem Renderizada como TIFF">}}

<p>O <code>IRawImage</code> fornece dados brutos de pixel BGRA32 que podem ser salvos em TIFF usando qualquer biblioteca de imagens .NET. Aqui está um exemplo usando <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Salvar quadro DICOM renderizado como TIFF - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("xray.dcm");
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

// Save as TIFF
bitmap.Save("xray.tiff", ImageFormat.Tiff);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="TIFF Multi‑Página a partir de DICOM Multi‑Quadro">}}

<p>Arquivos DICOM multi‑quadro (por exemplo, séries de CT ou MRI) contêm múltiplas fatias de imagem em um único arquivo. A capacidade de múltiplas páginas do TIFF permite armazenar todos os quadros em um único arquivo de saída, preservando o estudo completo em um formato universalmente legível. Use o codificador TIFF do <code>System.Drawing</code> com parâmetros multi‑quadro:</p>

<div class="codeblock" id="code">
 <h3>Converter DICOM multi‑quadro para TIFF multi‑página - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
int totalFrames = dicomFile.NumberOfFrames;

// Get the TIFF codec
var tiffCodec = ImageCodecInfo.GetImageEncoders()
    .First(c =&gt; c.FormatID == ImageFormat.Tiff.Guid);

// Multi-frame parameters
var multiFrameParams = new EncoderParameters(2);
multiFrameParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.MultiFrame);
multiFrameParams.Param[1] = new EncoderParameter(
    Encoder.Compression, (long)EncoderValue.CompressionLZW);

// Parameter for adding subsequent frames
var frameAddParams = new EncoderParameters(1);
frameAddParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.FrameDimensionPage);

// Parameter for flushing the final frame
var flushParams = new EncoderParameters(1);
flushParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.Flush);

Bitmap? tiffBitmap = null;

for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var frameBitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = frameBitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    frameBitmap.UnlockBits(bitmapData);

    if (i == 0)
    {
        // Save the first frame and start multi-page TIFF
        tiffBitmap = (Bitmap)frameBitmap.Clone();
        tiffBitmap.Save("ct_series.tiff", tiffCodec, multiFrameParams);
    }
    else
    {
        // Add subsequent frames
        tiffBitmap!.SaveAdd(frameBitmap, frameAddParams);
    }
}

// Flush and close the multi-page TIFF
tiffBitmap?.SaveAdd(flushParams);
tiffBitmap?.Dispose();</code></pre>
</div>

<p>Você também pode salvar cada quadro como um arquivo TIFF separado quando fatias individuais forem necessárias:</p>

<div class="codeblock" id="code">
 <h3>Converter cada quadro em um TIFF separado - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

for (int i = 0; i &lt; dicomFile.NumberOfFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var bitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = bitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    bitmap.UnlockBits(bitmapData);
    bitmap.Save($"frame_{i:D4}.tiff", ImageFormat.Tiff);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Janela/Nível Personalizado">}}

<p>Controle qual intervalo de valores de pixel é renderizado definindo o Window Center e o Window Width. Diferentes configurações de janela revelam diferentes estruturas anatômicas a partir dos mesmos dados DICOM:</p>

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
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

<p>A classe <code>GrayscaleRenderOptions</code> também expõe as propriedades <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> e <code>BitDepth</code> para controle total sobre o pipeline de renderização.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderização de DICOM Multi‑Quadro">}}

<p>Arquivos DICOM de CT, MRI e ultrassom frequentemente contêm múltiplos quadros (fatias). Renderize todos os quadros para dados de pixel:</p>

<div class="codeblock" id="code">
 <h3>Renderizar todos os quadros de DICOM multi‑quadro - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as TIFF using your preferred imaging library
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
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG para Imagens Médicas">}}

<p>Cada formato de saída atende a diferentes casos de uso:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Característica</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Compressão</strong></td><td>Sem perdas (LZW, ZIP) ou sem compressão</td><td>Sem perdas</td><td>Com perdas</td></tr>
<tr><td><strong>Multi‑Página</strong></td><td>Suportado &mdash; todos os quadros em um arquivo</td><td>Não suportado</td><td>Não suportado</td></tr>
<tr><td><strong>Qualidade da Imagem</strong></td><td>Pixel-perfect</td><td>Pixel-perfect</td><td>Possíveis artefatos de compressão</td></tr>
<tr><td><strong>Tamanho do Arquivo</strong></td><td>Maior</td><td>Médio</td><td>Menor</td></tr>
<tr><td><strong>Transparência</strong></td><td>Suportado</td><td>Suportado</td><td>Não suportado</td></tr>
<tr><td><strong>Melhor para</strong></td><td>Arquivamento, séries multi‑quadro, impressão, pesquisa</td><td>Revisão diagnóstica, web com qualidade</td><td>Compartilhamento web, miniaturas</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET usa o mesmo pipeline de renderização para todos os destinos de saída. Os dados de pixel <code>IRawImage</code> são idênticos independentemente do formato de destino &mdash; a escolha do formato afeta apenas a etapa final de gravação realizada pela sua biblioteca de imagens. O TIFF é o formato preferido quando você precisa preservar um estudo DICOM multi‑quadro completo em um único arquivo ou quando a saída destina‑se a arquivamento, impressão ou processamento de imagem adicional.</p>

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
