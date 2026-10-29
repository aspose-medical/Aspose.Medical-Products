---
title: Cliente DICOMweb en C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Buscar, descargar y cargar estudios DICOM a través de HTTP desde .NET. Un cliente DICOMweb en C# puro para QIDO-RS, WADO-RS y STOW-RS que funciona con PACS, VNA y servicios de imágenes en la nube.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Cliente DICOMweb para .NET C#" h2="Buscar, descargar y cargar estudios en cualquier servidor DICOMweb con QIDO-RS, WADO-RS y STOW-RS, en C# administrado sin necesidad de instalar nada en la máquina." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM sobre HTTP, desde su propio código">}}

<p>DICOMweb es la forma HTTP de DICOM. En lugar de una asociación DIMSE en el puerto 104, su aplicación envía solicitudes HTTPS ordinarias: QIDO-RS para buscar, WADO-RS para descargar, STOW-RS para cargar. Los servicios de imágenes en la nube, como el servicio DICOM de Azure Health Data Services y la API Google Cloud Healthcare, funcionan de esta manera, al igual que archivos como Orthanc y dcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> incluye un cliente DICOMweb desde la versión 26.10. <code>DicomWebClient</code> envuelve cada transacción estándar en un método tipado, y los resultados llegan como los mismos objetos <code>Dataset</code> y <code>DicomFile</code> que el resto de la biblioteca lee y escribe. El servidor del otro lado puede ser cualquier implementación DICOMweb; no necesita estar construido con Aspose.Medical.</p>

<p>El cliente es C# administrado sobre <code>HttpClient</code>. No hay un kit de herramientas nativo ni servicio proxy, por lo que el mismo código se ejecuta en Windows, Linux y en un contenedor.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Conectar a un servidor DICOMweb">}}

<p>Cree el cliente a partir de un <code>HttpClient</code> y la URL raíz del servicio del servidor. La autenticación, la configuración TLS, los proxies y el agrupamiento de conexiones permanecen en el <code>HttpClient</code>, donde su aplicación ya los configura. El cliente no es propietario del <code>HttpClient</code> y no lo elimina.</p>

<div class="codeblock" id="code">
 <h3>Crear un cliente DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Si una implementación sirve WADO-URI u otro servicio desde una URL diferente, <code>ServiceBaseUris</code> sobrescribe la raíz solo para ese servicio.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Buscar estudios con QIDO-RS">}}

<p>Una búsqueda toma parámetros de coincidencia con la palabra clave DICOM como nombre, los campos a devolver y los controles de paginación. Las coincidencias llegan como un flujo de datasets, leídos uno a la vez mientras la respuesta todavía se está descargando.</p>

<div class="codeblock" id="code">
 <h3>Encontrar los estudios de un paciente - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> y <code>SearchInstancesAsync</code> buscan un nivel abajo, en todo el archivo o dentro de un estudio o serie. Las mismas opciones también establecen coincidencia difusa, coincidencia de valores vacíos y el desplazamiento de resultados para la paginación.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Descargar un estudio con WADO-RS">}}

<p>Un estudio puede contener miles de instancias. <code>RetrieveStudyAsync</code> las devuelve como un flujo, de modo que cada archivo se analiza a medida que su parte de la respuesta llega y puede guardarse antes de que se lea la siguiente. El uso de memoria no crece con el tamaño del estudio.</p>

<div class="codeblock" id="code">
 <h3>Guardar cada instancia de un estudio - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> y <code>RetrieveInstanceAsync</code> descargan una serie o una instancia de la misma manera.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Obtener una miniatura o una imagen renderizada">}}

<p>Un visor o una pantalla de lista de trabajo a menudo necesita una imagen, no un archivo DICOM. El servidor la renderiza y envía un JPEG o PNG, por lo que el cliente no descarga ni decodifica los datos de píxeles originales.</p>

