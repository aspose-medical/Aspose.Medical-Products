---
title: Kompresi DICOM JPEG 2000 dalam C# .NET | Aspose.Medical
weight: 2000
description: Baca, tulis, dan transkode file DICOM dengan kompresi JPEG 2000 dalam C# .NET. Mendukung gambar 8-bit dan 16-bit, mode lossless dan lossy, data multi-komponen dengan API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Dukungan DICOM JPEG 2000 dalam .NET C#" h2="Baca, tulis, dan transkode file DICOM dengan kompresi JPEG 2000. Mode lossless dan lossy, data piksel 8-bit dan 16-bit, gambar multi-komponen — semuanya dalam .NET murni." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 dalam Pencitraan Medis">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) adalah standar kompresi berbasis wavelet yang paling banyak digunakan dalam pencitraan medis. Berbeda dengan JPEG tradisional, ia menawarkan kompresi lossless dan lossy dalam satu codec, dekoding progresif untuk akses region-of-interest, dan rasio kompresi yang superior &mdash; menjadikannya ideal untuk mengarsipkan studi besar dan mentransmisikan gambar melalui jaringan terbatas.</p>

<p><strong>Aspose.Medical untuk .NET</strong> menyediakan implementasi C# murni dari codec JPEG 2000 tanpa ketergantungan native. Library ini dapat membaca, merender, dan mentranskode file DICOM yang terkompresi dengan salah satu dari empat sintaks transfer standar JPEG 2000.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sintaks Transfer JPEG 2000 yang Didukung">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Sintaks Transfer</th>
<th>UID</th>
<th>Mode</th>
<th>Baca</th>
<th>Tulis</th>
</tr>
</thead>
<tbody>
<tr><td>JPEG 2000 Lossless Only</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>8-bit dan 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy atau lossless</td><td>8-bit dan 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Lossless Only</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>8-bit dan 16-bit</td><td>8-bit</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy atau lossless</td><td>8-bit dan 16-bit</td><td>8-bit</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Data Piksel 8-Bit dan 16-Bit">}}

<p>Gambar medis sering menggunakan 16 bit per sampel untuk menangkap rentang dinamis penuh dari modalitas seperti CT (biasanya 12-bit disimpan dalam 16-bit) dan MRI. Aspose.Medical menangani kedua kedalaman bit untuk JPEG 2000:</p>

<ul>
<li><strong>Reading (decompression)</strong>: Dukungan penuh untuk file DICOM yang terkompresi JPEG 2000 baik 8-bit maupun 16-bit. Library ini secara tepat mendekode data piksel terlepas dari nilai Bits Allocated, Bits Stored, dan High Bit yang asli.</li>
<li><strong>Writing (compression)</strong>: Saat ini mendukung gambar 8-bit. Dukungan penulisan 16-bit direncanakan untuk rilis mendatang.</li>
</ul>

<div class="codeblock" id="code">
 <h3>Baca dan inspeksi DICOM terkompresi JPEG 2000 - C#</h3>
 <pre><code class="cs">// Open a JPEG 2000 compressed DICOM file (8-bit or 16-bit)
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Check transfer syntax
TransferSyntax? ts = dicomFile.MetaInfo.TransferSyntax;
if (ts is null)
    return; // the file meta information carries no transfer syntax
Console.WriteLine($"Transfer Syntax: {ts}");
Console.WriteLine($"Encapsulated: {ts.IsEncapsulated}");
Console.WriteLine($"Lossy: {ts.IsLossy}");

// Inspect pixel data bit depth
PixelData pixelData = PixelData.Create(dicomFile.Dataset);
Console.WriteLine($"Bits Allocated: {pixelData.BitsAllocated}");
Console.WriteLine($"Bits Stored: {pixelData.BitsStored}");
Console.WriteLine($"High Bit: {pixelData.HighBit}");
Console.WriteLine($"Samples Per Pixel: {pixelData.SamplesPerPixel}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Transkode ke JPEG 2000">}}

<p>Gunakan metode <code>Transcode</code> untuk mengompresi file DICOM apa pun ke JPEG 2000 atau mengubah antara mode JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Kompres DICOM ke JPEG 2000 Lossless - C#</h3>
 <pre><code class="cs">// Load an uncompressed DICOM file
DicomFile dicomFile = DicomFile.Open("uncompressed.dcm");

