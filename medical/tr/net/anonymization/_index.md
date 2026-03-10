---
title: C# .NET'te DICOM Dosyalarını Anonimleştir | Aspose.Medical
weight: 1000
description: C# .NET'te yapılandırılabilir gizlilik profilleri kullanarak DICOM dosyalarını anonimleştirin. Hasta kişisel bilgilerini (PII) kaldırın, Aspose.Medical API ile HIPAA ve GDPR uyumluluğunu sağlayın.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'ta DICOM Dosyalarını Anonimleştir" h2="DICOM PS 3.15 gizlilik profilleri kullanarak DICOM dosyalarından hasta tanımlayıcı bilgileri kaldırın. Saf bir .NET kütüphanesi ile HIPAA ve GDPR uyumluluğunu sağlayın." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standart Tabanlı DICOM Anonimleştirme">}}

<p><strong>Aspose.Medical for .NET</strong>, <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Gizlilik Profilleri</a> temelinde kapsamlı bir DICOM anonimleştirme API'si sağlar. API, sağlık bilişim geliştiricilerinin DICOM dosyalarından hasta tanımlayıcı bilgilerini (PII) kaldırmasını veya değiştirmesini sağlarken görüntü verilerinin klinik değerini korur. Basit etiket kaldırma yaklaşımlarının aksine, Aspose.Medical uygun de‑identifikasyonu temin etmek için resmi DICOM standardını takip eder.</p>

<p><code>Anonymizer</code> sınıfı, her DICOM özniteliğinin tam olarak nasıl işleneceğini tanımlayan <code>ConfidentialityProfile</code> nesneleriyle çalışır — kaldırılıp kaldırılmayacağı, sahte bir değerle değiştirileceği, temizleneceği veya değişmeden bırakılacağı. Bu yaklaşım, düzenleyici gereksinimleri karşılayan tutarlı ve denetlenebilir bir anonimleştirme sağlar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#'ta bir DICOM Dosyasını Anonimleştir">}}

Bir DICOM dosyasını anonimleştirmenin en basit yolu, varsayılan gizlilik profilini kullanmaktır. Bu, DICOM PS 3.15 Temel Profilini uygular ve tüm standart hasta tanımlayıcı özniteliklerini işleme alır:

<div class="codeblock" id="code">
 <h3>Temel DICOM anonimleştirme - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p><code>Anonymize</code> yöntemi <strong>yıkıcı olmayan</strong> bir işlemdir &mdash; orijinal veri kümesini klonlar ve yeni bir anonimleştirilmiş kopya döndürür. Orijinal DICOM dosyası değişmeden kalır; bu, denetim izleri ve veri bütünlüğü iş akışları için gereklidir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Özel Hasta Kimliği Değiştirme">}}

<p>Birçok iş akışında, hasta bilgilerini yalnızca kaldırmak yerine belirli takma ad değerleriyle değiştirmek gerekir. <code>ConfidentialityProfile</code> sınıfı, hasta adı ve hasta kimliği için yer değiştirme değerleri ayarlamanıza izin verir:</p>

<div class="codeblock" id="code">
 <h3>Özel hasta kimliğiyle anonimleştir - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="Yerinde Anonimleştirme">}}

<p>Bellek verimliliğinin önemli olduğu veya orijinal veriyi tutmanız gerekmediği senaryolar için API, veri kümesini doğrudan değiştiren yerinde anonimleştirme sağlar:</p>

<div class="codeblock" id="code">
 <h3>Yerinde DICOM anonimleştirme - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>Hem <code>Anonymize</code> hem de <code>AnonymizeInPlace</code> yöntemleri ayrıca bir <code>Dataset</code> nesnesini doğrudan kabul eder ve hangi veri kümesinin işleneceği üzerinde tam kontrol sağlar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Yapılandırılabilir Gizlilik Profili Seçenekleri">}}

<p>DICOM PS 3.15 standardı, anonimleştirme sırasında belirli veri kategorilerinin nasıl işleneceğini kontrol eden bir <strong>Temel Profil</strong> ve çeşitli isteğe bağlı profiller tanımlar. Bu seçenekleri <code>ConfidentialityProfileOptions</code> enum'ı ile birleştirebilirsiniz:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Seçenek</th>
<th>Açıklama</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Varsayılan DICOM PS 3.15 Temel Uygulama Seviyesi Gizlilik Profili</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Anonimleştirme sırasında güvenli özel öznitelikleri koru</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Orijinal DICOM UID'lerini (Study, Series, Instance) değişmeden tut</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Cihaz tanımlama özniteliklerini (üretici, model, istasyon adı) koru</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Kurum tanımlama özniteliklerini (isim, adres, bölüm) koru</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Araştırma amaçlı hasta özelliklerini (yaş, cinsiyet, boy, kilo) koru</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Düşey (longitudinal) çalışmalar için tam tarih ve zaman değerlerini koru</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Düşey çalışmalar için değiştirilmiş tarihleri koru</td></tr>
<tr><td><code>CleanDesc</code></td><td>Tanımlayıcı bilgi içerebilecek metin açıklamalarını temizle</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Tanımlayıcı bilgi içerebilecek yapılandırılmış içeriği temizle</td></tr>
<tr><td><code>CleanGraph</code></td><td>Yanmış hasta bilgisi içerebilecek grafik verileri temizle</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>Belirli profil seçenekleriyle anonimleştir - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Öznitelik İşleme için Eylem Kodları">}}

