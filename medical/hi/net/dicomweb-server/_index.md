---
title: C# .NET में DICOMweb सर्वर - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: C# में अपना स्वयं का DICOMweb सर्वर या PACS बनाएँ। Aspose.Medical ASP.NET Core पर QIDO-RS, WADO-RS, STOW-RS और कार्यसूची सेवाओं को लागू करता है; आप स्टोरेज कनेक्ट करते हैं।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# के लिए DICOMweb सर्वर" h2="ASP.NET Core सेवा में QIDO-RS, WADO-RS और STOW-RS एंडपॉइंट जोड़ें। लाइब्रेरी प्रोटोकॉल को समझती है; आपका कोड तय करता है कि अध्ययन कहाँ संग्रहीत हों।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="C# में आपका अपना DICOMweb सर्वर">}}

<p>एक PACS, एक VNA, एक क्लाउड आर्काइव या एक रिसर्च प्लेटफ़ॉर्म को DICOMweb अनुरोधों का उत्तर देना होता है: QIDO-RS खोज, WADO-RS डाउनलोड और STOW-RS अपलोड, तथा अक्सर कार्यसूची सेवाएँ भी। प्रत्येक की अपनी रूट, मीडिया टाइप, मल्टीपार्ट बॉडी, मिलान नियम और स्थिति कोड होते हैं।</p>

<p><strong>Aspose.Medical for .NET</strong> ने संस्करण 26.10 से इस प्रोटोकॉल पक्ष को लागू किया है। यह प्रत्येक अनुरोध को पढ़ता है, इसे मानक के विरुद्ध जांचता है, DICOM नियमों द्वारा खोज कुंजियों को मिलाता है, परिणामों को पेज करता है, ट्रांसफ़र सिंटैक्स को परिवर्तित करता है और प्रतिक्रिया लिखता है। आपका कोड वही करता है जो केवल आप कर सकते हैं: एक इंस्टेंस संग्रहीत करना, खोज के लिए उम्मीदवार ढूँढ़ना और संग्रहीत फ़ाइलें लौटाना। व्यूअर्स, मोडैलिटीज़ और अन्य आर्काइव तब आपके सर्वर के साथ किसी भी अन्य DICOMweb सर्वर की तरह काम करते हैं, और उसी लाइब्रेरी का <a href="/medical/net/dicomweb-client/">DICOMweb क्लाइंट</a> भी ऐसा ही करता है।</p>

<p>सर्वर C# द्वारा प्रबंधित है। यह कहीं भी चलता है जहाँ ASP.NET Core चलता है: Windows पर, Linux पर और कंटेनर में।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ASP.NET Core में एक पूर्ण सर्वर">}}

<p><code>Aspose.Medical.Dicom.Web.AspNetCore</code> पैकेज इंस्टॉल करें। यह Aspose.Medical को साथ लाता है और DICOMweb सेवाओं को डिपेंडेंसी इंजेक्शन और एंडपॉइंट रूटिंग से जोड़ता है। यह <code>Program.cs</code> एक कार्यशील Studies सर्वर है; यह पंजीकृत दो क्लासेज़ नीचे दिखाए गए हैं।</p>

<div class="codeblock" id="code">
 <h3>DICOMweb सर्वर का Program.cs - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>अब प्रत्येक Studies रूट <code>/dicomweb</code> के तहत उत्तर देता है: खोज और अपलोड के लिए <code>/dicomweb/studies</code>, पुनर्प्राप्ति के लिए <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code>, और मेटाडेटा, फ्रेम, बल्क डेटा और रेंडर्ड इमेज़ के रूट्स। सर्विस रूट पर क्षमताओं का डॉक्यूमेंट ठीक वही ट्रांज़ैक्शन सूचीबद्ध करता है जिन्हें सर्वर सपोर्ट करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="अपना स्टोरेज कनेक्ट करें">}}

