---
title: Ubah XML menjadi DICOM di C# .NET | Aspose.Medical
weight: 5000

description: Buat file DICOM dari Native DICOM Model XML PS3.19 dalam C# .NET. Baca XML dari string, aliran (stream) atau pipe, alirkan dokumen berurutan, dan selesaikan referensi bulk data dengan API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Ubah XML menjadi DICOM di .NET C#" h2="Baca Native DICOM Model XML PS3.19 kembali menjadi dataset dan file DICOM. Kerjakan dari string, aliran (stream) atau pipe, alirkan dokumen berurutan, dan selesaikan referensi bulk data." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="XML Native DICOM Model Standar">}}

<p><strong>Aspose.Medical for .NET</strong> membaca <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a> yang didefinisikan dalam DICOM PS3.19. Ini adalah representasi XML yang ditulis ke dalam standar itu sendiri, bukan format yang diciptakan oleh Aspose, yang menjadikannya berguna untuk integrasi: sistem yang sudah menukar DICOM sebagai XML menghasilkan dokumen yang dapat diterima perpustakaan ini.</p>

<p>Akar dokumen adalah <code>NativeDicomModel</code>, dan setiap atribut adalah elemen <code>DicomAttribute</code> yang memuat tag, representasi nilai, dan kata kunci:</p>

<div class="codeblock" id="code">
 <h3>Format Native DICOM Model</h3>
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

<p>Halaman ini merupakan arah kebalikan dari <a href="/medical/net/dicom-to-xml/">DICOM ke XML</a>, dan keduanya menggunakan kelas yang sama, <code>DicomXmlSerializer</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Buat file DICOM dari XML di C#">}}

<p><code>Deserialize</code> mengubah dokumen menjadi <code>Dataset</code>, dan dataset ditulis ke disk sebagai file DICOM.</p>

<div class="codeblock" id="code">
 <h3>Buat file DICOM dari XML - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Native DICOM Model tidak memiliki grup File Meta Information, sehingga transfer syntax tidak termasuk dalam dokumen. Sebuah dataset yang dibungkus dalam <code>DicomFile</code> ditulis dengan transfer syntax default, Implicit VR Little Endian. Untuk menyimpan file dengan transfer syntax lain, lakukan transcoding, seperti yang ditunjukkan pada halaman <a href="/medical/net/dicom-transfer-syntax-conversion/">konversi transfer syntax</a>.</p>

<p>Membaca DICOM XML merupakan fitur berlisensi. Tanpa lisensi on-premise yang diterapkan, pembaca akan melempar <code>MedicalApiException</code>, jadi terapkan lisensi terlebih dahulu, sebagaimana dijelaskan dalam <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">panduan lisensi</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Stream, pipe, dan async">}}

<p>Setiap titik masuk memiliki overload stream dan overload asynchronous, dan yang asynchronous juga menerima <code>PipeReader</code>. Dokumen yang datang dari respons web diparse saat dibaca, tanpa harus diubah menjadi string terlebih dahulu.</p>

<div class="codeblock" id="code">
 <h3>Baca XML dari stream - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dokumen berurutan dalam satu stream">}}

<p>Ekspor dari sistem lain sering berisi satu elemen <code>NativeDicomModel</code> setelah yang lain dalam satu stream. <code>DeserializeAsyncEnumerable</code> menghasilkan satu dataset per elemen, sesuai urutan masukan, sehingga stream diproses tanpa harus dimuat seluruhnya ke memori. Elemen-elemen tersebut berurutan langsung: deklarasi XML hanya diizinkan di awal, sebagaimana pada masukan XML manapun.</p>

<div class="codeblock" id="code">
 <h3>Stream dokumen berurutan - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Referensi bulk data">}}

<p>Nilai besar seperti data piksel tidak ditulis secara inline. Mereka muncul sebagai elemen <code>BulkData</code> dengan URI yang menunjuk ke byte, sehingga dokumen tetap kecil. Untuk menyelesaikan referensi tersebut saat membaca, berikan serializer loader bulk data. <code>DefaultBulkDataLoader</code> mengambil URI <code>file</code>, <code>http</code>, dan <code>https</code> tanpa otentikasi; untuk arsip yang memerlukan kredensial, implementasikan <code>IBulkDataLoader</code> atau <code>IAsyncBulkDataLoader</code> sendiri.</p>

<div class="codeblock" id="code">
 <h3>Selesaikan bulk data saat membaca - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Siklus bolak-balik dengan DICOM ke XML">}}

<p>Dua arah tersebut dimaksudkan untuk digunakan bersama: sebuah study diekspor sebagai XML, melewati sistem yang berkomunikasi dengan XML, dan kembali sebagai file DICOM. Semua dikelola oleh .NET, sehingga siklus bolak-balik yang sama dapat dijalankan di Windows, Linux, dan macOS.</p>

<div class="codeblock" id="code">
 <h3>DICOM ke XML dan kembali - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>Untuk opsi yang mengatur tampilan XML, lihat halaman <a href="/medical/net/dicom-to-xml/">DICOM ke XML</a>. Pasangan yang sama tersedia untuk JSON: <a href="/medical/net/dicom-to-json/">DICOM ke JSON</a> dan <a href="/medical/net/json-to-dicom/">JSON ke DICOM</a>. <a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">Panduan serialisasi</a> mencakup seluruh API.</p>

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