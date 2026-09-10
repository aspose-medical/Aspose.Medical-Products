---
title: Compresión DICOM JPEG 2000 en C# .NET | Aspose.Medical
weight: 2000
description: Lea, escriba y transcode archivos DICOM con compresión JPEG 2000 en C# .NET. Compatibilidad con imágenes en color de 8 bits y monocromáticas de 16 bits, modos sin pérdida y con pérdida, además de HTJ2K con la API de Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Compatibilidad DICOM JPEG 2000 en .NET C#" h2="Lea, escriba y transcode archivos DICOM con compresión JPEG 2000. Modos sin pérdida y con pérdida, datos de píxeles en color de 8 bits y monocromáticos de 16 bits, HTJ2K incluido, todo en .NET puro." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 en Imágenes Médicas">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) es el estándar de compresión basado en wavelet más utilizado en imágenes médicas. A diferencia del JPEG tradicional, ofrece compresión sin pérdida y con pérdida en un único códec, decodificación progresiva para acceso a regiones de interés y relaciones de compresión superiores &mdash; lo que lo hace ideal para archivar estudios voluminosos y transmitir imágenes en redes con ancho de banda limitado.</p>

<p><strong>Aspose.Medical for .NET</strong> proporciona una implementación pura en C# del códec JPEG 2000 sin dependencias nativas. La biblioteca puede leer, renderizar y transcodificar archivos DICOM comprimidos con cualquiera de las cuatro sintaxis de transferencia estándar JPEG 2000.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sintaxis de Transferencia JPEG 2000 Compatibles">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Sintaxis de Transferencia</th>
<th>UID</th>
<th>Modo</th>
<th>Lectura</th>
<th>Escritura</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 Solo sin pérdida</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Sin pérdida</td><td>RGB de 8 bits, monocromo de 16 bits</td><td>Monocromo de 16 bits, RGB de 8 bits</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Con pérdida o sin pérdida</td><td>RGB de 8 bits, monocromo de 16 bits</td><td>Monocromo de 16 bits, RGB de 8 bits</td></tr>
<tr><td>JPEG 2000 Parte 2 Multi-componente Solo sin pérdida</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Sin pérdida</td><td>No compatible</td><td>No compatible</td></tr>
<tr><td>JPEG 2000 Parte 2 Multi-componente</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Con pérdida o sin pérdida</td><td>No compatible</td><td>No compatible</td></tr>
<tr><td>HTJ2K Solo sin pérdida</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Sin pérdida</td><td>Monocromo y color</td><td>Monocromo y color</td></tr>
<tr><td>HTJ2K con opciones RPCL Solo sin pérdida</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Sin pérdida</td><td>Monocromo y color</td><td>Monocromo y color</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Con pérdida o sin pérdida</td><td>Monocromo y color</td><td>Monocromo y color</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Datos de píxeles de 8 bits y 16 bits">}}

<p>Las imágenes médicas a menudo utilizan 16 bits por muestra para capturar todo el rango dinámico de modalidades como TC (típicamente 12 bits almacenados en 16 bits) y RM. Aspose.Medical maneja ambas profundidades de bits para JPEG 2000:</p>

<ul>
<li><strong>Lectura (descompresión)</strong>: archivos monocromáticos de 16 bits (CT, RM, Rayos X) y archivos de color de tres componentes de 8 bits (RGB, YBR_RCT, YBR_ICT). Paleta, CMYK, perfil ICC y flujos de código de color subsampleado se rechazan con una excepción clara en lugar de una imagen incorrecta de forma silenciosa.</li>
<li><strong>Escritura (compresión)</strong>: imágenes monocromáticas de 16 bits y RGB de 8 bits. La codificación monocromática de 8 bits y la de color de 16 bits no están disponibles; use HTJ2K o JPEG XL para esos casos, ambos aceptan monocromo y color en cualquier profundidad de bits.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Leer e inspeccionar DICOM comprimido con JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Transcodificar a JPEG 2000">}}

<p>Utilice el método <code>Transcode</code> para comprimir cualquier archivo DICOM a JPEG 2000 o para convertir entre modos JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Comprimir DICOM a JPEG 2000 sin pérdida - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Comprimir DICOM a JPEG 2000 con pérdida - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Descomprimir archivos DICOM JPEG 2000">}}

<p>Descomprima archivos JPEG 2000 a una sintaxis de transferencia sin comprimir para procesamiento, análisis o compatibilidad con sistemas que no admiten JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Descomprimir JPEG 2000 a sin comprimir - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>También puede descomprimir y transcodificar a otros formatos de compresión en un solo paso:</p>

<div class="codeblock" id="code">
 <h3>Transcodificar entre formatos de compresión - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderizar imágenes DICOM JPEG 2000">}}

<p>Los archivos DICOM comprimidos con JPEG 2000 pueden renderizarse a datos de píxeles para visualización o exportación, al igual que cualquier otra sintaxis de transferencia:</p>

<div class="codeblock" id="code">
 <h3>Renderizar un cuadro comprimido JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Sin pérdida vs con pérdida JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Aspecto</th>
<th>JPEG 2000 sin pérdida</th>
<th>JPEG 2000 con pérdida</th>
</tr>
</thead>
<tbody>
<tr><td>Sintaxis de Transferencia</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Calidad de imagen</td><td>Pixel-perfect &mdash; idéntica al original</td><td>Visualmente similar, algunos datos se pierden permanentemente</td></tr>
<tr><td>Relación de compresión</td><td>Típicamente 2:1 a 3:1</td><td>Típicamente 10:1 a 30:1 o más</td></tr>
<tr><td>Mejor para</td><td>Archivado diagnóstico, registros legales, lectura primaria</td><td>Revisión preliminar, telemedicina, transmisión en red</td></tr>
<tr><td>Seguro en ida y vuelta</td><td>Sí</td><td>No &mdash; volver a codificar degrada más la calidad</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 de alto rendimiento (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) reemplaza el codificador aritmético lento de JPEG 2000 con un codificador de bloques más rápido. Mantiene la misma transformación wavelet, órdenes de progresión y calidad, y decodifica y codifica varias veces más rápido. Aspose.Medical implementa las tres sintaxis de transferencia DICOM HTJ2K en .NET puro, para imágenes monocromáticas y a color, y transcodifica entre HTJ2K y cualquier otra sintaxis compatible:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; solo sin pérdida</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; sin pérdida con orden de progresión RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; con pérdida o sin pérdida</li>
</ul>

<div class="codeblock" id="code">
 <h3>Transcodificar JPEG 2000 a HTJ2K y viceversa - C#</h3>
 <pre><code class="cs">// Transcode a JPEG 2000 file to HTJ2K, and back to classic JPEG 2000
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile htj2kFile = j2kFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2kFile.Save("htj2k_lossless.dcm");

// HTJ2K with RPCL progression order, lossless
DicomFile rpclFile = j2kFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
rpclFile.Save("htj2k_rpcl.dcm");

// HTJ2K lossy
DicomFile htj2kLossy = j2kFile.Transcode(TransferSyntax.HTJ2K);
htj2kLossy.Save("htj2k_lossy.dcm");

// Any HTJ2K file decodes back to an uncompressed transfer syntax
DicomFile uncompressed = htj2kFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decoded.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Código fuente" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referencias API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Soporte del producto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Soporte gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Soporte de pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="¿Por qué Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Casos de éxito" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
