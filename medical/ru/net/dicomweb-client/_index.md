---
title: Клиент DICOMweb на C# .NET – QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Поиск, загрузка и выгрузка DICOM‑исследений по HTTP из .NET. Чистый C# клиент DICOMweb для QIDO-RS, WADO-RS и STOW-RS, совместимый с PACS, VNA и облачными сервисами визуализации.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Клиент DICOMweb для .NET C#" h2="Поиск, загрузка и выгрузка исследований на любом сервере DICOMweb с помощью QIDO-RS, WADO-RS и STOW-RS, в управляемом C# без необходимости устанавливать что‑либо на машину." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM по HTTP из вашего собственного кода">}}

<p>DICOMweb — это HTTP‑форма DICOM. Вместо ассоциации DIMSE на порту 104 ваше приложение отправляет обычные HTTPS‑запросы: QIDO-RS для поиска, WADO-RS для загрузки, STOW-RS для выгрузки. Облачные сервисы визуализации, такие как Azure Health Data Services DICOM service и Google Cloud Healthcare API, работают именно так, как и архивы Orthanc и dcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> включает клиент DICOMweb начиная с версии 26.10. <code>DicomWebClient</code> оборачивает каждую стандартную транзакцию в типизированный метод, а результаты приходят в виде тех же объектов <code>Dataset</code> и <code>DicomFile</code>, которые использует остальная часть библиотеки для чтения и записи. Сервер с другой стороны может быть любой реализацией DICOMweb; он не обязан быть построен на Aspose.Medical.</p>

<p>Клиент — управляемый C# поверх <code>HttpClient</code>. Нет нативного инструментария и прокси‑сервиса, поэтому один и тот же код работает на Windows, Linux и в контейнере.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Подключение к серверу DICOMweb">}}

<p>Создайте клиент, используя <code>HttpClient</code> и корневой URL сервиса сервера. Аутентификация, настройки TLS, прокси и пул соединений остаются в <code>HttpClient</code>, где ваше приложение уже их конфигурирует. Клиент не владеет <code>HttpClient</code> и не освобождает его.</p>

<div class="codeblock" id="code">
 <h3>Создание клиента DICOMweb — C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>Если один деплой обслуживает WADO-URI или другой сервис с другого URL, <code>ServiceBaseUris</code> переопределяет корень только для этого сервиса.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Поиск исследований с QIDO-RS">}}

<p>Поиск принимает параметры совпадения с DICOM‑ключевым словом в качестве имени, поля для возврата и элементы управления постраничной навигацией. Совпадения приходят в виде потока наборов данных, читаемых по одному, пока ответ ещё загружается.</p>

<div class="codeblock" id="code">
 <h3>Поиск исследований пациента — C#</h3>
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

<p><code>SearchSeriesAsync</code> и <code>SearchInstancesAsync</code> выполняют поиск на уровень ниже, по всему архиву или внутри одного исследования или серии. Те же параметры также задают нечёткое совпадение, совпадение пустых значений и смещение результатов для постраничного вывода.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Загрузка исследования с WADO-RS">}}

<p>Исследование может содержать тысячи экземпляров. <code>RetrieveStudyAsync</code> возвращает их в виде потока, поэтому каждый файл разбирается по мере поступления части ответа и может быть сохранён до чтения следующего. Потребление памяти не растёт с размером исследования.</p>

<div class="codeblock" id="code">
 <h3>Сохранение каждого экземпляра исследования — C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> и <code>RetrieveInstanceAsync</code> загружают одну серию или один экземпляр тем же способом.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Получить миниатюру или отрисованное изображение">}}

<p>Вьювер или экран листа заданий часто нуждаются в картинке, а не в DICOM‑файле. Сервер отрисовывает её и отправляет JPEG или PNG, поэтому клиент не загружает и не декодирует исходные пиксельные данные.</p>

