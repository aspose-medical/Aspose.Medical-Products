---
title: DICOM fájlok anonimizálása C# .NET-ben | Aspose.Medical
weight: 1000
description: Anonimizálja a DICOM fájlokat C# .NET-ben konfigurálható titoktartási profilok segítségével. Távolítsa el a beteg személyes adatait (PII), és biztosítsa a HIPAA és GDPR megfelelőséget az Aspose.Medical API-val.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOM fájlok anonimizálása .NET C#-ban" h2="Távolítsa el a beteg azonosító információit a DICOM fájlokból a DICOM PS 3.15 titoktartási profilok használatával. Biztosítsa a HIPAA és GDPR megfelelőséget egy tiszta .NET könyvtárral." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Szabványalapú DICOM anonimizálás">}}

<p><strong>Aspose.Medical for .NET</strong> átfogó DICOM anonimizálási API-t biztosít a <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Titoktartási Profilok</a> alapján. Az API lehetővé teszi az egészségügyi fejlesztők számára, hogy eltávolítsák vagy módosítsák a beteg azonosító információit (PII) a DICOM fájlokból, miközben megőrzik a képalkotó adatok klinikai értékét. Az egyszerű címke-eltávolítási módszerekkel ellentétben az Aspose.Medical a hivatalos DICOM szabványt követi a megfelelő adatnévtelenítés érdekében.</p>

<p>A <code>Anonymizer</code> osztály a <code>ConfidentialityProfile</code> objektumokkal dolgozik, amelyek pontosan meghatározzák, hogy egy DICOM attribútumot hogyan kell kezelni — eltávolítandó, dummy értékkel helyettesítendő, tisztítandó vagy változatlanul hagyandó. Ez a megközelítés biztosítja az egységes, auditálható anonimizálást, amely megfelel a szabályozási követelményeknek.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM fájl anonimizálása C#-ban">}}

<p>A DICOM fájl anonimizálásának legegyszerűbb módja az alapértelmezett titoktartási profil használata. Ez a DICOM PS 3.15 Alapprofil alkalmazását jelenti, amely kezeli az összes szabványos beteg-azonosító attribútumot:</p>

<div class="codeblock" id="code">
 <h3>Alap DICOM anonimizálás - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>A <code>Anonymize</code> metódus <strong>nem destruktív</strong> &mdash; az eredeti adathalmazt klónozza, és egy új anonimizált másolatot ad vissza. Az eredeti DICOM fájl változatlan marad, ami elengedhetetlen az audit nyomvonalakhoz és az adatintegritási munkafolyamatokhoz.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Egyéni betegazonosító helyettesítés">}}

<p>Sok munkafolyamatban szükség van a beteg információinak konkrét alias értékekkel való helyettesítésére, nem csak egyszerű eltávolításra. A <code>ConfidentialityProfile</code> osztály lehetővé teszi a beteg nevének és betegazonosítójának helyettesítő értékek megadását:</p>

<div class="codeblock" id="code">
 <h3>Anonymizálás egyéni betegazonosítóval - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Helyben történő anonimizálás">}}

<p>Azokban a helyzetekben, ahol a memóriahatékonyság fontos, vagy nem szükséges megtartani az eredeti adatot, az API helyben történő anonimizálást kínál, amely közvetlenül módosítja az adathalmazt:</p>

<div class="codeblock" id="code">
 <h3>Helyben történő DICOM anonimizálás - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Az <code>Anonymize</code> és az <code>AnonymizeInPlace</code> metódusok is közvetlenül elfogadják a <code>Dataset</code> objektumot, így teljes irányítást biztosítanak arról, mely adathalmazt dolgozzák fel.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Konfigurálható titoktartási profil beállítások">}}

<p>A DICOM PS 3.15 szabvány egy <strong>Alapprofilt</strong> és több opcionális profilt határoz meg, amelyek szabályozzák az adat bizonyos kategóriáinak anonimizálás közbeni kezelését. Ezeket a beállításokat a <code>ConfidentialityProfileOptions</code> enum segítségével kombinálhatja:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Beállítás</th>
<th>Leírás</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Az alapértelmezett DICOM PS 3.15 Alap Alkalmazási Szintű Titoktartási Profil</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Biztonságos privát attribútumok megtartása az anonimizálás során</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Az eredeti DICOM UID-ek (Study, Series, Instance) változatlanul megtartása</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Az eszköz azonosító attribútumok (gyártó, modell, állomás neve) megtartása</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Az intézmény azonosító attribútumok (név, cím, részleg) megtartása</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Beteg jellemzőinek (kor, nem, méret, súly) megtartása kutatási célokra</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Teljes dátum- és időértékek megtartása longitudinális vizsgálatokhoz</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Módosított dátumok megtartása a longitudinális vizsgálatokban</td></tr>
<tr><td><code>CleanDesc</code></td><td>Az azonosító információt tartalmazó szöveges leírások tisztítása</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Az azonosító információt tartalmazó strukturált tartalom tisztítása</td></tr>
<tr><td><code>CleanGraph</code></td><td>Beégetett beteginformációkat tartalmazó grafikus adatok tisztítása</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Anonymizálás specifikus profilbeállításokkal - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Műveleti kódok attribútumkezeléshez">}}

