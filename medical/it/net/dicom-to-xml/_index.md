---
title: Converti DICOM in XML in C# .NET | Aspose.Medical
weight: 3000
description: Serializza i dataset DICOM nel formato standard DICOM XML in C# .NET. Configura la gestione dei bulk data, l'elaborazione basata su stream e le operazioni asincrone con l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converti DICOM in XML in .NET C#" h2="Serializza i dataset DICOM nella rappresentazione standard DICOM XML (PS3.19). Configura i riferimenti ai bulk data, l'output basato su stream e l'elaborazione asincrona con una libreria .NET pura." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Serializzazione DICOM XML basata su standard">}}

<p><strong>Aspose.Medical for .NET</strong> serializza i dati DICOM in XML seguendo il <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Modello DICOM Nativo PS3.19</a>. Questo è lo standard ufficiale per rappresentare i dataset DICOM in XML, utilizzato dai servizi DICOMweb, dalle piattaforme di integrazione e dai sistemi che richiedono una rappresentazione leggibile dall'uomo e convalidata dallo schema dei metadati di imaging medico.</p>

<p>La classe <code>DicomXmlSerializer</code> fornisce metodi statici sia per la serializzazione che per la deserializzazione. A differenza degli approcci di semplice dump dei tag, l'output è conforme allo schema DICOM XML dove ogni elemento è rappresentato con il suo tag, VR e valori correttamente formattati &mdash; consentendo una conversione senza perdita di dati in entrambe le direzioni tra DICOM binario e XML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serializza DICOM in XML in C#">}}

<p>Utilizza la classe <code>DicomXmlSerializer</code> per convertire un dataset DICOM in una stringa XML. L'approccio più semplice produce un documento XML conforme agli standard:</p>

<div class="codeblock" id="code">
 <h3>Converti DICOM in XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serializzazione basata su stream e asincrona">}}

<p>Per file DICOM di grandi dimensioni o scenari ad alto throughput, serializza direttamente su uno stream per evitare di allocare grandi stringhe in memoria. Sono disponibili sia metodi sincroni che asincroni:</p>

<div class="codeblock" id="code">
 <h3>Serializzazione sincrona su stream - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Serializzazione asincrona su stream - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Streaming a pipeline per studi di grandi dimensioni">}}

<p>Gli studi completi non devono essere mantenuti in memoria. <code>DicomXmlSerializer</code> scrive su un <code>PipeWriter</code> e legge da un <code>PipeReader</code>, così l'XML può essere prodotto e consumato al volo, e una sequenza di dataset può essere letta uno alla volta tramite <code>DeserializeAsyncEnumerable</code>. Ogni metodo accetta un <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Serializza e deserializza tramite una pipe - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Leggi una sequenza di dataset uno alla volta - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opzioni di serializzazione">}}

<p>La classe <code>DicomXmlSerializerOptions</code> controlla come i dati DICOM sono rappresentati in XML. La configurazione principale riguarda la gestione dei bulk data per valori binari di grandi dimensioni:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Proprietà</th>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Convertitore personalizzato per scrivere dati di grandi dimensioni (ad es., dati pixel) come riferimenti URI BulkData invece di includere inline</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Caricatore personalizzato per risolvere gli URI BulkData durante la deserializzazione</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Istanza di opzioni predefinite utilizzata quando non vengono fornite opzioni personalizzate</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Serializza con opzioni personalizzate - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gestione dei Bulk Data">}}

<p>Valori binari di grandi dimensioni (dati pixel, forme d'onda, documenti incapsulati) possono essere esternalizzati come riferimenti URI BulkData invece di essere inseriti inline nell'output XML. Questo segue la specifica dell'<a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">elemento BulkData DICOM PS3.19</a>.</p>

<p>Implementa <code>IBulkDataConverter</code> per esternalizzare i dati di grandi dimensioni durante la serializzazione, e <code>IBulkDataLoader</code> per risolvere gli URI durante la deserializzazione. Nei casi più comuni non è necessario scrivere alcun loader: <code>DefaultBulkDataLoader.Instance</code> risolve gli URI <code>file</code>, <code>http</code> e <code>https</code>, e implementa anche <code>IAsyncBulkDataLoader</code>, così i bulk data vengono recuperati in modo asincrono nei percorsi di streaming.</p>

<div class="codeblock" id="code">
 <h3>Gestione personalizzata dei bulk data - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Deserializza XML in DICOM">}}

<p>Analizza l'XML DICOM per ricreare oggetti Dataset. Supporta input da stringa, input da stream e operazioni asincrone:</p>

<div class="codeblock" id="code">
 <h3>Deserializza XML in DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Serializzazione XML vs JSON">}}

<p>Aspose.Medical supporta sia la serializzazione DICOM XML (PS3.19) sia DICOM JSON (PS3.18). Entrambi i formati offrono una conversione round-trip senza perdita di dati, ma rispondono a scenari di integrazione differenti:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Caratteristica</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Standard</td><td>PS3.19 (Modello DICOM Nativo)</td><td>PS3.18 (Modello DICOM JSON)</td></tr>
<tr><td>Validazione schema</td><td>Schema XML (XSD) disponibile</td><td>Nessuno schema formale</td></tr>
<tr><td>Ideale per</td><td>Integrazione enterprise, HL7 CDA, log di audit, registri XDS</td><td>DICOMweb, API REST, FHIR ImagingStudy</td></tr>
<tr><td>Leggibilità umana</td><td>Verboso ma auto-descrivente</td><td>Compatto e ampiamente supportato</td></tr>
<tr><td>Bulk data</td><td>Elemento BulkData con URI</td><td>Proprietà BulkDataURI</td></tr>
<tr><td>Classe serializzatore</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Risorse di apprendimento" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentazione" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Codice sorgente" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Riferimenti API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Supporto prodotto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Supporto gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Supporto a pagamento" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Perché scegliere Aspose.Medical per .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Elenco clienti" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Storie di successo" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
