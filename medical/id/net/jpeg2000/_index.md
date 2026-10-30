---
title: Kompressi DICOM JPEG 2000 dalam C# .NET | Aspose.Medical
weight: 2000
description: Baca, tulis, dan transkode file DICOM dengan kompresi JPEG 2000 dalam C# .NET. Mendukung gambar monokrom dan berwarna dari 1 hingga 16 bit, mode lossless dan lossy, serta HTJ2K dengan API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Dukungan DICOM JPEG 2000 dalam .NET C#" h2="Baca, tulis, dan transkode file DICOM dengan kompresi JPEG 2000. Mode lossless dan lossy, data piksel monokrom dan berwarna dari 1 hingga 16 bit, HTJ2K disertakan - semua dalam .NET murni." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="JPEG 2000 dalam Pencitraan Medis">}}

<p><strong>JPEG 2000</strong> (ISO/IEC 15444) adalah standar kompresi berbasis wavelet yang paling banyak digunakan dalam pencitraan medis. Tidak seperti JPEG tradisional, ia menawarkan kompresi lossless dan lossy dalam satu codec, dekoding progresif untuk akses region-of-interest, dan rasio kompresi yang superior &mdash; menjadikannya ideal untuk mengarsipkan studi besar dan mentransmisikan gambar melalui jaringan dengan keterbatasan.</p>

<p><strong>Aspose.Medical untuk .NET</strong> menyediakan implementasi C# murni dari codec JPEG 2000 tanpa dependensi native. Perpustakaan ini dapat membaca, merender, dan mentranskode file DICOM yang dikompresi dengan kedua sintaks transfer JPEG 2000 Part 1, lossless dan lossy.</p>

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
<tr><td>JPEG 2000 Hanya Lossless</td><td><code>1.2.840.10008.1.2.4.90</code></td><td>Lossless</td><td>Monokrom dan berwarna, 1 sampai 16 bit</td><td>Monokrom, warna palet dan RGB, 1 sampai 16 bit</td></tr>
<tr><td>JPEG 2000</td><td><code>1.2.840.10008.1.2.4.91</code></td><td>Lossy atau lossless</td><td>Monokrom dan berwarna, 1 sampai 16 bit</td><td>Monokrom dan RGB, 1 sampai 16 bit</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component Hanya Lossless</td><td><code>1.2.840.10008.1.2.4.92</code></td><td>Lossless</td><td>Tidak didukung</td><td>Tidak didukung</td></tr>
<tr><td>JPEG 2000 Part 2 Multi-component</td><td><code>1.2.840.10008.1.2.4.93</code></td><td>Lossy atau lossless</td><td>Tidak didukung</td><td>Tidak didukung</td></tr>
<tr><td>HTJ2K Hanya Lossless</td><td><code>1.2.840.10008.1.2.4.201</code></td><td>Lossless</td><td>Monokrom dan berwarna</td><td>Monokrom dan berwarna</td></tr>
<tr><td>HTJ2K dengan Opsi RPCL Hanya Lossless</td><td><code>1.2.840.10008.1.2.4.202</code></td><td>Lossless</td><td>Monokrom dan berwarna</td><td>Monokrom dan berwarna</td></tr>
<tr><td>HTJ2K</td><td><code>1.2.840.10008.1.2.4.203</code></td><td>Lossy atau lossless</td><td>Monokrom dan berwarna</td><td>Monokrom dan berwarna</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Data Piksel dari 1 hingga 16 Bit">}}

<p>Gambar medis sering menggunakan 16 bit per sampel untuk menangkap rentang dinamis penuh dari modalitas seperti CT (biasanya 12-bit disimpan dalam 16-bit) dan MRI. Aspose.Medical menangani setiap kedalaman dari 1 sampai 16 bit untuk JPEG 2000, dalam sampel 8-bit atau 16-bit:</p>

<ul>
<li><strong>Pembacaan (dekompresi)</strong>: file monokrom (CT, MRI, X-ray), file warna palet dan file warna tiga-komponen (RGB, YBR_RCT, YBR_ICT, dan label warna lain seperti YBR_FULL, didekodekan sebagai kode) dengan 1 sampai 16 bit per sampel, bertanda atau tidak bertanda. Aliran kode warna CMYK, profil ICC, palet JP2 dan sub-sampled ditolak dengan pengecualian yang jelas alih-alih gambar yang salah secara diam-diam.</li>
<li><strong>Penulisan (kompresi)</strong>: gambar monokrom dan RGB dengan 1 sampai 16 bit per sampel, bertanda atau tidak bertanda, dan gambar warna palet dengan JPEG 2000 Lossless. Gambar YBR tidak dikodekan kecuali mereka didekodekan menjadi RGB, seperti file JPEG YBR lossy.</li>
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

