---
title: JPEG XL pro DICOM v C# .NET | Aspose.Medical
weight: 10500

description: Ukládejte DICOM obrázky v JPEG XL z C#. Bezeztrátový JPEG XL, který vrací pixely bit po bitu, v jedné spravované sestavě bez nativního kodeku k nasazení.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL pro DICOM v .NET C#" h2="C nejnovější komprese v standardu DICOM, s nejmenšími bezeztrátovými soubory, jaké jsme měřili, implementována ve spravovaném C# a dodávána v jediné sestavě." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Proč JPEG XL dorazil do DICOM">}}

<p>Medicínské archivy rostou a nikdy neskracují. JPEG XL je kodek, který svět zobrazování navrhl po dvou desetiletích zkušeností s JPEG a JPEG 2000, a DICOM jej přidal jako přenosovou syntaxi z důvodu, který zajímá týmy pro úložiště: při stejných pixelech je soubor menší.</p>

<p><strong>Aspose.Medical pro .NET</strong> zapisuje a čte JPEG XL prostřednictvím C# portu libjxl, který žije uvnitř knihovny. Balíček dodává jednu sestavu, <code>Aspose.Medical.dll</code>, a žádný nativní binární soubor vedle ní, takže takový nový kodek se nepřevádí na projekt nasazení: stejná sestava běží na Windows, na Linuxu, na sestavovacím agentovi i v kontejneru.</p>

<p>Dvě přenosové syntaxi nesou pixely:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), pro diagnostická data, která musí zůstat nezměněna.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), pro případy, kdy je menší soubor důležitější než přesná kopie.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Komprimujte studii, zachovejte každý pixel">}}

<p>Transkódování je jedna výzva a datová sada kolem pixelů s ním cestuje.</p>

<div class="codeblock" id="code">
 <h3>Překódovat DICOM soubor do JPEG XL – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Naměřili jsme to na 16‑bitovém obrázku o rozměrech 1714 × 1933 z našeho testovacího souboru: nekomprimovaných 6,3 MB se stane 2,7 MB v bezeztrátovém JPEG XL, což je méně než stejný obrázek v bezeztrátovém HTJ2K. Vaše vlastní výsledky závisí na modalitě, proto proveďte srovnání na složce vašich souborů před výběrem.</p>

<p>Bezeztrátový je zde slovo, které je třeba brát doslovně. Překódujte do JPEG XL a zpět a data pixelů se rovnají bajtům, se kterými jste začínali, takže archiv lze recomprimovat bez debaty o diagnostické kvalitě.</p>

<div class="codeblock" id="code">
 <h3>Zpět k nekomprimované syntaxi – C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Čtěte, co je již uloženo jako JPEG XL">}}

<p>Soubor, který přijde v JPEG XL, se otevírá jako každý jiný. Přenosová syntaxe říká, co to je, a data pixelů jsou k dispozici po dekódování snímku.</p>

<div class="codeblock" id="code">
 <h3>Otevřít JPEG XL soubor – C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL nebo HTJ2K">}}

<p>Oba jsou noví, oba jsou bezeztrátové, pokud požadujete bezeztrátový režim, a knihovna je zapisuje i čte. Odpovídají na odlišné otázky.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Otázka</th>
<th>Odpověď</th>
</tr>
</thead>
<tbody>
<tr><td>Který vytvořil menší soubor v našem testu</td><td>JPEG XL bezeztrátově, o několik procent</td></tr>
<tr><td>Který je určen pro progresivní prohlížení přes síť</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, zejména varianta RPCL</td></tr>
<tr><td>Který vstoupil do standardu DICOM jako první</td><td>HTJ2K, takže ho dnes akceptuje více archivů</td></tr>
<tr><td>Který zde vyžaduje nativní závislost</td><td>Žádný, oba jsou spravovaný kód v jedné sestavě</td></tr>
</tbody>
</table>

<p>Volba obvykle vychází z druhé strany odkazu: překódujte do syntaxi, kterou archiv akceptuje, a zbytek pipeline ponechte beze změny.</p>

<div class="codeblock" id="code">
 <h3>Nechte cílový archiv rozhodnout – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kde to má smysl">}}

<ul>
<li>Dlouhodobé archivy: stejné studie, méně terabajtů a žádná ztráta, kterou by bylo třeba ospravedlňovat radiologovi.</li>
<li>Účty za cloudové úložiště: úspora se opakuje každý měsíc, zatímco transkódování proběhne jednou.</li>
<li>Datové sady pro výzkum a AI: menší kopie se přesouvají rychleji mezi úložištěm a trénováním.</li>
<li>Nasazení: nový kodek obvykle znamená nativní sestavení pro každou platformu; zde je součástí sestavy, kterou již odkazujete.</li>
</ul>

<p>Knihovna také zapisuje kodeky, kterými je existující archiv plný: JPEG, JPEG‑LS, JPEG 2000, HTJ2K a RLE. Stránka <a href="/medical/net/dicom-transfer-syntax-conversion/">konverze přenosových syntaxi</a> pokrývá celý soubor, <a href="/medical/net/htj2k/">HTJ2K</a> má vlastní stránku a <a href="/medical/net/jpeg2000/">JPEG 2000</a> je místo, odkud oba nové kodeky pocházejí.</p>

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
