---
title: Komprese DICOM JPEG 2000 v C# .NET | Aspose.Medical
weight: 2000
description: Čtěte, zapisujte a transkódujte DICOM soubory s kompresí JPEG 2000 v C# .NET. Podpora 8‑bitových a 16‑bitových obrazů, bezztrátových i ztrátových režimů, vícekomponentních dat s API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Podpora DICOM JPEG 2000 v .NET C#" h2="Čtěte, zapisujte a transkódujte DICOM soubory s kompresí JPEG 2000. Bezztrátové i ztrátové režimy, 8‑bitová a 16‑bitová pixelová data, vícekomponentní obrazy — vše v čistém .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 v lékařské zobrazovací technice">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) je nejrozšířenější standard komprese založený na vlnkových transformacích v lékařském zobrazování. Na rozdíl od tradičního JPEG nabízí jak bezztrátovou, tak ztrátovou kompresi v jednom kodeku, progresivní dekódování pro přístup k oblastem zájmu a vyšší kompresní poměry &mdash; což jej činí ideálním pro archivaci rozsáhlých studií a přenos obrazů přes omezené sítě.</p>

<p><strong>Aspose.Medical pro .NET</strong> poskytuje čistou implementaci kodeku JPEG 2000 v C# bez nativních závislostí. Knihovna umí číst, vykreslovat a transkódovat DICOM soubory komprimované libovolnou ze čtyř standardních přenosových syntaxi JPEG 2000.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Podporované JPEG 2000 přenosové syntaxe">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Přenosová syntaxe</th>
<th>UID</th>
<th>Režim</th>
<th>Čtení</th>
<th>Zápis</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 pouze bezztrátové</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Bezztrátový</td><td>8‑bit a 16‑bit</td><td>8‑bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Ztrátový nebo bezztrátový</td><td>8‑bit a 16‑bit</td><td>8‑bit</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component (pouze bezztrátové)</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Bezztrátový</td><td>8‑bit a 16‑bit</td><td>8‑bit</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Ztrátový nebo bezztrátový</td><td>8‑bit a 16‑bit</td><td>8‑bit</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8‑bitová a 16‑bitová pixelová data">}}

<p>Lékařské obrazy často používají 16 bitů na vzorek k zachycení plného dynamického rozsahu modalit jako CT (obvykle 12‑bitové uložené v 16‑bitovém formátu) a MRI. Aspose.Medical podporuje oba bitové rozlišení pro JPEG 2000:</p>

<ul>
<li><strong>Čtení (dekomprese)</strong>: Úplná podpora jak 8‑bitových, tak 16‑bitových DICOM souborů komprimovaných JPEG 2000. Knihovna správně dekóduje pixelová data bez ohledu na původní hodnoty Bits Allocated, Bits Stored a High Bit.</li>
<li><strong>Zápis (komprese)</strong>: V současné době podporuje 8‑bitové obrazy. Podpora zápisu 16‑bitových dat je naplánována do budoucí verze.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Čtení a inspekce DICOM komprimovaného JPEG 2000 – C#</h3>
 <pre><code class="cs">// Open a JPEG 2000 compressed DICOM file (8-bit or 16-bit)
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Check transfer syntax
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax
Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");

// Inspect pixel data bit depth
PixelData pixelData = PixelData.Create(dicomFile.Dataset);
Console.WriteLine($"Bits Allocated: {pixelData.BitsAllocated}");
Console.WriteLine($"Bits Stored: {pixelData.BitsStored}");
Console.WriteLine($"High Bit: {pixelData.HighBit}");
Console.WriteLine($"Samples Per Pixel: {pixelData.SamplesPerPixel}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transkódovat do JPEG 2000">}}

<p>Použijte metodu <code>Transcode</code> pro kompresi libovolného DICOM souboru do JPEG 2000 nebo pro převod mezi režimy JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Kompresní DICOM do JPEG 2000 bezztrátově – C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Kompresní DICOM do JPEG 2000 se ztrátou – C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dekompresi DICOM souborů JPEG 2000">}}

<p>Dekompresujte soubory JPEG 2000 do nekomprimované přenosové syntaxe pro zpracování, analýzu nebo kompatibilitu se systémy, které JPEG 2000 nepodporují:</p>

<div class="codeblock" id="code">
 <h3>Dekompresovat JPEG 2000 do nekomprimovaného – C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Můžete také v jednom kroku dekomprimovat a transkódovat do jiných kompresních formátů:</p>

<div class="codeblock" id="code">
 <h3>Transkódovat mezi kompresními formáty – C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vykreslit DICOM obrazy JPEG 2000">}}

<p>Soubory DICOM komprimované JPEG 2000 lze vykreslit na pixelová data pro zobrazení nebo export, stejně jako u jiných přenosových syntaxi:</p>

<div class="codeblock" id="code">
 <h3>Vykreslit JPEG 2000 komprimovaný rámec – C#</h3>
 <pre><code class="cs">// Open JPEG 2000 DICOM file
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Render the first frame — decompression is handled automatically
using PixelImage&lt;Bgra32&gt; image = dicomFile.RenderImage(0);

// Copy the BGRA32 pixel data for display or export
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = new Bgra32[width * height];
image.CopyPixelsTo(pixels);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bezztrátové vs. ztrátové JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Aspekt</th>
<th>JPEG 2000 bezztrátové</th>
<th>JPEG 2000 se ztrátou</th>
</tr>
</thead>
<tbody>
<tr><td>Přenosová syntaxe</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Kvalita obrazu</td><td>Pixelově dokonalá &mdash; identická s originálem</td><td>Vizuálně podobná, některá data jsou trvale ztracena</td></tr>
<tr><td>Komprimační poměr</td><td>Obvykle 2:1 až 3:1</td><td>Obvykle 10:1 až 30:1 nebo vyšší</td></tr>
<tr><td>Nejvhodnější pro</td><td>Diagnostické archivy, právní záznamy, primární čtení</td><td>Předběžné hodnocení, telemedicína, přenos přes síť</td></tr>
<tr><td>Bezpečný při opakovaném kódování</td><td>Ano</td><td>Ne &mdash; opětovné kódování dále snižuje kvalitu</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 Multi-Component">}}

<p>JPEG 2000 Part 2 (ISO/IEC 15444-2) rozšiřuje standardní kodek o možnosti transformace více komponent. Používá se pro barevné lékařské obrazy a modality produkující vícekanálová data. Aspose.Medical podporuje obě přenosové syntaxe Part 2:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; bezztrátová komprese s interkomponentní dekorrelací pro optimální kompresi vícekanálových dat.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; ztrátová nebo bezztrátová komprese s vícekomponentními transformacemi.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K) — Již brzy">}}

<p>HTJ2K (ISO/IEC 15444-15) je generace rozšíření JPEG 2000 určená pro výrazně rychlejší kódování a dekódování při zachování stejné kompresní účinnosti. Očekává se, že se stane preferovaným kodekem pro pracovní postupy real‑time lékařského zobrazování.</p>

<p>Aspose.Medical přidá podporu HTJ2K v budoucí verzi, zahrnující tři přenosové syntaxe:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; Pouze bezztrátové</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; Bezztrátové s progresí RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; Ztrátové nebo bezztrátové</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Výukové materiály" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Zdrojový kód" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
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
