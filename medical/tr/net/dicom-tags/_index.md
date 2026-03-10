---
title: C# .NET'te DICOM Etiketlerini Okuma ve Değiştirme | Aspose.Medical
weight: 15000
description: C# .NET'te DICOM etiketlerini okuyun, ekleyin, güncelleyin ve kaldırın. Etiket meta verilerine erişin, özel etiketlerle çalışın, gruba göre listeleyin ve Aspose.Medical API'sını kullanarak sıralamaları (sequences) manipüle edin.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'ta DICOM Etiketlerini Okuma ve Değiştirme" h2="Tip güvenli bir API ile DICOM veri elemanlarına erişin, ekleyin, güncelleyin ve kaldırın. Standart etiketler, özel etiketler, sıralamalar ve tam DICOM etiket sözlüğüyle çalışın." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Tip Güvenli DICOM Etiket Erişimi">}}

<p><strong>Aspose.Medical for .NET</strong> DICOM etiketleriyle <code>Dataset</code> sınıfı üzerinden çalışmak için kapsamlı, tip-güvenli bir API sağlar. Her DICOM veri elemanı bir <code>Tag</code> &mdash; özniteliği benzersiz şekilde tanımlayan grup ve eleman numaralarının bir çifti — olarak temsil edilir. API, jenerik metodlarla güçlü tipli erişim, istisna oluşturmayan güvenli alma kalıpları ve tek değerler, diziler ve indeksli erişim desteği sunar.</p>

<p>Yaygın DICOM etiketleri, <code>Tag</code> sınıfındaki statik alanlar olarak (örnek: <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>) bulunur; sayısal etiket kodlarını ezberleme gereksinimini ortadan kaldırır. Her etiket, anahtar kelimesi, açıklaması, Değer Temsil (VR), değer çokluğu ve kullanım dışı bırakılma durumu içeren bir <code>TagMetadata</code> taşır.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#'ta DICOM Etiketlerini Okuma">}}

<p><code>Dataset</code> sınıfı, ihtiyaçlarınıza göre etiket değerlerini almanın çeşitli yöntemlerini sunar — tek değer, tüm değerler, indeksli erişim veya varsayılanlarla güvenli alma:</p>

<div class="codeblock" id="code">
 <h3>DICOM etiket değerlerini okuma - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Get a single value (throws if tag is missing or multi-valued)
string patientName = dataset.GetSingleValue&lt;string&gt;(Tag.PatientName);

// Safe retrieval with a default value
string institution = dataset.GetSingleValueOrDefault&lt;string&gt;(
    Tag.InstitutionName, "Unknown");

// Get all values from a multi-valued tag
double[] pixelSpacing = dataset.GetValues&lt;double&gt;(Tag.PixelSpacing);

// Get a value by index (supports negative indexing with ^)
double firstPosition = dataset.GetValue&lt;double&gt;(Tag.ImagePositionPatient, 0);

// Check if a tag exists before accessing it
if (dataset.Contains(Tag.PatientBirthDate))
{
    var birthDate = dataset.GetSingleValue&lt;DateOnly&gt;(Tag.PatientBirthDate);
}

// TryGet pattern for safe access without exceptions
if (dataset.TryGetSingleValue&lt;string&gt;(Tag.PatientID, out var patientId))
{
    // Use patientId
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM Etiketlerini Ekle ve Güncelle">}}

<p>Etiket değerlerini ayarlamak için <code>AddOrUpdate</code> kullanın. Etiket eksikse öğeyi oluşturur, mevcutsa değerini değiştirir. Tek değerler, diziler veya tam tipli öğeler ekleyebilirsiniz:</p>

<div class="codeblock" id="code">
 <h3>DICOM etiketlerini ekle ve güncelle - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Set a single value
dataset.AddOrUpdate(Tag.PatientName, "John Doe");
dataset.AddOrUpdate(Tag.PatientAge, new Age { Number = 42, Units = Age.Unit.Years });

// Set multiple values on a multi-valued tag
dataset.AddOrUpdate&lt;double&gt;(Tag.PixelSpacing, [0.5, 0.5]);

// Add multiple elements at once using typed element classes
dataset.AddOrUpdateRange(new IElement[]
{
    new PersonName(Tag.PatientName, ["John Doe"]),
    new LongString(Tag.PatientID, ["64AD6A3F"]),
    new Date(Tag.PatientBirthDate, [new DateOnly(2002, 10, 10)])
});

// Save the modified file
dicomFile.Save("updated.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM Etiketlerini Kaldır">}}

