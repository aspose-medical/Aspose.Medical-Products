---
title: DICOMweb клиент на C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Поиск, загрузка и выгрузка DICOM‑исследований по HTTP из .NET. Чистый C# клиент DICOMweb для QIDO‑RS, WADO‑RS и STOW‑RS, который работает с PACS, VNA и облачными сервисами визуализации.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb клиент для .NET C#" h2="Искать, загружать и выгружать исследования на любом DICOMweb‑сервере с помощью QIDO‑RS, WADO‑RS и STOW‑RS, используя управляемый C# без необходимости установки чего‑либо на машину." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM через HTTP из собственного кода">}}

<p>DICOMweb — это HTTP‑вариант DICOM. Вместо ассоциации DIMSE на порту 104 приложение отправляет обычные HTTPS‑запросы: QIDO‑RS для поиска, WADO‑RS для загрузки, STOW‑RS для выгрузки. Облачные сервисы визуализации, такие как DICOM‑служба Azure Health Data Services и Google Cloud Healthcare API, работают таким образом, как и архивы Orthanc и dcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> включает DICOMweb клиент, начиная с версии 26.10. <code>DicomWebClient</code> инкапсулирует каждую стандартную транзакцию в типизированный метод, а результаты приходят в виде тех же объектов <code>Dataset</code> и <code>DicomFile</code>, которые использует остальная часть библиотеки для чтения и записи. Сервер с другой стороны может быть любой реализацией DICOMweb; он не обязан быть построен на Aspose.Medical.</p>

<p>Клиент реализован в управляемом C# поверх <code>HttpClient</code>. Нет нативного тулкита и прокси‑службы, поэтому тот же код работает в Windows, в Linux и в контейнере.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Подключение к DICOMweb серверу">}}

<p>Создайте клиент, используя <code>HttpClient</code> и корневой URL сервиса сервера. Аутентификация, настройки TLS, прокси и пул соединений остаются в <code>HttpClient</code>, где их уже настроило ваше приложение. Клиент не владеет <code>HttpClient</code> и не уничтожает его.</p>

<div class="codeblock" id="code">
 <h3>Создание DICOMweb клиента - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Если развертывание обслуживает WADO-URI или другой сервис с другого URL, <code>ServiceBaseUris</code> переопределяет корневой путь только для этого сервиса.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Поиск исследований с QIDO-RS">}}

<p>Поиск принимает параметры сопоставления с DICOM‑ключевым словом в качестве имени, поля для возврата и параметры постраничного вывода. Совпадения приходят в виде потока наборов данных, читая их по одному, пока ответ всё ещё загружается.</p>

<div class="codeblock" id="code">
 <h3>Найти исследования пациента - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> и <code>SearchInstancesAsync</code> выполняют поиск на один уровень ниже, по всему архиву или внутри одного исследования или серии. Те же параметры также задают нечеткое сопоставление, сопоставление пустых значений и смещение результата для постраничного вывода.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Скачивание исследования с WADO-RS">}}

<p>Исследование может содержать тысячи экземпляров. <code>RetrieveStudyAsync</code> возвращает их в виде потока, поэтому каждый файл разбирается по мере поступления соответствующей части ответа и может быть сохранён до чтения следующего. Потребление памяти не растёт с размером исследования.</p>

<div class="codeblock" id="code">
 <h3>Сохранить каждый экземпляр исследования - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> и <code>RetrieveInstanceAsync</code> скачивают одну серию или один экземпляр одинаковым способом.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Получить миниатюру или отрисованное изображение">}}

<p>Просмотрщик или экран списка задач часто требует изображение, а не DICOM‑файл. Сервер отрисовывает его и отправляет JPEG или PNG, поэтому клиент не загружает и не декодирует оригинальные пиксельные данные.</p>

