---
title: C# .NET में Modality Worklist और MPPS - DICOM C-FIND, N-CREATE, N-SET | Aspose.Medical
weight: 9300

description: C-FIND के साथ DICOM मोडैलिटी वर्कलिस्ट को क्वेरी करें और C# से MPPS रिपोर्ट करें, या अपना स्वयं का वर्कलिस्ट सर्वर चलाएँ। DIMSE और DICOMweb एक ही .NET लाइब्रेरी में।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# के लिए Modality Worklist और MPPS" h2="RIS से दिन की परीक्षाएँ प्राप्त करें, प्रत्येक को शुरू और पूर्ण के रूप में रिपोर्ट करें, या स्वयं DIMSE या DICOMweb के माध्यम से वर्कलिस्ट सर्वर बनें।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="RIS शेड्यूल से लेकर किए गए परीक्षण तक">}}

<p>स्कैन से पहले, टेक्नोलॉजिस्ट को रोगी का नाम, रोगी आईडी और एक्सेशन नंबर मॉडैलिटी में टाइप नहीं करना चाहिए। RIS ने पहले ही परीक्षण शेड्यूल कर दिया है, और मॉडैलिटी उससे अनुरोध करती है: Modality Worklist क्वेरी इस स्कैनर और इस दिन के लिए निर्धारित प्रोसीजर स्टेप्स लौटाती है। मॉडैलिटी रोगी और ऑर्डर डेटा को इमेज़ में कॉपी करती है, जिससे अध्ययन PACS में पहले से ही उसके ऑर्डर से मेल खा जाता है।</p>

<p>परीक्षण के चलने के दौरान, मॉडैलिटी MPPS (Modality Performed Procedure Step) के साथ इसे रिपोर्ट करती है: स्कैन शुरू होने पर IN PROGRESS, समाप्त होने पर COMPLETED या DISCONTINUED, तथा उत्पन्न सीरीज के साथ। RIS इन रिपोर्टों का उपयोग शेड्यूल अपडेट करने, रेडियोलॉजिस्ट को इमेज़ आने की सूचना देने, और बिलिंग के लिए करता है।</p>

<p><strong>Aspose.Medical for .NET</strong> इस वर्कफ़्लो के दोनों पक्षों को Managed C# में कवर करता है: क्वेरी और रिपोर्ट करने वाली मॉडैलिटी, और उत्तर देने वाला वर्कलिस्ट सर्वर। दोनों DIMSE, क्लासिक DICOM नेटवर्क प्रोटोकॉल, और DICOMweb पर काम करते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C-FIND के साथ वर्कलिस्ट क्वेरी करें">}}

<p>वर्कलिस्ट क्वेरी Modality Worklist सूचना मॉडल में एक C-FIND अनुरोध है। <code>CFindRequest.CreateWorklistQuery</code> इसे सामान्य रिटर्न कीज़ के साथ बनाता है: रोगी जनसांख्यिकी, एक्सेशन नंबर, अनुरोधित प्रक्रिया और निर्धारित प्रोसीजर स्टेप। एक मॉडैलिटी द्वारा सेट किए गए फ़िल्टर उसके अपने AE टाइटल, उसकी मोडैलिटी और तारीख होते हैं, सभी निर्धारित प्रोसीजर स्टेप के भीतर।</p>

<div class="codeblock" id="code">
 <h3>CT स्कैनर के लिए आज की परीक्षाएँ प्राप्त करें - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "RIS_AE",
        Calling = "CT01",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.20"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityWorklistInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddCFindHandler((request, response, cancellationToken) =&gt;
    {
        // Every scheduled exam arrives as one identifier; the final response has none
        if (response.Identifier is Dataset item)
        {
            Dataset step = item.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);
            Console.WriteLine($"{item.GetSingleValueOrDefault(Tag.AccessionNumber, string.Empty)} " +
                              $"{item.GetSingleValueOrDefault(Tag.PatientName, string.Empty)} " +
                              $"{step.GetSingleValueOrDefault(Tag.ScheduledProcedureStepDescription, string.Empty)}");
        }

        return ValueTask.CompletedTask;
    })
    .Build();

