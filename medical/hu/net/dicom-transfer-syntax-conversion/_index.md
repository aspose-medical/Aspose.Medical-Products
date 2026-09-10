---
title: DICOM átviteli szintaxis konvertálás C# .NET-ben | Aspose.Medical
weight: 16000
description: Transzkódolja a DICOM fájlokat a különböző átviteli szintaxisok között C# .NET-ben. Támogatja a JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, RLE és a tömörítetlen formátumokat az Aspose.Medical API-val.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM átviteli szintaxis konvertálás .NET C#-ban" h2="Transzkódolja a DICOM fájlokat a tömörítetlen, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS és RLE átviteli szintaxisok között. Tiszta .NET könyvtár, natív függőségek nélkül." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Mi az átviteli szintaxis?">}}

<p>A <strong>Transfer Syntax</strong> meghatározza, hogyan kódolják a DICOM adatokat tárolás és továbbítás céljából. Három kulcsfontosságú szempontot specifikál: a byte sorrendet (endianness), hogy a Value Representation‑ök explicit vagy implicit módon vannak-e ábrázolva, valamint a pixeladatokra alkalmazott tömörítési algoritmust. Minden DICOM fájl a File Meta Information fejlécben deklarálja az átviteli szintaxisát.</p>

<p>Különböző orvosi eszközök, PACS szerverek és megjelenítő alkalmazások különböző átviteli szintaxis készleteket támogatnak. <strong>Aspose.Medical for .NET</strong> biztosítja a <code>Transcode</code> metódust az átviteli szintaxisok közötti átalakításhoz, lehetővé téve az interoperabilitást, a tárolás optimalizálását és a feldolgozó eszközökkel való kompatibilitást – mindezt egy tiszta .NET könyvtárban, natív függőségek nélkül.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transzkódolja a DICOM fájlt C#-ban">}}

<p>A <code>DicomFile.Transcode</code> metódus egy DICOM fájlt a jelenlegi átviteli szintaxisából bármely támogatott cél szintaxisba konvertál. A metódus egy új <code>DicomFile</code> példányt ad vissza – az eredeti változat érintetlen marad:</p>

<div class="codeblock" id="code">
 <h3>Alap DICOM transzkódolás – C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Transzkódolhatja közvetlenül a <code>Dataset</code> szinten is:</p>

<div class="codeblock" id="code">
 <h3>Dataset transzkódolása – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Támogatott átviteli szintaxisok">}}

<p>Az alábbi táblázat felsorolja az összes szabványos DICOM képadat-átviteli szintaxist és azok jelenlegi támogatási állapotát az Aspose.Medical for .NET-ben. Az összes támogatott kodek tiszta C#-ban van megvalósítva, és teljesen platformfüggetlen.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Átviteli szintaxis</th>
<th>UID</th>
<th>Típus</th>
<th>Állapot</th>
</tr>
</thead>
<tbody>
<tr><td colspan=\"4\"><strong>Tömörítetlen</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Tömörítetlen</td><td>Támogatott</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Tömörítetlen</td><td>Támogatott</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Tömörítetlen (elavult)</td><td>Támogatott</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Tömörítetlen</td><td>Nem támogatott</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Támogatott</td></tr>
<tr><td colspan=\"4\"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Veszteséges, 8-bit</td><td>Támogatott</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Veszteséges, 12-bit</td><td>Nem támogatott</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Veszteségmentes</td><td>Támogatott (csak 8-bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Veszteségmentes</td><td>Támogatott (csak 8-bit)</td></tr>
<tr><td colspan=\"4\"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Veszteségmentes</td><td>Támogatott</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Közel veszteségmentes</td><td>Támogatott</td></tr>
<tr><td colspan=\"4\"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Veszteségmentes</td><td>Támogatott (olvasás 8-bit szín és 16-bit monokróm; írás 16-bit monokróm vagy 8-bit RGB)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Veszteséges vagy veszteségmentes</td><td>Támogatott (olvasás 8-bit szín és 16-bit monokróm; írás 16-bit monokróm vagy 8-bit RGB)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Veszteségmentes</td><td>Nem támogatott</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Veszteséges vagy veszteségmentes</td><td>Nem támogatott</td></tr>
<tr><td colspan=\"4\"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Veszteségmentes</td><td>Támogatott</td></tr>
<tr><td colspan=\"4\"><strong>Nagy áteresztőképességű JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Veszteségmentes</td><td>Támogatott</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Veszteségmentes</td><td>Támogatott</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Veszteséges vagy veszteségmentes</td><td>Támogatott</td></tr>
<tr><td colspan=\"4\"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Veszteségmentes</td><td>Támogatott</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Veszteségmentes</td><td>Csak dekódolás (a kódoláshoz JPEG forrásfolyam szükséges, nem pixeladat)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Veszteséges vagy veszteségmentes</td><td>Támogatott (veszteséges mód)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gyakori transzkódolási forgatókönyvek">}}

