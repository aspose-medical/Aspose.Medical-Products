---
title: Anonymisera DICOM-filer i C# .NET | Aspose.Medical
weight: 1000
description: Anonymisera DICOM-filer i C# .NET med konfigurerbara sekretessprofiler. Ta bort patientens PII, säkerställ HIPAA- och GDPR-efterlevnad med Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Anonymisera DICOM-filer i .NET C#" h2="Ta bort patientidentifierande information från DICOM-filer med DICOM PS 3.15 sekretessprofiler. Säkerställ HIPAA- och GDPR-efterlevnad med ett rent .NET-bibliotek." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standardbaserad DICOM-anonymisering">}}

<p><strong>Aspose.Medical for .NET</strong> tillhandahåller ett omfattande DICOM-anonymiserings-API baserat på <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Confidentiality Profiles</a>. API:et möjliggör för utvecklare inom hälso- och sjukvård att ta bort eller ändra patientidentifierande information (PII) från DICOM-filer samtidigt som den kliniska värdet av bilddata bevaras. Till skillnad från enkla taggborttagningsmetoder följer Aspose.Medical den officiella DICOM-standarden för att säkerställa korrekt avidentifiering.</p>

<p>Klassen <code>Anonymizer</code> arbetar med <code>ConfidentialityProfile</code>-objekt som exakt definierar hur varje DICOM-attribut ska hanteras &mdash; om det ska tas bort, ersättas med ett dummy‑värde, rensas eller behållas oförändrat. Detta tillvägagångssätt säkerställer konsekvent, auditerbar anonymisering som uppfyller regulatoriska krav.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonymisera en DICOM-fil i C#">}}

<p>Det enklaste sättet att anonymisera en DICOM-fil är att använda standard sekretessprofilen. Detta tillämpar DICOM PS 3.15 Basic Profile, som hanterar alla standard patientidentifierande attribut:</p>

<div class="codeblock" id="code">
 <h3>Grundläggande DICOM-anonymisering - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>Metoden <code>Anonymize</code> är <strong>icke-destruktiv</strong> &mdash; den klonar den ursprungliga datasetet och returnerar en ny anonymiserad kopia. Den ursprungliga DICOM-filen förblir oförändrad, vilket är viktigt för revisionsspår och arbetsflöden för dataintegritet.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anpassad ersättning av patientidentitet">}}

<p>I många arbetsflöden behöver du ersätta patientinformation med specifika aliasvärden snarare än att bara ta bort den. Klassen <code>ConfidentialityProfile</code> låter dig ange ersättningsvärden för patientnamn och patient‑ID:</p>

<div class="codeblock" id="code">
 <h3>Anonymisera med anpassad patientidentitet - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="In-place-anonymisering">}}

<p>För scenarier där minneseffektivitet är viktig eller där du inte behöver behålla originaldata, erbjuder API:et in-place‑anonymisering som modifierar datasetet direkt:</p>

<div class="codeblock" id="code">
 <h3>In-place DICOM-anonymisering - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Både <code>Anonymize</code>- och <code>AnonymizeInPlace</code>-metoderna accepterar också ett <code>Dataset</code>-objekt direkt, vilket ger dig full kontroll över vilket dataset som bearbetas.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Konfigurerbara sekretessprofilalternativ">}}

<p>DICOM PS 3.15-standarden definierar en <strong>Basic Profile</strong> tillsammans med flera valfria profiler som styr hur specifika datakategorier hanteras under anonymisering. Du kan kombinera dessa alternativ med enum‑typen <code>ConfidentialityProfileOptions</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Alternativ</th>
<th>Beskrivning</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Standard DICOM PS 3.15 Basic Application Level Confidentiality Profile</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Behåll säkra privata attribut under anonymisering</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Behåll ursprungliga DICOM UID:er (Study, Series, Instance) oförändrade</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Behåll enhetsidentifieringsattribut (tillverkare, modell, stationsnamn)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Behåll institutionsidentifieringsattribut (namn, adress, avdelning)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Behåll patientkarakteristik (ålder, kön, storlek, vikt) för forskningsändamål</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Behåll fullständiga datum- och tidsvärden för longitudinella studier</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Behåll modifierade datum för longitudinella studier</td></tr>
<tr><td><code>CleanDesc</code></td><td>Rensa textbeskrivningar som kan innehålla identifierande information</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Rensa strukturerat innehåll som kan innehålla identifierande information</td></tr>
<tr><td><code>CleanGraph</code></td><td>Rensa grafisk data som kan innehålla inbränd patientinformation</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Anonymisera med specifika profilalternativ - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Åtgärdkoder för attributhantering">}}

