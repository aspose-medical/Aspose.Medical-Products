---
title: Bekerja dengan File DICOM Besar dalam C# .NET | Aspose.Medical
weight: 11500

description: Buka studi multi-frame dan whole slide image dalam C# tanpa memuatnya ke memori. Baca metadata tanpa data piksel, tunda elemen besar, dan pindahkan file melalui streams dan pipes.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="File DICOM Besar dalam .NET C#" h2="Baca metadata dari studi multi-frame tanpa piksel, tunda elemen besar hingga ada yang memintanya, dan pindahkan seluruh file melalui streams dan pipes." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="File-nya besar, pertanyaannya biasanya kecil">}}

<p>Whole slide image, serial CT yang panjang, atau volume OCT dapat berukuran ratusan megabita, dan sebagian besar adalah data piksel. Pekerjaan yang sebenarnya dilakukan aplikasi biasanya jauh lebih kecil: menampilkan isi folder, memeriksa identitas pasien, menghitung frame, memutuskan ke mana studi harus ditempatkan. Memuat setiap byte untuk menjawab itulah yang mengubah pekerjaan sederhana menjadi masalah memori.</p>

<p><strong>Aspose.Medical for .NET</strong> memungkinkan pemanggil menentukan seberapa banyak file yang dibaca. Pilihannya adalah satu argumen pada <code>DicomFile.Open</code>, dan berlaku untuk file, streams, dan pipes secara serupa.</p>

<p>Diukur pada studi 14 MB dengan 128 frame dari set uji kami, pada mesin dan file yang sama:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Strategi membaca</th>
<th>Waktu membuka</th>
<th>Memori yang dialokasikan</th>
</tr>
</thead>
<tbody>
<tr><td>Semua, default</td><td>653 ms</td><td>73 MB</td></tr>
<tr><td>Elemen besar dilewati</td><td>95 ms</td><td>55 MB</td></tr>
<tr><td>Elemen besar ditunda</td><td>95 ms</td><td>55 MB</td></tr>
</tbody>
</table>

<p>Kesenjangan meningkat seiring ukuran file. Folder berisi 10.000 studi adalah kasus di mana ini tidak lagi menjadi mikro-optimasi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Baca metadata, biarkan piksel apa adanya">}}

<p><code>TagDataReadingStrategies.SkipLargeTags</code> meninggalkan setiap elemen di atas ambang ukuran tidak dibaca. Dataset yang kembali berisi tag yang dibutuhkan oleh indeks atau router.</p>

<div class="codeblock" id="code">
 <h3>Baca studi tanpa data pikelnya - C#</h3>
 <pre><code class="cs">// Elements above the threshold are left out of the read
