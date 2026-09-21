---
title: Práce s velkými soubory DICOM v C# .NET | Aspose.Medical
weight: 11500

description: Otevřete vícerámcové studie a obrazy celých snímků v C# bez načítání do paměti. Čtěte metadata bez pixelových dat, odkládejte velké elementy a přenášejte soubory přes streamy a pipes.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Velké soubory DICOM v .NET C#" h2="Přečtěte metadata vícerámcové studie bez pixelů, odložte velké elementy, dokud o ně není požádáno, a přenášejte celé soubory přes streamy a pipe." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Soubor je velký, otázka je obvykle malá">}}

<p>Obraz celého snímku, dlouhá série CT nebo objem OCT má stovky megabajtů a většina je pixelová data. Práce, kterou aplikace ve skutečnosti vykonává, je často mnohem menší: vypsat obsah složky, ověřit identifikátor pacienta, spočítat snímky, rozhodnout, kam má být studie umístěna. Načtení každého bajtu pro odpověď na to je to, co **přemění jednoduchý úkol na problém s pamětí.**</p>

<p><strong>Aspose.Medical pro .NET</strong> umožňuje volajícímu rozhodnout, kolik souboru se načte. Volba je jeden argument u <code>DicomFile.Open</code> a vztahuje se stejně na soubory, streamy i pipe.</p>

<p>Naměřeno na studii o velikosti 14 MB s 128 snímky z našeho testovacího souboru, na stejném počítači a stejném souboru:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Strategie čtení</th>
<th>Čas otevření</th>
<th>Přidělená paměť</th>
</tr>
</thead>
<tbody>
<tr><td>Vše, výchozí</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Velké elementy přeskočeny</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Velké elementy odloženy</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>Rozdíl roste s velikostí souboru. Složka s 10 000 studiemi je případ, kdy již nejde o mikrooptimalizaci.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Přečtěte metadata, nechte pixely nedotčené">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> vynechává každý element nad prahovou velikostí z načítání. Vrácený dataset obsahuje tagy, které potřebuje index nebo router.</p>

<div class="codeblock" id="code">
 <h3>Přečtěte studii bez pixelových dat – C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>Práh je ve výchozím nastavení 64 kB a přijímá hodnotu v kilobajtech, takže pracovní postup, který považuje 8 kB za velké, může tak nastavit.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Odložit místo přeskakování">}}

<p>Když mohou být pixely potřeba, ale pravděpodobně později a pravděpodobně ne všechny, <code>ReadLargeOnDemand</code> je druhou částí páru. Otevření souboru stojí stejně jako přeskakování a velký element se načte v okamžiku, kdy se na něj kód dotkne.</p>

<div class="codeblock" id="code">
 <h3>Načíst snímek jen když je použit – C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>Odložené čtení je licencovaná funkce; ostatní strategie také fungují v evaluační verzi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Indexujte složku bez zásahu do pixelů">}}

<p>Stejná strategie platí pro stream, což je to, jak vypadá skenování archivu nebo úložiště objektů v cloudu z pohledu kódu.</p>

<div class="codeblock" id="code">
 <h3>Skenovat archiv – C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streamy a pipe, dovnitř i ven">}}

<p>Čtení i zápis přijímají streamy a asynchronní vstupní body také přijímají typy <code>System.IO.Pipelines</code>. Studie může přejít od síťové odpovědi k úložišti, aniž by proces kdykoliv držel celý soubor jako jedno pole.</p>

<div class="codeblock" id="code">
 <h3>Číst a zapisovat přes streamy – C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>Stejný princip zahrnuje textové reprezentace: dokument s mnoha datasety se načítá po jednom datasetu na stránkách <a href="/medical/net/json-to-dicom/">JSON do DICOM</a> a <a href="/medical/net/xml-to-dicom/">XML do DICOM</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Snímek po snímku">}}

<p>Data s více snímky jsou adresována po jednotlivých snímcích, takže série s 500 snímky spotřebuje jeden snímek najednou místo celého elementu pixelových dat.</p>

<div class="codeblock" id="code">
 <h3>Procházet snímky – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kde to určuje design">}}

<ul>
<li>Indexování a migrace archivů: milióny souborů, a jen hlavička je důležitá, dokud není něco přesunuto.</li>
<li>Routery a uzly úložiště: přijmou studii, přečtou, co je potřeba k jejímu směrování, a předají bajty dál.</li>
<li>AI pipeline: vytvořte manifest z metadat a poté načtěte snímky pro podmnožinu, na které se skutečně trénuje.</li>
<li>Kontejnery s omezením paměti: pracovní sada následuje strategii, nikoli velikost souboru.</li>
<li>Data celých snímků a OCT: soubory, kde čtení všeho není vůbec možnost.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">Průvodce správou paměti</a> podrobně vysvětluje strategie a <a href="/medical/net/dicom-networking/">DICOM networking</a> ukazuje stejná data přicházející přes DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Výukové zdroje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Průvodce vývojáře" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
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
