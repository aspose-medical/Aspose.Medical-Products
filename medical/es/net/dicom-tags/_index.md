---
title: Leer y modificar etiquetas DICOM en C# .NET | Aspose.Medical
weight: 15000
description: Lea, agregue, actualice y elimine etiquetas DICOM en C# .NET. Acceda a los metadatos de las etiquetas, trabaje con etiquetas privadas, enumere por grupo y manipule secuencias usando la API de Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Leer y modificar etiquetas DICOM en .NET C#" h2="Acceda, agregue, actualice y elimine elementos de datos DICOM con una API segura por tipo. Trabaje con etiquetas estándar, etiquetas privadas, secuencias y el diccionario completo de etiquetas DICOM." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Acceso a etiquetas DICOM seguro por tipo">}}

<p><strong>Aspose.Medical for .NET</strong> proporciona una API completa y segura por tipo para trabajar con etiquetas DICOM mediante la clase <code>Dataset</code>. Cada elemento de datos DICOM está representado por un <code>Tag</code> &mdash; un par de números de grupo y elemento que identifica de forma única el atributo. La API ofrece acceso fuertemente tipado con métodos genéricos, patrones de obtención seguros que evitan excepciones y compatibilidad con valores únicos, matrices y acceso indexado.</p>

<p>Las etiquetas DICOM comunes están disponibles como campos estáticos en la clase <code>Tag</code> (p. ej., <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), eliminando la necesidad de memorizar los códigos numéricos de las etiquetas. Cada etiqueta contiene <code>TagMetadata</code> con su palabra clave, descripción, Representación de Valor (VR), multiplicidad de valor y estado de obsolescencia.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Leer etiquetas DICOM en C#">}}

<p>La clase <code>Dataset</code> ofrece varios métodos para recuperar valores de etiquetas según sus necesidades &mdash; valor único, todos los valores, acceso indexado o recuperación segura con valores predeterminados:</p>

<div class="codeblock" id="code">
 <h3>Leer valores de etiquetas DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Get a single value (throws if tag is missing or multi-valued)
string patientName = dataset.GetSingleValue&lt;string&gt;(Tag.PatientName);

// Safe retrieval with a default value
string institution = dataset.GetSingleValueOrDefault&lt;string&gt;(
    Tag.InstitutionName, "Unknown");

// Get all values from a multi-valued tag
double[] pixelSpacing = dataset.GetValues&lt;double&gt;(Tag.PixelSpacing);

// Get a value by index (supports negative indexing with ^)
double firstPosition = dataset.GetValue&lt;double&gt;(Tag.ImagePositionPatient, 0);

// Check if a tag exists before accessing it
if (dataset.Contains(Tag.PatientBirthDate))
{
    var birthDate = dataset.GetSingleValue&lt;DateOnly&gt;(Tag.PatientBirthDate);
}

// TryGet pattern for safe access without exceptions
if (dataset.TryGetSingleValue&lt;string&gt;(Tag.PatientID, out var patientId))
{
    // Use patientId
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Agregar y actualizar etiquetas DICOM">}}

<p>Utilice <code>AddOrUpdate</code> para establecer valores de etiquetas. El método crea el elemento si la etiqueta falta, o reemplaza su valor si ya existe. Puede establecer valores únicos, matrices o agregar elementos totalmente tipados:</p>

<div class="codeblock" id="code">
 <h3>Agregar y actualizar etiquetas DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Set a single value
dataset.AddOrUpdate(Tag.PatientName, "John Doe");
dataset.AddOrUpdate(Tag.PatientAge, new Age { Number = 42, Units = Age.Unit.Years });

// Set multiple values on a multi-valued tag
dataset.AddOrUpdate&lt;double&gt;(Tag.PixelSpacing, [0.5, 0.5]);

// Add multiple elements at once using typed element classes
dataset.AddOrUpdateRange(new IElement[]
{
    new PersonName(Tag.PatientName, ["John Doe"]),
    new LongString(Tag.PatientID, ["64AD6A3F"]),
    new Date(Tag.PatientBirthDate, [new DateOnly(2002, 10, 10)])
});

// Save the modified file
dicomFile.Save("updated.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Eliminar etiquetas DICOM">}}

