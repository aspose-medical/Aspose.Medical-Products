---
title: JPEG XL voor DICOM in C# .NET | Aspose.Medical
weight: 10500

description: Sla DICOM-afbeeldingen op in JPEG XL vanuit C#. Lossless JPEG XL die de pixels bit voor bit retourneert, in één managed assembly zonder native codec om te implementeren.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL voor DICOM in .NET C#" h2="De nieuwste compressie in de DICOM-standaard, met de kleinste lossless-bestanden die we hebben gemeten, geïmplementeerd in managed C# en geleverd in één assembly." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Waarom JPEG XL DICOM bereikte">}}

<p>Medische archieven groeien en krimpen nooit. JPEG XL is de codec die de beeldvormingswereld heeft ontworpen na twee decennia ervaring met JPEG en JPEG 2000, en DICOM heeft het toegevoegd als een transfer syntax om een reden waar opslagteams om geven: voor dezelfde pixels is het bestand kleiner.</p>

<p><strong>Aspose.Medical for .NET</strong> schrijft en leest JPEG XL via een C#-port van libjxl die in de bibliotheek zit. Het pakket levert één assembly, <code>Aspose.Medical.dll</code>, en geen native binary daarnaast, dus een codec zo nieuw wordt geen deployment‑project: dezelfde assembly draait op Windows, op Linux, op een build‑agent en in een container.</p>

<p>Twee transfer syntaxes dragen de pixels:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), voor diagnostische gegevens die onveranderd moeten terugkomen.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), voor gevallen waarin een kleiner bestand belangrijker is dan een exacte kopie.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Comprimeer een studie, behoud elke pixel">}}

<p>Transcodering is één oproep, en de dataset rond de pixels reist mee.</p>

<div class="codeblock" id="code">
 <h3>Transcodeer een DICOM‑bestand naar JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>We hebben het gemeten op een 1714 bij 1933 16‑bit afbeelding uit onze eigen testset: 6,3 MB ongecomprimeerd wordt 2,7 MB in JPEG XL lossless, wat kleiner is dan dezelfde afbeelding in HTJ2K lossless. Uw eigen cijfers hangen af van de modaliteit, dus voer de vergelijking uit over een map met uw bestanden voordat u beslist.</p>

<p>Lossless is hier letterlijk te nemen. Transcodeer naar JPEG XL en terug, en de pixeldata is gelijk aan de bytes waarmee u begon, zodat een archief kan worden opnieuw gecomprimeerd zonder discussie over diagnostische kwaliteit.</p>

<div class="codeblock" id="code">
 <h3>Terug naar een ongecomprimeerde syntax - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lees wat al is opgeslagen als JPEG XL">}}

<p>Een bestand dat in JPEG XL arriveert opent zoals elk ander. De transfer syntax geeft aan wat het is, en de pixeldata is beschikbaar zodra het frame is gedecodeerd.</p>

<div class="codeblock" id="code">
 <h3>Open een JPEG XL‑bestand - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL of HTJ2K">}}

<p>Beiden zijn recent, beiden zijn lossless wanneer u lossless vraagt, en de bibliotheek schrijft en leest beide. Ze beantwoorden verschillende vragen.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Vraag</th>
<th>Antwoord</th>
</tr>
</thead>
<tbody>
<tr><td>Welke de kleinere file produceerde in onze test</td><td>JPEG XL lossless, met enkele procenten</td></tr>
<tr><td>Welke is gebouwd voor progressief bekijken over een netwerk</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, met name de RPCL‑variant</td></tr>
<tr><td>Welke eerst in de DICOM-standaard is opgenomen</td><td>HTJ2K, dus meer archieven accepteren het nu</td></tr>
<tr><td>Welke een native afhankelijkheid kost hier</td><td>Geen van beide, beide zijn managed code in één assembly</td></tr>
</tbody>
</table>

<p>De keuze komt meestal van de andere kant van de link: transcodeer naar de syntax die het archief accepteert, en houd de rest van de pijplijn hetzelfde.</p>

<div class="codeblock" id="code">
 <h3>Laat het doelarchief beslissen - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Waar het zich uitbetaalt">}}

<ul>
<li>Langetermijnarchieven: dezelfde studies, minder terabytes, en geen verlies om te verantwoorden aan een radioloog.</li>
<li>Cloudopslagfacturen: de besparing herhaalt zich elke maand, terwijl de transcoding één keer draait.</li>
<li>Datasets voor onderzoek en AI: kleinere kopieën bewegen sneller tussen opslag en training.</li>
<li>Implementatie: een codec zo nieuw betekent normaal een native build per platform; hier is het onderdeel van de assembly die u al referereert.</li>
</ul>

<p>De bibliotheek schrijft ook de codecs waar een bestaand archief vol van is: JPEG, JPEG-LS, JPEG 2000, HTJ2K en RLE. De pagina <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> behandelt de volledige set, <a href="/medical/net/htj2k/">HTJ2K</a> heeft zijn eigen pagina, en <a href="/medical/net/jpeg2000/">JPEG 2000</a> is waar beide nieuwe codecs vandaan komen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leermaterialen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Ontwikkelaarsgids" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
