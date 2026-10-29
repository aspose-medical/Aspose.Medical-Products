---
title: C# .NET में DICOMweb सर्वर - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: C# में अपना स्वयं का DICOMweb सर्वर या PACS बनाएं। Aspose.Medical QIDO-RS, WADO-RS, STOW-RS और वर्कलिस्ट सेवाओं को ASP.NET Core पर लागू करता है; आप स्टोरेज को कनेक्ट करते हैं।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# के लिए DICOMweb सर्वर" h2="ASP.NET Core सेवा में QIDO-RS, WADO-RS और STOW-RS एंडपॉइंट जोड़ें। लाइब्रेरी प्रोटोकॉल को समझती है; आपका कोड तय करता है कि अध्ययन कहाँ संग्रहीत हों।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="C# में आपका अपना DICOMweb सर्वर">}}

<p>एक PACS, VNA, क्लाउड आर्काइव या शोध प्लेटफ़ॉर्म को DICOMweb अनुरोधों का उत्तर देना होता है: QIDO-RS खोज, WADO-RS डाउनलोड और STOW-RS अपलोड, तथा अक्सर वर्कलिस्ट सेवाएँ भी। प्रत्येक का अपना रूट, मीडिया टाइप, मल्टीपार्ट बॉडी, मिलान नियम और स्टेटस कोड होता है।</p>

<p><strong>Aspose.Medical for .NET</strong> संस्करण 26.10 से इस प्रोटोकॉल पक्ष को लागू करता है। यह हर अनुरोध को पढ़ता है, मानक के अनुसार जांचता है, DICOM नियमों के अनुसार खोज कुंजियों का मिलान करता है, परिणामों को पेज करता है, ट्रांसफ़र सिंटैक्स को परिवर्तित करता है और प्रतिक्रिया लिखता है। आपका कोड वह करता है जिसे केवल आप ही कर सकते हैं: एक इंस्टेंस को स्टोर करना, खोज के लिए उम्मीदवार खोजना और संग्रहीत फ़ाइलें लौटाना। व्यूअर्स, मोडैलिटीज़ और अन्य आर्काइव फिर आपके सर्वर के साथ किसी भी अन्य DICOMweb सर्वर की तरह कार्य करते हैं, और इसी तरह <a href="/medical/net/dicomweb-client/">DICOMweb क्लाइंट</a> भी उसी लाइब्रेरी का।</p>

<p>सर्वर C# में प्रबंधित है। यह जहाँ भी ASP.NET Core चलता है, वहाँ चलता है: Windows, Linux और कंटेनर में।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ASP.NET Core में एक पूर्ण सर्वर">}}

<p><code>Aspose.Medical.Dicom.Web.AspNetCore</code> पैकेज स्थापित करें। यह Aspose.Medical को साथ लाता है और DICOMweb सेवाओं को डिपेंडेंसी इंजेक्शन और एंडपॉइंट रूटिंग से जोड़ता है। यह <code>Program.cs</code> एक कार्यशील Studies सर्वर है; नीचे दो क्लासेज़ दिखाए गए हैं जो यह रजिस्टर करता है।</p>

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

<p>अब हर Studies रूट <code>/dicomweb</code> के अंतर्गत उत्तर देता है: खोज और अपलोड के लिए <code>/dicomweb/studies</code>, पुनः प्राप्ति के लिए <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code>, और मेटाडाटा, फ्रेम्स, बल्क डेटा तथा रेंडर किए गए इमेज़ के लिए रूट्स। सेवा रूट पर स्थित क्षमताओं दस्तावेज़ में ठीक वही लेन‑देन सूचीबद्ध हैं जिन्हें सर्वर समर्थन करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="अपने स्टोरेज को कनेक्ट करें">}}

<p>Studies सेवा को एक इंटरफ़ेस की आवश्यकता होती है जिसमें तीन मेथड्स हों: एक इंस्टेंस को स्टोर करना, संग्रहीत फ़ाइलें लौटाना, और खोज के उम्मीदवार लौटाना। यह कार्यान्वयन प्रत्येक इंस्टेंस को अध्ययन और सीरीज़ के अनुसार फोल्डर में फ़ाइल के रूप में रखता है।</p>

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

