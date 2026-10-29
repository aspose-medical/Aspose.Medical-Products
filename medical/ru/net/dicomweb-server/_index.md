---
title: DICOMweb сервер на C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Создайте свой собственный DICOMweb сервер или PACS на C#. Aspose.Medical реализует QIDO-RS, WADO-RS, STOW-RS и сервисы листов работ на ASP.NET Core; вы подключаете хранилище.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb сервер для .NET C#" h2="Добавьте конечные точки QIDO-RS, WADO-RS и STOW-RS в сервис ASP.NET Core. Библиотека реализует протокол; ваш код определяет, где хранятся исследования." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Ваш собственный DICOMweb сервер на C#">}}

<p>PACS, VNA, облачное хранилище или исследовательская платформа должны отвечать на запросы DICOMweb: поиск QIDO-RS, загрузки WADO-RS и загрузки STOW-RS, а часто также сервисы листов работ. Каждый из них имеет свои маршруты, типы медиа, multipart‑тела, правила сопоставления и коды состояний.</p>

<p><strong>Aspose.Medical for .NET</strong> реализует эту сторону протокола с версии 26.10. Он читает каждый запрос, проверяет его согласно стандарту, сопоставляет ключи поиска по правилам DICOM, постранично выводит результаты, конвертирует синтаксы передачи и формирует ответ. Ваш код делает то, что может только вы: сохраняет экземпляр, ищет кандидатов для поиска и возвращает сохранённые файлы. Просмотрщики, модальности и другие архивы затем работают с вашим сервером как с любым другим DICOMweb сервером, и также делает <a href="/medical/net/dicomweb-client/">DICOMweb client</a> той же библиотеки.</p>

<p>Сервер написан на управляемом C#. Он работает везде, где работает ASP.NET Core: в Windows, в Linux и в контейнере.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Полный сервер на ASP.NET Core">}}

<p>Установите пакет <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. Он поставляет Aspose.Medical и подключает сервисы DICOMweb к внедрению зависимостей и маршрутизации конечных точек. Этот <code>Program.cs</code> представляет работающий сервер Studies; ниже показаны два класса, которые он регистрирует.</p>

<div class="codeblock" id="code">
 <h3>Program.cs DICOMweb сервера — C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>Каждый маршрут Studies теперь доступен по <code>/dicomweb</code>: <code>/dicomweb/studies</code> для поиска и загрузки, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> для получения, а также маршруты для метаданных, кадров, bulk‑данных и отрисованных изображений. Документ возможностей в корне сервиса точно перечисляет поддерживаемые сервером транзакции.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Подключите ваше хранилище">}}

<p>Сервис Studies требует один интерфейс с тремя методами: сохранить экземпляр, вернуть сохранённые файлы и вернуть кандидатов для поиска. Эта реализация хранит каждый экземпляр как файл в папке, соответствующей исследованию и серии.</p>

<div class="codeblock" id="code">
 <h3>Папочный архив за QIDO-RS, WADO-RS и STOW-RS — C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>Библиотека проверяет UID в каждом пути запроса и в каждом загруженном экземпляре перед вызовом провайдера. Экземпляр без действительных UID фиксируется как ошибочный и никогда не попадает в <code>StoreAsync</code>.</p>

<p>Папка достаточна для пробного запуска сервера. Производственный архив сохраняет те же три метода и размещает объектное хранилище за <code>StoreAsync</code> и <code>RetrieveAsync</code>, а индекс базы данных — за <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Поиск: библиотека применяет правила сопоставления DICOM">}}

<p>Сопоставление QIDO-RS — это больше, чем сравнение строк: подстановочные знаки, диапазоны дат и времени, списки UID, нечёткие имена пациентов, пустые значения и ключи внутри последовательностей. Провайдер не реализует это. Он возвращает кандидатов в стабильном порядке и может вернуть больше, чем совпадает. Библиотека применяет точное соответствие DICOM к каждому кандидату, сохраняет запрошенные поля и применяет <code>offset</code> и <code>limit</code>. Один ответ содержит не более 1000 результатов по умолчанию, а <code>DicomWebStandardServiceOptions</code> меняет этот лимит.</p>

<p>Чтобы ускорить поиск в большом архиве, передавайте отборочные ключи в базу данных. <code>search.Plan.Filter</code> содержит скомпилированные ключи поиска с атрибутом, типом совпадения и значениями каждого.</p>

<div class="codeblock" id="code">
 <h3>Чтение точного Patient ID из поиска — C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>Запрос к базе данных, построенный таким образом, может вернуть лишние строки, например когда он игнорирует ключ, который нельзя трансформировать. Окончательное сопоставление в библиотеке удаляет их. Оно не должно отбрасывать истинные совпадения, поскольку библиотека не может добавить их обратно.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Хранение: результат для каждого экземпляра">}}

<p>Запрос STOW-RS может содержать сотни экземпляров. Библиотека разбирает multipart‑тело и последовательно вызывает <code>StoreAsync</code> для каждого экземпляра, поэтому вся загрузка никогда не удерживается в памяти полностью. Для каждого экземпляра провайдер возвращает <code>Stored</code>, <code>Warning</code> для экземпляра, сохранённого с изменениями, или <code>Failed</code> с необязательным DICOM кодом причины.</p>

