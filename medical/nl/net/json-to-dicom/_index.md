---
title: Converteer JSON naar DICOM in C# .NET | Aspose.Medical
weight: 6000

description: Maak DICOM‑bestanden op basis van het standaard DICOM JSON‑model (PS3.18) in C# .NET. Lees JSON vanuit een string, een stream of een pipe, stream een reeks datasets en los bulk‑data‑referenties op met de Aspose.Medical‑API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Converteer JSON naar DICOM in .NET C#" h2="Lees het standaard DICOM JSON‑model (PS3.18) terug naar datasets en DICOM‑bestanden. Werk vanuit een string, een stream of een pipe, stream een reeks studies en los bulk‑data‑referenties op." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Van DICOM JSON naar een DICOM‑bestand">}}

<p><strong>Aspose.Medical for .NET</strong> leest het <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON‑model</a>, de representatie die wordt gebruikt door DICOMweb‑services en door systemen die studies uitwisselen via HTTP. Wat als JSON binnenkomt wordt een <code>Dataset</code>, en een <code>Dataset</code> wordt naar schijf geschreven als een DICOM‑bestand.</p>

<p>Dit is de omgekeerde richting van de <a href="/medical/net/dicom-to-json/">DICOM naar JSON</a> pagina, en beide gebruiken dezelfde klasse, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Maak een DICOM‑bestand van JSON - C#</h3>
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

<p>Een dataset die geen File Meta Information bevat, wordt geschreven met de standaard transfer syntax, Implicit VR Little Endian, wanneer deze wordt ingepakt in een <code>DicomFile</code>.</p>

<p>Het lezen van DICOM JSON is een gelicentieerde functie. Zonder een on‑premise licentie te hebben toegepast, gooit de lezer een <code>MedicalApiException</code>, dus pas de licentie eerst toe, zoals beschreven in de <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">licentiehandleiding</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Behoud de File Meta Information">}}

<p><code>Deserialize</code> geeft alleen de dataset terug. Wanneer het JSON‑document ook de File Meta Information‑groep bevat, bijvoorbeeld omdat het is gemaakt vanuit een volledig DICOM‑bestand, geeft <code>DeserializeFile</code> een <code>DicomFile</code> terug met die groep intact, inclusief de transfer syntax die het bestand declareert.</p>

<div class="codeblock" id="code">
 <h3>Lees een volledig DICOM‑bestand van JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streams, pipes en async">}}

<p>Elk toegangspunt heeft een stream‑overload en een asynchrone overload, en de asynchrone versies accepteren ook een <code>PipeReader</code>. Een document dat afkomstig is van een web‑respons of van schijf wordt gelezen zonder eerst omgezet te worden naar een string, wat van belang is zodra de JSON pixeldata bevat.</p>

<div class="codeblock" id="code">
 <h3>Lees JSON van een stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Een reeks datasets, één per keer">}}

<p>Een DICOMweb‑query beantwoordt met een array van datasets, en zo'n document kan groot zijn. <code>DeserializeList</code> leest de hele array in het geheugen; <code>DeserializeAsyncEnumerable</code> levert één dataset per keer, zodat het document nooit volledig in het geheugen wordt gehouden.</p>

<div class="codeblock" id="code">
 <h3>Stream een array van datasets - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Bulk‑data‑referenties">}}

<p>Het DICOM JSON‑model bevat geen pixeldata inline. Grote waarden worden vervangen door een <code>BulkDataURI</code> die naar de bytes wijst, waardoor het JSON‑document klein blijft. Om die referenties tijdens het lezen op te lossen, geef de serializer een bulk‑data‑loader. <code>DefaultBulkDataLoader</code> haalt <code>file</code>-, <code>http</code>- en <code>https</code>-URI’s op zonder authenticatie; voor een archief dat inloggegevens vereist, implementeer je zelf <code>IBulkDataLoader</code> of <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Los BulkDataURI op tijdens het lezen - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Round‑trip met DICOM naar JSON">}}

<p>De twee richtingen zijn bedoeld om samen te worden gebruikt: een onderzoek wordt geëxporteerd als JSON, reist via een webservice en komt terug als een DICOM‑bestand. Niets in het proces is afhankelijk van native code, zodat dezelfde round‑trip werkt op Windows, Linux en macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM naar JSON en terug - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Voor de opties die bepalen hoe de JSON eruitziet, zie de pagina <a href="/medical/net/dicom-to-json/">DICOM naar JSON</a>. Hetzelfde paar bestaat voor XML: <a href="/medical/net/dicom-to-xml/">DICOM naar XML</a> en <a href="/medical/net/xml-to-dicom/">XML naar DICOM</a>. De <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">JSON‑serialisatiehandleiding</a> behandelt de volledige API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leerbronnen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Ontwikkelaarsgids" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="API‑referenties" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Productondersteuning" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis ondersteuning" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betaalde ondersteuning" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Waarom Aspose.Medical voor .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Klantenlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}