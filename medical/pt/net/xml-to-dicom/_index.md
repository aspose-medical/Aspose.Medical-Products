---
title: Converter XML para DICOM em C# .NET | Aspose.Medical
weight: 5000

description: Crie arquivos DICOM a partir do XML do Native DICOM Model da PS3.19 em C# .NET. Leia XML de uma string, de um stream ou de um pipe, transmita documentos consecutivos e resolva referências de bulk data com a API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converter XML para DICOM em .NET C#" h2="Leia o XML do Native DICOM Model da PS3.19 de volta em datasets e arquivos DICOM. Trabalhe a partir de uma string, de um stream ou de um pipe, transmita documentos consecutivos e resolva referências de bulk data." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="XML Padrão do Native DICOM Model">}}

<p><strong>Aspose.Medical for .NET</strong> lê o <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a> definido no DICOM PS3.19. Esta é a representação XML escrita no próprio padrão, não um formato inventado pela Aspose, o que a torna útil para integração: um sistema que já troca DICOM como XML produz documentos que esta biblioteca aceita.</p>

<p>O elemento raiz do documento é <code>NativeDicomModel</code>, e cada atributo é um elemento <code>DicomAttribute</code> que contém sua tag, representação de valor e palavra‑chave:</p>

<div class="codeblock" id="code">
 <h3>Formato do Native DICOM Model</h3>
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

<p>Esta página representa a direção inversa de <a href="/medical/net/dicom-to-xml/">DICOM para XML</a>, e ambas utilizam a mesma classe, <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Criar um arquivo DICOM a partir de XML em C#">}}

<p><code>Deserialize</code> converte um documento em um <code>Dataset</code>, e um dataset é gravado no disco como um arquivo DICOM.</p>

<div class="codeblock" id="code">
 <h3>Criar um arquivo DICOM a partir de XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>O Native DICOM Model não possui o grupo File Meta Information, portanto a transfer syntax não faz parte do documento. Um dataset encapsulado em um <code>DicomFile</code> é gravado com a transfer syntax padrão, Implicit VR Little Endian. Para armazenar o arquivo com outra transfer syntax, transcode‑o, conforme mostra a página de <a href="/medical/net/dicom-transfer-syntax-conversion/">conversão de transfer syntax</a>.</p>

<p>A leitura de DICOM XML é um recurso licenciado. Sem uma licença local aplicada, o leitor lança uma <code>MedicalApiException</code>, portanto aplique a licença primeiro, conforme descrito no <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">guia de licenciamento</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streams, pipes e async">}}

<p>Todo ponto de entrada possui uma sobrecarga para stream e uma sobrecarga assíncrona, e as assíncronas também aceitam um <code>PipeReader</code>. Um documento que chega de uma resposta web é analisado à medida que é lido, sem necessidade de convertê‑lo primeiro em string.</p>

<div class="codeblock" id="code">
 <h3>Ler XML de um stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Documentos consecutivos em um único stream">}}

<p>Uma exportação de outro sistema frequentemente contém um elemento <code>NativeDicomModel</code> após outro em um único stream. <code>DeserializeAsyncEnumerable</code> gera um dataset por elemento, na ordem de entrada, de modo que o stream é processado sem ser mantido na memória. Os elementos seguem‑se diretamente: uma declaração XML é permitida apenas no início, como em qualquer entrada XML.</p>

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

{{< blocks/products/pf/feature-page-section h2="Referências de bulk data">}}

<p>Valores grandes, como dados de pixel, não são gravados inline. Eles aparecem como um elemento <code>BulkData</code> com um URI que aponta para os bytes, mantendo o documento pequeno. Para resolver essas referências durante a leitura, forneça ao serializador um carregador de bulk data. <code>DefaultBulkDataLoader</code> obtém URIs <code>file</code>, <code>http</code> e <code>https</code> sem autenticação; para um arquivo que requer credenciais, implemente <code>IBulkDataLoader</code> ou <code>IAsyncBulkDataLoader</code> você mesmo.</p>

<div class="codeblock" id="code">
 <h3>Resolver bulk data durante a leitura - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Viagem de ida e volta com DICOM para XML">}}

<p>As duas direções são projetadas para serem usadas em conjunto: um estudo sai como XML, passa por um sistema que entende XML e retorna como um arquivo DICOM. Tudo é gerenciado em .NET, portanto a mesma viagem de ida e volta funciona no Windows, Linux e macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM para XML e volta - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Para as opções que controlam a aparência do XML, veja a página <a href="/medical/net/dicom-to-xml/">DICOM para XML</a>. O mesmo par existe para JSON: <a href="/medical/net/dicom-to-json/">DICOM para JSON</a> e <a href="/medical/net/json-to-dicom/">JSON para DICOM</a>. O <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">guia de serialização</a> cobre toda a API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizado" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentação" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guia do Desenvolvedor" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
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