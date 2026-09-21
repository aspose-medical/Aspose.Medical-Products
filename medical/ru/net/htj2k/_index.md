---
title: HTJ2K в C# .NET - High-Throughput JPEG 2000 для DICOM | Aspose.Medical
weight: 10000

description: Сжимайте и читайте DICOM‑изображения в High-Throughput JPEG 2000 из C#. Lossless HTJ2K, вариант RPCL и lossy HTJ2K, реализованные в управляемом .NET без необходимости в нативном кодеке.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K в .NET C#" h2="High-Throughput JPEG 2000 для DICOM: сжатие, добавленное в стандарт для быстрого архивирования и облачного просмотра, реализованное в управляемом C# без установки нативных компонентов." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Что меняет HTJ2K">}}

<p>High-Throughput JPEG 2000 сохраняет вейвлет и качество изображения JPEG 2000 и заменяет часть, делавшую его медленным. Новый блоковый кодер, а декодирование в разы быстрее, поэтому стандарт DICOM принял его в трёх синтаксах передачи и облачные платформы перешли на него.</p>

<p>Для команды .NET практический вопрос иной: кто действительно может генерировать такие файлы. Большинство библиотек получают HTJ2K через нативную сборку OpenJPH, что подразумевает бинарник для каждой платформы, шаг сборки в контейнере и зависимость, о которой будет спрашивать проверка безопасности. <strong>Aspose.Medical for .NET</strong> реализует кодек в управляемом коде внутри того же пакета, который читает и записывает файлы, поэтому HTJ2K работает одинаково на Windows, на Linux и в контейнере, без необходимости установки чего‑нибудь.</p>

<p>Поддерживаются три синтакса передачи, и все три поддерживают как чтение, так и запись:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), High-Throughput JPEG 2000 lossless.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), lossless‑вариант с порядком прогрессии RPCL.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Сжать исследование в HTJ2K">}}

<p>Одним вызовом файл переводится в новый синтакс. Набор данных, приватные теги и мета‑информация файла перемещаются вместе с ним.</p>

<div class="codeblock" id="code">
 <h3>Транскодировать DICOM‑файл в HTJ2K — C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>Для 16‑битного изображения 1714×1933 из нашего тестового набора размер файла уменьшается с 6,3 МБ до 2,9 МБ, а пиксели восстанавливаются бит в бит. Значения различаются в зависимости от модальности и изображения, поэтому измеряйте на своих данных, выполнив один проход по имеющимся файлам.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossless означает lossless">}}

<p>Диагностические данные не терпят кодек, который лишь почти верен. Транскодируйте в HTJ2K lossless и обратно, и пиксельные данные будут идентичны исходным байтам, что можно проверять в собственном наборе тестов перед согласием на повторное сжатие архива.</p>

<div class="codeblock" id="code">
 <h3>Возврат к несжатому синтаксу — C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, вариант, созданный для просмотра по сети">}}

<p>Синтакс 1.2.840.10008.1.2.4.202 сохраняет тот же lossless‑поток кодеков в порядке прогрессии RPCL: сначала разрешение, затем позиция, затем компонент, затем слой. Читатель, получающий только начало потока, получает полное изображение низкого разрешения, что необходимо просмотровщику при открытии большого исследования по неконтролируемой ссылке.</p>

<div class="codeblock" id="code">
 <h3>Сжать с порядком прогрессии RPCL — C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Читать то, что отправляет архив">}}

<p>Вторая часть задачи — принимать HTJ2K от систем, которые уже его генерируют. Откройте файл, проверьте, как он хранится, и работайте с пиксельными данными.</p>

<div class="codeblock" id="code">
 <h3>Читать HTJ2K‑файл — C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>Многокадровые изображения обрабатываются кадр за кадром, поэтому длинная серия требует памяти на каждый кадр, а не на всё исследование.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Где HTJ2K заслуживает своё место">}}

<ul>
<li>Миграция архива: повторно сжать хранимое исследование в HTJ2K lossless, уменьшить объём, сохранить диагностические данные без изменений.</li>
<li>Облако и DICOMweb: скорость декодирования делает просмотр в браузере или на сервере ощущаемо мгновенным для больших изображений.</li>
<li>AI‑конвейеры: наборы обучения читаются гораздо чаще, чем записываются, и время декодирования – повторяющаяся стоимость.</li>
<li>Контейнеры и serverless: кодек включён в сборку, поэтому образу не нужна нативная библиотека или компилятор при сборке.</li>
</ul>

<p>Библиотека также поставляется с JPEG XL, другим недавним дополнением к стандарту, а также со старыми кодеками, которые может содержать архив: JPEG, JPEG‑LS, JPEG 2000 и RLE. Страница <a href="/medical/net/dicom-transfer-syntax-conversion/">конвертации синтаксов передачи</a> охватывает весь набор, а страница <a href="/medical/net/jpeg2000/">JPEG 2000</a> описывает кодек, из которого вырос HTJ2K.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Учебные ресурсы" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Руководство разработчика" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="Справочники API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Поддержка продукта" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Бесплатная поддержка" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Платная поддержка" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Блог" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Почему Aspose.Medical для .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Список клиентов" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Истории успеха" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
