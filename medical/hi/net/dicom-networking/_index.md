---
title: C# .NET में DICOM नेटवर्किंग - C-ECHO, C-STORE, C-FIND | Aspose.Medical
weight: 9000

description: .NET एप्लिकेशन को PACS से कनेक्ट करें। C-ECHO के साथ सत्यापित करें, C-STORE के साथ छवियां भेजें, C-FIND के साथ क्वेरी करें, और अपनी स्वयं की SCP के साथ छवियां प्राप्त करें। शुद्ध C# में एक DIMSE क्लाइंट और सर्वर।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में DICOM नेटवर्किंग" h2="अपने स्वयं के एप्लिकेशन से PACS से बात करें: C-ECHO, C-STORE, C-FIND, C-MOVE और C-GET, क्लाइंट और सर्वर दोनों रूप में, मैनेज्ड C# में बिना मशीन पर कुछ भी इंस्टॉल किए।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="अपने कोड से PACS से कनेक्ट करें">}}

<p>DICOM फ़ाइलें पढ़ना मेडिकल इमेजिंग का आसान हिस्सा है। जैसे ही आपका एप्लिकेशन वास्तविक अस्पताल सिस्टम के साथ काम करता है, उसे DIMSE बोलना पड़ता है: PACS के साथ एक एसोसिएशन खोलें, छवियां भेजें, पूछें कि कौन से अध्ययन उपलब्ध हैं, और जब कोई दूसरा सिस्टम कुछ भेजे तो उत्तर दें।</p>

<p><strong>Aspose.Medical for .NET</strong> उस प्रोटोकॉल को लाइब्रेरी के हिस्से के रूप में प्रदान करता है। <code>Aspose.Medical.Dicom.Network</code> आपको एक DIMSE क्लाइंट और एक DIMSE सर्वर देता है, दोनों मैनेज्ड C# में लिखे गए हैं। कोई नेटिव टूलकिट इंस्टॉल करने की जरूरत नहीं, कोई सेवा कॉन्फ़िगर करने की नहीं और कोई प्लेटफ़ॉर्म‑विशिष्ट चीज़ नहीं, इसलिए वही कोड Windows, Linux और कंटेनर में चलता है।</p>

<p>तीन चीज़ें अधिकांश इंटीग्रेशन को कवर करती हैं, और प्रत्येक कुछ पंक्तियों में है: C-ECHO से लिंक की जाँच करें, C-STORE से छवियां पुश करें, और C-FIND से अन्य पक्ष में क्या है, पता लगाएँ।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-ECHO से शुरू करें">}}

<p>C-ECHO DICOM पिंग है। यह साबित करता है कि होस्ट, पोर्ट और दो AE टाइटल सही हैं, इससे पहले कि कोई अन्य त्रुटि आए। एक बार क्लाइंट बनाएं, उसे ऐसा हैंडलर दें जो उत्तर को देखे, और अनुरोध भेजें।</p>

<div class="codeblock" id="code">
 <h3>C-ECHO - C# के साथ कनेक्शन सत्यापित करें</h3>
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

<p>Status <code>0x0000</code> का मतलब सफलता है। अनुरोधों को कतारबद्ध किया जाता है और फिर एक एसोसिएशन पर भेजा जाता है, इसलिए कार्य का बैच प्रत्येक आइटम के लिए अलग कनेक्शन नहीं खोलता।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-STORE से छवियां भेजें">}}

<p>C-STORE वह है जो कोई एप्लिकेशन एक छवि उत्पन्न या प्राप्त करने के बाद करता है: वह इंस्टेंस को आर्काइव में पुश करता है। प्रति इंस्टेंस एक अनुरोध कतारबद्ध करें और उन्हें एक साथ भेजें।</p>

<div class="codeblock" id="code">
 <h3>C# में DICOM फ़ाइल को PACS पर भेजें</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

client.QueueRequest(new CStoreRequest(dicomFile.Dataset, TransferSyntax.ExplicitVrLittleEndian));

await client.SendAsync(CancellationToken.None);</code></pre>
</div>

<p>आपके द्वारा प्रस्तावित प्रस्तुति कॉन्टेक्स्ट यह निर्धारित करते हैं कि आर्काइव क्या स्वीकार करेगा। यदि वह संकुचित सेंटैक्स चाहता है, तो भेजने से पहले ट्रांसकोड करें, जैसा कि <a href="/medical/net/dicom-transfer-syntax-conversion/">ट्रांसफर सेंटैक्स कन्वर्ज़न</a> पृष्ठ दिखाता है, या <code>AdditionalTransferSyntaxes</code> में विकल्प सूचीबद्ध करें और नेगोशिएशन को एक चुनने दें।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-FIND से अध्ययन खोजें">}}

<p>C-FIND यह प्रश्न का उत्तर देता है “आर्काइव में क्या है”। मिलान एक‑एक करके आते हैं, प्रत्येक के पास अपना पहचान डेटासेट होता है, और अंतिम प्रतिक्रिया क्वेरी को बंद कर देती है।</p>

