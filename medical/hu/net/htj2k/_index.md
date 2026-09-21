---
title: HTJ2K C# .NET-ben - Nagyteljesítményű JPEG 2000 DICOM-hoz | Aspose.Medical
weight: 10000

description: Tömörítsen és olvasson DICOM képeket Nagyteljesítményű JPEG 2000 formátumban C#-ból. Veszteségmentes HTJ2K, az RPCL változat és a veszteséges HTJ2K, .NET managed környezetben megvalósítva, natív kodek telepítése nélkül.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K .NET C#-ban" h2="Nagyteljesítményű JPEG 2000 DICOM-hoz: a szabvány által hozzáadott tömörítés gyors archívumokhoz és felhőalapú megtekintéshez, managed C#-ban megvalósítva, natív telepítést igényel nélkül." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Mit változtat a HTJ2K">}}

<p>A Nagyteljesítményű JPEG 2000 megőrzi a JPEG 2000 hullámtranszformációját és képminőségét, és lecseréli a lassú részt. Az új blokkkódolóval a dekódolás egy nagyságrenddel gyorsabb, ezért a DICOM szabvány három átvitel szintaxisban is felvette, és a felhő alapú képalkotó platformok is átváltottak rá.</p>

<p>Egy .NET csapat számára a gyakorlati kérdés más: ki képes valójában előállítani ezeket a fájlokat. A legtöbb könyvtár az HTJ2K-t egy natív OpenJPH builden keresztül érheti el, ami platformonként egy bináris fájlt, egy build lépést a konténerben, és egy függőséget jelent, amelyet a biztonsági felülvizsgálat megkérdez. <strong>Aspose.Medical for .NET</strong> a kodeket a managed kódban valósítja meg ugyanabban a csomagban, amely olvassa és írja a fájlokat, így az HTJ2K ugyanúgy működik Windowson, Linuxon és konténerben, telepítés nélkül.</p>

<p>Három átvitel szintaxis támogatott, és mindhárom olvasásra és írásra is képes:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), Nagyteljesítményű JPEG 2000 veszteségmentes.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), a veszteségmentes változat RPCL előrehaladási sorrenddel.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), Nagyteljesítményű JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tanulmány tömörítése HTJ2K formátumba">}}

<p>Egy hívás áthelyezi a fájlt az új szintaxisba. A dataset, a privát tagek és a fájl metaadatok vele együtt kerülnek át.</p>

<div class="codeblock" id="code">
 <h3>DICOM fájl átkódolása HTJ2K-re – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>A saját tesztsorozatunkból származó 1714 × 1933 16‑bit kép esetén a fájl 6,3 MB‑ról 2,9 MB‑ra csökken, és a pixelek bit‑ről‑bitre visszatérnek. Az értékek modalitásonként és képenként változnak, ezért saját adataival mérje, ami egy kör a már meglévő fájlokon.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="A veszteségmentes azt jelenti, hogy veszteségmentes">}}

<p>A diagnosztikai adatok nem tolerálják a csak közelítően helyes kodeket. Kódolja át HTJ2K veszteségmentesre, majd vissza, és a pixeladatok azonosak lesznek a kiindulási bájtokkal, ami egy olyan tulajdonság, amelyet saját tesztesetben ellenőrizhet, mielőtt egy archívumot újratömörítene.</p>

<div class="codeblock" id="code">
 <h3>Vissza egy tömörítetlen szintaxisra – C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, a hálózaton keresztüli megtekintésre tervezett változat">}}

<p>A 1.2.840.10008.1.2.4.202 szintaxis ugyanazt a veszteségmentes kódfolyamot RPCL előrehaladási sorrendben tárolja: először a felbontás, aztán a pozíció, majd a komponens, végül a réteg. Egy olvasó, amely csak a folyam elejét veszi, teljes alacsony felbontású képet kap, ami azt a nézőnek kell, amikor egy nagy tanulmányt nyit meg egy olyan linken, amelyet nem irányít.</p>

<div class="codeblock" id="code">
 <h3>Tömörítés RPCL előrehaladási sorrenddel – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Olvassa el, amit egy archívum küld">}}

<p>A feladat másik felét az jelenti, hogy HTJ2K-t fogadjon azoktól a rendszerektől, amelyek már előállítják. Nyissa meg a fájlt, ellenőrizze, milyen szintaxisban van tárolva, és dolgozzon a pixeladatokkal.</p>

<div class="codeblock" id="code">
 <h3>HTJ2K fájl olvasása – C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>A többkeretes képek keretről‑keretre kerülnek feldolgozásra, így egy hosszú sorozat memóriahasználata keretenként, nem tanulmányonként alakul.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Hol nyeri el a HTJ2K a helyét">}}

<ul>
<li>Archívum migráció: egy tárolt tanulmány újratömörítése HTJ2K veszteségmentes formátumba, a lábnyom csökkentése, a diagnosztikai adatok érintetlenül tartása.</li>
<li>Felhő és DICOMweb: a dekódolási sebesség teszi a böngésző‑ vagy szerver‑oldali nézőt azonnal reagálássá nagy képeknél.</li>
<li>AI munkafolyamatok: a tanító készleteket sokkal gyakrabban olvassák, mint írják, és a dekódolási idő a visszatérő költség.</li>
<li>Konténerek és serverless: a kodek az assembly része, így a képnél nincs szükség natív könyvtárra vagy fordítóra a build során.</li>
</ul>

<p>A könyvtár emellett tartalmazza a JPEG XL-t is, a szabvány legújabb kiegészítőjét, valamint a régebbi kodekeket, amelyeket egy archívum valószínűleg tartalmaz: JPEG, JPEG‑LS, JPEG 2000 és RLE. A <a href="/medical/net/dicom-transfer-syntax-conversion/">átviteli szintaxis konverzió</a> oldal lefedi a teljes halmazt, és a <a href="/medical/net/jpeg2000/">JPEG 2000</a> oldal bemutatja a HTJ2K‑ból származó kodeket.</p>

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
{{< blocks/products/pf/slr-element name="Fizetős támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért Aspose.Medical .NET-hez?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikertörténetek" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
