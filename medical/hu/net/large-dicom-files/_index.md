---
title: Munka nagy DICOM fájlokkal C# .NET | Aspose.Medical
weight: 11500

description: Nyissa meg a többkeretes vizsgálatokat és a teljes csúszókép fájlokat C#-ban anélkül, hogy memóriába töltené őket. Olvassa a metaadatokat pixeladatok nélkül, halassza el a nagy elemek beolvasását, és mozgassa a fájlokat adatfolyamok és csövek között.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Nagy DICOM fájlok .NET C#-ban" h2="Olvassa el egy többkeretes vizsgálat metaadatait pixeladatok nélkül, halassza el a nagy elemek beolvasását, amíg valami nem kéri őket, és mozgassa a teljes fájlokat adatfolyamok és csövek segítségével." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Az állomány nagy, a kérdés általában kicsi.">}}

<p>Egy teljes csúszókép, egy hosszú CT sorozat vagy egy OCT kötet több száz megabájtot tesz ki, és ennek nagy része pixeladat. Az alkalmazás által ténylegesen végzett munka gyakran sokkal kisebb: listázza, mi van egy mappában, ellenőrzi a betegazonosítót, megszámolja a kereteket, eldönti, hová kerüljön a vizsgálat. Minden bájt betöltése a válaszadáshoz teszi az egyszerű feladatot memória problémává.</p>

<p><strong>Aspose.Medical for .NET</strong> lehetővé teszi a hívónak, hogy meghatározza, a fájl mekkora részét olvassa be. A választás egy argumentum a <code>DicomFile.Open</code> metódusban, és fájlokra, adatfolyamokra és csövekre egyaránt vonatkozik.</p>

<p>14 MB-os vizsgálaton, 128 kerettel, a tesztkészletünkből, ugyanazon a gépen és ugyanazon a fájlon mérve:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Olvasási stratégia</th>
<th>Megnyitási idő</th>
<th>Kijelölt memória</th>
</tr>
</thead>
<tbody>
<tr><td>Mindent, az alapértelmezett</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Nagy elemek kihagyva</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Nagy elemek elhalasztva</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>A különbség a fájl méretével nő. Egy 10 000 vizsgálatot tartalmazó mappa már nem mikro-optimalizációnál van.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Olvassa a metaadatokat, hagyja a pixeleket érintetlenül">}}

<p>A <code>TagDataReadingStrategies.SkipLargeTags</code> kihagy minden a méretküszöbön felüli elemet a beolvasásból. A visszakapott adathalmaz tartalmazza azokat a tageket, amelyekre egy indexnek vagy egy routernek szüksége van.</p>

<div class="codeblock" id="code">
 <h3>Vizsgálat olvasása pixeladatai nélkül - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>A küszöb alapértelmezés szerint 64 kB, és kilobájtban adható meg, így egy olyan munkafolyamat, amely 8 kB-t nagyként kezeli, ezt kifejezheti.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Elhalasztás a kihagyás helyett">}}

<p>Amikor a pixelekre szükség lehet, de valószínűleg később és nem mindegyikre, a <code>ReadLargeOnDemand</code> a pár másik fele. A fájl megnyitása ugyanolyan költségű, mint a kihagyás, és egy nagy elem a kód által érintéskor kerül beolvasásra.</p>

<div class="codeblock" id="code">
 <h3>Keret betöltése csak használatkor - C#</h3>
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

<p>Az elhalasztott olvasás licencelt funkció; a többi stratégia értékelés alatt is működik.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Mappa indexelése a pixelek érintése nélkül">}}

<p>Ugyanez a stratégia alkalmazható egy adatfolyamra, amely egy archívumvizsgálat vagy felhő tárhely esetén a kódban így néz ki.</p>

<div class="codeblock" id="code">
 <h3>Archívum beolvasása - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Adatfolyamok és csövek, be és ki">}}

<p>Az olvasás és írás is elfogadja az adatfolyamokat, és az aszinkron belépési pontok szintén elfogadják a <code>System.IO.Pipelines</code> típusokat. Egy vizsgálat átjuthat a hálózati válaszból a tárolóba anélkül, hogy a folyamat egyetlen tömbként a teljes fájlt megtartaná.</p>

<div class="codeblock" id="code">
 <h3>Olvasás és írás adatfolyamokon keresztül - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>Ez az elképzelés kiterjed a szöveges reprezentációkra is: egy sok adathalmazt tartalmazó dokumentumot egy adathalmazonként olvassák a <a href="/medical/net/json-to-dicom/">JSON to DICOM</a> és a <a href="/medical/net/xml-to-dicom/">XML to DICOM</a> oldalakon.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Keretről keretre">}}

<p>A többkeretes adat keretről keretre kerül feldolgozásra, ezért egy 500 keretes sorozat egy keretet dolgoz fel egyszerre, a teljes pixeladat elem helyett.</p>

<div class="codeblock" id="code">
 <h3>Keretsétálás - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Ahol ez befolyásolja a tervezést">}}

<ul>
<li>Archívum indexelése és migrációja: milliók fájlja, és csak a fejléc számít, amíg valami nem kerül áthelyezésre.</li>
<li>Routerek és tárolócsomópontok: elfogadnak egy vizsgálatot, beolvassák a szükséges adatokat az útvonalhoz, továbbadják a bájtokat.</li>
<li>AI csővezetékek: metaadatokból építik fel a manifest-et, majd lekérik a kereteket a ténylegesen tanulásra használt részhalmazhoz.</li>
<li>Memóriakorlátos konténerek: a munkakészlet a stratégiát követi, nem a fájlméretet.</li>
<li>Teljes csúszókép és OCT adatok: olyan fájlok, ahol a teljes beolvasás egyáltalán nem opció.</li>
</ul>

<p>A <a href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/">memóriakezelési útmutató</a> részletesen ismerteti a stratégiákat, és a <a href="/medical/net/dicom-networking/">DICOM hálózat</a> bemutatja, hogy ugyanazok az adatok hogyan érkeznek DIMSE-n keresztül.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási erőforrások" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Fejlesztői útmutató" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetett támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért az Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyféllista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
