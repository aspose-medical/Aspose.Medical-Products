---
title: Anonimiseer DICOM-bestanden in C# .NET | Aspose.Medical
weight: 1000
description: Anonimiseer DICOM-bestanden in C# .NET met configureerbare vertrouwelijkheidsprofielen. Verwijder patiënt‑PII, zorg voor HIPAA- en GDPR-naleving met de Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Anonimiseer DICOM-bestanden in .NET C#" h2="Verwijder patiëntidentificerende informatie uit DICOM-bestanden met behulp van DICOM PS 3.15 vertrouwelijkheidsprofielen. Zorg voor HIPAA- en GDPR-naleving met een pure .NET-bibliotheek." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standaardgebaseerde DICOM-anonimisering">}}

<p><strong>Aspose.Medical for .NET</strong> biedt een uitgebreide DICOM-anonimisering API gebaseerd op de <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Confidentiality Profiles</a>. De API stelt zorgontwikkelaars in staat om patiëntidentificerende informatie (PII) uit DICOM-bestanden te verwijderen of te wijzigen, terwijl de klinische waarde van de beeldgegevens behouden blijft. In tegenstelling tot eenvoudige tag‑verwijderingsmethoden volgt Aspose.Medical de officiële DICOM-standaard om een correcte de‑identificatie te garanderen.</p>

<p>De <code>Anonymizer</code>-klasse werkt met <code>ConfidentialityProfile</code>-objecten die exact definiëren hoe elk DICOM‑attribuut moet worden verwerkt &mdash; of het moet worden verwijderd, vervangen door een dummy‑waarde, opgeschoond, of ongewijzigd blijven. Deze aanpak zorgt voor consistente, controleerbare anonimisatie die voldoet aan de regelgeving.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonimiseer een DICOM‑bestand in C#">}}

<p>De eenvoudigste manier om een DICOM‑bestand te anonimiseren is het gebruik van het standaard vertrouwelijkheidsprofiel. Dit past het DICOM PS 3.15 Basic Profile toe, dat alle standaard patiënt‑identificerende attributen verwerkt:</p>

<div class="codeblock" id="code">
 <h3>Basis DICOM-anonimisering - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>De <code>Anonymize</code>-methode is <strong>niet‑destructief</strong> &mdash; hij kloont de originele dataset en retourneert een nieuwe geanonimiseerde kopie. Het originele DICOM‑bestand blijft ongewijzigd, wat essentieel is voor audit‑trails en werkstromen voor gegevensintegriteit.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Aangepaste vervanging van patiëntidentiteit">}}

<p>In veel werkstromen moet u patiëntinformatie vervangen door specifieke alias‑waarden in plaats van deze eenvoudigweg te verwijderen. De <code>ConfidentialityProfile</code>-klasse maakt het mogelijk vervangingswaarden in te stellen voor de patiëntnaam en patiënt‑ID:</p>

<div class="codeblock" id="code">
 <h3>Anonimiseer met aangepaste patiëntidentiteit - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="In‑place anonimisering">}}

<p>Voor scenario's waarin geheugenefficiëntie belangrijk is of u de originele gegevens niet hoeft te behouden, biedt de API in‑place anonimisering die de dataset direct wijzigt:</p>

<div class="codeblock" id="code">
 <h3>In‑place DICOM-anonimisering - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Zowel de <code>Anonymize</code>- als de <code>AnonymizeInPlace</code>-methoden accepteren ook direct een <code>Dataset</code>-object, waardoor u volledige controle heeft over welke dataset wordt verwerkt.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Configureerbare opties voor vertrouwelijkheidsprofiel">}}

<p>De DICOM PS 3.15-standaard definieert een <strong>Basic Profile</strong> naast verschillende optionele profielen die bepalen hoe specifieke gegevenscategorieën worden behandeld tijdens anonimisering. U kunt deze opties combineren met behulp van de <code>ConfidentialityProfileOptions</code>-enum:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Optie</th>
<th>Beschrijving</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Het standaard DICOM PS 3.15 Basic Application Level Confidentiality Profile</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Behoud veilige private attributen tijdens anonimisering</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Behoud originele DICOM UID’s (Study, Series, Instance) ongewijzigd</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Behoud apparaat‑identificatie‑attributen (fabrikant, model, stationnaam)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Behoud institutionele identificatie‑attributen (naam, adres, afdeling)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Behoud patiëntkenmerken (leeftijd, geslacht, grootte, gewicht) voor onderzoeksdoeleinden</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Behoud volledige datum‑ en tijdwaarden voor longitudinale studies</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Behoud gewijzigde datums voor longitudinale studies</td></tr>
<tr><td><code>CleanDesc</code></td><td>Schoon tekstbeschrijvingen die identificerende informatie kunnen bevatten</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Schoon gestructureerde inhoud die identificerende informatie kan bevatten</td></tr>
<tr><td><code>CleanGraph</code></td><td>Schoon grafische gegevens die ingebakken patiëntinformatie kunnen bevatten</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Anonimiseer met specifieke profielopties - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Actiecodes voor attribuutbehandeling">}}

