---
title: Werken met grote DICOM-bestanden in C# .NET | Aspose.Medical
weight: 11500

description: Open multi-frame studies en whole slide‑beelden in C# zonder ze in het geheugen te laden. Lees metadata zonder pixelgegevens, stel grote elementen uit, en verplaats bestanden via streams en pipes.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Grote DICOM-bestanden in .NET C#" h2="Lees de metadata van een multi-frame study zonder de pixels, stel grote elementen uit totdat erom wordt gevraagd, en verplaats volledige bestanden via streams en pipes." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Het bestand is groot, de vraag is meestal klein">}}

<p>Een whole slide‑beeld, een lange CT‑reeks of een OCT‑volume is enkele honderden megabytes, en het grootste deel is pixeldata. Het werk dat een applicatie feitelijk doet, is vaak veel kleiner: opsommen wat er in een map staat, een patiënt‑identificatie controleren, het aantal frames tellen, bepalen waar een study heen moet. Elke byte laden om dat te beantwoorden, is wat een eenvoudige taak verandert in een geheugenprobleem.</p>

<p><strong>Aspose.Medical for .NET</strong> laat de aanroeper bepalen hoeveel van een bestand wordt gelezen. De keuze is één argument op <code>DicomFile.Open</code>, en het geldt zowel voor bestanden, streams als pipes.</p>

<p>Gemeten op een 14 MB study met 128 frames uit onze testset, op dezelfde machine en hetzelfde bestand:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Leesstrategie</th>
<th>Tijd om te openen</th>
<th>Toegewezen geheugen</th>
</tr>
</thead>
<tbody>
<tr><td>Alles, de standaard</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Grote elementen overgeslagen</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Grote elementen uitgesteld</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>Het verschil groeit naarmate het bestand groter is. Een map met 10.000 studies is het scenario waarin het geen micro‑optimalisatie meer is.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lees de metadata, laat de pixels onaangeroerd">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> laat elk element boven een grootte‑drempel buiten de leesoperatie. De dataset die terugkomt bevat alleen de tags die een index of router nodig heeft.</p>

<div class="codeblock" id="code">
 <h3>Lees een study zonder pixelgegevens - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>De drempel is standaard 64 kB en wordt opgegeven in kilobytes, zodat een workflow die 8 kB als groot beschouwt dit kan aangeven.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stel uit i.p.v. overslaan">}}

<p>Wanneer de pixels nodig kunnen zijn, maar waarschijnlijk later en niet allemaal, is <code>ReadLargeOnDemand</code> de andere helft van het pair. Het openen van het bestand kost evenveel als het overslaan, en een groot element wordt gelezen op het moment dat de code er toegang toe krijgt.</p>

<div class="codeblock" id="code">
 <h3>Laad een frame alleen wanneer het wordt gebruikt - C#</h3>
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

<p>Uitgesteld lezen is een gelicentieerde functie; de andere strategieën werken ook in de evaluatie.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Indexeer een map zonder de pixels aan te raken">}}

<p>Dezelfde strategie geldt voor een stream, wat is hoe een archiefscan of een cloud‑objectopslag eruitziet vanuit de code.</p>

<div class="codeblock" id="code">
 <h3>Scan een archief - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Streams en pipes, in en uit">}}

<p>Lezen en schrijven accepteren beide streams, en de asynchrone instappunten accepteren ook <code>System.IO.Pipelines</code>-types. Een study kan van een netwerk‑respons naar opslag reizen zonder dat het proces ooit het volledige bestand als één array vasthoudt.</p>

<div class="codeblock" id="code">
 <h3>Lezen en schrijven via streams - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>Ditzelfde idee geldt voor de tekstrepresentaties: een document met veel datasets wordt één dataset per keer gelezen op de <a href="/medical/net/json-to-dicom/">JSON naar DICOM</a> en <a href="/medical/net/xml-to-dicom/">XML naar DICOM</a> pagina's.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Frame voor frame">}}

<p>Multi‑frame gegevens worden per frame benaderd, zodat een serie van 500 frames één frame per keer kost in plaats van het volledige pixeldata‑element.</p>

<div class="codeblock" id="code">
 <h3>Loop door de frames - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Waar dit het ontwerp bepaalt">}}

<ul>
<li>Archief‑indexering en migratie: miljoenen bestanden, en alleen de header is relevant totdat er iets wordt verplaatst.</li>
<li>Routers en opslag‑nodes: accepteer een study, lees wat nodig is om deze te routeren, geef de bytes door.</li>
<li>AI‑pipelines: bouw het manifest op basis van metadata, en haal vervolgens frames voor de subset waarop daadwerkelijk getraind wordt.</li>
<li>Containers met een geheugenlimiet: de werkset volgt de strategie, niet de bestandsgrootte.</li>
<li>Whole slide‑ en OCT‑data: bestanden waarbij het lezen van alles geen optie is.</li>
</ul>

<p>De <a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">geheugenbeheergids</a> legt de strategieën in detail uit, en <a href="/medical/net/dicom-networking/">DICOM‑netwerken</a> toont dezelfde gegevens die via DIMSE aankomen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leermiddelen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Ontwikkelaarsgids" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
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
