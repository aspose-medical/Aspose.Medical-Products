---
title: Anonimisasi File DICOM dalam C# .NET | Aspose.Medical
weight: 1000
description: Anonimisasi file DICOM dalam C# .NET menggunakan profil kerahasiaan yang dapat dikonfigurasi. Hapus PII pasien, pastikan kepatuhan HIPAA dan GDPR dengan API Aspose.Medical.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="Anonimisasi File DICOM dalam .NET C#" h2="Hapus informasi identifikasi pasien dari file DICOM menggunakan profil kerahasiaan DICOM PS 3.15. Pastikan kepatuhan HIPAA dan GDPR dengan perpustakaan .NET murni." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Anonimisasi DICOM Berbasis Standar">}}

<p><strong>Aspose.Medical for .NET</strong> menyediakan API anonimisasi DICOM yang komprehensif berdasarkan <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">Profil Kerahasiaan DICOM PS 3.15</a>. API ini memungkinkan pengembang layanan kesehatan untuk menghapus atau memodifikasi informasi identifikasi pasien (PII) dari file DICOM sambil mempertahankan nilai klinis data pencitraan. Tidak seperti pendekatan penghapusan tag sederhana, Aspose.Medical mengikuti standar resmi DICOM untuk memastikan de‑identifikasi yang tepat.</p>

<p>Kelas <code>Anonymizer</code> bekerja dengan objek <code>ConfidentialityProfile</code> yang menentukan secara tepat bagaimana setiap atribut DICOM harus diproses &mdash; apakah harus dihapus, diganti dengan nilai tiruan, dibersihkan, atau dibiarkan tidak berubah. Pendekatan ini memastikan anonimisasi yang konsisten dan dapat diaudit serta memenuhi persyaratan regulatori.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonimkan File DICOM dalam C#">}}

<p>Cara paling sederhana untuk anonimasi file DICOM adalah menggunakan profil kerahasiaan default. Ini menerapkan Profil Dasar DICOM PS 3.15, yang menangani semua atribut standar yang mengidentifikasi pasien:</p>

<div class="codeblock" id="code">
 <h3>Anonimisasi DICOM Dasar - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>Metode <code>Anonymize</code> bersifat <strong>non-destruktif</strong> &mdash; metode ini menggandakan dataset asli dan mengembalikan salinan anonim baru. File DICOM asli tetap tidak berubah, yang penting untuk jejak audit dan alur kerja integritas data.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Penggantian Identitas Pasien Kustom">}}

<p>Dalam banyak alur kerja, Anda perlu mengganti informasi pasien dengan nilai alias khusus alih-alih sekadar menghapusnya. Kelas <code>ConfidentialityProfile</code> memungkinkan Anda menetapkan nilai pengganti untuk nama pasien dan ID pasien:</p>

<div class="codeblock" id="code">
 <h3>Anonimisasi dengan identitas pasien kustom - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create a confidentiality profile with custom replacement values
ConfidentialityProfile profile = new()
{
    PatientName = "ANONYMOUS PATIENT",
    PatientId = "00000000"
};

// Create an anonymizer with this profile
Anonymizer anonymizer = new(profile);

// Anonymize the file
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("custom_anonymized.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Anonimisasi Secara In-Place">}}

<p>Untuk skenario di mana efisiensi memori penting atau Anda tidak perlu menyimpan data asli, API menyediakan anonimisasi in-place yang memodifikasi dataset secara langsung:</p>

<div class="codeblock" id="code">
 <h3>Anonimisasi DICOM In-place - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Baik metode <code>Anonymize</code> maupun <code>AnonymizeInPlace</code> juga menerima objek <code>Dataset</code> secara langsung, memberi Anda kontrol penuh atas dataset yang diproses.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Opsi Profil Kerahasiaan yang Dapat Dikonfigurasi">}}

<p>Standar DICOM PS 3.15 mendefinisikan <strong>Profil Dasar</strong> beserta beberapa profil opsional yang mengatur cara kategori data tertentu diproses selama anonimisasi. Anda dapat menggabungkan opsi-opsi ini menggunakan enum <code>ConfidentialityProfileOptions</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Opsi</th>
<th>Deskripsi</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Profil Kerahasiaan Tingkat Aplikasi Dasar DICOM PS 3.15 default</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Pertahankan atribut pribadi yang aman selama anonimisasi</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Pertahankan UID DICOM asli (Study, Series, Instance) tidak berubah</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Pertahankan atribut identifikasi perangkat (pabrikan, model, nama stasiun)</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Pertahankan atribut identifikasi institusi (nama, alamat, departemen)</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Pertahankan karakteristik pasien (usia, jenis kelamin, ukuran, berat) untuk keperluan riset</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Pertahankan nilai tanggal dan waktu lengkap untuk studi longitudinal</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Pertahankan tanggal yang dimodifikasi untuk studi longitudinal</td></tr>
<tr><td><code>CleanDesc</code></td><td>Bersihkan deskripsi teks yang mungkin mengandung informasi identifikasi</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Bersihkan konten terstruktur yang mungkin mengandung informasi identifikasi</td></tr>
<tr><td><code>CleanGraph</code></td><td>Bersihkan data grafis yang mungkin berisi informasi pasien yang terbakar (burned‑in)</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Anonimisasi dengan opsi profil spesifik - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kode Aksi untuk Penanganan Atribut">}}