<p>Elk attribuut in een vertrouwelijkheidsprofiel krijgt een actiecode toegewezen die bepaalt hoe het moet worden verwerkt tijdens anonimisering. Deze volgen de DICOM PS 3.15 Table E.1-1a-standaard:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Code</th>
<th>Actie</th>
<th>Beschrijving</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Vervangen door dummy</td><td>Vervangen door een niet‑nul lengte waarde die consistent is met de VR</td></tr>
<tr><td><strong>Z</strong></td><td>Nul‑lengte of dummy</td><td>Vervangen door een nul‑lengte waarde of een dummy‑waarde die consistent is met de VR</td></tr>
<tr><td><strong>X</strong></td><td>Verwijderen</td><td>Verwijder het attribuut volledig uit de dataset</td></tr>
<tr><td><strong>K</strong></td><td>Behouden</td><td>Behoud het attribuut ongewijzigd (voor niet‑sequenties) of opgeschoond (voor sequenties)</td></tr>
<tr><td><strong>C</strong></td><td>Opschonen</td><td>Vervangen door een opgeschoonde waarde met een vergelijkbare betekenis die consistent is met de VR</td></tr>
<tr><td><strong>U</strong></td><td>Vervang UID</td><td>Vervangen door een nieuwe UID die intern consistent is binnen de set van instanties</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Aangepaste profielen uit CSV, JSON en XML">}}

<p>Voor geavanceerde use‑cases kunt u aangepaste anonimiseringprofielen definiëren door regels te laden uit externe CSV-, JSON- of XML‑bestanden. Hiermee kunt u het anonimisering‑gedrag afstemmen op uw specifieke organisatie‑beleid of reglementaire vereisten:</p>

<div class="codeblock" id="code">
 <h3>Laad anonimiseringprofiel vanuit CSV - C#</h3>
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
 <h3>Laad anonimiseringprofiel vanuit JSON - C#</h3>
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

<p>Aangepaste profielen kunnen ook worden geladen uit XML‑bestanden (<code>LoadFromXmlFile</code>), streams (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) of direct uit strings (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HIPAA- en GDPR-naleving">}}

<p>DICOM‑bestanden bevatten routinematig Protected Health Information (PHI) zoals gedefinieerd door HIPAA en persoonsgegevens zoals gedefinieerd door GDPR. Aspose.Medical for .NET helpt u te voldoen aan reglementaire eisen door het volgende te bieden:</p>

<ul>
<li><strong>DICOM PS 3.15‑naleving</strong>: De anonimisering‑engine volgt de officiële DICOM‑standaard voor de‑identificatie, waardoor erkende best practices op alle relevante attributen worden toegepast.</li>
<li><strong>Uitgebreide PII‑verwijdering</strong>: Patiëntnaam, ID, geboortedatum, adres en andere identificatoren worden behandeld volgens het geconfigureerde vertrouwelijkheidsprofiel.</li>
<li><strong>UID‑vervanging</strong>: Study-, Series- en SOP‑Instance‑UID’s kunnen worden vervangen door nieuwe, intern consistente UID’s om re‑identificatie via kruisverwijzingen te voorkomen.</li>
<li><strong>Configureerbare retentie</strong>: Wanneer onderzoeks- of klinische behoeften dit vereisen, kunnen specifieke gegevenscategorieën (patiëntkenmerken, datums, apparaat‑informatie) selectief worden behouden met de standaardoptie‑profielen.</li>
<li><strong>Auditbaar proces</strong>: De op standaarden gebaseerde aanpak met gedefinieerde actiecodes biedt een duidelijk, documenteerbaar anonimisatie‑proces voor reglementaire audits.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Leerbronnen" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentatie" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Broncode" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API‑referenties" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Productondersteuning" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis ondersteuning" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betaalde ondersteuning" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Waarom Aspose.Medical voor .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Klantenlijst" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Succesverhalen" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
