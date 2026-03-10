---
title: Anonimize Arquivos DICOM em C# .NET | Aspose.Medical
weight: 1000
description: Anonimize arquivos DICOM em C# .NET usando perfis de confidencialidade configuráveis. Remova informações de identificação do paciente (PII), garanta conformidade com HIPAA e GDPR com a API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Anonimize Arquivos DICOM em .NET C#" h2="Remova informações de identificação do paciente de arquivos DICOM usando perfis de confidencialidade DICOM PS 3.15. Garanta conformidade com HIPAA e GDPR com uma biblioteca .NET pura." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Anonimização DICOM Baseada em Padrões">}}

<p><strong>Aspose.Medical for .NET</strong> fornece uma API abrangente de anonimização DICOM baseada nos <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">Perfis de Confidencialidade DICOM PS 3.15</a>. A API permite que desenvolvedores de saúde removam ou modifiquem informações de identificação do paciente (PII) de arquivos DICOM preservando o valor clínico dos dados de imagem. Ao contrário de abordagens simples de remoção de tags, o Aspose.Medical segue o padrão DICOM oficial para garantir a deidentificação adequada.</p>

<p>A classe <code>Anonymizer</code> trabalha com objetos <code>ConfidentialityProfile</code> que definem exatamente como cada atributo DICOM deve ser tratado &mdash; se deve ser removido, substituído por um valor fictício, limpo ou mantido inalterado. Essa abordagem garante anonimização consistente e auditável que atende aos requisitos regulatórios.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonimize um Arquivo DICOM em C#">}}

<p>A maneira mais simples de anonimizar um arquivo DICOM é usar o perfil de confidencialidade padrão. Isso aplica o Perfil Básico DICOM PS 3.15, que trata todos os atributos padrão de identificação do paciente:</p>

<div class="codeblock" id="code">
 <h3>Anonimização DICOM Básica - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>O método <code>Anonymize</code> é <strong>não destrutivo</strong> &mdash; ele clona o conjunto de dados original e devolve uma nova cópia anonimizada. O arquivo DICOM original permanece inalterado, o que é essencial para trilhas de auditoria e fluxos de trabalho de integridade de dados.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Substituição Personalizada da Identidade do Paciente">}}

<p>Em muitos fluxos de trabalho, é necessário substituir as informações do paciente por valores de alias específicos em vez de simplesmente removê‑las. A classe <code>ConfidentialityProfile</code> permite definir valores de substituição para o nome do paciente e o ID do paciente:</p>

<div class="codeblock" id="code">
 <h3>Anonimize com identidade de paciente personalizada - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Anonimização In‑Place">}}

<p>Para cenários em que a eficiência de memória é importante ou não é necessário manter os dados originais, a API fornece anonimização in-place que modifica o conjunto de dados diretamente:</p>

<div class="codeblock" id="code">
 <h3>Anonimização DICOM In-place - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Ambos os métodos <code>Anonymize</code> e <code>AnonymizeInPlace</code> também aceitam um objeto <code>Dataset</code> diretamente, proporcionando controle total sobre qual conjunto de dados será processado.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opções Configuráveis de Perfil de Confidencialidade">}}

<p>O padrão DICOM PS 3.15 define um <strong>Perfil Básico</strong> junto com vários perfis opcionais que controlam como categorias específicas de dados são tratadas durante a anonimização. Você pode combinar essas opções usando o enum <code>ConfidentialityProfileOptions</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Opção</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>O Perfil de Confidencialidade de Nível de Aplicação Básico padrão DICOM PS 3.15</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Preservar atributos privados seguros durante a anonimização</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Manter os UIDs DICOM originais (Study, Series, Instance) inalterados</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Preservar atributos de identificação do dispositivo (manufacturer, model, station name)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Preservar atributos de identificação da instituição (name, address, department)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Preservar características do paciente (age, sex, size, weight) para fins de pesquisa</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Preservar valores completos de data e hora para estudos longitudinais</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Preservar datas modificadas para estudos longitudinais</td></tr>
<tr><td><code>CleanDesc</code></td><td>Limpar descrições de texto que podem conter informações identificadoras</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Limpar conteúdo estruturado que pode conter informações identificadoras</td></tr>
<tr><td><code>CleanGraph</code></td><td>Limpar dados gráficos que podem conter informações de paciente incorporadas</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Anonimize com opções de perfil específicas - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Códigos de Ação para Manipulação de Atributos">}}

<p>Cada atributo em um perfil de confidencialidade recebe um código de ação que define como ele deve ser processado durante a anonimização. Estes seguem o padrão DICOM PS 3.15 Tabela E.1-1a:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Código</th>
<th>Ação</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Substituir por fictício</td><td>Substituir por um valor de comprimento não zero consistente com o VR</td></tr>
<tr><td><strong>Z</strong></td><td>Comprimento zero ou fictício</td><td>Substituir por um valor de comprimento zero ou um valor fictício consistente com o VR</td></tr>
<tr><td><strong>X</strong></td><td>Remover</td><td>Remover o atributo completamente do conjunto de dados</td></tr>
<tr><td><strong>K</strong></td><td>Manter</td><td>Manter o atributo inalterado (para não‑sequências) ou limpo (para sequências)</td></tr>
<tr><td><strong>C</strong></td><td>Limpar</td><td>Substituir por um valor limpo de significado similar consistente com o VR</td></tr>
<tr><td><strong>U</strong></td><td>Substituir UID</td><td>Substituir por um novo UID que seja internamente consistente dentro do conjunto de instâncias</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Perfis Personalizados de CSV, JSON e XML">}}

<p>Para casos de uso avançados, você pode definir perfis de anonimização personalizados carregando regras de arquivos CSV, JSON ou XML externos. Isso permite ajustar o comportamento de anonimização às políticas organizacionais específicas ou aos requisitos regulatórios:</p>

<div class="codeblock" id="code">
 <h3>Carregar perfil de anonimização a partir de CSV - C#</h3>
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
 <h3>Carregar perfil de anonimização a partir de JSON - C#</h3>
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

<p>Perfis personalizados também podem ser carregados de arquivos XML (<code>LoadFromXmlFile</code>), streams (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) ou diretamente de strings (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Conformidade com HIPAA e GDPR">}}

<p>Arquivos DICOM contêm rotineiramente Informações de Saúde Protegidas (PHI) conforme definido pela HIPAA e dados pessoais conforme definido pelo GDPR. Aspose.Medical for .NET ajuda você a atender aos requisitos regulatórios ao fornecer:</p>

<ul>
<li><strong>Conformidade com DICOM PS 3.15</strong>: O mecanismo de anonimização segue o padrão DICOM oficial para desidentificação, garantindo que as melhores práticas reconhecidas sejam aplicadas a todos os atributos relevantes.</li>
<li><strong>Remoção abrangente de PII</strong>: Nome do paciente, ID, data de nascimento, endereço e outros identificadores são tratados de acordo com o perfil de confidencialidade configurado.</li>
<li><strong>Substituição de UID</strong>: UIDs de Estudo, Série e Instância SOP podem ser substituídos por novos UIDs internamente consistentes para impedir a reidentificação por meio de referências cruzadas.</li>
<li><strong>Retenção configurável</strong>: Quando a pesquisa ou necessidades clínicas exigirem, categorias específicas de dados (características do paciente, datas, informações do dispositivo) podem ser retidas seletivamente usando os perfis de opção padrão.</li>
<li><strong>Processo auditável</strong>: A abordagem baseada em padrões com códigos de ação definidos fornece um processo de anonimização claro e documentável para auditorias regulatórias.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizagem" tabId="resources" >}}
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