// CT exams for this scanner
CFindRequest query = CFindRequest.CreateWorklistQuery(
    patientId: null,
    patientName: null,
    stationAe: "CT01",
    stationName: null,
    modality: "CT",
    scheduledDateTime: null);

// Scheduled for today
query.Identifier.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence)
    .AddOrUpdate(Tag.ScheduledProcedureStepStartDate, DateTimeOffset.Now.ToString("yyyyMMdd"));

client.QueueRequest(query);

await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>प्रत्येक मिलान एक डेटासेट होता है जिसमें शीर्ष स्तर पर रोगी और ऑर्डर विशेषताएँ होती हैं और एक श्रृंखला में निर्धारित प्रोसीजर स्टेप होता है। अधिक रिटर्न कीज़ के लिए, अनुरोध कतार में जाने से पहले उन्हें <code>query.Identifier</code> में खाली मान के साथ जोड़ें।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="MPPS के साथ परीक्षा रिपोर्ट करें">}}

<p>MPPS Modality Performed Procedure Step SOP क्लास पर दो सामान्यीकृत DIMSE ऑपरेशन्स हैं: परीक्षा शुरू होने पर N-CREATE और समाप्त होने पर N-SET। मॉडैलिटी निष्पादित स्टेप का UID चुनती है और दोनों के लिए उपयोग करती है। N-CREATE विशेषताएँ बताती हैं कि किसकी जांच की गई और कौन सा निर्धारित स्टेप निष्पादित हुआ; उनमें से अधिकांश सीधे वर्कलिस्ट आइटम से आते हैं।</p>

<div class="codeblock" id="code">
 <h3>वर्कलिस्ट आइटम से N-CREATE विशेषताएँ - C#</h3>
 <pre><code class="cs">// N-CREATE attributes of a performed procedure step, filled from the worklist item the modality works on
public static Dataset InProgress(Dataset item, string stationAe)
{
    Dataset step = item.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);

    Dataset scheduled = new();
    Copy(item, scheduled, Tag.StudyInstanceUID, Tag.AccessionNumber, Tag.RequestedProcedureID, Tag.RequestedProcedureDescription);
    Copy(step, scheduled, Tag.ScheduledProcedureStepID, Tag.ScheduledProcedureStepDescription);
    scheduled.AddOrUpdate(new Sequence(Tag.ReferencedStudySequence, []));
    scheduled.AddOrUpdate(new Sequence(Tag.ScheduledProtocolCodeSequence, []));

    Dataset mpps = new();
    mpps.AddOrUpdate(new Sequence(Tag.ScheduledStepAttributesSequence, [scheduled]));
    Copy(item, mpps, Tag.PatientName, Tag.PatientID, Tag.PatientBirthDate, Tag.PatientSex, Tag.StudyID);
    Copy(step, mpps, Tag.Modality);
    mpps.AddOrUpdate(new Sequence(Tag.ReferencedPatientSequence, []));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepID, step.GetSingleValueOrDefault(Tag.ScheduledProcedureStepID, string.Empty));
    mpps.AddOrUpdate(Tag.PerformedStationAETitle, stationAe);
    mpps.AddOrUpdate(Tag.PerformedStationName, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedLocation, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedProcedureStepStartDate, DateTimeOffset.Now.ToString("yyyyMMdd"));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepStartTime, DateTimeOffset.Now.ToString("HHmmss"));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepStatus, "IN PROGRESS");
    mpps.AddOrUpdate(Tag.PerformedProcedureStepDescription, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedProcedureTypeDescription, string.Empty);
    mpps.AddOrUpdate(new Sequence(Tag.ProcedureCodeSequence, []));
    mpps.AddOrUpdate(Tag.PerformedProcedureStepEndDate, string.Empty);
    mpps.AddOrUpdate(Tag.PerformedProcedureStepEndTime, string.Empty);
    mpps.AddOrUpdate(new Sequence(Tag.PerformedProtocolCodeSequence, []));
    mpps.AddOrUpdate(new Sequence(Tag.PerformedSeriesSequence, []));
    return mpps;
}

