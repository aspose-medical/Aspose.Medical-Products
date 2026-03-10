---
title: Konversi DICOM ke TIFF dalam C# .NET | Aspose.Medical
weight: 12000
description: Render gambar DICOM ke data piksel dalam C# .NET dan simpan ke TIFF dengan dukungan multi-halaman untuk file DICOM multi-frame. Gunakan pipeline LUT DICOM penuh dengan Aspose.Medical API dan simpan ke TIFF menggunakan pustaka gambar .NET apa pun.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Konversi DICOM ke TIFF dalam .NET C#" h2="Render gambar DICOM ke data piksel mentah dengan dukungan pipeline grayscale penuh. Pertahankan kualitas gambar dengan kontrol window/level, rendering overlay, dan pemrosesan multi-frame. Simpan ke TIFF &mdash; termasuk multi-page TIFF &mdash; menggunakan pustaka gambar .NET apa pun." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Konversi DICOM ke TIFF">}}

<p><strong>Aspose.Medical for .NET</strong> merender gambar DICOM melalui pipeline grayscale DICOM standar (Modality LUT, VOI LUT, Presentation LUT, Output LUT) untuk menghasilkan data piksel BGRA32 yang terwindow dengan benar. TIFF adalah format serbaguna yang banyak digunakan dalam pencitraan medis, penelitian ilmiah, dan penerbitan karena mendukung kompresi lossless, kedalaman bit tinggi, dan &mdash; yang paling penting &mdash; <strong>dokumen multi-halaman</strong>. Hal ini menjadikan TIFF pilihan alami untuk mengonversi file DICOM multi-frame (CT, MRI, ultrasonik) menjadi satu file output yang mempertahankan semua slice.</p>

<p>Output yang dirender adalah sebuah <code>IRawImage</code> &mdash; sebuah buffer piksel BGRA32 dengan <code>Width</code>, <code>Height</code>, dan array <code>Pixels</code>. Anda dapat menyimpan data piksel ini ke TIFF menggunakan pustaka gambar .NET apa pun seperti <code>System.Drawing</code>, <code>SkiaSharp</code>, atau <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Render DICOM ke Data Piksel dalam C#">}}

<p>Muat file DICOM, render frame yang diinginkan, dan akses data piksel:</p>

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

{{< blocks/products/pf/feature-page-section h2="Simpan Gambar yang Dirender sebagai TIFF">}}

<p><code>IRawImage</code> menyediakan data piksel BGRA32 mentah yang dapat disimpan ke TIFF menggunakan pustaka gambar .NET apa pun. Berikut contoh menggunakan <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Simpan frame DICOM yang dirender sebagai TIFF - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("xray.dcm");
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

// Save as TIFF
bitmap.Save("xray.tiff", ImageFormat.Tiff);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="TIFF Multi-Halaman dari DICOM Multi-Frame">}}

<p>File DICOM multi-frame (mis., seri CT atau MRI) berisi banyak slice gambar dalam satu file. Kemampuan multi-halaman TIFF memungkinkan Anda menyimpan semua frame dalam satu file output, mempertahankan studi lengkap dalam format yang dapat dibaca secara universal. Gunakan encoder TIFF <code>System.Drawing</code> dengan parameter multi-frame:</p>

<div class="codeblock" id="code">
 <h3>Konversi DICOM multi-frame ke TIFF multi-halaman - C#</h3>
 <pre><code class="cs">using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Imaging;
using Aspose.Medical.Imaging.PixelFormats;

DicomFile dicomFile = DicomFile.Open("ct_series.dcm");
int totalFrames = dicomFile.NumberOfFrames;

// Get the TIFF codec
var tiffCodec = ImageCodecInfo.GetImageEncoders()
    .First(c =&gt; c.FormatID == ImageFormat.Tiff.Guid);

// Multi-frame parameters
var multiFrameParams = new EncoderParameters(2);
multiFrameParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.MultiFrame);
multiFrameParams.Param[1] = new EncoderParameter(
    Encoder.Compression, (long)EncoderValue.CompressionLZW);

// Parameter for adding subsequent frames
var frameAddParams = new EncoderParameters(1);
frameAddParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.FrameDimensionPage);

// Parameter for flushing the final frame
var flushParams = new EncoderParameters(1);
flushParams.Param[0] = new EncoderParameter(
    Encoder.SaveFlag, (long)EncoderValue.Flush);

Bitmap? tiffBitmap = null;