<p>Minden attribútumhoz a titoktartási profilban egy műveleti kód van rendelve, amely meghatározza, hogyan kell feldolgozni az anonimizálás során. Ezek a DICOM PS 3.15 E.1-1a táblázat szabványa szerint kerülnek meghatározásra:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Kód</th>
<th>Művelet</th>
<th>Leírás</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Helyettesítés dummy értékkel</td><td>Nem nulla hosszúságú, a VR-nek megfelelő értékre cserél</td></tr>
<tr><td><strong>Z</strong></td><td>Nullahosszú vagy dummy</td><td>Nullahosszú vagy a VR-nek megfelelő dummy értékre cserél</td></tr>
<tr><td><strong>X</strong></td><td>Eltávolítás</td><td>Az attribútum teljes eltávolítása az adathalmazból</td></tr>
<tr><td><strong>K</strong></td><td>Megtartás</td><td>Az attribútum változatlanul megtartása (nem szekvenciák esetén) vagy tisztítása (szekvenciák esetén)</td></tr>
<tr><td><strong>C</strong></td><td>Tisztítás</td><td>Hasonló jelentésű, a VR-nek megfelelő tisztított értékre cserél</td></tr>
<tr><td><strong>U</strong></td><td>UID helyettesítése</td><td>Új, a példánykészletben belsőleg koherens UID-re cserél</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Egyéni profilok CSV, JSON és XML fájlokból">}}

<p>Haladó felhasználási esetekben egyedi anonimizálási profilokat definiálhat külső CSV, JSON vagy XML fájlokból betöltött szabályok alapján. Ez lehetővé teszi, hogy az anonimizálási viselkedést a szervezeti szabályzataihoz vagy a szabályozási követelményekhez igazítsa:</p>

<div class="codeblock" id="code">
 <h3>Anonimizálási profil betöltése CSV-ből - C#</h3>
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
 <h3>Anonimizálási profil betöltése JSON-ból - C#</h3>
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

<p>Az egyéni profilok betölthetők XML fájlokból (<code>LoadFromXmlFile</code>), adatfolyamból (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>), vagy közvetlenül karakterláncokból (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HIPAA és GDPR megfelelőség">}}

<p>A DICOM fájlok rendszeresen tartalmaznak védett egészségügyi információkat (PHI) a HIPAA szerint, valamint személyes adatokat a GDPR szerint. Az Aspose.Medical for .NET segít a szabályozási követelmények teljesítésében a következő funkciók biztosításával:</p>

<ul>
<li><strong>DICOM PS 3.15 megfelelőség</strong>: Az anonimizálási motor a hivatalos DICOM szabványt követi az adatnévtelenítés során, biztosítva, hogy a elismert legjobb gyakorlatok minden releváns attribútumra alkalmazva legyenek.</li>
<li><strong>Átfogó PII eltávolítás</strong>: A beteg neve, azonosítója, születési dátuma, címe és egyéb azonosítók a konfigurált titoktartási profil szerint kezelődnek.</li>
<li><strong>UID helyettesítés</strong>: A Study, Series és SOP Instance UID-ek új, belsőleg koherens UID-ekre cserélhetők a kereszt-hivatkozásból eredő újraazonosítás megakadályozása érdekében.</li>
<li><strong>Konfigurálható megtartás</strong>: Kutatási vagy klinikai igény esetén adott adatkategóriák (betegjellemzők, dátumok, eszközinformációk) szelektíven megtarthatók a szabványos opcióprofillel.</li>
<li><strong>Auditálható folyamat</strong>: A szabványalapú megközelítés meghatározott műveleti kódokkal világos, dokumentálható anonimizálási folyamatot biztosít a szabályozói auditok számára.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Tanulási források" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentáció" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Forráskód" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API hivatkozások" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Terméktámogatás" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ingyenes támogatás" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Fizetett támogatás" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Miért az Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Ügyfelek listája" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Sikersztorik" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
