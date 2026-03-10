---
title: Convertir DICOM a JPG en C# .NET | Aspose.Medical
weight: 7000
description: Renderizar imágenes DICOM a datos de píxeles en C# .NET con control de ventana/nivel, renderizado de superposiciones y soporte multicuadro. Utilice la canalización completa de LUT DICOM con la API Aspose.Medical y guarde a JPG.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Convertir DICOM a JPG en .NET C#" h2="Renderizar imágenes DICOM a datos de píxeles sin procesar con soporte completo de la canalización en escala de grises, incluyendo ventana/nivel, LUT de modalidad, LUT VOI y renderizado de superposiciones. Guardar a JPG usando cualquier biblioteca de imágenes .NET." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Canalización de Renderizado de Imágenes DICOM">}}

<p><strong>Aspose.Medical for .NET</strong> renderiza imágenes DICOM mediante la canalización estándar en escala de grises DICOM definida en la especificación DICOM. El proceso de renderizado aplica una cadena de Tablas de Consulta (LUTs) para transformar los valores de píxeles almacenados sin procesar en imágenes visibles:</p>

<ol>
<li><strong>Modality LUT</strong> &mdash; convierte los valores de píxeles almacenados a valores independientes del fabricante usando Rescale Slope/Intercept o una Secuencia LUT.</li>
<li><strong>VOI LUT (Value of Interest)</strong> &mdash; aplica ventana/nivel (Window Center y Window Width) para seleccionar el rango de valores a mostrar, con soporte para transformaciones Linear, Linear Exact y Sigmoid.</li>
<li><strong>Presentation LUT</strong> &mdash; asigna los valores finales a P-Values mostrables, incluyendo soporte para INVERSE (inversión de imagen).</li>
<li><strong>Output LUT</strong> &mdash; convierte valores en escala de grises a RGB para visualización, con colorización opcional usando mapas de color personalizados o tablas Palette Color.</li>
</ol>

<p>La salida renderizada es un <code>IRawImage</code> &mdash; un búfer de píxeles BGRA32 (8 bits por canal) con <code>Width</code>, <code>Height</code> y una matriz <code>Pixels</code>. Luego puede guardar estos datos de píxeles en JPG usando cualquier biblioteca de imágenes .NET como <code>System.Drawing</code>, <code>SkiaSharp</code> o <code>ImageSharp</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderizar DICOM a Datos de Píxeles en C#">}}

<p>Cargue un archivo DICOM y renderice un cuadro a un <code>IRawImage</code> que contiene datos de píxeles BGRA32:</p>

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

<p>El método <code>RenderImage</code> aplica automáticamente la canalización completa en escala de grises utilizando los valores de ventana/nivel y los parámetros LUT almacenados en el archivo DICOM.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Guardar Imagen Renderizada como JPG">}}

<p>El <code>IRawImage</code> proporciona datos de píxeles BGRA32 sin procesar que pueden guardarse en JPG usando cualquier biblioteca de imágenes .NET. Aquí hay un ejemplo usando <code>System.Drawing</code>:</p>

<div class="codeblock" id="code">
 <h3>Guardar cuadro DICOM renderizado como JPG - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Ventana/Nivel Personalizada (Windowing)">}}

<p>Ventana/nivel (también llamado windowing o W/L) es el parámetro más importante para el renderizado de imágenes DICOM. Controla qué rango de valores de píxel se asigna al rango visible de escala de grises. El <strong>Window Center</strong> define el punto medio y el <strong>Window Width</strong> define el rango de valores mostrados:</p>

<ul>
<li><strong>Narrow window</strong> &mdash; alto contraste, menos niveles de gris mostrados (útil para tejido blando).</li>
<li><strong>Wide window</strong> &mdash; bajo contraste, más niveles de gris mostrados (útil para hueso o pulmón).</li>
</ul>

<div class="codeblock" id="code">
 <h3>Renderizar con ventana/nivel personalizada - C#</h3>
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

<p>Preajustes de ventana comunes para CT:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tipo de Tejido</th>
<th>Window Center</th>
<th>Window Width</th>
</tr>
</thead>
<tbody>
<tr><td>Tejido Blando</td><td>40</td><td>400</td></tr>
<tr><td>Pulmón</td><td>&minus;600</td><td>1500</td></tr>
<tr><td>Hueso</td><td>400</td><td>1800</td></tr>
<tr><td>Cerebro</td><td>40</td><td>80</td></tr>
<tr><td>Hígado</td><td>60</td><td>150</td></tr>
<tr><td>Mediastino</td><td>50</td><td>350</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opciones de Renderizado en Escala de Grises">}}

<p>La clase <code>GrayscaleRenderOptions</code> brinda control total sobre la canalización de renderizado en escala de grises:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Propiedad</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr><td><code>WindowCenter</code></td><td>Centro del rango de valores mostrados (punto medio horizontal del VOI LUT)</td></tr>
<tr><td><code>WindowWidth</code></td><td>Anchura del rango de valores mostrados (controla el contraste)</td></tr>
<tr><td><code>RescaleSlope</code></td><td>Pendiente de reescalado del Modality LUT &mdash; multiplicado con los valores de píxeles almacenados</td></tr>
<tr><td><code>RescaleIntercept</code></td><td>Intercepto de reescalado del Modality LUT &mdash; añadido después de la multiplicación de la pendiente</td></tr>
<tr><td><code>Invert</code></td><td>Invierte la imagen en escala de grises (aplica la forma INVERSE del Presentation LUT)</td></tr>
<tr><td><code>BitDepth</code></td><td>Información de profundidad de bits: BitsAllocated, BitsStored, HighBit, IsSigned</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Renderizado de escala de grises invertida - C#</h3>
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

