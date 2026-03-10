---
title: Konversi DICOM ke PNG dalam C# .NET | Aspose.Medical
weight: 11000
description: Render gambar DICOM ke data piksel dalam C# .NET dengan kualitas lossless, kontrol window/level, dan dukungan multi-frame. Gunakan pipeline LUT DICOM lengkap dengan Aspose.Medical API dan simpan sebagai PNG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konversi DICOM ke PNG dalam .NET C#" h2="Render gambar DICOM ke data piksel mentah dengan dukungan pipeline grayscale lengkap. Jaga kualitas gambar dengan kontrol window/level, rendering overlay, dan pemrosesan multi-frame. Simpan ke PNG lossless menggunakan perpustakaan imaging .NET apa pun." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Konversi DICOM ke PNG">}}

<p><strong>Aspose.Medical for .NET</strong> merender gambar DICOM melalui pipeline grayscale standar DICOM (Modality LUT, VOI LUT, Presentation LUT, Output LUT) untuk menghasilkan data piksel BGRA32 yang terwindow dengan benar. PNG adalah format lossless, menjadikannya pilihan utama ketika kualitas gambar harus dipertahankan tanpa artefak kompresi &mdash; penting untuk peninjauan diagnostik, arsip, dan penggunaan riset.</p>

<p>Output yang dirender adalah sebuah <code>IRawImage</code> &mdash; buffer piksel BGRA32 dengan <code>Width</code>, <code>Height</code>, dan array <code>Pixels</code>. Anda dapat menyimpan data piksel ini ke PNG menggunakan perpustakaan imaging .NET apa pun seperti <code>System.Drawing</code>, <code>SkiaSharp</code>, atau <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render DICOM ke Data Piksel dalam C#">}}

<p>Muat file DICOM, render frame yang diinginkan, dan akses data pikelnya:</p>

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

<p>Metode <code>RenderImage</code> secara otomatis menerapkan nilai window/level dan parameter LUT yang disimpan dalam dataset DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Simpan Gambar yang Dirender sebagai PNG">}}

<p><code>IRawImage</code> menyediakan data piksel BGRA32 mentah yang dapat disimpan ke PNG lossless menggunakan perpustakaan imaging .NET apa pun. Berikut contoh menggunakan <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Simpan frame DICOM yang dirender sebagai PNG - C#</h3>
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

// Save as lossless PNG
bitmap.Save("ct_scan.png", ImageFormat.Png);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Window/Level Kustom">}}

<p>Kontrol rentang nilai piksel yang dirender dengan mengatur Window Center dan Window Width. Pengaturan window yang berbeda mengungkap struktur anatomi yang berbeda dari data DICOM yang sama:</p>

<div class="codeblock" id="code">
 <h3>Render dengan window/level kustom - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_scan.dcm");

// Soft tissue window
var softTissue = new GrayscaleRenderOptions
{
    WindowCenter = 40,
    WindowWidth = 400
};
IRawImage softTissueImage = dicomFile.RenderImage(softTissue, 0);

// Bone window
var bone = new GrayscaleRenderOptions
{
    WindowCenter = 400,
    WindowWidth = 1800
};
IRawImage boneImage = dicomFile.RenderImage(bone, 0);</code></pre>
</div>

<p>Kelas <code>GrayscaleRenderOptions</code> juga mengekspos properti <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code>, dan <code>BitDepth</code> untuk kontrol penuh atas pipeline render.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendering DICOM Multi-Frame">}}

<p>File DICOM dari CT, MRI, dan ultrasonik sering berisi banyak frame (slice). Render semua frame ke data piksel:</p>

<div class="codeblock" id="code">
 <h3>Render semua frame dari DICOM multi-frame - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as PNG using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kontrol Overlay">}}

<p>Gambar DICOM mungkin berisi plane overlay dengan anotasi, pengukuran, atau wilayah minat. Kontrol visibilitas dan warna overlay saat render:</p>

<div class="codeblock" id="code">
 <h3>Render dengan dan tanpa overlay - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("xray_with_overlays.dcm");

// Render with overlays visible (default)
var withOverlays = new RenderOptions
{
    ShowOverlays = true,
    OverlayColor = Bgra32.White
};
IRawImage imageWithOverlays = dicomFile.RenderImage(withOverlays, 0);

// Render without overlays for a clean image
var noOverlays = new RenderOptions { ShowOverlays = false };
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="PNG vs JPG untuk Gambar Medis">}}

<p>Memilih antara PNG dan JPG tergantung pada kasus penggunaan:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Karakteristik</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Kompresi</strong></td><td>Lossless</td><td>Lossy</td></tr>
<tr><td><strong>Kualitas Gambar</strong></td><td>Pixel-perfect &mdash; tanpa artefak</td><td>Mungkin ada artefak kompresi minor</td></tr>
<tr><td><strong>Ukuran File</strong></td><td>Larger (typically 2&ndash;10x vs JPG)</td><td>Smaller</td></tr>
<tr><td><strong>Transparansi</strong></td><td>Supported (alpha channel)</td><td>Not supported</td></tr>
<tr><td><strong>Terbaik Untuk</strong></td><td>Diagnostic review, archival, research, overlays</td><td>Web sharing, thumbnails, previews</td></tr>
<tr><td><strong>Regulasi</strong></td><td>Preferred for quality-critical workflows</td><td>Acceptable for non-diagnostic use</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET menggunakan pipeline rendering yang sama untuk kedua target output. Data piksel <code>IRawImage</code> identik terlepas dari format target &mdash; pilihan format hanya memengaruhi langkah penyimpanan akhir yang dilakukan oleh perpustakaan imaging Anda.</p>

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