// Copies the attributes the source has; a missing one is sent empty
private static void Copy(Dataset source, Dataset target, params Tag[] tags)
{
    foreach (Tag tag in tags)
    {
        if (source.GetOrDefault(tag) is IElement element)
            target.AddOrUpdate(element);
        else
            target.AddOrUpdate(tag, string.Empty);
    }
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>स्टेप को पूर्ण करने वाली N-SET विशेषताएँ - C#</h3>
 <pre><code class="cs">// N-SET attributes that close the step and list the series the modality produced
public static Dataset Completed(string seriesInstanceUid, string protocolName)
{
    Dataset series = new();
    series.AddOrUpdate(Tag.SeriesInstanceUID, seriesInstanceUid);
    series.AddOrUpdate(Tag.ProtocolName, protocolName);
    series.AddOrUpdate(Tag.SeriesDescription, string.Empty);
    series.AddOrUpdate(Tag.PerformingPhysicianName, string.Empty);
    series.AddOrUpdate(Tag.OperatorsName, string.Empty);
    series.AddOrUpdate(Tag.RetrieveAETitle, string.Empty);
    series.AddOrUpdate(new Sequence(Tag.ReferencedImageSequence, []));
    series.AddOrUpdate(new Sequence(Tag.ReferencedNonImageCompositeSOPInstanceSequence, []));

    Dataset completed = new();
    completed.AddOrUpdate(Tag.PerformedProcedureStepStatus, "COMPLETED");
    completed.AddOrUpdate(Tag.PerformedProcedureStepEndDate, DateTimeOffset.Now.ToString("yyyyMMdd"));
    completed.AddOrUpdate(Tag.PerformedProcedureStepEndTime, DateTimeOffset.Now.ToString("HHmmss"));
    completed.AddOrUpdate(new Sequence(Tag.PerformedSeriesSequence, [series]));
    return completed;
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>IN PROGRESS भेजें, फिर COMPLETED - C#</h3>
 <pre><code class="cs">DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Called = "RIS_AE",
        Calling = "CT01",
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.20"), 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityPerformedProcedureStepSOPClass,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddNCreateHandler((request, response, cancellationToken) =&gt;
    {
        Console.WriteLine($"N-CREATE: 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .AddNSetHandler((request, response, cancellationToken) =&gt;
    {
        Console.WriteLine($"N-SET: 0x{response.Status:X4}");
        return ValueTask.CompletedTask;
    })
    .Build();

// The modality chooses the UID of the performed step; N-SET refers to it later
Uid mppsUid = Uid.CreateDerivedFromUuid();

client.QueueRequest(new NCreateRequest(Uid.ModalityPerformedProcedureStepSOPClass, mppsUid, InProgress(item, "CT01")));
await client.SendAsync(CancellationToken.None);

// ... the scan runs ...

client.QueueRequest(new NSetRequest(Uid.ModalityPerformedProcedureStepSOPClass, mppsUid, Completed("1.2.826.0.1.3680043.10.1.1", "CHEST")));
await client.SendAsync(CancellationToken.None);
await client.StopAsync();</code></pre>
</div>

<p>जो परीक्षा जल्दी रुकती है वह COMPLETED के बजाय DISCONTINUED के साथ समाप्त होती है, और N-SET तब Performed Procedure Step Discontinuation Reason Code Sequence में कारण ले जाता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOMweb पर समान वर्कफ़्लो">}}

<p>नए RIS और क्लाउड प्लेटफ़ॉर्म HTTP के माध्यम से वर्कलिस्ट और MPPS प्रदान करते हैं। Aspose.Medical का <a href="/medical/net/dicomweb-client/">DICOMweb क्लाइंट</a> निर्धारित प्रोसीजर स्टेप्स को खोजता है और ऊपर जैसा ही डेटासेट लेकर निष्पादित स्टेप को रिपोर्ट करता है। निर्धारित प्रोसीजर स्टेप के अंदर की एक कुंजी को पाथ के रूप में लिखा जाता है, उदाहरण के लिए <code>ScheduledProcedureStepSequence.Modality</code>।</p>

<div class="codeblock" id="code">
 <h3>DICOMweb पर वर्कलिस्ट क्वेरी और MPPS रिपोर्ट करें - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();
DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://ris.example.org/dicomweb/")
});

// The same keys as the C-FIND query, as query parameters
DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters =
    [
        new DicomWebQueryParameter("ScheduledProcedureStepSequence.Modality", "CT"),
        new DicomWebQueryParameter("ScheduledProcedureStepSequence.ScheduledStationAETitle", "CT01"),
        new DicomWebQueryParameter("ScheduledProcedureStepSequence.ScheduledProcedureStepStartDate", DateTimeOffset.Now.ToString("yyyyMMdd"))
    ]
};