<p>Utilice <code>RenderOptionsFactory.Create(pixelData)</code> para poblar automáticamente las opciones de renderizado a partir de los atributos de datos de píxeles del conjunto de datos DICOM, incluyendo profundidad de bits, parámetros de reescalado, ventana/nivel y configuraciones de inversión.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderizado de Superposiciones">}}

<p>Las imágenes DICOM pueden contener planos de superposición &mdash; gráficos o anotaciones de texto quemados en la imagen. La clase <code>RenderOptions</code> controla si las superposiciones se renderizan y en qué color aparecen:</p>

<div class="codeblock" id="code">
 <h3>Renderizar con control de superposición - C#</h3>
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

<p>DICOM admite dos tipos de superposición: superposiciones <strong>Graphics</strong> (anotaciones, mediciones) y superposiciones <strong>Region of Interest</strong> (ROI). Ambos tipos se controlan mediante la configuración <code>ShowOverlays</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Renderizado DICOM Multi‑Cuadro">}}

<p>Muchos archivos DICOM de CT, MRI y ultrasonido contienen múltiples cuadros (rebanadas). Puede verificar la cantidad de cuadros y renderizar cada uno individualmente:</p>

<div class="codeblock" id="code">
 <h3>Renderizar todos los cuadros de DICOM multi‑cuadro - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Tipos de Transformación VOI LUT">}}

<p>El estándar DICOM define varias funciones de transformación VOI LUT que controlan cómo se aplica el mapeo ventana/nivel. Aspose.Medical for .NET soporta todos los tipos estándar:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Tipo</th>
<th>Descripción</th>
<th>Caso de Uso</th>
</tr>
</thead>
<tbody>
<tr><td><strong>Linear</strong></td><td>Mapeo lineal estándar con desplazamiento desde el centro</td><td>Más común, usado para imágenes CT/MR generales</td></tr>
<tr><td><strong>Linear Exact</strong></td><td>Mapeo lineal sin desplazamiento desde el centro</td><td>Cuando se requiere un mapeo preciso de valores de píxel</td></tr>
<tr><td><strong>Sigmoid</strong></td><td>Transformación en curva S para transiciones de contraste más suaves</td><td>Visualización de tejido blando, mamografía</td></tr>
<tr><td><strong>VOI LUT Sequence</strong></td><td>LUT personalizada definida como tabla de búsqueda en el conjunto de datos DICOM</td><td>Curvas de visualización específicas del fabricante o de la modalidad</td></tr>
</tbody>
</table>

<p>El motor de renderizado selecciona automáticamente la transformación VOI LUT apropiada basándose en los atributos del conjunto de datos DICOM. Al usar <code>GrayscaleRenderOptions</code> personalizadas, la transformación Linear se aplica por defecto.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Modalidades Compatibles">}}

<p>Aspose.Medical for .NET soporta el renderizado de imágenes DICOM de todas las modalidades de imagen médica comunes:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Modalidad</th>
<th>Descripción</th>
<th>Formato Típico</th>
</tr>
</thead>
<tbody>
<tr><td><strong>CT</strong></td><td>Tomografía Computarizada</td><td>Escala de grises, multi‑cuadro, 12&ndash;16 bits</td></tr>
<tr><td><strong>MR</strong></td><td>Resonancia Magnética</td><td>Escala de grises, multi‑cuadro, 12&ndash;16 bits</td></tr>
<tr><td><strong>CR / DX</strong></td><td>Radiografía Computarizada / Digital</td><td>Escala de grises, un solo cuadro, 10&ndash;16 bits</td></tr>
<tr><td><strong>US</strong></td><td>Ecografía</td><td>Escala de grises o RGB, multi‑cuadro</td></tr>
<tr><td><strong>MG</strong></td><td>Mamografía</td><td>Escala de grises, alta resolución, 12&ndash;16 bits</td></tr>
<tr><td><strong>XA</strong></td><td>Angiografía por Rayos X</td><td>Escala de grises, multi‑cuadro</td></tr>
<tr><td><strong>NM</strong></td><td>Medicina Nuclear</td><td>Escala de grises, multi‑cuadro</td></tr>
<tr><td><strong>PT</strong></td><td>PET (Tomografía por Emisión de Positrones)</td><td>Escala de grises, multi‑cuadro</td></tr>
</tbody>
</table>

<p>Se admiten tanto interpretaciones fotométricas en escala de grises (MONOCHROME1/MONOCHROME2) como en color (RGB, YBR_FULL, PALETTE COLOR). El motor de renderizado maneja automáticamente la conversión al espacio de color apropiado.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Recursos de Aprendizaje" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentación" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Código Fuente" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="Referencias API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Soporte del Producto" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Soporte Gratuito" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Soporte de Pago" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="¿Por qué Aspose.Medical para .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Lista de Clientes" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Casos de Éxito" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
