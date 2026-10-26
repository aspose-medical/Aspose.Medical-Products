---
title: DICOMweb क्लाइंट C# .NET में - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: HTTP के माध्यम से .NET से DICOM स्टडीज को खोजें, डाउनलोड करें और अपलोड करें। QIDO-RS, WADO-RS और STOW-RS के लिए शुद्ध C# में DICOMweb क्लाइंट, जो PACS, VNA और क्लाउड इमेजिंग सेवाओं के साथ कार्य करता है।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# के लिए DICOMweb क्लाइंट" h2="किसी भी DICOMweb सर्वर पर QIDO-RS, WADO-RS और STOW-RS के साथ स्टडीज को खोजें, डाउनलोड करें और अपलोड करें, प्रबंधित C# में, बिना मशीन पर कोई इंस्टॉल किए।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="आपके अपने कोड से HTTP के ऊपर DICOM">}}

<p>DICOMweb, DICOM का HTTP रूप है। पोर्ट 104 पर DIMSE एसोसिएशन के बजाय, आपका एप्लिकेशन साधारण HTTPS अनुरोध भेजता है: खोज के लिए QIDO-RS, डाउनलोड के लिए WADO-RS, अपलोड के लिए STOW-RS। Azure Health Data Services DICOM सेवा और Google Cloud Healthcare API जैसी क्लाउड इमेजिंग सेवाएँ इसी तरह काम करती हैं, और Orthanc और dcm4chee जैसे आर्काइव भी।</p>

<p><strong>Aspose.Medical for .NET</strong> संस्करण 26.10 से एक DICOMweb क्लाइंट शामिल करता है। <code>DicomWebClient</code> प्रत्येक मानक लेनदेन को एक टाइप्ड मेथड में रैप करता है, और परिणाम उसी <code>Dataset</code> और <code>DicomFile</code> ऑब्जेक्ट्स के रूप में मिलते हैं जो लाइब्रेरी के अन्य भाग पढ़ते और लिखते हैं। दूसरी तरफ का सर्वर कोई भी DICOMweb इम्प्लीमेंटेशन हो सकता है; इसे Aspose.Medical के साथ बनाया जाना आवश्यक नहीं है।</p>

<p>क्लाइंट <code>HttpClient</code> के ऊपर प्रबंधित C# है। कोई मूल टूलकिट या प्रॉक्सी सेवा नहीं है, इसलिए वही कोड Windows, Linux और कंटेनर में चलता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOMweb सर्वर से कनेक्ट करें">}}

<p>क्लाइंट को <code>HttpClient</code> और सर्वर की सर्विस रूट URL से बनाएं। ऑथेंटिकेशन, TLS सेटिंग्स, प्रॉक्सी और कनेक्शन पूलिंग <code>HttpClient</code> पर रहती हैं, जहाँ आपका एप्लिकेशन पहले से इन्हें कॉन्फ़िगर करता है। क्लाइंट <code>HttpClient</code> का मालिक नहीं है और न ही उसे डिस्पोज़ करता है।</p>

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

<p>यदि कोई तैनाती WADO-URI या किसी अन्य सेवा को अलग URL से सर्व करती है, तो <code>ServiceBaseUris</code> केवल उस सेवा के लिए रूट को ओवरराइड करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="QIDO-RS के साथ स्टडीज खोजें">}}

<p>एक खोज में DICOM कीवर्ड को नाम के रूप में, मिलान पैरामीटर, लौटाने वाले फ़ील्ड और पेजिंग नियंत्रण शामिल होते हैं। मिलान परिणाम डेटा सेट की स्ट्रीम के रूप में आते हैं, जिसे प्रतिक्रिया अभी डाउनलोड हो रही हो, एक‑एक करके पढ़ा जाता है।</p>

<div class="codeblock" id="code">
 <h3>किसी रोगी की स्टडीज खोजें - C#</h3>
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