<p>लाइब्रेरी हर अनुरोध पाथ और हर अपलोड किए गए इंस्टेंस में UID की जाँच करती है इससे पहले कि वह प्रोवाइडर को कॉल करे। वैध UID न होने वाला इंस्टेंस असफल के रूप में रिपोर्ट किया जाता है और कभी भी <code>StoreAsync</code> तक नहीं पहुँचता।</p>

<p>सर्वर को आज़माने के लिए एक फ़ोल्डर पर्याप्त है। प्रोडक्शन आर्काइव भी वही तीन मेथड रखता है और <code>StoreAsync</code> तथा <code>RetrieveAsync</code> के पीछे एक ऑब्जेक्ट स्टोर तथा <code>FindCandidatesAsync</code> के पीछे एक डेटाबेस इंडेक्स रखता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="खोज: लाइब्रेरी DICOM मिलान नियम लागू करती है">}}

<p>QIDO-RS मिलान सिर्फ स्ट्रिंग तुलना से अधिक है: वाइल्डकार्ड, तिथि व समय रेंज, UID की सूचियाँ, फ़ज़ी पर्सन नाम, खाली मान और सीक्वेन्स के भीतर कुंजियाँ। प्रोवाइडर इनमें से कोई भी लागू नहीं करता। यह उम्मीदवारों को स्थिर क्रम में लौटाता है, और मिलान से अधिक भी लौट सकता है। लाइब्रेरी प्रत्येक उम्मीदवार पर सटीक DICOM मिलान लागू करती है, अनुरोधित फ़ील्ड रखती है, तथा <code>offset</code> और <code>limit</code> लागू करती है। डिफ़ॉल्ट रूप से एक प्रतिक्रिया अधिकतम 1000 परिणाम रखती है, और <code>DicomWebStandardServiceOptions</code> इस सीमा को बदलता है।</p>

<p>एक बड़े आर्काइव में खोज को तेज रखने के लिए चयनात्मक कुंजियों को डेटाबेस को पास करें। <code>search.Plan.Filter</code> संकलित खोज कुंजियों को रखता है, जिसमें एट्रीब्यूट, मिलान का प्रकार और प्रत्येक का मान शामिल है:</p>

<div class="codeblock" id="code">
 <h3>खोज से सटीक Patient ID पढ़ें - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>ऐसे बनाए गए डेटाबेस क्वेरी अतिरिक्त पंक्तियाँ लौटा सकते हैं, उदाहरण के तौर पर जब वह अनुवाद न हो सकने वाली कुंजी को अनदेखा करता है। लाइब्रेरी में अंतिम मिलान इन्हें हटा देता है। यह वास्तविक मिलान को हटाना नहीं चाहिए, क्योंकि लाइब्रेरी उन्हें वापस नहीं जोड़ सकती।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="स्टोर: प्रत्येक इंस्टेंस के लिए एक परिणाम">}}

<p>एक STOW-RS अनुरोध सैकड़ों इंस्टेंस ले जा सकता है। लाइब्रेरी मल्टीपार्ट बॉडी को पार्स करती है और क्रमशः प्रत्येक इंस्टेंस के लिए <code>StoreAsync</code> को कॉल करती है, इसलिए सम्पूर्ण अपलोड एक बार में मेमोरी में नहीं रहता। प्रत्येक इंस्टेंस के लिए प्रोवाइडर <code>Stored</code>, बदलाव के साथ स्टोर किए गए इंस्टेंस के लिए <code>Warning</code>, या वैकल्पिक DICOM कारण कोड के साथ <code>Failed</code> लौटाता है।</p>

