---
title: Anonymizujte soubory DICOM v C# .NET | Aspose.Medical
weight: 1000
description: Anonymizujte soubory DICOM v C# .NET pomocí konfigurovatelných profilů důvěrnosti. Odstraňte osobní údaje pacienta (PII), zajistěte soulad s HIPAA a GDPR pomocí Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Anonymizujte soubory DICOM v .NET C#" h2="Odstraňte identifikační informace pacienta ze souborů DICOM pomocí profilů důvěrnosti DICOM PS 3.15. Zajistěte soulad s HIPAA a GDPR pomocí čisté .NET knihovny." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standardově založená anonymizace DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> poskytuje komplexní API pro anonymizaci DICOM založené na <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Confidentiality Profiles</a>. API umožňuje vývojářům ve zdravotnictví odstraňovat nebo upravovat identifikační informace pacienta (PII) ze souborů DICOM při zachování klinické hodnoty zobrazovacích dat. Na rozdíl od jednoduchých přístupů odstraňujících značky Aspose.Medical dodržuje oficiální standard DICOM, aby zajistil řádnou de-identifikaci.</p>

<p>Třída <code>Anonymizer</code> pracuje s objekty <code>ConfidentialityProfile</code>, které přesně definují, jak má být každý atribut DICOM zpracován &mdash; zda má být odstraněn, nahrazen fiktivní hodnotou, vyčištěn nebo ponechán beze změny. Tento přístup zajišťuje konzistentní, auditovatelnou anonymizaci splňující regulační požadavky.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonymizujte soubor DICOM v C#">}}

<p>Nejjednodušší způsob, jak anonymizovat soubor DICOM, je použít výchozí profil důvěrnosti. Tento použije základní profil DICOM PS 3.15, který zpracovává všechny standardní atributy identifikující pacienta:</p>

<div class="codeblock" id="code">
 <h3>Základní anonymizace DICOM - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>Metoda <code>Anonymize</code> je <strong>nedestruktivní</strong> &mdash; klonuje původní dataset a vrací novou anonymizovanou kopii. Původní soubor DICOM zůstává beze změny, což je nezbytné pro auditní záznamy a workflow integrity dat.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vlastní nahrazení identity pacienta">}}

<p>V mnoha pracovních postupech potřebujete nahradit informace o pacientovi konkrétními aliasy místo jejich pouhého odstranění. Třída <code>ConfidentialityProfile</code> umožňuje nastavit hodnoty náhrady pro jméno pacienta a ID pacienta:</p>

<div class="codeblock" id="code">
 <h3>Anonymizace s vlastní identitou pacienta - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Anonymizace na místě">}}

<p>Pro scénáře, kde je důležitá úspora paměti nebo nepotřebujete uchovávat původní data, API poskytuje anonymizaci na místě, která přímo upravuje dataset:</p>

<div class="codeblock" id="code">
 <h3>Anonymizace DICOM na místě - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Obě metody <code>Anonymize</code> i <code>AnonymizeInPlace</code> také přijímají objekt <code>Dataset</code> přímo, což vám poskytuje plnou kontrolu nad tím, který dataset je zpracován.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Konfigurovatelné možnosti profilu důvěrnosti">}}

<p>Standard DICOM PS 3.15 definuje <strong>základní profil</strong> spolu s několika volitelnými profily, které řídí, jak jsou během anonymizace zpracovávány konkrétní kategorie dat. Tyto možnosti můžete kombinovat pomocí výčtu <code>ConfidentialityProfileOptions</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Volba</th>
<th>Popis</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Výchozí základní profil důvěrnosti úrovně aplikace DICOM PS 3.15</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Zachovat bezpečné soukromé atributy během anonymizace</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Zachovat původní DICOM UID (Study, Series, Instance) beze změny</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Zachovat atributy identifikace zařízení (výrobce, model, název stanice)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Zachovat atributy identifikace instituce (název, adresa, oddělení)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Zachovat charakteristiky pacienta (věk, pohlaví, velikost, váha) pro výzkumné účely</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Zachovat úplné datum a čas pro longitudinální studie</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Zachovat upravená data pro longitudinální studie</td></tr>
<tr><td><code>CleanDesc</code></td><td>Vyčistit textové popisy, které mohou obsahovat identifikační informace</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Vyčistit strukturovaný obsah, který může obsahovat identifikační informace</td></tr>
<tr><td><code>CleanGraph</code></td><td>Vyčistit grafická data, která mohou obsahovat vtržené informace o pacientovi</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Anonymizace s konkrétními možnostmi profilu - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Akční kódy pro zpracování atributů">}}