<div class="codeblock" id="code">
 <h3>C# में रोगी द्वारा अध्ययन क्वेरी करें</h3>
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

<p>एक ही फ़ैक्ट्री अन्य क्वेरी स्तर बनाती है: <code>CreatePatientQuery</code>, <code>CreateSeriesQuery</code> और <code>CreateImageQuery</code>. <code>CreateWorklistQuery</code> एक मोडालिटी वर्कलिस्ट क्वेरी बनाता है, जो स्कैन से पहले मोडालिटी द्वारा पूछी जाती है; <a href="/medical/net/modality-worklist/">मोडालिटी वर्कलिस्ट पृष्ठ</a> इसे MPPS और वर्कलिस्ट सर्वर साइड के साथ दिखाता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="छवियां प्राप्त करें: आपका अपना स्टोर SCP">}}

<p>लाइब्रेरी एक सर्वर भी है। जिस सेवा को आप प्रदान करना चाहते हैं उसके लिए एक हैंडलर रजिस्टर करें, सुनना शुरू करें, और आपका एप्लिकेशन एक DICOM नोड बन जाता है जिससे मोडालिटी या किसी अन्य PACS भेज सकता है।</p>

<div class="codeblock" id="code">
 <h3>C# में आने वाली छवियों को स्वीकार करें</h3>
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
 <h3>C# में आने वाले डेटा को संग्रहित करने वाला हैंडलर</h3>
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

<p>डेटासेट के साथ हैंडलर क्या करता है, यह आपके निर्णय पर निर्भर है: इसे डिस्क में लिखें, इसे कतार में रखें, पहले <a href="/medical/net/anonymization/">अनोनीमाइज़ेशन API</a> से अनोनिमाइज़ करें, या इसे आर्काइव के सेंटैक्स में ट्रांसकोड करें।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="नेटवर्किंग API और क्या कवर करता है">}}

<p>ऊपर बताई गई तीन सेवाएँ सामान्य हैं। बाकी DIMSE भी उपलब्ध है:</p>

<ul>
<li>रीट्रीवल: C-MOVE और C-GET, उप‑ऑपरेशन काउंटर्स प्रतिक्रियाओं में रिपोर्ट होते हैं।</li>
<li>N-सेवाएँ: N-CREATE, N-SET, N-GET, N-ACTION, N-DELETE और N-EVENT-REPORT, जिनसे स्टोरेज कमिटमेंट और MPPS बनते हैं।</li>
<li>एसोसिएशन नियंत्रण: प्रस्तुति कॉन्टेक्स्ट, सर्विस क्लास रोल्स, विस्तारित नेगोशिएशन, असिंक्रोनस ऑपरेशन्स विंडो, यूज़र आइडेंटिटी नेगोशिएशन, और एक पॉलिसी हुक जो AE टाइटल कॉल करके एसोसिएशन को अस्वीकार कर सकता है।</li>
<li>दोनों पक्षों पर TLS, <code>TlsInitiatorAuthenticator</code> और <code>TlsAcceptorAuthenticator</code> के माध्यम से, यदि आवश्यकता हो तो अपने स्वयं के प्रमाणपत्र वैलिडेशन के साथ।</li>
<li>हर चरण के लिए टाइम‑आउट, TCP कनेक्ट से लेकर रिलीज़ तक, और एसोसिएशन लाइफ़साइकल के नोटिफिकेशन्स ताकि लंबे समय तक चलने वाला नोड क्या हुआ, लॉग कर सके।</li>
</ul>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/">DICOM नेटवर्किंग गाइड</a> हर विकल्प और हर हैंडलर का दस्तावेज़ है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="शुद्ध .NET, सॉकेट से पिक्सेल डेटा तक">}}

<p>इस पृष्ठ पर सभी चीज़ें एक ही पैकेज से प्रबंधित कोड हैं जो फ़ाइलें पढ़ते और लिखते हैं। एसोसिएशन, कोडेक्स और पार्सर एक ही लाइब्रेरी से आते हैं, इसलिए नेटवर्क के माध्यम से प्राप्त अध्ययन को प्रक्रिया से बाहर निकले या किसी नेटिव डिपेंडेंसी के बिना अनोनिमाइज़, ट्रांसकोड या सीरियलाइज़ किया जा सकता है।</p>

<p>संबंधित पृष्ठ: क्या भेजना है इसके लिए <a href="/medical/net/dicom-transfer-syntax-conversion/">ट्रांसफर सेंटैक्स कन्वर्ज़न</a>, पहले क्या हटाना है इसके लिए <a href="/medical/net/anonymization/">अनोनीमाइज़ेशन</a>, और क्या आया पढ़ने के लिए <a href="/medical/net/dicom-tags/">DICOM टैग्स</a>।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="API रेफ़रेंसेस" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="निःशुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="सशुल्क समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता कहानियां" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
