---
title: Konversi DICOM ke JPG dalam C# .NET | Aspose.Medical
weight: 7000
description: Render gambar DICOM ke data piksel dalam C# .NET dengan kontrol window/level, rendering overlay, dan dukungan multi-frame. Gunakan pipeline LUT DICOM lengkap dengan API Aspose.Medical dan simpan ke JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konversi DICOM ke JPG dalam .NET C#" h2="Render gambar DICOM ke data piksel mentah dengan dukungan pipeline grayscale penuh termasuk window/level, Modality LUT, VOI LUT, dan rendering overlay. Simpan ke JPG menggunakan perpustakaan imaging .NET apa pun." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Pipeline Rendering Gambar DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> merender gambar DICOM melalui pipeline grayscale standar DICOM yang didefinisikan dalam spesifikasi DICOM. Proses rendering menerapkan rangkaian Look-Up Tables (LUTs) untuk mengubah nilai piksel mentah yang disimpan menjadi gambar yang dapat ditampilkan:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; mengonversi nilai piksel yang disimpan menjadi nilai independen produsen menggunakan Rescale Slope/Intercept atau LUT Sequence.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; menerapkan window/level (Window Center dan Window Width) untuk memilih rentang nilai yang akan ditampilkan, dengan dukungan untuk transformasi Linear, Linear Exact, dan Sigmoid.</li>
<li><strong>Presentation LUT</strong> &mdash; memetakan nilai akhir ke P-Values yang dapat ditampilkan, termasuk dukungan untuk INVERSE (pembalikan gambar).</li>
<li><strong>Output LUT</strong> &mdash; mengonversi nilai grayscale ke RGB untuk tampilan, dengan pewarnaan opsional menggunakan peta warna khusus atau tabel Palette Color.</li>
</ol>

<p>Output yang dirender adalah <code>IRawImage</code> &mdash; sebuah buffer piksel BGRA32 (8-bit per saluran) dengan <code>Width</code>, <code>Height</code>, dan array <code>Pixels</code>. Anda kemudian dapat menyimpan data piksel ini ke JPG menggunakan perpustakaan imaging .NET apa pun seperti <code>System.Drawing</code>, <code>SkiaSharp</code>, atau <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render DICOM ke Data Piksel dalam C#">}}

<p>Muat file DICOM dan render sebuah frame ke <code>IRawImage</code> yang berisi data piksel BGRA32:</p>

<div class="codeblock" id="code">
 <h3>Render gambar DICOM - C#</h3>
 <pre><code class="cs">using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;

// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Render the first frame (index 0) through the full LUT pipeline
IRawImage image = dicomFile.RenderImage(0);

// Access the rendered pixel data
int width = image.Width;
int height = image.Height;
Bgra32[] pixels = image.Pixels; // BGRA32 pixel buffer</code></pre>
</div>

<p>Metode <code>RenderImage</code> secara otomatis menerapkan pipeline grayscale penuh menggunakan nilai window/level dan parameter LUT yang disimpan dalam file DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Simpan Gambar yang Dirender sebagai JPG">}}

<p><code>IRawImage</code> menyediakan data piksel BGRA32 mentah yang dapat disimpan ke JPG menggunakan perpustakaan imaging .NET apa pun. Berikut contoh menggunakan <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Simpan frame DICOM yang dirender sebagai JPG - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");
IRawImage image = dicomFile.RenderImage(0);

// Create a Bitmap from the BGRA32 pixel data
using var bitmap = new Bitmap(image.Width, image.Height, PixelFormat.Format32bppArgb);
var bitmapData = bitmap.LockBits(
    new Rectangle(0, 0, image.Width, image.Height),
    ImageLockMode.WriteOnly,
    PixelFormat.Format32bppArgb);

// Copy BGRA32 pixels to the bitmap
MemoryMarshal.AsBytes(image.Pixels.AsSpan())
    .CopyTo(new Span&lt;byte&gt;(
        (void*)bitmapData.Scan0,
        bitmapData.Stride * bitmapData.Height));

bitmap.UnlockBits(bitmapData);

// Save as JPG
bitmap.Save("ct_scan.jpg", ImageFormat.Jpeg);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Window/Level Kustom (Windowing)">}}

<p>Window/level (juga disebut windowing atau W/L) adalah parameter paling penting untuk rendering gambar DICOM. Ini mengontrol rentang nilai piksel mana yang dipetakan ke rentang grayscale yang terlihat. <strong>Window Center</strong> menentukan titik tengah dan <strong>Window Width</strong> menentukan rentang nilai yang ditampilkan:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; kontras tinggi, lebih sedikit level abu-abu yang ditampilkan (berguna untuk jaringan lunak).</li>
<li><strong>Wide window</strong> &mdash; kontras rendah, lebih banyak level abu-abu yang ditampilkan (berguna untuk tulang atau paru).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Render dengan window/level kustom - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window: center=40, width=400
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window: center=400, width=1800
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Lung window: center=-600, width=1500
var lung = new GrayscaleRenderOptions
{
    WindowCenter = -600,
    WindowWidth = 1500
};
IRawImage lungImage = dicomFile.RenderImage(lung, 0);</code></pre>
</div>