<p><code>SearchSeriesAsync</code> और <code>SearchInstancesAsync</code> एक स्तर नीचे खोज करते हैं, पूरे आर्काइव में या किसी स्टडी या सीरीज़ के भीतर। वही विकल्प फज़ी मैचिंग, खाली मान मैचिंग और पेजिंग के लिए रिज़ल्ट ऑफ़सेट भी सेट करते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="WADO-RS के साथ स्टडी डाउनलोड करें">}}

<p>एक स्टडी में हजारों इंस्टेंसेस हो सकते हैं। <code>RetrieveStudyAsync</code> उन्हें स्ट्रीम के रूप में लौटाता है, इसलिए प्रत्येक फ़ाइल को प्रतिक्रिया के उस भाग के आने पर पार्स किया जाता है और अगली पढ़े जाने से पहले सहेजा जा सकता है। मेमोरी उपयोग स्टडी के आकार के साथ नहीं बढ़ता।</p>

<div class="codeblock" id="code">
 <h3>स्टडी के सभी इंस्टेंसेस सहेजें - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> और <code>RetrieveInstanceAsync</code> एक ही तरीके से एक श्रृंखला या एक इंस्टेंस को डाउनलोड करते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="थंबनेल या रेंडर किया गया इमेज प्राप्त करें">}}

<p>एक व्यूअर या वर्कलिस्ट स्क्रीन को अक्सर चित्र चाहिए होता है, न कि DICOM फ़ाइल। सर्वर इसे रेंडर करके JPEG या PNG भेजता है, इसलिए क्लाइंट मूल पिक्सेल डेटा को डाउनलोड या डिकोड नहीं करता।</p>

<div class="codeblock" id="code">
 <h3>एक इंस्टेंस का JPEG थंबनेल सहेजें - C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> और <code>RetrieveRenderedFramesAsync</code> इसके बजाय पूर्ण आकार की इमेज लौटाते हैं। <code>DicomWebRenderOptions</code> मानक रेंडरिंग कंट्रोल्स सर्वर को पास करता है: इमेज क्वालिटी, व्यूपोर्ट साइज़, विंडो और ICC प्रोफ़ाइल।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="STOW-RS के साथ इंस्टेंसेस अपलोड करें">}}

<p>STOW-RS एक अनुरोध में किसी भी संख्या में इंस्टेंसेस भेजता है। सर्वर एक रिपोर्ट के साथ उत्तर देता है जो इंस्टेंस दर इंस्टेंस बताता है कि उसने क्या संग्रहित किया और क्या अस्वीकार किया।</p>

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

<p>कुछ सर्वर एक HTTP त्रुटि, जैसे 409, वापस करते हैं जब एक या अधिक इंस्टेंसेस विफल हो जाते हैं। तब कॉल <code>DicomWebClientException</code> फेंकता है, जैसा कि अगले सेक्शन में दिखाया गया है, और वही रिपोर्ट अपवाद के रिस्पॉन्स बॉडी में होती है.</p>

<p><code>StoreStudyAsync</code> एक नामित स्टडी में अपलोड करता है। दोनों मेथड्स एक <code>IAsyncEnumerable&lt;DicomFile&gt;</code> भी स्वीकार करते हैं, ताकि फ़ाइलें अनुरोध भेजे जाने के दौरान निर्मित की जा सकें.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="सर्वर त्रुटियों को संभालें">}}

<p>प्रत्येक टाइप्ड ऑपरेशन रिटर्न करने से पहले HTTP स्थिति को जाँचता है। जब सर्वर अनुरोध को अस्वीकार करता है, तो कॉल <code>DicomWebClientException</code> फेंकता है, जिसमें स्थिति कोड, हेडर्स और लॉग के लिए रिस्पॉन्स बॉडी की शुरुआत शामिल होती है.</p>

<div class="codeblock" id="code">
 <h3>व्यर्थ अनुरोध की रिपोर्ट करें - C#</h3>
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

