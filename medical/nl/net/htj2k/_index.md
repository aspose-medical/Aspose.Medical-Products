---
title: HTJ2K in C# .NET - High-Throughput JPEG 2000 voor DICOM | Aspose.Medical
weight: 10000

description: Comprimeer en lees DICOM‑afbeeldingen in High-Throughput JPEG 2000 vanuit C#. Lossless HTJ2K, de RPCL‑variant en lossy HTJ2K, geïmplementeerd in managed .NET zonder native codec om te distribueren.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K in .NET C#" h2="High-Throughput JPEG 2000 voor DICOM: de compressie die de standaard heeft toegevoegd voor snelle archieven en cloud‑weergave, geïmplementeerd in managed C# zonder dat er native iets geïnstalleerd moet worden." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Wat HTJ2K verandert">}}

<p>High-Throughput JPEG 2000 behoudt de wavelet en de beeldkwaliteit van JPEG 2000 en vervangt het onderdeel dat het traag maakte. De blokcoder is nieuw, en decoderen is een orde van grootte sneller, waardoor de DICOM‑standaard het in drie transfer syntaxes heeft opgenomen en cloud‑imagingplatforms ernaar zijn overgestapt.</p>

<p>Voor een .NET‑team is de praktische vraag anders: wie kan die bestanden daadwerkelijk produceren. De meeste bibliotheken bereiken HTJ2K via een native OpenJPH‑build, wat een binaire per platform betekent, een build‑stap in de container en een afhankelijkheid waar de security‑review naar vraagt. <strong>Aspose.Medical for .NET</strong> implementeert de codec in managed code binnen hetzelfde pakket dat de bestanden leest en schrijft, zodat HTJ2K op dezelfde manier werkt op Windows, op Linux en in een container, zonder iets te installeren.</p>

<p>Drie transfer syntaxes worden ondersteund, en alle drie zowel lezen als schrijven:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), High-Throughput JPEG 2000 lossless.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), de lossless‑variant met de RPCL‑progressievolgorde.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Comprimeer een studie naar HTJ2K">}}

<p>Eén aanroep verplaatst een bestand naar de nieuwe syntax. De dataset, de private tags en de bestands‑meta‑informatie gaan mee.</p>

<div class="codeblock" id="code">
 <h3>Transcode een DICOM‑bestand naar HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>Op een 1714 × 1933 16‑bit afbeelding uit onze eigen testset gaat het bestand van 6,3 MB naar 2,9 MB, en de pixels komen bit‑voor‑bit terug. De cijfers verschillen per modaliteit en per afbeelding, dus meet op uw eigen data, wat één doorloop over de bestanden die u al heeft betekent.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossless betekent lossless">}}

<p>Diagnostische data tolereren geen codec die nauwelijks correct is. Transcode naar HTJ2K lossless en terug, en de pixeldata is identiek aan de bytes waarmee u begon, wat een eigenschap is die u kunt verifiëren in uw eigen testsuite voordat u akkoord gaat met het recomprimeren van een archief.</p>

<div class="codeblock" id="code">
 <h3>Terug naar een niet‑gecomprimeerde syntax - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, de variant ontworpen voor weergave via een netwerk">}}

<p>De syntax 1.2.840.10008.1.2.4.202 slaat dezelfde lossless‑codestream op in de RPCL‑progressievolgorde: eerst resolutie, dan positie, dan component, dan laag. Een lezer die alleen het begin van de stream neemt, krijgt een volledige lage‑resolutie‑afbeelding, wat een viewer nodig heeft wanneer hij een grote studie opent via een link die hij niet beheert.</p>

<div class="codeblock" id="code">
 <h3>Comprimeer met de RPCL‑progressievolgorde - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lees wat een archief u stuurt">}}

<p>De andere helft van de taak is het accepteren van HTJ2K van systemen die het al produceren. Open het bestand, controleer hoe het opgeslagen is, en werk met de pixeldata.</p>

<div class="codeblock" id="code">
 <h3>Lees een HTJ2K‑bestand - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Multi‑frame afbeeldingen worden frame‑voor‑frame verwerkt, zodat een lange reeks geheugen kost per frame in plaats van per studie.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Waar HTJ2K zich bewijst">}}

<ul>
<li>Archiefmigratie: recomprimeer een opgeslagen studie naar HTJ2K lossless, verklein de footprint, behoud de diagnostische data ongewijzigd.</li>
<li>Cloud en DICOMweb: de decodeersnelheid is wat een browser‑ of server‑viewer direct laat aanvoelen bij grote beelden.</li>
<li>AI‑pipelines: trainingsets worden veel vaker gelezen dan geschreven, en decodeertijd is de terugkerende kostenpost.</li>
<li>Containers en serverless: de codec maakt deel uit van de assembly, zodat een image geen native bibliotheek of compiler in de build nodig heeft.</li>
</ul>

<p>De bibliotheek bevat ook JPEG XL, de andere recente toevoeging aan de standaard, en de oudere codecs die een archief waarschijnlijk bevat: JPEG, JPEG‑LS, JPEG 2000 en RLE. De pagina <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> behandelt de volledige set, en de pagina <a href="/medical/net/jpeg2000/">JPEG 2000</a> behandelt de codec waar HTJ2K uit is voortgekomen.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leerbronnen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Ontwikkelaarsgids" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="API‑referenties" href="https://reference.aspose.com/medical/net/" >}}
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
