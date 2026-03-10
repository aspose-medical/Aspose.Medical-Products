---
title: Anonimizzare file DICOM in C# .NET | Aspose.Medical
weight: 1000
description: Anonimizzare file DICOM in C# .NET utilizzando profili di riservatezza configurabili. Rimuovi le informazioni personali del paziente (PII), garantisci la conformità a HIPAA e GDPR con l'API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Anonimizzare file DICOM in .NET C#" h2="Rimuovi le informazioni identificative del paziente dai file DICOM utilizzando i profili di riservatezza DICOM PS 3.15. Garantisci la conformità a HIPAA e GDPR con una libreria .NET pura." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Anonimizzazione DICOM basata su standard">}}

<p><strong>Aspose.Medical for .NET</strong> fornisce un'API completa per l'anonimizzazione DICOM basata sui <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">Profili di riservatezza DICOM PS 3.15</a>. L'API consente agli sviluppatori nel settore sanitario di rimuovere o modificare le informazioni identificative del paziente (PII) dai file DICOM preservando al contempo il valore clinico dei dati di imaging. A differenza degli approcci semplici di rimozione dei tag, Aspose.Medical segue lo standard DICOM ufficiale per garantire una corretta de‑identificazione.</p>

<p>La classe <code>Anonymizer</code> lavora con oggetti <code>ConfidentialityProfile</code> che definiscono esattamente come deve essere gestito ciascun attributo DICOM &mdash; se deve essere rimosso, sostituito con un valore fittizio, pulito o mantenuto invariato. Questo approccio garantisce un'anonimizzazione coerente e verificabile che soddisfa i requisiti normativi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonimizzare un file DICOM in C#">}}

<p>Il modo più semplice per anonimizzare un file DICOM è utilizzare il profilo di riservatezza predefinito. Questo applica il Profilo Base DICOM PS 3.15, che gestisce tutti gli attributi standard di identificazione del paziente:</p>

<div class="codeblock" id="code">
 <h3>Anonimizzazione DICOM di base - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>Il metodo <code>Anonymize</code> è <strong>non distruttivo</strong> &mdash; clona il dataset originale e restituisce una nuova copia anonimizzata. Il file DICOM originale rimane invariato, il che è essenziale per le tracce di audit e i flussi di lavoro di integrità dei dati.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sostituzione personalizzata dell'identità del paziente">}}

<p>In molti flussi di lavoro, è necessario sostituire le informazioni del paziente con valori alias specifici anziché semplicemente rimuoverle. La classe <code>ConfidentialityProfile</code> consente di impostare valori di sostituzione per il nome del paziente e l'ID del paziente:</p>

<div class="codeblock" id="code">
 <h3>Anonimizzare con identità del paziente personalizzata - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Anonimizzazione in loco">}}

<p>Per scenari in cui l'efficienza della memoria è importante o non è necessario conservare i dati originali, l'API offre anonimizzazione in loco che modifica direttamente il dataset:</p>

<div class="codeblock" id="code">
 <h3>Anonimizzazione DICOM in loco - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Sia i metodi <code>Anonymize</code> che <code>AnonymizeInPlace</code> accettano anche un oggetto <code>Dataset</code> direttamente, fornendo il pieno controllo sul dataset da elaborare.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opzioni configurabili del profilo di riservatezza">}}

<p>Lo standard DICOM PS 3.15 definisce un <strong>Profilo Base</strong> insieme a diversi profili opzionali che controllano come le specifiche categorie di dati vengano gestite durante l'anonimizzazione. È possibile combinare queste opzioni usando l'enumerazione <code>ConfidentialityProfileOptions</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Opzione</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Il profilo di riservatezza a livello di applicazione base DICOM PS 3.15 predefinito</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Mantiene gli attributi privati sicuri durante l'anonimizzazione</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Mantiene invariati gli UID DICOM originali (Studio, Serie, Istanza)</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Mantiene gli attributi di identificazione del dispositivo (produttore, modello, nome della stazione)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Mantiene gli attributi di identificazione dell'istituzione (nome, indirizzo, reparto)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Mantiene le caratteristiche del paziente (età, sesso, dimensioni, peso) per scopi di ricerca</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Mantiene i valori completi di data e ora per studi longitudinali</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Mantiene le date modificate per studi longitudinali</td></tr>
<tr><td><code>CleanDesc</code></td><td>Pulisce le descrizioni testuali che possono contenere informazioni identificative</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Pulisce il contenuto strutturato che può contenere informazioni identificative</td></tr>
<tr><td><code>CleanGraph</code></td><td>Pulisce i dati grafici che possono contenere informazioni del paziente incorporate</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Anonimizzare con opzioni di profilo specifiche - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Codici di azione per la gestione degli attributi">}}

