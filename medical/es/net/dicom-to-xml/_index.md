---
title: Convertir DICOM a XML en C# .NET | Aspose.Medical
weight: 3000
description: Serialice conjuntos de datos DICOM al formato estándar DICOM XML en C# .NET. Configure el manejo de datos masivos, el procesamiento basado en streams y operaciones asincrónicas con la API de Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convertir DICOM a XML en .NET C#" h2="Serialice conjuntos de datos DICOM a la representación estándar DICOM XML (PS3.19). Configure referencias a datos masivos, salida basada en streams y procesamiento asincrónico con una biblioteca .NET pura." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Serialización DICOM XML basada en estándares">}}

<p><strong>Aspose.Medical for .NET</strong> serializa datos DICOM a XML siguiendo el <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a>. Este es el estándar oficial para representar conjuntos de datos DICOM en XML, utilizado por servicios DICOMweb, plataformas de integración y sistemas que requieren una representación legible por humanos y validada mediante esquema de los metadatos de imágenes médicas.</p>

<p>La clase <code>DicomXmlSerializer</code> proporciona métodos estáticos tanto para serialización como para deserialización. A diferencia de los enfoques simples de volcado de etiquetas, la salida se ajusta al esquema DICOM XML donde cada elemento se representa con su tag, VR y valores formateados correctamente &mdash; lo que permite una conversión sin pérdidas de ida y vuelta entre DICOM binario y XML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serializar DICOM a XML en C#">}}

<p>Utilice la clase <code>DicomXmlSerializer</code> para convertir un conjunto de datos DICOM en una cadena XML. El enfoque más sencillo produce un documento XML conforme a los estándares:</p>

<div class="codeblock" id="code">
 <h3>Convertir DICOM a XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialización basada en streams y asincrónica">}}

<p>Para archivos DICOM grandes o escenarios de alto rendimiento, serialice directamente a un stream para evitar asignar grandes cadenas en memoria. Se disponen de métodos síncronos y asincrónicos:</p>

<div class="codeblock" id="code">
 <h3>Serialización de stream síncrona - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Serialización de stream asincrónica - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streaming en canalización para estudios grandes">}}

<p>Los estudios completos no tienen que mantenerse en memoria. <code>DicomXmlSerializer</code> escribe en un <code>PipeWriter</code> y lee de un <code>PipeReader</code>, de modo que el XML puede producirse y consumirse mientras fluye, y una secuencia de conjuntos de datos puede leerse uno a la vez mediante <code>DeserializeAsyncEnumerable</code>. Cada método recibe un <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Serializar y deserializar a través de un pipe - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Leer una secuencia de conjuntos de datos uno a la vez - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opciones de serialización">}}

<p>La clase <code>DicomXmlSerializerOptions</code> controla cómo se representa la información DICOM en XML. La configuración principal implica el manejo de datos masivos para valores binarios grandes:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Propiedad</th>
<th>Tipo</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Conversor personalizado para escribir datos grandes (p. ej., datos de píxeles) como referencias URI BulkData en lugar de incrustarlos</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Cargador personalizado para resolver URIs BulkData durante la deserialización</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Instancia de opciones predeterminada utilizada cuando no se proporcionan opciones personalizadas</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Serializar con opciones personalizadas - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Manejo de datos masivos">}}

<p>Los valores binarios grandes (datos de píxeles, formas de onda, documentos encapsulados) pueden externalizarse como referencias URI BulkData en lugar de incrustarse en la salida XML. Esto sigue la especificación del <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">elemento BulkData del DICOM PS3.19</a>.</p>

<p>Implemente <code>IBulkDataConverter</code> para externalizar datos grandes durante la serialización, y <code>IBulkDataLoader</code> para resolver URIs durante la deserialización. Para los casos comunes no es necesario escribir un cargador: <code>DefaultBulkDataLoader.Instance</code> resuelve URIs <code>file</code>, <code>http</code> y <code>https</code>, y también implementa <code>IAsyncBulkDataLoader</code>, por lo que los datos masivos se obtienen de forma asíncrona en las rutas de streaming.</p>

<div class="codeblock" id="code">
 <h3>Manejo personalizado de datos masivos - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
public class FileBulkDataConverter : IBulkDataConverter
{
    public string? GetBulkDataUri(IElement element)
    {
        // Externalize pixel data to a separate file
        if (element.Tag == Tag.PixelData)
            return "file:///bulk/pixeldata.raw";
        return null; // inline all other elements
    }
}

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Deserializar XML a DICOM">}}

<p>Analice XML DICOM y conviértalo nuevamente en objetos Dataset. Admite entrada de cadena, entrada de stream y operaciones asincrónicas:</p>

<div class="codeblock" id="code">
 <h3>Deserializar XML a DICOM - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialización XML vs JSON">}}

<p>Aspose.Medical admite la serialización tanto DICOM XML (PS3.19) como DICOM JSON (PS3.18). Ambos formatos ofrecen conversión sin pérdidas de ida y vuelta, pero sirven a diferentes escenarios de integración:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Característica</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Estándar</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>Validación de esquema</td><td>XML Schema (XSD) disponible</td><td>Sin esquema formal</td></tr>
<tr><td>Mejor para</td><td>Integración empresarial, HL7 CDA, registros de auditoría, registros XDS</td><td>DICOMweb, APIs REST, FHIR ImagingStudy</td></tr>
<tr><td>Legibilidad humana</td><td>Verborreico pero autodescriptivo</td><td>Compacto y ampliamente soportado</td></tr>
<tr><td>Datos masivos</td><td>Elemento BulkData con URI</td><td>Propiedad BulkDataURI</td></tr>
<tr><td>Clase serializadora</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Código fuente" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
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
