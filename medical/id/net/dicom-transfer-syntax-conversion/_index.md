---
title: Konversi Transfer Syntax DICOM dalam C# .NET | Aspose.Medical
weight: 16000
description: Transcode file DICOM antar transfer syntax dalam C# .NET. Mendukung JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, RLE, dan format tidak terkompresi dengan Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konversi Transfer Syntax DICOM dalam .NET C#" h2="Transcode file DICOM antar transfer syntax tidak terkompresi, JPEG, JPEG 2000, HTJ2K, JPEG XL, JPEG-LS, dan RLE. Perpustakaan .NET murni tanpa dependensi native." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Apa Itu Transfer Syntax?">}}

<p>Sebuah <strong>Transfer Syntax</strong> mendefinisikan cara data DICOM dikodekan untuk penyimpanan dan transmisi. Ini menentukan tiga aspek kunci: urutan byte (endianness), apakah Value Representations bersifat eksplisit atau implisit, dan algoritma kompresi yang diterapkan pada data piksel. Setiap file DICOM menyatakan transfer syntax-nya dalam header File Meta Information.</p>

<p>Berbagai perangkat medis, server PACS, dan aplikasi penampil mendukung set transfer syntax yang berbeda. <strong>Aspose.Medical for .NET</strong> menyediakan metode <code>Transcode</code> untuk mengkonversi antar transfer syntax, memungkinkan interoperabilitas, optimalisasi penyimpanan, dan kompatibilitas dengan alat pemrosesan &mdash; semuanya dalam pustaka .NET murni tanpa dependensi native.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transcode File DICOM dalam C#">}}

<p>Metode <code>DicomFile.Transcode</code> mengkonversi file DICOM dari transfer syntax saat ini ke target syntax yang didukung. Metode ini mengembalikan instance <code>DicomFile</code> baru &mdash; file asli tetap tidak berubah:</p>

<div class="codeblock" id="code">
 <h3>Transcoding DICOM Dasar - C#</h3>
 <pre><code class="cs">// Load a DICOM file (any transfer syntax)
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy for storage optimization
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("compressed.dcm");

// Transcode to Explicit VR Little Endian (uncompressed) for maximum compatibility
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<p>Anda juga dapat melakukan transcoding pada level <code>Dataset</code> secara langsung:</p>

<div class="codeblock" id="code">
 <h3>Transcode Dataset - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode the dataset to RLE Lossless
Dataset transcoded = dicomFile.Dataset.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transfer Syntax yang Didukung">}}