<p>लाइब्रेरी इन परिणामों से STOW-RS रिपोर्ट बनाती है और स्थिति चुनती है: जब सभी इंस्टेंस स्टोर हो जाएँ तो 200, कुछ में चेतावनी या विफलता हो तो 202, सभी विफल हों तो 409। एक अनुरोधित नामित अध्ययन के लिए ऐसे इंस्टेंस जो किसी अन्य अध्ययन से संबंधित हों, प्रोवाइडर को कॉल करने से पहले ही अस्वीकार कर दिए जाते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="रीट्रिव: ट्रांसफ़र सिंटैक्स, फ्रेम्स और रेंडर किए गए इमेज़">}}

<p>प्रोवाइडर संग्रहीत फ़ाइलें लौटाता है, और लाइब्रेरी उन्हें क्लाइंट द्वारा मांगे गए रूप में प्रदान करती है। एक WADO-RS क्लाइंट <code>Accept</code> हेडर में ट्रांसफ़र सिंटैक्स का अनुरोध कर सकता है, और लाइब्रेरी प्रत्येक फ़ाइल को भेजने से पहले परिवर्तित करती है, अपने स्वयं के कोडेक्स, HTJ2K और JPEG XL सहित। DICOM JSON या XML में मेटाडाटा, बल्क डेटा और सिंगल फ्रेम्स भी उसी प्रोवाइडर से आते हैं, अतिरिक्त कोड की आवश्यकता नहीं।</p>

<p>रेंडर किए गए इमेज़ के लिए एक अतिरिक्त क्लास की आवश्यकता होती है, क्योंकि केवल आप तय करते हैं कि सर्वर किन इमेज़ को उत्पन्न कर सकता है। यह क्लास रेंडर किए गए इंस्टेंस, रेंडर फ्रेम और थंबनेल के लिए JPEG उपलब्ध कराती है, और लाइब्रेरी उन्हें रेंडर करती है:</p>

<div class="codeblock" id="code">
 <h3>रेंडर किए गए JPEG इमेज़ और थंबनेल प्रदान करें - C#</h3>
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

<p>बिल्ट‑इन रेंडरर एक सिंगल‑फ़्रेम इंस्टेंस या अनुरोध द्वारा चयनित फ्रेम्स से JPEG उत्पन्न करता है। एक रेंडर किया गया मल्टी‑फ़्रेम इंस्टेंस पूरी तरह से, अन्य इमेज फ़ॉर्मेट, तथा viewport, window और ICC प्रोफ़ाइल पैरामीटर इस क्लास में आपके अपने <code>RetrieveAsync</code> की आवश्यकता रखते हैं।</p>

<p>थंबनेल को रोगी की गोपनीयता के लिए सावधानीपूर्वक संभाला जाता है। बिल्ट‑इन रेंडरर केवल उन इंस्टेंस से थंबनेल बनाता है जिनके Burned In Annotation और Recognizable Visual Features एट्रीब्यूट दोनों <code>NO</code> हों, जिसका अर्थ है कि इमेज स्वयं यह घोषित करता है कि उसमें कोई रोगी डेटा नहीं है। कई इमेज इन एट्रीब्यूट्स को नहीं ले जाते, और उनके लिए सर्वर 406 जवाब देता है। यदि आपके आर्काइव को ऐसी इमेज के थंबनेल चाहिए, तो उन्हें अपने स्वयं के <code>RetrieveAsync</code> में उत्पन्न करें और सुनिश्चित करें कि वे कोई भी रोगी डेटा न दिखाएँ।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="वर्कलिस्ट, प्रोसीजर स्टेप्स और अन्य सेवाएँ">}}

<p>अन्य DICOMweb सेवाएँ भी वही मॉडल अपनाती हैं: सेवा को रजिस्टर करें, उसकी स्टोरेज इंटरफ़ेस को लागू करें।</p>

