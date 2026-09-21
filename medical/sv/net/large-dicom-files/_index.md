---
title: Arbeta med stora DICOM‑filer i C# .NET | Aspose.Medical
weight: 11500

description: Öppna multi‑frame‑studier och hela skivbilder i C# utan att ladda dem i minnet. Läs metadata utan pixeldata, uppskjut stora element och flytta filer via strömmar och pipelines.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Stora DICOM‑filer i .NET C#" h2="Läs metadata för en multi‑frame‑studie utan pixlar, uppskjut stora element tills något begär dem, och flytta hela filer genom strömmar och pipelines." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Filen är stor, frågan är vanligtvis liten">}}

<p>En hel skivbild, en lång CT‑serie eller en OCT‑volym är hundratals megabyte, och det mesta är pixeldata. Det arbete en applikation faktiskt utför är ofta mycket mindre: lista vad som finns i en mapp, kontrollera en patientidentifierare, räkna ramarna, besluta var en studie ska placeras. Att ladda varje byte för att besvara detta är det som förvandlar ett enkelt jobb till ett minnesproblem.</p>

<p><strong>Aspose.Medical for .NET</strong> låter anroparen bestämma hur mycket av en fil som läses. Valet är ett argument på <code>DicomFile.Open</code>, och gäller både för filer, strömmar och pipelines.</p>

<p>Uppmätt på en 14 MB‑studie med 128 ramar från vårt testsats, på samma maskin och samma fil:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Lässtrategi</th>
<th>Öppningstid</th>
<th>Tilldelat minne</th>
</tr>
</thead>
<tbody>
<tr><td>Allt, standard</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Stora element hoppade över</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Stora element uppskjutna</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>Gapet ökar med filen. En mapp med 10 000 studier är fallet där det slutar vara en mikro‑optimering.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Läs metadata, lämna pixlarna orörda">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> lämnar varje element över en storlekströskel ute från läsningen. Datasetet som returneras har de taggar som ett index eller en router behöver.</p>

<div class="codeblock" id="code">
 <h3>Läs en studie utan dess pixeldata - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>Tröskeln är standard 64 kB och anges i kilobyte, så ett arbetsflöde som betraktar 8 kB som stort kan ange det.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Uppskjut istället för att hoppa över">}}

<p>När pixlarna kan behövas, men sannolikt senare och inte alla, är <code>ReadLargeOnDemand</code> den andra halvan av paret. Att öppna filen kostar lika mycket som att hoppa över, och ett stort element läses i det ögonblick koden berör det.</p>

<div class="codeblock" id="code">
 <h3>Läs in en ram endast när den används - C#</h3>
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

<p>Uppskjuten läsning är en licensierad funktion; de andra strategierna fungerar även i utvärderingsläget.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Indexera en mapp utan att röra pixlarna">}}

<p>Samma strategi gäller för en ström, vilket är vad en arkivskanning eller ett molnobjektlager ser ut som från koden.</p>

<div class="codeblock" id="code">
 <h3>Skanna ett arkiv - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Strömmar och pipelines, in och ut">}}

<p>Både läsning och skrivning accepterar strömmar, och de asynkrona ingångspunkterna accepterar även <code>System.IO.Pipelines</code>-typer. En studie kan färdas från ett nätverksrespons till lagring utan att processen någonsin håller hela filen som en enda array.</p>

<div class="codeblock" id="code">
 <h3>Läs och skriv via strömmar - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>Samma idé gäller för textrepresentationerna: ett dokument med många dataset läses ett dataset i taget på sidorna <a href="/medical/net/json-to-dicom/">JSON till DICOM</a> och <a href="/medical/net/xml-to-dicom/">XML till DICOM</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ram för ram">}}

<p>Multi‑frame‑data adresseras per ram, så en serie med 500 ramar kostar en ram i taget snarare än hela pixeldataelementet.</p>

<div class="codeblock" id="code">
 <h3>Gå igenom ramarna - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Där detta avgör designen">}}

<ul>
<li>Arkivindexering och migrering: miljontals filer, och bara headern är viktig tills något flyttas.</li>
<li>Routrar och lagringsnoder: ta emot en studie, läs vad som behövs för att routa den, skicka vidare bytes.</li>
<li>AI‑pipelines: bygg manifestet från metadata, och hämta sedan ramar för den delmängd som faktiskt tränas på.</li>
<li>Containrar med minnesgräns: arbetsuppsättningen följer strategin, inte filstorleken.</li>
<li>Hela skiv- och OCT‑data: filer där läsning av allt inte är ett alternativ över huvudtaget.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">Minneshanteringsguiden</a> förklarar strategierna i detalj, och <a href="/medical/net/dicom-networking/">DICOM‑nätverk</a> visar samma data som anländer via DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Utvecklarguide" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
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
