---
title: XML in DICOM konvertieren in C# .NET | Aspose.Medical
weight: 5000

description: Erstellen Sie DICOM-Dateien aus dem Native DICOM Model XML von PS3.19 in C# .NET. Lesen Sie XML aus einem String, einem Stream oder einer Pipe, streamen Sie aufeinanderfolgende Dokumente und lösen Sie Bulk‑Daten‑Referenzen mit der Aspose.Medical API auf.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="XML in DICOM konvertieren in .NET C#" h2="Lesen Sie das Native DICOM Model XML von PS3.19 zurück in Datasets und DICOM‑Dateien. Arbeiten Sie mit einem String, einem Stream oder einer Pipe, streamen Sie aufeinanderfolgende Dokumente und lösen Sie Bulk‑Daten‑Referenzen." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standard‑Native DICOM Model XML">}}

<p><strong>Aspose.Medical für .NET</strong> liest das <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a>, definiert in DICOM PS3.19. Dies ist die XML‑Repräsentation, die im Standard selbst definiert ist, kein von Aspose erfundenes Format, was es für die Integration nützlich macht: Ein System, das bereits DICOM als XML austauscht, erzeugt Dokumente, die diese Bibliothek akzeptiert.</p>

<p>Die Dokumentwurzel ist <code>NativeDicomModel</code>, und jedes Attribut ist ein <code>DicomAttribute</code>-Element, das Tag, Value Representation und Keyword enthält:</p>

<div class="codeblock" id="code">
 <h3>Native DICOM Model-Format</h3>
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

<p>Diese Seite stellt die umgekehrte Richtung von <a href="/medical/net/dicom-to-xml/">DICOM zu XML</a> dar, und beide verwenden dieselbe Klasse, <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Erstellen einer DICOM-Datei aus XML in C#">}}

<p><code>Deserialize</code> wandelt ein Dokument in ein <code>Dataset</code> um, und ein Dataset wird als DICOM‑Datei auf die Festplatte geschrieben.</p>

<div class="codeblock" id="code">
 <h3>Erstellen einer DICOM-Datei aus XML – C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Das Native DICOM Model enthält keine File Meta Information-Gruppe, sodass die Transfer Syntax nicht Teil des Dokuments ist. Ein Dataset, das in ein <code>DicomFile</code> eingebettet ist, wird mit der Standard‑Transfer‑Syntax Implicit VR Little Endian geschrieben. Um die Datei mit einer anderen Syntax zu speichern, transkodieren Sie sie, wie die Seite zur <a href="/medical/net/dicom-transfer-syntax-conversion/">Transfer‑Syntax-Konvertierung</a> zeigt.</p>

<p>Das Lesen von DICOM XML ist ein lizenziertes Feature. Ohne eine angelegte On‑Premise‑Lizenz wirft der Leser eine <code>MedicalApiException</code>, daher sollten Sie zuerst die Lizenz anwenden, wie im <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">Lizenzierungsleitfaden</a> beschrieben.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streams, Pipes und Async">}}

<p>Jeder Einstiegspunkt verfügt über eine Stream‑Überladung und eine asynchrone Überladung, wobei die asynchronen Varianten ebenfalls einen <code>PipeReader</code> akzeptieren. Ein Dokument, das aus einer Web‑Antwort eintrifft, wird beim Lesen direkt geparst, ohne vorher in einen String umgewandelt zu werden.</p>

<div class="codeblock" id="code">
 <h3>XML aus einem Stream lesen – C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Aufeinanderfolgende Dokumente in einem Stream">}}

<p>Ein Export aus einem anderen System enthält häufig ein <code>NativeDicomModel</code>-Element nach dem anderen in einem einzigen Stream. <code>DeserializeAsyncEnumerable</code> liefert pro Element ein Dataset in Eingabe­reihenfolge, sodass der Stream verarbeitet wird, ohne im Speicher gehalten zu werden. Die Elemente folgen direkt aufeinander: Eine XML‑Deklaration ist nur am aller Anfang zulässig, wie bei jeder XML‑Eingabe.</p>

<div class="codeblock" id="code">
 <h3>Aufeinanderfolgende Dokumente streamen – C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk‑Daten‑Referenzen">}}

<p>Große Werte wie Pixeldaten werden nicht inline geschrieben. Sie erscheinen als ein <code>BulkData</code>-Element mit einer URI, die auf die Bytes verweist, wodurch das Dokument klein bleibt. Um diese Referenzen beim Lesen aufzulösen, geben Sie dem Serializer einen Bulk‑Data‑Loader. <code>DefaultBulkDataLoader</code> ruft <code>file</code>-, <code>http</code>- und <code>https</code>-URIs ohne Authentifizierung ab; für ein Archiv, das Anmeldeinformationen benötigt, implementieren Sie selbst <code>IBulkDataLoader</code> oder <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Bulk‑Daten beim Lesen auflösen – C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Round‑Trip mit DICOM zu XML">}}

<p>Beide Richtungen sind für die gemeinsame Nutzung gedacht: Eine Studie wird als XML exportiert, durchläuft ein System, das XML versteht, und wird wieder als DICOM‑Datei zurückgegeben. Alles wird in verwaltetem .NET ausgeführt, sodass derselbe Round‑Trip auf Windows, Linux und macOS läuft.</p>

<div class="codeblock" id="code">
 <h3>DICOM zu XML und zurück – C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Für die Optionen, die das Aussehen des XML steuern, siehe die Seite <a href="/medical/net/dicom-to-xml/">DICOM zu XML</a>. Das gleiche Paar gibt es für JSON: <a href="/medical/net/dicom-to-json/">DICOM zu JSON</a> und <a href="/medical/net/json-to-dicom/">JSON zu DICOM</a>. Der <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">Serialisierungs‑Leitfaden</a> behandelt die gesamte API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Entwicklerhandbuch" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="API-Referenzen" href="https://reference.aspose.com/medical/net/" >}}
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