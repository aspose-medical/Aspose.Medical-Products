---
title: Anonymiser les fichiers DICOM en C# .NET | Aspose.Medical
weight: 1000
description: Anonymisez les fichiers DICOM en C# .NET à l’aide de profils de confidentialité configurables. Supprimez les informations personnelles identifiables (PII) des patients, assurez la conformité HIPAA et GDPR avec l’API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Anonymiser les fichiers DICOM en .NET C#" h2="Supprimez les informations d’identification du patient des fichiers DICOM en utilisant les profils de confidentialité DICOM PS 3.15. Assurez la conformité HIPAA et GDPR avec une bibliothèque pure .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Anonymisation DICOM basée sur les normes">}}

<p><strong>Aspose.Medical for .NET</strong> fournit une API complète d’anonymisation DICOM basée sur les <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">Profils de confidentialité DICOM PS 3.15</a>. L’API permet aux développeurs de santé de supprimer ou de modifier les informations d’identification du patient (PII) des fichiers DICOM tout en préservant la valeur clinique des données d’imagerie. Contrairement aux approches simples de suppression de balises, Aspose.Medical suit la norme officielle DICOM pour garantir une dé‑identification correcte.</p>

<p>La classe <code>Anonymizer</code> fonctionne avec des objets <code>ConfidentialityProfile</code> qui définissent précisément comment chaque attribut DICOM doit être traité &mdash; qu’il soit supprimé, remplacé par une valeur factice, nettoyé ou conservé tel quel. Cette approche garantit une anonymisation cohérente et auditable répondant aux exigences réglementaires.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonymiser un fichier DICOM en C#">}}

<p>La façon la plus simple d’anonymiser un fichier DICOM consiste à utiliser le profil de confidentialité par défaut. Celui‑ci applique le Basic Profile du DICOM PS 3.15, qui gère tous les attributs standard d’identification du patient :</p>

<div class="codeblock" id="code">
 <h3>Anonymisation DICOM de base - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>La méthode <code>Anonymize</code> est <strong>non destructive</strong> &mdash; elle clone le jeu de données original et renvoie une nouvelle copie anonymisée. Le fichier DICOM d’origine reste inchangé, ce qui est essentiel pour les pistes d’audit et les flux de travail d’intégrité des données.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Remplacement personnalisé de l’identité du patient">}}

<p>Dans de nombreux flux de travail, il faut remplacer les informations patient par des valeurs d’alias spécifiques plutôt que de les supprimer simplement. La classe <code>ConfidentialityProfile</code> vous permet de définir des valeurs de remplacement pour le nom du patient et l’ID du patient :</p>

<div class="codeblock" id="code">
 <h3>Anonymiser avec une identité patient personnalisée - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Anonymisation en place">}}

<p>Pour les scénarios où l’efficacité mémoire est importante ou lorsqu’il n’est pas nécessaire de conserver les données originales, l’API propose une anonymisation en place qui modifie directement le jeu de données :</p>

<div class="codeblock" id="code">
 <h3>Anonymisation DICOM en place - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Les méthodes <code>Anonymize</code> et <code>AnonymizeInPlace</code> acceptent également un objet <code>Dataset</code> directement, vous offrant un contrôle complet sur le jeu de données à traiter.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Options configurables du profil de confidentialité">}}

<p>La norme DICOM PS 3.15 définit un <strong>Basic Profile</strong> ainsi que plusieurs profils optionnels qui contrôlent la manière dont des catégories spécifiques de données sont traitées lors de l’anonymisation. Vous pouvez combiner ces options à l’aide de l’énumération <code>ConfidentialityProfileOptions</code> :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Option</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Le profil de confidentialité DICOM PS 3.15 de niveau d’application de base par défaut</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Conserver les attributs privés sûrs pendant l’anonymisation</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Conserver les UID DICOM originaux (Study, Series, Instance) inchangés</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Conserver les attributs d’identification du dispositif (fabricant, modèle, nom de station)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Conserver les attributs d’identification de l’institution (nom, adresse, département)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Conserver les caractéristiques du patient (âge, sexe, taille, poids) à des fins de recherche</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Conserver les valeurs complètes de date et d’heure pour les études longitudinales</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Conserver les dates modifiées pour les études longitudinales</td></tr>
<tr><td><code>CleanDesc</code></td><td>Nettoyer les descriptions textuelles pouvant contenir des informations d’identification</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Nettoyer le contenu structuré pouvant contenir des informations d’identification</td></tr>
<tr><td><code>CleanGraph</code></td><td>Nettoyer les données graphiques pouvant contenir des informations patient incrustées</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Anonymiser avec des options de profil spécifiques - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Codes d’action pour le traitement des attributs">}}

