---
title: C# .NET'te DICOM Dosyalarını Anonimleştir | Aspose.Medical
weight: 1000
description: C# .NET'te yapılandırılabilir gizlilik profilleri kullanarak DICOM dosyalarını anonimleştirin. Hasta kişisel tanımlayıcı bilgilerini (PII) kaldırın, HIPAA ve GDPR uyumluluğunu Aspose.Medical API ile sağlayın.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#'te DICOM Dosyalarını Anonimleştir" h2="DICOM dosyalarından hasta tanımlayıcı bilgilerini DICOM PS 3.15 gizlilik profilleri kullanarak kaldırın. Saf bir .NET kütüphanesi ile HIPAA ve GDPR uyumluluğunu sağlayın." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Standart Tabanlı DICOM Anonimleştirme">}}

<p><strong>Aspose.Medical for .NET</strong>, <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Gizlilik Profilleri</a> temelinde kapsamlı bir DICOM anonimleştirme API'si sunar. API, sağlık hizmeti geliştiricilerinin DICOM dosyalarındaki hasta tanımlayıcı bilgileri (PII) kaldırmasını veya değiştirmesini sağlarken görüntü verilerinin klinik değerini korur. Basit etiket‑kaldırma yöntemlerinin aksine, Aspose.Medical uygun kimliksizleştirmeyi sağlamak için resmi DICOM standardını takip eder.</p>

<p><code>Anonymizer</code> sınıfı, her DICOM özniteliğinin nasıl işleneceğini tam olarak tanımlayan <code>ConfidentialityProfile</code> nesneleriyle çalışır — kaldırılması, sahte bir değerle değiştirilmesi, temizlenmesi ya da değişmeden bırakılması gerektiğini belirler. Bu yaklaşım, düzenleyici gereksinimleri karşılayan tutarlı ve denetlenebilir bir anonimleştirme sağlar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#'te bir DICOM Dosyasını Anonimleştir">}}

<p>DICOM dosyasını anonimleştirmenin en basit yolu, varsayılan gizlilik profilini kullanmaktır. Bu, tüm standart hasta tanımlayıcı özniteliklerini işleyen DICOM PS 3.15 Temel Profilini uygular:</p>

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

<p><code>Anonymize</code> yöntemi <strong>yıkıcı olmayan</strong> bir işlemdir &mdash; orijinal veri kümesini kopyalar ve yeni bir anonimleştirilmiş kopya döndürür. Orijinal DICOM dosyası değişmeden kalır; bu, denetim izleri ve veri bütünlüğü iş akışları için gereklidir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Özel Hasta Kimliği Değiştirme">}}

<p>Birçok iş akışında, hasta bilgilerini yalnızca kaldırmak yerine belirli takma ad değerleriyle değiştirmek gerekir. <code>ConfidentialityProfile</code> sınıfı, hasta adı ve hasta kimliği için değiştirme değerleri ayarlamanıza olanak tanır:</p>

<div class="codeblock" id="code">
 <h3>Özel hasta kimliği ile anonimleştir - C#</h3>
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

<p>Bellek verimliliğinin önemli olduğu veya orijinal veriyi tutmanız gerekmediği senaryolar için API, veri kümesini doğrudan değiştiren yerinde anonimleştirme sunar:</p>

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

<p>Hem <code>Anonymize</code> hem de <code>AnonymizeInPlace</code> yöntemleri doğrudan bir <code>Dataset</code> nesnesi alır ve işlenecek veri kümesi üzerinde tam kontrol sağlar.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Yapılandırılabilir Gizlilik Profil Seçenekleri">}}

<p>DICOM PS 3.15 standardı, anonimleştirme sırasında belirli veri kategorilerinin nasıl işleneceğini kontrol eden <strong>Temel Profil</strong> ve bir dizi isteğe bağlı profil tanımlar. Bu seçenekleri <code>ConfidentialityProfileOptions</code> enum'ı kullanarak birleştirebilirsiniz:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Seçenek</th>
<th>Açıklama</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>Varsayılan DICOM PS 3.15 Temel Uygulama Seviyesi Gizlilik Profili</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>Anonimleştirme sırasında güvenli özel öznitelikleri tut</td></tr>
<tr><td><code>RetainUIDs</code></td><td>Orijinal DICOM UID'lerini (Çalışma, Seri, Örnek) değiştirmeden tut</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>Aygıt kimlik bilgisi özniteliklerini (üretici, model, istasyon adı) tut</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>Kurum kimlik bilgisi özniteliklerini (ad, adres, bölüm) tut</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>Araştırma amacıyla hasta özelliklerini (yaş, cinsiyet, boy, ağırlık) tut</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>Uzunlamasına çalışmalarda tam tarih ve saat değerlerini tut</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>Uzunlamasına çalışmalarda değiştirilen tarihleri tut</td></tr>
<tr><td><code>CleanDesc</code></td><td>Kimlik bilgisi içerebilecek metin açıklamalarını temizle</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>Kimlik bilgisi içerebilecek yapılandırılmış içeriği temizle</td></tr>
<tr><td><code>CleanGraph</code></td><td>Üzerine yanmış hasta bilgisi içerebilecek grafik verileri temizle</td></tr>
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

