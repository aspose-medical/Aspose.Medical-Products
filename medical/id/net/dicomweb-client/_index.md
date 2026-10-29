---
title: Klien DICOMweb dalam C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Cari, unduh, dan unggah studi DICOM melalui HTTP dari .NET. Sebuah klien DICOMweb dalam C# murni untuk QIDO-RS, WADO-RS, dan STOW-RS yang bekerja dengan PACS, VNA, dan layanan pencitraan cloud.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Klien DICOMweb untuk .NET C#" h2="Cari, unduh, dan unggah studi pada server DICOMweb apa pun dengan QIDO-RS, WADO-RS, dan STOW-RS, dalam C# terkelola tanpa harus menginstal apa pun di mesin." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM lewat HTTP, dari kode Anda sendiri">}}

<p>DICOMweb adalah bentuk HTTP dari DICOM. Alih-alih asosiasi DIMSE pada port 104, aplikasi Anda mengirim permintaan HTTPS biasa: QIDO-RS untuk pencarian, WADO-RS untuk pengunduhan, STOW-RS untuk unggahan. Layanan pencitraan cloud seperti layanan DICOM Azure Health Data Services dan Google Cloud Healthcare API beroperasi dengan cara ini, begitu pula arsip seperti Orthanc dan dcm4chee.</p>

<p><strong>Aspose.Medical untuk .NET</strong> mencakup klien DICOMweb sejak versi 26.10. <code>DicomWebClient</code> membungkus setiap transaksi standar dalam metode bertipe, dan hasilnya datang sebagai objek <code>Dataset</code> dan <code>DicomFile</code> yang sama yang dibaca dan ditulis oleh bagian lain perpustakaan. Server di sisi lain dapat berupa implementasi DICOMweb apa pun; tidak harus dibangun dengan Aspose.Medical.</p>

<p>Klien ini adalah C# terkelola di atas <code>HttpClient</code>. Tidak ada toolkit native dan tidak ada layanan proxy, sehingga kode yang sama dapat dijalankan di Windows, Linux, dan dalam container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Terhubung ke server DICOMweb">}}

<p>Buat klien dari <code>HttpClient</code> dan URL root layanan server. Otentikasi, pengaturan TLS, proxy, dan pooling koneksi tetap pada <code>HttpClient</code>, tempat aplikasi Anda sudah mengkonfigurasikannya. Klien tidak memiliki <code>HttpClient</code> dan tidak membuangnya.</p>

<div class="codeblock" id="code">
 <h3>Buat klien DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Jika satu penyebaran melayani WADO-URI atau layanan lain dari URL yang berbeda, <code>ServiceBaseUris</code> menggantikan root hanya untuk layanan tersebut.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Cari studi dengan QIDO-RS">}}

<p>Pencarian menggunakan parameter pencocokan dengan kata kunci DICOM sebagai nama, bidang yang akan dikembalikan, dan kontrol paging. Hasil cocok muncul sebagai aliran dataset, dibaca satu per satu sementara respons masih diunduh.</p>

<div class="codeblock" id="code">
 <h3>Temukan studi seorang pasien - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> dan <code>SearchInstancesAsync</code> mencari satu tingkat ke bawah, melintasi seluruh arsip atau di dalam satu studi atau seri. Opsi yang sama juga mengatur pencocokan fuzzy, pencocokan nilai kosong, dan offset hasil untuk paging.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Unduh studi dengan WADO-RS">}}

<p>Sebuah studi dapat berisi ribuan instance. <code>RetrieveStudyAsync</code> mengembalikannya sebagai aliran, sehingga setiap file diproses saat bagiannya dari respons tiba dan dapat disimpan sebelum yang berikutnya dibaca. Penggunaan memori tidak bertambah seiring ukuran studi.</p>

<div class="codeblock" id="code">
 <h3>Simpan setiap instance dari sebuah studi - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> dan <code>RetrieveInstanceAsync</code> mengunduh satu seri atau satu instance dengan cara yang sama.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dapatkan thumbnail atau gambar yang dirender">}}

<p>Penampil atau layar worklist sering memerlukan gambar, bukan file DICOM. Server merendernya dan mengirim JPEG atau PNG, sehingga klien tidak mengunduh atau mendekode data piksel asli.</p>

