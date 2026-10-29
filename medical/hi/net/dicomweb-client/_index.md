---
title: C# .NET में DICOMweb क्लाइंट - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: .NET से HTTP के माध्यम से DICOM स्टडीज़ को खोजें, डाउनलोड करें और अपलोड करें। QIDO-RS, WADO-RS और STOW-RS के लिए शुद्ध C# में एक DICOMweb क्लाइंट जो PACS, VNA और क्लाउड इमेजिंग सर्विसेज़ के साथ कार्य करता है।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# के लिए DICOMweb क्लाइंट" h2="QIDO-RS, WADO-RS और STOW-RS के साथ किसी भी DICOMweb सर्वर पर स्टडीज़ को खोजें, डाउनलोड करें और अपलोड करें, प्रबंधित C# में और मशीन पर कुछ भी इंस्टॉल करने की आवश्यकता नहीं।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="HTTP पर DICOM, अपने कोड से">}}

<p>DICOMweb, DICOM का HTTP रूप है। पोर्ट 104 पर DIMSE एसोसीऐशन के बजाय, आपके अनुप्रयोग सामान्य HTTPS अनुरोध भेजते हैं: खोज के लिए QIDO-RS, डाउनलोड के लिए WADO-RS, अपलोड के लिए STOW-RS। Azure Health Data Services DICOM सेवा और Google Cloud Healthcare API जैसी क्लाउड इमेजिंग सर्विसेज़ इसी तरह काम करती हैं, और Orthanc तथा dcm4chee जैसी आर्काइव्स भी।</p>

<p><strong>Aspose.Medical for .NET</strong> संस्करण 26.10 से DICOMweb क्लाइंट शामिल करता है। <code>DicomWebClient</code> प्रत्येक मानक लेन‑देन को टाइप्ड मेथड में लपेटता है, और परिणाम वही <code>Dataset</code> और <code>DicomFile</code> ऑब्जेक्ट्स के रूप में आते हैं जो लाइब्रेरी बाकी हिस्सों द्वारा पढ़े‑लिखे जाते हैं। दूसरे पक्ष का सर्वर कोई भी DICOMweb इम्प्लीमेंटेशन हो सकता है; इसे Aspose.Medical के साथ निर्मित होने की आवश्यकता नहीं है।</p>

<p>क्लाइंट <code>HttpClient</code> के ऊपर प्रबंधित C# है। कोई नेटिव टूलकिट या प्रॉक्सी सेवा नहीं है, इसलिए वही कोड Windows, Linux और कंटेनर में चलता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOMweb सर्वर से कनेक्ट करें">}}

<p>क्लाइंट को एक <code>HttpClient</code> और सर्वर की सर्विस रूट URL से बनाएं। प्रमाणन, TLS सेटिंग्स, प्रॉक्सी और कनेक्शन पूलिंग <code>HttpClient</code> पर रहती हैं, जहाँ आपका अनुप्रयोग इन्हें पहले ही कॉन्फ़िगर करता है। क्लाइंट <code>HttpClient</code> का मालिक नहीं है और इसे डिस्पोज़ नहीं करता।</p>

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

<p>यदि कोई डिप्लॉयमेंट WADO-URI या किसी अन्य सेवा को अलग URL से सर्व करता है, तो <code>ServiceBaseUris</code> केवल उसी सेवा के लिए रूट को ओवरराइड करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="QIDO-RS के साथ स्टडीज़ खोजें">}}

<p>खोज में DICOM कीवर्ड को नाम के रूप में, मिलान पैरामीटर, लौटाने वाले फ़ील्ड और पेजिंग कंट्रोल्स शामिल होते हैं। मिलान परिणाम एक डेटासेट स्ट्रीम के रूप में आते हैं, जिन्हें प्रतिक्रिया अभी भी डाउनलोड हो रही हो तो एक‑एक करके पढ़ा जाता है।</p>

<div class="codeblock" id="code">
 <h3>किसी रोगी की स्टडीज़ खोजें - C#</h3>
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

