---
title: Convertir DICOM a JSON en C# .NET | Aspose.Medical
weight: 4000
description: Serialice archivos DICOM al formato JSON DICOM estándar en C# .NET. Configure el manejo de números, URIs de datos masivos, salida de palabras clave y procesamiento asíncrono de flujos con la API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convertir DICOM a JSON en .NET C#" h2="Serialice conjuntos de datos y archivos DICOM al Modelo JSON DICOM estándar (PS3.18). Configure el manejo de números, referencias a datos masivos, salida de palabras clave y procesamiento asíncrono basado en flujos." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Modelo JSON DICOM estándar">}}

<p><strong>Aspose.Medical for .NET</strong> serializa datos DICOM a JSON siguiendo el <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">Modelo JSON DICOM PS3.18</a>. Este es el estándar oficial para representar conjuntos de datos DICOM en JSON, utilizado por los servicios DICOMweb, los recursos FHIR ImagingStudy y las API modernas de salud.</p>

<p>En el Modelo JSON DICOM, cada atributo se representa por su etiqueta como la clave JSON (p. ej., <code>"00100010"</code> para Patient Name), con la Representación de Valor y la matriz de valores como propiedades anidadas:</p>

<div class="codeblock" id="code">
 <h3>Formato del modelo JSON DICOM</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serializar DICOM a JSON en C#">}}

<p>Utilice la clase <code>DicomJsonSerializer</code> para convertir un archivo DICOM o un conjunto de datos a JSON. El enfoque más sencillo produce una salida compacta y conforme a los estándares:</p>

<div class="codeblock" id="code">
 <h3>Convertir DICOM a JSON - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p>Puede serializar un <code>Dataset</code>, un <code>DicomFile</code> completo (incluyendo la Información Meta del Archivo), o una matriz de conjuntos de datos:</p>

<div class="codeblock" id="code">
 <h3>Serializar DicomFile y matrices de conjuntos de datos - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialización basada en flujos y asíncrona">}}

<p>Para archivos DICOM grandes o escenarios de alto rendimiento, serialice directamente a un flujo UTF-8 para evitar asignar grandes cadenas en memoria. Ambos métodos sincrónicos y asíncronos están disponibles:</p>

<div class="codeblock" id="code">
 <h3>Serialización de flujo y asíncrona - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opciones de serialización">}}

<p>La clase <code>DicomJsonSerializerOptions</code> controla cómo se representan los datos DICOM en JSON:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Propiedad</th>
<th>Tipo</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Controla si los VR numéricos (IS, DS, SV, UV) se escriben como números JSON o como cadenas</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Utiliza palabras clave DICOM (p. ej., <code>"PatientName"</code>) en lugar de etiquetas (<code>"00100010"</code>) como claves JSON. Nota: no estándar</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Incluye la palabra clave DICOM como un atributo JSON adicional</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Incluye el nombre de la etiqueta DICOM como un atributo JSON adicional</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Convertidor personalizado para escribir datos grandes (p. ej., datos de píxeles) como referencias URI</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Cargador personalizado para resolver URIs de datos masivos durante la deserialización</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Configurar opciones de serialización - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Manejo de números">}}

<p>Las Representaciones de Valor numéricas DICOM (IS, DS, SV, UV) pueden serializarse como números JSON o como cadenas JSON. Esto es importante para la interoperabilidad, ya que algunas implementaciones DICOM almacenan números con espacios iniciales o formatos no estándar:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modo</th>
<th>Salida JSON</th>
<th>Caso de uso</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>Conforme al estándar, ideal para cálculo. Lanza excepción si el valor no puede analizarse.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>Preserva la representación original como cadena. Más seguro para la conversión de ida y vuelta de datos no estándar.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Manejo de datos masivos">}}

<p>Los valores binarios grandes (datos de píxeles, formas de onda, documentos encapsulados) pueden manejarse como referencias a datos masivos en lugar de incrustarse en la salida JSON. Esto sigue la especificación <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>.</p>

<p>Implemente <code>IBulkDataConverter</code> para externalizar datos grandes durante la serialización, y <code>IBulkDataLoader</code> para resolver URIs durante la deserialización:</p>

<div class="codeblock" id="code">
 <h3>Manejo personalizado de datos masivos - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Deserializar JSON a DICOM">}}

<p>Analice JSON DICOM de vuelta a objetos Dataset o DicomFile. Admite entrada de cadena, entrada de flujo y operaciones de flujo asíncronas:</p>

<div class="codeblock" id="code">
 <h3>Deserializar JSON a DICOM - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Integración con System.Text.Json">}}

<p>Aspose.Medical ofrece <code>DatasetJsonConverter</code> y <code>DicomFileJsonConverter</code> para la integración con la canalización de serialización estándar <code>System.Text.Json</code>. Esto permite que los objetos DICOM se serialicen junto con otros tipos .NET en su código de procesamiento JSON existente:</p>

<div class="codeblock" id="code">
 <h3>Integración de System.Text.Json - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
</div>

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
