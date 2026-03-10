---
title: Konversi DICOM ke JSON dalam C# .NET | Aspose.Medical
weight: 4000
description: Serialisasi file DICOM ke format JSON DICOM standar dalam C# .NET. Konfigurasikan penanganan angka, URI data bulk, output kata kunci, dan pemrosesan aliran async dengan API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konversi DICOM ke JSON dalam .NET C#" h2="Serialisasi dataset dan file DICOM ke Model JSON DICOM standar (PS3.18). Konfigurasikan penanganan angka, referensi data bulk, output kata kunci, dan pemrosesan async berbasis aliran." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Model JSON DICOM Standar">}}

<p><strong>Aspose.Medical untuk .NET</strong> menserialisasi data DICOM ke JSON sesuai <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">Model JSON DICOM PS3.18</a>. Ini adalah standar resmi untuk merepresentasikan dataset DICOM dalam JSON, digunakan oleh layanan DICOMweb, sumber daya FHIR ImagingStudy, dan API perawatan kesehatan modern.</p>

<p>Dalam Model JSON DICOM, setiap atribut direpresentasikan oleh tagnya sebagai kunci JSON (misalnya <code>"00100010"</code> untuk Patient Name), dengan Value Representation dan array nilai sebagai properti bersarang:</p>

<div class="codeblock" id="code">
 <h3>Format Model JSON DICOM</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialisasi DICOM ke JSON dalam C#">}}

<p>Gunakan kelas <code>DicomJsonSerializer</code> untuk mengonversi file atau dataset DICOM ke JSON. Pendekatan paling sederhana menghasilkan output yang kompak dan sesuai standar:</p>

<div class="codeblock" id="code">
 <h3>Konversi DICOM ke JSON - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p>Anda dapat menserialisasi <code>Dataset</code>, sebuah <code>DicomFile</code> lengkap (termasuk File Meta Information), atau sebuah array dataset:</p>

<div class="codeblock" id="code">
 <h3>Serialisasi DicomFile dan array dataset - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Serialisasi Berbasis Aliran dan Async">}}

<p>Untuk file DICOM besar atau skenario throughput tinggi, serialisasikan langsung ke aliran UTF-8 untuk menghindari alokasi string besar di memori. Baik metode sinkron maupun async tersedia:</p>

<div class="codeblock" id="code">
 <h3>Serialisasi aliran dan async - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opsi Serialisasi">}}

<p>Kelas <code>DicomJsonSerializerOptions</code> mengontrol bagaimana data DICOM direpresentasikan dalam JSON:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Properti</th>
<th>Tipe</th>
<th>Deskripsi</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>Mengontrol apakah VR numerik (IS, DS, SV, UV) ditulis sebagai angka JSON atau string</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>Gunakan kata kunci DICOM (mis., <code>\"PatientName\"</code>) alih-alih tag (<code>\"00100010\"</code>) sebagai kunci JSON. Catatan: non-standar</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>Sertakan kata kunci DICOM sebagai atribut JSON tambahan</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>Sertakan nama tag DICOM sebagai atribut JSON tambahan</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>Konverter khusus untuk menulis data besar (mis., data piksel) sebagai referensi URI</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>Loader khusus untuk menyelesaikan URI data bulk selama deserialisasi</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Konfigurasikan opsi serialisasi - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Penanganan Angka">}}

<p>Value Representation numerik DICOM (IS, DS, SV, UV) dapat diserialisasi sebagai angka JSON atau string JSON. Ini penting untuk interoperabilitas, karena beberapa implementasi DICOM menyimpan angka dengan spasi di depan atau format non-standar:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Mode</th>
<th>Output JSON</th>
<th>Kasus Penggunaan</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>\"00180086\":{\"vr\":\"IS\",\"Value\":[42]}</code></td><td>Kepatuhan standar, ideal untuk komputasi. Mengeluarkan pengecualian jika nilai tidak dapat diparsing.</td></tr>
<tr><td><code>AsString</code></td><td><code>\"00180086\":{\"vr\":\"IS\",\"Value\":[\"42\"]}</code></td><td>Mempertahankan representasi string asli. Lebih aman untuk round‑tripping data non‑standar.</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Penanganan Data Bulk">}}

<p>Nilai biner besar (data piksel, gelombang, dokumen terenkapsulasi) dapat ditangani sebagai referensi data bulk alih-alih di‑inline dalam output JSON. Ini mengikuti spesifikasi <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a>.</p>

<p>Implementasikan <code>IBulkDataConverter</code> untuk mengeksternalisasi data besar selama serialisasi, dan <code>IBulkDataLoader</code> untuk menyelesaikan URI selama deserialisasi:</p>

<div class="codeblock" id="code">
 <h3>Penanganan data bulk kustom - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Deserialisasi JSON ke DICOM">}}

<p>Parse JSON DICOM kembali menjadi objek Dataset atau DicomFile. Mendukung input string, input aliran, dan operasi aliran async:</p>

<div class="codeblock" id="code">
 <h3>Deserialisasi JSON ke DICOM - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Integrasi System.Text.Json">}}

<p>Aspose.Medical menyediakan <code>DatasetJsonConverter</code> dan <code>DicomFileJsonConverter</code> untuk integrasi dengan pipeline serialisasi standar <code>System.Text.Json</code>. Ini memungkinkan objek DICOM diserialisasi bersamaan dengan tipe .NET lainnya dalam kode pemrosesan JSON Anda yang sudah ada:</p>

<div class="codeblock" id="code">
 <h3>Integrasi System.Text.Json - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
</div>

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
{{< blocks/products/pf/slr-element name="Kisah Sukses" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