<p>Gunakan metode <code>Transcode</code> untuk mengompresi file DICOM apa pun ke JPEG 2000 atau untuk mengubah antara mode JPEG 2000:</p>

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

<p>Dekompresi file JPEG 2000 ke sintaks transfer tak terkompresi untuk pemrosesan, analisis, atau kompatibilitas dengan sistem yang tidak mendukung JPEG 2000:</p>

<div class="codeblock" id="code">
 <h3>Dekompresi JPEG 2000 menjadi tak terkompresi - C#</h3>
 <pre><code class="cs">// Load a JPEG 2000 compressed DICOM file
DicomFile compressed = DicomFile.Open("j2k_compressed.dcm");

// Decompress to Explicit VR Little Endian
DicomFile uncompressed = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decompressed.dcm");</code></pre>
</div>

<p>Anda juga dapat mendekompresi dan mentranskode ke format kompresi lain dalam satu langkah:</p>

<div class="codeblock" id="code">
 <h3>Transkode antara format kompresi - C#</h3>
 <pre><code class="cs">// Convert JPEG 2000 to JPEG-LS Lossless
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile jlsFile = j2kFile.Transcode(TransferSyntax.JpegLsLossless);
jlsFile.Save("jpegls_lossless.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render Gambar DICOM JPEG 2000">}}

<p>File DICOM terkompresi JPEG 2000 dapat dirender menjadi data piksel untuk tampilan atau ekspor, seperti halnya sintaks transfer lainnya:</p>

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
<tr><td>Rasio kompresi</td><td>Biasanya 2:1 sampai 3:1</td><td>Biasanya 10:1 sampai 30:1 atau lebih tinggi</td></tr>
<tr><td>Terbaik untuk</td><td>Pengarsipan diagnostik, catatan hukum, pembacaan utama</td><td>Review pendahuluan, telemedisin, transmisi jaringan</td></tr>
<tr><td>Aman untuk round-trip</td><td>Ya</td><td>Tidak &mdash; pengkodean ulang lebih lanjut menurunkan kualitas</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="High-Throughput JPEG 2000 (HTJ2K)">}}

<p>HTJ2K (ISO/IEC 15444-15) menggantikan coder aritmetika yang lambat pada JPEG 2000 dengan coder blok yang lebih cepat. Ia mempertahankan transformasi wavelet, urutan progresif dan kualitas yang sama, serta mendekode dan mengode beberapa kali lebih cepat. Aspose.Medical mengimplementasikan ketiga sintaks transfer DICOM HTJ2K dalam .NET murni, untuk gambar monokrom dan berwarna, serta mentranskode antara HTJ2K dan setiap sintaks lain yang didukung:</p>

<ul>
<li><code>HTJ2KLossless</code> (1.2.840.10008.1.2.4.201) &mdash; hanya lossless</li>
<li><code>HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202) &mdash; lossless dengan urutan progresi RPCL</li>
<li><code>HTJ2K</code> (1.2.840.10008.1.2.4.203) &mdash; lossy atau lossless</li>
</ul>

<div class="codeblock" id="code">
 <h3>Transkode JPEG 2000 ke HTJ2K dan kembali - C#</h3>
 <pre><code class="cs">// Transcode a JPEG 2000 file to HTJ2K, and back to classic JPEG 2000
DicomFile j2kFile = DicomFile.Open("j2k_lossless.dcm");
DicomFile htj2kFile = j2kFile.Transcode(TransferSyntax.HTJ2KLossless);
htj2kFile.Save("htj2k_lossless.dcm");

// HTJ2K with RPCL progression order, lossless
DicomFile rpclFile = j2kFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
rpclFile.Save("htj2k_rpcl.dcm");

// HTJ2K lossy
DicomFile htj2kLossy = j2kFile.Transcode(TransferSyntax.HTJ2K);
htj2kLossy.Save("htj2k_lossy.dcm");

// Any HTJ2K file decodes back to an uncompressed transfer syntax
DicomFile uncompressed = htj2kFile.Transcode(TransferSyntax.ExplicitVrLittleEndian);
uncompressed.Save("decoded.dcm");</code></pre>
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
