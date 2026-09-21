---
title: Converti JSON in DICOM in C# .NET | Aspose.Medical
weight: 6000

description: Crea file DICOM dal modello standard DICOM JSON (PS3.18) in C# .NET. Leggi JSON da una stringa, da uno stream o da una pipe, trasmetti una sequenza di dataset e risolvi i riferimenti a bulk data con l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converti JSON in DICOM in .NET C#" h2="Leggi il modello standard DICOM JSON (PS3.18) ritornando a dataset e file DICOM. Lavora da una stringa, da uno stream o da una pipe, trasmetti una sequenza di studi e risolvi i riferimenti a bulk data." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Da JSON DICOM a un file DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> legge il <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">modello DICOM PS3.18 JSON</a>, la rappresentazione usata dai servizi DICOMweb e dai sistemi che scambiano studi via HTTP. Ciò che arriva come JSON diventa un <code>Dataset</code>, e un <code>Dataset</code> viene scritto su disco come file DICOM.</p>

<p>Questa è la direzione inversa della pagina <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>, e le due usano la stessa classe, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Crea un file DICOM da JSON - C#</h3>
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

<p>Un dataset che non contiene File Meta Information viene scritto con la sintassi di trasferimento predefinita, Implicit VR Little Endian, quando è avvolto in un <code>DicomFile</code>.</p>

<p>La lettura di DICOM JSON è una funzionalità con licenza. Senza una licenza on-premise applicata, il lettore genera una <code>MedicalApiException</code>, quindi applica prima la licenza, come descritto nella <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">guida alla licenza</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Mantieni il File Meta Information">}}

<p><code>Deserialize</code> restituisce solo il dataset. Quando il documento JSON contiene anche il gruppo File Meta Information, ad esempio perché è stato prodotto da un file DICOM completo, <code>DeserializeFile</code> restituisce un <code>DicomFile</code> con quel gruppo intatto, inclusa la sintassi di trasferimento dichiarata dal file.</p>

<div class="codeblock" id="code">
 <h3>Leggi un file DICOM completo da JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stream, pipe e asincrono">}}

<p>Ogni punto di ingresso dispone di un overload per lo stream e di uno overload asincrono, e le versioni asincrone accettano anche un <code>PipeReader</code>. Un documento che arriva da una risposta web o dal disco viene letto senza essere prima trasformato in una stringa, il che è importante non appena il JSON contiene dati di pixel.</p>

<div class="codeblock" id="code">
 <h3>Leggi JSON da uno stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Una sequenza di dataset, uno alla volta">}}

<p>Una query DICOMweb restituisce un array di dataset, e tale documento può essere grande. <code>DeserializeList</code> legge l'intero array in memoria; <code>DeserializeAsyncEnumerable</code> restituisce un dataset alla volta, così il documento non viene mai caricato interamente.</p>

<div class="codeblock" id="code">
 <h3>Trasmetti un array di dataset - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Riferimenti a bulk data">}}

<p>Il modello DICOM JSON non include i dati dei pixel inline. Valori grandi sono sostituiti da un <code>BulkDataURI</code> che punta ai byte, mantenendo il documento JSON di dimensioni ridotte. Per risolvere questi riferimenti durante la lettura, fornisci al serializer un bulk data loader. <code>DefaultBulkDataLoader</code> recupera URI <code>file</code>, <code>http</code> e <code>https</code> senza autenticazione; per un archivio che richiede credenziali, implementa tu stesso <code>IBulkDataLoader</code> o <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Risolvi BulkDataURI durante la lettura - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Round trip con DICOM a JSON">}}

<p>Le due direzioni sono pensate per essere usate insieme: uno studio parte come JSON, attraversa un servizio web e ritorna come file DICOM. Nulla del processo dipende da codice nativo, quindi lo stesso round trip funziona su Windows, Linux e macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM a JSON e ritorno - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Per le opzioni che controllano l'aspetto del JSON, consulta la pagina <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>. La stessa coppia esiste per XML: <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> e <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>. La <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">guida alla serializzazione JSON</a> copre l'intera API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Risorse di apprendimento" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentazione" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Guida per sviluppatori" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="Riferimenti API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Supporto prodotto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Supporto gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Supporto a pagamento" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Perché Aspose.Medical per .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Elenco clienti" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Storie di successo" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}