List&lt;Dataset&gt; worklist = [];
await using (DicomWebClientStream&lt;Dataset&gt; steps = await client.ModalityWorklist.SearchAsync(search))
{
    await foreach (Dataset step in steps)
        worklist.Add(step);
}

// The same MPPS attributes go to the procedure step resource
Dataset item = worklist[0];
string mppsUid = Uid.CreateDerivedFromUuid().Code;
await client.ModalityPerformedProcedureSteps.CreateAsync(mppsUid, InProgress(item, "CT01"));
await client.ModalityPerformedProcedureSteps.UpdateAsync(mppsUid, Completed("1.2.826.0.1.3680043.10.1.2", "CHEST"));</code></pre>
</div>

<p>क्लाइंट आपके अपने <code>HttpClient</code> पर चलता है, इसलिए authentication और TLS वहाँ कॉन्फ़िगर किए जाते हैं。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DIMSE पर आपका अपना वर्कलिस्ट सर्वर">}}

<p>एक वर्कलिस्ट ब्रॉकर, एक छोटा RIS या मोडैलिटी इंटेग्रेशन के लिए टेस्ट सिस्टम को दूसरी ओर उत्तर देना होगा। वर्कलिस्ट के लिए C-FIND हैंडलर और MPPS के लिए N-CREATE और N-SET हैंडलर्स पंजीकृत करें, और एप्लिकेशन एक वर्कलिस्ट SCP और MPPS SCP बन जाता है।</p>

<div class="codeblock" id="code">
 <h3>वर्कलिस्ट और MPPS सर्वर शुरू करें - C#</h3>
 <pre><code class="cs">// One handler instance answers N-CREATE and N-SET, so both see the same steps
MppsHandler mpps = new MppsHandler();

DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions
        {
            TargetHost = new IPEndPoint(IPAddress.Any, 104)
        },
        AssociationNegotiation = new AssociationNegotiationOptions()
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityWorklistInformationModelFIND,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
            .WithPresentationContext(new PresentationContext
            {
                AbstractSyntax = Uid.ModalityPerformedProcedureStepSOPClass,
                Role = null,
                TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ExplicitVrLittleEndian)
            })
    })
    .AddSingletonCFindHandler(new WorklistHandler("worklist"))
    .AddSingletonNCreateHandler(mpps)
    .AddSingletonNSetHandler(mpps)
    .Build();

await server.StartAsync(CancellationToken.None);</code></pre>
</div>

<p>यह हैंडलर निर्धारित परीक्षाओं को <code>.wl</code> फ़ाइलों से पढ़ता है, प्रति फ़ाइल एक डेटासेट, और वह तीन कुंजियों की तुलना करता है जो सामान्यतः मोडैलिटी भेजती हैं। DIMSE पर हैंडलर मिलान को संभालता है: वाइल्डकार्ड, तारीख रेंज और रिटर्न कीज़ की चयन यहाँ लिखी जाती हैं।</p>

<div class="codeblock" id="code">
 <h3>वर्कलिस्ट C-FIND हैंडलर - C#</h3>
 <pre><code class="cs">// Answers worklist queries from the .wl files in a folder, one scheduled exam per file
public sealed class WorklistHandler(string folder) : ICFindRequestHandler
{
    public async IAsyncEnumerable&lt;CFindResponse&gt; Handle(CFindRequest request,
        [EnumeratorCancellation] CancellationToken cancellationToken)
    {
        // A modality filters inside the scheduled procedure step: its AE title, its modality, the date
        Dataset keys = request.Identifier.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);
        Tag[] filters = [Tag.ScheduledStationAETitle, Tag.Modality, Tag.ScheduledProcedureStepStartDate];

