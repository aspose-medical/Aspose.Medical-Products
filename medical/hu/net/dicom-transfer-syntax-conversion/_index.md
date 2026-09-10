---
title: DICOM átviteli szintaxis konverzió C# .NET-ben | Aspose.Medical
weight: 16000
description: Átkódolja a DICOM fájlokat különböző átviteli szintaxisok között C# .NET környezetben. Támogatja a JPEG, JPEG 2000, JPEG-LS, RLE és a tömörítetlen formátumokat az Aspose.Medical API-val.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM átviteli szintaxis konverzió .NET C#-ban" h2="Átkódolja a DICOM fájlokat a tömörítetlen, JPEG, JPEG 2000, JPEG-LS és RLE átviteli szintaxisok között. Tiszta .NET könyvtár natív függőségek nélkül." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Mi az átviteli szintaxis?">}}

<p>A <strong>Transfer Syntax</strong> meghatározza, hogy a DICOM adat hogyan van kódolva tárolásra és átvitelre. Három fő szempontot rögzít: a bájtsorrendet (endianness), hogy a Value Representation-ök explicitek vagy implicitek, valamint a képadatokra alkalmazott tömörítési algoritmust. Minden DICOM fájl a File Meta Information fejlécekben deklarálja az átviteli szintaxisát.</p>

<p>Különböző orvosi eszközök, PACS szerverek és megjelenítő alkalmazások különböző átviteli szintaxis készleteket támogatnak. <strong>Aspose.Medical for .NET</strong> a <code>Transcode</code> metódust biztosítja az átviteli szintaxisok közötti konverzióhoz, elősegítve az interoperabilitást, a tárolás optimalizálását és az feldolgozó eszközökkel való kompatibilitást &mdash; mindezt natív függőségek nélküli tiszta .NET könyvtárban.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM fájl átkódolása C#-ban">}}

<p>A <code>DicomFile.Transcode</code> metódus egy DICOM fájlt a jelenlegi átviteli szintaxisából egy tetszőleges támogatott cél szintaxisba konvertál. A metódus egy új <code>DicomFile</code> példányt ad vissza &mdash; az eredeti változat változatlan marad:</p>

<div class="codeblock" id="code">
 <h3>Alapvető DICOM átkódolás – C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Közvetlenül a <code>Dataset</code> szinten is átkódolhat:</p>

<div class="codeblock" id="code">
 <h3>Dataset átkódolása – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Támogatott átviteli szintaxisok">}}

<p>Az alábbi táblázat felsorolja az összes szabványos DICOM képadat-átviteli szintaxist és azok jelenlegi támogatási állapotát az Aspose.Medical for .NET-ben. Minden támogatott kodek tiszta C#-ban van megvalósítva, és platformfüggetlen.</p>

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
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Tömörítetlen</td><td>Támogatott</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Támogatott</td></tr>
<tr><td colspan=\"4\"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Veszteséges, 8‑bit</td><td>Támogatott</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Veszteséges, 12‑bit</td><td>Nem támogatott</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Veszteségmentes</td><td>Támogatott (csak 8‑bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Veszteségmentes</td><td>Támogatott (csak 8‑bit)</td></tr>
<tr><td colspan=\"4\"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Veszteségmentes</td><td>Támogatott</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Közel vesztésmentes</td><td>Támogatott</td></tr>
<tr><td colspan=\"4\"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Veszteségmentes</td><td>Támogatott (olvasás 8/16‑bit, írás 8‑bit)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Veszteséges vagy veszeségmentes</td><td>Támogatott (olvasás 8/16‑bit, írás 8‑bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Veszteségmentes</td><td>Támogatott (olvasás 8/16‑bit, írás 8‑bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Veszteséges vagy veszeségmentes</td><td>Támogatott (olvasás 8/16‑bit, írás 8‑bit)</td></tr>
<tr><td colspan=\"4\"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Veszteségmentes</td><td>Támogatott</td></tr>
<tr><td colspan=\"4\"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Veszteségmentes</td><td>Hamarosan</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Veszteségmentes</td><td>Hamarosan</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Veszteséges vagy veszeségmentes</td><td>Hamarosan</td></tr>
<tr><td colspan=\"4\"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Veszteségmentes</td><td>Hamarosan</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Veszteségmentes</td><td>Hamarosan</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Veszteséges vagy veszeségmentes</td><td>Hamarosan</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Gyakori átkódolási forgatókönyvek">}}

<p>Különböző munkafolyamatokhoz eltérő átkódolási stratégiák szükségesek. Íme a leggyakoribb forgatókönyvek:</p>

<div class="codeblock" id="code">
 <h3>Dekompresszió feldolgozáshoz – C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Kompresszió archiválási tároláshoz – C#</h3>
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
 <h3>Kompresszió hálózati átvitelhez – C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Átviteli szintaxis tulajdonságainak ellenőrzése">}}

<p>A <code>TransferSyntax</code> osztály olyan tulajdonságokat tesz közzé, amelyek leírják a kódolási jellemzőket. Ezeket használhatja a fájl aktuális átviteli szintaxisának ellenőrzésére vagy egy megfelelő cél szintaxis kiválasztására:</p>

<div class="codeblock" id="code">
 <h3>Átviteli szintaxis tulajdonságainak olvasása – C#</h3>
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
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>A transfer szintaxis egyedi azonosítója</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Megmutatja, hogy a Value Representation-ök explicit módon vannak-e kódolva</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Megmutatja, hogy a bájtsorrend little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Megmutatja, hogy a képadat kapszulázott (tömörített)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Megmutatja, hogy a tömörítési módszer veszteséges-e</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Megmutatja, hogy a szintaxis deflate tömörítést használ-e</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Megmutatja, hogy a DICOM szabvány elavultnak nyilvánította-e a szintaxist</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>Az ISO szabvány szerinti azonosítója a veszteséges tömörítési módszernek</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Veszteséges vs. veszteségmentes tömörítés">}}

<p>A veszteséges és veszteségmentes tömörítés közti különbség megértése kritikus a DICOM fájlok átkódolásakor:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Szempont</th>
<th>Veszteségmentes</th>
<th>Veszteséges</th>
</tr>
</thead>
<tbody>
<tr><td>Képminőség</td><td>Pixel-perfekt – az eredeti adatok teljesen megtartva</td><td>Néhány adat végleges elvesztése a kisebb méret érdekében</td></tr>
<tr><td>Tömörítési arány</td><td>Általában 2:1‑től 3:1‑ig</td><td>Általában 10:1‑től 30:1‑ig vagy magasabb</td></tr>
<tr><td>Körkörös biztonság</td><td>Igen – dekompresszió után azonos pixelek</td><td>Nem – minden újra veszteséges kódolás tovább ronthatja a minőséget</td></tr>
<tr><td>Alkalmazási területek</td><td>Archíválás, diagnosztika, jogi feljegyzések</td><td>Előzetes áttekintés, távgyógyászat, hálózati átvitel</td></tr>
<tr><td>Támogatott kodekek</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000</td></tr>
</tbody>
</table>

<p><strong>Fontos:</strong> A veszteségesen tömörített fájl veszteségmentes szintaxisba történő átkódolása nem állítja vissza a elveszett adatokat. A eredeti veszteséges tömörítés miatti minőségromlás végleges.</p>

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

{{< blocks/products/pf/slr-tab tabTitle="Miért az Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