<p>Bir gizlilik profilindeki her özniteliğe, anonimleştirme sırasında nasıl işleneceğini tanımlayan bir eylem kodu atanır. Bu kodlar DICOM PS 3.15 Tablo E.1-1a standardını izler:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>Kod</th>
<th>Eylem</th>
<th>Açıklama</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>Taklit ile değiştir</td><td>VR ile uyumlu sıfır olmayan uzunlukta bir değerle değiştir</td></tr>
<tr><td><strong>Z</strong></td><td>Zero‑length veya taklit</td><td>VR ile uyumlu sıfır uzunlukta bir değer ya da taklit bir değerle değiştir</td></tr>
<tr><td><strong>X</strong></td><td>Kaldır</td><td>Özniteliği veri kümesinden tamamen kaldır</td></tr>
<tr><td><strong>K</strong></td><td>Tut</td><td>Özniteliği değişmeden tut (diziler için değil) ya da temizle (diziler için)</td></tr>
<tr><td><strong>C</strong></td><td>Temizle</td><td>VR ile uyumlu benzer anlamda temizlenmiş bir değerle değiştir</td></tr>
<tr><td><strong>U</strong></td><td>UID'yi Değiştir</td><td>Örnek kümesi içinde dahili olarak tutarlı yeni bir UID ile değiştir</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="CSV, JSON ve XML'den Özel Profiller">}}

<p>İleri düzey kullanım senaryoları için kuralları dış CSV, JSON veya XML dosyalarından yükleyerek özel anonimleştirme profilleri tanımlayabilirsiniz. Bu, anonimleştirme davranışını belirli kuruluş politikalarınıza veya düzenleyici gereksinimlerinize göre özelleştirmenizi sağlar:</p>

<div class="codeblock" id="code">
 <h3>CSV'den anonimleştirme profili yükle - C#</h3>
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
 <h3>JSON'dan anonimleştirme profili yükle - C#</h3>
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

<p>Özel profiller XML dosyalarından (<code>LoadFromXmlFile</code>), akışlardan (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) ya da doğrudan dizelerden (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>) da yüklenebilir.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HIPAA ve GDPR Uyumluluğu">}}

<p>DICOM dosyaları rutin olarak HIPAA tarafından tanımlanan Korumalı Sağlık Bilgileri (PHI) ve GDPR tarafından tanımlanan kişisel verileri içerir. Aspose.Medical for .NET, aşağıdakileri sunarak düzenleyici gereksinimlerinizi karşılamanıza yardımcı olur:</p>

<ul>
<li><strong>DICOM PS 3.15 uyumluluğu</strong>: Anonimleştirme motoru, kimliksizleştirme için resmi DICOM standardını takip eder ve tüm ilgili özniteliklere tanınmış en iyi uygulamaları uygular.</li>
<li><strong>Kapsamlı PII kaldırma</strong>: Hasta adı, kimliği, doğum tarihi, adresi ve diğer tanımlayıcılar, yapılandırılmış gizlilik profiline göre işlenir.</li>
<li><strong>UID değiştirme</strong>: Çalışma, Seri ve SOP Örnek UID'leri, çapraz referanslama yoluyla yeniden tanımlamayı önlemek için yeni, dahili olarak tutarlı UID'lerle değiştirilebilir.</li>
<li><strong>Yapılandırılabilir tutma</strong>: Araştırma veya klinik ihtiyaçlar gerektirdiğinde, belirli veri kategorileri (hasta özellikleri, tarihler, aygıt bilgileri) standart seçenek profilleri kullanılarak seçici şekilde tutulabilir.</li>
<li><strong>Denetlenebilir süreç</strong>: Tanımlı eylem kodlarıyla standart tabanlı yaklaşım, düzenleyici denetimler için net ve belgelenebilir bir anonimleştirme süreci sunar.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Öğrenim Kaynakları" tabId="resources" >}}
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