<p>Studies सेवा को एक इंटरफ़ेस की आवश्यकता होती है जिसमें तीन मेथड हों: एक इंस्टेंस संग्रहीत करना, संग्रहीत फ़ाइलें लौटाना, और खोज उम्मीदवार लौटाना। यह इम्प्लीमेंटेशन प्रत्येक इंस्टेंस को अध्ययन और सीरीज़ के अनुसार फ़ोल्डर में फ़ाइल के रूप में रखता है।</p>

<div class="codeblock" id="code">
 <h3>QIDO-RS, WADO-RS और STOW-RS के पीछे एक फ़ोल्डर आर्काइव - C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>लाइब्रेरी प्रत्येक अनुरोध पथ और प्रत्येक अपलोड किए गए इंस्टेंस में UIDs की जाँच करती है इससे पहले कि वह प्रोवाइडर को कॉल करे। वैध UIDs के बिना एक इंस्टेंस को विफल के रूप में रिपोर्ट किया जाता है और कभी भी <code>StoreAsync</code> तक नहीं पहुँचता।</p>

<p>सर्वर को आज़माने के लिए एक फ़ोल्डर पर्याप्त है। एक प्रोडक्शन आर्काइव समान तीन मेथड रखता है और <code>StoreAsync</code> और <code>RetrieveAsync</code> के पीछे एक ऑब्जेक्ट स्टोर तथा <code>FindCandidatesAsync</code> के पीछे एक डेटाबेस इंडेक्स रखता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="खोज: लाइब्रेरी DICOM मिलान नियम लागू करती है">}}

<p>QIDO-RS मिलान केवल स्ट्रिंग तुलना से अधिक है: वाइल्डकार्ड, तिथि और समय रेंज, UIDs की सूची, फज़ी व्यक्ति नाम, खाली मान और क्रमों के भीतर की कुंजियाँ। प्रोवाइडर इनमें से कोई भी लागू नहीं करता। यह उम्मीदवारों को एक स्थिर क्रम में लौटाता है, और मिलान से अधिक भी लौटाया जा सकता है। लाइब्रेरी प्रत्येक उम्मीदवार पर सटीक DICOM मिलान लागू करती है, अनुरोधित फ़ील्ड रखती है, और <code>offset</code> तथा <code>limit</code> लागू करती है। एक प्रतिक्रिया डिफ़ॉल्ट रूप से अधिकतम 1000 परिणाम रखती है, और <code>DicomWebStandardServiceOptions</code> इस सीमा को बदलता है।</p>

<p>एक बड़े आर्काइव पर खोजों को तेज़ रखने के लिए, चयनात्मक कुंजियों को डेटाबेस में पास करें। <code>search.Plan.Filter</code> संकलित खोज कुंजियों को रखता है, जिसमें एट्रिब्यूट, मिलान का प्रकार और प्रत्येक का मान शामिल है:</p>

<div class="codeblock" id="code">
 <h3>खोज से एक सटीक Patient ID पढ़ें - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>ऐसे निर्मित डेटाबेस क्वेरी अतिरिक्त पंक्तियाँ लौट सकती हैं, उदाहरण के लिए जब यह किसी ऐसी कुंजी को नजरअंदाज करती है जिसे वह अनुवाद नहीं कर सकता। लाइब्रेरी में अंतिम मिलान इन्हें हटा देता है। यह सही मिलानों को नहीं गिराना चाहिए, क्योंकि लाइब्रेरी उन्हें वापस नहीं जोड़ सकती।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="संग्रहण: प्रत्येक इंस्टेंस के लिए एक परिणाम">}}

<p>एक STOW-RS अनुरोध सैकड़ों इंस्टेंस ले जा सकता है। लाइब्रेरी मल्टीपार्ट बॉडी को पार्स करती है और क्रमशः प्रत्येक इंस्टेंस के लिए <code>StoreAsync</code> को कॉल करती है, इसलिए पूरी अपलोड एक बार में मेमोरी में नहीं रहती। प्रत्येक इंस्टेंस के लिए प्रोवाइडर <code>Stored</code>, बदलावों के साथ संग्रहीत इंस्टेंस के लिए <code>Warning</code>, या वैकल्पिक DICOM कारण कोड के साथ <code>Failed</code> लौटाता है।</p>

