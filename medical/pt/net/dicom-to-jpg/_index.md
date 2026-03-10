---
title: Converter DICOM para JPG em C# .NET | Aspose.Medical
weight: 7000
description: Renderizar imagens DICOM para dados de pixel em C# .NET com controle de window/level, renderização de overlay e suporte a multi‑frame. Use o pipeline completo de LUT DICOM com a API Aspose.Medical e salve em JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converter DICOM para JPG em .NET C#" h2="Renderizar imagens DICOM para dados de pixel brutos com suporte total ao pipeline de tons de cinza, incluindo window/level, Modality LUT, VOI LUT e renderização de overlay. Salve em JPG usando qualquer biblioteca de imagens .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Pipeline de Renderização de Imagem DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> renderiza imagens DICOM através do pipeline padrão de tons de cinza DICOM definido na especificação DICOM. O processo de renderização aplica uma cadeia de Look‑Up Tables (LUTs) para transformar valores de pixel armazenados brutos em imagens exibíveis:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; converte valores de pixel armazenados em valores independentes do fabricante usando Rescale Slope/Intercept ou uma sequência LUT.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; aplica window/level (Window Center e Window Width) para selecionar a faixa de valores a ser exibida, com suporte a transformações Linear, Linear Exact e Sigmoid.</li>
<li><strong>Presentation LUT</strong> &mdash; mapeia os valores finais para P-Values exibíveis, incluindo suporte a INVERSE (inversão da imagem).</li>
<li><strong>Output LUT</strong> &mdash; converte valores em tons de cinza para RGB para exibição, com colorização opcional usando mapas de cores personalizados ou tabelas Palette Color.</li>
</ol>

<p>A saída renderizada é um <code>IRawImage</code> &mdash; um buffer de pixels BGRA32 (8 bits por canal) com <code>Width</code>, <code>Height</code> e um array <code>Pixels</code>. Você pode então salvar esses dados de pixel em JPG usando qualquer biblioteca de imagens .NET, como <code>System.Drawing</code>, <code>SkiaSharp</code> ou <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderizar DICOM para Dados de Pixel em C#">}}

<p>Carregue um arquivo DICOM e renderize um frame para um <code>IRawImage</code> contendo dados de pixel BGRA32:</p>

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

<p>O método <code>RenderImage</code> aplica automaticamente o pipeline completo de tons de cinza usando os valores de window/level e parâmetros LUT armazenados no arquivo DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Salvar Imagem Renderizada como JPG">}}

<p>O <code>IRawImage</code> fornece dados de pixel BGRA32 brutos que podem ser salvos como JPG usando qualquer biblioteca de imagens .NET. Aqui está um exemplo usando <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Salvar frame DICOM renderizado como JPG - C#</h3>
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

// Save as JPG
bitmap.Save("ct_scan.jpg", ImageFormat.Jpeg);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Window/Level Personalizado (Windowing)">}}

<p>Window/level (também chamado de windowing ou W/L) é o parâmetro mais importante para a renderização de imagens DICOM. Ele controla qual faixa de valores de pixel é mapeada para a faixa de tons de cinza visível. O <strong>Window Center</strong> define o ponto médio e o <strong>Window Width</strong> define a faixa de valores exibidos:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; alto contraste, menos níveis de cinza exibidos (útil para tecido mole).</li>
<li><strong>Wide window</strong> &mdash; contraste reduzido, mais níveis de cinza exibidos (útil para osso ou pulmão).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Renderizar com window/level personalizado - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window: center=40, width=400
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window: center=400, width=1800
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Lung window: center=-600, width=1500
var lung = new GrayscaleRenderOptions
{
    WindowCenter = -600,
    WindowWidth = 1500
};
IRawImage lungImage = dicomFile.RenderImage(lung, 0);</code></pre>
</div>

<p>Predefinições comuns de janelas CT:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tipo de Tecido</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Tecido Mole</td><td>40</td><td>400</td></tr>
<tr><td>Pulmão</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Osso</td><td>400</td><td>1800</td></tr>
<tr><td>Cérebro</td><td>40</td><td>80</td></tr>
<tr><td>Fígado</td><td>60</td><td>150</td></tr>
<tr><td>Mediastino</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opções de Renderização em Tons de Cinza">}}

