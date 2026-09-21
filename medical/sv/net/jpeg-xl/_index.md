---
title: JPEG XL för DICOM i C# .NET | Aspose.Medical
weight: 10500

description: Lagra DICOM‑bilder i JPEG XL från C#. Förlustfri JPEG XL som returnerar pixlarna bit för bit, i en enda hanterad samling utan någon inbyggd codec att distribuera.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL för DICOM i .NET C#" h2="Den senaste komprimeringen i DICOM‑standarden, med de minsta förlustfria filerna vi har mätt, implementerad i hanterad C# och levererad i en enda samling." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Varför JPEG XL nådde DICOM">}}

<p>Medicinska arkiv växer och krymper aldrig. JPEG XL är den codec som bildvärlden designade efter två decennier av erfarenhet av JPEG och JPEG 2000, och DICOM lade till den som en överföringssyntaks av den anledning som lagringsteamen bryr sig om: för samma pixlar är filen mindre.</p>

<p><strong>Aspose.Medical för .NET</strong> skriver och läser JPEG XL via en C#‑port av libjxl som finns inuti biblioteket. Paketet levererar en ensam samling, <code>Aspose.Medical.dll</code>, och ingen inbyggd binär fil bredvid den, så en så ny codec blir inte ett deploymentsprojekt: samma samling körs på Windows, på Linux, på en byggagent och i en container.</p>

<p>Två överföringssyntakser transporterar pixlarna:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), för diagnostisk data som måste återvända oförändrad.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), för de fall där en mindre fil är viktigare än en exakt kopia.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Komprimera en studie, behåll varje pixel">}}

<p>Transkodning är ett anrop, och datasetet runt pixlarna följer med.</p>

<div class="codeblock" id="code">
 <h3>Transkoda en DICOM‑fil till JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Vi mätte det på en 1714 × 1933 16‑bits bild från vår egen testuppsättning: 6,3 MB okomprimerad blir 2,7 MB i JPEG XL förlustfri, vilket är mindre än samma bild i HTJ2K förlustfri. Dina egna siffror beror på modaliteten, så kör jämförelsen över en mapp med dina filer innan du väljer.</p>

<p>Förlustfri är ordet som bör tas bokstavligt här. Transkoda till JPEG XL och tillbaka, och pixeldata är exakt de bytes du började med, så ett arkiv kan recomprimeras utan diskussion om diagnostisk kvalitet.</p>

<div class="codeblock" id="code">
 <h3>Tillbaka till en okomprimerad syntaks - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Läs det som redan är lagrat som JPEG XL">}}

<p>En fil som anländer i JPEG XL öppnas likt alla andra. Transfer‑syntaxen anger vad den är, och pixeldata blir tillgänglig när ramen har dekodats.</p>

<div class="codeblock" id="code">
 <h3>Öppna en JPEG XL‑fil - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL eller HTJ2K">}}

<p>Båda är moderna, båda är lossless när du begär lossless, och biblioteket skriver och läser båda. De svarar på olika frågor.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Fråga</th>
<th>Svar</th>
</tr>
</thead>
<tbody>
<tr><td>Vilken producerade den mindre filen i vårt test</td><td>JPEG XL lossless, med några procent</td></tr>
<tr><td>Vilken är byggd för progressiv visning över ett nätverk</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, speciellt RPCL‑varianten</td></tr>
<tr><td>Vilken kom in i DICOM‑standarden först</td><td>HTJ2K, så fler arkiv accepterar den idag</td></tr>
<tr><td>Vilken kostar ett inbyggt beroende här</td><td>Ingen, båda är hanterad kod i en ensam samling</td></tr>
</tbody>
</table>

<p>Valet kommer vanligtvis från den andra sidan av länken: transkoda till den syntaks som arkivet accepterar, och behåll resten av pipeline samma.</p>

<div class="codeblock" id="code">
 <h3>Låt målarkivet bestämma - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="När det lönar sig">}}

<ul>
<li>Långtidsarkiv: samma studier, färre terabyte, och ingen förlust att motivera för en radiolog.</li>
<li>Molnlagringskostnader: besparingen återkommer varje månad, medan transkodningen bara körs en gång.</li>
<li>Datamängder för forskning och AI: mindre kopior flyttar snabbare mellan lagring och träning.</li>
<li>Distribution: en codec så ny betyder vanligtvis en inbyggd byggnad per plattform; här är den en del av samlingen du redan refererar till.</li>
</ul>

<p>Biblioteket skriver också de codecs ett befintligt arkiv är fullt av: JPEG, JPEG‑LS, JPEG 2000, HTJ2K och RLE. Sidan <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> täcker hela uppsättningen, <a href="/medical/net/htj2k/">HTJ2K</a> har sin egen sida, och <a href="/medical/net/jpeg2000/">JPEG 2000</a> är där båda de nya codecs kommer ifrån.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Utvecklarguide" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
