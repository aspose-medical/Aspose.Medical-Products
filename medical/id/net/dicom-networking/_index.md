---
title: Jaringan DICOM dalam C# .NET - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: Hubungkan aplikasi .NET Anda ke PACS. Verifikasi dengan C-ECHO, kirim gambar dengan C-STORE, kueri dengan C-FIND, dan terima gambar dengan SCP Anda sendiri. Sebuah klien dan server DIMSE dalam C# murni.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Jaringan DICOM dalam .NET C#" h2="Berinteraksi dengan PACS dari aplikasi Anda: C-ECHO, C-STORE, C-FIND, C-MOVE, dan C-GET, sebagai klien maupun server, dalam C# terkelola tanpa harus menginstal apa pun di mesin." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Hubungkan ke PACS dari kode Anda sendiri">}}

<p>Membaca file DICOM adalah setengah bagian mudah dari pencitraan medis. Begitu aplikasi Anda harus bekerja dengan sistem rumah sakit yang nyata, ia harus berbicara DIMSE: membuka asosiasi dengan PACS, mengirim gambar, menanyakan studi apa yang ada, dan menjawab ketika sistem lain mengirim sesuatu kembali.</p>

<p><strong>Aspose.Medical for .NET</strong> menyertakan protokol tersebut sebagai bagian dari pustaka. <code>Aspose.Medical.Dicom.Network</code> memberikan Anda klien DIMSE dan server DIMSE, keduanya ditulis dalam C# terkelola. Tidak ada toolkit native yang harus diinstal, tidak ada layanan yang perlu dikonfigurasi, dan tidak ada yang spesifik platform, sehingga kode yang sama dapat dijalankan di Windows, Linux, dan dalam container.</p>

<p>Tiga hal mencakup sebagian besar integrasi, dan masing‑masingnya hanya beberapa baris kode: periksa sambungan dengan C-ECHO, dorong gambar dengan C-STORE, dan temukan apa yang ada di sisi lain dengan C-FIND.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Mulai dengan C-ECHO">}}

<p>C-ECHO adalah ping DICOM. Ini membuktikan bahwa host, port, dan dua AE title sudah benar sebelum hal lain dipersalahkan. Buat klien satu kali, berikan handler yang mengamati balasan, dan kirim permintaan.</p>

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

<p>Status <code>0x0000</code> berarti berhasil. Permintaan dikeranjangkan dan kemudian dikirim pada satu asosiasi, sehingga sekumpulan pekerjaan tidak membuka koneksi per item.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kirim gambar dengan C-STORE">}}

<p>C-STORE adalah tindakan yang dilakukan aplikasi setelah menghasilkan atau menerima sebuah gambar: mengirimkan instance ke arsip. Antri satu permintaan per instance dan kirim semuanya bersama-sama.</p>

<div class="codeblock" id="code">
 <h3>Kirim file DICOM ke PACS - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>Konteks presentasi yang Anda usulkan menentukan apa yang akan diterima arsip. Jika arsip menginginkan sintaks terkompresi, lakukan transkoding sebelum mengirim, seperti yang ditunjukkan pada halaman <a href="/medical/net/dicom-transfer-syntax-conversion/">konversi sintaks transfer</a>, atau daftar alternatif dalam <code>AdditionalTransferSyntaxes</code> dan biarkan negosiasi memilih satu.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Temukan studi dengan C-FIND">}}

<p>C-FIND menjawab pertanyaan "apa yang dimiliki arsip". Hasil cocok muncul satu per satu, masing‑masing dengan dataset pengidentifikasinya, dan respons akhir menutup kueri.</p>

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

<p>Faktori yang sama membangun tingkat kueri lainnya: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code>, dan <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> membangun kueri worklist modality, yaitu kueri yang diminta modality sebelum pemindaian; halaman <a href="/medical/net/modality-worklist/">worklist modality</a> menampilkannya bersama MPPS dan sisi server worklist.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Terima gambar: SCP penyimpanan Anda sendiri">}}

<p>Pustaka ini juga berfungsi sebagai server. Daftarkan handler untuk layanan yang ingin Anda tawarkan, mulai mendengarkan, dan aplikasi Anda menjadi node DICOM yang dapat dikirimkan oleh modality atau PACS lain.</p>

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

<p>Apa yang dilakukan handler dengan dataset adalah keputusan Anda: menulisnya ke disk, menempatkannya dalam antrian, meng-anonimkan terlebih dahulu dengan <a href="/medical/net/anonymization/">API anonimasi</a>, atau mentranskodenya ke dalam sintaks arsip.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Apa lagi yang dicakup oleh API jaringan">}}

<p>Tiga layanan di atas adalah yang paling umum. Sisanya dari DIMSE juga tersedia:</p>

<ul>
<li>Pengambilan: C-MOVE dan C-GET, dengan penghitung sub‑operasi yang dilaporkan pada respons.</li>
<li>Layanan N: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE, dan N-EVENT-REPORT, yang menjadi dasar pembuatan storage commitment dan MPPS.</li>
<li>Kontrol asosiasi: konteks presentasi, peran kelas layanan, negosiasi lanjutan, jendela operasi asinkron, negosiasi identitas pengguna, dan hook kebijakan yang dapat menolak asosiasi dengan memanggil AE title.</li>
<li>TLS di kedua sisi, melalui <code>TlsInitiatorAuthenticator</code> dan <code>TlsAcceptorAuthenticator</code>, dengan validasi sertifikat Anda sendiri jika diperlukan.</li>
<li>Timeout untuk setiap tahap, mulai dari koneksi TCP hingga release, serta notifikasi untuk siklus hidup asosiasi sehingga node yang berjalan lama dapat mencatat apa yang terjadi.</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">Panduan jaringan DICOM</a> mendokumentasikan setiap opsi dan setiap handler.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Murni .NET, dari soket hingga data piksel">}}

<p>Semua yang ada di halaman ini adalah kode terkelola dari paket yang sama yang membaca dan menulis file. Asosiasi, codec, dan parser berasal dari satu pustaka, sehingga studi yang diterima melalui jaringan dapat di-anonimkan, ditranskode, atau diserialisasi tanpa keluar dari proses dan tanpa ketergantungan native di mana pun dalam rantai.</p>

<p>Halaman terkait: <a href="/medical/net/dicom-transfer-syntax-conversion/">konversi sintaks transfer</a> untuk apa yang dikirim, <a href="/medical/net/anonymization/">anonimisasi</a> untuk apa yang harus dihapus terlebih dahulu, dan <a href="/medical/net/dicom-tags/">tag DICOM</a> untuk membaca apa yang diterima.</p>

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
{{< blocks/products/pf/slr-element name="Kisah Sukses" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
