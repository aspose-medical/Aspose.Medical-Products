---
title: Komprese DICOM JPEG 2000 v C# .NET | Aspose.Medical
weight: 2000
description: Čtěte, zapisujte a transkódujte soubory DICOM s kompresí JPEG 2000 v C# .NET. Podpora 8‑bitových barevných a 16‑bitových monochromatických snímků, bezztrátových i ztrátových režimů, plus HTJ2K s API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Podpora DICOM JPEG 2000 v .NET C#" h2="Čtěte, zapisujte a transkódujte soubory DICOM s kompresí JPEG 2000. Bezztrátové i ztrátové režimy, 8‑bitová barevná a 16‑bitová monochromatická data pixelů, HTJ2K zahrnuto – vše v čistém .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 v lékařském zobrazování">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) je nejrozšířenější standard komprese založený na vlnkových transformacích v lékařském zobrazování. Na rozdíl od tradičního JPEG nabízí jak bezztrátovou, tak ztrátovou kompresi v jediném kodeku, progresivní dekódování pro přístup k oblastem zájmu a vyšší poměry komprese &mdash; což jej činí ideálním pro archivaci rozsáhlých studií a přenos obrazu po omezených sítích.</p>

<p><strong>Aspose.Medical pro .NET</strong> poskytuje čistou implementaci JPEG 2000 kodeku v C# bez nativních závislostí. Knihovna dokáže číst, vykreslovat a transkódovat soubory DICOM komprimované libovolnou ze čtyř standardních JPEG 2000 přenosových syntaxí.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Podporované přenosové syntaxe JPEG 2000">}}

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
<tr><td>JPEG 2000 bezztrátové pouze</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Bezztrátový</td><td>8‑bit RGB, 16‑bit monochromatický</td><td>16‑bit monochromatický, 8‑bit RGB</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Ztrátový nebo bezztrátový</td><td>8‑bit RGB, 16‑bit monochromatický</td><td>16‑bit monochromatický, 8‑bit RGB</td></tr>
<tr><td>JPEG 2000 Part 2 Multi‑component bezztrátové pouze</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Bezztrátový</td><td>Ne podporováno</td><td>Ne podporováno</td></tr>
<tr><td>JPEG 2000 Part 2 Multi‑component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Ztrátový nebo bezztrátový</td><td>Ne podporováno</td><td>Ne podporováno</td></tr>
<tr><td>HTJ2K bezztrátové pouze</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Bezztrátový</td><td>Monochromatické a barevné</td><td>Monochromatické a barevné</td></tr>
<tr><td>HTJ2K s RPCL možnostmi bezztrátové pouze</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Bezztrátový</td><td>Monochromatické a barevné</td><td>Monochromatické a barevné</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Ztrátový nebo bezztrátový</td><td>Monochromatické a barevné</td><td>Monochromatické a barevné</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8‑bitová a 16‑bitová data pixelů">}}

<p>Lékařské snímky často používají 16 bitů na vzorek k zachycení plného dynamického rozsahu modalit jako CT (typicky 12‑bit uložené v 16‑bitovém formátu) a MRI. Aspose.Medical zpracovává oba bitové rozlišení pro JPEG 2000:</p>

<ul>
<li><strong>Čtení (dekomprese)</strong>: 16‑bitové monochromatické soubory (CT, MRI, rentgen) a 8‑bitové tříkomponentní barevné soubory (RGB, YBR_RCT, YBR_ICT). Paletové, CMYK, ICC‑profile a sub‑samplované barevné kodeky jsou odmítnuty s jasnou výjimkou místo tichého vytvoření špatného obrazu.</li>
<li><strong>Zápis (komprese)</strong>: 16‑bitové monochromatické a 8‑bitové RGB obrazy. 8‑bitové monochromatické a 16‑bitové barevné kódování nejsou k dispozici; pro ně použijte HTJ2K nebo JPEG XL, oba přijímají monochromatické i barevné obrazy v obou bitových hloubkách.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Číst a kontrolovat DICOM komprimovaný JPEG 2000 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Transkódovat na JPEG 2000">}}

