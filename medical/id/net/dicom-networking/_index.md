---
title: Jaringan DICOM dalam C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Hubungkan aplikasi .NET Anda ke PACS. Verifikasi dengan C-ECHO, kirim gambar dengan C-STORE, kueri dengan C-FIND, dan terima gambar dengan SCP Anda sendiri. Sebuah klien dan server DIMSE dalam C# murni.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Jaringan DICOM dalam .NET C#" h2="Berinteraksi dengan PACS dari aplikasi Anda: C-ECHO, C-STORE, C-FIND, C-MOVE, dan C-GET, baik sebagai klien maupun server, dalam C# yang dikelola tanpa perlu menginstal apa pun di mesin." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Hubungkan ke PACS dari kode Anda sendiri">}}

<p>Membaca file DICOM adalah setengah mudah dari pencitraan medis. Begitu aplikasi Anda harus berinteraksi dengan sistem rumah sakit nyata, ia harus berbicara DIMSE: membuka asosiasi dengan PACS, mengirim gambar, menanyakan studi apa yang ada, dan merespons ketika sistem lain mengirim sesuatu kembali.</p>

<p><strong>Aspose.Medical for .NET</strong> menyertakan protokol itu sebagai bagian dari perpustakaan. <code>Aspose.Medical.Dicom.Network</code> memberikan Anda klien DIMSE dan server DIMSE, keduanya ditulis dalam C# yang dikelola. Tidak ada toolkit native yang harus diinstal, tidak ada layanan yang perlu dikonfigurasi, dan tidak ada yang spesifik platform, sehingga kode yang sama dapat dijalankan di Windows, Linux, dan dalam container.</p>

<p>Tiga hal mencakup sebagian besar integrasi, dan masing‑masing hanya beberapa baris kode: memeriksa koneksi dengan C‑ECHO, mengirim gambar dengan C‑STORE, dan menemukan apa yang ada di sisi lain dengan C‑FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Mulai dengan C-ECHO">}}

<p>C-ECHO adalah ping DICOM. Ini membuktikan bahwa host, port, dan dua judul AE sudah benar sebelum ada kesalahan lainnya. Buat satu klien, berikan handler yang memantau jawaban, dan kirim permintaan.</p>

<div class="codeblock" id="code">
 <h3>Verifikasi koneksi dengan C-ECHO - C#</h3>
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

<p>Status <code>0x0000</code> berarti sukses. Permintaan dikelola dalam antrian dan kemudian dikirim dalam satu asosiasi, sehingga sekumpulan pekerjaan tidak membuka koneksi per item.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kirim gambar dengan C-STORE">}}

<p>C-STORE adalah apa yang dilakukan aplikasi setelah menghasilkan atau menerima sebuah gambar: ia mengirimkan instansi ke arsip. Antrikan satu permintaan per instansi dan kirimkan bersama-sama.</p>

<div class="codeblock" id="code">
 <h3>Kirim file DICOM ke PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Konteks presentasi yang Anda usulkan menentukan apa yang akan diterima arsip. Jika menginginkan sintaks terkompresi, lakukan transkode sebelum mengirim, seperti yang ditunjukkan pada halaman <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a>, atau daftarkan alternatif dalam <code>AdditionalTransferSyntaxes</code> dan biarkan negosiasi memilih satu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Temukan studi dengan C-FIND">}}

<p>C-FIND menjawab pertanyaan "apa yang dimiliki arsip". Hasil cocok datang satu per satu, masing‑masing dengan dataset pengidentifikasi sendiri, dan respons akhir menutup kueri.</p>

<div class="codeblock" id="code">
 <h3>Kueri studi berdasarkan pasien - C#</h3>
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

<p>Fabrik yang sama membangun tingkat kueri lainnya: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code>, dan <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> membuat kueri worklist modality, yaitu kueri yang diajukan modality sebelum pemindaian.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Terima gambar: SCP penyimpanan Anda sendiri">}}

<p>Perpustakaan ini juga berfungsi sebagai server. Daftarkan handler untuk layanan yang ingin Anda tawarkan, mulai mendengarkan, dan aplikasi Anda menjadi node DICOM yang dapat dikirimkan oleh modality atau PACS lain.</p>

<div class="codeblock" id="code">
 <h3>Terima gambar masuk - C#</h3>
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
 <h3>Handler yang menyimpan apa yang datang - C#</h3>
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

<p>Apa yang dilakukan handler dengan dataset adalah keputusan Anda: menulisnya ke disk, menaruhnya dalam antrian, menganonimkan terlebih dahulu dengan <a href="/medical/net/anonymization/">anonymization API</a>, atau mentranskode ke dalam sintaks arsip.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Apa lagi yang dicakup oleh API jaringan">}}

<p>Tiga layanan di atas adalah yang umum. Sisanya dari DIMSE juga tersedia:</p>

<ul>
<li>Pengambilan: C-MOVE dan C-GET, dengan penghitung sub‑operasi dilaporkan pada respons.</li>
<li>Layanan N: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE, dan N-EVENT-REPORT, yang menjadi dasar dari storage commitment dan MPPS.</li>
<li>Kontrol asosiasi: konteks presentasi, peran kelas layanan, negosiasi diperluas, jendela operasi asinkron, negosiasi identitas pengguna, dan hook kebijakan yang dapat menolak asosiasi dengan memanggil judul AE.</li>
<li>TLS pada kedua sisi, melalui <code>TlsInitiatorAuthenticator</code> dan <code>TlsAcceptorAuthenticator</code>, dengan validasi sertifikat Anda sendiri bila diperlukan.</li>
<li>Timeout untuk setiap tahap, dari koneksi TCP hingga rilis, serta notifikasi untuk siklus hidup asosiasi sehingga node yang berjalan lama dapat mencatat apa yang terjadi.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">Panduan jaringan DICOM</a> mendokumentasikan setiap opsi dan setiap handler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Murni .NET, dari socket hingga data piksel">}}

<p>Semua yang ada di halaman ini adalah kode yang dikelola dari paket yang sama yang membaca dan menulis file. Asosiasi, codec, dan parser berasal dari satu perpustakaan, sehingga studi yang diterima melalui jaringan dapat dianonimkan, ditranskode, atau diserialisasi tanpa meninggalkan proses dan tanpa ketergantungan native di mana pun dalam rantai.</p>

<p>Halaman terkait: <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> untuk apa yang akan dikirim, <a href="/medical/net/anonymization/">anonymization</a> untuk apa yang harus dihapus terlebih dahulu, dan <a href="/medical/net/dicom-tags/">DICOM tags</a> untuk membaca apa yang diterima.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Sumber Belajar" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentasi" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Panduan Pengembang" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="Referensi API" href="https://reference.aspose.com/medical/net/" >}}
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
