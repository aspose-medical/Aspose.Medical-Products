---
title: Anonimizar archivos DICOM en C# .NET | Aspose.Medical
weight: 1000
description: Anonimice archivos DICOM en C# .NET usando perfiles de confidencialidad configurables. Elimine la información de identificación personal (PII) del paciente, garantizando el cumplimiento de HIPAA y GDPR con la API de Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Anonimizar archivos DICOM en .NET C#" h2="Elimine la información de identificación del paciente de los archivos DICOM usando los perfiles de confidencialidad DICOM PS 3.15. Garantice el cumplimiento de HIPAA y GDPR con una biblioteca .NET pura." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Anonimización DICOM basada en estándares">}}

<p><strong>Aspose.Medical for .NET</strong> ofrece una API integral de anonimización DICOM basada en los <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">Perfiles de Confidencialidad DICOM PS 3.15</a>. La API permite a los desarrolladores de salud eliminar o modificar la información de identificación del paciente (PII) de los archivos DICOM mientras se preserva el valor clínico de los datos de imagen. A diferencia de los enfoques simples de eliminación de etiquetas, Aspose.Medical sigue el estándar oficial DICOM para garantizar una desidentificación adecuada.</p>

<p>La clase <code>Anonymizer</code> trabaja con objetos <code>ConfidentialityProfile</code> que definen exactamente cómo debe manejarse cada atributo DICOM &mdash; si debe eliminarse, reemplazarse por un valor ficticio, limpiarse o mantenerse sin cambios. Este enfoque garantiza una anonimización coherente y auditable que cumple con los requisitos regulatorios.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonimizar un archivo DICOM en C#">}}

<p>La forma más sencilla de anonimizar un archivo DICOM es usar el perfil de confidencialidad predeterminado. Esto aplica el Perfil Básico DICOM PS 3.15, que gestiona todos los atributos estándar de identificación del paciente:</p>

<div class="codeblock" id="code">
 <h3>Anonimización DICOM básica - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>El método <code>Anonymize</code> es <strong>no destructivo</strong> &mdash; clona el conjunto de datos original y devuelve una nueva copia anonimizada. El archivo DICOM original permanece sin cambios, lo cual es esencial para los rastros de auditoría y los flujos de trabajo de integridad de datos.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Reemplazo personalizado de la identidad del paciente">}}

<p>En muchos flujos de trabajo, es necesario reemplazar la información del paciente con valores alias específicos en lugar de simplemente eliminarla. La clase <code>ConfidentialityProfile</code> le permite establecer valores de reemplazo para el nombre del paciente y el ID del paciente:</p>

<div class="codeblock" id="code">
 <h3>Anonimizar con identidad de paciente personalizada - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create a confidentiality profile with custom replacement values
ConfidentialityProfile profile = new()
{
    PatientName = "ANONYMOUS PATIENT",
    PatientId = "00000000"
};

// Create an anonymizer with this profile
Anonymizer anonymizer = new(profile);

// Anonymize the file
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("custom_anonymized.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonimización in-place">}}

<p>Para escenarios donde la eficiencia de memoria es importante o no necesita conservar los datos originales, la API ofrece anonimización in-place que modifica el conjunto de datos directamente:</p>

<div class="codeblock" id="code">
 <h3>Anonimización DICOM in-place - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Los métodos <code>Anonymize</code> y <code>AnonymizeInPlace</code> también aceptan un objeto <code>Dataset</code> directamente, brindándole control total sobre qué conjunto de datos se procesa.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opciones configurables del perfil de confidencialidad">}}

<p>El estándar DICOM PS 3.15 define un <strong>Perfil Básico</strong> junto con varios perfiles opcionales que controlan cómo se manejan categorías específicas de datos durante la anonimización. Puede combinar estas opciones usando el enum <code>ConfidentialityProfileOptions</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Opción</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>El Perfil de Confidencialidad de Nivel de Aplicación Básico DICOM PS 3.15 predeterminado</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Conservar atributos privados seguros durante la anonimización</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Mantener los UIDs DICOM originales (Estudio, Serie, Instancia) sin cambios</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Conservar los atributos de identificación del dispositivo (fabricante, modelo, nombre de estación)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Conservar los atributos de identificación de la institución (nombre, dirección, departamento)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Conservar las características del paciente (edad, sexo, talla, peso) con fines de investigación</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Conservar los valores completos de fecha y hora para estudios longitudinales</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Conservar las fechas modificadas para estudios longitudinales</td></tr>
<tr><td><code>CleanDesc</code></td><td>Limpiar descripciones de texto que puedan contener información identificativa</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Limpiar contenido estructurado que pueda contener información identificativa</td></tr>
<tr><td><code>CleanGraph</code></td><td>Limpiar datos gráficos que puedan contener información de paciente incrustada</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Anonimizar con opciones de perfil específicas - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Códigos de acción para el manejo de atributos">}}

