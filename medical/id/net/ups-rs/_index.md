---
title: UPS-RS dalam C# .NET - DICOMweb Unified Procedure Step Workitems | Aspose.Medical
weight: 9400

description: Buat, klaim, dan selesaikan workitem DICOM UPS dari C#, terima perubahan status melalui WebSocket, atau jalankan server UPS-RS Anda sendiri di ASP.NET Core dengan Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS untuk .NET C#" h2="Bagikan pekerjaan pencitraan antar sistem dengan DICOM Unified Procedure Step: buat workitem, biarkan pekerja mengklaim dan menyelesaikannya, serta lacak setiap perubahan, baik sebagai klien maupun sebagai server." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Antrian kerja yang berbahasa DICOM">}}

<p>Sebuah model AI harus memproses setiap CT dada baru. Layanan teleradiologi menugaskan studi kepada pembaca. Laboratorium 3D menerima perintah pasca‑pemrosesan. Masing‑masing ini adalah antrian tugas antar sistem, dan DICOM mendefinisikan satu untuk itu: Unified Procedure Step (UPS), yang ditawarkan melalui HTTP oleh layanan UPS‑RS DICOMweb.</p>

<p>Sebuah workitem UPS menyatakan apa yang harus dilakukan, untuk pasien mana, dan dengan masukan apa. Workitem tersebut berstatus SCHEDULED saat dibuat. Seorang pekerja mengklaimnya dan menjadikannya IN PROGRESS, lalu menyelesaikannya sebagai COMPLETED, atau berakhir sebagai CANCELED. Sistem lain berlangganan dan menerima setiap perubahan status melalui WebSocket, sehingga tidak ada yang perlu melakukan polling.</p>

<p><strong>Aspose.Medical untuk .NET</strong> mengimplementasikan kedua sisi dalam C# terkelola: klien bertipe untuk server UPS‑RS apa pun, dan sisi server untuk ASP.NET Core, di mana perpustakaan memeriksa aturan UPS dan kode Anda menyimpan workitem.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Jadwalkan sebuah workitem">}}

<p>Sistem yang menginginkan pekerjaan selesai membuat workitem. SOP Instance UID-nya mengidentifikasinya sejak saat itu; label, label worklist, dan prioritas memberi tahu pekerja apa itu.</p>

<div class="codeblock" id="code">
 <h3>Buat sebuah UPS workitem - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Klaim, lakukan pekerjaan, selesaikan">}}

<p>Seorang pekerja mencari worklist untuk menemukan apa yang dapat dikerjakan dan mengklaim sebuah workitem dengan memindahkannya ke IN PROGRESS menggunakan transaction UID yang dihasilkannya. Sejak saat itu hanya pemegang UID tersebut yang dapat memperbarui atau menyelesaikan workitem, sehingga dua pekerja tidak pernah memproses tugas yang sama.</p>

<div class="codeblock" id="code">
 <h3>Temukan, klaim, dan selesaikan workitem terjadwal - C#</h3>
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

<p>COMPLETED memerlukan catatan pekerjaan: siapa yang melakukannya, apa yang dilakukan, waktu mulai dan selesai, serta outputnya. Pekerja menyimpannya dengan pembaruan sebelum mengubah status.</p>

<div class="codeblock" id="code">
 <h3>Data prosedur yang dilakukan - C#</h3>
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

<p>Server yang dibangun dengan Aspose.Medical menegakkan aturan‑aturan ini. Pekerja kedua yang mencoba mengklaim workitem yang sedang diproses menerima HTTP 409, begitu pula perubahan status pada workitem yang sudah COMPLETED. Permintaan COMPLETED tanpa data prosedur yang dilakukan menerima HTTP 400. Klien melaporkan jawaban tersebut sebagai <code>DicomWebClientException</code> dengan respons HTTP.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lacak worklist melalui WebSocket">}}

<p>Dashboard, sistem yang memesan pekerjaan, atau pekerja yang menunggu tugas baru berlangganan dengan judul AE mereka dan membuka koneksi notifikasi. Setiap laporan peristiwa tiba sebagai dataset dengan UID workitem dan status barunya.</p>

<div class="codeblock" id="code">
 <h3>Terima laporan peristiwa UPS - C#</h3>
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

<p>Selain seluruh worklist, klien dapat berlangganan ke satu workitem dengan <code>SubscribeWorkitemAsync</code> atau ke bagian worklist yang difilter dengan <code>SubscribeFilteredWorklistAsync</code>, bila server mendukung filter. Membatalkan penerimaan menutup koneksi; buka koneksi baru dengan <code>ConnectNotificationsAsync</code> untuk melanjutkan.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Server UPS‑RS milik Anda">}}

<p>Paket <code>Aspose.Medical.Dicom.Web.AspNetCore</code> menambahkan UPS‑RS ke layanan ASP.NET Core. Perpustakaan menangani setiap permintaan: memeriksa payload workitem baru, transisi status, transaction UID pekerja, dan data yang diperlukan oleh COMPLETED, serta menerapkan aturan pencocokan DICOM pada pencarian. Kode Anda menyimpan workitem.</p>

<div class="codeblock" id="code">
 <h3>Program.cs dari server UPS‑RS - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Penyimpanan workitem - C#</h3>
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

<p>Setiap workitem yang disimpan memiliki revisi. <code>TryReplaceAsync</code> menggantinya hanya bila revisinya masih sama seperti yang dibaca perpustakaan, sehingga dua permintaan yang mengubah workitem yang sama pada saat bersamaan tidak dapat menimpa satu sama lain; perpustakaan mencoba lagi atau melaporkan konflik. Ganti kamus dengan basis data Anda dan pertahankan pemeriksaan tersebut.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Notifikasi pada server">}}

<p>Untuk mengirim laporan peristiwa, daftarkan sebuah <code>IDicomWebSubscriptionProvider</code> dan panggil <code>app.UseWebSockets()</code> sebelum <code>MapDicomWeb</code>. Penyedia menyimpan langganan dan satu antrean laporan peristiwa per pelanggan, dan perpustakaan memberikannya laporan peristiwa setiap kali workitem berubah. Penyedia juga memberi tahu perpustakaan judul AE yang dimiliki pemanggil yang terautentikasi, karena laporan peristiwa menyebutkan sistem yang mengubah status.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">Panduan penyedia alur kerja</a> berisi penyedia langganan in‑memory lengkap, dan <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">panduan ASP.NET Core</a> mencakup pengaturan WebSocket.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL, dan MPPS">}}

<p>UPS mencakup apa yang dilakukan Modality Worklist dan MPPS lama antara RIS dan modalitas, serta menambahkan pekerja yang bukan modalitas, dengan klaim dan notifikasi. Banyak modalitas masih hanya berbicara MWL dan MPPS melalui DIMSE; <a href="/medical/net/modality-worklist/">halaman modality worklist</a> menampilkan alur kerja tersebut dengan perpustakaan yang sama.</p>

<p>Halaman terkait: <a href="/medical/net/dicomweb-client/">klien DICOMweb</a> untuk layanan DICOMweb lainnya, <a href="/medical/net/dicomweb-server/">server DICOMweb</a> untuk QIDO‑RS, WADO‑RS, dan STOW‑RS pada server yang sama, serta <a href="/medical/net/dicom-networking/">jaringan DICOM</a> untuk DIMSE.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Sumber Belajar" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentasi" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Panduan Pengembang" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="Referensi API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
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
