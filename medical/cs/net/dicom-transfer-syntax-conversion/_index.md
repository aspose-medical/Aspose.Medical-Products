---
title: Konverze DICOM přenosových syntaxí v C# .NET | Aspose.Medical
weight: 16000
description: Překódujte DICOM soubory mezi přenosovými syntaxemi v C# .NET. Podpora pro JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG‑LS, RLE a nekomprimované formáty s API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konverze DICOM přenosových syntaxí v .NET C#" h2="Překódujte DICOM soubory mezi nekomprimovanými, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG‑LS a RLE přenosovými syntaxemi. Čistá .NET knihovna bez nativních závislostí." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Co je přenosová syntaxe?">}}

<p><strong>Přenosová syntaxe</strong> určuje, jak jsou data DICOM kódována pro ukládání a přenos. Specifikuje tři klíčové aspekty: pořadí bajtů (endianness), zda jsou Value Representations explicitní nebo implicitní, a kompresní algoritmus aplikovaný na pixelová data. Každý DICOM soubor deklaruje svou přenosovou syntaxi v hlavičce File Meta Information.</p>

<p>Různá medicínská zařízení, PACS servery a zobrazovací aplikace podporují různé sady přenosových syntaxí. <strong>Aspose.Medical for .NET</strong> poskytuje metodu <code>Transcode</code> pro konverzi mezi přenosovými syntaxemi, což umožňuje interoperabilitu, optimalizaci úložiště a kompatibilitu s nástroji pro zpracování &mdash; vše v čisté .NET knihovně bez nativních závislostí.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Překódujte DICOM soubor v C#">}}

<p>Metoda <code>DicomFile.Transcode</code> převádí DICOM soubor z jeho aktuální přenosové syntaxe do libovolné podporované cílové syntaxe. Metoda vrací novou instanci <code>DicomFile</code> &mdash; původní zůstává beze změny:</p>

<div class="codeblock" id="code">
 <h3>Základní DICOM překódování – C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Můžete také přímo překódovat na úrovni <code>Dataset</code>:</p>

<div class="codeblock" id="code">
 <h3>Překódujte Dataset – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Podporované přenosové syntaxe">}}

<p>Následující tabulka uvádí všechny standardní DICOM přenosové syntaxe obrazových dat a jejich aktuální stav podpory v Aspose.Medical pro .NET. Všechny podporované kodeky jsou implementovány v čistém C# a jsou zcela platformně nezávislé.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Přenosová syntaxe</th>
<th>UID</th>
<th>Typ</th>
<th>Stav</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Nekomprimované</strong></td></tr>
<tr><td>Implicitní VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Nekomprimované</td><td>Podporováno</td></tr>
<tr><td>Explicitní VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Nekomprimované</td><td>Podporováno</td></tr>
<tr><td>Explicitní VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Nekomprimované (zrušeno)</td><td>Podporováno</td></tr>
<tr><td>Zapouzdřené nekomprimované Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Nekomprimované</td><td>Není podporováno</td></tr>
<tr><td>Deflovaný Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflovaný</td><td>Podporováno</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Ztrátová, 8-bitová</td><td>Podporováno</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Ztrátová, 12-bitová</td><td>Podporováno</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Bez ztráty</td><td>Podporováno</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Bez ztráty</td><td>Podporováno</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Bez ztráty</td><td>Podporováno</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Téměř bezztrátová</td><td>Podporováno</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Bez ztráty</td><td>Podporováno</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Ztrátová nebo bezztrátová</td><td>Podporováno</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Bez ztráty</td><td>Není podporováno</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Ztrátová nebo bezztrátová</td><td>Není podporováno</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Bez ztráty</td><td>Podporováno</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Bez ztráty</td><td>Podporováno</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Bez ztráty</td><td>Podporováno</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Ztrátová nebo bezztrátová</td><td>Podporováno</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Bez ztráty</td><td>Podporováno</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Bez ztráty</td><td>Pouze dekódování (kódování vyžaduje JPEG vstupní stream, ne pixelová data)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Ztrátová nebo bezztrátová</td><td>Podporováno (ztrátový režim)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Běžné scénáře překódování">}}

<p>Různé pracovní postupy vyžadují různé strategie překódování. Zde jsou nejčastější scénáře:</p>

<div class="codeblock" id="code">
 <h3>Dekomprese pro zpracování – C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Komprese pro archivní úložiště – C#</h3>
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
 <h3>Komprese pro síťový přenos – C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Použijte nejnovější kodeky: HTJ2K a JPEG XL – C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Prozkoumejte vlastnosti přenosové syntaxe">}}

<p>Třída <code>TransferSyntax</code> vystavuje vlastnosti, které popisují charakteristiky kódování. Použijte je ke kontrole aktuální přenosové syntaxe souboru nebo k výběru vhodné cílové syntaxe:</p>

<div class="codeblock" id="code">
 <h3>Čtení vlastností přenosové syntaxe – C#</h3>
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
<th>Vlastnost</th>
<th>Typ</th>
<th>Popis</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>Jedinečný identifikátor přenosové syntaxe</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Zda jsou Value Representations explicitně kódovány</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Zda je pořadí bajtů little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Zda jsou pixelová data zapouzdřena (komprimována)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Zda je metoda komprese ztrátová</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Zda syntaxe používá kompresi deflate</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Zda je přenosová syntaxe stažena ze standardu DICOM</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>Identifikátor ztrátové kompresní metody podle standardu ISO</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ztrátová vs bezztrátová komprese">}}

<p>Pochopení rozdílu mezi ztrátovou a bezztrátovou kompresí je při překódování DICOM souborů zásadní:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Aspekt</th>
<th>Bez ztráty</th>
<th>Ztrátová</th>
</tr>
</thead>
<tbody>
<tr><td>Kvalita obrazu</td><td>Pixel-perfect &mdash; originální data jsou zcela zachována</td><td>Některá data jsou trvale ztracena pro dosažení menší velikosti</td></tr>
<tr><td>Komprimační poměr</td><td>Obvykle 2:1 až 3:1</td><td>Obvykle 10:1 až 30:1 nebo více</td></tr>
<tr><td>Bezpečný při zpětném převodu</td><td>Ano &mdash; dekomprese vrátí identické pixely</td><td>Ne &mdash; každé ztrátové překódování dále snižuje kvalitu</td></tr>
<tr><td>Využití</td><td>Archivace, diagnostika, právní záznamy</td><td>Předběžná revize, telemedicína, síťový přenos</td></tr>
<tr><td>Podporované kodeky</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG Extended, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>Důležité:</strong> Překódování ze souboru s‑ztrátovou kompresí do bezztrátové syntaxe neobnoví ztracená data. Zhoršení kvality původní ztrátové komprese je trvalé.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Výukové zdroje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Zdrojový kód" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referenční příručky API" href="https://reference.aspose.com/medical/net/" >}}
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
