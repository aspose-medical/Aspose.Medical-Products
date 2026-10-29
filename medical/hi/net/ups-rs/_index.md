---
title: UPS-RS in C# .NET - DICOMweb Unified Procedure Step Workitems | Aspose.Medical
weight: 9400

description: C# से DICOM UPS वर्कआइटम्स बनाएं, उनका दावा करें और उन्हें पूर्ण करें, WebSocket के माध्यम से स्थिति परिवर्तन प्राप्त करें, या Aspose.Medical के साथ ASP.NET Core पर अपना स्वयं का UPS-RS सर्वर चलाएँ।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="UPS-RS for .NET C#" h2="DICOM Unified Procedure Step के साथ सिस्टमों के बीच इमेजिंग कार्य वितरित करें: वर्कआइटम्स बनाएं, कार्यकर्ता उन्हें दावा करें और पूर्ण करें, और हर बदलाव को ट्रैक करें, क्लाइंट के रूप में या सर्वर के रूप में।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="एक कार्य कतार जो DICOM से बात करती है">}}

<p>एक AI मॉडल को हर नई छाती CT को प्रोसेस करना होता है। एक टेली‑रेडियोलॉजी सेवा अध्ययन को रीडरों को असाइन करती है। एक 3D लैब पोस्ट‑प्रोसेसिंग आदेश प्राप्त करती है। इन सभी में सिस्टमों के बीच कार्यों की एक कतार होती है, और DICOM इसके लिए एक परिभाषित करता है: Unified Procedure Step (UPS), जो DICOMweb के UPS‑RS सर्विस द्वारा HTTP पर प्रदान किया जाता है।</p>

<p>एक UPS वर्कआइटम बताता है कि क्या करना है, किस रोगी के लिए और किस इनपुट के साथ। यह बनते समय SCHEDULED होता है। एक कार्यकर्ता इसे दावा करता है और इसे IN PROGRESS बनाता है, फिर इसे COMPLETED के रूप में समाप्त करता है, या यह CANCELED के रूप में समाप्त हो सकता है। अन्य सिस्टम सब्सक्राइब करते हैं और WebSocket के माध्यम से हर स्थिति परिवर्तन प्राप्त करते हैं, ताकि किसी को भी पोल करने की जरूरत न पड़े।</p>

<p><strong>Aspose.Medical for .NET</strong> प्रबंधित C# में दोनों पक्षों को लागू करता है: किसी भी UPS‑RS सर्वर के लिए टाइप्ड क्लाइंट, और ASP.NET Core के लिए सर्वर साइड, जहाँ लाइब्रेरी UPS नियमों की जाँच करती है और आपका कोड वर्कआइटम्स को संग्रहीत करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="वर्कआइटम शेड्यूल करें">}}

<p>जो सिस्टम कार्य पूर्ण करना चाहता है, वह वर्कआइटम बनाता है। इसका SOP Instance UID इसे उसके बाद पहचानता है; लेबल, वर्कलिस्ट लेबल और प्राथमिकता कार्यकर्ताओं को बताती है कि यह क्या है।</p>

<div class="codeblock" id="code">
 <h3>UPS वर्कआइटम बनाएँ - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// What to do, for which patient, and with which input
Dataset input = new();
input.AddOrUpdate(Tag.StudyInstanceUID, "1.2.826.0.1.3680043.10.1.100");

Dataset workitem = new();
workitem.AddOrUpdate(Tag.SOPInstanceUID, Uid.CreateDerivedFromUuid());
workitem.AddOrUpdate(Tag.ProcedureStepState, "SCHEDULED");
workitem.AddOrUpdate(Tag.TransactionUID, string.Empty);
workitem.AddOrUpdate(Tag.InputReadinessState, "READY");
workitem.AddOrUpdate(Tag.ProcedureStepLabel, "Lung nodule detection");
workitem.AddOrUpdate(Tag.WorklistLabel, "AI");
workitem.AddOrUpdate(Tag.ScheduledProcedureStepPriority, "HIGH");
workitem.AddOrUpdate(Tag.PatientName, "DOE^JANE");
workitem.AddOrUpdate(Tag.PatientID, "P001");
workitem.AddOrUpdate(new Sequence(Tag.InputInformationSequence, [input]));

