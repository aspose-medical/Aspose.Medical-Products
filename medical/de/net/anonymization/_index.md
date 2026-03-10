---
title: DICOM-Dateien in C# .NET anonymisieren | Aspose.Medical
weight: 1000
description: DICOM-Dateien in C# .NET mit konfigurierbaren Vertraulichkeitsprofilen anonymisieren. Patienten‑PII entfernen, HIPAA‑ und GDPR‑Konformität mit der Aspose.Medical API sicherstellen.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM-Dateien in .NET C# anonymisieren" h2="Identifizierende Patientendaten aus DICOM-Dateien mit DICOM PS 3.15 Vertraulichkeitsprofilen entfernen. HIPAA‑ und GDPR‑Konformität mit einer reinen .NET‑Bibliothek sicherstellen." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standardbasierte DICOM-Anonymisierung">}}

<p><strong>Aspose.Medical for .NET</strong> bietet eine umfassende DICOM-Anonymisierungs‑API, basierend auf den <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Confidentiality Profiles</a>. Die API ermöglicht es Entwicklern im Gesundheitswesen, patientenidentifizierende Informationen (PII) aus DICOM-Dateien zu entfernen oder zu modifizieren und dabei den klinischen Wert der Bilddaten zu erhalten. Im Gegensatz zu einfachen Tag‑Entfernungs‑Ansätzen folgt Aspose.Medical dem offiziellen DICOM‑Standard, um eine korrekte De‑Identifizierung sicherzustellen.</p>

<p>Die Klasse <code>Anonymizer</code> arbeitet mit <code>ConfidentialityProfile</code>-Objekten, die genau festlegen, wie jedes DICOM‑Attribut behandelt werden soll &mdash; ob es entfernt, durch einen Dummy‑Wert ersetzt, bereinigt oder unverändert belassen wird. Dieser Ansatz gewährleistet eine konsistente, prüfbare Anonymisierung, die den regulatorischen Anforderungen entspricht.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Eine DICOM-Datei in C# anonymisieren">}}

<p>Der einfachste Weg, eine DICOM‑Datei zu anonymisieren, besteht darin, das Standard‑Vertraulichkeitsprofil zu verwenden. Dieses wendet das DICOM PS 3.15 Basic Profile an, das alle standardmäßigen patientenidentifizierenden Attribute verarbeitet:</p>

<div class="codeblock" id="code">
 <h3>Basis‑DICOM-Anonymisierung - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>Die Methode <code>Anonymize</code> ist <strong>nicht‑destruktiv</strong> &mdash; sie klont den ursprünglichen Datensatz und gibt eine neue anonymisierte Kopie zurück. Die ursprüngliche DICOM‑Datei bleibt unverändert, was für Prüfpfade und Datenintegritäts‑Workflows entscheidend ist.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Benutzerdefinierter Austausch von Patientenidentität">}}

<p>In vielen Arbeitsabläufen müssen Patienteninformationen durch spezifische Alias‑Werte ersetzt werden, anstatt sie lediglich zu entfernen. Die Klasse <code>ConfidentialityProfile</code> ermöglicht das Festlegen von Ersatzwerten für den Patientennamen und die Patienten‑ID:</p>

<div class="codeblock" id="code">
 <h3>Mit benutzerdefinierter Patientenidentität anonymisieren - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create a confidentiality profile with custom replacement values
ConfidentialityProfile profile = new()
{
    PatientName = "ANONYMOUS PATIENT",
    PatientId = "00000000"
};

// Create an anonymizer with this profile
Anonymizer anonymizer = new(profile);