        foreach (string path in Directory.EnumerateFiles(folder, "*.wl"))
        {
            DicomFile file = await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
            Dataset step = file.Dataset.GetSingleValue&lt;Dataset&gt;(Tag.ScheduledProcedureStepSequence);
            if (filters.All(tag =&gt; Matches(keys, step, tag)))
                yield return WithStatus(new CFindResponse(file.Dataset), 0xFF00);
        }

        yield return WithStatus(new CFindResponse(null), 0x0000);
    }

    // Single value matching: an empty key matches every value
    private static bool Matches(Dataset keys, Dataset step, Tag tag)
    {
        string key = keys.GetSingleValueOrDefault(tag, string.Empty)!;
        return key.Length == 0 || key == step.GetSingleValueOrDefault(tag, string.Empty);
    }

    private static CFindResponse WithStatus(CFindResponse response, ushort status)
    {
        response.Command.AddOrUpdate(Tag.Status, status);
        return response;
    }
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>MPPS N-CREATE और N-SET हैंडलर - C#</h3>
 <pre><code class="cs">// Receives the performed procedure steps the modalities report
public sealed class MppsHandler : INCreateRequestHandler, INSetRequestHandler
{
    private readonly ConcurrentDictionary&lt;string, Dataset&gt; _steps = new();

    // N-CREATE: the exam has started
    public ValueTask&lt;NCreateResponse&gt; Handle(NCreateRequest request, CancellationToken cancellationToken)
    {
        bool created = _steps.TryAdd(request.AffectedSopInstanceUid.Code, request.Attributes ?? new Dataset());

        NCreateResponse response = new(null);
        response.Command.AddOrUpdate(Tag.Status, created ? (ushort)0x0000 : (ushort)0x0111); // 0x0111: duplicate instance
        return ValueTask.FromResult(response);
    }

    // N-SET: the exam is COMPLETED or DISCONTINUED
    public ValueTask&lt;NSetResponse&gt; Handle(NSetRequest request, CancellationToken cancellationToken)
    {
        NSetResponse response = new(null);
        if (!_steps.TryGetValue(request.RequestedSopInstanceUid.Code, out Dataset? step))
        {
            response.Command.AddOrUpdate(Tag.Status, (ushort)0x0112); // 0x0112: no such instance
            return ValueTask.FromResult(response);
        }

        step.AddOrUpdateRange(request.ModificationList);
        Console.WriteLine($"{step.GetSingleValueOrDefault(Tag.PatientID, string.Empty)}: " +
                          $"{step.GetSingleValueOrDefault(Tag.PerformedProcedureStepStatus, string.Empty)}");

        response.Command.AddOrUpdate(Tag.Status, (ushort)0x0000);
        return ValueTask.FromResult(response);
    }
}</code></pre>
</div>

<p>यहाँ स्टेप्स मेमोरी में रखे जाते हैं और स्थिति प्रिंट की जाती है। वास्तविक MPPS SCP स्टेप को संग्रहित करता है, पहले से COMPLETED या DISCONTINUED स्टेप में बदलाव को अस्वीकार करता है, और परिणाम RIS को भेजता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOMweb पर आपका अपना वर्कलिस्ट सर्वर">}}

<p>DICOMweb पर लाइब्रेरी अधिक सर्वर कार्य करती है। वर्कलिस्ट खोज के लिए यह DICOM मिलान नियम, वाइल्डकार्ड और तारीख रेंज लागू करता है और परिणामों को पेज करता है। MPPS के लिए यह आवश्यक विशेषताओं और स्थिति परिवर्तन की जाँच करता है। आपके क्लास डेटा प्रदान करते हैं।</p>

<div class="codeblock" id="code">
 <h3>DICOMweb वर्कलिस्ट और MPPS सर्वर का Program.cs - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// The scheduled exams and the reported procedure steps
builder.Services.AddSingleton&lt;IDicomWebModalityWorklistProvider&gt;(new FolderWorklist("worklist"));
builder.Services.AddSingleton&lt;IDicomWebMppsProvider, MppsStore&gt;();
builder.Services.AddDicomWeb()
    .AddStandardModalityWorklistService()
    .AddStandardModalityPerformedProcedureStepService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>वर्कलिस्ट और MPPS स्टोरेज - C#</h3>
 <pre><code class="cs">// The scheduled exams for DICOMweb searches; the library applies the matching rules and the paging
