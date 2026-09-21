---
title: JPEG XL для DICOM на C# .NET | Aspose.Medical
weight: 10500

description: Сохраняйте DICOM‑изображения в JPEG XL из C#. Lossless JPEG XL, возвращающий пиксели бит‑по‑бит, в одной управляемой сборке без нативного кодека для развертывания.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL для DICOM в .NET C#" h2="Самый современный метод сжатия в стандарте DICOM, дающий самые маленькие lossless‑файлы по нашим измерениям, реализованный на управляемом C# и поставляемый в одной сборке." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Почему JPEG XL попал в DICOM">}}

<p>Медицинские архивы только растут и никогда не уменьшаются. JPEG XL — это кодек, разработанный миром визуализации после двух десятилетий опыта с JPEG и JPEG 2000, и DICOM включил его как синтаксис передачи по причине, важной для команд хранения: при одинаковых пикселях файл меньше.</p>

<p><strong>Aspose.Medical for .NET</strong> записывает и читает JPEG XL через порт libjxl на C#, встроенный в библиотеку. Пакет поставляется в виде одной сборки, <code>Aspose.Medical.dll</code>, без нативного бинарника, поэтому такой новый кодек не превращается в отдельный проект развертывания: эта же сборка работает на Windows, Linux, в сборочном агенте и в контейнере.</p>

<p>Два синтакса передачи несут пиксели:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), для диагностических данных, которые должны возвращаться без изменений.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), для случаев, когда важнее меньший размер файла, чем точная копия.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Сжатие исследования, сохранение каждого пикселя">}}

<p>Транскодирование – это один вызов, а набор данных, окружающий пиксели, передаётся вместе с ним.</p>

<div class="codeblock" id="code">
 <h3>Транскодировать DICOM‑файл в JPEG XL — C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>Мы измерили на изображении 1714×1933, 16‑бит из нашего тестового набора: 6,3 МБ несжатого объёма становятся 2,7 МБ в JPEG XL lossless, что меньше, чем тот же образ в HTJ2K lossless. Ваши результаты зависят от модальности, поэтому выполните сравнение над папкой ваших файлов перед выбором.</p>

<p>Lossless – здесь слово, которое следует воспринимать буквально. Транскодируйте в JPEG XL и обратно, и данные пикселей совпадают с исходными байтами, поэтому архив можно перекомпрессировать без обсуждения диагностического качества.</p>

<div class="codeblock" id="code">
 <h3>Вернуться к несжатому синтаксису — C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Чтение уже сохранённого в JPEG XL">}}

<p>Файл, полученный в JPEG XL, открывается как любой другой. Синтаксис передачи указывает его тип, и данные пикселей становятся доступны после декодирования кадра.</p>

<div class="codeblock" id="code">
 <h3>Открыть файл JPEG XL — C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL или HTJ2K">}}

<p>Оба новы, оба поддерживают lossless при запросе lossless, и библиотека записывает и читает их. Они отвечают на разные вопросы.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Вопрос</th>
<th>Ответ</th>
</tr>
</thead>
<tbody>
<tr><td>Какой создал меньший файл в нашем тесте</td><td>JPEG XL lossless, на несколько процентов</td></tr>
<tr><td>Какой предназначен для прогрессивного просмотра по сети</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, особенно вариант RPCL</td></tr>
<tr><td>Каким первым вошёл в стандарт DICOM</td><td>HTJ2K, поэтому сегодня его поддерживает больше архивов</td></tr>
<tr><td>Какой требует нативную зависимость</td><td>Ни один, оба реализованы как управляемый код в одной сборке</td></tr>
</tbody>
</table>

<p>Выбор обычно определяется с другой стороны ссылки: транскодировать в синтаксис, принимаемый архивом, и оставить остальные части конвейера без изменений.</p>

<div class="codeblock" id="code">
 <h3>Позвольте целевому архиву решить — C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Где это окупается">}}

<ul>
<li>Долгосрочные архивы: те же исследования, меньше терабайт, и отсутствие потерь, оправдывающееся перед радиологом.</li>
<li>Счета за облачное хранение: экономия повторяется каждый месяц, а транскодирование выполняется один раз.</li>
<li>Наборы данных для исследований и ИИ: меньшие копии быстрее перемещаются между хранилищем и обучением.</li>
<li>Развёртывание: такой новый кодек обычно требует нативной сборки под каждую платформу; здесь он входит в сборку, которую вы уже подключаете.</li>
</ul>

<p>Библиотека также записывает кодеки, которыми полон существующий архив: JPEG, JPEG‑LS, JPEG 2000, HTJ2K и RLE. Страница <a href="/medical/net/dicom-transfer-syntax-conversion/">конвертации синтаксиса передачи</a> охватывает весь набор, у <a href="/medical/net/htj2k/">HTJ2K</a> есть отдельная страница, а <a href="/medical/net/jpeg2000/">JPEG 2000</a> — источник обоих новых кодеков.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Обучающие ресурсы" tabId="resources" >}}
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
