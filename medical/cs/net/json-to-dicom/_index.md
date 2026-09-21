---
title: Převod JSON na DICOM v C# .NET | Aspose.Medical
weight: 6000

description: Vytvářejte DICOM soubory ze standardního modelu DICOM JSON (PS3.18) v C# .NET. Čtěte JSON ze stringu, streamu nebo pipe, streamujte sekvenci datasetů a řešte odkazy na bulk data pomocí Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Převod JSON na DICOM v .NET C#" h2="Načtěte standardní model DICOM JSON (PS3.18) zpět do datasetů a DICOM souborů. Pracujte se stringem, streamem nebo pipe, streamujte sekvenci studií a řešte odkazy na bulk data." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Z DICOM JSON na DICOM soubor">}}

<p><strong>Aspose.Medical for .NET</strong> čte <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">model DICOM PS3.18 JSON</a>, reprezentaci používanou službami DICOMweb a systémy, které vyměňují studie přes HTTP. To, co přijde jako JSON, se stane <code>Dataset</code> a <code>Dataset</code> je zapsán na disk jako DICOM soubor.</p>

<p>Toto je opačný směr než stránka <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> a oba používají stejnou třídu, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Vytvořit DICOM soubor z JSON – C#</h3>
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

<p>Dataset, který neobsahuje File Meta Information, je zápisem s výchozí přenosovou syntaxí Implicit VR Little Endian, pokud je zabalen do <code>DicomFile</code>.</p>

<p>Čtení DICOM JSON je licencovaná funkce. Bez aplikované on-premise licence čteč vrhá <code>MedicalApiException</code>, takže nejprve aplikujte licenci, jak popisuje <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">průvodce licencováním</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zachovat File Meta Information">}}

<p><code>Deserialize</code> vrací pouze dataset. Když JSON dokument také obsahuje skupinu File Meta Information, například protože byl vytvořen z kompletního DICOM souboru, <code>DeserializeFile</code> vrací <code>DicomFile</code> s touto skupinou zachovanou, včetně přenosové syntaxe, kterou soubor deklaruje.</p>

<div class="codeblock" id="code">
 <h3>Načíst kompletní DICOM soubor z JSON – C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streamy, pipe a asynchronní operace">}}

<p>Každý vstupní bod má přetížení pro stream a asynchronní přetížení, přičemž asynchronní verze také akceptuje <code>PipeReader</code>. Dokument, který přijde z webové odpovědi nebo z disku, je čten přímo, aniž by byl nejprve převeden na string, což má význam, jakmile JSON obsahuje data pixelů.</p>

<div class="codeblock" id="code">
 <h3>Číst JSON ze streamu – C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sekvence datasetů, jeden po druhém">}}

<p>Dotaz DICOMweb vrací pole datasetů a takový dokument může být velký. <code>DeserializeList</code> načte celé pole do paměti; <code>DeserializeAsyncEnumerable</code> poskytuje jeden dataset po druhém, takže dokument není nikdy načten celý.</p>

<div class="codeblock" id="code">
 <h3>Streamovat pole datasetů – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Odkazy na bulk data">}}

<p>Model DICOM JSON neobsahuje pixelová data inline. Velké hodnoty jsou nahrazeny <code>BulkDataURI</code>, která ukazuje na bajty, což udržuje JSON dokument malý. Pro vyřešení těchto odkazů během čtení poskytněte serializeru bulk data loader. <code>DefaultBulkDataLoader</code> načítá URI typu <code>file</code>, <code>http</code> a <code>https</code> bez autentizace; pro archiv vyžadující přihlašovací údaje implementujte vlastní <code>IBulkDataLoader</code> nebo <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Vyřešit BulkDataURI během čtení – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Obousměrný převod s DICOM na JSON">}}

<p>Obě směry jsou určeny k společnému použití: studie je exportována jako JSON, prochází webovou službou a vrací se jako DICOM soubor. V procesu není závislost na nativním kódu, takže stejný obousměrný převod funguje na Windows, Linux a macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM na JSON a zpět – C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Pro možnosti, které určují vzhled JSON, viz stránka <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>. Stejný dvojice existuje i pro XML: <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> a <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">Průvodce serializací JSON</a> pokrývá celé API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zdroje pro učení" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Průvodce vývojáře" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="Reference API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Podpora produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Bezplatná podpora" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Placená podpora" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Proč Aspose.Medical pro .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Seznam zákazníků" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Příběhy úspěchů" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}