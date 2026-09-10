---
title: DICOM nach XML konvertieren in C# .NET | Aspose.Medical
weight: 3000
description: Serialisieren Sie DICOM-Datensätze in das standardmäßige DICOM‑XML‑Format in C# .NET. Konfigurieren Sie die Bulk‑Datenverarbeitung, streambasierte Verarbeitung und asynchrone Vorgänge mit der Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM nach XML konvertieren in .NET C#" h2="Serialisieren Sie DICOM‑Datensätze in die standardisierte DICOM‑XML‑Darstellung (PS3.19). Konfigurieren Sie Bulk‑Datenreferenzen, streambasierten Output und asynchrone Verarbeitung mit einer reinen .NET‑Bibliothek." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standardbasierte DICOM‑XML‑Serialisierung">}}

<p><strong>Aspose.Medical for .NET</strong> serialisiert DICOM-Daten nach XML nach dem <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a>. Dies ist der offizielle Standard zur Darstellung von DICOM‑Datensätzen in XML, verwendet von DICOMweb‑Diensten, Integrationsplattformen und Systemen, die eine menschenlesbare, schema‑validierte Repräsentation von Metadaten medizinischer Bildgebung benötigen.</p>

<p>Die Klasse <code>DicomXmlSerializer</code> stellt statische Methoden sowohl für die Serialisierung als auch für die Deserialisierung bereit. Im Gegensatz zu einfachen Tag‑Dump‑Ansätzen entspricht die Ausgabe dem DICOM‑XML‑Schema, bei dem jedes Element mit seinem Tag, VR und korrekt formatierten Werten dargestellt wird &mdash; was eine verlustfreie Rundreise‑Konvertierung zwischen binärem DICOM und XML ermöglicht.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM nach XML serialisieren in C#">}}

<p>Verwenden Sie die Klasse <code>DicomXmlSerializer</code>, um einen DICOM‑Datensatz in einen XML‑String zu konvertieren. Der einfachste Ansatz erzeugt ein standardkonformes XML‑Dokument:</p>

<div class="codeblock" id="code">
 <h3>DICOM nach XML konvertieren - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streambasierte und asynchrone Serialisierung">}}

<p>Für große DICOM‑Dateien oder Hochdurchsatz‑Szenarien serialisieren Sie direkt in einen Stream, um das Anlegen großer Strings im Speicher zu vermeiden. Sowohl synchrone als auch asynchrone Methoden sind verfügbar:</p>

<div class="codeblock" id="code">
 <h3>Synchrone Stream‑Serialisierung - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Asynchrone Stream‑Serialisierung - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Pipeline‑Streaming für große Studien">}}

<p>Komplette Studien müssen nicht komplett im Speicher gehalten werden. <code>DicomXmlSerializer</code> schreibt in einen <code>PipeWriter</code> und liest aus einem <code>PipeReader</code>, sodass das XML während des Flusses erzeugt und konsumiert werden kann und eine Sequenz von Datensätzen einzeln über <code>DeserializeAsyncEnumerable</code> gelesen werden kann. Jede Methode akzeptiert ein <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Serialisieren und deserialisieren über eine Pipe - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Eine Sequenz von Datensätzen einzeln lesen - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialisierungsoptionen">}}

<p>Die Klasse <code>DicomXmlSerializerOptions</code> steuert, wie DICOM‑Daten in XML dargestellt werden. Die primäre Konfiguration bezieht sich auf die Bulk‑Datenverarbeitung für große binäre Werte:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Eigenschaft</th>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Benutzerdefinierter Konverter zum Schreiben großer Daten (z. B. Pixel‑Daten) als BulkData‑URI‑Referenzen anstelle von Inline‑Einbettung</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Benutzerdefinierter Loader zum Auflösen von BulkData‑URIs während der Deserialisierung</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Standard‑Optioneninstanz, die verwendet wird, wenn keine benutzerdefinierten Optionen angegeben sind</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Serialisieren mit benutzerdefinierten Optionen - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk‑Datenverarbeitung">}}

<p>Große binäre Werte (Pixel‑Daten, Waveforms, gekapselte Dokumente) können als BulkData‑URI‑Referenzen externalisiert werden, anstatt sie in die XML‑Ausgabe einzubetten. Dies entspricht der Spezifikation des <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">DICOM PS3.19 BulkData‑Elements</a>.</p>

<p>Implementieren Sie <code>IBulkDataConverter</code>, um große Daten während der Serialisierung zu externalisieren, und <code>IBulkDataLoader</code>, um URIs während der Deserialisierung aufzulösen. Für die üblichen Fälle muss kein Loader geschrieben werden: <code>DefaultBulkDataLoader.Instance</code> löst <code>file</code>-, <code>http</code>- und <code>https</code>-URIs auf und implementiert zudem <code>IAsyncBulkDataLoader</code>, sodass Bulk‑Daten asynchron über die Streaming‑Pfade abgerufen werden.</p>

<div class="codeblock" id="code">
 <h3>Benutzerdefinierte Bulk‑Datenverarbeitung - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="XML nach DICOM deserialisieren">}}

<p>Parsen Sie DICOM‑XML zurück in Dataset‑Objekte. Unterstützt String‑Eingaben, Stream‑Eingaben und asynchrone Vorgänge:</p>

<div class="codeblock" id="code">
 <h3>XML nach DICOM deserialisieren - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="XML‑ vs JSON‑Serialisierung">}}

<p>Aspose.Medical unterstützt sowohl die DICOM‑XML‑ (PS3.19) als auch die DICOM‑JSON‑ (PS3.18) Serialisierung. Beide Formate ermöglichen eine verlustfreie Rundreise‑Konvertierung, bedienen jedoch unterschiedliche Integrationsszenarien:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Funktion</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Standard</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>Schema‑Validierung</td><td>XML‑Schema (XSD) verfügbar</td><td>Kein formales Schema</td></tr>
<tr><td>Am besten geeignet für</td><td>Enterprise‑Integration, HL7 CDA, Audit‑Logs, XDS‑Register</td><td>DICOMweb, REST‑APIs, FHIR ImagingStudy</td></tr>
<tr><td>Menschliche Lesbarkeit</td><td>Ausführlich aber selbsterklärend</td><td>Kompakt und weit verbreitet</td></tr>
<tr><td>Bulk‑Daten</td><td>BulkData‑Element mit URI</td><td>BulkDataURI‑Eigenschaft</td></tr>
<tr><td>Serialisierer‑Klasse</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Quellcode" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑Referenzen" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktunterstützung" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Kostenloser Support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Bezahlter Support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Warum Aspose.Medical für .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundenliste" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Erfolgsgeschichten" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
