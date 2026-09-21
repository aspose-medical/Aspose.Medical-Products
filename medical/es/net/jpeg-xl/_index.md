---
title: JPEG XL para DICOM en C# .NET | Aspose.Medical
weight: 10500

description: Almacene imágenes DICOM en JPEG XL desde C#. JPEG XL sin pérdida que devuelve los píxeles bit a bit, en un único ensamblado administrado sin ningún códec nativo que desplegar.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL para DICOM en .NET C#" h2="La compresión más reciente en el estándar DICOM, con los archivos sin pérdida más pequeños que medimos, implementada en C# administrado y distribuida dentro de un solo ensamblado." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Por qué JPEG XL llegó a DICOM">}}

<p>Los archivos médicos crecen y nunca disminuyen. JPEG XL es el códec que el mundo de la imagen diseñó después de dos décadas de experiencia con JPEG y JPEG 2000, y DICOM lo añadió como una sintaxis de transferencia por la razón que a los equipos de almacenamiento les importa: para los mismos píxeles, el archivo es más pequeño.</p>

<p><strong>Aspose.Medical for .NET</strong> escribe y lee JPEG XL mediante un puerto C# de libjxl que reside dentro de la biblioteca. El paquete incluye un ensamblado, <code>Aspose.Medical.dll</code>, y no contiene binario nativo alguno, por lo que un códec tan nuevo no se convierte en un proyecto de despliegue: el mismo ensamblado se ejecuta en Windows, en Linux, en un agente de compilación y en un contenedor.</p>

<p>Dos sintaxis de transferencia transportan los píxeles:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), para datos diagnósticos que deben volver sin cambios.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), para los casos en que un archivo más pequeño es más importante que una copia exacta.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Comprima un estudio, conserve cada píxel">}}

<p>La transcodificación es una sola llamada, y el conjunto de datos alrededor de los píxeles viaja con ella.</p>

<div class="codeblock" id="code">
 <h3>Transcodifique un archivo DICOM a JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Lo medimos en una imagen de 1714 × 1933 de 16 bits de nuestro propio conjunto de pruebas: 6,3 MB sin comprimir pasa a 2,7 MB en JPEG XL sin pérdida, lo que es más pequeño que la misma imagen en HTJ2K sin pérdida. Sus propios resultados dependen de la modalidad, así que ejecute la comparación sobre una carpeta de sus archivos antes de decidir.</p>

<p>«Sin pérdida» es una expresión que debe tomarse literalmente aquí. Transcodifique a JPEG XL y regrese, y los datos de píxeles coinciden con los bytes con los que comenzó, de modo que un archivo puede volver a comprimirse sin discutir la calidad diagnóstica.</p>

<div class="codeblock" id="code">
 <h3>Volver a una sintaxis sin comprimir - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lea lo que ya está almacenado como JPEG XL">}}

<p>Un archivo que llega en JPEG XL se abre como cualquier otro. La sintaxis de transferencia indica lo que es, y los datos de píxeles están disponibles una vez que el cuadro se decodifica.</p>

<div class="codeblock" id="code">
 <h3>Abrir un archivo JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL o HTJ2K">}}

<p>Ambos son recientes, ambos son sin pérdida cuando se solicita sin pérdida, y la biblioteca escribe y lee ambos. Responden a preguntas diferentes.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Pregunta</th>
<th>Respuesta</th>
</tr>
</thead>
<tbody>
<tr><td>Cuál produjo el archivo más pequeño en nuestra prueba</td><td>JPEG XL sin pérdida, en unos pocos por ciento</td></tr>
<tr><td>Cuál está diseñado para visualización progresiva a través de una red</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, especialmente la variante RPCL</td></tr>
<tr><td>Cuál ingresó primero al estándar DICOM</td><td>HTJ2K, por lo que hoy más archivos lo aceptan</td></tr>
<tr><td>Cuál implica una dependencia nativa aquí</td><td>Ninguno, ambos son código administrado en un solo ensamblado</td></tr>
</tbody>
</table>

<p>La elección suele provenir del otro extremo del enlace: transcode a la sintaxis que acepta el archivo, y mantenga el resto de la canalización igual.</p>

<div class="codeblock" id="code">
 <h3>Deje que el archivo de destino decida - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dónde resulta ventajoso">}}

<ul>
<li>Archivos a largo plazo: los mismos estudios, menos terabytes y sin pérdida que justificar a un radiólogo.</li>
<li>Facturas de almacenamiento en la nube: el ahorro se repite cada mes, mientras que la transcodificación se ejecuta una sola vez.</li>
<li>Conjuntos de datos para investigación e IA: copias más pequeñas se mueven más rápido entre el almacenamiento y el entrenamiento.</li>
<li>Despliegue: un códec tan nuevo normalmente implica una compilación nativa por plataforma; aquí forma parte del ensamblado que ya referencia.</li>
</ul>

<p>La biblioteca también escribe los códecs que contiene un archivo existente: JPEG, JPEG-LS, JPEG 2000, HTJ2K y RLE. La página de <a href="/medical/net/dicom-transfer-syntax-conversion/">conversión de sintaxis de transferencia</a> cubre todo el conjunto, <a href="/medical/net/htj2k/">HTJ2K</a> tiene su propia página, y <a href="/medical/net/jpeg2000/">JPEG 2000</a> es de donde provienen ambos códecs nuevos.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guía del desarrollador" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="Referencias API" href="https://reference.aspose.com/medical/net/" >}}
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
