---
title: Konwertuj XML do DICOM w C# .NET | Aspose.Medical
weight: 5000

description: Twórz pliki DICOM z natywnego modelu DICOM XML PS3.19 w C# .NET. Czytaj XML ze stringa, streama lub pipe'a, streamuj kolejne dokumenty i rozwiązuj odniesienia do danych masowych przy użyciu API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konwertuj XML do DICOM w .NET C#" h2="Odczytaj natywny model DICOM XML PS3.19 z powrotem do datasetów i plików DICOM. Pracuj ze stringiem, streamem lub pipe'em, streamuj kolejne dokumenty i rozwiązuj odniesienia do danych masowych." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standardowy natywny model DICOM XML">}}

<p><strong>Aspose.Medical for .NET</strong> odczytuje <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Natywny model DICOM</a> zdefiniowany w DICOM PS3.19. Jest to reprezentacja XML zapisana w samym standardzie, a nie format wymyślony przez Aspose, co czyni go przydatnym w integracji: system, który już wymienia DICOM jako XML, generuje dokumenty akceptowane przez tę bibliotekę.</p>

<p>Korzeń dokumentu to <code>NativeDicomModel</code>, a każdy atrybut jest elementem <code>DicomAttribute</code> zawierającym jego tag, reprezentację wartości oraz słowo kluczowe:</p>

<div class="codeblock" id="code">
 <h3>Format natywnego modelu DICOM</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>Ta strona jest odwrotą kierunku <a href="/medical/net/dicom-to-xml/">DICOM do XML</a>, a obie używają tej samej klasy, <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Utwórz plik DICOM z XML w C#">}}

<p><code>Deserialize</code> przekształca dokument w <code>Dataset</code>, a dataset jest zapisywany na dysku jako plik DICOM.</p>

<div class="codeblock" id="code">
 <h3>Utwórz plik DICOM z XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Natywny model DICOM nie posiada grupy File Meta Information, więc składnia transferu nie jest częścią dokumentu. Dataset opakowany w <code>DicomFile</code> jest zapisywany z domyślną składnią transferu, Implicit VR Little Endian. Aby zapisać plik w innej składni, przetranskoduj go, jak pokazuje strona <a href="/medical/net/dicom-transfer-syntax-conversion/">konwersji składni transferu</a>.</p>

<p>Odczyt DICOM XML jest funkcją objętą licencją. Bez zastosowanej licencji on-premise czytnik rzuca <code>MedicalApiException</code>, dlatego najpierw zastosuj licencję, zgodnie z <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">przewodnikiem licencyjnym</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Strumienie, potoki i asynchroniczność">}}

<p>Każdy punkt wejścia posiada przeciążenie stream oraz asynchroniczne przeciążenie, a wersje asynchroniczne akceptują także <code>PipeReader</code>. Dokument przychodzący z odpowiedzi sieciowej jest parsowany w trakcie odczytu, bez uprzedniego przekształcenia go w string.</p>

<div class="codeblock" id="code">
 <h3>Czytaj XML ze streamu - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kolejne dokumenty w jednym strumieniu">}}

<p>Eksport z innego systemu często zawiera po kolei element <code>NativeDicomModel</code> w jednym strumieniu. <code>DeserializeAsyncEnumerable</code> zwraca jeden dataset na każdy element, w kolejności wejścia, więc strumień jest przetwarzany bez konieczności przechowywania w pamięci. Elementy następują po sobie bezpośrednio: deklaracja XML jest dozwolona tylko na samym początku, jak w każdym wejściu XML.</p>

<div class="codeblock" id="code">
 <h3>Strumieniuj kolejne dokumenty - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Odniesienia do danych masowych">}}

<p>Duże wartości, takie jak dane pikseli, nie są zapisywane inline. Pojawiają się jako element <code>BulkData</code> z URI wskazującym na bajty, co utrzymuje dokument małym. Aby rozwiązać te odniesienia podczas odczytu, przekaż serializerowi loader danych masowych. <code>DefaultBulkDataLoader</code> pobiera URI <code>file</code>, <code>http</code> i <code>https</code> bez autoryzacji; w przypadku archiwum wymagającego poświadczeń, zaimplementuj własny <code>IBulkDataLoader</code> lub <code>IAsyncBulkDataLoader</code>.</p>

<div class="codeblock" id="code">
 <h3>Rozwiąż dane masowe podczas odczytu - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pętla zwrotna z DICOM do XML">}}

<p>Oba kierunki są przeznaczone do wspólnego użycia: badanie zostaje wyeksportowane jako XML, przechodzi przez system obsługujący XML i wraca jako plik DICOM. Wszystko jest zarządzane w .NET, więc ta sama pętla zwrotna działa na Windows, Linux i macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM do XML i z powrotem - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Opcje kontrolujące wygląd XML znajdziesz na stronie <a href="/medical/net/dicom-to-xml/">DICOM do XML</a>. Ten sam zestaw istnieje dla JSON: <a href="/medical/net/dicom-to-json/">DICOM do JSON</a> oraz <a href="/medical/net/json-to-dicom/">JSON do DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">Przewodnik serializacji</a> obejmuje całe API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Zasoby edukacyjne" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentacja" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Przewodnik dla programistów" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
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