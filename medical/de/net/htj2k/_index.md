---
title: HTJ2K in C# .NET - High-Throughput JPEG 2000 für DICOM | Aspose.Medical
weight: 10000

description: Komprimieren und lesen Sie DICOM-Bilder in High-Throughput JPEG 2000 aus C#. Verlustfreies HTJ2K, die RPCL‑Variante und verlustbehaftetes HTJ2K, implementiert in verwaltetem .NET ohne native Codec-Installation.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K in .NET C#" h2="High-Throughput JPEG 2000 für DICOM: die Kompression, die der Standard für schnelle Archive und Cloud‑Anzeige hinzugefügt hat, implementiert in verwaltetem C# ohne native Komponenten zur Installation." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Was HTJ2K ändert">}}

<p>High-Throughput JPEG 2000 behält die Wavelet‑ und Bildqualität von JPEG 2000 bei und ersetzt den Teil, der es langsam machte. Der Block‑Coder ist neu, und das Dekodieren ist um eine Größenordnung schneller, weshalb der DICOM‑Standard es in drei Transfer‑Syntaxen übernommen hat und warum Cloud‑Imaging‑Plattformen darauf umgestiegen sind.</p>

<p>Für ein .NET‑Team ist die praktische Frage anders: Wer kann diese Dateien tatsächlich erzeugen? Die meisten Bibliotheken erreichen HTJ2K über einen nativen OpenJPH‑Build, was ein Binary pro Plattform, einen Build‑Schritt im Container und eine Abhängigkeit bedeutet, die im Security‑Review hinterfragt wird. <strong>Aspose.Medical for .NET</strong> implementiert den Codec in verwaltetem Code innerhalb desselben Pakets, das die Dateien liest und schreibt, sodass HTJ2K sowohl unter Windows, Linux als auch in einem Container gleich funktioniert, ohne dass etwas installiert werden muss.</p>

<p>Drei Transfer‑Syntaxen werden unterstützt, und alle drei können sowohl lesen als auch schreiben:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), High-Throughput JPEG 2000 verlustfrei.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), die verlustfreie Variante mit dem RPCL‑Progressionsorder.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Komprimiere eine Studie zu HTJ2K">}}

<p>Ein Aufruf wandelt eine Datei in die neue Syntax um. Der Datensatz, die privaten Tags und die Datei‑Meta‑Informationen bleiben erhalten.</p>

<div class="codeblock" id="code">
 <h3>Transkodieren einer DICOM‑Datei zu HTJ2K – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>Bei einem 1714 × 1933‑Pixel‑Bild mit 16 Bit aus unserem eigenen Test‑Set reduziert sich die Dateigröße von 6,3 MB auf 2,9 MB, und die Pixel kommen bitweise exakt zurück. Die Werte variieren je nach Modalität und Bild, daher sollten Sie sie mit Ihren eigenen Daten messen, also einer Schleife über die bereits vorhandenen Dateien.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Verlustfrei bedeutet verlustfrei">}}

<p>Diagnostische Daten tolerieren keinen Codec, der nur annähernd korrekt ist. Transkodieren Sie zu HTJ2K lossless und zurück, und die Pixeldaten sind identisch zu den Ausgangsbytes – eine Eigenschaft, die Sie in Ihrer eigenen Testsuite prüfen können, bevor Sie einer erneuten Kompression eines Archivs zustimmen.</p>

<div class="codeblock" id="code">
 <h3>Zurück zu einer unkomprimierten Syntax – C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, die für die Anzeige über ein Netzwerk konzipierte Variante">}}

<p>Die Syntax 1.2.840.10008.1.2.4.202 speichert denselben verlustfreien Codestream in der RPCL‑Progressionsreihenfolge: zuerst Auflösung, dann Position, dann Komponente, dann Ebene. Ein Reader, der nur den Anfang des Streams liest, erhält ein vollständiges Bild mit niedriger Auflösung, was ein Viewer benötigt, wenn er eine große Studie über einen nicht kontrollierten Link öffnet.</p>

<div class="codeblock" id="code">
 <h3>Komprimieren mit der RPCL‑Progressionsreihenfolge – C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lesen Sie, was ein Archiv sendet">}}

<p>Der andere Teil der Aufgabe besteht darin, HTJ2K von Systemen zu akzeptieren, die es bereits erzeugen. Öffnen Sie die Datei, prüfen Sie, in welchem Format sie gespeichert ist, und arbeiten Sie mit den Pixeldaten.</p>

<div class="codeblock" id="code">
 <h3>Lesen einer HTJ2K‑Datei – C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Mehrfachbild‑Bilder werden Bild für Bild verarbeitet, sodass eine lange Serie pro Frame und nicht pro Studie Speicher verbraucht.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Wo HTJ2K sein Anwendungsfeld findet">}}

<ul>
<li>Archivmigration: Eine gespeicherte Studie verlustfrei zu HTJ2K recomprimieren, den Speicherbedarf reduzieren und die diagnostischen Daten intakt halten.</li>
<li>Cloud und DICOMweb: Die Dekodiergeschwindigkeit sorgt dafür, dass ein Browser‑ oder Server‑Viewer bei großen Bildern sofort reagiert.</li>
<li>KI‑Pipelines: Trainingsdatensätze werden viel häufiger gelesen als geschrieben, und die Dekodierzeit ist der wiederkehrende Kostenfaktor.</li>
<li>Container und Serverless: Der Codec ist Teil des Assemblies, sodass ein Image keine native Bibliothek oder einen Compiler im Build benötigt.</li>
</ul>

<p>Die Bibliothek liefert außerdem JPEG XL, die weitere aktuelle Ergänzung des Standards, sowie die älteren Codecs, die ein Archiv typischerweise enthält: JPEG, JPEG‑LS, JPEG 2000 und RLE. Die Seite <a href="/medical/net/dicom-transfer-syntax-conversion/">Transfer‑Syntax‑Konvertierung</a> behandelt das gesamte Set, und die Seite <a href="/medical/net/jpeg2000/">JPEG 2000</a> beschreibt den Codec, aus dem HTJ2K hervorgegangen ist.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Entwicklerhandbuch" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="API‑Referenzen" href="https://reference.aspose.com/medical/net/" >}}
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