<p>Bir gizlilik profilindeki her özniteliğe, anonimleştirme sırasında nasıl işleneceğini tanımlayan bir eylem kodu atanır. Bunlar DICOM PS 3.15 Tablo E.1-1a standardına uyar:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Kod</th>
<th>Eylem</th>
<th>Açıklama</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Sahte ile değiştir</td><td>VR ile uyumlu sıfır olmayan uzunlukta bir değerle değiştir</td></tr>
<tr><td><strong>Z</strong></td><td>Sıfır uzunluklu veya sahte</td><td>VR ile uyumlu sıfır uzunluklu bir değer veya sahte bir değerle değiştir</td></tr>
<tr><td><strong>X</strong></td><td>Kaldır</td><td>Özniteliği veri kümesinden tamamen kaldır</td></tr>
<tr><td><strong>K</strong></td><td>Tut</td><td>Özniteliği değişmeden tut (diziler için değilse) veya temizle (diziler için)</td></tr>
<tr><td><strong>C</strong></td><td>Temizle</td><td>VR ile uyumlu, benzer anlamda temizlenmiş bir değerle değiştir</td></tr>
<tr><td><strong>U</strong></td><td>UID'yi Değiştir</td><td>Örnekler kümesi içinde dahili tutarlılığı olan yeni bir UID ile değiştir</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="CSV, JSON ve XML'den Özel Profiller">}}

<p>İleri düzey kullanım senaryoları için, dış CSV, JSON veya XML dosyalarından kurallar yükleyerek özel anonimleştirme profilleri tanımlayabilirsiniz. Bu, anonimleştirme davranışını organizasyonunuzun politikalarına veya düzenleyici gereksinimlere göre özelleştirmenizi sağlar:</p>

<div class="codeblock" id="code">
 <h3>CSV'den anonimleştirme profilini yükle - C#</h3>
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
 <h3>JSON'dan anonimleştirme profilini yükle - C#</h3>
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

<p>Özel profiller XML dosyalarından (<code>LoadFromXmlFile</code>), akışlardan (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) veya doğrudan dizgilerden (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>) da yüklenebilir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HIPAA ve GDPR Uyumluluğu">}}

<p>DICOM dosyaları, HIPAA tarafından tanımlanan Korumalı Sağlık Bilgileri (PHI) ve GDPR tarafından tanımlanan kişisel verileri düzenli olarak içerir. Aspose.Medical for .NET, aşağıdakileri sağlayarak düzenleyici gereksinimlerinizi karşılamanıza yardımcı olur:</p>

<ul>
<li><strong>DICOM PS 3.15 uyumluluğu</strong>: Anonimleştirme motoru, de‑identifikasyon için resmi DICOM standardını izler ve tüm ilgili özniteliklere tanınmış en iyi uygulamaların uygulanmasını sağlar.</li>
<li><strong>Kapsamlı Kişisel Bilgi (PII) kaldırma</strong>: Hasta adı, kimliği, doğum tarihi, adres ve diğer tanımlayıcılar yapılandırılmış gizlilik profiline göre işlenir.</li>
<li><strong>UID değiştirme</strong>: Çalışma, Seri ve SOP Örnek UID'leri, çapraz referanslamayla yeniden tanımlamayı önlemek için yeni, dahili tutarlı UID'lerle değiştirilebilir.</li>
<li><strong>Yapılandırılabilir tutma</strong>: Araştırma veya klinik ihtiyaçlar gerektirdiğinde, belirli veri kategorileri (hasta özellikleri, tarih, cihaz bilgileri) standart seçenek profilleri kullanılarak seçici olarak tutulabilir.</li>
<li><strong>Denetlenebilir süreç</strong>: Tanımlı eylem kodlarıyla standart tabanlı yaklaşım, düzenleyici denetimler için açık ve belgelenebilir bir anonimleştirme süreci sunar.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenme Kaynakları" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Dokümantasyon" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Kaynak Kodu" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API Referansları" href="https://reference.aspose.com/medical/net/" >}}
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
