---
title: XML naar DICOM converteren in C# .NET | Aspose.Medical
weight: 5000

description: Maak DICOM‑bestanden vanuit de Native DICOM Model XML van PS3.19 in C# .NET. Lees XML vanuit een string, een stream of een pipe, stream opeenvolgende documenten en los bulk‑data‑referenties op met de Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="XML naar DICOM converteren in .NET C#" h2="Lees de Native DICOM Model XML van PS3.19 terug naar datasets en DICOM‑bestanden. Werk vanuit een string, een stream of een pipe, stream opeenvolgende documenten en los bulk‑data‑referenties op." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standaard Native DICOM Model XML">}}

<p><strong>Aspose.Medical for .NET</strong> leest het <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a> gedefinieerd in DICOM PS3.19. Dit is de XML‑representatie die in de standaard zelf is geschreven, geen door Aspose uitgevonden formaat, wat het nuttig maakt voor integratie: een systeem dat al DICOM als XML uitwisselt, produceert documenten die deze bibliotheek accepteert.</p>

<p>De documentroot is <code>NativeDicomModel</code>, en elk attribuut is een <code>DicomAttribute</code>-element dat zijn tag, value representation en keyword bevat:</p>

<div class="codeblock" id="code">
 <h3>Native DICOM Model-indeling</h3>
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

<p>Deze pagina is de omgekeerde richting van <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>, en beide gebruiken dezelfde klasse, <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Creëer een DICOM‑bestand vanuit XML in C#">}}

<p><code>Deserialize</code> zet een document om in een <code>Dataset</code>, en een dataset wordt op schijf weggeschreven als een DICOM‑bestand.</p>

<div class="codeblock" id="code">
 <h3>Creëer een DICOM‑bestand vanuit XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Het Native DICOM Model heeft geen File Meta Information-groep, dus de transfer syntax is geen onderdeel van het document. Een dataset verpakt in een <code>DicomFile</code> wordt weggeschreven met de standaard transfer syntax, Implicit VR Little Endian. Om het bestand met een andere syntax op te slaan, transcodeer het, zoals de <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> pagina laat zien.</p>

<p>Het lezen van DICOM XML is een gelicentieerde functie. Zonder een on‑premise licentie wordt een <code>MedicalApiException</code> gegooid, dus pas eerst de licentie toe, zoals de <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">licensing guide</a> beschrijft.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streams, pipes en async">}}

<p>Elk toegangspunt heeft een stream‑overload en een asynchrone overload, en de asynchrone overloads accepteren ook een <code>PipeReader</code>. Een document dat van een webrespons arriveert, wordt geparseerd terwijl het wordt gelezen, zonder eerst te worden omgezet naar een string.</p>

<div class="codeblock" id="code">
 <h3>Lees XML vanuit een stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opeenvolgende documenten in één stream">}}

<p>Een export van een ander systeem bevat vaak een <code>NativeDicomModel</code>-element na het andere in één enkele stream. <code>DeserializeAsyncEnumerable</code> levert één dataset per element op, in de volgorde van invoer, zodat de stream wordt verwerkt zonder in het geheugen te worden gehouden. De elementen volgen elkaar direct: een XML‑declaratie is alleen aan het begin toegestaan, zoals bij elke XML‑invoer.</p>

<div class="codeblock" id="code">
 <h3>Stream opeenvolgende documenten - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk‑data‑referenties">}}

<p>Grote waarden zoals pixeldata worden niet inline weggeschreven. Ze verschijnen als een <code>BulkData</code>-element met een URI die naar de bytes wijst, waardoor het document klein blijft. Om die referenties tijdens het lezen op te lossen, geef de serializer een bulk‑data‑loader. <code>DefaultBulkDataLoader</code> haalt <code>file</code>, <code>http</code> en <code>https</code> URI's op zonder authenticatie; voor een archief dat inloggegevens vereist, implementeer zelf <code>IBulkDataLoader</code> of <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Los bulk‑data op tijdens lezen - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Round‑trip met DICOM naar XML">}}

<p>De twee richtingen zijn bedoeld om samen te worden gebruikt: een onderzoek wordt geëxporteerd als XML, gaat door een systeem dat XML begrijpt, en keert terug als een DICOM‑bestand. Alles wordt beheerd door .NET, zodat dezelfde round‑trip werkt op Windows, Linux en macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM naar XML en terug - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Voor de opties die bepalen hoe de XML eruitziet, zie de <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> pagina. Hetzelfde paar bestaat voor JSON: <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> en <a href="/medical/net/json-to-dicom/">JSON to DICOM</a>. De <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">serialization guide</a> behandelt de volledige API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leerbronnen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Ontwikkelaarsgids" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="API-referenties" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Productondersteuning" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis ondersteuning" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betaalde ondersteuning" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Waarom Aspose.Medical voor .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Klantlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}