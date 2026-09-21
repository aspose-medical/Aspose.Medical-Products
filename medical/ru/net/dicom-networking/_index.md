---
title: Сетевое взаимодействие DICOM в C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Подключите ваше .NET‑приложение к PACS. Проверьте соединение с помощью C-ECHO, отправляйте изображения через C-STORE, делайте запросы через C-FIND и получайте изображения с собственным SCP. DIMSE‑клиент и сервер на чистом C#.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Сетевое взаимодействие DICOM в .NET C#" h2="Общайтесь с PACS из собственного приложения: C-ECHO, C-STORE, C-FIND, C-MOVE и C-GET, как клиент и как сервер, на управляемом C# без необходимости установки чего‑либо на компьютере." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Подключитесь к PACS из собственного кода">}}

<p>Чтение файлов DICOM — это легкая часть медицинской визуализации. Как только ваше приложение должно работать с реальной больничной системой, оно должно использовать протокол DIMSE: открыть ассоциацию с PACS, отправить изображения, запросить, какие исследования доступны, и отвечать, когда другая система отправляет что‑то обратно.</p>

<p><strong>Aspose.Medical for .NET</strong> поставляется с этим протоколом в составе библиотеки. <code>Aspose.Medical.Dicom.Network</code> предоставляет вам DIMSE‑клиент и DIMSE‑сервер, оба написаны на управляемом C#. Нет необходимости устанавливать нативный набор инструментов, нет сервисов для настройки и ничего, зависящего от платформы, поэтому один и тот же код работает в Windows, в Linux и в контейнере.</p>

<p>Три действия покрывают большинство интеграций, и каждое из них реализуется в паре строк: проверка связи с помощью C-ECHO, загрузка изображений через C-STORE и поиск того, что находится на другом конце, с помощью C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Начните с C-ECHO">}}

<p>C-ECHO — это ping DICOM. Он подтверждает, что хост, порт и оба AE‑заголовка указаны правильно, прежде чем будет выдана какая‑либо другая ошибка. Создайте клиент один раз, назначьте обработчик, наблюдающий за ответом, и отправьте запрос.</p>

<div class="codeblock" id="code">
 <h3>Проверьте соединение с помощью C‑ECHO - C#</h3>
 <pre><code class="cs">AssociationNegotiationOptions negotiation = new AssociationNegotiationOptions()
    .WithPresentationContext(new PresentationContext
    {
        AbstractSyntax = Uid.Verification,
        Role = null,
        TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
    });

DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = negotiation
    })
    .AddCEchoHandler((request, response, cancellationToken) =>
    {
        Console.WriteLine($"C-ECHO answered with status 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(new CEchoRequest());

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Статус <code>0x0000</code> означает успех. Запросы ставятся в очередь и отправляются по одной ассоциации, поэтому пакет задач не открывает соединение для каждого элемента.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Отправка изображений с помощью C-STORE">}}

<p>C-STORE — это действие приложения после создания или получения изображения: оно отправляет экземпляр в архив. Поместите запрос в очередь для каждого экземпляра и отправьте их совместно.</p>

<div class="codeblock" id="code">
 <h3>Отправка DICOM‑файла в PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Предлагаемые вами контексты представления определяют, что архив примет. Если требуется сжатый синтаксис, выполните транскодирование перед отправкой, как показано на странице <a href="/medical/net/dicom-transfer-syntax-conversion/">конвертации синтаксиса передачи</a>, либо перечислите альтернативы в <code>AdditionalTransferSyntaxes</code> и позвольте переговорам выбрать одну.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Поиск исследований с помощью C-FIND">}}

<p>C-FIND отвечает на вопрос «что содержит архив». Совпадения приходят по одному, каждый со своим набором идентификационных данных, а окончательный ответ завершает запрос.</p>

<div class="codeblock" id="code">
 <h3>Запрос исследований по пациенту - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "PACS_AE",
        Calling = "MY_SCU",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.StudyRootQueryRetrieveInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddCFindHandler((request, response, cancellationToken) =>
    {
        // A match arrives with an identifier; the final response carries the status only
        if (response.Identifier is not null)
            Console.WriteLine(response.Identifier.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty));

        return ValueTask.CompletedTask;
    })
    .Build();

client.QueueRequest(CFindRequest.CreateStudyQuery(
    patientId: "PATIENT-001",
    patientName: null,
    studyDateTime: null,
    accession: null,
    studyId: null,
    modalitiesInStudy: null,
    studyInstanceUid: null,
    priority: DimsePriority.Medium));

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>Тот же фабричный метод создает запросы других уровней: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> и <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> формирует запрос списка работ модальности, который модальность отправляет перед сканированием.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Получение изображений: ваш собственный SCP-хранилище">}}

<p>Библиотека также может выступать в роли сервера. Зарегистрируйте обработчик для сервиса, который хотите предоставить, начните прослушивание, и ваше приложение станет DICOM‑узлом, к которому может отправлять данные модальность или другой PACS.</p>

<div class="codeblock" id="code">
 <h3>Приём входящих изображений - C#</h3>
 <pre><code class="cs">DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Any, 11112)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.SecondaryCaptureImageStorage,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddSingletonCStoreHandler(new StoreHandler())
    .Build();

await server.StartAsync(CancellationToken.None);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Обработчик, сохраняющий полученные данные - C#</h3>
 <pre><code class="cs">public sealed class StoreHandler : ICStoreRequestHandler
{
    public ValueTask&lt;CStoreResponse&gt; Handle(CStoreRequest request, CancellationToken cancellationToken)
    {
        // Write what arrived, then answer Success
        new DicomFile(request.Dataset).Save($"{request.AffectedSopInstanceUid}.dcm");

        CStoreResponse response = new();
        response.Command.AddOrUpdate(Tag.Status, (ushort)0x0000);
        return ValueTask.FromResult(response);
    }
}</code></pre>
</div>

<p>То, что делает обработчик с набором данных — ваше решение: записать его на диск, поместить в очередь, предварительно анонимизировать с помощью <a href="/medical/net/anonymization/">API анонимизации</a>, либо транскодировать в синтаксис архива.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Что ещё покрывает сетевой API">}}

<p>Три перечисленных сервиса являются типичными. Остальная часть DIMSE также доступна:</p>

<ul>
<li>Извлечение: C-MOVE и C-GET, с отчётом о счетчиках подопераций в ответах.</li>
<li>N‑сервисы: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE и N-EVENT-REPORT, на основе которых реализованы обязательство хранения и MPPS.</li>
<li>Контроль ассоциации: контексты представления, роли сервисных классов, расширенные переговоры, окно асинхронных операций, согласование пользовательской идентификации и хук политики, позволяющий отклонять ассоциацию по AE‑заголовку.</li>
<li>TLS с обеих сторон, посредством <code>TlsInitiatorAuthenticator</code> и <code>TlsAcceptorAuthenticator</code>, с возможностью использовать собственную проверку сертификатов при необходимости.</li>
<li>Тайм‑ауты для каждого этапа, от TCP‑соединения до завершения, и уведомления о жизненном цикле ассоциации, чтобы долговременный узел мог вести журнал событий.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">Руководство по сетевому взаимодействию DICOM</a> описывает каждый параметр и каждый обработчик.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Чистый .NET, от сокета до пиксельных данных">}}

<p>Весь код на этой странице — управляемый код из того же пакета, который читает и записывает файлы. Ассоциация, кодеки и парсер берутся из одной библиотеки, поэтому исследование, полученное по сети, может быть анонимизировано, транскодировано или сериализовано без выхода из процесса и без нативных зависимостей в любой части цепочки.</p>

<p>См. также: <a href="/medical/net/dicom-transfer-syntax-conversion/">конвертация синтаксиса передачи</a> — что отправлять, <a href="/medical/net/anonymization/">анонимизация</a> — что удалять в первую очередь, и <a href="/medical/net/dicom-tags/">теги DICOM</a> — как читать полученные данные.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Обучающие ресурсы" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Документация" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Руководство разработчика" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
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