<p>El método <code>Remove</code> permite eliminar una sola etiqueta, varias etiquetas a la vez, o etiquetas que coincidan con un predicado &mdash; útil para eliminar grupos completos de elementos:</p>

<div class="codeblock" id="code">
 <h3>Eliminar etiquetas DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Remove a single tag
dataset.Remove(Tag.PatientAddress);

// Remove multiple tags at once
dataset.Remove(Tag.PatientName, Tag.PatientID, Tag.PatientBirthDate);

// Remove all tags in a specific group (e.g., group 0x0002 - File Meta Information)
dataset.Remove(element => element.Tag.Group == 0x0002);

dicomFile.Save("cleaned.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Enumerar etiquetas por grupo">}}

<p>Las etiquetas DICOM se organizan en grupos &mdash; por ejemplo, el grupo <code>0x0010</code> contiene información del paciente, el grupo <code>0x0008</code> contiene la identificación del estudio, y el grupo <code>0x0028</code> contiene atributos de píxeles de la imagen. Utilice <code>EnumerateGroup</code> para iterar sobre todos los elementos de un grupo específico:</p>

<div class="codeblock" id="code">
 <h3>Enumerar elementos por grupo - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Enumerate all patient-related tags (group 0x0010)
foreach (IElement element in dataset.EnumerateGroup(0x0010))
{
    Tag tag = element.Tag;
    string keyword = tag.Metadata.Keyword;
    string vr = element.ValueRepresentation.ToString();

    Console.WriteLine($"({tag.Group:X4},{tag.Element:X4}) {keyword} [{vr}]");
}