DicomFile dicomFile = DicomFile.Open(
    "whole_slide.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.SkipLargeTags());

string? patient = dicomFile.Dataset.GetSingleValueOrDefault(Tag.PatientName, string.Empty);
string? study = dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
Console.WriteLine($"{patient} / {study} / {dicomFile.NumberOfFrames} frames");</code></pre>
</div>

<p>Ambang nilai default adalah 64 kB dan menerima nilai dalam kilobita, jadi alur kerja yang menganggap 8 kB besar dapat menyatakannya.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Tunda alih-alih lewati">}}

<p>Ketika piksel mungkin diperlukan, tetapi kemungkinan nanti dan tidak semuanya, <code>ReadLargeOnDemand</code> adalah pasangan lainnya. Membuka file memerlukan biaya yang sama seperti melewati, dan elemen besar dibaca pada saat kode menyentuhnya.</p>

<div class="codeblock" id="code">
 <h3>Muat frame hanya saat digunakan - C#</h3>
 <pre><code class="cs">// Elements above 64 kB are read when they are used, not when the file is opened
DicomFile dicomFile = DicomFile.Open(
    "study.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 64));

// Nothing heavy has been read yet
PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// This is the point where the frame comes off the disk
Span&lt;byte&gt; frame = pixelData.GetFrame(0);
Console.WriteLine($"{frame.Length} bytes in frame 0 of {pixelData.NumberOfFrames}");</code></pre>
</div>

<p>Pembacaan tertunda adalah fitur berlisensi; strategi lainnya juga berfungsi dalam evaluasi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Indeks folder tanpa menyinggung piksel">}}

<p>Strategi yang sama berlaku pada stream, yang merupakan cara pemindaian arsip atau penyimpanan objek cloud terlihat dari kode.</p>

<div class="codeblock" id="code">
 <h3>Pindai arsip - C#</h3>
 <pre><code class="cs">foreach (string path in Directory.EnumerateFiles("archive", "*.dcm"))
{
    await using FileStream stream = File.OpenRead(path);

    DicomFile dicomFile = await DicomFile.OpenAsync(
        stream,
        ReadDicomStreamOptions.Default,
        TagDataReadingStrategies.SkipLargeTags());

    Console.WriteLine($"{path}: {dicomFile.Dataset.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty)}");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Streams dan pipes, masuk dan keluar">}}

<p>Pembacaan dan penulisan keduanya menerima streams, dan titik masuk asynchronous juga menerima tipe <code>System.IO.Pipelines</code>. Sebuah studi dapat berpindah dari respons jaringan ke penyimpanan tanpa proses harus menampung seluruh file sebagai satu array.</p>

<div class="codeblock" id="code">
 <h3>Baca dan tulis melalui streams - C#</h3>
 <pre><code class="cs">await using FileStream input = File.OpenRead("study.dcm");
DicomFile dicomFile = await DicomFile.OpenAsync(input);

await using FileStream output = File.Create("copy.dcm");
await dicomFile.SaveAsync(output);</code></pre>
</div>

<p>Ide yang sama mencakup representasi teks: sebuah dokumen dengan banyak dataset dibaca satu dataset pada satu waktu pada halaman <a href=\"/medical/net/json-to-dicom/\">JSON ke DICOM</a> dan <a href=\"/medical/net/xml-to-dicom/\">XML ke DICOM</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Frame demi frame">}}

<p>Data multi-frame ditangani per frame, sehingga seri 500 frame memproses satu frame pada satu waktu alih-alih seluruh elemen data piksel.</p>

<div class="codeblock" id="code">
 <h3>Jelajahi frame - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open(
    "series.dcm",
    ReadDicomFileOptions.Default,
    TagDataReadingStrategies.ReadLargeOnDemand());

PixelData pixelData = PixelData.Read(dicomFile.Dataset);

for (int frame = 0; frame &lt; pixelData.NumberOfFrames; frame++)
{
    Span&lt;byte&gt; bytes = pixelData.GetFrame(frame);
    Console.WriteLine($"frame {frame}: {bytes.Length} bytes");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Di mana ini menentukan desain">}}

<ul>
<li>Pengindeksan dan migrasi arsip: jutaan file, dan hanya header yang penting hingga sesuatu dipindahkan.</li>
<li>Router dan node penyimpanan: menerima sebuah studi, membaca apa yang diperlukan untuk merutekannya, meneruskan byte-byte tersebut.</li>
<li>Pipeline AI: membangun manifest dari metadata, lalu mengambil frame untuk subset yang benar-benar dilatih.</li>
<li>Kontainer dengan batas memori: set kerja mengikuti strategi, bukan ukuran file.</li>
<li>Data whole slide dan OCT: file di mana membaca semuanya bukan pilihan sama sekali.</li>
</ul>

<p><a href=\"https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/\">Panduan manajemen memori</a> menjelaskan strategi secara detail, dan <a href=\"/medical/net/dicom-networking/\">Jaringan DICOM</a> menampilkan data yang sama datang melalui DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Sumber Belajar" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentasi" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Panduan Pengembang" href="https://docs.aspose.com/medical/net/developer-guide/open-dicom-file/memory-management/" >}}
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