<p>ऐसे मेथड्स जो कच्चा रिस्पॉन्स लौटाते हैं, जैसे ऊपर का थंबनेल, स्थिति जाँच आपको <code>IsSuccessStatusCode</code> या <code>EnsureSuccessStatusCode</code> के माध्यम से छोड़ते हैं.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="क्लाइंट और क्या कवर करता है">}}

<p>खोज, डाउनलोड और अपलोड सामान्य कॉल्स हैं। क्लाइंट में अन्य DICOMweb सेवाओं के लिए भी प्रत्येक के लिए एक ऑपरेशन ग्रुप है:</p>

<ul>
<li>एक इंस्टेंस के बल्क डेटा, पिक्सेल डेटा और चयनित फ्रेम, एक‑आधारित फ्रेम नंबर के अनुसार, जैसा सर्वर ने भेजा है।</li>
<li>WADO-URI, पुराना सिंगल‑URL पुनर्प्राप्ति जो कई व्यूअर्स और आर्काइव अभी भी उपयोग करते हैं, DICOM फ़ाइलों और रेंडर की गई इमेजेज़ के लिए।</li>
<li>UPS-RS वर्कलिस्ट: वर्कआइटम बनाना, खोजना, अपडेट करना और क्लेम करना, उनकी स्थिति बदलना, रद्दीकरण का अनुरोध करना, और WebSocket पर आने वाले इवेंट्स की सदस्यता लेना।</li>
<li>मॉडैलिटी वर्कलिस्ट (MWL-RS), मॉडैलिटी पर फ़ॉर्म किए गए प्रोसीजर स्टेप्स (MPPS-RS) और स्टोरेज कमिटमेंट।</li>
<li>रोगी‑बाह्य इंस्टेंसेस जैसे कलर पैलेट और हैंगिंग प्रोटोकॉल, तथा सर्वर का क्षमताओं दस्तावेज़।</li>
<li>जब कोई टाइप्ड मेथड उपयुक्त न हो, तो <code>SendAsync</code> का उपयोग करके अपने रूट वैल्यूज़, क्वेरी पैरामीटर्स, हेडर्स और बॉडी के साथ किसी भी मानक ट्रांज़ैक्शन को किया जा सकता है।</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb क्लाइंट गाइड</a> हर ऑपरेशन और विकल्प को दस्तावेज़ित करता है, और <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">क्लाइंट ऑपरेशन रेफरेंस</a> सभी मेथड्स की सूची देता है.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HTTP रिस्पॉन्स से पिक्सेल डेटा तक एक ही लाइब्रेरी">}}

<p>क्लाइंट से डाउनलोड किया गया फ़ाइल एक सामान्य <code>DicomFile</code> है। इसे उसी प्रोसेस में अनामित, ट्रांसकोड, रेंडर या सीरियलाइज़ किया जा सकता है, बिना किसी रूपांतरण चरण के और श्रृंखला में दूसरे लाइब्रेरी की आवश्यकता नहीं।</p>

<p>संबंधित पृष्ठ: <a href="/medical/net/dicom-networking/">DICOM नेटवर्किंग</a> उन आर्काइव्स के लिए जो HTTP के बजाय DIMSE का उपयोग करते हैं, <a href="/medical/net/anonymization/">अनामिकरण</a> जब एक स्टडी अस्पताल से बाहर जाती है, <a href="/medical/net/dicom-transfer-syntax-conversion/">ट्रांसफ़र सिंटैक्स कन्वर्ज़न</a> सर्वर द्वारा संग्रहीत डेटा के लिए, और <a href="/medical/net/dicom-to-json/">DICOM से JSON</a> QIDO-RS खोज परिणामों के फ़ॉर्मेट के लिए.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="API रेफ़रेंसेज़" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="नि:शुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="भुगतानिक समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical .NET के लिए?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहक सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
