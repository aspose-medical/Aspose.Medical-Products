---
title: C# .NET'te UPS-RS - DICOMweb Unified Procedure Step İş Öğeleri | Aspose.Medical
weight: 9400

description: C# ile DICOM UPS iş öğelerini oluşturun, talep edin ve tamamlayın, durum değişikliklerini WebSocket üzerinden alın veya Aspose.Medical ile ASP.NET Core üzerinde kendi UPS-RS sunucunuzu çalıştırın.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS for .NET C#" h2="Sistemler arasında görüntüleme işlerini DICOM Unified Procedure Step ile dağıtın: iş öğelerini oluşturun, çalışanların talep edip tamamlamasına izin verin ve her değişikliği, istemci olarak ya da sunucu olarak izleyin." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM konuşan bir iş kuyruğu">}}

<p>Bir AI modeli her yeni göğüs BT'sini işlemek zorunda. Bir tele radyoloji servisi çalışmaları okuyuculara atar. Bir 3D laboratuvarı iş sonrası siparişleri alır. Bunların her biri sistemler arasındaki görev kuyruğudur ve DICOM bunun için bir tanım sunar: Unified Procedure Step (UPS), DICOMweb'in UPS-RS hizmeti tarafından HTTP üzerinden sunulur.</p>

<p>Bir UPS iş öğesi ne yapılacağını, hangi hasta için ve hangi girdiyle olduğunu belirtir. Oluşturulduğunda SCHEDULED (planlanmış) durumundadır. Bir çalışan bunu talep eder ve IN PROGRESS (devam ediyor) hâline getirir, ardından COMPLETED (tamamlandı) olarak bitirir veya CANCELED (iptal edildi) olarak sona erer. Diğer sistemler abone olur ve her durum değişikliğini WebSocket üzerinden alır, böylece kimsenin periyodik sorgu yapmasına gerek kalmaz.</p>

<p><strong>Aspose.Medical for .NET</strong> yönetilen C#'ta her iki tarafı da uygular: herhangi bir UPS-RS sunucusu için tiplenmiş istemci ve ASP.NET Core için sunucu tarafı; burada kütüphane UPS kurallarını denetler ve kodunuz iş öğelerini depolar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bir iş öğesi planlayın">}}

<p>İşi gerçekleştirmek isteyen sistem iş öğesini oluşturur. SOP Instance UID'si bundan itibaren onu tanımlar; etiket, iş listesi etiketi ve öncelik, çalışanlara ne olduğunu söyler.</p>

<div class="codeblock" id="code">
 <h3>UPS iş öğesi oluşturma - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// What to do, for which patient, and with which input
Dataset input = new();
input.AddOrUpdate(Tag.StudyInstanceUID, "1.2.826.0.1.3680043.10.1.100");

Dataset workitem = new();
workitem.AddOrUpdate(Tag.SOPInstanceUID, Uid.CreateDerivedFromUuid());
workitem.AddOrUpdate(Tag.ProcedureStepState, "SCHEDULED");
workitem.AddOrUpdate(Tag.TransactionUID, string.Empty);
workitem.AddOrUpdate(Tag.InputReadinessState, "READY");
workitem.AddOrUpdate(Tag.ProcedureStepLabel, "Lung nodule detection");
workitem.AddOrUpdate(Tag.WorklistLabel, "AI");
workitem.AddOrUpdate(Tag.ScheduledProcedureStepPriority, "HIGH");
workitem.AddOrUpdate(Tag.PatientName, "DOE^JANE");
workitem.AddOrUpdate(Tag.PatientID, "P001");
workitem.AddOrUpdate(new Sequence(Tag.InputInformationSequence, [input]));

DicomWebClientResult&lt;CreateWorkitemResult&gt; created = await client.Worklist.CreateAsync(workitem);
Console.WriteLine($"Scheduled {created.Value.WorkitemUid}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Talep edin, işi yapın, tamamlayın">}}

<p>Bir çalışan, yapabileceklerini iş listesinde arar ve oluşturduğu bir transaction UID ile iş öğesini IN PROGRESS durumuna taşıyarak talep eder. O andan itibaren yalnızca bu UID'nin sahibi iş öğesini güncelleyebilir veya tamamlayabilir, böylece iki çalışan aynı görevi asla işlemeyez.</p>

<div class="codeblock" id="code">
 <h3>Planlanmış iş öğelerini bul, talep et ve tamamla - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// Scheduled workitems on the AI worklist
DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters =
    [
        new DicomWebQueryParameter("ProcedureStepState", "SCHEDULED"),
        new DicomWebQueryParameter("WorklistLabel", "AI")
    ]
};

