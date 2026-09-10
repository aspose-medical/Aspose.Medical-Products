---
title: Ubah DICOM ke XML dalam C# .NET | Aspose.Medical
weight: 3000
description: Serialisasikan dataset DICOM ke format DICOM XML standar dalam C# .NET. Konfigurasikan penanganan bulk data, pemrosesan berbasis aliran, dan operasi async dengan API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Ubah DICOM ke XML dalam .NET C#" h2="Serialisasikan dataset DICOM ke representasi DICOM XML standar (PS3.19). Konfigurasikan referensi bulk data, output berbasis aliran, dan pemrosesan async dengan pustaka .NET murni." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Serialisasi DICOM XML Berbasis Standar">}}

<p><strong>Aspose.Medical for .NET</strong> menserialisasikan data DICOM ke XML mengikuti <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Model DICOM PS3.19 Native DICOM</a>. Ini adalah standar resmi untuk merepresentasikan dataset DICOM dalam XML, digunakan oleh layanan DICOMweb, platform integrasi, dan sistem yang memerlukan representasi metadata pencitraan medis yang dapat dibaca manusia dan tervalidasi skema.</p>

<p>Kelas <code>DicomXmlSerializer</code> menyediakan metode statis untuk serialisasi maupun deserialisasi. Tidak seperti pendekatan dump tag sederhana, output mematuhi skema DICOM XML di mana setiap elemen direpresentasikan dengan tag, VR, dan nilai yang diformat dengan tepat &mdash; memungkinkan konversi bolak‑balik lossless antara DICOM biner dan XML.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialisasikan DICOM ke XML dalam C#">}}

<p>Gunakan kelas <code>DicomXmlSerializer</code> untuk mengonversi dataset DICOM menjadi string XML. Pendekatan paling sederhana menghasilkan dokumen XML yang mematuhi standar:</p>

<div class="codeblock" id="code">
 <h3>Ubah DICOM ke XML - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialisasi Berbasis Aliran dan Async">}}

<p>Untuk file DICOM besar atau skenario throughput tinggi, serialisasikan langsung ke aliran untuk menghindari alokasi string besar di memori. Baik metode sinkron maupun async tersedia:</p>

<div class="codeblock" id="code">
 <h3>Serialisasi aliran sinkron - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Serialisasi aliran async - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streaming Pipeline untuk Studi Besar">}}

<p>Seluruh studi tidak perlu disimpan dalam memori. <code>DicomXmlSerializer</code> menulis ke <code>PipeWriter</code> dan membaca dari <code>PipeReader</code>, sehingga XML dapat dihasilkan dan dikonsumsi selama aliran, dan urutan dataset dapat dibaca satu per satu melalui <code>DeserializeAsyncEnumerable</code>. Setiap metode menerima <code>CancellationToken</code>.</p>

<div class="codeblock" id="code">
 <h3>Serialisasi dan deserialisasi melalui pipe - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Baca urutan dataset satu per satu - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opsi Serialisasi">}}

<p>Kelas <code>DicomXmlSerializerOptions</code> mengontrol bagaimana data DICOM direpresentasikan dalam XML. Konfigurasi utama melibatkan penanganan bulk data untuk nilai biner besar:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Properti</th>
<th>Tipe</th>
<th>Deskripsi</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Konverter khusus untuk menulis data besar (mis., data piksel) sebagai referensi URI BulkData alih-alih di-inline</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Loader khusus untuk menyelesaikan URI BulkData selama deserialisasi</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>Instansi opsi default yang digunakan ketika tidak ada opsi khusus yang disediakan</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Serialisasi dengan opsi khusus - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Penanganan Bulk Data">}}

<p>Nilai biner besar (data piksel, gelombang, dokumen terenkapsulasi) dapat dieksternalisasi sebagai referensi URI BulkData alih‑alih di‑inline dalam output XML. Ini mengikuti spesifikasi <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">elemen BulkData DICOM PS3.19</a>.</p>

<p>Implementasikan <code>IBulkDataConverter</code> untuk mengeksternalisasi data besar selama serialisasi, dan <code>IBulkDataLoader</code> untuk menyelesaikan URI selama deserialisasi. Untuk kasus umum tidak perlu menulis loader sama sekali: <code>DefaultBulkDataLoader.Instance</code> menyelesaikan URI <code>file</code>, <code>http</code>, dan <code>https</code>, serta mengimplementasikan <code>IAsyncBulkDataLoader</code>, sehingga bulk data diambil secara async pada jalur streaming.</p>

<div class="codeblock" id="code">
 <h3>Penanganan bulk data khusus - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
public class FileBulkDataConverter : IBulkDataConverter
{
    public string? GetBulkDataUri(IElement element)
    {
        // Externalize pixel data to a separate file
        if (element.Tag == Tag.PixelData)
            return "file:///bulk/pixeldata.raw";
        return null; // inline all other elements
    }
}

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Deserialisasi XML ke DICOM">}}

<p>Parse XML DICOM kembali menjadi objek Dataset. Mendukung input string, input aliran, dan operasi async:</p>

<div class="codeblock" id="code">
 <h3>Deserialisasi XML ke DICOM - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialisasi XML vs JSON">}}

<p>Aspose.Medical mendukung serialisasi DICOM XML (PS3.19) dan DICOM JSON (PS3.18). Kedua format menawarkan konversi round‑trip lossless, namun melayani skenario integrasi yang berbeda:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Fitur</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>Standar</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>Validasi skema</td><td>XML Schema (XSD) tersedia</td><td>Tidak ada skema formal</td></tr>
<tr><td>Paling cocok untuk</td><td>Enterprise integration, HL7 CDA, audit logs, XDS registries</td><td>DICOMweb, REST APIs, FHIR ImagingStudy</td></tr>
<tr><td>Keterbacaan manusia</td><td>Verbose namun self‑describing</td><td>Ringkas dan didukung luas</td></tr>
<tr><td>Data bulk</td><td>BulkData element with URI</td><td>Properti BulkDataURI</td></tr>
<tr><td>Kelas serializer</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
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