<p>Setiap atribut dalam profil kerahasiaan diberikan kode aksi yang menentukan bagaimana atribut tersebut diproses selama anonimisasi. Kode ini mengikuti standar Tabel E.1-1a DICOM PS 3.15:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Kode</th>
<th>Aksi</th>
<th>Deskripsi</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Ganti dengan nilai tiruan</td><td>Ganti dengan nilai panjang tidak nol yang sesuai dengan VR</td></tr>
<tr><td><strong>Z</strong></td><td>Panjang nol atau tiruan</td><td>Ganti dengan nilai panjang nol atau nilai tiruan yang sesuai dengan VR</td></tr>
<tr><td><strong>X</strong></td><td>Hapus</td><td>Hapus atribut sepenuhnya dari dataset</td></tr>
<tr><td><strong>K</strong></td><td>Pertahankan</td><td>Pertahankan atribut tidak berubah (untuk non‑sekuens) atau bersihkan (untuk sekuens)</td></tr>
<tr><td><strong>C</strong></td><td>Bersihkan</td><td>Ganti dengan nilai bersih dengan makna serupa yang sesuai dengan VR</td></tr>
<tr><td><strong>U</strong></td><td>Ganti UID</td><td>Ganti dengan UID baru yang secara internal konsisten dalam kumpulan instance</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Profil Kustom dari CSV, JSON, dan XML">}}

<p>Untuk kasus penggunaan lanjutan, Anda dapat mendefinisikan profil anonimisasi kustom dengan memuat aturan dari file CSV, JSON, atau XML eksternal. Hal ini memungkinkan Anda menyesuaikan perilaku anonimisasi dengan kebijakan organisasi atau persyaratan regulatori spesifik Anda:</p>

<div class="codeblock" id="code">
 <h3>Muat profil anonimisasi dari CSV - C#</h3>
 <pre><code class="cs">// CSV format: TagPattern;Action
// 0010,0010;Z   (Anonymize PatientName)
// 0010,0020;D   (Replace PatientID with dummy)
// 0020,000D;U   (Replace StudyInstanceUID)

var profile = ConfidentialityProfile.LoadFromCsvFile(
    "anonymization_rules.csv",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>Muat profil anonimisasi dari JSON - C#</h3>
 <pre><code class="cs">// JSON format:
// [
//     { "Tag": "0010,0010", "Action": "Z" },
//     { "Tag": "0010,0020", "Action": "D" },
//     { "Tag": "0020,000D", "Action": "U" }
// ]

var profile = ConfidentialityProfile.LoadFromJsonFile(
    "anonymization_rules.json",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<p>Profil kustom juga dapat dimuat dari file XML (<code>LoadFromXmlFile</code>), aliran (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>), atau langsung dari string (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>).</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Kepatuhan HIPAA dan GDPR">}}

<p>File DICOM secara rutin berisi Informasi Kesehatan yang Dilindungi (PHI) sebagaimana didefinisikan oleh HIPAA dan data pribadi sebagaimana didefinisikan oleh GDPR. Aspose.Medical untuk .NET membantu Anda memenuhi persyaratan regulatori dengan menyediakan:</p>

<ul>
<li><strong>Kepatuhan DICOM PS 3.15</strong>: Mesin anonimisasi mengikuti standar resmi DICOM untuk de‑identifikasi, memastikan praktik terbaik yang diakui diterapkan pada semua atribut yang relevan.</li>
<li><strong>Penghapusan PII yang komprehensif</strong>: Nama pasien, ID, tanggal lahir, alamat, dan pengidentifikasi lainnya ditangani sesuai dengan profil kerahasiaan yang dikonfigurasi.</li>
<li><strong>Penggantian UID</strong>: UID Study, Series, dan SOP Instance dapat diganti dengan UID baru yang secara internal konsisten untuk mencegah re‑identifikasi melalui referensi silang.</li>
<li><strong>Retensi yang dapat dikonfigurasi</strong>: Ketika kebutuhan riset atau klinis memerlukannya, kategori data tertentu (karakteristik pasien, tanggal, informasi perangkat) dapat dipertahankan secara selektif menggunakan profil opsi standar.</li>
<li><strong>Proses yang dapat diaudit</strong>: Pendekatan berbasis standar dengan kode aksi yang ditetapkan memberikan proses anonimisasi yang jelas dan dapat didokumentasikan untuk audit regulatori.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Sumber Pembelajaran" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokumentasi" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Kode Sumber" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
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