List&lt;string&gt; scheduled = [];
await using (DicomWebClientStream&lt;Dataset&gt; found = await client.Worklist.SearchAsync(search))
{
    await foreach (Dataset item in found)
        scheduled.Add(item.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID));
}

foreach (string workitemUid in scheduled)
{
    // Claim the workitem; only the holder of this transaction UID can update or finish it
    string transactionUid = Uid.CreateDerivedFromUuid().Code;
    await client.Worklist.ChangeStateAsync(workitemUid, WorkitemState.InProgress, transactionUid);

    // ... run the job ...

    await client.Worklist.UpdateAsync(workitemUid, transactionUid, Performed());
    await client.Worklist.ChangeStateAsync(workitemUid, WorkitemState.Completed, transactionUid);
}</code></pre>
</div>

<p>COMPLETED durumu, işin bir kaydını gerektirir: kim tarafından yapıldı, ne yapıldı, başlangıç ve bitiş zamanı ve çıktı. Çalışan, durum değişmeden önce bir güncelleme ile bunu kaydeder.</p>

<div class="codeblock" id="code">
 <h3>Yapılan prosedür verileri - C#</h3>
 <pre><code class="cs">// What COMPLETED requires: who did the work, what was done, when, and the output (here none)
public static Dataset Performed()
{
    Dataset performed = new();
    performed.AddOrUpdate(new Sequence(Tag.PerformedStationNameCodeSequence, [Code("AI-NODE-1", "99LOCAL", "AI node 1")]));
    performed.AddOrUpdate(new Sequence(Tag.PerformedWorkitemCodeSequence, [Code("NODULE", "99LOCAL", "Lung nodule detection")]));
    performed.AddOrUpdate(Tag.PerformedProcedureStepStartDateTime, DateTimeOffset.Now.AddMinutes(-2).ToString("yyyyMMddHHmmss"));
    performed.AddOrUpdate(Tag.PerformedProcedureStepEndDateTime, DateTimeOffset.Now.ToString("yyyyMMddHHmmss"));
    performed.AddOrUpdate(new Sequence(Tag.OutputInformationSequence, []));

    Dataset update = new();
    update.AddOrUpdate(new Sequence(Tag.UnifiedProcedureStepPerformedProcedureSequence, [performed]));
    return update;
}

private static Dataset Code(string value, string scheme, string meaning)
{
    Dataset code = new();
    code.AddOrUpdate(Tag.CodeValue, value);
    code.AddOrUpdate(Tag.CodingSchemeDesignator, scheme);
    code.AddOrUpdate(Tag.CodeMeaning, meaning);
    return code;
}</code></pre>
</div>

<p>Aspose.Medical ile inşa edilmiş bir sunucu bu kuralları uygular. Devam eden bir iş öğesini talep etmeye çalışan ikinci bir çalışan HTTP 409 alır, aynı şekilde tamamlanmış bir öğenin durum değişikliği de 409 döner. Yapılan prosedür verileri olmadan bir COMPLETED isteği HTTP 400 alır. İstemci, bu yanıtları HTTP yanıtı ile <code>DicomWebClientException</code> olarak bildirir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="WebSocket üzerinden iş listesini izleyin">}}

<p>Bir gösterge tablosu, işi sipariş eden sistem ya da yeni görev bekleyen bir çalışan, AE başlığıyla abone olur ve bir bildirim bağlantısı açar. Her olay raporu, iş öğesi UID'si ve yeni durumu içeren bir veri kümesi olarak gelir.</p>

<div class="codeblock" id="code">
 <h3>UPS olay raporlarını al - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// Every change on the worklist, pushed over a WebSocket
await client.Worklist.SubscribeWorklistAsync("DASHBOARD");
await using DicomWebNotificationConnection connection = await client.ConnectNotificationsAsync("DASHBOARD");

while (await connection.ReceiveAsync(cancellationToken) is Dataset report)
{
    Console.WriteLine($"{report.GetSingleValueOrDefault(Tag.AffectedSOPInstanceUID, string.Empty)}: " +
                      $"{report.GetSingleValueOrDefault(Tag.ProcedureStepState, string.Empty)}");
}</code></pre>
</div>

<p>Tüm iş listesine ek olarak, bir istemci <code>SubscribeWorkitemAsync</code> ile tek bir iş öğesine veya sunucu filtreleri destekliyorsa <code>SubscribeFilteredWorklistAsync</code> ile iş listesinin filtrelenmiş bir kısmına abone olabilir. Alımı iptal etmek bağlantıyı kapatır; devam etmek için <code>ConnectNotificationsAsync</code> ile yenisini açın.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kendi UPS-RS sunucunuz">}}