<p>Ad ogni attributo in un profilo di riservatezza è assegnato un codice di azione che definisce come deve essere elaborato durante l'anonimizzazione. Questi seguono lo standard DICOM PS 3.15 Tabella E.1-1a:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Codice</th>
<th>Azione</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Sostituisci con dummy</td><td>Sostituisce con un valore di lunghezza non zero coerente con il VR</td></tr>
<tr><td><strong>Z</strong></td><td>Lunghezza zero o dummy</td><td>Sostituisce con un valore a lunghezza zero o un valore dummy coerente con il VR</td></tr>
<tr><td><strong>X</strong></td><td>Rimuovi</td><td>Rimuove completamente l'attributo dal dataset</td></tr>
<tr><td><strong>K</strong></td><td>Mantieni</td><td>Mantiene l'attributo invariato (per non-sequenze) o pulito (per sequenze)</td></tr>
<tr><td><strong>C</strong></td><td>Pulisci</td><td>Sostituisce con un valore pulito di significato simile coerente con il VR</td></tr>
<tr><td><strong>U</strong></td><td>Sostituisci UID</td><td>Sostituisce con un nuovo UID che è internamente coerente all'interno del set di istanze</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Profili personalizzati da CSV, JSON e XML">}}

<p>Per casi d'uso avanzati, è possibile definire profili di anonimizzazione personalizzati caricando regole da file CSV, JSON o XML esterni. Questo consente di personalizzare il comportamento di anonimizzazione in base alle politiche organizzative o ai requisiti normativi specifici:</p>

<div class="codeblock" id="code">
 <h3>Carica profilo di anonimizzazione da CSV - C#</h3>
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
 <h3>Carica profilo di anonimizzazione da JSON - C#</h3>
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

<p>I profili personalizzati possono anche essere caricati da file XML (<code>LoadFromXmlFile</code>), stream (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) o direttamente da stringhe (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Conformità a HIPAA e GDPR">}}

<p>I file DICOM contengono regolarmente Informazioni Sanitarie Protette (PHI) come definito da HIPAA e dati personali come definito dal GDPR. Aspose.Medical per .NET ti aiuta a soddisfare i requisiti normativi fornendo:</p>

<ul>
<li><strong>Conformità DICOM PS 3.15</strong>: Il motore di anonimizzazione segue lo standard DICOM ufficiale per la de‑identificazione, garantendo l'applicazione delle migliori pratiche riconosciute a tutti gli attributi pertinenti.</li>
<li><strong>Rimozione completa di PII</strong>: Il nome del paziente, l'ID, la data di nascita, l'indirizzo e altri identificatori sono gestiti secondo il profilo di riservatezza configurato.</li>
<li><strong>Sostituzione UID</strong>: Gli UID di Studio, Serie e Istanza SOP possono essere sostituiti con nuovi UID internamente coerenti per prevenire la re‑identificazione tramite riferimenti incrociati.</li>
<li><strong>Conservazione configurabile</strong>: Quando le esigenze di ricerca o cliniche lo richiedono, specifiche categorie di dati (caratteristiche del paziente, date, informazioni sul dispositivo) possono essere conservate in modo selettivo utilizzando i profili di opzione standard.</li>
<li><strong>Processo verificabile</strong>: L'approccio basato su standard con codici di azione definiti fornisce un processo di anonimizzazione chiaro e documentabile per le verifiche normative.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Risorse di apprendimento" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentazione" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Codice sorgente" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Riferimenti API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Supporto prodotto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Supporto gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Supporto a pagamento" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Perché Aspose.Medical per .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Elenco clienti" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Storie di successo" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