<p>Tabel berikut mencantumkan semua transfer syntax data gambar DICOM standar dan status dukungan saat ini di Aspose.Medical for .NET. Semua codec yang didukung diimplementasikan dalam C# murni dan sepenuhnya independen platform.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Transfer Syntax</th>
<th>UID</th>
<th>Type</th>
<th>Status</th>
</tr>
</thead>
<tbody>
<tr><td colspan="4"><strong>Tidak Terkompresi</strong></td></tr>
<tr><td>Implicit VR Little Endian</td><td><code>1.2.840.10008.1.2</code></td><td>Tidak Terkompresi</td><td>Didukung</td></tr>
<tr><td>Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1</code></td><td>Tidak Terkompresi</td><td>Didukung</td></tr>
<tr><td>Explicit VR Big Endian</td><td><code>1.2.840.10008.1.2.2</code></td><td>Tidak Terkompresi (ditinggalkan)</td><td>Didukung</td></tr>
<tr><td>Encapsulated Uncompressed Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.98</code></td><td>Tidak Terkompresi</td><td>Tidak Didukung</td></tr>
<tr><td>Deflated Explicit VR Little Endian</td><td><code>1.2.840.10008.1.2.1.99</code></td><td>Deflated</td><td>Didukung</td></tr>
<tr><td colspan="4"><strong>JPEG</strong></td></tr>
<tr><td>JPEG Baseline (Process 1)</td><td><code>1.2.840.10008.1.2.4.50</code></td><td>Lossy, 8-bit</td><td>Didukung</td></tr>
<tr><td>JPEG Extended (Process 2 &amp; 4)</td><td><code>1.2.840.10008.1.2.4.51</code></td><td>Lossy, 12-bit</td><td>Tidak Didukung</td></tr>
<tr><td>JPEG Lossless (Process 14)</td><td><code>1.2.840.10008.1.2.4.57</code></td><td>Lossless</td><td>Didukung (hanya 8-bit)</td></tr>
<tr><td>JPEG Lossless, First-Order Prediction (Process 14, SV1)</td><td><code>1.2.840.10008.1.2.4.70</code></td><td>Lossless</td><td>Didukung (hanya 8-bit)</td></tr>
<tr><td colspan="4"><strong>JPEG-LS</strong></td></tr>
<tr><td>JPEG-LS Lossless</td><td><code>1.2.840.10008.1.2.4.80</code></td><td>Lossless</td><td>Didukung</td></tr>
<tr><td>JPEG-LS Near-Lossless</td><td><code>1.2.840.10008.1.2.4.81</code></td><td>Near-lossless</td><td>Didukung</td></tr>
<tr><td colspan="4"><strong>JPEG 2000</strong></td></tr>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>Didukung (baca warna 8-bit dan monokrom 16-bit; tulis monokrom 16-bit atau RGB 8-bit)</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy or lossless</td><td>Didukung (baca warna 8-bit dan monokrom 16-bit; tulis monokrom 16-bit atau RGB 8-bit)</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Tidak Didukung</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy or lossless</td><td>Tidak Didukung</td></tr>
<tr><td colspan="4"><strong>RLE</strong></td></tr>
<tr><td>RLE Lossless</td><td><code>1.2.840.10008.1.2.5</code></td><td>Lossless</td><td>Didukung</td></tr>
<tr><td colspan="4"><strong>High-Throughput JPEG 2000 (HTJ2K)</strong></td></tr>
<tr><td>HTJ2K Lossless Only</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Didukung</td></tr>
<tr><td>HTJ2K with RPCL Options Lossless Only</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Didukung</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy or lossless</td><td>Didukung</td></tr>
<tr><td colspan="4"><strong>JPEG XL</strong></td></tr>
<tr><td>JPEG XL Lossless</td><td><code>1.2.840.10008.1.2.4.110</code></td><td>Lossless</td><td>Didukung</td></tr>
<tr><td>JPEG XL JPEG Recompression</td><td><code>1.2.840.10008.1.2.4.111</code></td><td>Lossless</td><td>Hanya decode (pengkodean membutuhkan aliran sumber JPEG, bukan data piksel)</td></tr>
<tr><td>JPEG XL</td><td><code>1.2.840.10008.1.2.4.112</code></td><td>Lossy or lossless</td><td>Didukung (mode lossy)</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Skenario Transcoding Umum">}}

<p>Alur kerja yang berbeda memerlukan strategi transcoding yang berbeda. Berikut adalah skenario paling umum:</p>

<div class="codeblock" id="code">
 <h3>Decompress untuk pemrosesan - C#</h3>
 <pre><code class="cs">// Decompress any DICOM file to uncompressed format for image processing
DicomFile dicomFile = DicomFile.Open("compressed.dcm");
DicomFile uncompressed = dicomFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("uncompressed.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Compress untuk penyimpanan arsip - C#</h3>
 <pre><code class="cs">// Lossless compression for long-term archival (no quality loss)
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Option 1: JPEG 2000 Lossless — best compression ratio
DicomFile j2kArchive = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);

// Option 2: JPEG-LS Lossless — fast encode/decode
DicomFile jlsArchive = dicomFile.Transcode(TransferSyntax.JpegLsLossless);