public sealed class FolderWorklist(string folder) : IDicomWebModalityWorklistProvider
{
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Directory.EnumerateFiles(folder, "*.wl").Order(StringComparer.Ordinal))
            yield return (await DicomFile.OpenAsync(path, cancellationToken: cancellationToken)).Dataset;
    }
}

// The performed procedure steps; the library checks the attributes and the state changes
public sealed class MppsStore : IDicomWebMppsProvider
{
    private readonly ConcurrentDictionary&lt;string, DicomWebVersionedDataset&gt; _steps = new();

    public ValueTask&lt;DicomWebVersionedDataset?&gt; GetAsync(string mppsUid, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_steps.TryGetValue(mppsUid, out DicomWebVersionedDataset? step) ? Copy(step) : null);

    public ValueTask&lt;bool&gt; TryCreateAsync(string mppsUid, Dataset procedureStep, CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_steps.TryAdd(mppsUid, NewRevision(procedureStep)));

    // Replaces the step only if nobody changed it since it was read
    public ValueTask&lt;bool&gt; TryReplaceAsync(string mppsUid, string expectedRevision, Dataset procedureStep,
        CancellationToken cancellationToken = default) =&gt;
        ValueTask.FromResult(_steps.TryGetValue(mppsUid, out DicomWebVersionedDataset? current)
                             &amp;&amp; current.Revision == expectedRevision
                             &amp;&amp; _steps.TryUpdate(mppsUid, NewRevision(procedureStep), current));

    private static DicomWebVersionedDataset NewRevision(Dataset dataset) =&gt;
        new() { Dataset = new Dataset(dataset), Revision = Guid.NewGuid().ToString("N") };

    private static DicomWebVersionedDataset Copy(DicomWebVersionedDataset step) =&gt;
        new() { Dataset = new Dataset(step.Dataset), Revision = step.Revision };
}</code></pre>
</div>

<p>सर्वर को <code>Aspose.Medical.Dicom.Web.AspNetCore</code> पैकेज की आवश्यकता है। <a href="/medical/net/dicomweb-server/">DICOMweb सर्वर पेज</a> दिखाता है कि समान एप्लिकेशन में Studies सर्विसेज़ कैसे जोड़ें, और <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/workflow-providers/">वर्कफ़्लो प्रोवाइडर्स गाइड</a> <code>TryReplaceAsync</code> के रिविजन कॉन्ट्रैक्ट को समझाता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="मॉडैलिटी से आगे की वर्कलिस्ट">}}

<p>MWL और MPPS एक मॉडैलिटी को RIS से जोड़ते हैं। अन्य सिस्टमों के बीच चलने वाले कार्य जैसे AI प्रोसेसिंग, रीडिंग असाइनमेंट या पोस्ट‑प्रोसेसिंग के लिए, DICOM Unified Procedure Step को परिभाषित करता है। <a href="/medical/net/ups-rs/">UPS-RS पेज</a> दिखाता है कि समान लाइब्रेरी के साथ ऐसे कार्य आइटम बनाना, दावा करना और पूरा करना कैसे है।</p>

<p>संबंधित पृष्ठ: C-ECHO, C-STORE और अन्य DIMSE सर्विसेज़ के लिए <a href="/medical/net/dicom-networking/">DICOM नेटवर्किंग</a>, HTTP पर स्टडीज़ के लिए <a href="/medical/net/dicomweb-client/">DICOMweb क्लाइंट</a> और <a href="/medical/net/dicomweb-server/">DICOMweb सर्वर</a>, और यह जानने के लिए कि वर्कलिस्ट आइटम में क्या है, <a href="/medical/net/dicom-tags/">DICOM टैग्स</a>।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/dicom-networking/" >}}
{{< blocks/products/pf/slr-element name="API संदर्भ" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="मुफ़्त समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="भुगतान समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Aspose.Medical for .NET क्यों?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता की कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
