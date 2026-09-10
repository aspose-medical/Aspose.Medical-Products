---
title: DICOM JPEG 2000 tömörítés C# .NET-ben | Aspose.Medical
weight: 2000
description: Olvassa, írja és transzkódolja a DICOM fájlokat JPEG 2000 tömörítéssel C# .NET-ben. Támogatás 8 bites színes és 16 bites monokróm képekhez, veszteségmentes és veszteséges módok, valamint HTJ2K az Aspose.Medical API-val.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM JPEG 2000 támogatás .NET C#-ban" h2="Olvassa, írja és transzkódolja a DICOM fájlokat JPEG 2000 tömörítéssel. Veszteségmentes és veszteséges módok, 8 bites szín és 16 bites monokróm pixeladat, HTJ2K beépítve – mindegyik tiszta .NET- környezetben." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 az orvosi képalkotásban">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) a legelterjedtebb hullámtörzs-alapú tömörítési szabvány az orvosi képalkotásban. A hagyományos JPEG-től eltérően egyszerre kínál veszteségmentes és veszteséges tömörítést egyetlen kodekben, progresszív dekódolást a érdeklődési terület (ROI) eléréséhez, és kiemelkedő tömörítési arányokat &mdash; ami ideálissá teszi nagy tanulmányok archiválásához és képek továbbításához korlátozott hálózatokon.</p>

<p><strong>Aspose.Medical for .NET</strong> tiszta C# megvalósítást biztosít a JPEG 2000 kodekről, natív függőségek nélkül. A könyvtár képes olvasni, megjeleníteni és transzkódolni a DICOM fájlokat, amelyeket bármelyik a négy szabványos JPEG 2000 átviteli szintaxis közül tömörít.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Támogatott JPEG 2000 átviteli szintaxisok">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Átviteli szintaxis</th>
<th>UID</th>
<th>Mód</th>
<th>Olvasás</th>
<th>Írás</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 csak veszteségmentes</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Veszteségmentes</td><td>8 bites RGB, 16 bites monokróm</td><td>16 bites monokróm, 8 bites RGB</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Veszteséges vagy veszteségmentes</td><td>8 bites RGB, 16 bites monokróm</td><td>16 bites monokróm, 8 bites RGB</td></tr>
<tr><td>JPEG 2000 Part 2 Többkomponensú csak veszteségmentes</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Veszteségmentes</td><td>Nem támogatott</td><td>Nem támogatott</td></tr>
<tr><td>JPEG 2000 Part 2 Többkomponensú</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Veszteséges vagy veszteségmentes</td><td>Nem támogatott</td><td>Nem támogatott</td></tr>
<tr><td>HTJ2K csak veszteségmentes</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Veszteségmentes</td><td>Monokróm és színes</td><td>Monokróm és színes</td></tr>
<tr><td>HTJ2K RPCL opciókkal csak veszteségmentes</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Veszteségmentes</td><td>Monokróm és színes</td><td>Monokróm és színes</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Veszteséges vagy veszteségmentes</td><td>Monokróm és színes</td><td>Monokróm és színes</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8 bites és 16 bites pixeladat">}}

<p>Az orvosi képek gyakran 16 bitet használnak mintavételenként a modalitások, például a CT (általában 12-bit tárolva 16-bitben) és az MRI teljes dinamikus tartományának rögzítéséhez. Az Aspose.Medical kezeli mindkét bitmélységet a JPEG 2000 esetén:</p>

<ul>
<li><strong>Olvasás (dekompresszió)</strong>: 16 bites monokróm fájlok (CT, MRI, röntgen) és 8 bites háromkomponensű színes fájlok (RGB, YBR_RCT, YBR_ICT). Paletta, CMYK, ICC-profil és al-mintavételezett színkód áramlások elutasításra kerülnek egyértelmű kivétellel a hallgatólagosan hibás kép helyett.</li>
<li><strong>Írás (kompresszió)</strong>: 16 bites monokróm és 8 bites RGB képek. 8 bites monokróm és 16 bites színes kódolás nem érhető el; ilyen esetben használjon HTJ2K vagy JPEG XL megoldást, melyek mindkettőnek támogatják a monokróm és színes adatot bármely bitmélységben.</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 tömörítésű DICOM olvasása és ellenőrzése – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Transzkódolás JPEG 2000-ra">}}

<p>A <code>Transcode</code> metódus használatával bármely DICOM fájlt JPEG 2000-ra tömöríthet vagy átalakíthat a JPEG 2000 különböző módjai között:</p>

<div class="codeblock" id="code">
 <h3>DICOM tömörítése JPEG 2000 veszteségmentes módra – C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>DICOM tömörítése JPEG 2000 veszteséges módra – C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM fájlok dekódolása">}}

<p>A JPEG 2000 fájlok dekódolása egy nem tömörített átviteli szintaxisra feldolgozás, elemzés vagy a JPEG 2000-at nem támogató rendszerekkel való kompatibilitás érdekében:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 dekódolása nem tömörített formára – C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Egy lépésben is dekódolhat és transzkódolhat más tömörítési formátumokra:</p>

<div class="codeblock" id="code">
 <h3>Többlet formátumok közötti transzkódolás – C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM képek renderelése">}}

<p>A JPEG 2000 tömörítésű DICOM fájlok renderelhetők pixeladatokká megjelenítés vagy export céljából, akárcsak bármely más átviteli szintaxis esetén:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 tömörített keret renderelése – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Veszteségmentes vs veszteséges JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Szempont</th>
<th>JPEG 2000 veszteségmentes</th>
<th>JPEG 2000 veszteséges</th>
</tr>
</thead>
<tbody>
<tr><td>Átviteli szintaxis</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Képminőség</td><td>Pixel-pontos &mdash; azonos az eredetivel</td><td>Vizuálisan hasonló, néhány adat véglegesen elveszik</td></tr>
<tr><td>Tömörítési arány</td><td>Általában 2:1‑től 3:1‑ig</td><td>Általában 10:1‑től 30:1‑ig vagy magasabb</td></tr>
<tr><td>Legalkalmasabb</td><td>Diagnosztikai archiválás, jogi feljegyzések, elsődleges olvasás</td><td>Előzetes áttekintés, távgyógyászat, hálózati továbbítás</td></tr>
<tr><td>Körutazás biztonságos</td><td>Igen</td><td>Nem &mdash; az újrakódolás tovább rontja a minőséget</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Magas áteresztőképességű JPEG 2000 (HTJ2K)">}}

<p>Az HTJ2K (ISO/IEC 15444-15) a JPEG 2000 lassú aritmetikai kódolója helyett gyorsabb blokk-kódolót használ. Megtartja a ugyanazt a hullámtranszformációt, a progressziós sorrendet és a minőséget, és több szorozva gyorsabban dekódol és kódol. Az Aspose.Medical megvalósítja a három DICOM HTJ2K átviteli szintaxist tiszta .NET-ben, monokróm és színes képekhez, és transzkódol a HTJ2K és minden más támogatott szintaxis között:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; csak veszteségmentes</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; veszteségmentes RPCL progressziós sorrenddel</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; veszteséges vagy veszteségmentes</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 transzkódolása HTJ2K-ra és vissza – C#</h3>
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
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Forráskód" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetős támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért az Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