<p><code>Remove</code> metodu, tek bir etiketi, birden fazla etiketi aynı anda veya bir koşula uyan etiketleri kaldırmayı destekler — tüm grup öğelerini temizlemek için kullanışlıdır:</p>

<div class="codeblock" id="code">
 <h3>DICOM etiketlerini kaldır - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Remove a single tag
dataset.Remove(Tag.PatientAddress);

// Remove multiple tags at once
dataset.Remove(Tag.PatientName, Tag.PatientID, Tag.PatientBirthDate);

// Remove all tags in a specific group (e.g., group 0x0002 - File Meta Information)
dataset.Remove(element => element.Tag.Group == 0x0002);

dicomFile.Save("cleaned.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Etiketleri Gruplara Göre Listele">}}

<p>DICOM etiketleri gruplar halinde düzenlenir — örneğin, <code>0x0010</code> grubu hasta bilgilerini, <code>0x0008</code> grubu çalışma tanımlamasını ve <code>0x0028</code> grubu görüntü piksel özelliklerini içerir. Belirli bir gruptaki tüm öğeleri döngülemek için <code>EnumerateGroup</code> kullanın:</p>

<div class="codeblock" id="code">
 <h3>Ögeleri grup bazında listele - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Enumerate all patient-related tags (group 0x0010)
foreach (IElement element in dataset.EnumerateGroup(0x0010))
{
    Tag tag = element.Tag;
    string keyword = tag.Metadata.Keyword;
    string vr = element.ValueRepresentation.ToString();

    Console.WriteLine($"({tag.Group:X4},{tag.Element:X4}) {keyword} [{vr}]");
}

