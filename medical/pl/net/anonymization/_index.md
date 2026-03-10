---
title: Anonimizuj pliki DICOM w C# .NET | Aspose.Medical
weight: 1000
description: Anonimizuj pliki DICOM w C# .NET przy użyciu konfigurowalnych profili poufności. Usuń dane osobowe pacjenta (PII), zapewnij zgodność z HIPAA i GDPR dzięki API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Anonimizuj pliki DICOM w .NET C#" h2="Usuń informacje identyfikujące pacjenta z plików DICOM przy użyciu profili poufności DICOM PS 3.15. Zapewnij zgodność z HIPAA i GDPR dzięki czystej bibliotece .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Anonimizacja DICOM oparta na standardach">}}

<p><strong>Aspose.Medical for .NET</strong> udostępnia kompleksowe API do anonimizacji DICOM oparte na <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Confidentiality Profiles</a>. API umożliwia programistom w dziedzinie opieki zdrowotnej usuwanie lub modyfikowanie informacji identyfikujących pacjenta (PII) z plików DICOM, zachowując jednocześnie wartość kliniczną danych obrazowych. W przeciwieństwie do prostych podejść polegających na usuwaniu tagów, Aspose.Medical stosuje oficjalny standard DICOM, aby zapewnić prawidłową dezidentyfikację.</p>

<p>Klasa <code>Anonymizer</code> współpracuje z obiektami <code>ConfidentialityProfile</code>, które dokładnie określają, jak każdy atrybut DICOM ma być obsłużony &mdash; czy ma być usunięty, zastąpiony wartością zastępczą, wyczyszczony, czy pozostawiony bez zmian. To podejście zapewnia spójną, audytowalną anonimizację spełniającą wymogi regulacyjne.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonimizuj plik DICOM w C#">}}

<p>Najprostszym sposobem anonimizacji pliku DICOM jest użycie domyślnego profilu poufności. Stosuje on DICOM PS 3.15 Basic Profile, który obsługuje wszystkie standardowe atrybuty identyfikujące pacjenta:</p>

<div class="codeblock" id="code">
 <h3>Podstawowa anonimizacja DICOM - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>Metoda <code>Anonymize</code> jest <strong>nie niszcząca</strong> &mdash; tworzy klon oryginalnego zestawu danych i zwraca nową zanonimizowaną kopię. Oryginalny plik DICOM pozostaje niezmieniony, co jest kluczowe dla ścieżek audytu i procesów zapewniających integralność danych.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Niestandardowa wymiana tożsamości pacjenta">}}

<p>W wielu przepływach pracy konieczne jest zastąpienie informacji o pacjencie określonymi wartościami aliasu, zamiast ich po prostu usunięcia. Klasa <code>ConfidentialityProfile</code> umożliwia ustawienie wartości zastępczych dla nazwy pacjenta i identyfikatora pacjenta:</p>

<div class="codeblock" id="code">
 <h3>Anonimizuj z niestandardową tożsamością pacjenta - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Anonimizacja w miejscu">}}

<p>W sytuacjach, gdy istotna jest wydajność pamięci lub nie ma potrzeby zachowywania oryginalnych danych, API oferuje anonimizację w miejscu, która modyfikuje zestaw danych bezpośrednio:</p>

<div class="codeblock" id="code">
 <h3>Anonimizacja DICOM w miejscu - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Zarówno metody <code>Anonymize</code>, jak i <code>AnonymizeInPlace</code> przyjmują również bezpośrednio obiekt <code>Dataset</code>, dając pełną kontrolę nad tym, który zestaw danych jest przetwarzany.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Konfigurowalne opcje profilu poufności">}}

<p>Standard DICOM PS 3.15 definiuje <strong>Basic Profile</strong> oraz kilka profilów opcjonalnych, które kontrolują sposób obsługi konkretnych kategorii danych podczas anonimizacji. Możesz łączyć te opcje przy użyciu wyliczenia <code>ConfidentialityProfileOptions</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Opcja</th>
<th>Opis</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Domyślny profil poufności DICOM PS 3.15 Basic Application Level</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Zachowaj bezpieczne prywatne atrybuty podczas anonimizacji</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Zachowaj niezmienione oryginalne UIDy DICOM (Study, Series, Instance)</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Zachowaj atrybuty identyfikacji urządzenia (producent, model, nazwa stacji)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Zachowaj atrybuty identyfikacji instytucji (nazwa, adres, wydział)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Zachowaj charakterystyki pacjenta (wiek, płeć, rozmiar, waga) w celach badawczych</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Zachowaj pełne wartości daty i czasu w badaniach longitudinalnych</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Zachowaj zmodyfikowane daty w badaniach longitudinalnych</td></tr>
<tr><td><code>CleanDesc</code></td><td>Wyczyść opisy tekstowe, które mogą zawierać informacje identyfikujące</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Wyczyść strukturalną zawartość, która może zawierać informacje identyfikujące</td></tr>
<tr><td><code>CleanGraph</code></td><td>Wyczyść dane graficzne, które mogą zawierać wbudowane informacje o pacjencie</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Anonimizuj z określonymi opcjami profilu - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kody akcji dla obsługi atrybutów">}}

