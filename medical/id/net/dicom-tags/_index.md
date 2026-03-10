---
title: Baca & Modifikasi Tag DICOM di C# .NET | Aspose.Medical
weight: 15000
description: Baca, tambahkan, perbarui, dan hapus tag DICOM di C# .NET. Akses metadata tag, kerja dengan tag privat, enumerasi berdasarkan grup, dan manipulasi urutan menggunakan API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Baca & Modifikasi Tag DICOM di .NET C#" h2="Akses, tambahkan, perbarui, dan hapus elemen data DICOM dengan API yang tipe-aman. Kerja dengan tag standar, tag privat, urutan, dan kamus tag DICOM lengkap." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Akses Tag DICOM Tipe-Aman">}}

<p><strong>Aspose.Medical for .NET</strong> menyediakan API yang komprehensif dan tipe-aman untuk bekerja dengan tag DICOM melalui kelas <code>Dataset</code>. Setiap elemen data DICOM direpresentasikan oleh <code>Tag</code> &mdash; sepasang nomor grup dan elemen yang secara unik mengidentifikasi atribut. API ini menawarkan akses bertipe kuat dengan metode generik, pola pengambilan yang aman yang menghindari pengecualian, serta dukungan untuk nilai tunggal, array, dan akses berindeks.</p>

<p>Tag DICOM umum tersedia sebagai field statis pada kelas <code>Tag</code> (misalnya, <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), menghilangkan kebutuhan menghafal kode numerik tag. Setiap tag memiliki <code>TagMetadata</code> yang berisi kata kunci, deskripsi, Value Representation (VR), multiplicity nilai, dan status pensiun.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Baca Tag DICOM di C#">}}

<p>Kelas <code>Dataset</code> menyediakan berbagai metode untuk mengambil nilai tag sesuai kebutuhan Anda &mdash; nilai tunggal, semua nilai, akses berindeks, atau pengambilan aman dengan nilai default:</p>

<div class="codeblock" id="code">
 <h3>Baca nilai tag DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Tambah dan Perbarui Tag DICOM">}}

<p>Gunakan <code>AddOrUpdate</code> untuk menetapkan nilai tag. Metode ini membuat elemen jika tag belum ada, atau mengganti nilainya jika sudah ada. Anda dapat menetapkan nilai tunggal, array, atau menambahkan elemen bertipe penuh:</p>

<div class="codeblock" id="code">
 <h3>Tambah dan perbarui tag DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Hapus Tag DICOM">}}

<p>Metode <code>Remove</code> mendukung penghapusan satu tag, beberapa tag sekaligus, atau tag yang cocok dengan predikat &mdash; berguna untuk menghapus seluruh grup elemen:</p>

<div class="codeblock" id="code">
 <h3>Hapus tag DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Enumerasi Tag berdasarkan Grup">}}

<p>Tag DICOM diatur dalam grup &mdash; misalnya, grup <code>0x0010</code> berisi informasi pasien, grup <code>0x0008</code> berisi identifikasi studi, dan grup <code>0x0028</code> berisi atribut pixel citra. Gunakan <code>EnumerateGroup</code> untuk mengiterasi semua elemen dalam grup tertentu:</p>

<div class="codeblock" id="code">
 <h3>Enumerasi elemen berdasarkan grup - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Manipulasi Data Tingkat Elemen">}}

<p>Setiap elemen DICOM menyediakan metode untuk membaca, menambah, menyisipkan, menghapus, dan mengganti nilai individu di dalam elemen. Ini berguna untuk tag multi-nilai di mana Anda memerlukan kontrol detail atas daftar nilai:</p>

<div class="codeblock" id="code">
 <h3>Manipulasi nilai elemen - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Kamus Tag dan Metadata">}}

<p>Kelas <code>TagDictionary</code> menyediakan registri tag DICOM yang dikenal beserta metadatanya &mdash; kata kunci, deskripsi, Value Representation, multiplicity nilai, dan status pensiun. Anda dapat mencari tag berdasarkan kata kunci atau memeriksa metadata mereka secara programatik:</p>

<div class="codeblock" id="code">
 <h3>Query kamus tag - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Dukungan Tag Privat">}}

<p>DICOM memungkinkan vendor dan organisasi mendefinisikan tag privat untuk data kepemilikan. Aspose.Medical untuk .NET sepenuhnya mendukung pembacaan, penulisan, dan pendaftaran tag privat. Anda dapat memuat kamus tag khusus dari file XML untuk memungkinkan penanganan yang tepat atas elemen privat:</p>

<div class="codeblock" id="code">
 <h3>Kerja dengan tag privat - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Kerja dengan Urutan DICOM">}}

<p>Urutan DICOM (tipe VR SQ) berisi dataset bersarang, membentuk struktur pohon dalam file DICOM. Kelas <code>Sequence</code> menyediakan akses bertipe ke item urutan:</p>

<div class="codeblock" id="code">
 <h3>Baca dan buat urutan DICOM - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Grup Tag DICOM Umum">}}

<p>Tabel berikut mencantumkan grup tag DICOM yang sering digunakan serta contoh tag yang dapat diakses melalui kelas <code>Tag</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Grup</th>
<th>Kategori</th>
<th>Contoh Tag</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>Informasi Meta Berkas</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>Identifikasi Studi</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>Informasi Pasien</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>Parameter Akuisisi</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>Posisi Gambar</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>Atribut Pixel Gambar</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>Data Pixel</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Sumber Belajar" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentasi" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Kode Sumber" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referensi API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Dukungan Produk" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Dukungan Gratis" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Dukungan Berbayar" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Mengapa Aspose.Medical untuk .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Daftar Pelanggan" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Cerita Sukses" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