for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var frameBitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = frameBitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    frameBitmap.UnlockBits(bitmapData);

    if (i == 0)
    {
        // Save the first frame and start multi-page TIFF
        tiffBitmap = (Bitmap)frameBitmap.Clone();
        tiffBitmap.Save("ct_series.tiff", tiffCodec, multiFrameParams);
    }
    else
    {
        // Add subsequent frames
        tiffBitmap!.SaveAdd(frameBitmap, frameAddParams);
    }
}

// Flush and close the multi-page TIFF
tiffBitmap?.SaveAdd(flushParams);
tiffBitmap?.Dispose();</code></pre>
</div>

<p>Anda juga dapat menyimpan setiap frame sebagai file TIFF terpisah ketika slice individu diperlukan:</p>

<div class="codeblock" id="code">
 <h3>Konversi setiap frame ke TIFF terpisah - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

for (int i = 0; i &lt; dicomFile.NumberOfFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);

    using var bitmap = new Bitmap(
        image.Width, image.Height, PixelFormat.Format32bppArgb);
    var bitmapData = bitmap.LockBits(
        new Rectangle(0, 0, image.Width, image.Height),
        ImageLockMode.WriteOnly,
        PixelFormat.Format32bppArgb);

    MemoryMarshal.AsBytes(image.Pixels.AsSpan())
        .CopyTo(new Span&lt;byte&gt;(
            (void*)bitmapData.Scan0,
            bitmapData.Stride * bitmapData.Height));

    bitmap.UnlockBits(bitmapData);
    bitmap.Save($"frame_{i:D4}.tiff", ImageFormat.Tiff);
}</code></pre>
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
IRawImage boneImage = dicomFile.RenderImage(bone, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

<p>Kelas <code>GrayscaleRenderOptions</code> juga mengekspos properti <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code>, dan <code>BitDepth</code> untuk kontrol penuh atas pipeline rendering.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Rendering DICOM Multi-Frame">}}

<p>File DICOM dari CT, MRI, dan ultrasonik sering kali berisi banyak frame (slice). Render semua frame ke data piksel:</p>

<div class="codeblock" id="code">
 <h3>Render semua frame dari DICOM multi-frame - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("ct_series.dcm");

int totalFrames = dicomFile.NumberOfFrames;

// Render each frame to pixel data
for (int i = 0; i &lt; totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Frame {i}: {image.Width}x{image.Height}");

    // Save each frame as TIFF using your preferred imaging library
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kontrol Overlay">}}

<p>Gambar DICOM mungkin berisi bidang overlay dengan anotasi, pengukuran, atau wilayah minat. Kontrol visibilitas dan warna overlay saat rendering:</p>

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
IRawImage cleanImage = dicomFile.RenderImage(noOverlays, 0);

// Save each rendered image to TIFF using your preferred imaging library</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG untuk Gambar Medis">}}

<p>Setiap format output melayani kasus penggunaan yang berbeda:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Karakteristik</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Kompresi</strong></td><td>Lossless (LZW, ZIP) atau tidak terkompresi</td><td>Lossless</td><td>Lossy</td></tr>
<tr><td><strong>Multi-Halaman</strong></td><td>Didukung &mdash; semua frame dalam satu file</td><td>Tidak didukung</td><td>Tidak didukung</td></tr>
<tr><td><strong>Kualitas Gambar</strong></td><td>Pixel-perfect</td><td>Pixel-perfect</td><td>Kemungkinan artefak kompresi</td></tr>
<tr><td><strong>Ukuran File</strong></td><td>Terbesar</td><td>Sedang</td><td>Terkecil</td></tr>
<tr><td><strong>Transparansi</strong></td><td>Didukung</td><td>Didukung</td><td>Tidak didukung</td></tr>
<tr><td><strong>Terbaik Untuk</strong></td><td>Arsip, seri multi-frame, cetak, penelitian</td><td>Review diagnostik, web dengan kualitas</td><td>Berbagi web, thumbnail</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET menggunakan pipeline rendering yang sama untuk semua target output. Data piksel <code>IRawImage</code> identik terlepas dari format target — pilihan format hanya memengaruhi langkah penyimpanan akhir yang dilakukan oleh pustaka gambar Anda. TIFF adalah format yang disarankan ketika Anda perlu mempertahankan studi DICOM multi-frame lengkap dalam satu file atau ketika output ditujukan untuk arsip, pencetakan, atau pemrosesan gambar lebih lanjut.</p>

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