<p>Każdemu atrybutowi w profilu poufności przypisany jest kod akcji określający, jak ma być przetwarzany podczas anonimizacji. Są one zgodne ze standardem DICOM PS 3.15 Table E.1-1a:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Kod</th>
<th>Akcja</th>
<th>Opis</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Zastąp wartością zastępczą</td><td>Zastąp wartością niezerowej długości zgodną z VR</td></tr>
<tr><td><strong>Z</strong></td><td>Wartość zerowej długości lub zastępcza</td><td>Zastąp wartością zerowej długości lub wartością zastępczą zgodną z VR</td></tr>
<tr><td><strong>X</strong></td><td>Usuń</td><td>Usuń atrybut całkowicie z zestawu danych</td></tr>
<tr><td><strong>K</strong></td><td>Zachowaj</td><td>Zachowaj atrybut niezmieniony (dla nie‑sekwencji) lub wyczyść (dla sekwencji)</td></tr>
<tr><td><strong>C</strong></td><td>Wyczyść</td><td>Zastąp wyczyszczoną wartością o podobnym znaczeniu zgodną z VR</td></tr>
<tr><td><strong>U</strong></td><td>Zastąp UID</td><td>Zastąp nowym UID, który jest wewnętrznie spójny w ramach zestawu instancji</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Niestandardowe profile z CSV, JSON i XML">}}

<p>W zaawansowanych przypadkach użycia możesz definiować niestandardowe profile anonimizacji, wczytując reguły z zewnętrznych plików CSV, JSON lub XML. Umożliwia to dostosowanie zachowania anonimizacji do konkretnych polityk organizacyjnych lub wymagań regulacyjnych:</p>

<div class="codeblock" id="code">
 <h3>Wczytaj profil anonimizacji z CSV - C#</h3>
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
 <h3>Wczytaj profil anonimizacji z JSON - C#</h3>
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

<p>Niestandardowe profile można także wczytać z plików XML (<code>LoadFromXmlFile</code>), strumieni (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) lub bezpośrednio z łańcuchów znaków (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zgodność z HIPAA i GDPR">}}

<p>Pliki DICOM zazwyczaj zawierają chronione informacje zdrowotne (PHI) określone przez HIPAA oraz dane osobowe określone przez GDPR. Aspose.Medical for .NET pomaga spełnić wymogi regulacyjne, zapewniając:</p>

<ul>
<li><strong>DICOM PS 3.15 compliance</strong>: Silnik anonimizacji stosuje oficjalny standard DICOM do dezidentyfikacji, zapewniając zastosowanie uznanych najlepszych praktyk do wszystkich istotnych atrybutów.</li>
<li><strong>Comprehensive PII removal</strong>: Nazwa pacjenta, ID, data urodzenia, adres oraz inne identyfikatory są obsługiwane zgodnie z skonfigurowanym profilem poufności.</li>
<li><strong>UID replacement</strong>: UIDy badania, serii i instancji SOP mogą być zastąpione nowymi, wewnętrznie spójnymi UIDami, aby zapobiec ponownej identyfikacji poprzez odniesienia krzyżowe.</li>
<li><strong>Configurable retention</strong>: Gdy potrzeby badawcze lub kliniczne tego wymagają, określone kategorie danych (charakterystyki pacjenta, daty, informacje o urządzeniu) mogą być selektywnie zachowane przy użyciu standardowych profili opcji.</li>
<li><strong>Auditable process</strong>: Podejście oparte na standardach z określonymi kodami akcji zapewnia przejrzysty, dokumentowalny proces anonimizacji dla audytów regulacyjnych.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zasoby edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Kod źródłowy" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referencje API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Wsparcie produktu" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Wsparcie darmowe" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Wsparcie płatne" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Dlaczego Aspose.Medical dla .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista klientów" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Historie sukcesu" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
