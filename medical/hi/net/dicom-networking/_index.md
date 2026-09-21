---
title: C# .NET में DICOM नेटवर्किंग - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: .NET एप्लिकेशन को PACS से कनेक्ट करें। C-ECHO से सत्यापित करें, C-STORE से इमेज भेजें, C-FIND से क्वेरी करें, और अपना स्वयं का SCP उपयोग करके इमेज प्राप्त करें। शुद्ध C# में एक DIMSE क्लाइंट और सर्वर।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में DICOM नेटवर्किंग" h2="अपने एप्लिकेशन से PACS से बात करें: C-ECHO, C-STORE, C-FIND, C-MOVE और C-GET, क्लाइंट तथा सर्वर दोनों रूप में, प्रबंधित C# में और मशीन पर कुछ भी स्थापित करने की आवश्यकता नहीं।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="अपने कोड से PACS से कनेक्ट करें">}}

<p>DICOM फ़ाइलें पढ़ना मेडिकल इमेजिंग का आसान आधा है। जैसे ही आपका एप्लिकेशन वास्तविक अस्पताल प्रणाली के साथ काम करता है, उसे DIMSE बोलनी पड़ती है: PACS के साथ एक एसोसिएशन खोलें, इमेज भेजें, पूछें कि कौन‑से स्टडीज़ उपलब्ध हैं, और जब कोई अन्य सिस्टम कुछ वापस भेजे तो उत्तर दें।</p>

<p><strong>Aspose.Medical for .NET</strong> इस प्रोटोकॉल को लाइब्रेरी के हिस्से के रूप में प्रदान करता है। <code>Aspose.Medical.Dicom.Network</code> आपको एक DIMSE क्लाइंट और एक DIMSE सर्वर देता है, दोनों प्रबंधित C# में लिखे गए हैं। स्थापित करने के लिए कोई नेटीव टूलकिट नहीं, कॉन्फ़िगर करने के लिए कोई सेवा नहीं और कोई प्लेटफ़ॉर्म‑विशिष्ट चीज़ नहीं, इसलिए वही कोड Windows, Linux और कंटेनर में चलता है।</p>

<p>बहुत सी इंटीग्रेशन्स के लिए केवल तीन चीज़ें पर्याप्त हैं, और प्रत्येक कुछ ही पंक्तियों में होती है: लिंक की जाँच C-ECHO से करें, इमेज C-STORE से भेजें, और दूसरी ओर क्या है यह C-FIND से पता लगाएँ।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-ECHO से शुरू करें">}}

<p>C-ECHO DICOM पिंग है। यह साबित करता है कि होस्ट, पोर्ट और दो AE टाइटल सही हैं, इससे पहले कि अन्य कोई त्रुटि न आए। एक बार क्लाइंट बनाएं, उसे एक हैंडलर दें जो उत्तर को देखे, और अनुरोध भेजें।</p>

<div class="codeblock" id="code">
 <h3>C-ECHO से कनेक्शन सत्यापित करें - C#</h3>
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

<p>स्थिति <code>0x0000</code> सफलता दर्शाती है। अनुरोधों को कतारबद्ध किया जाता है और फिर एक ही एसोसिएशन पर भेजा जाता है, इसलिए कार्य के बैच को प्रत्येक आइटम के लिए अलग कनेक्शन खोलना नहीं पड़ता।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-STORE से इमेज भेजें">}}

<p>C-STORE वह कार्य है जो एप्लिकेशन इमेज बनाने या प्राप्त करने के बाद करता है: यह इंस्टेंस को आर्काइव में पुश करता है। प्रति इंस्टेंस एक अनुरोध कतारबद्ध करें और उन्हें साथ में भेजें।</p>

<div class="codeblock" id="code">
 <h3>DICOM फ़ाइल को PACS पर भेजें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>आपके द्वारा प्रस्तावित प्रस्तुति कॉन्टेक्स्ट तय करते हैं कि आर्काइव क्या स्वीकार करेगा। यदि वह संकुचित सिंटैक्स चाहता है, तो भेजने से पहले ट्रांसकोड करें, जैसा कि <a href="/medical/net/dicom-transfer-syntax-conversion/">ट्रांसफ़र सिंटैक्स कन्वर्ज़न</a> पेज दिखाता है, या <code>AdditionalTransferSyntaxes</code> में विकल्प सूचीबद्ध करें और नेगोसिएशन को एक चुनने दें।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-FIND से स्टडीज़ खोजें">}}