<ul>
<li>UPS-RS वर्कलिस्ट <code>AddStandardWorklistService</code> के साथ: वर्कआइटम बनाना, खोजना और अपडेट करना तथा उनकी स्थिति बदलना, जिसमें स्थिति नियम लाइब्रेरी द्वारा जाँचे जाते हैं और इवेंट नोटिफिकेशन WebSocket के माध्यम से भेजे जाते हैं।</li>
<li>मोडैलिटी वर्कलिस्ट (MWL-RS) <code>AddStandardModalityWorklistService</code> के साथ, जिससे मोडैलिटीज़ आपके RIS के निर्धारित प्रोसीजर स्टेप्स को क्वेरी कर सकते हैं।</li>
<li>Modality performed procedure steps (MPPS-RS), स्टोरेज कमिटमेंट और रंग पैलेट तथा हैंगिंग प्रोटोकॉल जैसी नॉन‑पेशेंट इंस्टेंस।</li>
<li>पुराने व्यूअर्स के लिए WADO-URI रीट्रिव, जो Studies सेवा द्वारा प्रदान किया जाता है।</li>
</ul>

<p>जब किसी ट्रांज़ैक्शन का मानक व्यवहार फिट नहीं बैठता, तो <code>AddHandler</code> के साथ अपना हैंडलर रजिस्टर करें। लाइब्रेरी अभी भी अनुरोध को रूट और जांचती है, और अन्य ट्रांज़ैक्शन अपने मानक हैंडलर रखते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="सुरक्षा और होस्टिंग आपके नियंत्रण में रहती है">}}

<p>DICOMweb एंडपॉइंट साधारण ASP.NET Core एंडपॉइंट हैं। ऑथेंटिकेशन, ऑथराइज़ेशन, CORS, TLS, फॉरवर्डेड हेडर्स और रिक्वेस्ट साइज लिमिट्स को आपके सेवा के बाकी हिस्सों की तरह ही कॉन्फ़िगर किया जाता है:</p>

<div class="codeblock" id="code">
 <h3>प्रामाणिक उपयोगकर्ता की आवश्यकता - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>प्रोटोकॉल भाग ASP.NET Core पर निर्भर नहीं है। <code>DicomWebApplication</code> एक <code>DicomWebRequest</code> को प्रतिक्रिया में परिवर्तित करता है, इसलिए समान सेवाएँ किसी अन्य HTTP सर्वर के पीछे या नेटवर्क के बिना परीक्षण में चल सकती हैं:</p>

<div class="codeblock" id="code">
 <h3>एक ही सर्वर ASP.NET Core के बिना - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="एक ही लाइब्रेरी से क्लाइंट और सर्वर">}}

<p>Aspose.Medical का <a href="/medical/net/dicomweb-client/">DICOMweb क्लाइंट</a> आपके सर्वर को यूनिट टेस्ट या कंसोल प्रोग्राम से परीक्षण करता है, और सर्वर द्वारा प्राप्त प्रत्येक फ़ाइल एक नियमित <code>DicomFile</code> होती है जिसे समान प्रक्रिया में एन्नोनिमाइज़, ट्रांसकोड या रेंडर किया जा सकता है।</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb डेवलपर गाइड</a> प्रत्येक सेवा और स्टोरेज इंटरफ़ेस को कवर करता है, और <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core गाइड</a> एंडपॉइंट मैपिंग और WebSockets को कवर करता है।</p>

<p>संबंधित पृष्ठ: <a href="/medical/net/dicom-networking/">DICOM नेटवर्किंग</a> उन सिस्टमों के लिए जो HTTP के बजाय DIMSE पर अध्ययन भेजते हैं, <a href="/medical/net/anonymization/">अनॉनिमाइज़ेशन</a> इनकमिंग अध्ययन का, <a href="/medical/net/dicom-transfer-syntax-conversion/">ट्रांसफ़र सिंटैक्स रूपांतरण</a>, और <a href="/medical/net/dicom-to-json/">DICOM से JSON</a> वह फॉर्मेट जिसके लिए QIDO-RS परिणाम उपयोग करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="डॉक्यूमेंटेशन" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="API रेफ़रेंसेज़" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="निःशुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="सशुल्क समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle=".NET के लिए Aspose.Medical क्यों?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता की कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
