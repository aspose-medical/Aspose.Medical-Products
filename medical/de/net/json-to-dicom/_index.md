---
title: JSON nach DICOM konvertieren in C# .NET | Aspose.Medical
weight: 6000

description: Erstellen Sie DICOM‑Dateien aus dem standardisierten DICOM‑JSON‑Modell (PS3.18) in C# .NET. Lesen Sie JSON aus einem String, einem Stream oder einer Pipe, streamen Sie eine Sequenz von Datasets und lösen Sie Bulk‑Daten‑Referenzen mit der Aspose.Medical‑API auf.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JSON nach DICOM konvertieren in .NET C#" h2="Lesen Sie das standardisierte DICOM‑JSON‑Modell (PS3.18) zurück in Datasets und DICOM‑Dateien. Arbeiten Sie mit einem String, einem Stream oder einer Pipe, streamen Sie eine Sequenz von Studien und lösen Sie Bulk‑Daten‑Referenzen auf." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Von DICOM‑JSON zu einer DICOM‑Datei">}}

<p><strong>Aspose.Medical für .NET</strong> liest das <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON‑Modell</a>, die Darstellung, die von DICOMweb‑Diensten und von Systemen verwendet wird, die Studien über HTTP austauschen. Was als JSON ankommt, wird zu einem <code>Dataset</code>, und ein <code>Dataset</code> wird als DICOM‑Datei auf die Festplatte geschrieben.</p>

<p>Dies ist die umgekehrte Richtung der <a href="/medical/net/dicom-to-json/">DICOM‑zu‑JSON</a>-Seite, und beide verwenden dieselbe Klasse, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Erstellen Sie eine DICOM‑Datei aus JSON - C#</h3>
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

<p>Ein Dataset, das keine File‑Meta‑Informationen enthält, wird mit der Standard‑Transfer‑Syntax Implicit VR Little Endian geschrieben, wenn es in ein <code>DicomFile</code> eingebettet ist.</p>

<p>Das Lesen von DICOM‑JSON ist eine lizenzierte Funktion. Ohne angewandte On‑Premise‑Lizenz wirft der Reader eine <code>MedicalApiException</code>; daher sollten Sie zuerst die Lizenz aktivieren, wie im <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">Lizenzleitfaden</a> beschrieben.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="File‑Meta‑Informationen beibehalten">}}

<p><code>Deserialize</code> gibt ausschließlich das Dataset zurück. Wenn das JSON‑Dokument auch die File‑Meta‑Information‑Gruppe enthält, zum Beispiel weil es aus einer vollständigen DICOM‑Datei erzeugt wurde, gibt <code>DeserializeFile</code> ein <code>DicomFile</code> mit dieser Gruppe unverändert zurück, einschließlich der im Dateikopf deklarierten Transfer‑Syntax.</p>

<div class="codeblock" id="code">
 <h3>Eine vollständige DICOM‑Datei aus JSON lesen - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streams, Pipes und asynchron">}}

<p>Jeder Einstiegspunkt hat eine Stream‑Überladung und eine asynchrone Überladung, wobei die asynchronen Varianten ebenfalls einen <code>PipeReader</code> akzeptieren. Ein Dokument, das von einer Web‑Antwort oder von der Festplatte kommt, wird gelesen, ohne vorher in einen String umgewandelt zu werden – das ist wichtig, sobald das JSON Pixeldaten enthält.</p>

<div class="codeblock" id="code">
 <h3>JSON aus einem Stream lesen - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Eine Sequenz von Datasets, einzeln">}}

<p>Eine DICOMweb‑Abfrage liefert ein Array von Datasets, und ein solches Dokument kann groß sein. <code>DeserializeList</code> liest das gesamte Array in den Speicher; <code>DeserializeAsyncEnumerable</code> gibt ein Dataset nach dem anderen zurück, sodass das Dokument nie vollständig im Speicher gehalten wird.</p>

<div class="codeblock" id="code">
 <h3>Ein Array von Datasets streamen - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Bulk‑Daten‑Referenzen">}}

<p>Das DICOM‑JSON‑Modell enthält Pixeldaten nicht inline. Große Werte werden durch eine <code>BulkDataURI</code> ersetzt, die auf die Bytes verweist, wodurch das JSON‑Dokument klein bleibt. Um diese Referenzen beim Lesen aufzulösen, geben Sie dem Serializer einen Bulk‑Daten‑Loader. <code>DefaultBulkDataLoader</code> holt <code>file</code>-, <code>http</code>- und <code>https</code>-URIs ohne Authentifizierung; für ein Archiv, das Anmeldeinformationen benötigt, implementieren Sie selbst <code>IBulkDataLoader</code> oder <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>BulkDataURI beim Lesen auflösen - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Rundweg mit DICOM zu JSON">}}

<p>Die beiden Richtungen sind für den gemeinsamen Einsatz gedacht: Eine Studie wird als JSON exportiert, durchläuft einen Web‑Service und wird wieder als DICOM‑Datei zurückgegeben. Im gesamten Prozess wird kein nativer Code benötigt, sodass derselbe Rundweg auf Windows, Linux und macOS funktioniert.</p>

<div class="codeblock" id="code">
 <h3>DICOM zu JSON und zurück - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Für die Optionen, die das Aussehen des JSON bestimmen, siehe die Seite <a href="/medical/net/dicom-to-json/">DICOM zu JSON</a>. Das gleiche Paar existiert für XML: <a href="/medical/net/dicom-to-xml/">DICOM zu XML</a> und <a href="/medical/net/xml-to-dicom/">XML zu DICOM</a>. Der <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">JSON‑Serialisierungs‑Leitfaden</a> behandelt die gesamte API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Entwicklerhandbuch" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="API‑Referenzen" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktunterstützung" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Kostenloser Support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Kostenpflichtiger Support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Warum Aspose.Medical für .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundenliste" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Erfolgsgeschichten" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}