<p><code>SearchSeriesAsync</code> और <code>SearchInstancesAsync</code> एक स्तर नीचे खोज करते हैं, पूरे आर्काइव में या किसी एक स्टडी या सीरीज़ के भीतर। वही विकल्प फज़ी मैचिंग, खाली मूल्य मैचिंग और पेजिंग के लिए परिणाम ऑफ़सेट भी सेट करते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="WADO-RS के साथ स्टडी डाउनलोड करें">}}

<p>एक स्टडी में हजारों इंस्टेंस हो सकते हैं। <code>RetrieveStudyAsync</code> उन्हें स्ट्रीम के रूप में लौटाता है, इसलिए प्रत्येक फ़ाइल को प्रतिक्रिया का हिस्सा आते ही पार्स किया जाता है और अगली फ़ाइल पढ़े जाने से पहले सहेजा जा सकता है। मेमोरी उपयोग स्टडी के आकार के साथ नहीं बढ़ता।</p>

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

<p><code>RetrieveSeriesAsync</code> और <code>RetrieveInstanceAsync</code> एक श्रृंखला या एक इंस्टेंस को समान तरीके से डाउनलोड करते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="एक थंबनेल या रेंडर्ड इमेज प्राप्त करें">}}

<p>व्यूअर या वर्कलिस्ट स्क्रीन को अक्सर चित्र चाहिए होता है, न कि DICOM फ़ाइल। सर्वर इसे रेंडर करता है और JPEG या PNG भेजता है, इसलिए क्लाइंट मूल पिक्सेल डेटा को डाउनलोड या डिकोड नहीं करता।</p>

<div class="codeblock" id="code">
 <h3>इंस्टेंस का JPEG थंबनेल सेव करें - C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> और <code>RetrieveRenderedFramesAsync</code> इसके बजाय पूर्ण‑आकार की इमेज लौटाते हैं। <code>DicomWebRenderOptions</code> मानक रेंडरिंग कंट्रोल्स सर्वर को भेजता है: इमेज क्वालिटी, व्यूपोर्ट साइज, विंडो और ICC प्रोफ़ाइल।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="STOW-RS के साथ इंस्टेंस अपलोड करें">}}

<p>STOW-RS एक ही अनुरोध में किसी भी संख्या में इंस्टेंस भेजता है। सर्वर एक रिपोर्ट के साथ उत्तर देता है जो प्रत्येक इंस्टेंस के अनुसार बताती है कि क्या संग्रहीत किया गया और क्या अस्वीकृत किया गया।</p>

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

<p>कुछ सर्वर एक या अधिक इंस्टेंस विफल होने पर HTTP त्रुटि जैसे 409 के साथ जवाब देते हैं। तब कॉल <code>DicomWebClientException</code> फेंकता है, जैसा कि अगला अनुभाग दिखाता है, और वही रिपोर्ट अपवाद के प्रतिक्रिया बॉडी में होती है।</p>

<p><code>StoreStudyAsync</code> एक नामित स्टडी में अपलोड करता है। दोनों मेथड्स <code>IAsyncEnumerable&lt;DicomFile&gt;</code> को भी स्वीकार करते हैं, जिससे फ़ाइलें अनुरोध भेजे जाने के दौरान उत्पन्न की जा सकती हैं.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="सर्वर त्रुटियों को संभालें">}}

<p>प्रत्येक टाइप्ड ऑपरेशन रिटर्न होने से पहले HTTP स्टेटस जांचता है। जब सर्वर अनुरोध को अस्वीकार करता है, तो कॉल <code>DicomWebClientException</code> फेंकता है, जिसमें स्टेटस कोड, हेडर्स और लॉग के लिए प्रतिक्रिया बॉडी की शुरुआत शामिल होती है।</p>

<div class="codeblock" id="code">
 <h3>फ़ेल्ड अनुरोध की रिपोर्ट करें - C#</h3>
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

<p>वे मेथड्स जो कच्ची प्रतिक्रिया लौटाते हैं, जैसे ऊपर का थंबनेल, स्थिति जांच को आपकी जिम्मेदारी बनाते हैं <code>IsSuccessStatusCode</code> या <code>EnsureSuccessStatusCode</code> के माध्यम से।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="क्लाइंट और क्या-क्या कवर करता है">}}

