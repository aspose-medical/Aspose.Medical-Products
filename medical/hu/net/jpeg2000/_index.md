---
title: DICOM JPEG 2000 tömörítés C# .NET-ben | Aspose.Medical
weight: 2000
description: Olvasás, írás és transzkódolás DICOM fájlok JPEG 2000 tömörítéssel C# .NET-ben. Támogatás 8‑bit és 16‑bit képekhez, veszteségmentes és veszteséges módokhoz, többkomponensű adatokhoz az Aspose.Medical API-val.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM JPEG 2000 támogatás .NET C#-ban" h2="Olvasás, írás és transzkódolás DICOM fájlok JPEG 2000 tömörítéssel. Veszteségmentes és veszteséges módok, 8‑bit és 16‑bit pixeladatok, többkomponensű képek — mind mindegyik tiszta .NET-ben." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 az orvosi képalkotásban">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) a legelterjedtebb hullámtranszformáció alapú tömörítési szabvány az orvosi képalkotásban. A hagyományos JPEG-től eltérően egyetlen kodekben kínál veszteségmentes és veszteséges tömörítést is, progresszív dekódolást a ROI eléréséhez, és kiváló tömörítési arányokat &mdash; ami ideálissá teszi nagy tanulmányok archiválására és képek továbbítására korlátozott hálózatokon.</p>

<p><strong>Aspose.Medical for .NET</strong> tiszta C# implementációt biztosít a JPEG 2000 kodekhez natív függőségek nélkül. A könyvtár képes olvasni, renderelni és transzkódolni a négy szabványos JPEG 2000 átvitel szintaxis egyikével tömörített DICOM fájlokat.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Támogatott JPEG 2000 átvitel szintaxisek">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Átvitel Szintaxis</th>
<th>UID</th>
<th>Mód</th>
<th>Olvasás</th>
<th>Írás</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 csak veszteségmentes</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Veszteségmentes</td><td>8‑bit és 16‑bit</td><td>8‑bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Veszteséges vagy veszteségmentes</td><td>8‑bit és 16‑bit</td><td>8‑bit</td></tr>
<tr><td>JPEG 2000 Part 2 Többkomponensű csak veszteségmentes</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Veszteségmentes</td><td>8‑bit és 16‑bit</td><td>8‑bit</td></tr>
<tr><td>JPEG 2000 Part 2 Többkomponensű</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Veszteséges vagy veszteségmentes</td><td>8‑bit és 16‑bit</td><td>8‑bit</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="8‑bit és 16‑bit pixeladatok">}}

<p>Az orvosi képek gyakran 16 bitet használnak mintavételezésre a módszerek (pl. CT, amely általában 12‑bitet tárol 16‑bitben) és MRI teljes dinamikatartományának rögzítéséhez. Az Aspose.Medical mindkét bitmélységet kezeli a JPEG 2000 esetén:</p>

<ul>
<li><strong>Olvasás (dekompresszió)</strong>: Teljes támogatás mind a 8‑bit, mind a 16‑bit JPEG 2000 tömörített DICOM fájlokhoz. A könyvtár helyesen dekódolja a pixeladatokat az eredeti Bits Allocated, Bits Stored és High Bit értékektől függetlenül.</li>
<li><strong>Írás (kompresszió)</strong>: Jelenleg 8‑bit képeket támogat. A 16‑bit írás támogatása egy későbbi kiadásban van tervezve.</li>
</ul>

<div class="codeblock" id="code">
 <h3>JPEG 2000 tömörített DICOM olvasása és ellenőrzése – C#</h3>
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

<p>Használja a <code>Transcode</code> metódust bármely DICOM fájl JPEG 2000-ra tömörítéséhez vagy a JPEG 2000 módok közötti átalakításhoz:</p>

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

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM fájlok dekompressziója">}}

<p>Dekompresszió JPEG 2000 fájlokra egy nem tömörített átvitel szintaxisra feldolgozás, elemzés vagy a JPEG 2000-at nem támogató rendszerekkel való kompatibilitás céljából:</p>

<div class="codeblock" id="code">
 <h3>JPEG 2000 dekompresszió nem tömörített formátumra – C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Egy lépésben is elvégezhető a dekompresszió és a transzkódolás más tömörítési formátumokra:</p>

<div class="codeblock" id="code">
 <h3>Transzkódolás a különböző tömörítési formátumok között – C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 DICOM képek renderelése">}}

<p>A JPEG 2000 tömörített DICOM fájlok pixeladatokká renderelhetők megjelenítés vagy export céljából, akárcsak bármely más átvitel szintaxis esetén:</p>

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
<th>Jellemző</th>
<th>JPEG 2000 veszteségmentes</th>
<th>JPEG 2000 veszteséges</th>
</tr>
</thead>
<tbody>
<tr><td>Átvitel Szintaxis</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Képminőség</td><td>Pixel‑tökéletes &mdash; azonos az eredetivel</td><td>Vizualisan hasonló, némi adat véglegesen elveszve</td></tr>
<tr><td>Tömörítési arány</td><td>Általában 2:1‑3:1</td><td>Általában 10:1‑30:1 vagy nagyobb</td></tr>
<tr><td>Legalkalmasabb</td><td>Diagnosztikai archiválás, jogi feljegyzések, elsődleges olvasás</td><td>Előzetes felülvizsgálat, telemedicina, hálózati átvitel</td></tr>
<tr><td>Körút biztonságos</td><td>Igen</td><td>Nem &mdash; az újrakódolás tovább rontja a minőséget</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 többkomponensű">}}

<p>A JPEG 2000 Part 2 (ISO/IEC 15444-2) bővíti a szabványos kodeket többkomponensú transzformációs képességekkel. Ezt színes orvosi képekhez és többcsatornás adatot előállító modalitásokhoz használják. Az Aspose.Medical mindkét Part 2 átvitel szintaxist támogatja:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; veszteségmentes tömörítés inter-komponens dekorelációval a többcsatornás adatok optimális tömörítéséhez.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; veszteséges vagy veszteségmentes tömörítés többkomponensú transzformációkkal.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Nagysebességű JPEG 2000 (HTJ2K) — hamarosan">}}

<p>Az HTJ2K (ISO/IEC 15444-15) a JPEG 2000 következő generációs kiterjesztése, amely drámai módon gyorsabb kódolási és dekódolási sebességet biztosít, miközben megőrzi a tömörítési hatékonyságot. Várhatóan a valós idejű orvosi képalkotási munkafolyamatok preferált kodeke lesz.</p>

<p>Az Aspose.Medical a jövőben hozzáadja az HTJ2K támogatást, három átvitel szintaxist lefedve:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; csak veszteségmentes</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; veszteségmentes RPCL progressziós sorrenddel</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; veszteséges vagy veszteségmentes</li>
</ul>

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
{{< blocks/products/pf/slr-element name="Fizetett támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
