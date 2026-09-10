---
title: Conversión de Sintaxis de Transferencia DICOM en C# .NET | Aspose.Medical
weight: 16000
description: Transcodifique archivos DICOM entre sintaxis de transferencia en C# .NET. Soporte para JPEG, JPEG 2000, JPEG-LS, RLE y formatos sin comprimir con la API de Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Conversión de Sintaxis de Transferencia DICOM en .NET C#" h2="Transcodifique archivos DICOM entre sintaxis de transferencia sin comprimir, JPEG, JPEG 2000, JPEG-LS y RLE. Biblioteca .NET pura sin dependencias nativas." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="¿Qué es la Sintaxis de Transferencia?">}}

<p>Una <strong>Transfer Syntax</strong> define cómo se codifican los datos DICOM para almacenamiento y transmisión. Especifica tres aspectos clave: el orden de bytes (endianness), si las Representaciones de Valor son explícitas o implícitas, y el algoritmo de compresión aplicado a los datos de píxeles. Cada archivo DICOM declara su sintaxis de transferencia en el encabezado de Información de Metadatos del archivo.</p>

<p>Diferentes dispositivos médicos, servidores PACS y aplicaciones de visualización soportan distintos conjuntos de sintaxis de transferencia. <strong>Aspose.Medical for .NET</strong> ofrece el método <code>Transcode</code> para convertir entre sintaxis de transferencia, facilitando la interoperabilidad, la optimización del almacenamiento y la compatibilidad con herramientas de procesamiento &mdash; todo en una biblioteca .NET pura sin dependencias nativas.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transcodificar un archivo DICOM en C#">}}

<p>El método <code>DicomFile.Transcode</code> convierte un archivo DICOM de su sintaxis de transferencia actual a cualquier sintaxis de destino admitida. El método devuelve una nueva instancia de <code>DicomFile</code> &mdash; el original permanece sin cambios:</p>

<div class="codeblock" id="code">
 <h3>Transcodificación DICOM básica - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>También puede transcodificar directamente a nivel de <code>Dataset</code>:</p>

<div class="codeblock" id="code">
 <h3>Transcodificar un Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sintaxis de Transferencia Compatibles">}}

<p>La tabla siguiente enumera todas las sintaxis de transferencia de datos de imagen DICOM estándar y su estado de compatibilidad actual en Aspose.Medical for .NET. Todos los códecs compatibles están implementados en C# puro y son totalmente independientes de la plataforma.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Sintaxis de Transferencia</th>
<th>UID</th>
<th>Tipo</th>
<th>Estado</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Sin comprimir</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Sin comprimir</td><td>Compatible</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Sin comprimir</td><td>Compatible</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Sin comprimir</td><td>Compatible</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Compatible</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Con pérdida, 8‑bits</td><td>Compatible</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Con pérdida, 12‑bits</td><td>No compatible</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Sin pérdida</td><td>Compatible (solo 8‑bits)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Sin pérdida</td><td>Compatible (solo 8‑bits)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Sin pérdida</td><td>Compatible</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Casi sin pérdida</td><td>Compatible</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Sin pérdida</td><td>Compatible (lectura 8/16 bits, escritura 8 bits)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Con pérdida o sin pérdida</td><td>Compatible (lectura 8/16 bits, escritura 8 bits)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Sin pérdida</td><td>Compatible (lectura 8/16 bits, escritura 8 bits)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Con pérdida o sin pérdida</td><td>Compatible (lectura 8/16 bits, escritura 8 bits)</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Sin pérdida</td><td>Compatible</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Sin pérdida</td><td>Próximamente</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Sin pérdida</td><td>Próximamente</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Con pérdida o sin pérdida</td><td>Próximamente</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Sin pérdida</td><td>Próximamente</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Sin pérdida</td><td>Próximamente</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Con pérdida o sin pérdida</td><td>Próximamente</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Escenarios Comunes de Transcodificación">}}

<p>Diferentes flujos de trabajo requieren distintas estrategias de transcodificación. Aquí están los escenarios más comunes:</p>

<div class="codeblock" id="code">
 <h3>Descomprimir para procesamiento - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Comprimir para almacenamiento de archivo - C#</h3>
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
 <h3>Comprimir para transmisión en red - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Inspeccionar Propiedades de la Sintaxis de Transferencia">}}

<p>La clase <code>TransferSyntax</code> expone propiedades que describen las características de codificación. Úselas para inspeccionar la sintaxis de transferencia actual de un archivo o para seleccionar una sintaxis de destino apropiada:</p>

<div class="codeblock" id="code">
 <h3>Leer propiedades de la sintaxis de transferencia - C#</h3>
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
<th>Propiedad</th>
<th>Tipo</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>El identificador único de la sintaxis de transferencia</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Indica si las Representaciones de Valor están codificadas explícitamente</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Indica si el orden de bytes es little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Indica si los datos de píxeles están encapsulados (comprimidos)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Indica si el método de compresión es con pérdida</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Indica si la sintaxis usa compresión deflate</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Indica si la sintaxis de transferencia está retirada por el estándar DICOM</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>El identificador estándar ISO del método de compresión con pérdida</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Compresión con Pérdida vs Sin Pérdida">}}

<p>Comprender la diferencia entre compresión con pérdida y sin pérdida es fundamental al transcodificar archivos DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Aspecto</th>
<th>Sin pérdida</th>
<th>Con pérdida</th>
</tr>
</thead>
<tbody>
<tr><td>Calidad de imagen</td><td>Pixel-perfect &mdash; datos originales totalmente preservados</td><td>Algunos datos perdidos permanentemente para lograr un tamaño menor</td></tr>
<tr><td>Relación de compresión</td><td>Typical 2:1 a 3:1</td><td>Typical 10:1 a 30:1 o más</td></tr>
<tr><td>Seguridad de ida y vuelta</td><td>Sí &mdash; descomprimir y obtener píxeles idénticos</td><td>No &mdash; cada re‑codificación con pérdida degrada más la calidad</td></tr>
<tr><td>Caso de uso</td><td>Archivado, diagnóstico, registros legales</td><td>Revisión preliminar, telemedicina, transmisión en red</td></tr>
<tr><td>Códecs compatibles</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>Importante:</strong> Transcodificar de un archivo comprimido con pérdida a una sintaxis sin pérdida no restaura los datos perdidos. La degradación de calidad de la compresión original con pérdida es permanente.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Código Fuente" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referencias API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Soporte del Producto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Soporte Gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Soporte de Pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="¿Por qué Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Listado de Clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Historias de Éxito" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