<div class="codeblock" id="code">
 <h3>Сохранение JPEG‑миниатюры экземпляра — C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> и <code>RetrieveRenderedFramesAsync</code> возвращают вместо этого полноразмерное изображение. <code>DicomWebRenderOptions</code> передаёт серверу стандартные параметры отрисовки: качество изображения, размер области просмотра, окно и ICC‑профиль.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Загрузка экземпляров с STOW-RS">}}

<p>STOW-RS отправляет любое количество экземпляров в одном запросе. Сервер отвечает отчетом, в котором перечисляет, экземпляр за экземпляром, что было сохранено и что отклонено.</p>

<div class="codeblock" id="code">
 <h3>Загрузка папки DICOM‑файлов — C#</h3>
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

<p>Некоторые серверы отвечают HTTP‑ошибкой, например 409, если один или несколько экземпляров не удалось сохранить. В таком случае вызов бросает <code>DicomWebClientException</code>, как показано в следующем разделе, а тот же отчет находится в теле ответа исключения.</p>

<p><code>StoreStudyAsync</code> загружает в одно названное исследование. Оба метода также принимают <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, поэтому файлы могут генерироваться во время отправки запроса.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Обработка ошибок сервера">}}

<p>Каждая типизированная операция проверяет HTTP‑статус перед возвратом. Когда сервер отклоняет запрос, вызов бросает <code>DicomWebClientException</code>, содержащий код статуса, заголовки и начало тела ответа для журналирования.</p>

<div class="codeblock" id="code">
 <h3>Отчет о неудачном запросе — C#</h3>
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

<p>Поиск, загрузка и выгрузка — это базовые вызовы. Клиент также имеет группу операций для каждого из остальных сервисов DICOMweb:</p>

<ul>
<li>Большие данные, данные пикселей и выбранные кадры экземпляра по 1‑based номеру кадра, как их отправляет сервер.</li>
<li>WADO-URI, более старый способ получения по единому URL, который всё ещё используют многие вьюверы и архивы, для DICOM‑файлов и отрисованных изображений.</li>
<li>UPS-RS листы заданий: создание, поиск, обновление и захват элементов заданий, изменение их состояния, запрос отмены и подписка на события, приходящие через WebSocket.</li>
<li>Лист заданий модальности (MWL-RS), выполненные процедуры модальности (MPPS-RS) и подтверждение хранения.</li>
<li>Экземпляры, не связанные с пациентом, такие как цветовые палитры и протоколы подвешивания, а также документ возможностей сервера.</li>
<li><code>SendAsync</code> для любой стандартной транзакции с вашими собственными параметрами маршрута, параметрами запроса, заголовками и телом, когда типизированный метод не подходит.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">Руководство по клиенту DICOMweb</a> описывает каждую операцию и параметр, а <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">справочник операций клиента</a> перечисляет все методы.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Одна библиотека от HTTP‑ответа до пиксельных данных">}}

<p>Файл, загруженный клиентом, представляет собой обычный <code>DicomFile</code>. Его можно анонимизировать, транскодировать, отрисовать или сериализовать в том же процессе, без промежуточного преобразования и без второй библиотеки в цепочке.</p>

<p>Служебные страницы: <a href="/medical/net/dicomweb-server/">сервер DICOMweb</a> для другой стороны протокола в вашем собственном сервисе ASP.NET Core, <a href="/medical/net/dicom-networking/">DICOM‑сетевые взаимодействия</a> для архивов, работающих по DIMSE вместо HTTP, <a href="/medical/net/anonymization/">анонимизация</a> перед тем как исследование покинет больницу, <a href="/medical/net/dicom-transfer-syntax-conversion/">преобразование синтаксиса передачи</a> для того, как сервер хранит данные, и <a href="/medical/net/dicom-to-json/">DICOM в JSON</a> для формата результатов поиска QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Обучающие ресурсы" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Руководство разработчика" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="Справочник API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
