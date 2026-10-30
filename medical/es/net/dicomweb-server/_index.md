---
title: Servidor DICOMweb en C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Cree su propio servidor DICOMweb o PACS en C#. Aspose.Medical implementa QIDO-RS, WADO-RS, STOW-RS y los servicios de lista de trabajo en ASP.NET Core; usted conecta el almacenamiento.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Servidor DICOMweb para .NET C#" h2="Agregue los endpoints QIDO-RS, WADO-RS y STOW-RS a un servicio ASP.NET Core. La biblioteca habla el protocolo; su código decide dónde se almacenan los estudios." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Su propio servidor DICOMweb en C#">}}

<p>Un PACS, un VNA, un archivo en la nube o una plataforma de investigación debe responder a solicitudes DICOMweb: búsquedas QIDO-RS, descargas WADO-RS y cargas STOW-RS, y a menudo también los servicios de lista de trabajo. Cada uno de ellos tiene sus propias rutas, tipos de medio, cuerpos multipart, reglas de coincidencia y códigos de estado.</p>

<p><strong>Aspose.Medical for .NET</strong> implementa esa parte del protocolo desde la versión 26.10. Lee cada solicitud, la verifica contra el estándar, coincide las claves de búsqueda según las reglas DICOM, pagina los resultados, convierte sintaxis de transferencia y escribe la respuesta. Su código hace lo que solo usted puede hacer: almacenar una instancia, encontrar candidatos para una búsqueda y devolver los archivos almacenados. Los visores, modalidades y otros archivos entonces funcionan con su servidor como con cualquier otro servidor DICOMweb, y también lo hace el <a href="/medical/net/dicomweb-client/">cliente DICOMweb</a> de la misma biblioteca.</p>

<p>El servidor está gestionado en C#. Se ejecuta dondequiera que ASP.NET Core se ejecute: en Windows, en Linux y en un contenedor.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Un servidor completo en ASP.NET Core">}}

<p>Instale el paquete <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Incluye Aspose.Medical y conecta los servicios DICOMweb a la inyección de dependencias y al enrutamiento de endpoints. Este <code>Program.cs</code> es un servidor de Estudios funcional; las dos clases que registra se muestran a continuación.</p>

<div class="codeblock" id="code">
 <h3>Program.cs de un servidor DICOMweb - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>Ahora cada ruta de Studies responde bajo <code>/dicomweb</code>: <code>/dicomweb/studies</code> para búsqueda y carga, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> para recuperación, y las rutas para metadatos, fotogramas, datos masivos e imágenes renderizadas. El documento de capacidades en la raíz del servicio enumera exactamente las transacciones que el servidor soporta.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Conecte su almacenamiento">}}

<p>El servicio Studies necesita una interfaz con tres métodos: almacenar una instancia, devolver los archivos almacenados y devolver los candidatos de búsqueda. Esta implementación mantiene cada instancia como un archivo en una carpeta por estudio y serie.</p>

<div class="codeblock" id="code">
 <h3>Un archivo de carpetas detrás de QIDO-RS, WADO-RS y STOW-RS - C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>La biblioteca verifica los UID en cada ruta de solicitud y en cada instancia cargada antes de llamar al proveedor. Una instancia sin UID válidos se registra como fallida y nunca llega a <code>StoreAsync</code>.</p>

<p>Una carpeta es suficiente para probar el servidor. Un archivo de producción mantiene los mismos tres métodos y coloca un almacén de objetos detrás de <code>StoreAsync</code> y <code>RetrieveAsync</code> y un índice de base de datos detrás de <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Búsqueda: la biblioteca aplica las reglas de coincidencia DICOM">}}

<p>La coincidencia QIDO-RS es más que una comparación de cadenas: comodines, rangos de fecha y hora, listas de UID, nombres de persona difusos, valores vacíos y claves dentro de secuencias. El proveedor no implementa nada de ello. Devuelve candidatos en un orden estable, y puede devolver más de los que coinciden. La biblioteca aplica la coincidencia DICOM exacta a cada candidato, mantiene los campos solicitados y aplica <code>offset</code> y <code>limit</code>. Una respuesta contiene como máximo 1000 resultados por defecto, y <code>DicomWebStandardServiceOptions</code> modifica ese límite.</p>

<p>Para mantener rápidas las búsquedas en un archivo grande, pase las claves selectivas a la base de datos. <code>search.Plan.Filter</code> contiene las claves de búsqueda compiladas, con el atributo, el tipo de coincidencia y los valores de cada una:</p>

<div class="codeblock" id="code">
 <h3>Leer un Patient ID exacto de una búsqueda - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Una consulta a la base de datos construida de esta manera puede devolver filas extra, por ejemplo cuando ignora una clave que no puede traducir. La coincidencia final en la biblioteca las elimina. No debe descartar coincidencias reales, porque la biblioteca no puede volver a agregarlas.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Almacenamiento: un resultado para cada instancia">}}

<p>Una solicitud STOW-RS puede contener cientos de instancias. La biblioteca analiza el cuerpo multipart y llama a <code>StoreAsync</code> para cada instancia en turno, de modo que toda la carga nunca se mantiene en memoria de una sola vez. Para cada instancia, el proveedor devuelve <code>Stored</code>, <code>Warning</code> para una instancia almacenada con cambios, o <code>Failed</code> con un código de razón DICOM opcional.</p>

