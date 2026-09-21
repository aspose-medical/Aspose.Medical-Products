---
title: Converter JSON para DICOM em C# .NET | Aspose.Medical
weight: 6000

description: Crie arquivos DICOM a partir do Modelo JSON DICOM padrão (PS3.18) em C# .NET. Leia JSON de uma string, de um fluxo ou de um pipe, transmita uma sequência de datasets e resolva referências de dados em massa com a API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converter JSON para DICOM em .NET C#" h2="Leia o Modelo JSON DICOM padrão (PS3.18) de volta para datasets e arquivos DICOM. Trabalhe a partir de uma string, de um fluxo ou de um pipe, transmita uma sequência de estudos e resolva referências de dados em massa." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="De JSON DICOM para um arquivo DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> lê o <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>, a representação usada pelos serviços DICOMweb e por sistemas que trocam estudos via HTTP. O que chega como JSON se torna um <code>Dataset</code>, e um <code>Dataset</code> é gravado no disco como um arquivo DICOM.</p>

<p>Esta é a direção reversa da página <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>, e as duas usam a mesma classe, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Criar um arquivo DICOM a partir de JSON - C#</h3>
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

<p>Um dataset que não contém File Meta Information é gravado com a sintaxe de transferência padrão, Implicit VR Little Endian, quando está encapsulado em um <code>DicomFile</code>.</p>

<p>A leitura de DICOM JSON é um recurso licenciado. Sem uma licença on-premise aplicada, o leitor lança uma <code>MedicalApiException</code>, portanto aplique a licença primeiro, como descreve o <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">guia de licenciamento</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Manter o File Meta Information">}}

<p><code>Deserialize</code> retorna apenas o dataset. Quando o documento JSON também contém o grupo File Meta Information, por exemplo porque foi gerado a partir de um arquivo DICOM completo, <code>DeserializeFile</code> retorna um <code>DicomFile</code> com esse grupo intacto, incluindo a sintaxe de transferência declarada pelo arquivo.</p>

<div class="codeblock" id="code">
 <h3>Ler um arquivo DICOM completo a partir de JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Fluxos, pipes e async">}}

<p>Cada ponto de entrada tem uma sobrecarga de stream e uma sobrecarga assíncrona, e as assíncronas também aceitam um <code>PipeReader</code>. Um documento que chega de uma resposta web ou do disco é lido sem ser convertido primeiro em string, o que importa assim que o JSON contém dados de pixel.</p>

<div class="codeblock" id="code">
 <h3>Ler JSON de um stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Uma sequência de datasets, um de cada vez">}}

<p>Uma consulta DICOMweb responde com um array de datasets, e tal documento pode ser grande. <code>DeserializeList</code> lê todo o array na memória; <code>DeserializeAsyncEnumerable</code> fornece um dataset de cada vez, de modo que o documento nunca é mantido completo na memória.</p>

<div class="codeblock" id="code">
 <h3>Transmitir um array de datasets - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Referências de dados em massa">}}

<p>O Modelo JSON DICOM não contém dados de pixel embutidos. Valores grandes são substituídos por um <code>BulkDataURI</code> que aponta para os bytes, mantendo o documento JSON pequeno. Para resolver essas referências durante a leitura, forneça ao serializer um carregador de dados em massa. <code>DefaultBulkDataLoader</code> busca URIs <code>file</code>, <code>http</code> e <code>https</code> sem autenticação; para um arquivo que requer credenciais, implemente <code>IBulkDataLoader</code> ou <code>IAsyncBulkDataLoader</code> você mesmo.</p>

<div class="codeblock" id="code">
 <h3>Resolver BulkDataURI durante a leitura - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Viagem de ida e volta com DICOM para JSON">}}

<p>As duas direções foram projetadas para serem usadas juntas: um estudo sai como JSON, atravessa um serviço web e retorna como um arquivo DICOM. Nada no processo depende de código nativo, portanto a mesma ida e volta funciona no Windows, Linux e macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM para JSON e volta - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Para as opções que controlam a aparência do JSON, consulte a página <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>. O mesmo par existe para XML: <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> e <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>. O <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">guia de serialização JSON</a> cobre toda a API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizado" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentação" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guia do desenvolvedor" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="Referências da API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Suporte ao produto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Suporte gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Suporte pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Por que Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Casos de sucesso" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}