<p>Chaque attribut d’un profil de confidentialité se voit attribuer un code d’action qui définit comment il doit être traité lors de l’anonymisation. Ceux‑ci respectent la norme DICOM PS 3.15 Table E.1-1a :</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Code</th>
<th>Action</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Remplacer par une valeur factice</td><td>Remplacer par une valeur de longueur non nulle conforme au VR</td></tr>
<tr><td><strong>Z</strong></td><td>Longueur zéro ou factice</td><td>Remplacer par une valeur de longueur zéro ou une valeur factice conforme au VR</td></tr>
<tr><td><strong>X</strong></td><td>Supprimer</td><td>Supprimer complètement l’attribut du jeu de données</td></tr>
<tr><td><strong>K</strong></td><td>Conserver</td><td>Conserver l’attribut tel quel (pour les non‑séquences) ou le nettoyer (pour les séquences)</td></tr>
<tr><td><strong>C</strong></td><td>Nettoyer</td><td>Remplacer par une valeur nettoyée de signification similaire conforme au VR</td></tr>
<tr><td><strong>U</strong></td><td>Remplacer UID</td><td>Remplacer par un nouvel UID cohérent à l’interne dans l’ensemble des instances</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Profils personnalisés depuis CSV, JSON et XML">}}

<p>Pour des cas d’utilisation avancés, vous pouvez définir des profils d’anonymisation personnalisés en chargeant des règles à partir de fichiers CSV, JSON ou XML externes. Cela vous permet d’adapter le comportement d’anonymisation à vos politiques organisationnelles spécifiques ou aux exigences réglementaires :</p>

<div class="codeblock" id="code">
 <h3>Charger un profil d’anonymisation depuis CSV - C#</h3>
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
 <h3>Charger un profil d’anonymisation depuis JSON - C#</h3>
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

<p>Les profils personnalisés peuvent également être chargés depuis des fichiers XML (<code>LoadFromXmlFile</code>), des flux (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) ou directement depuis des chaînes de caractères (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Conformité HIPAA et GDPR">}}

<p>Les fichiers DICOM contiennent régulièrement des Protected Health Information (PHI) telles que définies par HIPAA et des données personnelles telles que définies par le GDPR. Aspose.Medical for .NET vous aide à satisfaire les exigences réglementaires en offrant :</p>

<ul>
<li><strong>Conformité DICOM PS 3.15</strong> : le moteur d’anonymisation suit la norme officielle DICOM pour la dé‑identification, garantissant que les meilleures pratiques reconnues sont appliquées à tous les attributs pertinents.</li>
<li><strong>Suppression complète des PII</strong> : le nom, l’ID, la date de naissance, l’adresse du patient et d’autres identifiants sont traités conformément au profil de confidentialité configuré.</li>
<li><strong>Remplacement des UID</strong> : les UID d’étude, de série et d’instance SOP peuvent être remplacés par de nouveaux UID cohérents en interne afin de prévenir la ré‑identification par recoupement.</li>
<li><strong>Rétention configurable</strong> : lorsque les besoins de recherche ou cliniques l’exigent, des catégories spécifiques de données (caractéristiques du patient, dates, informations sur le dispositif) peuvent être retenues sélectivement à l’aide des profils d’options standard.</li>
<li><strong>Processus auditable</strong> : l’approche basée sur les normes avec des codes d’action définis fournit un processus d’anonymisation clair et documentable pour les audits réglementaires.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Ressources d’apprentissage" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Code source" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Références API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Support produit" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Support gratuit" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Support payant" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Pourquoi Aspose.Medical pour .NET ?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Liste des clients" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Histoires de réussite" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