<p>La biblioteca genera el informe STOW-RS a partir de estos resultados y selecciona el estado: 200 cuando todas las instancias se almacenan, 202 cuando algunas tienen advertencias o fallan, 409 cuando todas fallan. Las instancias en una solicitud para un estudio nombrado que pertenecen a otro estudio son rechazadas antes de que se invoque al proveedor.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Recuperación: sintaxis de transferencia, fotogramas e imágenes renderizadas">}}

<p>El proveedor devuelve los archivos almacenados, y la biblioteca los entrega en la forma que el cliente solicitó. Un cliente WADO-RS puede solicitar una sintaxis de transferencia en el encabezado <code>Accept</code>, y la biblioteca convierte cada archivo antes de enviarlo, con sus propios códecs, incluidos HTJ2K y JPEG XL. Los metadatos en DICOM JSON o XML, los datos masivos y los fotogramas individuales provienen del mismo proveedor, sin código adicional.</p>

<p>Las imágenes renderizadas necesitan una clase adicional, porque solo usted decide qué imágenes puede producir el servidor. Esta ofrece JPEG para instancias renderizadas, fotogramas renderizados y miniaturas, y la biblioteca los renderiza:</p>

<div class="codeblock" id="code">
 <h3>Ofrecer imágenes JPEG renderizadas y miniaturas - C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>El renderizador incorporado produce JPEG a partir de una instancia de un solo marco o de los marcos que selecciona una solicitud. Una instancia multi-marco renderizada como un todo, una imagen más ancha o alta que 65500 píxeles (las miniaturas pasan por encima de tales imágenes), otros formatos de imagen y los parámetros de viewport, ventana y perfil ICC requieren su propio <code>RetrieveAsync</code> en esta clase.</p>

<p>Las miniaturas se manejan con cuidado para la privacidad del paciente. El renderizador incorporado crea una miniatura solo a partir de una instancia cuyos atributos Burned In Annotation y Recognizable Visual Features son ambos <code>NO</code>, lo que indica que la propia imagen declara que no muestra datos del paciente. Muchas imágenes no poseen estos atributos, y para ellas el servidor responde 406. Si su archivo necesita miniaturas de dichas imágenes, génerelas en su propio <code>RetrieveAsync</code> y asegúrese de que oculten cualquier dato del paciente.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Listas de trabajo, pasos de procedimiento y los otros servicios">}}

<p>Los demás servicios DICOMweb siguen el mismo modelo: registrar el servicio, implementar su interfaz de almacenamiento.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> listas de trabajo con <code>AddStandardWorklistService</code>: crear, buscar y actualizar ítems de trabajo y cambiar su estado, con las reglas de estado verificadas por la biblioteca y notificaciones de eventos enviadas mediante WebSocket.</li>
<li>La <a href="/medical/net/modality-worklist/">lista de trabajo de modalidad</a> (MWL-RS) con <code>AddStandardModalityWorklistService</code>, para que las modalidades puedan consultar los pasos de procedimiento programados de su RIS.</li>
<li>Pasos de procedimiento realizados por la modalidad (MPPS-RS), compromiso de almacenamiento e instancias no de pacientes como paletas de colores y protocolos de colgado.</li>
<li>Recuperación WADO-URI para visores antiguos, servida por el servicio Studies.</li>
</ul>

<p>Cuando el comportamiento estándar de una transacción no se ajusta, registre su propio manejador para ella con <code>AddHandler</code>. La biblioteca sigue enrutando y verificando la solicitud, y las demás transacciones conservan sus manejadores estándar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="La seguridad y el alojamiento permanecen en sus manos">}}

<p>Los endpoints DICOMweb son endpoints ordinarios de ASP.NET Core. La autenticación, autorización, CORS, TLS, encabezados reenviados y los límites de tamaño de solicitud se configuran de la misma manera que para el resto de su servicio:</p>

<div class="codeblock" id="code">
 <h3>Requerir un usuario autenticado - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>La parte del protocolo no depende de ASP.NET Core. <code>DicomWebApplication</code> convierte una <code>DicomWebRequest</code> en una respuesta, de modo que los mismos servicios pueden ejecutarse detrás de otro servidor HTTP o en una prueba sin red:</p>

<div class="codeblock" id="code">
 <h3>El mismo servidor sin ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Cliente y servidor de una sola biblioteca">}}

<p>El <a href="/medical/net/dicomweb-client/">cliente DICOMweb</a> de Aspose.Medical prueba su servidor desde una prueba unitaria o un programa de consola, y cada archivo que el servidor recibe es un <code>DicomFile</code> regular que puede anonimizarse, transcodificarse o renderizarse en el mismo proceso.</p>

<p>La <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">guía para desarrolladores DICOMweb</a> cubre cada servicio e interfaz de almacenamiento, y la <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">guía de ASP.NET Core</a> cubre el mapeo de endpoints y WebSockets.</p>

<p>Páginas relacionadas: <a href="/medical/net/dicom-networking/">Redes DICOM</a> para sistemas que envían estudios mediante DIMSE en lugar de HTTP, <a href="/medical/net/anonymization/">anonimización</a> de estudios entrantes, <a href="/medical/net/dicom-transfer-syntax-conversion/">conversión de sintaxis de transferencia</a>, y <a href="/medical/net/dicom-to-json/">DICOM a JSON</a> para el formato que utilizan los resultados QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guía del desarrollador" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="Referencias API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Soporte del producto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Soporte gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Soporte de pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="¿Por qué Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Historias de éxito" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
