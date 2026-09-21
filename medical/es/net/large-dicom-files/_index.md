---
title: Trabaje con archivos DICOM grandes en C# .NET | Aspose.Medical
weight: 11500

description: Abra estudios multicuadro y Whole Slide Images en C# sin cargarlos en la memoria. Lea metadatos sin datos de píxeles, difiera elementos grandes y mueva archivos a través de streams y pipes.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Archivos DICOM grandes en .NET C#" h2="Lea los metadatos de un estudio multicuadro sin los píxeles, difiera los elementos grandes hasta que algo los solicite y mueva archivos completos a través de streams y pipes." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="El archivo es grande, la pregunta suele ser pequeña">}}

<p>Una imagen de diapositiva completa, una serie de TC larga o un volumen OCT son cientos de megabytes, y la mayor parte son datos de píxeles. El trabajo que realmente realiza una aplicación suele ser mucho menor: enumerar lo que hay en una carpeta, verificar un identificador de paciente, contar los cuadros, decidir dónde debe ir un estudio. Cargar cada byte para responder eso es lo que convierte una tarea sencilla en un problema de memoria.</p>

<p><strong>Aspose.Medical para .NET</strong> permite al llamador decidir cuánto se lee de un archivo. La opción es un argumento en <code>DicomFile.Open</code>, y se aplica por igual a archivos, streams y pipes.</p>

<p>Medido en un estudio de 14 MB con 128 cuadros de nuestro conjunto de pruebas, en la misma máquina y el mismo archivo:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Estrategia de lectura</th>
<th>Tiempo de apertura</th>
<th>Memoria asignada</th>
</tr>
</thead>
<tbody>
<tr><td>Todo, predeterminado</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Elementos grandes omitidos</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Elementos grandes diferidos</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>La brecha aumenta con el tamaño del archivo. Una carpeta de 10 000 estudios es el caso en que deja de ser una micro‑optimización.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lea los metadatos, deje los píxeles sin tocar">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> deja fuera de la lectura cada elemento que supera un umbral de tamaño. El conjunto de datos que regresa contiene solo las etiquetas que necesita un índice o un enrutador.</p>

<div class="codeblock" id="code">
 <h3>Leer un estudio sin sus datos de píxel - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>El umbral predeterminado es 64 kB y acepta un valor en kilobytes, por lo que un flujo de trabajo que considera 8 kB como grande puede indicarlo.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Diferir en lugar de omitir">}}

<p>Cuando los píxeles pueden ser necesarios, pero probablemente después y probablemente no todos, <code>ReadLargeOnDemand</code> es la otra mitad del par. Abrir el archivo tiene el mismo costo que omitir, y un elemento grande se lee en el momento en que el código lo accede.</p>

<div class="codeblock" id="code">
 <h3>Cargar un cuadro solo cuando se use - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>La lectura diferida es una característica licenciada; las demás estrategias también funcionan en evaluación.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Indexe una carpeta sin tocar los píxeles">}}

<p>La misma estrategia se aplica a un stream, que es lo que parece un escaneo de archivo o un almacén de objetos en la nube desde el código.</p>

<div class="codeblock" id="code">
 <h3>Escanear un archivo - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streams y pipes, entrada y salida">}}

<p>La lectura y escritura aceptan streams, y los puntos de entrada asíncronos también aceptan tipos <code>System.IO.Pipelines</code>. Un estudio puede viajar de una respuesta de red a almacenamiento sin que el proceso mantenga nunca el archivo completo como una sola matriz.</p>

<div class="codeblock" id="code">
 <h3>Leer y escribir a través de streams - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>La misma idea cubre las representaciones textuales: un documento con muchos conjuntos de datos se lee un conjunto a la vez en las páginas <a href="/medical/net/json-to-dicom/">JSON a DICOM</a> y <a href="/medical/net/xml-to-dicom/">XML a DICOM</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Cuadro a cuadro">}}

<p>Los datos multicuadro se abordan por cuadro, por lo que una serie de 500 cuadros procesa un cuadro a la vez en lugar del elemento completo de datos de píxeles.</p>

<div class="codeblock" id="code">
 <h3>Recorrer los cuadros - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Donde esto decide el diseño">}}

<ul>
<li>Indexación y migración de archivos: millones de archivos, y solo el encabezado importa hasta que algo se mueve.</li>
<li>Enrutadores y nodos de almacenamiento: aceptan un estudio, leen lo necesario para enrutarlo, pasan los bytes.</li>
<li>Tuberías de IA: construya el manifiesto a partir de los metadatos, luego extraiga los cuadros del subconjunto que realmente se entrena.</li>
<li>Contenedores con límite de memoria: el conjunto de trabajo sigue la estrategia, no el tamaño del archivo.</li>
<li>Datos de Whole Slide y OCT: archivos donde leer todo no es una opción.</li>
</ul>

<p>La <a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">guía de gestión de memoria</a> explica las estrategias en detalle, y <a href="/medical/net/dicom-networking/">la red DICOM</a> muestra los mismos datos llegando a través de DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guía del desarrollador" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
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
