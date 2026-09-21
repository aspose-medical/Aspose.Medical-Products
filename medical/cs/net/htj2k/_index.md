---
title: HTJ2K v C# .NET - High-Throughput JPEG 2000 pro DICOM | Aspose.Medical
weight: 10000

description: Komprimujte a čtěte DICOM obrazy v High-Throughput JPEG 2000 z C#. Bezztrátové HTJ2K, varianta RPCL a ztrátové HTJ2K, implementované v řízeném .NET bez nutnosti nasazení nativního kodeku.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K v .NET C#" h2="High-Throughput JPEG 2000 pro DICOM: komprese, kterou standard přidal pro rychlé archivy a cloudové prohlížení, implementovaná v řízeném C# bez nutnosti instalace nativního komponentu." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Co HTJ2K mění">}}

<p>High-Throughput JPEG 2000 zachovává vlnkovou transformaci a kvalitu obrazu JPEG 2000 a nahrazuje část, která zpomalovala. Blokový kodér je nový a dekódování je řádově rychlejší, což je důvod, proč standard DICOM přijal tuto technologii ve třech syntaxech přenosu a proč cloudové zobrazovací platformy přešly na ni.</p>

<p>Pro .NET tým je praktická otázka jiná: kdo dokáže skutečně tyto soubory vytvořit. Většina knihoven získává HTJ2K přes nativní sestavení OpenJPH, což znamená binárku pro každou platformu, krok sestavení v kontejneru a závislost, o které se bezpečnostní revize ptá. <strong>Aspose.Medical for .NET</strong> implementuje kodek v řízeném kódu uvnitř stejného balíčku, který soubory čte a zapisuje, takže HTJ2K funguje stejně na Windows, na Linuxu i v kontejneru, bez nutnosti instalace.</p>

<p>Jsou podporovány tři syntaxy přenosu a všechny tři lze jak číst, tak zapisovat:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), High-Throughput JPEG 2000 bezztrátově.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), bezztrátová varianta s RPCL pořadím průběhu.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Komprimujte studii do HTJ2K">}}

<p>Jedním voláním se soubor převede do nové syntaxe. Dataset, soukromé tagy i meta informace souboru jsou s ním přeneseny.</p>

<div class="codeblock" id="code">
 <h3>Překódovat DICOM soubor do HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>U 16‑bitového obrazu 1714 × 1933 z našeho testovacího souboru se velikost souboru sníží ze 6,3 MB na 2,9 MB a pixely jsou vráceny bit po bitu. Čísla se liší podle modality a obrazu, takže měřte na vlastních datech, což je jeden průběh přes soubory, které již máte.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bezztrátové znamená bezztrátové">}}

<p>Diagnostická data neumožňují kodek, který je jen téměř správný. Překódujte do HTJ2K bezztrátově a zpět, a pixelová data jsou identická s bajty, se kterými jste začínali, což je vlastnost, kterou můžete ověřit ve svém testovacím balíčku, než souhlasíte s recompresí archivu.</p>

<div class="codeblock" id="code">
 <h3>Zpět k nekomprimované syntaxi - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, varianta určená pro prohlížení přes síť">}}

<p>Syntax 1.2.840.10008.1.2.4.202 ukládá stejný bezztrátový kódovací proud v RPCL pořadí průběhu: nejprve rozlišení, poté pozice, poté komponenta, poté vrstva. Čteč, který načte jen začátek proudu, získá kompletní nízké rozlišení obrazu, což je to, co prohlížeč potřebuje při otevření velké studie přes odkaz, který neřídí.</p>

<div class="codeblock" id="code">
 <h3>Komprimovat s RPCL pořadím průběhu - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Čtěte, co vám archiv posílá">}}

<p>Druhá polovina úkolu spočívá v přijímání HTJ2K od systémů, které jej již vytvářejí. Otevřete soubor, zkontrolujte, jak je uložen, a pracujte s pixelovými daty.</p>

<div class="codeblock" id="code">
 <h3>Číst HTJ2K soubor - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Víceframcové obrazy jsou zpracovávány rám po rámci, takže dlouhá série spotřebuje paměť na rám místo na studii.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kde HTJ2K získává své místo">}}

<ul>
<li>Migrace archivů: recomprimujte uloženou studii do HTJ2K bezztrátově, snížíte nároky na úložiště a zachováte diagnostická data nedotčena.</li>
<li>Cloud a DICOMweb: rychlost dekódování je to, co dělá prohlížeč na straně klienta nebo serveru pocit okamžitého zobrazení velkých obrazů.</li>
<li>AI pipeline: tréninkové sady jsou čteny mnohem častěji než zapisovány a čas dekódování je opakující se náklad.</li>
<li>Kontejnery a serverless: kodek je součástí sestavení, takže obraz nevyžaduje nativní knihovnu ani kompilátor při sestavování.</li>
</ul>

<p>Knihovna také obsahuje JPEG XL, další nedávnou novinku ve standardu, a starší kodeky, které archiv pravděpodobně obsahuje: JPEG, JPEG‑LS, JPEG 2000 a RLE. Stránka <a href="/medical/net/dicom-transfer-syntax-conversion/">převod syntaxi přenosu</a> pokrývá celý soubor a stránka <a href="/medical/net/jpeg2000/">JPEG 2000</a> pokrývá kodek, ze kterého HTJ2K vyrostl.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Výukové zdroje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Průvodce vývojáře" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
