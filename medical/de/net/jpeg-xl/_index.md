---
title: JPEG XL für DICOM in C# .NET | Aspose.Medical
weight: 10500

description: Speichern Sie DICOM-Bilder in JPEG XL aus C#. Verlustfreies JPEG XL, das die Pixel Bit für Bit zurückgibt, in einer einzigen verwalteten Assembly ohne native Codec-Installation.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL für DICOM in .NET C#" h2="Die neueste Kompression im DICOM-Standard, mit den kleinsten von uns gemessenen verlustfreien Dateien, implementiert in verwaltetem C# und in einer einzigen Assembly ausgeliefert." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Warum JPEG XL DICOM erreicht hat">}}

<p>Medizinische Archive wachsen und schrumpfen nie. JPEG XL ist der Codec, den die Bildwelt nach zwei Jahrzehnten Erfahrung mit JPEG und JPEG 2000 entwickelte, und DICOM hat ihn als Transfersyntax aus dem Grund hinzugefügt, der Speicher‑Teams wichtig ist: Für dieselben Pixel ist die Datei kleiner.</p>

<p><strong>Aspose.Medical für .NET</strong> schreibt und liest JPEG XL über einen C#‑Port von libjxl, der in der Bibliothek enthalten ist. Das Paket liefert eine Assembly, <code>Aspose.Medical.dll</code>, und keine native Binärdatei daneben, sodass ein Codec dieser Art nicht zu einem Deploy‑Projekt wird: dieselbe Assembly läuft unter Windows, Linux, auf einem Build‑Agent und in einem Container.</p>

<p>Zwei Transfersyntaxen transportieren die Pixel:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), für diagnostische Daten, die unverändert zurückkehren müssen.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), für Fälle, in denen eine kleinere Datei wichtiger ist als eine exakte Kopie.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Komprimieren Sie eine Studie, behalten Sie jedes Pixel">}}

<p>Transcoding erfolgt in einem Aufruf, und der Datensatz rund um die Pixel wird dabei mitübertragen.</p>

<div class="codeblock" id="code">
 <h3>DICOM-Datei nach JPEG XL transkodieren – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Wir haben es an einem 1714 × 1933‑Pixel‑Bild mit 16 Bit aus unserem eigenen Test‑Set gemessen: 6,3 MB unkomprimiert werden zu 2,7 MB in JPEG XL lossless, was kleiner ist als dasselbe Bild in HTJ2K lossless. Ihre eigenen Werte hängen vom Modalitätstyp ab, daher sollten Sie den Vergleich über einen Ordner Ihrer Dateien durchführen, bevor Sie entscheiden.</p>

<p>„Lossless“ ist hier wörtlich zu verstehen. Transkodieren Sie zu JPEG XL und zurück, und die Pixeldaten entsprechen den Bytes, mit denen Sie begonnen haben, sodass ein Archiv ohne Diskussion über die diagnostische Qualität erneut komprimiert werden kann.</p>

<div class="codeblock" id="code">
 <h3>Zurück zu einer unkomprimierten Syntax – C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lesen Sie, was bereits als JPEG XL gespeichert ist">}}

<p>Eine Datei, die in JPEG XL vorliegt, lässt sich wie jede andere öffnen. Die Transfersyntax gibt an, was es ist, und die Pixeldaten stehen zur Verfügung, sobald das Bild decodiert ist.</p>

<div class="codeblock" id="code">
 <h3>Eine JPEG XL‑Datei öffnen – C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL oder HTJ2K">}}

<p>Beide sind neu, beide sind lossless, wenn Sie lossless anfordern, und die Bibliothek schreibt und liest beide. Sie beantworten unterschiedliche Fragen.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Frage</th>
<th>Antwort</th>
</tr>
</thead>
<tbody>
<tr><td>Welcher erzeugte die kleinere Datei in unserem Test</td><td>JPEG XL lossless, um einige Prozent</td></tr>
<tr><td>Welcher ist für progressives Anzeigen über ein Netzwerk ausgelegt</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, insbesondere die RPCL‑Variante</td></tr>
<tr><td>Welcher wurde zuerst in den DICOM‑Standard aufgenommen</td><td>HTJ2K, sodass heute mehr Archive ihn akzeptieren</td></tr>
<tr><td>Welcher verursacht hier eine native Abhängigkeit</td><td>Keiner, beide sind Managed‑Code in einer Assembly</td></tr>
</tbody>
</table>

<p>Die Wahl kommt in der Regel von der anderen Seite der Kette: Transkodieren Sie in die Syntax, die das Archiv akzeptiert, und belassen Sie den Rest der Pipeline unverändert.</p>

<div class="codeblock" id="code">
 <h3>Das Zielarchiv entscheiden lassen – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Wo es sich auszahlt">}}

<ul>
<li>Langzeitarchive: dieselben Studien, weniger Terabyte, und kein Qualitätsverlust, den man einem Radiologen rechtfertigen muss.</li>
<li>Cloud‑Speicherrechnungen: die Einsparungen wiederholen sich monatlich, während das Transcoding nur einmal läuft.</li>
<li>Daten­sätze für Forschung und KI: kleinere Kopien bewegen sich schneller zwischen Speicher und Training.</li>
<li>Deployment: Ein Codec dieser Art bedeutet normalerweise ein natives Build pro Plattform; hier ist er Teil der Assembly, die Sie bereits referenzieren.</li>
</ul>

<p>Die Bibliothek schreibt auch die Codecs, die ein bestehendes Archiv enthält: JPEG, JPEG‑LS, JPEG 2000, HTJ2K und RLE. Die Seite <a href="/medical/net/dicom-transfer-syntax-conversion/">Transfer Syntax Conversion</a> deckt das gesamte Set ab, <a href="/medical/net/htj2k/">HTJ2K</a> hat eine eigene Seite, und <a href="/medical/net/jpeg2000/">JPEG 2000</a> ist die Quelle beider neuer Codecs.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Entwicklerhandbuch" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="API-Referenzen" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktsupport" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Kostenloser Support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Kostenpflichtiger Support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Warum Aspose.Medical für .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundenliste" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Erfolgsgeschichten" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
