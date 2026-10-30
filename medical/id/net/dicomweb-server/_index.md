---
title: Server DICOMweb dalam C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Bangun server DICOMweb atau PACS Anda sendiri dalam C#. Aspose.Medical mengimplementasikan QIDO-RS, WADO-RS, STOW-RS, dan layanan worklist pada ASP.NET Core; Anda menghubungkan penyimpanan.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Server DICOMweb untuk .NET C#" h2="Tambahkan endpoint QIDO-RS, WADO-RS, dan STOW-RS ke layanan ASP.NET Core. Perpustakaan ini berbicara dengan protokol; kode Anda menentukan di mana studi disimpan." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Server DICOMweb milik Anda sendiri dalam C#">}}

<p>Sebuah PACS, VNA, arsip cloud, atau platform riset harus merespon permintaan DICOMweb: pencarian QIDO-RS, unduhan WADO-RS, unggahan STOW-RS, dan sering juga layanan worklist. Masing‑masing memiliki rute, tipe media, badan multipart, aturan pencocokan, dan kode statusnya sendiri.</p>

<p><strong>Aspose.Medical untuk .NET</strong> mengimplementasikan sisi protokol tersebut sejak versi 26.10. Ia membaca setiap permintaan, memeriksanya terhadap standar, mencocokkan kunci pencarian berdasarkan aturan DICOM, mem-paginasi hasil, mengkonversi transfer syntax, dan menulis respons. Kode Anda melakukan apa yang hanya Anda yang dapat: menyimpan sebuah instance, menemukan kandidat untuk pencarian, dan mengembalikan file yang disimpan. Viewer, modality, dan arsip lainnya kemudian bekerja dengan server Anda seperti server DICOMweb lainnya, begitu pula <a href="/medical/net/dicomweb-client/">klien DICOMweb</a> dari perpustakaan yang sama.</p>

<p>Server ini dikelola dengan C#. Ia berjalan di mana pun ASP.NET Core berjalan: di Windows, di Linux, dan dalam sebuah kontainer.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Server lengkap dalam ASP.NET Core">}}

<p>Pasang paket <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Paket ini menyertakan Aspose.Medical dan menghubungkan layanan DICOMweb ke dependency injection serta endpoint routing. <code>Program.cs</code> ini adalah server Studies yang berfungsi; dua kelas yang didaftarkannya ditampilkan di bawah.</p>

<div class="codeblock" id="code">
 <h3>Program.cs dari server DICOMweb - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>Setiap rute Studies kini menjawab di bawah <code>/dicomweb</code>: <code>/dicomweb/studies</code> untuk pencarian dan unggahan, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> untuk pengambilan, serta rute untuk metadata, frame, bulk data, dan gambar yang dirender. Dokumen kapabilitas di akar layanan mencantumkan secara tepat transaksi yang didukung server.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Hubungkan penyimpanan Anda">}}

<p>Layanan Studies membutuhkan satu antarmuka dengan tiga metode: menyimpan sebuah instance, mengembalikan file yang disimpan, dan mengembalikan kandidat pencarian. Implementasi ini menyimpan setiap instance sebagai file dalam folder per study dan series.</p>

<div class="codeblock" id="code">
 <h3>Arsip folder di belakang QIDO-RS, WADO-RS, dan STOW-RS - C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>Perpustakaan memeriksa UID pada setiap jalur permintaan dan pada setiap instance yang diunggah sebelum memanggil provider. Instance tanpa UID yang valid dilaporkan sebagai gagal dan tidak pernah sampai pada <code>StoreAsync</code>.</p>

<p>Sebuah folder sudah cukup untuk mencoba server. Arsip produksi mempertahankan tiga metode yang sama dan menempatkan object store di belakang <code>StoreAsync</code> dan <code>RetrieveAsync</code> serta indeks basis data di belakang <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Pencarian: perpustakaan menerapkan aturan pencocokan DICOM">}}

<p>Pencocokan QIDO-RS lebih dari sekadar perbandingan string: wildcard, rentang tanggal dan waktu, daftar UID, nama orang yang tidak tepat, nilai kosong, dan kunci di dalam urutan. Provider tidak mengimplementasikan apapun dari itu. Provider mengembalikan kandidat dalam urutan yang stabil, dan mungkin mengembalikan lebih dari yang cocok. Perpustakaan menerapkan pencocokan DICOM yang tepat pada setiap kandidat, menyimpan bidang yang diminta, dan menerapkan <code>offset</code> serta <code>limit</code>. Satu respons menampung paling banyak 1000 hasil secara default, dan <code>DicomWebStandardServiceOptions</code> mengubah batas tersebut.</p>

<p>Untuk menjaga kecepatan pencarian pada arsip besar, lewati kunci selektif ke basis data. <code>search.Plan.Filter</code> menyimpan kunci pencarian yang telah dikompilasi, dengan atribut, jenis pencocokan, dan nilai masing‑masing.</p>

<div class="codeblock" id="code">
 <h3>Baca Patient ID yang tepat dari pencarian - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Query basis data yang dibangun seperti ini dapat mengembalikan baris ekstra, misalnya ketika mengabaikan kunci yang tidak dapat diterjemahkan. Pencocokan akhir di perpustakaan menghapusnya. Itu tidak boleh menghilangkan pencocokan yang sebenarnya, karena perpustakaan tidak dapat menambahkannya kembali.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Simpan: hasil untuk setiap instance">}}

<p>Permintaan STOW-RS dapat membawa ratusan instance. Perpustakaan mem-parsing badan multipart dan memanggil <code>StoreAsync</code> untuk tiap instance secara bergantian, sehingga seluruh unggahan tidak pernah disimpan sekaligus di memori. Untuk tiap instance, provider mengembalikan <code>Stored</code>, <code>Warning</code> untuk instance yang disimpan dengan perubahan, atau <code>Failed</code> dengan kode alasan DICOM opsional.</p>

