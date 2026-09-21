---
title: Konvertera XML till DICOM i C# .NET | Aspose.Medical
weight: 5000

description: Skapa DICOM-filer från Native DICOM Model XML enligt PS3.19 i C# .NET. Läs XML från en sträng, en stream eller ett pipe, strömma på varandra följande dokument, och lösa bulkdata-referenser med Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konvertera XML till DICOM i .NET C#" h2="Läs Native DICOM Model XML enligt PS3.19 tillbaka till dataset och DICOM-filer. Arbeta från en sträng, en stream eller ett pipe, strömma på varandra följande dokument, och lösa bulkdata-referenser." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standard Native DICOM Model XML">}}

<p><strong>Aspose.Medical för .NET</strong> läser den <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a> som definieras i DICOM PS3.19. Detta är den XML-representation som skrivs in i själva standarden, inte ett format som Aspose har uppfunnit, vilket gör det användbart för integration: ett system som redan utbyter DICOM som XML producerar dokument som detta bibliotek accepterar.</p>

<p>Dokumentroten är <code>NativeDicomModel</code>, och varje attribut är ett <code>DicomAttribute</code>-element som bär sin tagg, value representation och nyckelord:</p>

<div class="codeblock" id="code">
 <h3>Native DICOM Model-format</h3>
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

<p>Denna sida är den omvända riktningen av <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>, och båda använder samma klass, <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Skapa en DICOM-fil från XML i C#">}}

<p><code>Deserialize</code> omvandlar ett dokument till ett <code>Dataset</code>, och ett dataset skrivs till disk som en DICOM-fil.</p>

<div class="codeblock" id="code">
 <h3>Skapa en DICOM-fil från XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Native DICOM Model har ingen File Meta Information-grupp, så överföringssyntaxen är inte en del av dokumentet. Ett dataset insvept i en <code>DicomFile</code> skrivs med standardöverföringssyntaxen, Implicit VR Little Endian. För att lagra filen med en annan, transkoda den, som sidan <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> visar.</p>

<p>Läsning av DICOM XML är en licensierad funktion. Utan en applicerad on-premise-licens kastar läsaren ett <code>MedicalApiException</code>, så applicera licensen först, enligt <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">licensguiden</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Strömmar, pipes och async">}}

<p>Varje ingångspunkt har en stream overload och en asynchronous overload, och de asynkrona accepterar även en <code>PipeReader</code>. Ett dokument som anländer från ett webbsvar parsas medan det läses, utan att först omvandlas till en sträng.</p>

<div class="codeblock" id="code">
 <h3>Läs XML från en stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sekventiella dokument i en stream">}}

<p>En export från ett annat system innehåller ofta ett <code>NativeDicomModel</code>-element efter ett annat i en enda stream. <code>DeserializeAsyncEnumerable</code> ger ett dataset per element, i inmatningsordning, så streamen bearbetas utan att hållas i minnet. Elementen följer varandra direkt: en XML-deklaration är endast tillåten i början, som i vilken XML-indata som helst.</p>

<div class="codeblock" id="code">
 <h3>Strömma sekventiella dokument - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulkdata-referenser">}}

<p>Stora värden såsom pixeldata skrivs inte inline. De visas som ett <code>BulkData</code>-element med en URI som pekar på bytena, vilket håller dokumentet litet. För att lösa dessa referenser vid läsning, ge serialiseraren en bulkdata‑laddare. <code>DefaultBulkDataLoader</code> hämtar <code>file</code>-, <code>http</code>- och <code>https</code>-URI:er utan autentisering; för ett arkiv som kräver inloggning, implementera själv <code>IBulkDataLoader</code> eller <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Lös bulkdata vid läsning - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rundresa med DICOM till XML">}}

<p>De två riktningarna är avsedda att användas tillsammans: en studie exporteras som XML, passerar genom ett system som förstår XML, och återvänder som en DICOM-fil. Allt är .NET‑hanterat, så samma rundresa fungerar på Windows, Linux och macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM till XML och tillbaka - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>För de alternativ som styr hur XML ser ut, se sidan <a href="/medical/net/dicom-to-xml/">DICOM to XML</a>. Samma par finns för JSON: <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> och <a href="/medical/net/json-to-dicom/">JSON to DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">Serialiseringsguiden</a> täcker hela API:et.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Utvecklarguide" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="API‑referenser" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktstöd" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis stöd" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betalt stöd" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blogg" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Varför Aspose.Medical för .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundlista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Framgångshistorier" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}