<div class="codeblock" id="code">
 <h3>Guardar una miniatura JPEG de una instancia - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p><code>RetrieveRenderedInstanceAsync</code> y <code>RetrieveRenderedFramesAsync</code> devuelven una imagen a tamaño completo en su lugar. <code>DicomWebRenderOptions</code> pasa los controles de renderizado estándar al servidor: calidad de imagen, tamaño de la ventana gráfica, ventana y perfil ICC.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Cargar instancias con STOW-RS">}}

<p>STOW-RS envía cualquier número de instancias en una sola solicitud. El servidor responde con un informe que enumera, instancia por instancia, lo que almacenó y lo que rechazó.</p>

<div class="codeblock" id="code">
 <h3>Cargar una carpeta de archivos DICOM - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>Algunos servidores responden con un error HTTP, como 409, cuando una o más instancias fallan. Entonces la llamada lanza <code>DicomWebClientException</code>, como muestra la siguiente sección, y el mismo informe está en el cuerpo de la respuesta de la excepción.</p>

<p><code>StoreStudyAsync</code> carga en un estudio con nombre. Ambos métodos también aceptan un <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, de modo que los archivos pueden producirse mientras se envía la solicitud.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Manejar errores del servidor">}}

<p>Cada operación tipada verifica el estado HTTP antes de devolver. Cuando el servidor rechaza una solicitud, la llamada lanza <code>DicomWebClientException</code>, que lleva el código de estado, los encabezados y el inicio del cuerpo de la respuesta para el registro.</p>

<div class="codeblock" id="code">
 <h3>Informar una solicitud fallida - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>Los métodos que devuelven una respuesta cruda, como la miniatura anterior, le dejan la verificación de estado a través de <code>IsSuccessStatusCode</code> o <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Qué más cubre el cliente">}}

<p>Buscar, descargar y cargar son las llamadas comunes. El cliente también tiene un grupo de operaciones para cada uno de los demás servicios DICOMweb:</p>

<ul>
<li>Datos en bloque, datos de píxeles y fotogramas seleccionados de una instancia, por número de fotograma basado en 1, tal como los envía el servidor.</li>
<li>WADO-URI, la recuperación de una sola URL más antigua que muchos visores y archivos aún utilizan, para archivos DICOM e imágenes renderizadas.</li>
<li>Listas de trabajo UPS-RS: crear, buscar, actualizar y reclamar elementos de trabajo, cambiar su estado, solicitar cancelación y suscribirse a eventos que llegan a través de un WebSocket.</li>
<li>La lista de trabajo de modalidad (MWL-RS), los pasos de procedimiento realizados por la modalidad (MPPS-RS) y el compromiso de almacenamiento.</li>
<li>Instancias no relacionadas con pacientes, como paletas de color y protocolos de colgado, y el documento de capacidades del servidor.</li>
<li><code>SendAsync</code> para cualquier transacción estándar con sus propios valores de ruta, parámetros de consulta, encabezados y cuerpo, cuando un método tipado no es adecuado.</li>
</ul>

<p>La <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">guía del cliente DICOMweb</a> documenta cada operación y opción, y la <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">referencia de operaciones del cliente</a> enumera cada método.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Una biblioteca desde la respuesta HTTP hasta los datos de píxel">}}

<p>Un archivo descargado con el cliente es un <code>DicomFile</code> regular. Puede anonimizarse, transcodificarse, renderizarse o serializarse en el mismo proceso, sin paso de conversión y sin una segunda biblioteca en la cadena.</p>

<p>Páginas relacionadas: <a href="/medical/net/dicomweb-server/">servidor DICOMweb</a> para el otro lado del protocolo en su propio servicio ASP.NET Core, <a href="/medical/net/dicom-networking/">red DICOM</a> para archivos que utilizan DIMSE en lugar de HTTP, <a href="/medical/net/anonymization/">anonimización</a> antes de que un estudio salga del hospital, <a href="/medical/net/dicom-transfer-syntax-conversion/">conversión de sintaxis de transferencia</a> para lo que almacena el servidor, y <a href="/medical/net/dicom-to-json/">DICOM a JSON</a> para el formato que usan los resultados de búsqueda QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guía del desarrollador" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="Referencias de API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Soporte del producto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Soporte gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Soporte pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="¿Por qué Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Casos de éxito" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
