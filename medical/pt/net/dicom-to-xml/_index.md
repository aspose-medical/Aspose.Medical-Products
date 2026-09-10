---
title: Converter DICOM para XML em C# .NET | Aspose.Medical
weight: 3000
description: Serializar conjuntos de dados DICOM para o formato DICOM XML padrão em C# .NET. Configure o tratamento de bulk data, processamento baseado em streams e operações assíncronas com a API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converter DICOM para XML em .NET C#" h2="Serializar conjuntos de dados DICOM para a representação DICOM XML padrão (PS3.19). Configure referências de bulk data, saída baseada em streams e processamento assíncrono com uma biblioteca .NET pura." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Serialização DICOM XML baseada em padrões">}}

<p><strong>Aspose.Medical for .NET</strong> serializa dados DICOM para XML seguindo o <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Modelo DICOM Nativo PS3.19</a>. Este é o padrão oficial para representar conjuntos de dados DICOM em XML, usado por serviços DICOMweb, plataformas de integração e sistemas que exigem uma representação legível por humanos e validada por esquema dos metadados de imagens médicas.</p>

<p>A classe <code>DicomXmlSerializer</code> fornece métodos estáticos tanto para serialização quanto para desserialização. Ao contrário de abordagens simples de despejo de tags, a saída está em conformidade com o esquema DICOM XML, onde cada elemento é representado com sua tag, VR e valores formatados corretamente &mdash; permitindo conversão lossless bidirecional entre DICOM binário e XML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serializar DICOM para XML em C#">}}

<p>Utilize a classe <code>DicomXmlSerializer</code> para converter um conjunto de dados DICOM em uma string XML. A abordagem mais simples produz um documento XML compatível com os padrões:</p>

<div class="codeblock" id="code">
 <h3>Converter DICOM para XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialização baseada em streams e assíncrona">}}

<p>Para arquivos DICOM grandes ou cenários de alta taxa de transferência, serialize diretamente para um stream para evitar a alocação de strings grandes na memória. Métodos síncronos e assíncronos estão disponíveis:</p>

<div class="codeblock" id="code">
 <h3>Serialização síncrona em stream - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Serialização assíncrona em stream - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Streaming em pipeline para estudos grandes">}}

<p>Estudos completos não precisam ser mantidos na memória. <code>DicomXmlSerializer</code> grava em um <code>PipeWriter</code> e lê de um <code>PipeReader</code>, permitindo que o XML seja produzido e consumido conforme flui, e que uma sequência de conjuntos de dados seja lida um de cada vez através de <code>DeserializeAsyncEnumerable</code>. Cada método aceita um <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Serializar e desserializar através de um pipe - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Ler uma sequência de conjuntos de dados um de cada vez - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opções de Serialização">}}

<p>A classe <code>DicomXmlSerializerOptions</code> controla como os dados DICOM são representados em XML. A configuração principal envolve o tratamento de bulk data para valores binários grandes:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Propriedade</th>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Conversor personalizado para gravar dados grandes (ex., dados de pixel) como referências URI BulkData ao invés de inseri‑los inline</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Carregador personalizado para resolver URIs BulkData durante a desserialização</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Instância padrão de opções usada quando nenhuma opção personalizada é fornecida</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Serializar com opções personalizadas - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Manipulação de Bulk Data">}}

<p>Valores binários grandes (dados de pixel, formas de onda, documentos encapsulados) podem ser externalizados como referências URI BulkData ao invés de serem incorporados na saída XML. Isso segue a especificação do <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">elemento BulkData do DICOM PS3.19</a>.</p>

<p>Implemente <code>IBulkDataConverter</code> para externalizar dados grandes durante a serialização, e <code>IBulkDataLoader</code> para resolver URIs durante a desserialização. Na maioria dos casos não é necessário escrever um carregador: <code>DefaultBulkDataLoader.Instance</code> resolve URIs <code>file</code>, <code>http</code> e <code>https</code>, e também implementa <code>IAsyncBulkDataLoader</code>, de modo que os bulk data são obtidos de forma assíncrona nos caminhos de streaming.</p>

<div class="codeblock" id="code">
 <h3>Manipulação personalizada de bulk data - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Desserializar XML para DICOM">}}

<p>Analisar XML DICOM de volta para objetos Dataset. Suporta entrada de string, entrada de stream e operações assíncronas:</p>

<div class="codeblock" id="code">
 <h3>Desserializar XML para DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Serialização XML vs JSON">}}

<p>Aspose.Medical suporta tanto a serialização DICOM XML (PS3.19) quanto DICOM JSON (PS3.18). Ambos os formatos oferecem conversão lossless bidirecional, mas atendem a diferentes cenários de integração:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Recurso</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Padrão</td><td>PS3.19 (Modelo DICOM Nativo)</td><td>PS3.18 (Modelo DICOM JSON)</td></tr>
<tr><td>Validação de esquema</td><td>XML Schema (XSD) disponível</td><td>Sem esquema formal</td></tr>
<tr><td>Melhor para</td><td>Integração empresarial, HL7 CDA, logs de auditoria, registros XDS</td><td>DICOMweb, APIs REST, FHIR ImagingStudy</td></tr>
<tr><td>Legibilidade humana</td><td>Verboso porém auto‑descritivo</td><td>Compacto e amplamente suportado</td></tr>
<tr><td>Dados em massa</td><td>Elemento BulkData com URI</td><td>Propriedade BulkDataURI</td></tr>
<tr><td>Classe serializadora</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizado" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentação" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Código Fonte" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referências de API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Suporte ao Produto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Suporte Gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Suporte Pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Por que o Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de Clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Casos de Sucesso" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