<p>C-FIND इस प्रश्न का उत्तर देता है "आर्काइव में क्या है"। मेल एक‑एक करके आते हैं, प्रत्येक के पास अपना पहचान डेटा सेट होता है, और अंतिम प्रतिक्रिया क्वेरी को बंद कर देती है।</p>

<div class="codeblock" id="code">
 <h3>रोगी द्वारा स्टडीज़ क्वेरी करें - C#</h3>
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

<p>वही फ़ैक्टरी अन्य क्वेरी स्तर बनाती है: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> और <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> एक मोडैलिटी वर्कलिस्ट क्वेरी बनाता है, जो स्कैन से पहले मोडैलिटी द्वारा पूछी जाती है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="इमेज प्राप्त करें: अपना स्वयं का स्टोर SCP">}}

<p>यह लाइब्रेरी एक सर्वर भी है। आप जिस सेवा को देना चाहते हैं उसके लिए एक हैंडलर रजिस्टर करें, सुनना शुरू करें, और आपका एप्लिकेशन एक DICOM नोड बन जाता है जिससे मोडैलिटी या अन्य PACS भेज सकते हैं।</p>

<div class="codeblock" id="code">
 <h3>आने वाली इमेज स्वीकार करें - C#</h3>
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
 <h3>आने वाले डेटा को स्टोर करने वाला हैंडलर - C#</h3>
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

<p>डेटासेट के साथ हैंडलर क्या करता है, यह आपका निर्णय है: डिस्क पर लिखें, कतार में रखें, पहले <a href="/medical/net/anonymization/">अनामिकरण API</a> से अनाम करें, या इसे आर्काइव के सिंटैक्स में ट्रांसकोड करें।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="नेटवर्किंग API और क्या कवर करता है">}}

<p>ऊपर बताए गए तीन सर्विस आम हैं। बाकी DIMSE भी उपलब्ध है:</p>

<ul>
<li>रिट्राइवल: C-MOVE और C-GET, साथ ही सब‑ऑपरेशन काउंटर प्रतिक्रियाओं में रिपोर्ट किए जाते हैं।</li>
<li>N-सेवाएं: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE और N-EVENT-REPORT, जो स्टोरेज कमिटमेंट और MPPS के निर्माण में प्रयोग होती हैं।</li>
<li>एसोसिएशन नियंत्रण: प्रस्तुति कॉन्टेक्स्ट, सर्विस क्लास रोल्स, विस्तारित नेगोसिएशन, असिंक्रोनस ऑपरेशन्स विंडो, यूज़र आइडेंटिटी नेगोसिएशन, और एक पॉलिसी हुक जो AE टाइटल को कॉल करके एसोसिएशन को अस्वीकार कर सकता है।</li>
<li>दोनों पक्षों पर TLS, <code>TlsInitiatorAuthenticator</code> और <code>TlsAcceptorAuthenticator</code> के माध्यम से, आवश्यक होने पर स्वयं का प्रमाणपत्र वैधता भी संभव है।</li>
<li>प्रत्येक चरण के लिए टाइम‑आउट, TCP कनेक्ट से रिलीज़ तक, और एसोसिएशन लाइफ़साइकल के लिए सूचनाएं ताकि लंबी अवधि चलने वाला नोड क्या हुआ, उसे लॉग कर सके।</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">DICOM नेटवर्किंग गाइड</a> हर विकल्प और हर हैंडलर का विवरण देता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="शुद्ध .NET, सॉकेट से पिक्सेल डेटा तक">}}

<p>इस पेज की सभी चीज़ें एक ही पैकेज के प्रबंधित कोड से आती हैं जो फ़ाइलें पढ़ती और लिखती है। एसोसिएशन, कोडेक और पार्सर एक ही लाइब्रेरी से आते हैं, इसलिए नेटवर्क के माध्यम से प्राप्त स्टडी को प्रक्रिया से बाहर जाए बिना या किसी नेटीव डिपेंडेंसी के बिना अनाम किया, ट्रांसकोड किया या सीरियलाइज़ किया जा सकता है।</p>

<p>संबंधित पेज: क्या भेजना है इसके लिए <a href="/medical/net/dicom-transfer-syntax-conversion/">ट्रांसफ़र सिंटैक्स कन्वर्ज़न</a>, पहले क्या हटाना है इसके लिए <a href="/medical/net/anonymization/">अनामिकरण</a>, और क्या आया पढ़ने के लिए <a href="/medical/net/dicom-tags/">DICOM टैग्स</a>।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="API रेफ़रेंसेज़" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="नि:शुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="भुगतान‑सहायता" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