<p>Použijte metodu <code>Transcode</code> pro kompresi libovolného souboru DICOM do JPEG 2000 nebo pro převod mezi režimy JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Komprimovat DICOM do JPEG 2000 bezztrátově - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Komprimovat DICOM do JPEG 2000 se ztrátou - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dekompresovat soubory DICOM JPEG 2000">}}

<p>Dekompresujte soubory JPEG 2000 do nekomprimované přenosové syntaxe pro zpracování, analýzu nebo kompatibilitu se systémy, které JPEG 2000 nepodporují:</p>

<div class="codeblock" id="code">
 <h3>Dekompresovat JPEG 2000 na nekomprimovaný formát - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Můžete také dekompresovat a transkódovat do jiných formátů komprese v jednom kroku:</p>

<div class="codeblock" id="code">
 <h3>Transkódovat mezi formáty komprese - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vykreslit DICOM obrazy JPEG 2000">}}

<p>Soubory DICOM komprimované JPEG 2000 lze vykreslit do pixelových dat pro zobrazení nebo export, stejně jako jakákoliv jiná přenosová syntaxe:</p>

<div class="codeblock" id="code">
 <h3>Vykreslit JPEG 2000 komprimovaný snímek - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Bezztrátové vs ztrátové JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Aspekt</th>
<th>JPEG 2000 bezztrátové</th>
<th>JPEG 2000 se ztrátou</th>
</tr>
</thead>
<tbody>
<tr><td>Přenosová syntaxe</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Kvalita obrazu</td><td>Pixel‑perfect &mdash; identické s originálem</td><td>Vizuelně podobné, některá data jsou trvale ztracena</td></tr>
<tr><td>Komprimační poměr</td><td>Obvykle 2:1 až 3:1</td><td>Obvykle 10:1 až 30:1 nebo vyšší</td></tr>
<tr><td>Nejlepší pro</td><td>Diagnostické archivování, právní záznamy, primární čtení</td><td>Úvodní revizi, telemedicínu, přenos přes síť</td></tr>
<tr><td>Bezpečný při zpětném převodu</td><td>Ano</td><td>Ne &mdash; opakované kódování dále snižuje kvalitu</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vysokorychlostní JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) nahrazuje pomalý aritmetický kodér JPEG 2000 rychlejším blokovým kodérem. Zachovává stejnou vlnkovou transformaci, pořadí progresí a kvalitu a dekóduje a kóduje několikanásobně rychleji. Aspose.Medical implementuje všechny tři DICOM HTJ2K přenosové syntaxe v čistém .NET, pro monochromatické i barevné obrazy, a transkóduje mezi HTJ2K a všemi ostatními podporovanými syntaxemi:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; pouze bezztrátové</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; bezztrátové s progresivním pořadím RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; ztrátový nebo bezztrátový</li>
</ul>

<div class="codeblock" id="code">
 <h3>Transkódovat JPEG 2000 na HTJ2K a zpět - C#</h3>
 <pre><code class="cs">// Transcode a JPEG 2000 file to HTJ2K, and back to classic JPEG 2000
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile htj2kFile = j2kFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2kFile.Save("htj2k_lossless.dcm");

// HTJ2K with RPCL progression order, lossless
DicomFile rpclFile = j2kFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
rpclFile.Save("htj2k_rpcl.dcm");

// HTJ2K lossy
DicomFile htj2kLossy = j2kFile.Transcode(TransferSyntax.HTJ2K);
htj2kLossy.Save("htj2k_lossy.dcm");

// Any HTJ2K file decodes back to an uncompressed transfer syntax
DicomFile uncompressed = htj2kFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decoded.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Výukové zdroje" tabId="resources" >}}
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
