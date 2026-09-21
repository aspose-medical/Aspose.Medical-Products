---
title: Konversi JSON ke DICOM dalam C# .NET | Aspose.Medical
weight: 6000

description: Buat file DICOM dari Model JSON DICOM standar (PS3.18) dalam C# .NET. Baca JSON dari string, aliran, atau pipe, alirkan urutan dataset, dan selesaikan referensi bulk data dengan API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konversi JSON ke DICOM dalam .NET C#" h2="Baca Model JSON DICOM standar (PS3.18) kembali menjadi dataset dan file DICOM. Kerjakan dari string, aliran, atau pipe, alirkan urutan studi, dan selesaikan referensi bulk data." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Dari JSON DICOM ke file DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> membaca <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">Model JSON DICOM PS3.18</a>, representasi yang digunakan oleh layanan DICOMweb dan oleh sistem yang menukar studi melalui HTTP. Apa yang datang sebagai JSON menjadi <code>Dataset</code>, dan <code>Dataset</code> ditulis ke disk sebagai file DICOM.</p>

<p>Ini adalah arah berlawanan dari halaman <a href="/medical/net/dicom-to-json/">DICOM ke JSON</a>, dan keduanya menggunakan kelas yang sama, <code>DicomJsonSerializer</code>.</p>

<div class="codeblock" id="code">
 <h3>Buat file DICOM dari JSON - C#</h3>
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

<p>Dataset yang tidak membawa File Meta Information ditulis dengan sintaks transfer default, Implicit VR Little Endian, ketika dibungkus dalam <code>DicomFile</code>.</p>

<p>Membaca DICOM JSON adalah fitur berlisensi. Tanpa lisensi on-premise yang diterapkan, pembaca akan melempar <code>MedicalApiException</code>, jadi terapkan lisensi terlebih dahulu, seperti yang dijelaskan dalam <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">panduan lisensi</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pertahankan File Meta Information">}}

<p><code>Deserialize</code> mengembalikan hanya dataset saja. Ketika dokumen JSON juga membawa grup File Meta Information, misalnya karena dihasilkan dari file DICOM lengkap, <code>DeserializeFile</code> mengembalikan <code>DicomFile</code> dengan grup tersebut utuh, termasuk sintaks transfer yang dideklarasikan file.</p>

<div class="codeblock" id="code">
 <h3>Baca file DICOM lengkap dari JSON - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stream, pipe, dan async">}}

<p>Setiap titik masuk memiliki overload stream dan overload asynchronous, dan yang asynchronous juga menerima <code>PipeReader</code>. Dokumen yang datang dari respons web atau dari disk dibaca tanpa terlebih dahulu diubah menjadi string, yang penting ketika JSON membawa data piksel.</p>

<div class="codeblock" id="code">
 <h3>Baca JSON dari stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Urutan dataset, satu per satu">}}

<p>Query DICOMweb menjawab dengan array dataset, dan dokumen semacam itu dapat besar. <code>DeserializeList</code> membaca seluruh array ke memori; <code>DeserializeAsyncEnumerable</code> menghasilkan satu dataset per iterasi, sehingga dokumen tidak pernah dimuat seluruhnya.</p>

<div class="codeblock" id="code">
 <h3>Stream array dataset - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Referensi bulk data">}}

<p>Model JSON DICOM tidak membawa data piksel secara inline. Nilai besar digantikan oleh <code>BulkDataURI</code> yang mengarah ke byte, sehingga dokumen JSON tetap kecil. Untuk menyelesaikan referensi tersebut saat membaca, berikan serializer loader bulk data. <code>DefaultBulkDataLoader</code> mengambil URI <code>file</code>, <code>http</code>, dan <code>https</code> tanpa otentikasi; untuk arsip yang memerlukan kredensial, implementasikan <code>IBulkDataLoader</code> atau <code>IAsyncBulkDataLoader</code> sendiri.</p>

<div class="codeblock" id="code">
 <h3>Selesaikan BulkDataURI saat membaca - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Perjalanan bolak-balik dengan DICOM ke JSON">}}

<p>Kedua arah dimaksudkan untuk digunakan bersama: sebuah studi keluar sebagai JSON, melewati layanan web, dan kembali sebagai file DICOM. Tidak ada yang bergantung pada kode native, sehingga perjalanan bolak-balik yang sama berjalan di Windows, Linux, dan macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM ke JSON dan kembali - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Untuk opsi yang mengontrol tampilan JSON, lihat halaman <a href="/medical/net/dicom-to-json/">DICOM ke JSON</a>. Pasangan yang sama ada untuk XML: <a href="/medical/net/dicom-to-xml/">DICOM ke XML</a> dan <a href="/medical/net/xml-to-dicom/">XML ke DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">Panduan serialisasi JSON</a> mencakup seluruh API.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Sumber Belajar" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentasi" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Panduan Pengembang" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
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