<p>लाइब्रेरी इन परिणामों से STOW-RS रिपोर्ट बनाती है और स्थिति चुनती है: जब सभी इंस्टेंस संग्रहीत हों तो 200, कुछ पर चेतावनियाँ या विफलता हों तो 202, और सभी विफल हों तो 409। एक नामित अध्ययन के लिए अनुरोध में ऐसी इंस्टेंस जो किसी अन्य अध्ययन से संबंधित हों, प्रोवाइडर को कॉल करने से पहले अस्वीकार कर दी जाती हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="प्राप्ति: ट्रांसफ़र सिंटैक्स, फ्रेम और रेंडर्ड इमेज़">}}

<p>प्रोवाइडर संग्रहीत फ़ाइलें लौटाता है, और लाइब्रेरी उन्हें उस रूप में प्रदान करती है जो क्लाइंट ने माँगा है। एक WADO-RS क्लाइंट <code>Accept</code> हेडर में ट्रांसफ़र सिंटैक्स माँग सकता है, और लाइब्रेरी प्रत्येक फ़ाइल को भेजने से पहले अपने कोडेक, HTJ2K और JPEG XL सहित परिवर्तित करती है। DICOM JSON या XML में मेटाडेटा, बल्क डेटा और सिंगल फ्रेम समान प्रोवाइडर से आते हैं, बिना अतिरिक्त कोड के।</p>

<p>रेंडर्ड इमेज़ के लिए एक अतिरिक्त क्लास की आवश्यकता होती है, क्योंकि केवल आप तय करते हैं कि सर्वर कौन सी इमेज़ उत्पन्न कर सकता है। यह क्लास रेंडर्ड इंस्टेंस, रेंडर्ड फ्रेम और थंबनेल के लिए JPEG प्रदान करता है, और लाइब्रेरी उन्हें रेंडर करती है:</p>

<div class="codeblock" id="code">
 <h3>रेंडर्ड JPEG इमेज़ और थंबनेल प्रदान करें - C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>बिल्ट‑इन रेंडरर एक सिंगल‑फ़्रेम इंस्टेंस या अनुरोध द्वारा चयनित फ्रेम से JPEG उत्पन्न करता है। एक संपूर्ण रेंडर्ड मल्टी‑फ़्रेम इंस्टेंस, 65500 पिक्सेल से बड़ी या ऊँची इमेज (थंबनेल ऐसे इमेज को पार करता है), अन्य इमेज फ़ॉर्मैट, तथा viewport, window और ICC प्रोफ़ाइल पैरामीटरों के लिए इस क्लास में आपका अपना <code>RetrieveAsync</code> आवश्यक है।</p>

<p>थंबनेल को रोगी गोपनीयता को ध्यान में रखकर संभाला जाता है। बिल्ट‑इन रेंडरर केवल उन इंस्टेंस से थंबनेल बनाता है जिनके Burned In Annotation और Recognizable Visual Features एट्रिब्यूट दोनों <code>NO</code> हैं, जिसका अर्थ है कि इमेज स्वयं यह घोषित करती है कि उसमें कोई रोगी डेटा नहीं दिखता। कई इमेज में ये एट्रिब्यूट नहीं होते, और ऐसे मामलों में सर्वर 406 उत्तर देता है। यदि आपके आर्काइव को ऐसी इमेज के थंबनेल चाहिए, तो उन्हें अपने <code>RetrieveAsync</code> में बनाएँ और सुनिश्चित करें कि वे सभी रोगी डेटा को छिपाएँ।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="वर्कलिस्ट, प्रक्रिया चरण और अन्य सेवाएँ">}}