// Transcode to JPEG 2000 Lossless — no quality loss, reduced file size
DicomFile lossless = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossless);
lossless.Save("j2k_lossless.dcm");</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Kompres DICOM ke JPEG 2000 Lossy - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Transcode to JPEG 2000 Lossy — smaller file size for transmission
DicomFile lossy = dicomFile.Transcode(TransferSyntax.Jpeg2000Lossy);
lossy.Save("j2k_lossy.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dekompresi File DICOM JPEG 2000">}}

<p>Dekompresi file JPEG 2000 ke sintaks transfer tidak terkompresi untuk pemrosesan, analisis, atau kompatibilitas dengan sistem yang tidak mendukung JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Dekompresi JPEG 2000 ke tidak terkompresi - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Anda juga dapat dekompresi dan mentranskode ke format kompresi lain dalam satu langkah:</p>

<div class="codeblock" id="code">
 <h3>Transkode antara format kompresi - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render Gambar DICOM JPEG 2000">}}

<p>File DICOM yang terkompresi JPEG 2000 dapat dirender menjadi data piksel untuk tampilan atau ekspor, seperti halnya sintaks transfer lainnya:</p>

<div class="codeblock" id="code">
 <h3>Render frame terkompresi JPEG 2000 - C#</h3>
 <pre><code class="cs">// Open JPEG 2000 DICOM file
DicomFile dicomFile = DicomFile.Open("j2k_compressed.dcm");

// Render the first frame — decompression is handled automatically
using PixelImage&lt;Bgra32&gt; image = dicomFile.RenderImage(0);

// Copy the BGRA32 pixel data for display or export
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = new Bgra32[width * height];
image.CopyPixelsTo(pixels);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossless vs Lossy JPEG 2000">}}

<table class="table table-bordered">
<thead>
<tr>
<th>Aspek</th>
<th>JPEG 2000 Lossless</th>
<th>JPEG 2000 Lossy</th>
</tr>
</thead>
<tbody>
<tr><td>Sintaks Transfer</td><td><code>Jpeg2000Lossless</code> (1.2.840.10008.1.2.4.90)</td><td><code>Jpeg2000Lossy</code> (1.2.840.10008.1.2.4.91)</td></tr>
<tr><td>Kualitas gambar</td><td>Pixel-perfect &mdash; identik dengan asli</td><td>Serupa secara visual, beberapa data hilang secara permanen</td></tr>
<tr><td>Rasio kompresi</td><td>Biasanya 2:1 hingga 3:1</td><td>Biasanya 10:1 hingga 30:1 atau lebih</td></tr>
<tr><td>Terbaik untuk</td><td>Arsip diagnostik, catatan hukum, pembacaan utama</td><td>Review awal, telemedisin, transmisi jaringan</td></tr>
<tr><td>Aman untuk round-trip</td><td>Ya</td><td>Tidak &mdash; enkoding ulang lebih lanjut menurunkan kualitas</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 Part 2 Multi-Komponen">}}

<p>JPEG 2000 Part 2 (ISO/IEC 15444-2) memperluas codec standar dengan kemampuan transformasi multi-komponen. Ini digunakan untuk gambar medis berwarna dan modalitas yang menghasilkan data multi-kanal. Aspose.Medical mendukung kedua sintaks transfer Part 2:</p>

<ul>
<li><code>Jpeg2000Part2MultiComponentLosslessOnly</code> &mdash; kompresi lossless dengan decorrelation antar-komponen untuk kompresi optimal data multi-kanal.</li>
<li><code>Jpeg2000Part2MultiComponent</code> &mdash; kompresi lossy atau lossless dengan transformasi multi-komponen.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K) — Segera Hadir">}}

<p>HTJ2K (ISO/IEC 15444-15) adalah ekstensi generasi berikutnya dari JPEG 2000 yang dirancang untuk kecepatan enkode dan dekode yang jauh lebih cepat sambil mempertahankan efisiensi kompresi yang sama. Diperkirakan menjadi codec pilihan untuk alur kerja pencitraan medis waktu nyata.</p>

<p>Aspose.Medical akan menambahkan dukungan HTJ2K pada rilis mendatang, mencakup tiga sintaks transfer:</p>

<ul>
<li><code>HTJ2KLossles</code> (1.2.840.10008.1.2.4.201) &mdash; Hanya lossless</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; Lossless dengan urutan progresi RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; Lossy atau lossless</li>
</ul>

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