<div class="codeblock" id="code">
 <h3>Simpan thumbnail JPEG dari sebuah instance - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p><code>RetrieveRenderedInstanceAsync</code> dan <code>RetrieveRenderedFramesAsync</code> mengembalikan gambar berukuran penuh sebagai gantinya. <code>DicomWebRenderOptions</code> meneruskan kontrol rendering standar ke server: kualitas gambar, ukuran viewport, window, dan profil ICC.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Unggah instance dengan STOW-RS">}}

<p>STOW-RS mengirim sejumlah instance dalam satu permintaan. Server menjawab dengan laporan yang mencantumkan, per instance, apa yang disimpan dan apa yang ditolak.</p>

<div class="codeblock" id="code">
 <h3>Unggah folder berisi file DICOM - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>Beberapa server menjawab dengan kesalahan HTTP, misalnya 409, ketika satu atau lebih instance gagal. Panggilan kemudian melempar <code>DicomWebClientException</code>, seperti yang ditunjukkan pada bagian berikut, dan laporan yang sama ada di badan respons pengecualian.</p>

<p><code>StoreStudyAsync</code> mengunggah ke satu studi bernama. Kedua metode juga menerima <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, sehingga file dapat dihasilkan saat permintaan sedang dikirim.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tangani kesalahan server">}}

<p>Setiap operasi bertipe memeriksa status HTTP sebelum mengembalikan. Ketika server menolak permintaan, panggilan melempar <code>DicomWebClientException</code>, yang membawa kode status, header, dan awal badan respons untuk log.</p>

<div class="codeblock" id="code">
 <h3>Laporan permintaan yang gagal - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>Metode yang mengembalikan respons mentah, seperti thumbnail di atas, menyerahkan pemeriksaan status kepada Anda melalui <code>IsSuccessStatusCode</code> atau <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Apa lagi yang dicakup klien">}}

<p>Pencarian, pengunduhan, dan unggahan adalah panggilan umum. Klien juga memiliki grup operasi untuk masing-masing layanan DICOMweb lainnya:</p>

<ul>
<li>Data bulk, data piksel, dan frame terpilih dari sebuah instance, berdasarkan nomor frame satu‑berdasarkan (one‑based), sebagaimana dikirim server.</li>
<li>WADO-URI, pengambilan single‑URL lama yang masih digunakan banyak penampil dan arsip, untuk file DICOM dan gambar yang dirender.</li>
<li>Worklist UPS-RS: membuat, mencari, memperbarui, dan mengklaim workitem, mengubah statusnya, meminta pembatalan, serta berlangganan ke event yang datang melalui WebSocket.</li>
<li>Modality worklist (MWL-RS), langkah prosedur yang dilakukan oleh modality (MPPS-RS), dan komitmen penyimpanan.</li>
<li>Instance non‑pasien seperti palet warna dan protokol hanging, serta dokumen kemampuan server.</li>
<li><code>SendAsync</code> untuk transaksi standar apa pun dengan nilai rute, parameter kueri, header, dan body Anda sendiri, bila metode bertipe tidak cocok.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">Panduan klien DICOMweb</a> mendokumentasikan setiap operasi dan opsi, dan <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">referensi operasi klien</a> mencantumkan semua metode.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Satu perpustakaan dari respons HTTP hingga data piksel">}}

<p>File yang diunduh dengan klien adalah <code>DicomFile</code> biasa. File tersebut dapat dianonimkan, ditranskode, dirender, atau diserialisasi dalam proses yang sama, tanpa langkah konversi dan tanpa perpustakaan kedua dalam rantai.</p>

<p>Halaman terkait: <a href="/medical/net/dicomweb-server/">Server DICOMweb</a> untuk sisi lain protokol dalam layanan ASP.NET Core Anda, <a href="/medical/net/dicom-networking/">Jaringan DICOM</a> untuk arsip yang menggunakan DIMSE alih-alih HTTP, <a href="/medical/net/anonymization/">Anonimisasi</a> sebelum studi meninggalkan rumah sakit, <a href="/medical/net/dicom-transfer-syntax-conversion/">Konversi sintaks transfer</a> untuk apa yang disimpan server, dan <a href="/medical/net/dicom-to-json/">DICOM ke JSON</a> untuk format hasil pencarian QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Sumber Daya Pembelajaran" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentasi" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Panduan Pengembang" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="Referensi API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