<p>अन्य DICOMweb सेवाएँ समान मॉडल का पालन करती हैं: सेवा को रजिस्टर्ड करें, उसकी स्टोरेज इंटरफ़ेस को लागू करें।</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> वर्कलिस्ट <code>AddStandardWorklistService</code> के साथ: कार्य आइटम बनाना, खोजना और अपडेट करना और उनकी स्थिति बदलना, जहाँ स्थिति नियम लाइब्रेरी द्वारा जाँचें जाते हैं और इवेंट नोटिफ़िकेशन WebSocket पर भेजे जाते हैं।</li>
<li><a href="/medical/net/modality-worklist/">Modality वर्कलिस्ट</a> (MWL-RS) <code>AddStandardModalityWorklistService</code> के साथ, जिससे मोडैलिटीज़ आपके RIS के निर्धारित प्रक्रिया चरणों को क्वेरी कर सकें।</li>
<li>Modality performed procedure steps (MPPS-RS), स्टोरेज कमिटमेंट और रंग पैलेट व हेंगिंग प्रोटोकॉल जैसे नॉन‑पेशेंट इंस्टेंस।</li>
<li>पुराने व्यूअर्स के लिए WADO-URI पुनर्प्राप्ति, जो Studies सेवा द्वारा प्रदान की जाती है।</li>
</ul>

<p>जब किसी ट्रांज़ैक्शन का मानक व्यवहार उपयुक्त नहीं हो, तो <code>AddHandler</code> के साथ अपना स्वयं का हैंडलर रजिस्टर्ड करें। लाइब्रेरी अभी भी अनुरोध को रूट और चेक करती है, और अन्य ट्रांज़ैक्शन अपने मानक हैंडलर रखते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="सुरक्षा और होस्टिंग आपके हाथ में रहें">}}

<p>DICOMweb एंडपॉइंट सामान्य ASP.NET Core एंडपॉइंट होते हैं। ऑथेंटिकेशन, ऑथराइज़ेशन, CORS, TLS, फॉरवर्डेड हेडर और अनुरोध आकार सीमाएँ आपके सर्विस के बाकी हिस्सों की तरह ही कॉन्फ़िगर की जाती हैं:</p>

<div class="codeblock" id="code">
 <h3>प्रमाणित उपयोगकर्ता की आवश्यकता - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>प्रोटोकॉल भाग ASP.NET Core पर निर्भर नहीं करता। <code>DicomWebApplication</code> एक <code>DicomWebRequest</code> को प्रतिक्रिया में बदल देता है, इसलिए समान सेवाएँ किसी अन्य HTTP सर्वर के पीछे या बिना नेटवर्क के टेस्ट में चलायी जा सकती हैं:</p>

<div class="codeblock" id="code">
 <h3>एक ही सर्वर बिना ASP.NET Core के - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="एक लाइब्रेरी से क्लाइंट और सर्वर">}}

<p>Aspose.Medical का <a href="/medical/net/dicomweb-client/">DICOMweb क्लाइंट</a> आपके सर्वर को यूनिट टेस्ट या कंसोल प्रोग्राम से टेस्ट करता है, और सर्वर द्वारा प्राप्त प्रत्येक फ़ाइल एक सामान्य <code>DicomFile</code> होती है जिसे समान प्रक्रिया में अनामिकरण, ट्रांसकोडिंग या रेंडर किया जा सकता है।</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb डेवलपर गाइड</a> प्रत्येक सेवा और स्टोरेज इंटरफ़ेस को कवर करता है, और <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core गाइड</a> एंडपॉइंट मैपिंग और WebSockets को कवर करता है।</p>

<p>संबंधित पृष्ठ: <a href="/medical/net/dicom-networking/">DICOM नेटवर्किंग</a> उन सिस्टमों के लिए जो HTTP के बजाय DIMSE के माध्यम से अध्ययन भेजते हैं, <a href="/medical/net/anonymization/">अनामिकरण</a> इनकमिंग अध्ययनों का, <a href="/medical/net/dicom-transfer-syntax-conversion/">ट्रांसफ़र सिंटैक्स रूपांतरण</a>, और <a href="/medical/net/dicom-to-json/">DICOM से JSON</a> QIDO-RS परिणामों के फ़ॉर्मेट के लिए।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="API संदर्भ" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="मुफ़्त समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="भुगतान समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता की कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