DicomWebClientResult&lt;CreateWorkitemResult&gt; created = await client.Worklist.CreateAsync(workitem);
Console.WriteLine($"Scheduled {created.Value.WorkitemUid}");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="इसका दावा करें, कार्य करें, इसे पूर्ण करें">}}

<p>एक कार्यकर्ता वर्कलिस्ट में उन चीज़ों की खोज करता है जो वह कर सकता है और एक ट्रांजेक्शन UID द्वारा उत्पन्न करके वर्कआइटम को IN PROGRESS में ले जाता है। उस क्षण से केवल इस UID धारक ही वर्कआइटम को अपडेट या समाप्त कर सकता है, इसलिए दो कार्यकर्ता कभी भी एक ही कार्य को प्रोसेस नहीं करते।</p>

<div class="codeblock" id="code">
 <h3>निर्धारित वर्कआइटम्स को खोजें, दावा करें और पूर्ण करें - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// Scheduled workitems on the AI worklist
DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters =
    [
        new DicomWebQueryParameter("ProcedureStepState", "SCHEDULED"),
        new DicomWebQueryParameter("WorklistLabel", "AI")
    ]
};

List&lt;string&gt; scheduled = [];
await using (DicomWebClientStream&lt;Dataset&gt; found = await client.Worklist.SearchAsync(search))
{
    await foreach (Dataset item in found)
        scheduled.Add(item.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID));
}

foreach (string workitemUid in scheduled)
{
    // Claim the workitem; only the holder of this transaction UID can update or finish it
    string transactionUid = Uid.CreateDerivedFromUuid().Code;
    await client.Worklist.ChangeStateAsync(workitemUid, WorkitemState.InProgress, transactionUid);

    // ... run the job ...

    await client.Worklist.UpdateAsync(workitemUid, transactionUid, Performed());
    await client.Worklist.ChangeStateAsync(workitemUid, WorkitemState.Completed, transactionUid);
}</code></pre>
</div>

<p>COMPLETED के लिए कार्य का एक रिकॉर्ड आवश्यक है: किसने किया, क्या किया गया, प्रारंभ और समाप्ति समय, और आउटपुट। कार्यकर्ता इसे स्थिति बदलने से पहले एक अपडेट के साथ संग्रहीत करता है।</p>

<div class="codeblock" id="code">
 <h3>कराए गए प्रक्रिया डेटा - C#</h3>
 <pre><code class="cs">// What COMPLETED requires: who did the work, what was done, when, and the output (here none)
public static Dataset Performed()
{
    Dataset performed = new();
    performed.AddOrUpdate(new Sequence(Tag.PerformedStationNameCodeSequence, [Code("AI-NODE-1", "99LOCAL", "AI node 1")]));
    performed.AddOrUpdate(new Sequence(Tag.PerformedWorkitemCodeSequence, [Code("NODULE", "99LOCAL", "Lung nodule detection")]));
    performed.AddOrUpdate(Tag.PerformedProcedureStepStartDateTime, DateTimeOffset.Now.AddMinutes(-2).ToString("yyyyMMddHHmmss"));
    performed.AddOrUpdate(Tag.PerformedProcedureStepEndDateTime, DateTimeOffset.Now.ToString("yyyyMMddHHmmss"));
    performed.AddOrUpdate(new Sequence(Tag.OutputInformationSequence, []));

    Dataset update = new();
    update.AddOrUpdate(new Sequence(Tag.UnifiedProcedureStepPerformedProcedureSequence, [performed]));
    return update;
}

private static Dataset Code(string value, string scheme, string meaning)
{
    Dataset code = new();
    code.AddOrUpdate(Tag.CodeValue, value);
    code.AddOrUpdate(Tag.CodingSchemeDesignator, scheme);
    code.AddOrUpdate(Tag.CodeMeaning, meaning);
    return code;
}</code></pre>
</div>