<div class="codeblock" id="code">
 <h3>Сохранить JPEG‑миниатюру экземпляра - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p><code>RetrieveRenderedInstanceAsync</code> и <code>RetrieveRenderedFramesAsync</code> возвращают полнорзмерное изображение. <code>DicomWebRenderOptions</code> передаёт серверу стандартные параметры отрисовки: качество изображения, размер области просмотра, оконные параметры и ICC‑профиль.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Загрузка экземпляров с STOW-RS">}}

<p>STOW-RS отправляет произвольное количество экземпляров в одном запросе. Сервер отвечает отчётом, в котором построчно перечисляет, что было сохранено, а что отклонено.</p>

<div class="codeblock" id="code">
 <h3>Загрузка папки DICOM‑файлов - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>Некоторые серверы отвечают HTTP‑ошибкой, например 409, когда один или несколько экземпляров не прошли. В этом случае вызов бросает <code>DicomWebClientException</code>, как показано в следующем разделе, и тот же отчёт находится в теле ответа исключения.</p>

<p><code>StoreStudyAsync</code> загружает в одно именованное исследование. Оба метода также принимают <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, поэтому файлы могут генерироваться во время отправки запроса.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Обработка ошибок сервера">}}

<p>Каждая типизированная операция проверяет статус HTTP перед возвратом. Когда сервер отклоняет запрос, вызов бросает <code>DicomWebClientException</code>, содержащий код статуса, заголовки и начало тела ответа для журнала.</p>

<div class="codeblock" id="code">
 <h3>Сообщить о неудачном запросе - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>Методы, возвращающие необработанный ответ, такие как миниатюра выше, оставляют проверку статуса вам через <code>IsSuccessStatusCode</code> или <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Что ещё покрывает клиент">}}

<p>Поиск, загрузка и выгрузка — это основные вызовы. Клиент также имеет группы операций для остальных сервисов DICOMweb:</p>

<ul>
<li>Большие данные, пиксельные данные и выбранные кадры экземпляра по номеру кадра, начинающемуся с 1, в том виде, в котором их отправил сервер.</li>
<li>WADO-URI, более старый способ получения по единой URL, который по‑прежнему используют многие просмотрщики и архивы, для DICOM‑файлов и отрисованных изображений.</li>
<li>UPS-RS списки задач: создание, поиск, обновление и захват элементов работы, изменение их состояния, запрос отмены и подписка на события, поступающие через WebSocket.</li>
<li>Список задач модальности (MWL‑RS), выполненные процедуры модальностей (MPPS‑RS) и подтверждение хранения.</li>
<li>Экземпляры, не относящиеся к пациенту, такие как цветовые палитры и протоколы размещения, а также документ возможностей сервера.</li>
<li><code>SendAsync</code> для любой стандартной транзакции с вашими собственными значениями маршрута, параметрами запроса, заголовками и телом, когда типизированный метод не подходит.</li>
</ul>

<p>В <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">руководстве по DICOMweb клиенту</a> задокументированы все операции и параметры, а в <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">справочнике операций клиента</a> перечислены все методы.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Одна библиотека от HTTP‑ответа до пиксельных данных">}}

<p>Файл, загруженный с помощью клиента, представляет собой обычный <code>DicomFile</code>. Его можно анонимизировать, транскодировать, отрисовать или сериализовать в том же процессе, без промежуточного преобразования и без второй библиотеки в цепочке.</p>

<p>Связанные страницы: <a href="/medical/net/dicom-networking/">DICOM‑сеть</a> для архивов, работающих по DIMSE вместо HTTP, <a href="/medical/net/anonymization/">анонимизация</a> до выхода исследования из больницы, <a href="/medical/net/dicom-transfer-syntax-conversion/">конвертация синтаксиса передачи</a> для того, что хранит сервер, и <a href="/medical/net/dicom-to-json/">DICOM в JSON</a> для формата результатов поиска QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Обучающие ресурсы" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Руководство разработчика" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="Справочники API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
