---
title: Redes DICOM en C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Conecte su aplicación .NET a un PACS. Verifique con C-ECHO, envíe imágenes con C-STORE, consulte con C-FIND y reciba imágenes con su propio SCP. Un cliente y servidor DIMSE en C# puro.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Redes DICOM en .NET C#" h2="Comunique con un PACS desde su propia aplicación: C-ECHO, C-STORE, C-FIND, C-MOVE y C-GET, como cliente y como servidor, en C# gestionado sin necesidad de instalar nada en la máquina." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Conéctese a un PACS desde su propio código">}}

<p>Leer archivos DICOM es la mitad fácil de la imagen médica. En el momento en que su aplicación debe trabajar con un sistema hospitalario real, tiene que hablar DIMSE: abrir una asociación con un PACS, enviar imágenes, preguntar qué estudios existen y responder cuando otro sistema envía algo de vuelta.</p>

<p><strong>Aspose.Medical for .NET</strong> incluye ese protocolo como parte de la biblioteca. <code>Aspose.Medical.Dicom.Network</code> le brinda un cliente DIMSE y un servidor DIMSE, ambos escritos en C# gestionado. No hay ninguna herramienta nativa que instalar, ningún servicio que configurar y nada específico de plataforma, por lo que el mismo código se ejecuta en Windows, Linux y en un contenedor.</p>

<p>Tres cosas cubren la mayoría de las integraciones, y cada una requiere solo unas pocas líneas: verificar la conexión con C-ECHO, enviar imágenes con C-STORE y buscar lo que hay del otro lado con C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Comience con C-ECHO">}}

<p>C-ECHO es el ping DICOM. Demuestra que el host, el puerto y los dos títulos AE son correctos antes de que se culpe cualquier otra cosa. Construya un cliente una vez, asígnele un manejador que observe la respuesta y envíe la solicitud.</p>

<div class="codeblock" id="code">
 <h3>Verifique la conexión con C-ECHO - C#</h3>
 <pre><code class="cs">AssociationNegotiationOptions negotiation = new AssociationNegotiationOptions()
    .WithPresentationContext(new PresentationContext
    {
        AbstractSyntax = Uid.Verification,
        Role = null,
        TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
    });

DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = negotiation
    })
    .AddCEchoHandler((request, response, cancellationToken) =>
    {
        Console.WriteLine($"C-ECHO answered with status 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(new CEchoRequest());

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>El estado <code>0x0000</code> significa éxito. Las solicitudes se encolan y luego se envían en una sola asociación, de modo que un lote de trabajo no abre una conexión por cada elemento.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Envíe imágenes con C-STORE">}}

<p>C-STORE es lo que hace una aplicación después de haber producido o recibido una imagen: envía la instancia al archivo. Enliste una solicitud por instancia y envíelas juntas.</p>

<div class="codeblock" id="code">
 <h3>Envíe un archivo DICOM a un PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Los contextos de presentación que proponga deciden qué aceptará el archivo. Si desea una sintaxis comprimida, transcode antes de enviar, como muestra la página de <a href="/medical/net/dicom-transfer-syntax-conversion/">conversión de sintaxis de transferencia</a>, o enumere las alternativas en <code>AdditionalTransferSyntaxes</code> y permita que la negociación elija una.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Busque estudios con C-FIND">}}

<p>C-FIND responde a la pregunta "qué tiene el archivo". Los resultados llegan uno a la vez, cada uno con su propio conjunto de datos de identificación, y una respuesta final cierra la consulta.</p>

<div class="codeblock" id="code">
 <h3>Consulte estudios por paciente - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.StudyRootQueryRetrieveInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddCFindHandler((request, response, cancellationToken) =>
    {
        // A match arrives with an identifier; the final response carries the status only
        if (response.Identifier is not null)
            Console.WriteLine(response.Identifier.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty));

        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(CFindRequest.CreateStudyQuery(
    patientId: "PATIENT-001",
    patientName: null,
    studyDateTime: null,
    accession: null,
    studyId: null,
    modalitiesInStudy: null,
    studyInstanceUid: null,
    priority: DimsePriority.Medium));

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>La misma fábrica construye los demás niveles de consulta: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> y <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> crea una consulta de lista de trabajo de modalidad, que es la consulta que una modalidad realiza antes de un escaneo.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Reciba imágenes: su propio SCP de almacenamiento">}}

<p>La biblioteca también es un servidor. Registre un manejador para el servicio que desea ofrecer, comience a escuchar, y su aplicación se convierte en un nodo DICOM al que una modalidad u otro PACS pueden enviar datos.</p>

<div class="codeblock" id="code">
 <h3>Acepte imágenes entrantes - C#</h3>
 <pre><code class="cs">DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Any, 11112)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.SecondaryCaptureImageStorage,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddSingletonCStoreHandler(new StoreHandler())
    .Build();

await server.StartAsync(CancellationToken.None);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>El manejador que almacena lo que llega - C#</h3>
 <pre><code class="cs">public sealed class StoreHandler : ICStoreRequestHandler
{
    public ValueTask&lt;CStoreResponse&gt; Handle(CStoreRequest request, CancellationToken cancellationToken)
    {
        // Write what arrived, then answer Success
        new DicomFile(request.Dataset).Save($"{request.AffectedSopInstanceUid}.dcm");

        CStoreResponse response = new();
        response.Command.AddOrUpdate(Tag.Status, (ushort)0x0000);
        return ValueTask.FromResult(response);
    }
}</code></pre>
</div>

<p>Lo que el manejador haga con el conjunto de datos es su decisión: escribirlo en disco, colocarlo en una cola, anonimizarlo primero con la <a href="/medical/net/anonymization/">API de anonimización</a>, o transcodificarlo a la sintaxis del archivo.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Qué más cubre la API de redes">}}

<p>Los tres servicios anteriores son los comunes. El resto de DIMSE también está disponible:</p>

<ul>
<li>Recuperación: C-MOVE y C-GET, con los contadores de suboperaciones reportados en las respuestas.</li>
<li>Los servicios N: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE y N-EVENT-REPORT, de los que se construyen el compromiso de almacenamiento y MPPS.</li>
<li>Control de asociación: contextos de presentación, roles de clase de servicio, negociación extendida, ventana de operaciones asíncronas, negociación de identidad de usuario y un gancho de política que puede rechazar una asociación llamando al título AE.</li>
<li>TLS en ambos extremos, mediante <code>TlsInitiatorAuthenticator</code> y <code>TlsAcceptorAuthenticator</code>, con su propia validación de certificados si la necesita.</li>
<li>Timeouts para cada etapa, desde la conexión TCP hasta la liberación, y notificaciones del ciclo de vida de la asociación para que un nodo de larga duración pueda registrar lo ocurrido.</li>
</ul>

<p>La <a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">guía de redes DICOM</a> documenta cada opción y cada manejador.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2=".NET puro, desde el socket hasta los datos de píxel">}}

<p>Todo en esta página es código gestionado del mismo paquete que lee y escribe los archivos. La asociación, los códecs y el analizador provienen de una única biblioteca, de modo que un estudio recibido por la red puede anonimizarse, transcodificarse o serializarse sin salir del proceso y sin ninguna dependencia nativa en la cadena.</p>

<p>Páginas relacionadas: <a href="/medical/net/dicom-transfer-syntax-conversion/">conversión de sintaxis de transferencia</a> para saber qué enviar, <a href="/medical/net/anonymization/">anonimización</a> para saber qué eliminar primero, y <a href="/medical/net/dicom-tags/">etiquetas DICOM</a> para leer lo que llegó.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guía del desarrollador" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="Referencias de API" href="https://reference.aspose.com/medical/net/" >}}
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