<p>Aspose.Medical से बना एक सर्वर इन नियमों को लागू करता है। एक दूसरा कार्यकर्ता जो प्रगति में एक वर्कआइटम का दावा करने की कोशिश करता है, उसे HTTP 409 मिलता है, और एक पूर्ण किए गए वर्कआइटम की स्थिति परिवर्तन पर भी वही मिलता है। बिना कराए गए प्रक्रिया डेटा के COMPLETED अनुरोध को HTTP 400 मिलता है। क्लाइंट ऐसे उत्तरों को <code>DicomWebClientException</code> के रूप में HTTP प्रतिक्रिया के साथ रिपोर्ट करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="WebSocket के माध्यम से वर्कलिस्ट को ट्रैक करें">}}

<p>एक डैशबोर्ड, कार्य को ऑर्डर करने वाला सिस्टम या नया कार्य प्रतीक्षा करने वाला कार्यकर्ता अपने AE टाइटल के साथ सब्सक्राइब करता है और एक नोटिफिकेशन कनेक्शन खोलता है। प्रत्येक इवेंट रिपोर्ट एक डेटासेट के रूप में आती है जिसमें वर्कआइटम UID और उसकी नई स्थिति होती है।</p>

<div class="codeblock" id="code">
 <h3>UPS इवेंट रिपोर्ट प्राप्त करें - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://workflow.example.org/dicomweb/")
});

// Every change on the worklist, pushed over a WebSocket
await client.Worklist.SubscribeWorklistAsync("DASHBOARD");
await using DicomWebNotificationConnection connection = await client.ConnectNotificationsAsync("DASHBOARD");

while (await connection.ReceiveAsync(cancellationToken) is Dataset report)
{
    Console.WriteLine($"{report.GetSingleValueOrDefault(Tag.AffectedSOPInstanceUID, string.Empty)}: " +
                      $"{report.GetSingleValueOrDefault(Tag.ProcedureStepState, string.Empty)}");
}</code></pre>
</div>

<p>पूरी वर्कलिस्ट के अलावा, क्लाइंट एक वर्कआइटम को <code>SubscribeWorkitemAsync</code> के साथ सब्सक्राइब कर सकता है या वर्कलिस्ट के फ़िल्टर किए गए हिस्से को <code>SubscribeFilteredWorklistAsync</code> के साथ, यदि सर्वर फ़िल्टर समर्थन करता है। एक रिसीव को रद्द करने से कनेक्शन बंद हो जाता है; जारी रखने के लिए <code>ConnectNotificationsAsync</code> के साथ नया खोलें।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="आपका अपना UPS-RS सर्वर">}}

<p><code>Aspose.Medical.Dicom.Web.AspNetCore</code> पैकेज UPS‑RS को एक ASP.NET Core सेवा में जोड़ता है। लाइब्रेरी हर अनुरोध को संभालती है: यह एक नए वर्कआइटम के पेलोड, स्थिति परिवर्तन, कार्यकर्ता के ट्रांज़ैक्शन UID और COMPLETED द्वारा आवश्यक डेटा की जाँच करती है, और खोजों पर DICOM मिलान नियम लागू करती है। आपका कोड वर्कआइटम्स को संग्रहीत करता है।</p>

<div class="codeblock" id="code">
 <h3>UPS‑RS सर्वर का Program.cs - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Workitems in your storage; UPS-RS comes from the library
builder.Services.AddSingleton&lt;IDicomWebWorkitemProvider, WorkitemStore&gt;();
builder.Services.AddDicomWeb().AddStandardWorklistService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>वर्कआइटम संग्रहण - C#</h3>
 <pre><code class="cs">// Workitems with a revision each; the library applies the UPS state rules and the search matching