<p>Cada atributo en un perfil de confidencialidad tiene asignado un código de acción que define cómo debe procesarse durante la anonimización. Estos siguen el estándar DICOM PS 3.15 Tabla E.1-1a:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Código</th>
<th>Acción</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Reemplazar con dummy</td><td>Reemplazar con un valor de longitud no cero consistente con el VR</td></tr>
<tr><td><strong>Z</strong></td><td>Longitud cero o dummy</td><td>Reemplazar con un valor de longitud cero o un valor dummy consistente con el VR</td></tr>
<tr><td><strong>X</strong></td><td>Eliminar</td><td>Eliminar el atributo completamente del conjunto de datos</td></tr>
<tr><td><strong>K</strong></td><td>Mantener</td><td>Mantener el atributo sin cambios (para no secuencias) o limpio (para secuencias)</td></tr>
<tr><td><strong>C</strong></td><td>Limpiar</td><td>Reemplazar con un valor limpiado de significado similar consistente con el VR</td></tr>
<tr><td><strong>U</strong></td><td>Reemplazar UID</td><td>Reemplazar con un nuevo UID que sea internamente consistente dentro del conjunto de instancias</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Perfiles personalizados desde CSV, JSON y XML">}}

<p>Para casos de uso avanzados, puede definir perfiles de anonimización personalizados cargando reglas desde archivos CSV, JSON o XML externos. Esto le permite adaptar el comportamiento de anonimización a sus políticas organizacionales específicas o a requisitos regulatorios:</p>

<div class="codeblock" id="code">
 <h3>Cargar perfil de anonimización desde CSV - C#</h3>
 <pre><code class="cs">// CSV format: TagPattern;Action
// 0010,0010;Z   (Anonymize PatientName)
// 0010,0020;D   (Replace PatientID with dummy)
// 0020,000D;U   (Replace StudyInstanceUID)

var profile = ConfidentialityProfile.LoadFromCsvFile(
    "anonymization_rules.csv",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Cargar perfil de anonimización desde JSON - C#</h3>
 <pre><code class="cs">// JSON format:
// [
//     { "Tag": "0010,0010", "Action": "Z" },
//     { "Tag": "0010,0020", "Action": "D" },
//     { "Tag": "0020,000D", "Action": "U" }
// ]

var profile = ConfidentialityProfile.LoadFromJsonFile(
    "anonymization_rules.json",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<p>Los perfiles personalizados también pueden cargarse desde archivos XML (<code>LoadFromXmlFile</code>), flujos (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) o directamente desde cadenas (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Cumplimiento de HIPAA y GDPR">}}

<p>Los archivos DICOM suelen contener Información de Salud Protegida (PHI) según la definición de HIPAA y datos personales según la definición de GDPR. Aspose.Medical for .NET le ayuda a cumplir los requisitos regulatorios al proporcionar:</p>

<ul>
<li><strong>cumplimiento DICOM PS 3.15</strong>: El motor de anonimización sigue el estándar DICOM oficial para la desidentificación, asegurando que se apliquen prácticas recomendadas reconocidas a todos los atributos relevantes.</li>
<li><strong>Eliminación integral de PII</strong>: El nombre del paciente, ID, fecha de nacimiento, dirección y otros identificadores se manejan según el perfil de confidencialidad configurado.</li>
<li><strong>Reemplazo de UID</strong>: Los UIDs de Estudio, Serie y SOP Instance pueden ser reemplazados por nuevos UIDs internamente consistentes para prevenir la reidentificación mediante referencias cruzadas.</li>
<li><strong>Retención configurable</strong>: Cuando la investigación o las necesidades clínicas lo requieren, categorías específicas de datos (características del paciente, fechas, información del dispositivo) pueden retenerse selectivamente usando los perfiles de opciones estándar.</li>
<li><strong>Proceso auditable</strong>: El enfoque basado en estándares con códigos de acción definidos brinda un proceso de anonimización claro y documentable para auditorías regulatorias.</li>
</ul>

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