// Iterate over all elements in the dataset
foreach (IElement element in dataset)
{
    // Process each element
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Öğe Düzeyinde Veri Manipülasyonu">}}

<p>Her DICOM öğesi, öğe içinde bireysel değerleri okuma, ekleme, ekleme (insert), kaldırma ve değiştirme metodları sunar. Bu, değer listesi üzerinde ince ayar kontrolü gerektiren çoklu değerli etiketler için faydalıdır:</p>

<div class="codeblock" id="code">
 <h3>Öğe değerlerini manipüle et - C#</h3>
 <pre><code class="cs">// Create an element with multiple values
var element = new FloatingPointDouble(
    Tag.TableOfYBreakPoints, [50.1, 100.2, 150.3]);

// Access values by index (supports ^ for indexing from end)
double first = element.Get&lt;double&gt;(0);
double last = element.Get&lt;double&gt;(^1);

// Safe access with default
double safe = element.GetOrDefault&lt;double&gt;(10); // returns 0.0 if out of range

// Get all values or a range
double[] all = element.GetValues&lt;double&gt;();
double[] subset = element.GetValues&lt;double&gt;(1..3);

// Modify element values
element.Add(200.4);
element.AddRange([300.5, 400.6]);
element.Insert(2, 75.0);
element.RemoveAt(0);
element.Replace([1.0, 2.0, 3.0]);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Etiket Sözlüğü ve Meta Verileri">}}

<p><code>TagDictionary</code> sınıfı, bilinen DICOM etiketlerinin ve bunların meta verilerinin (anahtar kelime, açıklama, Değer Temsil, değer çokluğu ve kullanım dışı bırakılma durumu) bir kaydını sağlar. Etiketleri anahtar kelimeye göre arayabilir veya meta verilerini programlı olarak inceleyebilirsiniz:</p>

<div class="codeblock" id="code">
 <h3>Etiket sözlüğünü sorgula - C#</h3>
 <pre><code class="cs">// Look up a tag by its keyword
Tag? tag = TagDictionary.Default.FindByKeyword("PatientName");

// Access tag metadata from the dictionary
TagMetadata meta = TagDictionary.Default[Tag.PatientName];
Console.WriteLine($"Keyword: {meta.Keyword}");
Console.WriteLine($"Name: {meta.Name}");
Console.WriteLine($"VR: {string.Join("/", meta.ValueRepresentations)}");
Console.WriteLine($"VM: {meta.Multiplicity}");
Console.WriteLine($"Retired: {meta.Retired}");

// Access metadata directly from a tag instance
TagMetadata info = Tag.Rows.Metadata;

// Parse a tag from its string representation
Tag parsed = Tag.Parse("0010,0010");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Özel Etiket Desteği">}}

<p>DICOM, satıcıların ve organizasyonların özel veriler için özel etiketler tanımlamasına izin verir. Aspose.Medical for .NET, özel etiketlerin okunması, yazılması ve kaydedilmesini tam olarak destekler. Özel öğelerin doğru işlenmesini sağlamak için XML dosyalarından özel etiket sözlükleri yükleyebilirsiniz:</p>

<div class="codeblock" id="code">
 <h3>Özel etiketlerle çalış - C#</h3>
 <pre><code class="cs">// Load a custom private tag dictionary from XML
// XML format:
// &lt;dictionaries&gt;
//   &lt;dictionary creator="MY ORGANIZATION"&gt;
//     &lt;tag group="3011" element="xx00" vr="SL" vm="1"&gt;Custom Value&lt;/tag&gt;
//   &lt;/dictionary&gt;
// &lt;/dictionaries&gt;
TagDictionary.Default.Load("custom-tag-dictionary.xml");

// Check if a tag is private
Tag tag = Tag.GetOrCreate(0x3011, 0x0010);
bool isPrivate = tag.IsPrivate;

// Get a valid private tag (creates Private Creator element if needed)
Dataset dataset = dicomFile.Dataset;
Tag privateTag = dataset.GetPrivateTag(tag);

// Register a private creator programmatically
PrivateCreator creator = new("MY ORGANIZATION");
TagDictionary privateDictionary =
    TagDictionary.Default.GetPrivateDictionary(creator);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM Sıralamalarıyla (Sequences) Çalışma">}}

<p>DICOM sıralamaları (VR tipi SQ), DICOM dosyasında ağaç yapısı oluşturan iç içe veri kümeleri içerir. <code>Sequence</code> sınıfı, sıralama öğelerine tipli erişim sağlar:</p>

<div class="codeblock" id="code">
 <h3>DICOM sıralamaları okuyun ve oluşturun - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Read a sequence element
IElement seqElement = dataset.GetOrDefault(Tag.ReferencedStudySequence);
if (seqElement is Sequence sequence)
{
    // Iterate over sequence items (each item is a Dataset)
    foreach (Dataset item in sequence.Items)
    {
        string uid = item.GetSingleValueOrDefault&lt;string&gt;(
            Tag.ReferencedSOPInstanceUID, "");
    }

    // Access an item by index
    Dataset firstItem = sequence.Get(0);
}

// Create a new sequence
var items = new List&lt;Dataset&gt;
{
    new(new IElement[]
    {
        new UniqueIdentifier(Tag.ReferencedSOPClassUID, ["1.2.840.10008.5.1.4.1.1.2"]),
        new UniqueIdentifier(Tag.ReferencedSOPInstanceUID, ["1.2.3.4.5.6.7.8.9"])
    })
};
dataset.Add(new Sequence(Tag.ReferencedStudySequence, items));</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Yaygın DICOM Etiket Grupları">}}

<p>Aşağıdaki tablo, sık kullanılan DICOM etiket gruplarını ve <code>Tag</code> sınıfı üzerinden erişilebilen örnek etiketleri listeler:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Grup</th>
<th>Kategori</th>
<th>Örnek Etiketler</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Dosya Meta Bilgisi</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Çalışma Tanımlaması</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Hasta Bilgileri</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Edinim Parametreleri</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Görüntü Konumlandırma</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Görüntü Piksel Özellikleri</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Piksel Verisi</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokümantasyon" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Kaynak Kodu" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API Referansları" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Ürün Desteği" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ücretsiz Destek" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Ücretli Destek" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Neden Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Müşteri Listesi" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Başarı Hikayeleri" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
