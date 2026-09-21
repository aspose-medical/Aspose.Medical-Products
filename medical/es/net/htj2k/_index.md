---
title: HTJ2K en C# .NET - High-Throughput JPEG 2000 para DICOM | Aspose.Medical
weight: 10000

description: Comprima y lea imágenes DICOM en High-Throughput JPEG 2000 desde C#. HTJ2K sin pérdida, la variante RPCL y HTJ2K con pérdida, implementados en .NET administrado sin necesidad de un códec nativo para desplegar.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K en .NET C#" h2="High-Throughput JPEG 2000 para DICOM: la compresión que el estándar añadió para archivos rápidos y visualización en la nube, implementada en C# administrado sin nada nativo que instalar." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Qué cambia HTJ2K">}}

<p>High-Throughput JPEG 2000 mantiene la wavelet y la calidad de imagen de JPEG 2000 y reemplaza la parte que lo hacía lento. El codificador por bloques es nuevo, y la decodificación es un orden de magnitud más rápida, por lo que el estándar DICOM lo adoptó en tres sintaxis de transferencia y las plataformas de imágenes en la nube lo adoptaron.</p>

<p>Para un equipo .NET la pregunta práctica es diferente: quién puede realmente producir esos archivos. La mayoría de las bibliotecas acceden a HTJ2K a través de una compilación nativa de OpenJPH, lo que implica un binario por plataforma, un paso de compilación en el contenedor y una dependencia que la revisión de seguridad cuestionará. <strong>Aspose.Medical for .NET</strong> implementa el códec en código administrado dentro del mismo paquete que lee y escribe los archivos, por lo que HTJ2K funciona igual en Windows, en Linux y en un contenedor, sin necesidad de instalar nada.</p>

<p>Se admiten tres sintaxis de transferencia, y las tres permiten tanto lectura como escritura:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), High-Throughput JPEG 2000 sin pérdida.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), la variante sin pérdida con el orden de progresión RPCL.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Comprimir un estudio a HTJ2K">}}

<p>Una llamada mueve un archivo a la nueva sintaxis. El conjunto de datos, las etiquetas privadas y la información meta del archivo viajan con él.</p>

<div class="codeblock" id="code">
 <h3>Transcodificar un archivo DICOM a HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>En una imagen de 1714 × 1933 de 16 bits de nuestro propio conjunto de pruebas, el archivo pasa de 6,3 MB a 2,9 MB, y los píxeles se recuperan bit por bit. Los valores varían según la modalidad y la imagen, así que mida en sus propios datos, que es un bucle sobre los archivos que ya tiene.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sin pérdida significa sin pérdida">}}

<p>Los datos diagnósticos no toleran un códec que sea casi correcto. Transcodifique a HTJ2K sin pérdida y vuelva, y los datos de píxeles son idénticos a los bytes con los que comenzó, lo cual es una propiedad que puede validar en su propio conjunto de pruebas antes de aceptar recomprimir un archivo.</p>

<div class="codeblock" id="code">
 <h3>Volver a una sintaxis sin comprimir - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, la variante diseñada para visualización a través de una red">}}

<p>La sintaxis 1.2.840.10008.1.2.4.202 almacena el mismo flujo de codificación sin pérdida en el orden de progresión RPCL: resolución primero, luego posición, luego componente y luego capa. Un lector que solo toma el comienzo del flujo obtiene una imagen de baja resolución completa, que es lo que un visor necesita al abrir un estudio grande a través de un enlace que no controla.</p>

<div class="codeblock" id="code">
 <h3>Comprimir con el orden de progresión RPCL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Leer lo que un archivo envía">}}

<p>La otra mitad del trabajo es aceptar HTJ2K de sistemas que ya lo generan. Abra el archivo, verifique cómo está almacenado y trabaje con los datos de píxeles.</p>

<div class="codeblock" id="code">
 <h3>Leer un archivo HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Las imágenes multi‑frame se manejan cuadro por cuadro, por lo que una serie larga consume memoria por cuadro en lugar de por estudio.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dónde HTJ2K se gana su lugar">}}

<ul>
<li>Migración de archivo: recomprimir un estudio almacenado a HTJ2K sin pérdida, reducir la huella, mantener los datos diagnósticos intactos.</li>
<li>Nube y DICOMweb: la velocidad de decodificación es lo que hace que un visor del lado del navegador o del servidor se sienta inmediato en imágenes grandes.</li>
<li>Tuberías de IA: los conjuntos de entrenamiento se leen con mucho más frecuencia que se escriben, y el tiempo de decodificación es el costo que se repite.</li>
<li>Contenedores y serverless: el códec forma parte del ensamblado, por lo que una imagen no necesita una biblioteca nativa ni un compilador en la compilación.</li>
</ul>

<p>La biblioteca también incluye JPEG XL, la otra incorporación reciente al estándar, y los códecs más antiguos que un archivo probablemente contenga: JPEG, JPEG-LS, JPEG 2000 y RLE. La página de <a href="/medical/net/dicom-transfer-syntax-conversion/">conversión de sintaxis de transferencia</a> cubre todo el conjunto, y la página de <a href="/medical/net/jpeg2000/">JPEG 2000</a> cubre el códec del que surgió HTJ2K.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guía del desarrollador" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="Referencias de API" href="https://reference.aspose.com/medical/net/" >}}
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