<p>खोज, डाउनलोड और अपलोड सामान्य कॉल हैं। क्लाइंट के पास अन्य सभी DICOMweb सेवाओं के लिए भी एक ऑपरेशन ग्रुप है:</p>

<ul>
<li>इंस्टेंस का बल्क डेटा, पिक्सेल डेटा और चयनित फ्रेम, एक‑आधारित फ्रेम नंबर द्वारा, जैसा कि सर्वर भेजता है।</li>
<li>WADO-URI, पुराना सिंगल‑URL रिट्रीवल जो कई व्यूअर और आर्काइव अभी भी उपयोग करते हैं, DICOM फ़ाइलों और रेंडर्ड इमेजेज़ के लिए।</li>
<li>UPS-RS वर्कलिस्ट: वर्कआइटम बनाना, खोजना, अपडेट करना और क्लेम करना, उनकी स्थिति बदलना, रद्द करने का अनुरोध, और WebSocket के माध्यम से आने वाले इवेंट्स की सदस्यता लेना।</li>
<li>मॉडॅलिटी वर्कलिस्ट (MWL-RS), मॉडॅलिटी परफ़ॉर्म्ड प्रोसीज़र स्टेप्स (MPPS-RS) और स्टोरेज कमिटमेंट।</li>
<li>रोगी‑असंबंधित इंस्टेंस जैसे कलर पैलट्स और हैंगिंग प्रोटोकॉल, तथा सर्वर का क्षमताओं दस्तावेज़।</li>
<li>जब कोई टाइप्ड मेथड उपयुक्त न हो, तो अपनी रूट वैल्यूज़, क्वेरी पैरामीटर्स, हेडर्स और बॉडी के साथ किसी भी मानक ट्रांजैक्शन के लिए <code>SendAsync</code>।</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb क्लाइंट गाइड</a> प्रत्येक ऑपरेशन और विकल्प को दस्तावेज़ करता है, और <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">क्लाइंट ऑपरेशन रेफ़रेंस</a> सभी मेथड्स की सूची देता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTTP प्रतिक्रिया से पिक्सेल डेटा तक एक ही लाइब्रेरी">}}

<p>क्लाइंट से डाउनलोड की गई फ़ाइल एक सामान्य <code>DicomFile</code> है। इसे उसी प्रक्रिया में अनॉनिमाइज़, ट्रांसकोड, रेंडर या सीरियलाइज़ किया जा सकता है, बिना किसी रूपांतरण चरण के और चैन में दूसरी लाइब्रेरी की आवश्यकता नहीं।</p>

<p>संबंधित पृष्ठ: <a href="/medical/net/dicomweb-server/">DICOMweb सर्वर</a> आपके स्वयं के ASP.NET Core सेवा में प्रोटोकॉल की दूसरी तरफ, <a href="/medical/net/dicom-networking/">DICOM नेटवर्किंग</a> उन आर्काइव्स के लिए जो HTTP के बजाय DIMSE का उपयोग करते हैं, <a href="/medical/net/anonymization/">अनॉनिमाइज़ेशन</a> जब एक स्टडी अस्पताल से बाहर जाती है, <a href="/medical/net/dicom-transfer-syntax-conversion/">ट्रांसफ़र सिंटैक्स रूपांतरण</a> वह चीज़ जो सर्वर संग्रहीत करता है, और <a href="/medical/net/dicom-to-json/">DICOM से JSON</a> वह फ़ॉर्मेट जिस पर QIDO-RS खोज परिणाम उपयोग करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="लर्निंग रिसोर्सेज़" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="डॉक्यूमेंटेशन" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="API रेफ़रेंसेज़" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="प्रोडक्ट सपोर्ट" tabId="support" >}}
{{< blocks/products/pf/slr-element name="फ़्री सपोर्ट" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="पेड सपोर्ट" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सक्सेस स्टोरीज़" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
