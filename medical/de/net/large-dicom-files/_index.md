---
title: Arbeiten mit großen DICOM-Dateien in C# .NET | Aspose.Medical
weight: 11500

description: Öffnen Sie Multi‑Frame‑Studien und Whole‑Slide‑Bilder in C# ohne sie in den Speicher zu laden. Metadaten ohne Pixeldaten lesen, große Elemente zurückstellen und Dateien über Streams und Pipes bewegen.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Große DICOM-Dateien in .NET C#" h2="Lesen Sie die Metadaten einer Multi‑Frame‑Studie ohne die Pixel, stellen Sie große Elemente zurück, bis sie angefordert werden, und bewegen Sie komplette Dateien über Streams und Pipes." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Die Datei ist groß, die Frage ist meist klein">}}

<p>Ein Whole‑Slide‑Bild, eine lange CT‑Serie oder ein OCT‑Volumen umfasst mehrere hundert Megabyte, wobei der größte Teil Pixeldaten sind. Die eigentliche Arbeit einer Anwendung ist oft viel kleiner: Auflisten, was sich in einem Ordner befindet, Überprüfen einer Patienten‑ID, Zählen der Frames, Entscheiden, wohin eine Studie gehört. Das Laden jedes einzelnen Bytes, um das zu beantworten, verwandelt eine einfache Aufgabe in ein Speicherproblem.</p>

<p><strong>Aspose.Medical für .NET</strong> ermöglicht dem Aufrufer zu entscheiden, wie viel einer Datei gelesen wird. Die Wahl ist ein Argument bei <code>DicomFile.Open</code> und gilt gleichermaßen für Dateien, Streams und Pipes.</p>

<p>Gemessen an einer 14 MB‑Studie mit 128 Frames aus unserem Testdatensatz, auf derselben Maschine und derselben Datei:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Lese‑Strategie</th>
<th>Öffnungszeit</th>
<th>Zugewiesener Speicher</th>
</tr>
</thead>
<tbody>
<tr><td>Alles, Standard</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Große Elemente übersprungen</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Große Elemente zurückgestellt</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>Die Lücke wächst mit der Dateigröße. Ein Ordner mit 10.000 Studien ist der Fall, in dem es keine Mikro‑Optimierung mehr ist.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Metadaten lesen, Pixel unangetastet lassen">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> lässt jedes Element, das über einem Größenschwellenwert liegt, aus dem Lesevorgang aus. Der zurückgegebene Datensatz enthält nur die Tags, die ein Index oder ein Router benötigt.</p>

<div class="codeblock" id="code">
 <h3>Eine Studie ohne Pixeldaten lesen – C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>Der Schwellenwert ist standardmäßig 64 kB und wird in Kilobyte angegeben, sodass ein Workflow, der 8 kB als groß betrachtet, das angeben kann.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zurückstellen statt überspringen">}}

<p>Wenn die Pixel eventuell benötigt werden, aber wahrscheinlich später und nicht alle, ist <code>ReadLargeOnDemand</code> die zweite Hälfte des Paares. Das Öffnen der Datei kostet genauso wie das Überspringen, und ein großes Element wird gelesen, sobald der Code darauf zugreift.</p>

<div class="codeblock" id="code">
 <h3>Einen Frame nur bei Bedarf laden – C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>Deferred Reading ist ein lizenziertes Feature; die anderen Strategien funktionieren ebenfalls in der Evaluierung.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Einen Ordner indizieren, ohne die Pixel zu berühren">}}

<p>Die gleiche Strategie gilt für einen Stream, wie er bei einem Archiv‑Scan oder einem Cloud‑Object‑Store aus dem Code heraus aussieht.</p>

<div class="codeblock" id="code">
 <h3>Ein Archiv scannen – C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streams und Pipes, rein und raus">}}

<p>Sowohl Lesen als auch Schreiben akzeptieren Streams, und die asynchronen Einstiegspunkte akzeptieren ebenfalls <code>System.IO.Pipelines</code>-Typen. Eine Studie kann von einer Netzwerkantwort bis zur Speicherung transportiert werden, ohne dass der Prozess die gesamte Datei als ein Array hält.</p>

<div class="codeblock" id="code">
 <h3>Lesen und Schreiben über Streams – C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>Dasselbe Prinzip gilt für die Text‑Representationen: Ein Dokument mit vielen Datensätzen wird jeweils ein Datensatz nach dem anderen auf den Seiten <a href="/medical/net/json-to-dicom/">JSON to DICOM</a> und <a href="/medical/net/xml-to-dicom/">XML to DICOM</a> gelesen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Frame für Frame">}}

<p>Multi‑Frame‑Daten werden frameweise adressiert, sodass eine Serie mit 500 Frames jeweils ein Frame nach dem anderen kostet, anstatt das gesamte Pixeldaten‑Element zu laden.</p>

<div class="codeblock" id="code">
 <h3>Frames durchlaufen – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Wo das das Design bestimmt">}}

<ul>
<li>Archivindizierung und Migration: Millionen von Dateien, wobei nur der Header wichtig ist, bis etwas verschoben wird.</li>
<li>Router und Speicher‑Knoten: akzeptieren eine Studie, lesen, was zum Weiterleiten nötig ist, und geben die Bytes weiter.</li>
<li>KI‑Pipelines: Manifest aus Metadaten erstellen und dann Frames für den tatsächlich trainierten Teil extrahieren.</li>
<li>Container mit Speicherlimit: Der Arbeitssatz folgt der Strategie, nicht der Dateigröße.</li>
<li>Whole‑Slide‑ und OCT‑Daten: Dateien, bei denen das Lesen des gesamten Inhalts keine Option ist.</li>
</ul>

<p>Der <a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">Speicherverwaltungs‑Leitfaden</a> erklärt die Strategien im Detail, und <a href="/medical/net/dicom-networking/">DICOM‑Networking</a> zeigt dieselben Daten, die über DIMSE eintreffen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Entwicklerhandbuch" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
{{< blocks/products/pf/slr-element name="API‑Referenzen" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktsupport" tabId="support" >}}
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