public sealed class WorkitemStore : IDicomWebWorkitemProvider
{
    private readonly ConcurrentDictionary&lt;string, DicomWebVersionedDataset&gt; _items = new();

    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string uid in _items.Keys.Order(StringComparer.Ordinal))
            if (_items.TryGetValue(uid, out DicomWebVersionedDataset? item))
                yield return new Dataset(item.Dataset);
        await Task.CompletedTask;
    }

    public ValueTask&lt;DicomWebVersionedDataset?&gt; GetAsync(string workitemUid, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryGetValue(workitemUid, out DicomWebVersionedDataset? item) ? Copy(item) : null);

    public ValueTask&lt;bool&gt; TryCreateAsync(string workitemUid, Dataset workitem, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryAdd(workitemUid, NewRevision(workitem)));

    // Replaces the workitem only if nobody changed it since it was read
    public ValueTask&lt;bool&gt; TryReplaceAsync(string workitemUid, string expectedRevision, Dataset workitem,
        CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_items.TryGetValue(workitemUid, out DicomWebVersionedDataset? current)
                             &amp;&amp; current.Revision == expectedRevision
                             &amp;&amp; _items.TryUpdate(workitemUid, NewRevision(workitem), current));

    private static DicomWebVersionedDataset NewRevision(Dataset dataset) =&gt;
        new() { Dataset = new Dataset(dataset), Revision = Guid.NewGuid().ToString("N") };

    private static DicomWebVersionedDataset Copy(DicomWebVersionedDataset item) =&gt;
        new() { Dataset = new Dataset(item.Dataset), Revision = item.Revision };
}</code></pre>
</div>

<p>हर संग्रहित वर्कआइटम की एक रिवीजन होती है। <code>TryReplaceAsync</code> इसे तभी बदलता है जब रिवीजन अभी भी वही हो जिसे लाइब्रेरी ने पढ़ा है, इसलिए एक ही क्षण में दो अनुरोध जो एक ही वर्कआइटम को बदलते हैं, एक दूसरे को ओवरराइट नहीं कर सकते; लाइब्रेरी पुनः प्रयास करती है या संघर्ष की रिपोर्ट करती है। शब्दकोश को अपने डेटाबेस से बदलें और इस जांच को रखें।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="सर्वर पर सूचनाएँ">}}

<p>इवेंट रिपोर्ट भेजने के लिए, एक <code>IDicomWebSubscriptionProvider</code> पंजीकृत करें और <code>MapDicomWeb</code> से पहले <code>app.UseWebSockets()</code> को कॉल करें। प्रोवाइडर सब्सक्रिप्शन और प्रत्येक सब्सक्राइबर के लिए इवेंट रिपोर्ट की एक कतार रखता है, और लाइब्रेरी जब भी कोई वर्कआइटम बदलता है, उसे एक इवेंट रिपोर्ट देती है। प्रोवाइडर यह भी लाइब्रेरी को बताता है कि प्रमाणित कॉलर का कौन सा AE टाइटल है, क्योंकि इवेंट रिपोर्ट उस सिस्टम का नाम लेती है जिसने स्थिति बदली।</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">वर्कफ़्लो प्रोवाइडर्स गाइड</a> में एक पूर्ण इन‑मेमोरी सब्सक्रिप्शन प्रोवाइडर शामिल है, और <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core गाइड</a> WebSocket सेटिंग्स को कवर करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="UPS, MWL और MPPS">}}

<p>UPS उन कार्यों को कवर करता है जो पुराना Modality Worklist और MPPS एक RIS और मोडालिटी के बीच करते हैं, और यह मोडालिटीज़ नहीं होने वाले कार्यकर्ताओं को दावा और सूचनाओं के साथ जोड़ता है। कई मोडालिटीज़ अभी भी केवल DIMSE पर MWL और MPPS की भाषा बोलती हैं; <a href="/medical/net/modality-worklist/">modality worklist पेज</a> समान लाइब्रेरी के साथ उस वर्कफ़्लो को दर्शाता है।</p>

<p>संबंधित पृष्ठ: <a href="/medical/net/dicomweb-client/">DICOMweb क्लाइंट</a> अन्य DICOMweb सेवाओं के लिए, <a href="/medical/net/dicomweb-server/">DICOMweb सर्वर</a> QIDO‑RS, WADO‑RS और STOW‑RS के लिए उसी सर्वर पर, और <a href="/medical/net/dicom-networking/">DICOM नेटवर्किंग</a> DIMSE के लिए।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="सीखने के संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/" >}}
{{< blocks/products/pf/slr-element name="API संदर्भ" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="नि:शुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="सशुल्क समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहक सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता की कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
