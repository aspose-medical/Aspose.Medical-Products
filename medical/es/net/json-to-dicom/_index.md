---
title: Convertir JSON a DICOM en C# .NET | Aspose.Medical
weight: 6000

description: Cree archivos DICOM a partir del Modelo JSON estándar DICOM (PS3.18) en C# .NET. Lea JSON desde una cadena, un flujo o una tubería, transmita una secuencia de datasets y resuelva referencias de datos masivos con la API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convertir JSON a DICOM en .NET C#" h2="Lea el Modelo JSON estándar DICOM (PS3.18) de nuevo en datasets y archivos DICOM. Trabaje desde una cadena, un flujo o una tubería, transmita una secuencia de estudios y resuelva referencias de datos masivos." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="De JSON DICOM a un archivo DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> lee el <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">Modelo JSON DICOM PS3.18</a>, la representación utilizada por los servicios DICOMweb y por los sistemas que intercambian estudios mediante HTTP. Lo que llega como JSON se convierte en un <code>Dataset</code>, y un <code>Dataset</code> se escribe en disco como un archivo DICOM.</p>

<p>Esta es la dirección inversa de la página <a href="/medical/net/dicom-to-json/">DICOM a JSON</a>, y ambas usan la misma clase, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Crear un archivo DICOM a partir de JSON - C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Un dataset que no contiene File Meta Information se escribe con la sintaxis de transferencia predeterminada, Implicit VR Little Endian, cuando se envuelve en un <code>DicomFile</code>.</p>

<p>Leer JSON DICOM es una función con licencia. Sin una licencia on-premise aplicada, el lector lanza una <code>MedicalApiException</code>, por lo que primero debe aplicar la licencia, como describe la <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">guía de licenciamiento</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Conservar el File Meta Information">}}

<p><code>Deserialize</code> devuelve solo el dataset. Cuando el documento JSON también incluye el grupo File Meta Information, por ejemplo porque se generó a partir de un archivo DICOM completo, <code>DeserializeFile</code> devuelve un <code>DicomFile</code> con ese grupo intacto, incluida la sintaxis de transferencia que declara el archivo.</p>

<div class="codeblock" id="code">
 <h3>Leer un archivo DICOM completo desde JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Flujos, tuberías y async">}}

<p>Cada punto de entrada tiene una sobrecarga de stream y una sobrecarga asincrónica, y las versiones asincrónicas también aceptan un <code>PipeReader</code>. Un documento que llega desde una respuesta web o desde disco se lee sin convertirlo primero en una cadena, lo cual es importante cuando el JSON contiene datos de píxeles.</p>

<div class="codeblock" id="code">
 <h3>Leer JSON desde un stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Una secuencia de datasets, uno a la vez">}}

<p>Una consulta DICOMweb responde con una matriz de datasets, y dicho documento puede ser grande. <code>DeserializeList</code> lee toda la matriz en memoria; <code>DeserializeAsyncEnumerable</code> devuelve un dataset a la vez, de modo que el documento nunca se mantiene completo en memoria.</p>

<div class="codeblock" id="code">
 <h3>Transmitir una matriz de datasets - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Referencias a datos masivos">}}

<p>El Modelo JSON DICOM no lleva los datos de píxeles incrustados. Los valores grandes se reemplazan por un <code>BulkDataURI</code> que apunta a los bytes, lo que mantiene el documento JSON pequeño. Para resolver esas referencias durante la lectura, proporcione al serializador un cargador de datos masivos. <code>DefaultBulkDataLoader</code> recupera URIs <code>file</code>, <code>http</code> y <code>https</code> sin autenticación; para un archivo que necesita credenciales, implemente usted mismo <code>IBulkDataLoader</code> o <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Resolver BulkDataURI durante la lectura - C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Viaje de ida y vuelta con DICOM a JSON">}}

<p>Las dos direcciones están diseñadas para usarse juntas: un estudio sale como JSON, viaja a través de un servicio web y vuelve como un archivo DICOM. Nada del proceso depende de código nativo, por lo que el mismo viaje de ida y vuelta se ejecuta en Windows, Linux y macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM a JSON y de vuelta - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Para las opciones que controlan cómo se ve el JSON, consulte la página <a href="/medical/net/dicom-to-json/">DICOM a JSON</a>. El mismo par existe para XML: <a href="/medical/net/dicom-to-xml/">DICOM a XML</a> y <a href="/medical/net/xml-to-dicom/">XML a DICOM</a>. La <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">guía de serialización JSON</a> cubre toda la API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guía del desarrollador" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="Referencias de API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Soporte del producto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Soporte gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Soporte pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="¿Por qué Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Historias de éxito" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}