<p>Varje attribut i en sekretessprofil tilldelas en åtgärdkod som definierar hur det ska bearbetas under anonymisering. Dessa följer DICOM PS 3.15 Tabell E.1-1a-standarden:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Kod</th>
<th>Åtgärd</th>
<th>Beskrivning</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Ersätt med dummy</td><td>Ersätt med ett värde av icke‑noll längd i enlighet med VR</td></tr>
<tr><td><strong>Z</strong></td><td>Zero‑length eller dummy</td><td>Ersätt med ett värde med noll längd eller ett dummy‑värde i enlighet med VR</td></tr>
<tr><td><strong>X</strong></td><td>Ta bort</td><td>Ta bort attributet helt från datasetet</td></tr>
<tr><td><strong>K</strong></td><td>Behåll</td><td>Behåll attributet oförändrat (för icke‑sekvenser) eller rensa (för sekvenser)</td></tr>
<tr><td><strong>C</strong></td><td>Rensa</td><td>Ersätt med ett rensat värde av liknande betydelse i enlighet med VR</td></tr>
<tr><td><strong>U</strong></td><td>Ersätt UID</td><td>Ersätt med ett nytt UID som är internt konsistent inom uppsättningen av instanser</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anpassade profiler från CSV, JSON och XML">}}

<p>För avancerade användningsfall kan du definiera anpassade anonymiseringsprofiler genom att läsa in regler från externa CSV-, JSON- eller XML-filer. Detta låter dig skräddarsy anonymiseringsbeteendet efter dina specifika organisationspolicyer eller regulatoriska krav:</p>

<div class="codeblock" id="code">
 <h3>Läs in anonymiseringsprofil från CSV - C#</h3>
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
 <h3>Läs in anonymiseringsprofil från JSON - C#</h3>
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

<p>Anpassade profiler kan också laddas från XML-filer (<code>LoadFromXmlFile</code>), strömmar (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) eller direkt från strängar (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HIPAA- och GDPR-efterlevnad">}}

<p>DICOM-filer innehåller rutinmässigt Protected Health Information (PHI) enligt HIPAA samt personuppgifter enligt GDPR. Aspose.Medical for .NET hjälper dig att uppfylla regulatoriska krav genom att tillhandahålla:</p>

<ul>
<li><strong>DICOM PS 3.15‑efterlevnad</strong>: Anonymiseringsmotorn följer den officiella DICOM-standarden för avidentifiering, vilket säkerställer att erkända bästa praxis tillämpas på alla relevanta attribut.</li>
<li><strong>Omfattande PII‑borttagning</strong>: Patientnamn, ID, födelsedatum, adress och andra identifierare hanteras enligt den konfigurerade sekretessprofilen.</li>
<li><strong>UID‑ ersättning</strong>: Study-, Series- och SOP Instance‑UID:er kan ersättas med nya, internt konsistenta UID:er för att förhindra återidentifiering via korsreferenser.</li>
<li><strong>Konfigurerbar behållning</strong>: När forsknings- eller kliniska behov kräver det kan specifika datakategorier (patientkarakteristik, datum, enhetsinformation) selektivt behållas med hjälp av de standardalternativprofilerna.</li>
<li><strong>Auditerbar process</strong>: Den standardbaserade metoden med definierade åtgärdkoder ger en klar, dokumenterbar anonymiseringsprocess för regulatoriska granskningar.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Lärresurser" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Källkod" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API-referenser" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Produktsupport" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Gratis support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Betald support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blogg" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Varför Aspose.Medical för .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Kundlista" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Framgångshistorier" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