// Option 3: RLE Lossless — universal compatibility
DicomFile rleArchive = dicomFile.Transcode(TransferSyntax.RleLossless);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Compress untuk transmisi jaringan - C#</h3>
 <pre><code class="cs">// Lossy compression for fast transmission (smaller file size)
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
DicomFile compressed = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
compressed.Save("for_transmission.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Gunakan codec terbaru: HTJ2K dan JPEG XL - C#</h3>
 <pre><code class="cs">// HTJ2K: JPEG 2000 quality with much faster encode and decode
DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
DicomFile htj2k = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2k.Save("ct_htj2k.dcm");

// JPEG XL: lossless or lossy, monochrome and color input
DicomFile jxlLossless = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
jxlLossless.Save("ct_jxl_lossless.dcm");

DicomFile jxlLossy = dicomFile.Transcode(TransferSyntax.JpegXL);
jxlLossy.Save("ct_jxl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Periksa Properti Transfer Syntax">}}

<p>Kelas <code>TransferSyntax</code> menampilkan properti yang menggambarkan karakteristik enkoding. Gunakan ini untuk memeriksa transfer syntax file saat ini atau untuk memilih target syntax yang sesuai:</p>

<div class="codeblock" id="code">
 <h3>Baca properti transfer syntax - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax

Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"UID: {ts.Uid}");
Console.WriteLine($"Explicit VR: {ts.IsExplicitVr}");
Console.WriteLine($"Little Endian: {ts.IsLittleEndian}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");
Console.WriteLine($"Retired: {ts.IsRetired}");</code></pre>
</div>

<table class="table table-bordered">
<thead>
<tr>
<th>Properti</th>
<th>Type</th>
<th>Deskripsi</th>
</tr>
</thead>
<tbody>
<tr><td><code>Uid</code></td><td><code>Uid</code></td><td>Pengidentifikasi unik dari transfer syntax</td></tr>
<tr><td><code>IsExplicitVr</code></td><td><code>bool</code></td><td>Apakah Value Representations dikodekan secara eksplisit</td></tr>
<tr><td><code>IsLittleEndian</code></td><td><code>bool</code></td><td>Apakah urutan byte adalah little endian</td></tr>
<tr><td><code>IsEncapsulated</code></td><td><code>bool</code></td><td>Apakah data piksel terenkapsulasi (terkompresi)</td></tr>
<tr><td><code>IsLossy</code></td><td><code>bool</code></td><td>Apakah metode kompresi bersifat lossy</td></tr>
<tr><td><code>IsDeflate</code></td><td><code>bool</code></td><td>Apakah syntax menggunakan kompresi deflate</td></tr>
<tr><td><code>IsRetired</code></td><td><code>bool</code></td><td>Apakah transfer syntax telah ditinggalkan oleh standar DICOM</td></tr>
<tr><td><code>LossyCompressionMethod</code></td><td><code>LossyCompressionMethods</code></td><td>Identifikasi standar ISO untuk metode kompresi lossy</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kompresi Lossy vs Lossless">}}

<p>Memahami perbedaan antara kompresi lossy dan lossless sangat penting saat transcoding file DICOM:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Aspek</th>
<th>Lossless</th>
<th>Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Kualitas gambar</td><td>Pixel-perfect &mdash; data asli terjaga sepenuhnya</td><td>Beberapa data hilang secara permanen untuk mencapai ukuran lebih kecil</td></tr>
<tr><td>Rasio kompresi</td><td>Biasanya 2:1 hingga 3:1</td><td>Biasanya 10:1 hingga 30:1 atau lebih</td></tr>
<tr><td>Aman pada proses round-trip</td><td>Ya &mdash; dekompresi menghasilkan piksel yang identik</td><td>Tidak &mdash; setiap re-encode lossy menurunkan kualitas lebih lanjut</td></tr>
<tr><td>Kasus penggunaan</td><td>Arsip, diagnostik, catatan hukum</td><td>Review awal, telemedisin, transmisi jaringan</td></tr>
<tr><td>Codec yang didukung</td><td>JPEG Lossless, JPEG-LS, JPEG 2000 Lossless, HTJ2K Lossless, JPEG XL Lossless, RLE</td><td>JPEG Baseline, JPEG-LS Near-Lossless, JPEG 2000, HTJ2K, JPEG XL</td></tr>
</tbody>
</table>

<p><strong>Penting:</strong> Transcoding dari file terkompresi lossy ke syntax lossless tidak mengembalikan data yang hilang. Penurunan kualitas dari kompresi lossy asli bersifat permanen.</p>

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