// Iterate over all elements in the dataset
foreach (IElement element in dataset)
{
    // Process each element
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Manipulación de datos a nivel de elemento">}}

<p>Cada elemento DICOM ofrece métodos para leer, agregar, insertar, eliminar y reemplazar valores individuales dentro del elemento. Esto es útil para etiquetas multivalor donde se necesita un control granular sobre la lista de valores:</p>

<div class="codeblock" id="code">
 <h3>Manipular valores de elemento - C#</h3>
 <pre><code class="cs">// Create an element with multiple values
var element = new FloatingPointDouble(
    Tag.TableOfYBreakPoints, [50.1, 100.2, 150.3]);

// Access values by index (supports ^ for indexing from end)
double first = element.Get&lt;double&gt;(0);
double last = element.Get&lt;double&gt;(^1);

// Safe access with default
double safe = element.GetOrDefault&lt;double&gt;(10); // returns 0.0 if out of range

// Get all values or a range
double[] all = element.GetValues&lt;double&gt;();
double[] subset = element.GetValues&lt;double&gt;(1..3);

// Modify element values
element.Add(200.4);
element.AddRange([300.5, 400.6]);
element.Insert(2, 75.0);
element.RemoveAt(0);
element.Replace([1.0, 2.0, 3.0]);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Diccionario de etiquetas y metadatos">}}

<p>La clase <code>TagDictionary</code> ofrece un registro de etiquetas DICOM conocidas junto con sus metadatos &mdash; palabra clave, descripción, Representación de Valor, multiplicidad de valor y estado de obsolescencia. Puede buscar etiquetas por palabra clave o inspeccionar sus metadatos programáticamente:</p>

<div class="codeblock" id="code">
 <h3>Consultar el diccionario de etiquetas - C#</h3>
 <pre><code class="cs">// Look up a tag by its keyword
Tag? tag = TagDictionary.Default.FindByKeyword("PatientName");

// Access tag metadata from the dictionary
TagMetadata meta = TagDictionary.Default[Tag.PatientName];
Console.WriteLine($"Keyword: {meta.Keyword}");
Console.WriteLine($"Name: {meta.Name}");
Console.WriteLine($"VR: {string.Join("/", meta.ValueRepresentations)}");
Console.WriteLine($"VM: {meta.Multiplicity}");
Console.WriteLine($"Retired: {meta.Retired}");

// Access metadata directly from a tag instance
TagMetadata info = Tag.Rows.Metadata;

// Parse a tag from its string representation
Tag parsed = Tag.Parse("0010,0010");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Soporte de etiquetas privadas">}}

<p>DICOM permite a proveedores y organizaciones definir etiquetas privadas para datos propietarios. Aspose.Medical para .NET soporta completamente la lectura, escritura y registro de etiquetas privadas. Puede cargar diccionarios de etiquetas personalizados desde archivos XML para habilitar el manejo adecuado de elementos privados:</p>

<div class="codeblock" id="code">
 <h3>Trabajar con etiquetas privadas - C#</h3>
 <pre><code class="cs">// Load a custom private tag dictionary from XML
// XML format:
// &lt;dictionaries&gt;
//   &lt;dictionary creator="MY ORGANIZATION"&gt;
//     &lt;tag group="3011" element="xx00" vr="SL" vm="1"&gt;Custom Value&lt;/tag&gt;
//   &lt;/dictionary&gt;
// &lt;/dictionaries&gt;
TagDictionary.Default.Load("custom-tag-dictionary.xml");

// Check if a tag is private
Tag tag = Tag.GetOrCreate(0x3011, 0x0010);
bool isPrivate = tag.IsPrivate;

// Get a valid private tag (creates Private Creator element if needed)
Dataset dataset = dicomFile.Dataset;
Tag privateTag = dataset.GetPrivateTag(tag);

// Register a private creator programmatically
PrivateCreator creator = new("MY ORGANIZATION");
TagDictionary privateDictionary =
    TagDictionary.Default.GetPrivateDictionary(creator);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Trabajar con secuencias DICOM">}}

<p>Las secuencias DICOM (tipo VR SQ) contienen conjuntos de datos anidados, formando una estructura de árbol dentro del archivo DICOM. La clase <code>Sequence</code> proporciona acceso tipado a los ítems de la secuencia:</p>

<div class="codeblock" id="code">
 <h3>Leer y crear secuencias DICOM - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Read a sequence element
IElement seqElement = dataset.GetOrDefault(Tag.ReferencedStudySequence);
if (seqElement is Sequence sequence)
{
    // Iterate over sequence items (each item is a Dataset)
    foreach (Dataset item in sequence.Items)
    {
        string uid = item.GetSingleValueOrDefault&lt;string&gt;(
            Tag.ReferencedSOPInstanceUID, "");
    }

    // Access an item by index
    Dataset firstItem = sequence.Get(0);
}

// Create a new sequence
var items = new List&lt;Dataset&gt;
{
    new(new IElement[]
    {
        new UniqueIdentifier(Tag.ReferencedSOPClassUID, ["1.2.840.10008.5.1.4.1.1.2"]),
        new UniqueIdentifier(Tag.ReferencedSOPInstanceUID, ["1.2.3.4.5.6.7.8.9"])
    })
};
dataset.Add(new Sequence(Tag.ReferencedStudySequence, items));</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Grupos de etiquetas DICOM comunes">}}

<p>La tabla siguiente enumera los grupos de etiquetas DICOM de uso frecuente y ejemplos de etiquetas accesibles mediante la clase <code>Tag</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Grupo</th>
<th>Categoría</th>
<th>Etiquetas de ejemplo</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Información de metadatos del archivo</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Identificación del estudio</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Información del paciente</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Parámetros de adquisición</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Posicionamiento de la imagen</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Atributos de píxeles de la imagen</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Datos de píxel</td><td>PixelData</td></tr>
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