<p>Preset window CT umum:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tipe Jaringan</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Jaringan Lunak</td><td>40</td><td>400</td></tr>
<tr><td>Paru</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Tulang</td><td>400</td><td>1800</td></tr>
<tr><td>Otak</td><td>40</td><td>80</td></tr>
<tr><td>Hati</td><td>60</td><td>150</td></tr>
<tr><td>Mediastinum</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opsi Render Grayscale">}}

<p>Kelas <code>GrayscaleRenderOptions</code> menyediakan kontrol penuh atas pipeline rendering grayscale:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Properti</th>
<th>Deskripsi</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Pusat rentang nilai yang ditampilkan (titik tengah horizontal VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Lebar rentang nilai yang ditampilkan (mengontrol kontras)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Rescale slope Modality LUT &mdash; dikalikan dengan nilai piksel yang disimpan</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Rescale intercept Modality LUT &mdash; ditambahkan setelah perkalian slope</td></tr>
<tr><td><code>Invert</code></td><td>Membalik gambar grayscale (menerapkan bentuk INVERSE Presentation LUT)</td></tr>
<tr><td><code>BitDepth</code></td><td>Informasi kedalaman bit: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Rendering grayscale terbalik - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray.dcm");

// Render with inverted grayscale (white-on-black becomes black-on-white)
var options = new GrayscaleRenderOptions
{
    WindowCenter = 2048,
    WindowWidth = 4096,
    Invert = true
};

IRawImage image = dicomFile.RenderImage(options, 0);</code></pre>
</div>

<p>Gunakan <code>RenderOptionsFactory.Create(pixelData)</code> untuk secara otomatis mengisi opsi render dari atribut data piksel dataset DICOM, termasuk kedalaman bit, parameter rescale, window/level, dan pengaturan inversi.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendering Overlay">}}

<p>Gambar DICOM dapat berisi overlay plane — grafik atau anotasi teks yang tertanam pada gambar. Kelas <code>RenderOptions</code> mengontrol apakah overlay dirender dan warna apa yang ditampilkan:</p>

<div class="codeblock" id="code">
 <h3>Render dengan kontrol overlay - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible in white (default behavior)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions
{
    ShowOverlays = false
};
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

<p>DICOM mendukung dua jenis overlay: overlay <strong>Graphics</strong> (anotasi, pengukuran) dan overlay <strong>Region of Interest</strong> (ROI). Kedua jenis dikontrol oleh pengaturan <code>ShowOverlays</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendering DICOM Multi-Frame">}}

<p>Banyak file DICOM dari CT, MRI, dan ultrasonik berisi banyak frame (iris). Anda dapat memeriksa jumlah frame dan merender masing-masing secara terpisah:</p>

<div class="codeblock" id="code">
 <h3>Render semua frame dari DICOM multi-frame - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;
Console.WriteLine($"Total frames: {totalFrames}");

// Render all frames to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as JPG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Jenis Transformasi VOI LUT">}}

<p>Standar DICOM mendefinisikan beberapa fungsi transformasi VOI LUT yang mengontrol cara penerapan pemetaan window/level. Aspose.Medical untuk .NET mendukung semua tipe standar:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Jenis</th>
<th>Deskripsi</th>
<th>Kasus Penggunaan</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Pemetaan linear standar dengan offset dari pusat</td><td>Paling umum, digunakan untuk pencitraan CT/MR umum</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Pemetaan linear tanpa offset dari pusat</td><td>Ketika pemetaan nilai piksel yang tepat diperlukan</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>Transformasi kurva S untuk transisi kontras yang lebih halus</td><td>Visualisasi jaringan lunak, mammografi</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>LUT khusus yang didefinisikan sebagai tabel pencarian dalam dataset DICOM</td><td>Kurva tampilan khusus vendor atau modality</td></tr>
</tbody>
</table>

<p>Mesin rendering secara otomatis memilih transformasi VOI LUT yang tepat berdasarkan atribut dataset DICOM. Saat menggunakan <code>GrayscaleRenderOptions</code> kustom, transformasi Linear diterapkan secara default.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Modality yang Didukung">}}

<p>Aspose.Medical untuk .NET mendukung rendering gambar DICOM dari semua modality pencitraan medis umum:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modality</th>
<th>Deskripsi</th>
<th>Format Umum</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Tomografi Terkomputasi</td><td>Grayscale, multi-frame, 12&ndash;16 bit</td></tr>
<tr><td><strong>MR</strong></td><td>Resonansi Magnetik</td><td>Grayscale, multi-frame, 12&ndash;16 bit</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Radiografi Komputasi / Digital</td><td>Grayscale, single frame, 10&ndash;16 bit</td></tr>
<tr><td><strong>US</strong></td><td>Ultrasonik</td><td>Grayscale atau RGB, multi-frame</td></tr>
<tr><td><strong>MG</strong></td><td>Mammografi</td><td>Grayscale, high resolution, 12&ndash;16 bit</td></tr>
<tr><td><strong>XA</strong></td><td>Angiografi Sinar-X</td><td>Grayscale, multi-frame</td></tr>
<tr><td><strong>NM</strong></td><td>Medis Nuklir</td><td>Grayscale, multi-frame</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Tomografi Emisi Positron)</td><td>Grayscale, multi-frame</td></tr>
</tbody>
</table>

<p>Kedua interpretasi fotometrik grayscale (MONOCHROME1/MONOCHROME2) dan warna (RGB, YBR_FULL, PALETTE COLOR) didukung. Mesin rendering secara otomatis menangani konversi ruang warna yang tepat.</p>

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