<p><code>Aspose.Medical.Dicom.Web.AspNetCore</code> paketi, bir ASP.NET Core hizmetine UPS-RS ekler. Kütüphane her isteği işler: yeni bir iş öğesinin yükünü, durum geçişlerini, çalışanın transaction UID'sini ve COMPLETED için gereken verileri kontrol eder ve aramalara DICOM eşleştirme kurallarını uygular. Kodunuz iş öğelerini depolar.</p>

<div class="codeblock" id="code">
 <h3>UPS-RS sunucusunun Program.cs - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>İş öğesi depolama - C#</h3>
 <pre><code class="cs">// Workitems with a revision each; the library applies the UPS state rules and the search matching
public sealed class WorkitemStore : IDicomWebWorkitemProvider
{
    private readonly ConcurrentDictionary&lt;string, DicomWebVersionedDataset&gt; _items = new();

    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string uid in _items.Keys.Order(StringComparer.Ordinal))
            if (_items.TryGetValue(uid, out DicomWebVersionedDataset? item))
                yield return new Dataset(item.Dataset);
        await Task.CompletedTask;
    }

    public ValueTask&lt;DicomWebVersionedDataset?&gt; GetAsync(string workitemUid, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryGetValue(workitemUid, out DicomWebVersionedDataset? item) ? Copy(item) : null);

    public ValueTask&lt;bool&gt; TryCreateAsync(string workitemUid, Dataset workitem, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryAdd(workitemUid, NewRevision(workitem)));

    // Replaces the workitem only if nobody changed it since it was read
    public ValueTask&lt;bool&gt; TryReplaceAsync(string workitemUid, string expectedRevision, Dataset workitem,
        CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryGetValue(workitemUid, out DicomWebVersionedDataset? current)
                             &amp;&amp; current.Revision == expectedRevision
                             &amp;&amp; _items.TryUpdate(workitemUid, NewRevision(workitem), current));

    private static DicomWebVersionedDataset NewRevision(Dataset dataset) =&gt;
        new() { Dataset = new Dataset(dataset), Revision = Guid.NewGuid().ToString("N") };

    private static DicomWebVersionedDataset Copy(DicomWebVersionedDataset item) =&gt;
        new() { Dataset = new Dataset(item.Dataset), Revision = item.Revision };
}</code></pre>
</div>

<p>Her depolanan iş öğesinin bir revizyonu vardır. <code>TryReplaceAsync</code>, sadece revizyon hâlâ kütüphanenin okuduğu revizyon olduğunda değiştirir, böylece aynı anda aynı iş öğesini değiştiren iki istek birbirini üzerine yazamaz; kütüphane yeniden dener ya da bir çakışma raporlar. Sözlüğü veritabanınızla değiştirin ve bu kontrolü sürdürün.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Sunucuda bildirimler">}}

<p>Olay raporları göndermek için bir <code>IDicomWebSubscriptionProvider</code> kaydedin ve <code>MapDicomWeb</code>'den önce <code>app.UseWebSockets()</code> çağırın. Sağlayıcı, abonelikleri ve her abone için bir olay raporu kuyruğunu tutar ve bir iş öğesi değiştiğinde kütüphane ona bir olay raporu verir. Sağlayıcı aynı zamanda kütüphaneye kimliği doğrulanmış bir çağıranın hangi AE başlığına sahip olduğunu bildirir, çünkü olay raporları durumu değiştiren sistemi adlandırır.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">İş akışı sağlayıcıları rehberi</a>, tam bir bellek içi abonelik sağlayıcısı içerir ve <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core rehberi</a>, WebSocket ayarlarını kapsar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL ve MPPS">}}

<p>UPS, eski Modality Worklist ve MPPS'nin bir RIS ile modalite arasında yaptığı işleri kapsar ve modalite olmayan çalışanları, talep etmeyi ve bildirimleri ekler. Birçok modalite hâlâ sadece DIMSE üzerinden MWL ve MPPS kullanır; <a href="/medical/net/modality-worklist/">modalite iş listesi sayfası</a>, aynı kütüphane ile o iş akışını gösterir.</p>

<p>İlgili sayfalar: diğer DICOMweb hizmetleri için <a href="/medical/net/dicomweb-client/">DICOMweb istemcisi</a>, aynı sunucuda QIDO-RS, WADO-RS ve STOW-RS için <a href="/medical/net/dicomweb-server/">DICOMweb sunucusu</a>, ve DIMSE için <a href="/medical/net/dicom-networking/">DICOM ağ iletişimi</a>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokümantasyon" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Geliştirici Kılavuzu" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="API Referansları" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Ürün Desteği" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Ücretsiz Destek" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Ücretli Destek" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Neden Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Müşteri Listesi" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Başarı Hikayeleri" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