<p>A classe <code>GrayscaleRenderOptions</code> fornece controle total sobre o pipeline de renderização em tons de cinza:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Propriedade</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Centro da faixa de valores exibidos (ponto médio horizontal do VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Largura da faixa de valores exibidos (controla o contraste)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Inclinação de reescalonamento do Modality LUT &mdash; multiplicada pelos valores de pixel armazenados</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Intercepto de reescalonamento do Modality LUT &mdash; adicionado após a multiplicação da inclinação</td></tr>
<tr><td><code>Invert</code></td><td>Inverte a imagem em tons de cinza (aplica forma INVERSE do Presentation LUT)</td></tr>
<tr><td><code>BitDepth</code></td><td>Informação de profundidade de bits: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Renderização em tons de cinza invertida - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray.dcm");

// Render with inverted grayscale (white-on-black becomes black-on-white)
var options = new GrayscaleRenderOptions
{
    WindowCenter = 2048,
    WindowWidth = 4096,
    Invert = true
};

IRawImage image = dicomFile.RenderImage(options, 0);</code></pre>
</div>

<p>Use <code>RenderOptionsFactory.Create(pixelData)</code> para preencher automaticamente as opções de renderização a partir dos atributos de dados de pixel do conjunto de dados DICOM, incluindo profundidade de bits, parâmetros de reescalonamento, window/level e configurações de inversão.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderização de Overlay">}}

<p>Imagens DICOM podem conter planos de overlay &mdash; gráficos ou anotações de texto incorporados à imagem. A classe <code>RenderOptions</code> controla se os overlays são renderizados e em que cor eles aparecem:</p>

<div class="codeblock" id="code">
 <h3>Renderizar com controle de overlay - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible in white (default behavior)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions
{
    ShowOverlays = false
};
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

<p>DICOM suporta dois tipos de overlay: overlays de <strong>Graphics</strong> (anotações, medições) e overlays de <strong>Region of Interest</strong> (ROI). Ambos os tipos são controlados pela configuração <code>ShowOverlays</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderização DICOM Multi‑Frame">}}

<p>Muitos arquivos DICOM de CT, MRI e ultrassom contêm múltiplos frames (fatias). Você pode verificar o número de frames e renderizar cada um individualmente:</p>

<div class="codeblock" id="code">
 <h3>Renderizar todos os frames de DICOM multi‑frame - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;
Console.WriteLine($"Total frames: {totalFrames}");

// Render all frames to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as JPG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tipos de Transformação VOI LUT">}}

<p>O padrão DICOM define várias funções de transformação VOI LUT que controlam como o mapeamento window/level é aplicado. Aspose.Medical for .NET suporta todos os tipos padrão:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
<th>Caso de Uso</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Mapeamento linear padrão com deslocamento a partir do centro</td><td>Mais comum, usado para imagens gerais de CT/MR</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Mapeamento linear sem deslocamento a partir do centro</td><td>Quando é necessário mapeamento preciso de valores de pixel</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>Transformação em curva S para transições de contraste mais suaves</td><td>Visualização de tecido mole, mamografia</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>LUT personalizada definida como tabela de consulta no conjunto de dados DICOM</td><td>Curvas de exibição específicas de fornecedor ou de modalidade</td></tr>
</tbody>
</table>

<p>O mecanismo de renderização seleciona automaticamente a transformação VOI LUT apropriada com base nos atributos do conjunto de dados DICOM. Ao usar <code>GrayscaleRenderOptions</code> personalizados, a transformação Linear é aplicada por padrão.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Modalidades Suportadas">}}

<p>Aspose.Medical for .NET suporta renderização de imagens DICOM de todas as modalidades de imagem médica comuns:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modalidade</th>
<th>Descrição</th>
<th>Formato Típico</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Tomografia Computadorizada</td><td>Monocromático, multi‑frame, 12&ndash;16 bits</td></tr>
<tr><td><strong>MR</strong></td><td>Ressonância Magnética</td><td>Monocromático, multi‑frame, 12&ndash;16 bits</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Radiografia Computadorizada / Digital</td><td>Monocromático, frame único, 10&ndash;16 bits</td></tr>
<tr><td><strong>US</strong></td><td>Ultrassom</td><td>Monocromático ou RGB, multi‑frame</td></tr>
<tr><td><strong>MG</strong></td><td>Mamografia</td><td>Monocromático, alta resolução, 12&ndash;16 bits</td></tr>
<tr><td><strong>XA</strong></td><td>An angiografia de raios‑X</td><td>Monocromático, multi‑frame</td></tr>
<tr><td><strong>NM</strong></td><td>Medicina Nuclear</td><td>Monocromático, multi‑frame</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Tomografia por Emissão de Pósitrons)</td><td>Monocromático, multi‑frame</td></tr>
</tbody>
</table>

<p>São suportadas interpretações fotométricas tanto em tons de cinza (MONOCHROME1/MONOCHROME2) quanto em cores (RGB, YBR_FULL, PALETTE COLOR). O mecanismo de renderização lida automaticamente com a conversão de espaço de cores apropriada.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizagem" tabId="resources" >}}
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
