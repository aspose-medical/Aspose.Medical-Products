---
title: C# .NET में DICOMweb क्लाइंट - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: .NET से HTTP के माध्यम से DICOM स्टडीज़ को खोजें, डाउनलोड करें और अपलोड करें। QIDO-RS, WADO-RS और STOW-RS के लिए शुद्ध C# में DICOMweb क्लाइंट, जो PACS, VNA और क्लाउड इमेजिंग सेवाओं के साथ काम करता है।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# के लिए DICOMweb क्लाइंट" h2="QIDO-RS, WADO-RS और STOW-RS के साथ किसी भी DICOMweb सर्वर पर स्टडीज़ को खोजें, डाउनलोड करें और अपलोड करें, प्रबंधित C# में, मशीन पर कुछ भी स्थापित करने की आवश्यकता नहीं।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="अपने कोड से HTTP पर DICOM">}}

<p>DICOMweb, DICOM का HTTP रूप है। पोर्ट 104 पर DIMSE कनेक्शन के बजाय, आपका एप्लिकेशन सामान्य HTTPS अनुरोध भेजता है: खोज के लिए QIDO-RS, डाउनलोड के लिए WADO-RS, अपलोड के लिए STOW-RS। Azure Health Data Services DICOM सेवा और Google Cloud Healthcare API जैसी क्लाउड इमेजिंग सेवाएँ इसी तरह काम करती हैं, तथा Orthanc और dcm4chee जैसे आर्काइव भी।</p>

<p><strong>Aspose.Medical for .NET</strong> संस्करण 26.10 से DICOMweb क्लाइंट शामिल करता है। <code>DicomWebClient</code> हर मानक लेन‑देन को टाइपेड मेथड में लपेटता है, और परिणाम उसी <code>Dataset</code> और <code>DicomFile</code> ऑब्जेक्ट्स के रूप में आते हैं जो लाइब्रेरी के बाकी हिस्से पढ़ते और लिखते हैं। दूसरी ओर का सर्वर कोई भी DICOMweb कार्यान्वयन हो सकता है; इसे Aspose.Medical से बना होना आवश्यक नहीं है।</p>

<p>क्लाइंट <code>HttpClient</code> के ऊपर प्रबंधित C# है। कोई नेटिव टूलकिट या प्रॉक्सी सेवा नहीं है, इसलिए वही कोड Windows, Linux और कंटेनर में चलता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="एक DICOMweb सर्वर से कनेक्ट करें">}}

<p>क्लाइंट को एक <code>HttpClient</code> और सर्वर की सेवा रूट URL से बनाएं। प्रमाणीकरण, TLS सेटिंग्स, प्रॉक्सी और कनेक्शन पूलिंग <code>HttpClient</code> पर ही रहती हैं, जहाँ आपका एप्लिकेशन पहले से ही उन्हें कॉन्फ़िगर करता है। क्लाइंट <code>HttpClient</code> का स्वामित्व नहीं रखता और न ही उसे डिस्पोज़ करता है।</p>

<div class="codeblock" id="code">
 <h3>DICOMweb क्लाइंट बनाएं - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>यदि कोई डिप्लॉयमेंट WADO-URI या अन्य सेवा को अलग URL से सर्व करता है, तो <code>ServiceBaseUris</code> केवल उस सेवा के लिए रूट को ओवरराइड करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="QIDO-RS के साथ स्टडीज़ खोजें">}}

<p>सर्च में DICOM कीवर्ड को नाम के रूप में, वापस करने वाले फ़ील्ड्स और पेजिंग नियंत्रणों के साथ मिलान पैरामीटर लेता है। मिलान परिणाम एक dataset की स्ट्रीम के रूप में आते हैं, जिन्हें प्रतिक्रिया अभी डाउनलोड हो रही होने के दौरान एक‑एक करके पढ़ा जाता है।</p>

<div class="codeblock" id="code">
 <h3>एक रोगी की स्टडीज़ खोजें - C#</h3>
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