<p>Különböző munkafolyamatok különböző transzkódolási stratégiákat igényelnek. Íme a leggyakoribb forgatókönyvek:</p>

<div class="codeblock" id="code">
 <h3>Dekompresszió feldolgozáshoz – C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Tömörítés archiválási tároláshoz – C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Tömörítés hálózati átvitelhez – C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Legújabb kodekek használata: HTJ2K és JPEG XL – C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Átviteli szintaxis tulajdonságainak vizsgálata">}}

<p>A <code>TransferSyntax</code> osztály olyan tulajdonságokat tesz közzé, amelyek leírják a kódolás jellemzőit. Ezeket használhatja egy fájl aktuális átviteli szintaxisának vizsgálatához vagy a megfelelő cél szintaxis kiválasztásához:</p>

<div class="codeblock" id="code">
 <h3>Átviteli szintaxis tulajdonságok olvasása – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>Tulajdonság</th>
<th>Típus</th>
<th>Leírás</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>Az átviteli szintaxis egyedi azonosítója</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>A Value Representation‑ek explicit módon vannak‑e kódolva</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>A byte sorrend kis-endian‑e</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>A pixeladat kapszulázott (tömörített)‑e</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>A tömörítési módszer veszteséges‑e</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>A szintaxis deflate tömörítést használ‑e</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Az átviteli szintaxis a DICOM szabványban elavult‑e</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>A veszteséges tömörítési módszer ISO szabvány azonosítója</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Veszteséges vs veszteségmentes tömörítés">}}

<p>A veszteséges és veszteségmentes tömörítés közötti különbség megértése kritikus a DICOM fájlok transzkódolásakor:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Szempont</th>
<th>Veszteségmentes</th>
<th>Veszteséges</th>
</tr>
</thead>
<tbody>
<tr><td>Képminőség</td><td>Pixel‑tökéletes &mdash; az eredeti adat teljesen megmarad</td><td>Néhány adat véglegesen elveszik a kisebb méret érdekében</td></tr>
<tr><td>Tömörítési arány</td><td>Általában 2:1‑től 3:1‑ig</td><td>Általában 10:1‑től 30:1‑ig vagy magasabb</td></tr>
<tr><td>Körkörös visszaolvasás biztonságos</td><td>Igen &mdash; dekompresszió után azonos pixelek</td><td>Nem &mdash; minden veszteséges újrakódolás tovább rontja a minőséget</td></tr>
<tr><td>Felhasználási esetek</td><td>Archiválás, diagnosztika, jogi nyilvántartások</td><td>Előzetes áttekintés, távgyógyászat, hálózati átvitel</td></tr>
<tr><td>Támogatott kodekek</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>Fontos:</strong> A veszteségesen tömörített fájlból veszteségmentes szintaxisba történő transzkódolás nem állítja vissza a elveszett adatokat. Az eredeti veszteséges tömörítés miatti minőségromlás végleges.</p>

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
{{< blocks/products/pf/slr-element name="Ügyféllista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