<p>Perpustakaan membangun laporan STOW-RS dari hasil ini dan memilih status: 200 ketika setiap instance disimpan, 202 ketika beberapa memiliki peringatan atau gagal, 409 ketika semua gagal. Instance dalam permintaan untuk satu study bernama yang milik study lain ditolak sebelum provider dipanggil.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Ambil: transfer syntax, frame, dan gambar yang dirender">}}

<p>Provider mengembalikan file yang disimpan, dan perpustakaan menyampaikannya dalam format yang diminta klien. Klien WADO-RS dapat meminta transfer syntax pada header <code>Accept</code>, dan perpustakaan mengkonversi tiap file sebelum mengirimkannya, dengan codec miliknya sendiri, termasuk HTJ2K dan JPEG XL. Metadata dalam DICOM JSON atau XML, bulk data, dan frame tunggal berasal dari provider yang sama, tanpa kode tambahan.</p>

<p>Gambar yang dirender memerlukan satu kelas tambahan, karena hanya Anda yang menentukan gambar mana yang dapat diproduksi server. Kelas ini menyediakan JPEG untuk instance yang dirender, frame yang dirender, dan thumbnail, dan perpustakaan merendernya:</p>

<div class="codeblock" id="code">
 <h3>Tawarkan gambar JPEG yang dirender dan thumbnail - C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>Renderer bawaan menghasilkan JPEG dari instance satu frame atau dari frame yang dipilih permintaan. Instance multi-frame yang dirender secara keseluruhan, gambar yang lebarnya atau tingginya lebih dari 65500 piksel (thumbnail melewati gambar seperti itu), format gambar lain, serta parameter viewport, window, dan profil ICC memerlukan <code>RetrieveAsync</code> milik Anda dalam kelas ini.</p>

<p>Thumbnail ditangani dengan hati-hati demi privasi pasien. Renderer bawaan membuat thumbnail hanya dari instance yang atribut Burned In Annotation dan Recognizable Visual Features‑nya keduanya <code>NO</code>, yang berarti gambar tersebut menyatakan tidak menampilkan data pasien. Banyak gambar tidak memiliki atribut ini, dan untuk mereka server menjawab 406. Jika arsip Anda memerlukan thumbnail untuk gambar semacam itu, hasilkan dalam <code>RetrieveAsync</code> milik Anda sendiri dan pastikan mereka menyembunyikan semua data pasien.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklist, langkah prosedur, dan layanan lainnya">}}

<p>Layanan DICOMweb lainnya mengikuti model yang sama: daftarkan layanan, implementasikan antarmuka penyimpanannya.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> worklist dengan <code>AddStandardWorklistService</code>: membuat, mencari, dan memperbarui workitem serta mengubah statusnya, dengan aturan status diperiksa oleh perpustakaan dan notifikasi peristiwa dikirim melalui WebSocket.</li>
<li><a href="/medical/net/modality-worklist/">Modality worklist</a> (MWL-RS) dengan <code>AddStandardModalityWorklistService</code>, sehingga modality dapat menanyakan langkah prosedur terjadwal pada RIS Anda.</li>
<li>Langkah prosedur yang dilakukan modality (MPPS-RS), komitmen penyimpanan, dan instance non-pasien seperti palet warna dan hanging protocol.</li>
<li>Pengambilan WADO-URI untuk viewer lama, dilayani oleh layanan Studies.</li>
</ul>

<p>Ketika perilaku standar suatu transaksi tidak cocok, daftarkan handler Anda sendiri untuk itu dengan <code>AddHandler</code>. Perpustakaan tetap merutekan dan memeriksa permintaan, dan transaksi lainnya mempertahankan handler standar mereka.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Keamanan dan hosting tetap di tangan Anda">}}

<p>Endpoint DICOMweb adalah endpoint ASP.NET Core biasa. Autentikasi, otorisasi, CORS, TLS, header yang diteruskan, dan batas ukuran permintaan dikonfigurasi dengan cara yang sama seperti bagian lain layanan Anda:</p>

<div class="codeblock" id="code">
 <h3>Memerlukan pengguna yang terautentikasi - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Bagian protokol tidak bergantung pada ASP.NET Core. <code>DicomWebApplication</code> mengubah <code>DicomWebRequest</code> menjadi respons, sehingga layanan yang sama dapat berjalan di belakang server HTTP lain atau dalam tes tanpa jaringan:</p>

<div class="codeblock" id="code">
 <h3>Server yang sama tanpa ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Klien dan server dari satu perpustakaan">}}

<p><a href="/medical/net/dicomweb-client/">Klien DICOMweb</a> dari Aspose.Medical menguji server Anda melalui unit test atau program konsol, dan setiap file yang diterima server adalah <code>DicomFile</code> biasa yang dapat dianonimkan, ditranskode, atau dirender dalam proses yang sama.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">Panduan pengembang DICOMweb</a> mencakup setiap layanan dan antarmuka penyimpanan, dan <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">panduan ASP.NET Core</a> mencakup pemetaan endpoint serta WebSockets.</p>

<p>Halaman terkait: <a href="/medical/net/dicom-networking/">Jaringan DICOM</a> untuk sistem yang mengirim studi melalui DIMSE bukan HTTP, <a href="/medical/net/anonymization/">anonimisasi</a> studi masuk, <a href="/medical/net/dicom-transfer-syntax-conversion/">konversi transfer syntax</a>, dan <a href="/medical/net/dicom-to-json/">DICOM ke JSON</a> untuk format yang digunakan hasil QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Sumber Belajar" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentasi" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Panduan Pengembang" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
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
