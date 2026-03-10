---
title: Converter DICOM para JSON em C# .NET | Aspose.Medical
weight: 4000
description: Serializar arquivos DICOM para o formato DICOM JSON padrão em C# .NET. Configure o tratamento de números, URIs de dados em massa, saída de palavras‑chave e processamento assíncrono de streams com a API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converter DICOM para JSON em .NET C#" h2="Serializar conjuntos de dados e arquivos DICOM para o Modelo DICOM JSON padrão (PS3.18). Configure o tratamento de números, referências de dados em massa, saída de palavras‑chave e processamento assíncrono baseado em stream." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Modelo DICOM JSON Padrão">}}

<p><strong>Aspose.Medical for .NET</strong> serializa dados DICOM para JSON seguindo o <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>. Este é o padrão oficial para representar conjuntos de dados DICOM em JSON, usado por serviços DICOMweb, recursos FHIR ImagingStudy e APIs de saúde modernas.</p>

<p>No Modelo DICOM JSON, cada atributo é representado por sua tag como a chave JSON (por exemplo, <code>"00100010"</code> para Patient Name), com a Representação de Valor e o array de valores como propriedades aninhadas:</p>

<div class="codeblock" id="code">
 <h3>Formato do Modelo DICOM JSON</h3>
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

{{< blocks/products/pf/feature-page-section h2="Serializar DICOM para JSON em C#">}}

<p>Use a classe <code>DicomJsonSerializer</code> para converter um arquivo ou conjunto de dados DICOM para JSON. A abordagem mais simples produz saída compacta e em conformidade com os padrões:</p>

<div class="codeblock" id="code">
 <h3>Converter DICOM para JSON - C#</h3>
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

<p>Você pode serializar um <code>Dataset</code>, um <code>DicomFile</code> completo (incluindo File Meta Information), ou um array de datasets:</p>

<div class="codeblock" id="code">
 <h3>Serializar DicomFile e arrays de datasets - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialização Baseada em Stream e Assíncrona">}}

<p>Para arquivos DICOM grandes ou cenários de alta taxa de transferência, serialize diretamente para um stream UTF-8 para evitar alocar strings grandes na memória. Métodos síncronos e assíncronos estão disponíveis:</p>

<div class="codeblock" id="code">
 <h3>Serialização de stream e assíncrona - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Opções de Serialização">}}

<p>A classe <code>DicomJsonSerializerOptions</code> controla como os dados DICOM são representados em JSON:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Propriedade</th>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Controla se os VRs numéricos (IS, DS, SV, UV) são gravados como números JSON ou strings</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Usa palavras‑chave DICOM (por exemplo, <code>"PatientName"</code>) em vez de tags (<code>"00100010"</code>) como chaves JSON. Observação: não padrão</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Inclui a palavra‑chave DICOM como um atributo JSON adicional</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Inclui o nome da tag DICOM como um atributo JSON adicional</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Conversor personalizado para gravar dados grandes (por exemplo, dados de pixel) como referências URI</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Carregador personalizado para resolver URIs de dados em massa durante a desserialização</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Configurar opções de serialização - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Tratamento de Números">}}

<p>As Representações de Valor numéricas DICOM (IS, DS, SV, UV) podem ser serializadas como números JSON ou strings JSON. Isto é importante para interoperabilidade, pois algumas implementações DICOM armazenam números com espaços iniciais ou formatação não padrão:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modo</th>
<th>Saída JSON</th>
<th>Caso de Uso</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>Conforme o padrão, ideal para computação. Lança exceção se o valor não puder ser analisado.</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>Preserva a representação original como string. Mais seguro para round‑tripping de dados não padrão.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Manipulação de Dados em Massa">}}

<p>Valores binários grandes (dados de pixel, formas de onda, documentos encapsulados) podem ser tratados como referências de dados em massa em vez de serem inseridos na saída JSON. Isso segue a especificação <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>.</p>

<p>Implemente <code>IBulkDataConverter</code> para externalizar dados grandes durante a serialização, e <code>IBulkDataLoader</code> para resolver URIs durante a desserialização:</p>

<div class="codeblock" id="code">
 <h3>Manipulação personalizada de dados em massa - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Desserializar JSON para DICOM">}}

<p>Parse o JSON DICOM de volta em objetos Dataset ou DicomFile. Suporta entrada como string, entrada de stream e operações assíncronas de stream:</p>

<div class="codeblock" id="code">
 <h3>Desserializar JSON para DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Integração com System.Text.Json">}}

<p>Aspose.Medical fornece <code>DatasetJsonConverter</code> e <code>DicomFileJsonConverter</code> para integração com o pipeline padrão de serialização <code>System.Text.Json</code>. Isso permite que objetos DICOM sejam serializados junto com outros tipos .NET em seu código existente de processamento JSON:</p>

<div class="codeblock" id="code">
 <h3>Integração com System.Text.Json - C#</h3>
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

{{< blocks/products/pf/slr-tab tabTitle="Por que Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de Clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Casos de Sucesso" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
