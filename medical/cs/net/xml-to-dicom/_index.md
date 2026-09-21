---
title: Převod XML na DICOM v C# .NET | Aspose.Medical
weight: 5000

description: Vytvořte soubory DICOM z XML nativního modelu DICOM PS3.19 v C# .NET. Čtěte XML ze řetězce, proudu nebo roury, streamujte po sobě jdoucí dokumenty a řešte odkazy na hromadná data pomocí Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Převod XML na DICOM v .NET C#" h2="Načtěte XML nativního modelu DICOM PS3.19 zpět do datasetů a souborů DICOM. Pracujte s řetězcem, proudem nebo rourou, streamujte po sobě jdoucí dokumenty a řešte odkazy na hromadná data." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standardní XML nativního modelu DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> čte <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">nativní model DICOM</a> definovaný v DICOM PS3.19. Jedná se o XML reprezentaci zapsanou přímo ve standardu, nikoli o formát vymyslený společností Aspose, což jej činí užitečným pro integraci: systém, který již vyměňuje DICOM jako XML, produkuje dokumenty, které tato knihovna přijímá.</p>

<p>Kořen dokumentu je <code>NativeDicomModel</code> a každý atribut je prvek <code>DicomAttribute</code> obsahující svůj tag, reprezentaci hodnoty a klíčové slovo:</p>

<div class="codeblock" id="code">
 <h3>Formát nativního modelu DICOM</h3>
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

<p>Tato stránka představuje opačný směr než <a href="/medical/net/dicom-to-xml/">DICOM na XML</a> a oba používají stejnou třídu, <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vytvořte soubor DICOM z XML v C#">}}

<p><code>Deserialize</code> převede dokument na <code>Dataset</code> a dataset se zapíše na disk jako soubor DICOM.</p>

<div class="codeblock" id="code">
 <h3>Vytvoření souboru DICOM z XML – C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Nativní model DICOM neobsahuje skupinu File Meta Information, takže přenosová syntaxe není součástí dokumentu. Dataset zabalený do <code>DicomFile</code> se zapíše s výchozí přenosovou syntaxí, Implicit VR Little Endian. Pro uložení souboru s jinou syntaxí jej převodte, jak ukazuje stránka <a href="/medical/net/dicom-transfer-syntax-conversion/">převodu přenosové syntaxe</a>.</p>

<p>Čtení DICOM XML je licencovaná funkce. Bez aplikované lokální licence čtečová funkce vyvolá výjimku <code>MedicalApiException</code>, proto nejprve aplikujte licenci, jak popisuje <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">průvodce licencováním</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Proudy, roury a asynchronní operace">}}

<p>Každý vstupní bod má přetížení pro proud a asynchronní přetížení, přičemž asynchronní také přijímají <code>PipeReader</code>. Dokument, který přichází z webové odpovědi, se parsuje během čtení, aniž by byl nejprve převeden na řetězec.</p>

<div class="codeblock" id="code">
 <h3>Čtení XML z proudu – C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Po sobě jdoucí dokumenty v jednom proudu">}}

<p>Export z jiného systému často obsahuje jeden prvek <code>NativeDicomModel</code> za druhým v jediném proudu. <code>DeserializeAsyncEnumerable</code> vrací jeden dataset na prvek v pořadí vstupu, takže proud je zpracován bez načtení do paměti. Prvky následují přímo po sobě: deklarace XML je povolena jen na úplném začátku, jako u libovolného XML vstupu.</p>

<div class="codeblock" id="code">
 <h3>Streamování po sobě jdoucích dokumentů – C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Odkazy na hromadná data">}}

<p>Velké hodnoty, například pixlová data, nejsou zapisovány inline. Objevují se jako prvek <code>BulkData</code> s URI, které odkazuje na bajty, což udržuje dokument malý. Pro vyřešení těchto odkazů během čtení poskytněte serializeru načítač hromadných dat. <code>DefaultBulkDataLoader</code> načítá URI typu <code>file</code>, <code>http</code> a <code>https</code> bez autentizace; pro archiv vyžadující přihlašovací údaje implementujte vlastní <code>IBulkDataLoader</code> nebo <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Řešení hromadných dat během čtení – C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Obousměrná konverze DICOM na XML">}}

<p>Obě směry jsou určeny k společnému použití: studie je exportována jako XML, projde systémem, který pracuje s XML, a vrátí se jako soubor DICOM. Vše je v .NET, takže stejný obousměrný proces běží na Windows, Linuxu i macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM na XML a zpět – C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Pro možnosti, které řídí vzhled XML, viz stránka <a href="/medical/net/dicom-to-xml/">DICOM na XML</a>. Stejný pár existuje pro JSON: <a href="/medical/net/dicom-to-json/">DICOM na JSON</a> a <a href="/medical/net/json-to-dicom/">JSON na DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">Průvodce serializací</a> pokrývá celé API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Učební materiály" tabId="resources" >}}
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
{{< blocks/products/pf/slr-element name="Úspěšné příběhy" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}