<p>Každému atributu v profilu důvěrnosti je přiřazen akční kód, který určuje, jak má být během anonymizace zpracován. Tyto kódy vycházejí ze standardu DICOM PS 3.15 Tabulka E.1-1a:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Kód</th>
<th>Akce</th>
<th>Popis</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Nahradit fiktivní hodnotou</td><td>Nahradit nenulovou hodnotou odpovídající VR</td></tr>
<tr><td><strong>Z</strong></td><td>Nula-délka nebo fiktivní</td><td>Nahradit hodnotou nulové délky nebo fiktivní hodnotou odpovídající VR</td></tr>
<tr><td><strong>X</strong></td><td>Odstranit</td><td>Úplně odstranit atribut z datasetu</td></tr>
<tr><td><strong>K</strong></td><td>Ponechat</td><td>Ponechat atribut beze změny (pro ne-sekvence) nebo vyčistit (pro sekvence)</td></tr>
<tr><td><strong>C</strong></td><td>Vyčistit</td><td>Nahradit vyčištěnou hodnotou podobného významu odpovídající VR</td></tr>
<tr><td><strong>U</strong></td><td>Nahradit UID</td><td>Nahradit novým UID, který je v rámci sady instancí interně konzistentní</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Vlastní profily z CSV, JSON a XML">}}

<p>Pro pokročilé scénáře můžete definovat vlastní profily anonymizace načtením pravidel z externích CSV, JSON nebo XML souborů. To vám umožní přizpůsobit chování anonymizace vašim konkrétním organizačním politikám nebo regulačním požadavkům:</p>

<div class="codeblock" id="code">
 <h3>Načtení profilu anonymizace z CSV - C#</h3>
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
 <h3>Načtení profilu anonymizace z JSON - C#</h3>
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

<p>Vlastní profily lze také načíst ze XML souborů (<code>LoadFromXmlFile</code>), proudů (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) nebo přímo ze řetězců (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Soulad s HIPAA a GDPR">}}

<p>Soubory DICOM běžně obsahují chráněné zdravotní informace (PHI) podle definice HIPAA a osobní údaje podle GDPR. Aspose.Medical pro .NET vám pomáhá splnit regulační požadavky tím, že poskytuje:</p>

<ul>
<li><strong>DICOM PS 3.15 compliance</strong>: Anonymizační engine dodržuje oficiální standard DICOM pro de-identifikaci, čímž zajišťuje uplatnění osvědčených postupů na všechny relevantní atributy.</li>
<li><strong>Comprehensive PII removal</strong>: Jméno pacienta, ID, datum narození, adresa a další identifikátory jsou zpracovány podle nakonfigurovaného profilu důvěrnosti.</li>
<li><strong>UID replacement</strong>: UID studií, sérií a SOP instancí mohou být nahrazeny novými, interně konzistentními UID, aby se zabránilo reidentifikaci přes křížové odkazy.</li>
<li><strong>Configurable retention</strong>: Když výzkum nebo klinické potřeby vyžadují, lze pomocí standardních profilů možností selektivně zachovat konkrétní kategorie dat (charakteristiky pacienta, data, informace o zařízení).</li>
<li><strong>Auditable process</strong>: Standardový přístup s definovanými akčními kódy poskytuje jasný, dokumentovatelný proces anonymizace pro regulatorní audity.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Vzdělávací zdroje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentace" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Zdrojový kód" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Reference API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Podpora produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Bezplatná podpora" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Placená podpora" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Proč Aspose.Medical pro .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Seznam zákazníků" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Úspěšné příběhy" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
