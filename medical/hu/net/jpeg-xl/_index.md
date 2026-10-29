---
title: JPEG XL a DICOM-hoz C# .NET-ben | Aspose.Medical
weight: 10500

description: Tároljon DICOM képeket JPEG XL formátumban C#-ról. Veszteségmentes JPEG XL, amely a pixeleket bitről bitre visszaadja, egyetlen kezelt összeállításban, natív kodek telepítése nélkül.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL a DICOM-hoz .NET C#-ben" h2="A legújabb tömörítés a DICOM szabványban, a legkisebb mérésünk szerinti veszteségmentes fájlokkal, kezelt C#-ban megvalósítva és egyetlen összeállításban szállítva." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Miért lépett be a JPEG XL a DICOM-ba">}}

<p>A orvosi archívumok nőnek, és soha nem zsugorodnak. A JPEG XL a kódoló, amelyet a képalkotó világ tervezett két évtizedes JPEG és JPEG 2000 tapasztalat után, és a DICOM átvitel szintaxisaként adta hozzá azért, ami miatt a tároló csapatoknak számít: ugyanazoknál a pixeleknél a fájl kisebb.</p>

<p><strong>Aspose.Medical for .NET</strong> JPEG XL-t ír és olvas egy C#-os libjxl porton keresztül, amely a könyvtárban él. A csomag egyetlen összeállítást szállít, <code>Aspose.Medical.dll</code>, és nincs mellette natív bináris, ezért egy ilyen új kodek nem válik telepítési projektté: ugyanaz az összeállítás fut Windows-on, Linux-on, egy build ügynökön és konténerben is.</p>

<p>Két átviteli szintaxis szállítja a pixeleket:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), diagnosztikai adatokhoz, amelyeknek változatlanul vissza kell térniük.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), olyan esetekhez, ahol egy kisebb fájl fontosabb, mint egy pontos másolat.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tanulmány tömörítése, minden pixel megőrzése">}}

<p>A transzkódolás egy hívás, és a pixelek körüli adatkészlet vele együtt utazik.</p>

<div class="codeblock" id="code">
 <h3>DICOM fájl transzkódolása JPEG XL-re - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Ezt egy 1714 × 1933 pixeles, 16 bites képen mértük a saját teszthalmazunkból: a 6,3 MB tömörítetlen 2,7 MB JPEG XL veszteségmentes állományra csökken, amely kisebb, mint ugyanaz a kép HTJ2K veszteségmentes formátumban. A saját számítások a modalitástól függenek, ezért futtassa a összehasonlítást a fájljait tartalmazó mappán, mielőtt döntene.</p>

<p>A "Lossless" szót itt szó szerint kell érteni. Transzkódolja JPEG XL-re és vissza, és a pixeladatok egyenlőek a kiinduló bájtokkal, így egy archívum újratömöríthető anélkül, hogy a diagnosztikai minőségről vitába keveredne.</p>

<div class="codeblock" id="code">
 <h3>Vissza egy tömörítetlen szintaxisra - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Olvassa el, ami már JPEG XL-ként van tárolva">}}

<p>Egy JPEG XL-ben érkező fájl ugyanúgy nyílik meg, mint bármely más. Az átviteli szintaxis megmondja, mi az, és a pixeladatok elérhetők, amint a keret dekódolva van.</p>

<div class="codeblock" id="code">
 <h3>JPEG XL fájl megnyitása - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL vagy HTJ2K">}}

<p>Mindkét formátum újszerű, mindkettő veszteségmentes, ha ezt kérjük, és a könyvtár mindkettőt írja és olvassa. Különböző kérdésekre válaszolnak.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Kérdés</th>
<th>Válasz</th>
</tr>
</thead>
<tbody>
<tr><td>Melyik eredményezte a kisebb fájlt a tesztünkben</td><td>JPEG XL veszteségmentes, néhány százalékkal</td></tr>
<tr><td>Melyik épült progresszív megtekintésre hálózaton keresztül</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, különösen az RPCL változat</td></tr>
<tr><td>Melyik lépett be először a DICOM szabványba</td><td>HTJ2K, így ma több archívum is elfogadja</td></tr>
<tr><td>Melyik igényel natív függőséget itt</td><td>Egyik sem, mindkettő kezelt kód egyetlen összeállításban</td></tr>
</tbody>
</table>

<p>A választás általában a lánc másik oldaláról származik: transzkódolja a szintaxist, amelyet az archívum elfogad, és a folyamat többi részét változatlanul hagyja.</p>

<div class="codeblock" id="code">
 <h3>Hagyja, hogy a célarchívum döntse el - C#</h3>
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
<li>Hosszú távú archívumok: ugyanazok a tanulmányok, kevesebb terabájt, és nincs veszteség, amit radiológusnak indokolni kellene.</li>
<li>Felhőalapú tárolási számlák: a megtakarítás minden hónapban ismétlődik, míg a transzkódolás egyszer fut.</li>
<li>Kutatási és AI adathalmazok: a kisebb másolatok gyorsabban mozognak a tároló és a tréning között.</li>
<li>Telepítés: egy ilyen új kodek általában natív buildet jelent platformonként; itt már a hivatkozott összeállítás része.</li>
</ul>

<p>A könyvtár emellett írja azokat a kodekeket is, amelyekkel egy meglévő archívum tele van: JPEG, JPEG-LS, JPEG 2000, HTJ2K és RLE. A <a href="/medical/net/dicom-transfer-syntax-conversion/">átviteli szintaxis konverzió</a> oldal lefedi az egész készletet, a <a href="/medical/net/htj2k/">HTJ2K</a> saját oldallal rendelkezik, és a <a href="/medical/net/jpeg2000/">JPEG 2000</a> az, ahonnan mindkét új kodek származik.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Fejlesztői útmutató" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetett támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért az Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