// Anonymize the file
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("custom_anonymized.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="In‑Place‑Anonymisierung">}}

<p>Für Szenarien, in denen Speichereffizienz wichtig ist oder die Originaldaten nicht erhalten bleiben müssen, bietet die API eine In‑Place‑Anonymisierung, die den Datensatz direkt modifiziert:</p>

<div class="codeblock" id="code">
 <h3>In‑Place‑DICOM-Anonymisierung - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Sowohl die Methoden <code>Anonymize</code> als auch <code>AnonymizeInPlace</code> akzeptieren ebenfalls ein <code>Dataset</code>-Objekt direkt, sodass Sie die volle Kontrolle darüber haben, welcher Datensatz verarbeitet wird.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Konfigurierbare Optionen für Vertraulichkeitsprofile">}}

<p>Der DICOM PS 3.15‑Standard definiert ein <strong>Basic Profile</strong> sowie mehrere optionale Profile, die steuern, wie bestimmte Datenkategorien während der Anonymisierung behandelt werden. Sie können diese Optionen mithilfe des Enums <code>ConfidentialityProfileOptions</code> kombinieren:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Option</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Das standardmäßige DICOM PS 3.15 Basic Application Level Confidentiality Profile</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Sichere private Attribute während der Anonymisierung beibehalten</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Originale DICOM‑UIDs (Study, Series, Instance) unverändert beibehalten</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Geräteidentifikationsattribute (Hersteller, Modell, Stationsname) beibehalten</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Institutionsidentifikationsattribute (Name, Adresse, Abteilung) beibehalten</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Patientencharakteristika (Alter, Geschlecht, Größe, Gewicht) zu Forschungszwecken beibehalten</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Vollständige Datums‑ und Zeitwerte für longitudinale Studien beibehalten</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Modifizierte Daten für longitudinale Studien beibehalten</td></tr>
<tr><td><code>CleanDesc</code></td><td>Textbeschreibungen, die identifizierende Informationen enthalten könnten, bereinigen</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Strukturierten Inhalt, der identifizierende Informationen enthalten könnte, bereinigen</td></tr>
<tr><td><code>CleanGraph</code></td><td>Grafische Daten, die eingebettete Patientendaten enthalten könnten, bereinigen</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Mit spezifischen Profiloptionen anonymisieren - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Aktionscodes für die Attributverarbeitung">}}

<p>Jedem Attribut in einem Vertraulichkeitsprofil ist ein Aktionscode zugewiesen, der definiert, wie es während der Anonymisierung verarbeitet werden soll. Diese folgen dem DICOM PS 3.15 Table E.1-1a Standard:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Code</th>
<th>Aktion</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Durch Dummy ersetzen</td><td>Durch einen nicht‑null‑Länge‑Wert ersetzen, der zum VR passt</td></tr>
<tr><td><strong>Z</strong></td><td>Null‑Länge oder Dummy</td><td>Durch einen Null‑Länge‑Wert oder einen Dummy‑Wert ersetzen, der zum VR passt</td></tr>
<tr><td><strong>X</strong></td><td>Entfernen</td><td>Das Attribut vollständig aus dem Datensatz entfernen</td></tr>
<tr><td><strong>K</strong></td><td>Behalten</td><td>Das Attribut unverändert beibehalten (bei Nicht‑Sequenzen) oder bereinigen (bei Sequenzen)</td></tr>
<tr><td><strong>C</strong></td><td>Bereinigen</td><td>Durch einen bereinigten Wert mit ähnlicher Bedeutung ersetzen, der zum VR passt</td></tr>
<tr><td><strong>U</strong></td><td>UID ersetzen</td><td>Durch eine neue UID ersetzen, die intern innerhalb des Satzes von Instanzen konsistent ist</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Benutzerdefinierte Profile aus CSV, JSON und XML">}}

<p>Für erweiterte Anwendungsfälle können Sie benutzerdefinierte Anonymisierungsprofile definieren, indem Sie Regeln aus externen CSV‑, JSON‑ oder XML‑Dateien laden. Dadurch lässt sich das Anonymisierungsverhalten an Ihre spezifischen Organisationsrichtlinien oder regulatorischen Anforderungen anpassen:</p>

<div class="codeblock" id="code">
 <h3>Anonymisierungsprofil aus CSV laden - C#</h3>
 <pre><code class="cs">// CSV format: TagPattern;Action
// 0010,0010;Z   (Anonymize PatientName)
// 0010,0020;D   (Replace PatientID with dummy)
// 0020,000D;U   (Replace StudyInstanceUID)

var profile = ConfidentialityProfile.LoadFromCsvFile(
    "anonymization_rules.csv",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Anonymisierungsprofil aus JSON laden - C#</h3>
 <pre><code class="cs">// JSON format:
// [
//     { "Tag": "0010,0010", "Action": "Z" },
//     { "Tag": "0010,0020", "Action": "D" },
//     { "Tag": "0020,000D", "Action": "U" }
// ]

var profile = ConfidentialityProfile.LoadFromJsonFile(
    "anonymization_rules.json",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<p>Benutzerdefinierte Profile können auch aus XML‑Dateien (<code>LoadFromXmlFile</code>), Streams (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) oder direkt aus Zeichenketten (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>) geladen werden.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HIPAA‑ und GDPR‑Konformität">}}

<p>DICOM‑Dateien enthalten routinemäßig geschützte Gesundheitsinformationen (PHI) gemäß HIPAA sowie personenbezogene Daten gemäß GDPR. Aspose.Medical for .NET unterstützt Sie dabei, regulatorische Anforderungen zu erfüllen, indem es Folgendes bereitstellt:</p>

<ul>
<li><strong>DICOM PS 3.15‑Konformität</strong>: Die Anonymisierungs‑Engine folgt dem offiziellen DICOM‑Standard für De‑Identifizierung und stellt sicher, dass anerkannte Best Practices auf alle relevanten Attribute angewendet werden.</li>
<li><strong>Umfassende PII‑Entfernung</strong>: Patientenname, ID, Geburtsdatum, Adresse und weitere Identifikatoren werden gemäß dem konfigurierten Vertraulichkeitsprofil verarbeitet.</li>
<li><strong>UID‑Ersetzung</strong>: Study-, Series‑ und SOP‑Instance‑UIDs können durch neue, intern konsistente UIDs ersetzt werden, um eine Re‑Identifizierung durch Querverweise zu verhindern.</li>
<li><strong>Konfigurierbarer Erhalt</strong>: Wenn Forschungs- oder klinische Bedürfnisse es erfordern, können bestimmte Datenkategorien (Patientencharakteristika, Daten, Geräteinformationen) mithilfe der Standard‑Optionsprofile selektiv erhalten bleiben.</li>
<li><strong>Prüfbarkeit des Prozesses</strong>: Der standardbasierte Ansatz mit definierten Aktionscodes liefert einen klaren, dokumentierbaren Anonymisierungsprozess für regulatorische Audits.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lernressourcen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Quellcode" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑Referenzen" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktunterstützung" tabId="support" >}}
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
