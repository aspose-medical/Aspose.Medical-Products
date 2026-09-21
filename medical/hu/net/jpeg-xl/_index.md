---
title: JPEG XL DICOM-hoz C# .NET-ben | Aspose.Medical
weight: 10500

description: Tárolja a DICOM-képeket JPEG XL formátumban C#-ból. Veszteségmentes JPEG XL, amely a pixeleket bit‑ről‑bitre visszaad, egyetlen menedzselt összeállításban, natív kódkönyvtár nélkül.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL DICOM-hoz .NET C#-ben" h2="A legújabb tömörítés a DICOM szabványban, a legkisebb veszteségmentes fájlokkal, amelyeket mértünk, menedzselt C#‑ban megvalósítva és egyetlen összeállításban szállítva." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Miért került a JPEG XL a DICOM-ba">}}

<p>Az orvosi archívumok nőnek és soha nem zsugorodnak. A JPEG XL a képfeldolgozó világ által a JPEG és JPEG 2000 több évtizedes tapasztalata után tervezett kodek, és a DICOM átvitelnyelvként vette fel, mivel a tárolási csapatok egy fontos okra figyelnek: ugyanazoknál a pixeleknél a fájl kisebb.</p>

<p><strong>Aspose.Medical for .NET</strong> ír és olvas JPEG XL‑t egy C#‑os libjxl porton keresztül, amely a könyvtárban él. A csomag egyetlen összeállítást szállít, <code>Aspose.Medical.dll</code>, és nem tartalmaz natív binárist, így ez a új kodek nem válik telepítési projektté: ugyanaz az összeállítás fut Windows‑on, Linux‑on, egy build‑ügynökön és konténerben is.</p>

<p>Két átvitelnyelv hordozza a pixeleket:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), diagnosztikai adatokhoz, amelyeknek változatlanul kell visszatérniük.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), azokhoz az esetekhez, amikor egy kisebb fájl fontosabb, mint a pontos másolat.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kódolja a vizsgálatot, őrizze meg minden pixelt">}}

<p>A transzkódolás egyetlen hívás, és a pixeleket körülvevő adatkészlet vele együtt utazik.</p>

<div class="codeblock" id="code">
 <h3>Transzkódolja a DICOM fájlt JPEG XL‑re – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Egy saját tesztkészletünkben lévő 1714 × 1933 pixeles 16‑bit kép alapján mértük: a 6,3 MB tömörítetlen állomány 2,7 MB JPEG XL veszteségmentes méretűre zsugorodik, ami kisebb, mint a ugyanaz a kép HTJ2K veszteségmentes esetben. Az Ön eredményei a modalitástól függnek, ezért a választás előtt futtassa az összehasonlítást egy mappában lévő fájljain.

<p>A „veszteségmentes” szót itt szó szerint kell érteni. Transzkódolj JPEG XL‑re és vissza, és a pixeladatok megegyeznek a kezdeti bájtokkal, így egy archívum újrakódolható anélkül, hogy a diagnosztikai minőségről vita merülne fel.</p>

<div class="codeblock" id="code">
 <h3>Vissza a tömörítetlen szintaxisra – C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Olvassa el, ami már JPEG XL‑ként van tárolva">}}

<p>Egy JPEG XL‑ben érkező fájl ugyanúgy nyílik meg, mint bármely más. Az átvitelnyelv megadja a típusát, és a pixeladatok a keret dekódolása után elérhetők.</p>

<div class="codeblock" id="code">
 <h3>JPEG XL fájl megnyitása – C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL vagy HTJ2K">}}

<p>Mindkettő új, mindkettő veszteségmentes, ha azt kéri, és a könyvtár mindkettőt írja és olvassa. Különböző kérdésekre adnak választ.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Kérdés</th>
<th>Válasz</th>
</tr>
</thead>
<tbody>
<tr><td>Melyik készítette a kisebb fájlt a tesztünkben</td><td>JPEG XL veszteségmentes, néhány százalékkal</td></tr>
<tr><td>Melyik épült progresszív megjelenítésre hálózaton</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, különösen az RPCL változat</td></tr>
<tr><td>Melyik lépett be először a DICOM szabványba</td><td>HTJ2K, ezért ma több archívum is elfogadja</td></tr>
<tr><td>Melyik igényel natív függőséget itt</td><td>Egyik sem, mindkettő menedzselt kód egyetlen összeállításban</td></tr>
</tbody>
</table>

<p>A választás általában a lánc másik oldaláról származik: transzkóduja arra a szintaxisra, amelyet az archívum elfogad, és a pipeline többi részét változatlanul hagyja.</p>

<div class="codeblock" id="code">
 <h3>Hagyja, hogy a célarchívum döntse el – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ahol megtérül">}}

<ul>
<li>Hosszú távú archívumok: ugyanazok a vizsgálatok, kevesebb terabájt, és nincs veszteség, amit radiológusnak igazolni kellene.</li>
<li>Felhőalapú tárolási számlák: a megtakarítás havonta ismétlődik, míg a transzkódolás egyszer fut.</li>
<li>Adathalmazok kutatáshoz és MI-hez: a kisebb másolatok gyorsabban mozognak a tároló és a tanítás között.</li>
<li>Telepítés: egy ilyen új kodek általában natív buildet igényel platformonként; itt a már hivatkozott összeállítás része.</li>
</ul>

<p>A könyvtár a meglévő archívumokban megtalálható kodekeket is írja: JPEG, JPEG-LS, JPEG 2000, HTJ2K és RLE. A <a href="/medical/net/dicom-transfer-syntax-conversion/">átvitelnyelv konverzió</a> oldal lefedi az egész készletet, a <a href="/medical/net/htj2k/">HTJ2K</a> saját oldallal rendelkezik, és a <a href="/medical/net/jpeg2000/">JPEG 2000</a> az a hely, ahonnan a két új kodek származik.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Fejlesztői útmutató" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="API‑referenciák" href="https://reference.aspose.com/medical/net/" >}}
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
