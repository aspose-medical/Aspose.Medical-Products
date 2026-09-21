---
title: Konwertuj JSON do DICOM w C# .NET | Aspose.Medical
weight: 6000

description: Twórz pliki DICOM z standardowego modelu DICOM JSON (PS3.18) w C# .NET. Czytaj JSON ze łańcucha znaków, strumienia lub rury, strumieniuj sekwencję zestawów danych i rozwiązuj odwołania do danych zbiorczych przy użyciu API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konwertuj JSON do DICOM w .NET C#" h2="Odczytaj standardowy model DICOM JSON (PS3.18) z powrotem do zestawów danych i plików DICOM. Pracuj ze łańcuchem znaków, strumieniem lub rurą, strumieniuj sekwencję badań i rozwiązuj odwołania do danych zbiorczych." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Z DICOM JSON do pliku DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> odczytuje <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">Model DICOM PS3.18 JSON</a>, reprezentację używaną przez usługi DICOMweb oraz systemy wymieniające badania przez HTTP. To, co przychodzi jako JSON, staje się <code>Dataset</code>, a <code>Dataset</code> jest zapisywany na dysku jako plik DICOM.</p>

<p>Jest to odwrócony kierunek względem strony <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>, a obie używają tej samej klasy, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Utwórz plik DICOM z JSON - C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Zestaw danych, który nie zawiera File Meta Information, jest zapisywany z domyślną składnią transferu, Implicit VR Little Endian, gdy jest opakowany w <code>DicomFile</code>.</p>

<p>Odczyt DICOM JSON jest funkcją wymagającą licencji. Bez zastosowanej licencji lokalnej czytnik zgłasza <code>MedicalApiException</code>, więc najpierw zastosuj licencję, zgodnie z opisem w <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">przewodniku licencyjnym</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Zachowaj File Meta Information">}}

<p><code>Deserialize</code> zwraca tylko zestaw danych. Gdy dokument JSON zawiera także grupę File Meta Information, na przykład ponieważ został wygenerowany z pełnego pliku DICOM, <code>DeserializeFile</code> zwraca <code>DicomFile</code> z tą grupą nienaruszoną, łącznie ze składnią transferu zadeklarowaną w pliku.</p>

<div class="codeblock" id="code">
 <h3>Odczytaj pełny plik DICOM z JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Strumienie, rury i async">}}

<p>Każdy punkt wejścia ma przeciążenie przyjmujące strumień oraz wersję asynchroniczną, a te asynchroniczne akceptują także <code>PipeReader</code>. Dokument przychodzący z odpowiedzi sieciowej lub z dysku jest czytany bez uprzedniego konwertowania na łańcuch znaków, co ma znaczenie, gdy JSON zawiera dane pikseli.</p>

<div class="codeblock" id="code">
 <h3>Czytaj JSON ze strumienia - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sekwencja zestawów danych, po jednym">}}

<p>Zapytanie DICOMweb zwraca tablicę zestawów danych, a taki dokument może być duży. <code>DeserializeList</code> odczytuje całą tablicę do pamięci; <code>DeserializeAsyncEnumerable</code> zwraca po jednym zestawie danych, więc dokument nigdy nie jest przechowywany w całości.</p>

<div class="codeblock" id="code">
 <h3>Strumieniuj tablicę zestawów danych - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Odwołania do danych zbiorczych">}}

<p>Model DICOM JSON nie zawiera danych pikseli w treści. Duże wartości są zastępowane <code>BulkDataURI</code>, które wskazuje na bajty, co utrzymuje dokument JSON małym. Aby rozwiązać te odwołania podczas odczytu, przekaż serializerowi loader danych zbiorczych. <code>DefaultBulkDataLoader</code> pobiera URI <code>file</code>, <code>http</code> i <code>https</code> bez uwierzytelniania; w przypadku archiwum wymagającego poświadczeń, zaimplementuj własny <code>IBulkDataLoader</code> lub <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Rozwiąż BulkDataURI podczas odczytu - C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pełny cykl DICOM do JSON">}}

<p>Oba kierunki są przeznaczone do wspólnego użycia: badanie jest wyeksportowane jako JSON, przemieszcza się przez usługę sieciową i wraca jako plik DICOM. Nic w tym procesie nie zależy od kodu natywnego, więc ten sam cykl działa na Windows, Linux i macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM do JSON i z powrotem - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Opcje kontrolujące wygląd JSON znajdziesz na stronie <a href="/medical/net/dicom-to-json/">DICOM to JSON</a>. Ten sam zestaw istnieje dla XML: <a href="/medical/net/dicom-to-xml/">DICOM to XML</a> oraz <a href="/medical/net/xml-to-dicom/">XML to DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">Przewodnik serializacji JSON</a> obejmuje całe API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zasoby edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Przewodnik dewelopera" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
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