<p>Библиотека формирует отчёт STOW-RS из этих результатов и выбирает статус: 200, когда каждый экземпляр сохранён; 202, когда есть предупреждения или ошибки; 409, когда все экземпляры не удалось сохранить. Экземпляры в запросе, относящиеся к одному именованному исследованию, но принадлежащие другому исследованию, отклоняются до вызова провайдера.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Получение: синтаксы передачи, кадры и отрисованные изображения">}}

<p>Провайдер возвращает сохранённые файлы, а библиотека доставляет их в запрошенной клиентом форме. WADO-RS клиент может запросить синтаксис передачи в заголовке <code>Accept</code>, и библиотека конвертирует каждый файл перед отправкой, используя собственные кодеки, включая HTJ2K и JPEG XL. Метаданные в DICOM JSON или XML, bulk‑данные и отдельные кадры получаются от того же провайдера без дополнительного кода.</p>

<p>Для отрисованных изображений требуется ещё один класс, поскольку только вы решаете, какие изображения сервер может генерировать. Этот класс предоставляет JPEG для отрисованных экземпляров, кадров и миниатюр, а библиотека их отрисовывает:</p>

<div class="codeblock" id="code">
 <h3>Предоставление отрисованных JPEG изображений и миниатюр — C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>Встроенный рендерер генерирует JPEG из одно‑кадрового экземпляра или из кадров, выбранных запросом. Для отрисовки многокадрового экземпляра целиком, других форматов изображений, а также параметров viewport, window и ICC‑профиля требуется ваш собственный <code>RetrieveAsync</code> в этом классе.</p>

<p>Миниатюры обрабатываются с учётом конфиденциальности пациентов. Встроенный рендерер создаёт миниатюру только из экземпляра, у которого атрибуты Burned In Annotation и Recognizable Visual Features оба имеют значение <code>NO</code>, что означает, что изображение не содержит данных пациента. Во многих изображениях эти атрибуты отсутствуют, и сервер отвечает 406. Если вашему архиву нужны миниатюры таких изображений, создавайте их в вашем собственном <code>RetrieveAsync</code> и убедитесь, что они скрывают любые данные пациента.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Листы работ, шаги процедур и другие сервисы">}}

<p>Остальные DICOMweb сервисы следуют той же модели: регистрируют сервис и реализуют его интерфейс хранилища.</p>

<ul>
<li>UPS-RS листы работ с <code>AddStandardWorklistService</code>: создание, поиск и обновление элементов работ и изменение их состояния, при этом правила состояний проверяются библиотекой, а уведомления о событиях отправляются через WebSocket.</li>
<li>Лист работ модальности (MWL-RS) с <code>AddStandardModalityWorklistService</code>, позволяющий модальностям запрашивать запланированные шаги процедур вашего RIS.</li>
<li>Выполненные шаги процедур модальностей (MPPS-RS), подтверждение хранения и экземпляры, не связанные с пациентом, такие как цветовые палитры и протоколы подвешивания.</li>
<li>Получение WADO-URI для старых просмотрщиков, обслуживается сервисом Studies.</li>
</ul>

<p>Если стандартное поведение транзакции не подходит, зарегистрируйте свой обработчик для неё с помощью <code>AddHandler</code>. Библиотека по‑прежнему маршрутизирует и проверяет запрос, а остальные транзакции сохраняют свои стандартные обработчики.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Безопасность и хостинг находятся в ваших руках">}}

<p>Конечные точки DICOMweb — обычные ASP.NET Core конечные точки. Аутентификация, авторизация, CORS, TLS, перенаправленные заголовки и ограничения размеров запросов настраиваются так же, как и для остальных компонентов вашего сервиса:</p>

<div class="codeblock" id="code">
 <h3>Требовать аутентифицированного пользователя — C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>Часть протокола не зависит от ASP.NET Core. <code>DicomWebApplication</code> преобразует <code>DicomWebRequest</code> в ответ, так что те же сервисы могут работать за другим HTTP‑сервером или в тесте без сети.</p>

<div class="codeblock" id="code">
 <h3>Тот же сервер без ASP.NET Core — C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Клиент и сервер из одной библиотеки">}}

<p><a href="/medical/net/dicomweb-client/">DICOMweb клиент</a> библиотеки Aspose.Medical проверяет ваш сервер из модульного теста или консольной программы, и каждый файл, получаемый сервером, является обычным <code>DicomFile</code>, который можно анонимизировать, перекодировать или отрисовать в том же процессе.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">Руководство разработчика DICOMweb</a> охватывает каждый сервис и интерфейс хранилища, а <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">руководство по ASP.NET Core</a> описывает привязку конечных точек и WebSockets.</p>

<p>Связанные страницы: <a href="/medical/net/dicom-networking/">DICOM сеть</a> для систем, отправляющих исследования по DIMSE вместо HTTP, <a href="/medical/net/anonymization/">анонимизация</a> входящих исследований, <a href="/medical/net/dicom-transfer-syntax-conversion/">конверсия синтаксов передачи</a> и <a href="/medical/net/dicom-to-json/">DICOM в JSON</a> — формат, используемый результатами QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Учебные ресурсы" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Руководство разработчика" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
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