<p><code>SearchSeriesAsync</code> और <code>SearchInstancesAsync</code> एक स्तर नीचे खोज करते हैं, पूरे आर्काइव में या किसी एक स्टडी या सीरीज़ के भीतर। वही विकल्प fuzzy matching, खाली मान मिलान और पेजिंग के लिए result offset भी सेट करते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="WADO-RS के साथ एक स्टडी डाउनलोड करें">}}

<p>एक स्टडी में हजारों इंस्टेंस हो सकते हैं। <code>RetrieveStudyAsync</code> उन्हें स्ट्रीम के रूप में लौटाता है, इसलिए प्रत्येक फ़ाइल को उसके हिस्से के रूप में जब प्रतिक्रिया आती है, तब पार्स किया जाता है और अगली फ़ाइल पढ़े जाने से पहले सहेजा जा सकता है। मेमोरी उपयोग स्टडी के आकार के साथ नहीं बढ़ता।</p>

<div class="codeblock" id="code">
 <h3>स्टडी के प्रत्येक इंस्टेंस को सेव करें - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> और <code>RetrieveInstanceAsync</code> एक सीरीज़ या एक इंस्टेंस को उसी तरीके से डाउनलोड करते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="एक थंबनेल या रेंडर किया गया इमेज प्राप्त करें">}}

<p>एक व्यूअर या वर्कलिस्ट स्क्रीन अक्सर एक चित्र की आवश्यकता रखती है, DICOM फ़ाइल नहीं। सर्वर इसे रेंडर कर JPEG या PNG के रूप में भेजता है, इसलिए क्लाइंट मूल पिक्सेल डेटा को डाउनलोड या डीकोड नहीं करता।</p>

<div class="codeblock" id="code">
 <h3>एक इंस्टेंस का JPEG थंबनेल सेव करें - C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> और <code>RetrieveRenderedFramesAsync</code> इसके बजाय पूर्ण‑आकार की इमेज लौटाते हैं। <code>DicomWebRenderOptions</code> मानक रेंडरिंग नियंत्रण (इमेज क्वालिटी, व्यूपोर्ट आकार, विंडो और ICC प्रोफ़ाइल) सर्वर को भेजता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="STOW-RS के साथ इंस्टेंस अपलोड करें">}}

<p>STOW-RS एक अनुरोध में किसी भी संख्या में इंस्टेंस भेजता है। सर्वर एक रिपोर्ट के साथ जवाब देता है जिसमें इंस्टेंस दर इंस्टेंस बताया जाता है कि उसने क्या संग्रहीत किया और क्या अस्वीकार किया।</p>

<div class="codeblock" id="code">
 <h3>DICOM फ़ाइलों के फ़ोल्डर को अपलोड करें - C#</h3>
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

<p>कुछ सर्वर जब एक या अधिक इंस्टेंस फेल होते हैं तो HTTP त्रुटि (जैसे 409) के साथ जवाब देते हैं। तब कॉल <code>DicomWebClientException</code> फेंकता है, जैसे अगला अनुभाग दर्शाता है, और वही रिपोर्ट अपवाद के प्रतिक्रिया बॉडी में होती है।</p>

<p><code>StoreStudyAsync</code> एक नामित स्टडी में अपलोड करता है। दोनों मेथड्स <code>IAsyncEnumerable&lt;DicomFile&gt;</code> को भी स्वीकार करते हैं, ताकि फ़ाइलें अनुरोध भेजे जाने के दौरान उत्पन्न हो सकें।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="सर्वर त्रुटियों को संभालें">}}

<p>हर टाइप्ड ऑपरेशन रिटर्न होने से पहले HTTP स्टेटस जाँचता है। जब सर्वर अनुरोध को अस्वीकार करता है, तो कॉल <code>DicomWebClientException</code> फेंकता है, जिसमें स्टेटस कोड, हेडर्स और लॉग के लिए प्रतिक्रिया बॉडी की शुरुआत शामिल होती है।</p>

<div class="codeblock" id="code">
 <h3>एक विफल अनुरोध की रिपोर्ट करें - C#</h3>
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

