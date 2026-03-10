---
title: Ler & Modificar Tags DICOM em C# .NET | Aspose.Medical
weight: 15000
description: Leia, adicione, atualize e remova tags DICOM em C# .NET. Acesse metadados de tags, trabalhe com tags privadas, enumere por grupo e manipule sequências usando a API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Ler & Modificar Tags DICOM em .NET C#" h2="Acesse, adicione, atualize e remova elementos de dados DICOM com uma API type-safe. Trabalhe com tags padrão, tags privadas, sequências e o dicionário completo de tags DICOM." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Acesso a Tags DICOM Type-Safe">}}

<p><strong>Aspose.Medical for .NET</strong> fornece uma API abrangente e type-safe para trabalhar com tags DICOM por meio da classe <code>Dataset</code>. Cada elemento de dados DICOM é representado por um <code>Tag</code> &mdash; um par de números de grupo e elemento que identifica exclusivamente o atributo. A API oferece acesso fortemente tipado com métodos genéricos, padrões de recuperação seguros que evitam exceções e suporte a valores únicos, arrays e acesso indexado.</p>

<p>Tags DICOM comuns estão disponíveis como campos estáticos na classe <code>Tag</code> (por exemplo, <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), eliminando a necessidade de memorizar códigos numéricos de tags. Cada tag contém <code>TagMetadata</code> com sua palavra‑chave, descrição, Value Representation (VR), multiplicidade de valor e status de descontinuação.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ler Tags DICOM em C#">}}

<p>A classe <code>Dataset</code> oferece múltiplos métodos para recuperar valores de tags conforme suas necessidades &mdash; valor único, todos os valores, acesso indexado ou recuperação segura com valores padrão:</p>

<div class="codeblock" id="code">
 <h3>Ler valores de tags DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Adicionar e Atualizar Tags DICOM">}}

<p>Use <code>AddOrUpdate</code> para definir valores de tags. O método cria o elemento se a tag estiver ausente, ou substitui seu valor se já existir. Você pode definir valores únicos, arrays ou adicionar elementos totalmente tipados:</p>

<div class="codeblock" id="code">
 <h3>Adicionar e atualizar tags DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Remover Tags DICOM">}}

<p>O método <code>Remove</code> suporta a remoção de uma única tag, múltiplas tags de uma vez, ou tags que correspondam a um predicado &mdash; útil para eliminar grupos inteiros de elementos:</p>

<div class="codeblock" id="code">
 <h3>Remover tags DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Enumerar Tags por Grupo">}}

<p>As tags DICOM são organizadas em grupos &mdash; por exemplo, o grupo <code>0x0010</code> contém informações do paciente, o grupo <code>0x0008</code> contém identificação do estudo e o grupo <code>0x0028</code> contém atributos de pixels da imagem. Use <code>EnumerateGroup</code> para iterar sobre todos os elementos de um grupo específico:</p>

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

{{< blocks/products/pf/feature-page-section h2="Manipulação de Dados a Nível de Elemento">}}

<p>Cada elemento DICOM fornece métodos para ler, adicionar, inserir, remover e substituir valores individuais dentro do elemento. Isso é útil para tags multivaloradas onde é necessário controle granular sobre a lista de valores:</p>

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

{{< blocks/products/pf/feature-page-section h2="Dicionário de Tags e Metadados">}}

<p>A classe <code>TagDictionary</code> fornece um registro das tags DICOM conhecidas juntamente com seus metadados &mdash; palavra‑chave, descrição, Value Representation, multiplicidade de valor e status de descontinuação. Você pode buscar tags por palavra‑chave ou inspecionar seus metadados programaticamente:</p>

<div class="codeblock" id="code">
 <h3>Consultar o dicionário de tags - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Suporte a Tags Privadas">}}

<p>DICOM permite que fornecedores e organizações definam tags privadas para dados proprietários. Aspose.Medical for .NET oferece suporte completo à leitura, gravação e registro de tags privadas. Você pode carregar dicionários de tags personalizados a partir de arquivos XML para habilitar o tratamento adequado de elementos privados:</p>

<div class="codeblock" id="code">
 <h3>Trabalhar com tags privadas - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Trabalhar com Sequências DICOM">}}

<p>Sequências DICOM (tipo VR SQ) contêm datasets aninhados, formando uma estrutura em árvore dentro do arquivo DICOM. A classe <code>Sequence</code> fornece acesso tipado aos itens de sequência:</p>

<div class="codeblock" id="code">
 <h3>Ler e criar sequências DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Grupos de Tags DICOM Comuns">}}

<p>A tabela a seguir lista grupos de tags DICOM frequentemente usados e tags de exemplo acessíveis através da classe <code>Tag</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Grupo</th>
<th>Categoria</th>
<th>Tags de Exemplo</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Informação de Metadados do Arquivo</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Identificação do Estudo</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Informação do Paciente</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Parâmetros de Aquisição</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Posicionamento da Imagem</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Atributos de Pixels da Imagem</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Dados de Pixels</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizado" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentação" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Código Fonte" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referências da API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Suporte ao Produto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Suporte Gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Suporte Pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Por que Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de Clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Casos de Sucesso" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
