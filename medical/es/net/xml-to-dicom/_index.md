---
title: Convertir XML a DICOM en C# .NET | Aspose.Medical
weight: 5000

description: Cree archivos DICOM a partir del XML del Modelo DICOM Nativo de PS3.19 en C# .NET. Lea XML desde una cadena, un flujo o una tubería, transmita documentos consecutivos y resuelva referencias a datos masivos con la API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convertir XML a DICOM en .NET C#" h2="Lea el XML del Modelo DICOM Nativo de PS3.19 de nuevo en conjuntos de datos y archivos DICOM. Trabaje desde una cadena, un flujo o una tubería, transmita documentos consecutivos y resuelva referencias a datos masivos." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="XML estándar del Modelo DICOM Nativo">}}

<p><strong>Aspose.Medical for .NET</strong> lee el <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Modelo DICOM Nativo</a> definido en DICOM PS3.19. Esta es la representación XML incorporada en el propio estándar, no un formato inventado por Aspose, lo que la hace útil para la integración: un sistema que ya intercambia DICOM como XML genera documentos que esta biblioteca acepta.</p>

<p>La raíz del documento es <code>NativeDicomModel</code>, y cada atributo es un elemento <code>DicomAttribute</code> que lleva su etiqueta, representación de valor y palabra clave:</p>

<div class="codeblock" id="code">
 <h3>Formato del Modelo DICOM Nativo</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>Esta página corresponde a la dirección inversa de <a href="/medical/net/dicom-to-xml/">DICOM a XML</a>, y ambas utilizan la misma clase, <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Crear un archivo DICOM a partir de XML en C#">}}

<p><code>Deserialize</code> convierte un documento en un <code>Dataset</code>, y un dataset se escribe en disco como un archivo DICOM.</p>

<div class="codeblock" id="code">
 <h3>Crear un archivo DICOM a partir de XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>El Modelo DICOM Nativo no tiene grupo de Información Meta del Archivo, por lo que la sintaxis de transferencia no forma parte del documento. Un dataset envuelto en un <code>DicomFile</code> se escribe con la sintaxis de transferencia predeterminada, Implicit VR Little Endian. Para almacenar el archivo con otra sintaxis, transcodeelo, como muestra la página de <a href="/medical/net/dicom-transfer-syntax-conversion/">conversión de sintaxis de transferencia</a>.</p>

<p>Leer DICOM XML es una función con licencia. Sin una licencia on-premise aplicada, el lector lanza una <code>MedicalApiException</code>, por lo que debe aplicar la licencia primero, como describe la <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">guía de licenciamiento</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Flujos, tuberías y asincrónico">}}

<p>Cada punto de entrada tiene una sobrecarga de flujo y una sobrecarga asincrónica, y las asincrónicas también aceptan un <code>PipeReader</code>. Un documento que llega desde una respuesta web se analiza a medida que se lee, sin convertirlo primero en una cadena.</p>

<div class="codeblock" id="code">
 <h3>Leer XML desde un flujo - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Documentos consecutivos en un solo flujo">}}

<p>Una exportación de otro sistema a menudo contiene un elemento <code>NativeDicomModel</code> tras otro en un solo flujo. <code>DeserializeAsyncEnumerable</code> genera un dataset por elemento, en el orden de entrada, por lo que el flujo se procesa sin almacenarse en memoria. Los elementos se siguen directamente: una declaración XML solo se permite al inicio, como en cualquier entrada XML.</p>

<div class="codeblock" id="code">
 <h3>Transmitir documentos consecutivos - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Referencias a datos masivos">}}

<p>Los valores grandes, como los datos de píxeles, no se escriben en línea. Aparecen como un elemento <code>BulkData</code> con una URI que apunta a los bytes, lo que mantiene el documento pequeño. Para resolver esas referencias durante la lectura, proporcione al serializador un cargador de datos masivos. <code>DefaultBulkDataLoader</code> recupera URIs <code>file</code>, <code>http</code> y <code>https</code> sin autenticación; para un archivo que requiera credenciales, implemente usted mismo <code>IBulkDataLoader</code> o <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Resolver datos masivos durante la lectura - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Viaje de ida y vuelta con DICOM a XML">}}

<p>Se pretende que ambas direcciones se usen juntas: un estudio sale como XML, pasa por un sistema que interpreta XML y regresa como un archivo DICOM. Todo está gestionado en .NET, por lo que el mismo ciclo funciona en Windows, Linux y macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM a XML y regreso - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Para las opciones que controlan la apariencia del XML, consulte la página <a href="/medical/net/dicom-to-xml/">DICOM a XML</a>. El mismo par existe para JSON: <a href="/medical/net/dicom-to-json/">DICOM a JSON</a> y <a href="/medical/net/json-to-dicom/">JSON a DICOM</a>. La <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">guía de serialización</a> cubre toda la API.</p>

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
{{< blocks/products/pf/slr-element name="Casos de éxito" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}