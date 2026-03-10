---
title: Convertir DICOM a TIFF en C# .NET | Aspose.Medical
weight: 12000
description: Renderizar imágenes DICOM a datos de píxeles en C# .NET y guardar en TIFF con soporte de varias páginas para archivos DICOM multicuadro. Utilice la canalización completa DICOM LUT con la API Aspose.Medical y guarde en TIFF usando cualquier biblioteca de imágenes .NET.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convertir DICOM a TIFF en .NET C#" h2="Renderizar imágenes DICOM a datos de píxeles crudos con soporte completo de la canalización en escala de grises. Preserve la calidad de la imagen con control de ventana/nivel, renderizado de superposiciones y procesamiento multicuadro. Guardar en TIFF &mdash; incluidos TIFF de varias páginas &mdash; usando cualquier biblioteca de imágenes .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Conversión de DICOM a TIFF">}}

<p><strong>Aspose.Medical for .NET</strong> renderiza imágenes DICOM a través de la canalización estándar DICOM en escala de grises (Modality LUT, VOI LUT, Presentation LUT, Output LUT) para producir datos de píxeles BGRA32 correctamente ventana. TIFF es un formato versátil ampliamente usado en imágenes médicas, investigación científica y publicación porque admite compresión sin pérdida, profundidades de bits altas y &mdash; lo más importante &mdash; <strong>documentos multipágina</strong>. Esto hace que TIFF sea la elección natural para convertir archivos DICOM multicuadro (CT, MRI, ultrasonido) en un único archivo de salida que preserve todas las secciones.</p>

<p>La salida renderizada es un <code>IRawImage</code> &mdash; un búfer de píxeles BGRA32 con <code>Width</code>, <code>Height</code> y una matriz <code>Pixels</code>. Puede guardar estos datos de píxeles en TIFF usando cualquier biblioteca de imágenes .NET como <code>System.Drawing</code>, <code>SkiaSharp</code> o <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderizar DICOM a datos de píxeles en C#">}}

<p>Cargue un archivo DICOM, renderice el cuadro deseado y acceda a los datos de píxeles:</p>

<div class="codeblock" id="code">
 <h3>Renderizar imagen DICOM - C#</h3>
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

<p>El método <code>RenderImage</code> aplica automáticamente los valores de ventana/nivel y los parámetros LUT almacenados en el conjunto de datos DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Guardar imagen renderizada como TIFF">}}

<p>El <code>IRawImage</code> proporciona datos de píxeles BGRA32 crudos que pueden guardarse en TIFF usando cualquier biblioteca de imágenes .NET. Aquí hay un ejemplo usando <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Guardar cuadro DICOM renderizado como TIFF - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF multipágina a partir de DICOM multicuadro">}}

<p>Los archivos DICOM multicuadro (p. ej., series de CT o MRI) contienen múltiples secciones de imagen en un solo archivo. La capacidad multipágina de TIFF le permite almacenar todos los cuadros en un único archivo de salida, preservando el estudio completo en un formato universalmente legible. Use el codificador TIFF de <code>System.Drawing</code> con parámetros multicuadro:</p>

<div class="codeblock" id="code">
 <h3>Convertir DICOM multicuadro a TIFF multipágina - C#</h3>
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

<p>También puede guardar cada cuadro como un archivo TIFF separado cuando se necesitan secciones individuales:</p>

<div class="codeblock" id="code">
 <h3>Convertir cada cuadro a un TIFF separado - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Ventana/Nivel personalizada">}}

<p>Controle qué rango de valores de píxel se renderiza configurando el Centro de ventana y el Ancho de ventana. Diferentes configuraciones de ventana revelan distintas estructuras anatómicas a partir de los mismos datos DICOM:</p>

<div class="codeblock" id="code">
 <h3>Renderizar con ventana/nivel personalizada - C#</h3>
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

<p>La clase <code>GrayscaleRenderOptions</code> también expone las propiedades <code>RescaleSlope</code>, <code>RescaleIntercept</code>, <code>Invert</code> y <code>BitDepth</code> para un control total sobre la canalización de renderizado.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderizado de DICOM multicuadro">}}

<p>Los archivos DICOM de CT, MRI y ultrasonido a menudo contienen múltiples cuadros (secciones). Renderice todos los cuadros a datos de píxeles:</p>

<div class="codeblock" id="code">
 <h3>Renderizar todos los cuadros de DICOM multicuadro - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Control de superposición">}}

<p>Las imágenes DICOM pueden contener planos de superposición con anotaciones, mediciones o regiones de interés. Controle la visibilidad y el color de la superposición al renderizar:</p>

<div class="codeblock" id="code">
 <h3>Renderizar con y sin superposiciones - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="TIFF vs PNG vs JPG para imágenes médicas">}}

<p>Cada formato de salida sirve a diferentes casos de uso:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Característica</th>
<th>TIFF</th>
<th>PNG</th>
<th>JPG</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Compresión</strong></td><td>Sin pérdida (LZW, ZIP) o sin comprimir</td><td>Sin pérdida</td><td>Con pérdida</td></tr>
<tr><td><strong>Multipágina</strong></td><td>Compatible &mdash; todos los cuadros en un archivo</td><td>No compatible</td><td>No compatible</td></tr>
<tr><td><strong>Calidad de imagen</strong></td><td>Precisión de píxel</td><td>Precisión de píxel</td><td>Posibles artefactos de compresión</td></tr>
<tr><td><strong>Tamaño de archivo</strong></td><td>Más grande</td><td>Mediano</td><td>Más pequeño</td></tr>
<tr><td><strong>Transparencia</strong></td><td>Compatible</td><td>Compatible</td><td>No compatible</td></tr>
<tr><td><strong>Mejor para</strong></td><td>Archivo, series multicuadro, impresión, investigación</td><td>Revisión diagnóstica, web con calidad</td><td>Compartir en web, miniaturas</td></tr>
</tbody>
</table>

<p>Aspose.Medical for .NET usa la misma canalización de renderizado para todos los destinos de salida. Los datos de píxeles del <code>IRawImage</code> son idénticos independientemente del formato de destino &mdash; la elección del formato solo afecta el paso final de guardado realizado por su biblioteca de imágenes. TIFF es el formato preferido cuando necesita preservar un estudio DICOM multicuadro completo en un solo archivo o cuando la salida está destinada a archivado, impresión o procesamiento de imágenes adicional.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Código fuente" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referencias de API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Soporte del producto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Soporte gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Soporte de pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="¿Por qué Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Historias de éxito" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
