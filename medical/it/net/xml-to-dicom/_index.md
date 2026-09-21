---
title: Converti XML in DICOM in C# .NET | Aspose.Medical
weight: 5000

description: Crea file DICOM dal Native DICOM Model XML della PS3.19 in C# .NET. Leggi XML da una stringa, da uno stream o da un pipe, trasmetti documenti consecutivi e risolvi i riferimenti a bulk data con l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converti XML in DICOM in .NET C#" h2="Leggi il Native DICOM Model XML della PS3.19 nuovamente in dataset e file DICOM. Lavora da una stringa, da uno stream o da un pipe, trasmetti documenti consecutivi e risolvi i riferimenti a bulk data." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="XML Standard del Native DICOM Model">}}

<p><strong>Aspose.Medical for .NET</strong> legge il <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a> definito in DICOM PS3.19. Questa è la rappresentazione XML scritta nello standard stesso, non un formato inventato da Aspose, il che lo rende utile per l'integrazione: un sistema che già scambia DICOM come XML produce documenti che questa libreria accetta.</p>

<p>La radice del documento è <code>NativeDicomModel</code>, e ogni attributo è un elemento <code>DicomAttribute</code> che trasporta il suo tag, la rappresentazione del valore e la keyword:</p>

<div class="codeblock" id="code">
 <h3>Formato del Native DICOM Model</h3>
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

<p>Questa pagina è la direzione inversa di <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>, e entrambi usano la stessa classe, <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Crea un file DICOM da XML in C#">}}

<p><code>Deserialize</code> converte un documento in un <code>Dataset</code>, e un dataset viene scritto su disco come file DICOM.</p>

<div class="codeblock" id="code">
 <h3>Crea un file DICOM da XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Il Native DICOM Model non contiene il gruppo File Meta Information, quindi la transfer syntax non fa parte del documento. Un dataset avvolto in un <code>DicomFile</code> viene scritto con la transfer syntax predefinita, Implicit VR Little Endian. Per memorizzare il file con un'altra transfer syntax, è necessario transcodificarlo, come mostrato nella pagina di <a href="/medical/net/dicom-transfer-syntax-conversion/">conversione della transfer syntax</a>.</p>

<p>La lettura di DICOM XML è una funzionalità a licenza. Senza una licenza on-premise applicata, il lettore genera una <code>MedicalApiException</code>, quindi è necessario applicare la licenza prima, come descritto nella <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">guida alla licenza</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stream, pipe e asincrono">}}

<p>Ogni punto di ingresso ha una sovraccarico per lo stream e una sovraccarico asincrona, e le versioni asincrone accettano anche un <code>PipeReader</code>. Un documento che proviene da una risposta web viene analizzato mentre viene letto, senza prima convertirlo in una stringa.</p>

<div class="codeblock" id="code">
 <h3>Leggi XML da uno stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Documenti consecutivi in un unico stream">}}

<p>Una esportazione da un altro sistema spesso contiene un elemento <code>NativeDicomModel</code> dopo l’altro in un singolo stream. <code>DeserializeAsyncEnumerable</code> restituisce un dataset per elemento, nell’ordine di input, così lo stream viene elaborato senza essere caricato interamente in memoria. Gli elementi si susseguono direttamente: una dichiarazione XML è consentita solo all’inizio, come in qualsiasi input XML.</p>

<div class="codeblock" id="code">
 <h3>Stream di documenti consecutivi - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Riferimenti a bulk data">}}

<p>Valori di grandi dimensioni come i dati dei pixel non vengono scritti inline. Appaiono come un elemento <code>BulkData</code> con un URI che punta ai byte, mantenendo il documento di dimensioni contenute. Per risolvere questi riferimenti durante la lettura, fornire al serializer un bulk data loader. <code>DefaultBulkDataLoader</code> recupera gli URI <code>file</code>, <code>http</code> e <code>https</code> senza autenticazione; per un archivio che richiede credenziali, implementare <code>IBulkDataLoader</code> o <code>IAsyncBulkDataLoader</code> autonomamente.</p>

<div class="codeblock" id="code">
 <h3>Risolvi i bulk data durante la lettura - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Round trip con DICOM a XML">}}

<p>Le due direzioni sono pensate per essere usate insieme: uno studio viene esportato come XML, passa attraverso un sistema che utilizza XML e ritorna come file DICOM. Tutto è gestito in .NET, quindi lo stesso round trip funziona su Windows, Linux e macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM to XML e ritorno - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Per le opzioni che controllano la struttura dell'XML, consultate la pagina <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>. La stessa coppia esiste per JSON: <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> e <a href="/medical/net/json-to-dicom/">JSON to DICOM</a>. La <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">guida alla serializzazione</a> copre tutta l'API.</p>

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