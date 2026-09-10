---
title: Compresión DICOM JPEG 2000 en C# .NET | Aspose.Medical
weight: 2000
description: Lea, escriba y transcode archivos DICOM con compresión JPEG 2000 en C# .NET. Soporte para imágenes de 8 bits y 16 bits, modos sin pérdida y con pérdida, datos multi‑componente con la API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Compatibilidad DICOM JPEG 2000 en .NET C#" h2="Lea, escriba y transcode archivos DICOM con compresión JPEG 2000. Modos sin pérdida y con pérdida, datos de píxeles de 8 bits y 16 bits, imágenes multi‑componente — todo en .NET puro." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 en imágenes médicas">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) es el estándar de compresión basado en wavelets más utilizado en imágenes médicas. A diferencia del JPEG tradicional, ofrece compresión sin pérdida y con pérdida en un único códec, decodificación progresiva para acceso a regiones de interés y relaciones de compresión superiores &mdash; lo que lo hace ideal para archivar estudios extensos y transmitir imágenes en redes con limitaciones.</p>

<p><strong>Aspose.Medical para .NET</strong> ofrece una implementación pura en C# del códec JPEG 2000 sin dependencias nativas. La biblioteca puede leer, renderizar y transcodificar archivos DICOM comprimidos con cualquiera de los cuatro sintaxis de transferencia estándar JPEG 2000.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sintaxis de transferencia JPEG 2000 compatibles">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Sintaxis de transferencia</th>
<th>UID</th>
<th>Modo</th>
<th>Lectura</th>
<th>Escritura</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 solo sin pérdida</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Sin pérdida</td><td>8 bits y 16 bits</td><td>8 bits</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Con pérdida o sin pérdida</td><td>8 bits y 16 bits</td><td>8 bits</td></tr>
<tr><td>JPEG 2000 Parte 2 Multi‑componente solo sin pérdida</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Sin pérdida</td><td>8 bits y 16 bits</td><td>8 bits</td></tr>
<tr><td>JPEG 2000 Parte 2 Multi‑componente</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Con pérdida o sin pérdida</td><td>8 bits y 16 bits</td><td>8 bits</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Datos de píxel de 8 bits y 16 bits">}}

<p>Las imágenes médicas a menudo utilizan 16 bits por muestra para capturar todo el rango dinámico de modalidades como TC (típicamente 12 bits almacenados en 16 bits) y RM. Aspose.Medical maneja ambas profundidades de bits para JPEG 2000:</p>

<ul>
<li><strong>Lectura (descompresión)</strong>: Soporte completo para archivos DICOM comprimidos JPEG 2000 de 8 bits y 16 bits. La biblioteca decodifica correctamente los datos de píxel sin importar los valores originales de Bits Allocated, Bits Stored y High Bit.</li>
<li><strong>Escritura (compresión)</strong>: Actualmente soporta imágenes de 8 bits. El soporte de escritura de 16 bits está previsto para una próxima versión.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Leer e inspeccionar DICOM comprimido JPEG 2000 - C#</h3>
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

<p>Descomprima archivos JPEG 2000 a una sintaxis de transferencia sin compresión para procesamiento, análisis o compatibilidad con sistemas que no soportan JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Descomprimir JPEG 2000 a sin compresión - C#</h3>
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

<p>Los archivos DICOM comprimidos JPEG 2000 pueden renderizarse a datos de píxel para visualización o exportación, al igual que cualquier otra sintaxis de transferencia:</p>

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

{{< blocks/products/pf/feature-page-section h2="Sin pérdida vs Con pérdida JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Aspecto</th>
<th>JPEG 2000 sin pérdida</th>
<th>JPEG 2000 con pérdida</th>
</tr>
</thead>
<tbody>
<tr><td>Sintaxis de transferencia</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Calidad de imagen</td><td>Pixel‑perfect &mdash; idéntica al original</td><td>Visualmente similar, algunos datos se pierden permanentemente</td></tr>
<tr><td>Relación de compresión</td><td>Typicamente 2:1 a 3:1</td><td>Typicamente 10:1 a 30:1 o más</td></tr>
<tr><td>Mejor para</td><td>Archivado diagnóstico, registros legales, lectura primaria</td><td>Revisión preliminar, telemedicina, transmisión en red</td></tr>
<tr><td>Seguridad de ida y vuelta</td><td>Sí</td><td>No &mdash; volver a codificar degrade aún más la calidad</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Parte 2 Multi‑componente">}}

<p>JPEG 2000 Parte 2 (ISO/IEC 15444-2) amplía el códec estándar con capacidades de transformada multi‑componente. Se utiliza para imágenes médicas en color y modalidades que generan datos multicanal. Aspose.Medical soporta ambas sintaxis de transferencia de la Parte 2:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; compresión sin pérdida con decorrelación inter‑componente para una compresión óptima de datos multicanal.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; compresión con pérdida o sin pérdida con transformadas multi‑componente.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 de alta velocidad (HTJ2K) — Próximamente">}}

<p>HTJ2K (ISO/IEC 15444-15) es una extensión de siguiente generación de JPEG 2000 diseñada para velocidades de codificación y decodificación mucho más rápidas, manteniendo la misma eficiencia de compresión. Se espera que se convierta en el códec preferido para flujos de trabajo de imágenes médicas en tiempo real.</p>

<p>Aspose.Medical añadirá soporte para HTJ2K en una futura versión, cubriendo tres sintaxis de transferencia:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; Sólo sin pérdida</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; Sin pérdida con orden de progresión RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; Con pérdida o sin pérdida</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Código fuente" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referencias de API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Soporte de producto" tabId="support" >}}
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
