---
title: JPEG XL untuk DICOM dalam C# .NET | Aspose.Medical
weight: 10500

description: Simpan gambar DICOM dalam JPEG XL dari C#. JPEG XL lossless yang mengembalikan piksel bit demi bit, dalam satu assembly terkelola tanpa codec native untuk dideploy.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL untuk DICOM dalam .NET C#" h2="Kompresi terbaru dalam standar DICOM, dengan file lossless terkecil yang kami ukur, diimplementasikan dalam C# terkelola dan dikemas dalam satu assembly." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Mengapa JPEG XL sampai ke DICOM">}}

<p>Arsip medis terus bertambah dan tidak pernah berkurang. JPEG XL adalah codec yang dirancang dunia pencitraan setelah dua dekade pengalaman dengan JPEG dan JPEG 2000, dan DICOM menambahkannya sebagai transfer syntax karena alasan yang penting bagi tim penyimpanan: untuk piksel yang sama, berkasnya lebih kecil.</p>

<p><strong>Aspose.Medical untuk .NET</strong> menulis dan membaca JPEG XL melalui port C# dari libjxl yang berada di dalam perpustakaan. Paket ini mengirim satu assembly, <code>Aspose.Medical.dll</code>, dan tidak ada biner native di sampingnya, sehingga codec baru ini tidak menjadi proyek deployment: assembly yang sama berjalan di Windows, Linux, pada agen build, dan di dalam container.</p>

<p>Dua transfer syntax membawa piksel:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), untuk data diagnostik yang harus kembali tanpa perubahan.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), untuk kasus di mana berkas yang lebih kecil lebih penting daripada salinan yang persis.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kompres sebuah studi, pertahankan setiap piksel">}}

<p>Transcoding dilakukan dalam satu panggilan, dan dataset yang mengelilingi piksel ikut bersamanya.</p>

<div class="codeblock" id="code">
 <h3>Transcode file DICOM ke JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Kami mengukurnya pada gambar 16-bit berukuran 1714 x 1933 dari set pengujian kami: 6,3 MB tanpa kompresi menjadi 2,7 MB dalam JPEG XL lossless, yang lebih kecil daripada gambar yang sama dalam HTJ2K lossless. Angka Anda sendiri tergantung pada modalitas, jadi jalankan perbandingan pada folder berkas Anda sebelum memilih.</p>

<p>Lossless adalah istilah yang harus diartikan secara harfiah di sini. Transcode ke JPEG XL dan kembali, dan data piksel sama dengan byte yang Anda mulai, sehingga arsip dapat dikompres ulang tanpa dipertanyakan kualitas diagnostik.</p>

<div class="codeblock" id="code">
 <h3>Kembali ke syntax tidak terkompresi - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Baca apa yang sudah disimpan sebagai JPEG XL">}}

<p>File yang datang dalam format JPEG XL terbuka seperti file lain. Transfer syntax menunjukkan apa itu, dan data piksel tersedia setelah frame didekode.</p>

<div class="codeblock" id="code">
 <h3>Buka file JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL atau HTJ2K">}}

<p>Kedua codec ini baru, keduanya lossless bila Anda meminta lossless, dan perpustakaan menulis serta membaca keduanya. Mereka menjawab pertanyaan yang berbeda.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Pertanyaan</th>
<th>Jawaban</th>
</tr>
</thead>
<tbody>
<tr><td>Mana yang menghasilkan berkas lebih kecil dalam pengujian kami</td><td>JPEG XL lossless, beberapa persen lebih kecil</td></tr>
<tr><td>Mana yang dirancang untuk tampilan progresif melalui jaringan</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, khususnya varian RPCL</td></tr>
<tr><td>Mana yang pertama masuk ke standar DICOM</td><td>HTJ2K, sehingga lebih banyak arsip yang menerimanya saat ini</td></tr>
<tr><td>Mana yang membutuhkan dependensi native di sini</td><td>Tidak ada, keduanya adalah kode terkelola dalam satu assembly</td></tr>
</tbody>
</table>

<p>Pilihan biasanya berasal dari sisi lain tautan: transcode ke syntax yang diterima arsip, dan pertahankan sisa alur proses tetap sama.</p>

<div class="codeblock" id="code">
 <h3>Biarkan arsip target yang memutuskan - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Dimana manfaatnya">}}

<ul>
<li>Arsip jangka panjang: studi yang sama, terabyte lebih sedikit, dan tidak ada kehilangan yang perlu dibenarkan kepada radiolog.</li>
<li>Tagihan penyimpanan awan: penghematan berulang setiap bulan, sementara transcoding hanya dijalankan sekali.</li>
<li>Set data untuk riset dan AI: salinan lebih kecil berpindah lebih cepat antara penyimpanan dan pelatihan.</li>
<li>Deployment: codec baru seperti ini biasanya memerlukan build native per platform; di sini menjadi bagian dari assembly yang sudah Anda referensikan.</li>
</ul>

<p>Perpustakaan ini juga menulis codec yang terdapat dalam arsip yang ada: JPEG, JPEG-LS, JPEG 2000, HTJ2K, dan RLE. Halaman <a href="/medical/net/dicom-transfer-syntax-conversion/">konversi transfer syntax</a> mencakup seluruh set, <a href="/medical/net/htj2k/">HTJ2K</a> memiliki halaman tersendiri, dan <a href="/medical/net/jpeg2000/">JPEG 2000</a> adalah asal kedua codec baru tersebut.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Sumber Belajar" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentasi" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Panduan Pengembang" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
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