<p>वे मेथड्स जो रॉ प्रतिक्रिया लौटाते हैं, जैसे उपरोक्त थंबनेल, आपको <code>IsSuccessStatusCode</code> या <code>EnsureSuccessStatusCode</code> के माध्यम से स्टेटस जाँच करने देते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="क्लाइंट अन्य क्या-क्या कवर करता है">}}

<p>सर्च, डाउनलोड और अपलोड सामान्य कॉल हैं। क्लाइंट के पास बाकी DICOMweb सेवाओं के लिए भी प्रत्येक का एक ऑपरेशन समूह है:</p>

<ul>
<li>एक इंस्टेंस के Bulk डेटा, पिक्सेल डेटा और चुने हुए फ्रेम, एक‑आधारित फ्रेम नंबर द्वारा, जैसा सर्वर ने भेजा है।</li>
<li>WADO-URI, पुराना सिंगल‑URL रिट्रीवल जिसे कई व्यूअर और आर्काइव अभी भी उपयोग करते हैं, DICOM फ़ाइलों और रेंडर की गई इमेजेज़ के लिए।</li>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> वर्कलिस्ट: कार्य आइटम बनाना, खोज, अपडेट और क्लेम करना, उनकी स्थिति बदलना, रद्दीकरण का अनुरोध करना, और वेबसॉकेट के माध्यम से आने वाले इवेंट्स को सब्सक्राइब करना।</li>
<li><a href="/medical/net/modality-worklist/">modality worklist</a> (MWL-RS), modality performed procedure steps (MPPS-RS) और storage commitment।</li>
<li>रोगी‑बाह्य इंस्टेंस जैसे कलर पैलेट्स और हैंगिंग प्रोटोकॉल, तथा सर्वर का क्षमताओं दस्तावेज़।</li>
<li>जब कोई टाइप्ड मेथड उपयुक्त न हो, तो अपने रूट वैल्यूज़, क्वेरी पैरामीटर्स, हेडर्स और बॉडी के साथ किसी भी मानक लेन‑देन के लिए <code>SendAsync</code> उपयोग करें।</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb क्लाइंट गाइड</a> प्रत्येक ऑपरेशन और विकल्प को दस्तावेज़ करता है, और <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">क्लाइंट ऑपरेशन रेफरेंस</a> सभी मेथड्स की सूची देता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTTP प्रतिक्रिया से पिक्सेल डेटा तक एक ही लाइब्रेरी">}}

<p>क्लाइंट से डाउनलोड की गई फ़ाइल एक सामान्य <code>DicomFile</code> है। इसे उसी प्रक्रिया में अनामित, ट्रांसकोड, रेंडर या सीरियलाइज़ किया जा सकता है, बिना किसी परिवर्तित चरण के और बिना चेन में दूसरी लाइब्रेरी के।</p>

<p>संबंधित पृष्ठ: <a href="/medical/net/dicomweb-server/">DICOMweb सर्वर</a> आपके स्वयं के ASP.NET Core सेवा में प्रोटोकॉल के दूसरे पक्ष के लिए, <a href="/medical/net/dicom-networking/">DICOM नेटवर्किंग</a> उन आर्काइव के लिए जो HTTP के बजाय DIMSE का उपयोग करते हैं, <a href="/medical/net/anonymization/">अनॉनिमाइज़ेशन</a> जब एक स्टडी अस्पताल छोड़ने से पहले, <a href="/medical/net/dicom-transfer-syntax-conversion/">ट्रांसफ़र सिंटैक्स कन्वर्ज़न</a> उन डेटा के लिए जो सर्वर संग्रहीत करता है, और <a href="/medical/net/dicom-to-json/">DICOM से JSON</a> उस स्वरूप के लिए जिसका QIDO-RS सर्च परिणाम उपयोग करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="API रेफ़रेंसेज़" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="निःशुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="भुगतान योग्य समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Aspose.Medical .NET के लिए क्यों?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता की कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
