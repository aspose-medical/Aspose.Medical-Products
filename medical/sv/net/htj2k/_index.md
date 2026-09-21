---
title: HTJ2K i C# .NET - High-Throughput JPEG 2000 för DICOM | Aspose.Medical
weight: 10000

description: Komprimera och läs DICOM‑bilder i High-Throughput JPEG 2000 från C#. Förlustfri HTJ2K, RPCL‑varianten och HTJ2K med förlust, implementerad i hanterad .NET utan någon inbyggd codec att distribuera.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K i .NET C#" h2="High-Throughput JPEG 2000 för DICOM: den komprimering standarden lade till för snabba arkiv och molnvisning, implementerad i hanterad C# utan någon inbyggd komponent att installera." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Vad HTJ2K förändrar">}}

<p>High-Throughput JPEG 2000 behåller waveleten och bildkvaliteten i JPEG 2000 och ersätter den del som gjorde den långsam. Blockkodaren är ny, och avkodning är en tiopotens snabbare, vilket är varför DICOM‑standarden antog den i tre transfer syntaxer och varför molnbaserade bildplattformar har gått över till den.</p>

<p>För ett .NET‑team är den praktiska frågan annorlunda: vem som faktiskt kan producera dessa filer. De flesta bibliotek når HTJ2K via en inbyggd OpenJPH‑byggnad, vilket innebär en binär per plattform, ett byggsteg i containern och ett beroende som säkerhetsgranskningen kommer att fråga om. <strong>Aspose.Medical for .NET</strong> implementerar codec:en i hanterad kod i samma paket som läser och skriver filerna, så fungerar HTJ2K likadant på Windows, på Linux och i en container, utan något att installera.</p>

<p>Tre transfer syntaxer stöds, och alla tre både läses och skrivs:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), High-Throughput JPEG 2000 förlustfri.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), den förlustfria varianten med RPCL‑progressionsordning.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Komprimera en studie till HTJ2K">}}

<p>Ett anrop flyttar en fil till den nya syntaxen. Datasetet, de privata taggarna och filens metainformation följer med.</p>

<div class="codeblock" id="code">
 <h3>Kodkonvertera en DICOM‑fil till HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>På en 1714×1933 16‑bit bild från vår egen testuppsättning går filen från 6,3 MB till 2,9 MB, och pixlarna återkommer bit för bit. Siffrorna varierar per modalitet och per bild, så mät på dina egna data, vilket är en loop över de filer du redan har.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Förlustfri betyder förlustfri">}}

<p>Diagnostiska data tolererar inte en codec som bara är nästan korrekt. Koda om till HTJ2K förlustfri och tillbaka, och pixeldata är identisk med de byte du började med, vilket är en egenskap du kan verifiera i din egen testsvit innan du godkänner att recomprimera ett arkiv.</p>

<div class="codeblock" id="code">
 <h3>Tillbaka till en okomprimerad syntax - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, varianten skapad för visning över ett nätverk">}}

<p>Syntaxen 1.2.840.10008.1.2.4.202 lagrar samma förlustfria kodström i RPCL‑progressionsordning: upplösning först, sedan position, sedan komponent, sedan lager. En läsare som bara tar början av strömmen får en komplett lågupplöst bild, vilket är vad en visare behöver när den öppnar en stor studie via en länk den inte kontrollerar.</p>

<div class="codeblock" id="code">
 <h3>Komprimera med RPCL‑progressionsordning - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Läs vad ett arkiv skickar dig">}}

<p>Den andra delen av uppgiften är att acceptera HTJ2K från system som redan producerar det. Öppna filen, kontrollera hur den är lagrad, och arbeta med pixeldata.</p>

<div class="codeblock" id="code">
 <h3>Läs en HTJ2K‑fil - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Multiram-bilder hanteras ram för ram, så en lång serie kostar minne per ram snarare än per studie.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Var HTJ2K förtjänar sin plats">}}

<ul>
<li>Arkivmigration: recomprimera en lagrad studie till HTJ2K förlustfri, minska fotavtrycket, behåll diagnostiska data intakta.</li>
<li>Moln och DICOMweb: avkodningshastigheten är det som får en webbläsar‑ eller server‑visare att kännas omedelbar på stora bilder.</li>
<li>AI‑pipelines: träningsset läses mycket oftare än de skrivs, och avkodningstid är kostnaden som återkommer.</li>
<li>Containrar och serverlös: codec:en är en del av samlingen, så en bild behöver ingen native‑bibliotek eller kompilator i byggsteget.</li>
</ul>

<p>Biblioteket levereras också med JPEG XL, den andra senaste tillsättningen till standarden, samt de äldre codec:arna ett arkiv sannolikt innehåller: JPEG, JPEG‑LS, JPEG 2000 och RLE. Sidan <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> täcker hela settet, och sidan <a href="/medical/net/jpeg2000/">JPEG 2000</a> täcker codec:en som HTJ2K utvecklades från.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Utvecklardokumentation" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="API‑referenser" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktsupport" tabId="support" >}}
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
