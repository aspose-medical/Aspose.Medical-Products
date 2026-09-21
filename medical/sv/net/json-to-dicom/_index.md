---
title: Konvertera JSON till DICOM i C# .NET | Aspose.Medical
weight: 6000

description: Skapa DICOM‑filer från den standardiserade DICOM JSON‑modellen (PS3.18) i C# .NET. Läs JSON från en sträng, en ström eller ett rör, strömma en sekvens av dataset och lös upp bulk‑data‑referenser med Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konvertera JSON till DICOM i .NET C#" h2="Läs den standardiserade DICOM JSON‑modellen (PS3.18) tillbaka till dataset och DICOM‑filer. Arbeta från en sträng, en ström eller ett rör, strömma en sekvens av studier och lös upp bulk‑data‑referenser." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Från DICOM JSON till en DICOM‑fil">}}

<p><strong>Aspose.Medical for .NET</strong> läser <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a>, representationen som används av DICOMweb‑tjänster och av system som utbyter studier via HTTP. Det som anländer som JSON blir ett <code>Dataset</code>, och ett <code>Dataset</code> skrivs till disk som en DICOM‑fil.</p>

<p>Detta är den omvända riktningen av sidan <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>, och de två använder samma klass, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Skapa en DICOM‑fil från JSON - C#</h3>
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

<p>Ett dataset som saknar File Meta Information skrivs med standardöverföringssyntaxen, Implicit VR Little Endian, när det paketeras i en <code>DicomFile</code>.</p>

<p>Läsning av DICOM JSON är en licensierad funktion. Utan en installerad licens på plats kastar läsaren ett <code>MedicalApiException</code>, så applicera licensen först, enligt <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">licensieringsguiden</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Behåll File Meta Information">}}

<p><code>Deserialize</code> returnerar endast datasetet. När JSON‑dokumentet också innehåller File Meta Information‑gruppen, till exempel eftersom det producerades från en komplett DICOM‑fil, returnerar <code>DeserializeFile</code> en <code>DicomFile</code> med den gruppen intakt, inklusive den överföringssyntax som filen deklarerar.</p>

<div class="codeblock" id="code">
 <h3>Läs en komplett DICOM‑fil från JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Strömmar, rör och async">}}

<p>Varje ingångspunkt har en overload för ström och en asynkron overload, och de asynkrona accepterar också en <code>PipeReader</code>. Ett dokument som kommer från ett webbsvar eller från disk läses utan att först konverteras till en sträng, vilket är viktigt så snart JSON innehåller pixeldata.</p>

<div class="codeblock" id="code">
 <h3>Läs JSON från en ström - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="En sekvens av dataset, ett i taget">}}

<p>En DICOMweb‑fråga svarar med en array av dataset, och ett sådant dokument kan vara stort. <code>DeserializeList</code> läser hela arrayen till minnet; <code>DeserializeAsyncEnumerable</code> returnerar ett dataset i taget, så dokumentet aldrig lagras helt i minnet.</p>

<div class="codeblock" id="code">
 <h3>Strömma en array av dataset - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Bulk‑data‑referenser">}}

<p>DICOM JSON‑modellen innehåller inte pixeldata inbäddad. Stora värden ersätts av en <code>BulkDataURI</code> som pekar på bytena, vilket håller JSON‑dokumentet litet. För att lösa upp dessa referenser under läsning, ge serializern en bulk‑data‑laddare. <code>DefaultBulkDataLoader</code> hämtar <code>file</code>, <code>http</code> och <code>https</code> URI:er utan autentisering; för ett arkiv som kräver inloggning, implementera <code>IBulkDataLoader</code> eller <code>IAsyncBulkDataLoader</code> själv.</p>

<div class="codeblock" id="code">
 <h3>Lös upp BulkDataURI vid läsning - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Round‑trip med DICOM till JSON">}}

<p>De två riktningarna är avsedda att användas tillsammans: en studie lämnar som JSON, färdas genom en webbtjänst och återvänder som en DICOM‑fil. Ingenting i processen är beroende av native‑kod, så samma round‑trip körs på Windows, Linux och macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM till JSON och tillbaka - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>För de alternativ som styr hur JSON ser ut, se sidan <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>. Samma par finns för XML: <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> och <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">JSON‑serialiseringsguiden</a> täcker hela API:et.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Utvecklarguide" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="API‑referenser" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktstöd" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betald support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blogg" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Varför Aspose.Medical för .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundlista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Framgångshistorier" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}