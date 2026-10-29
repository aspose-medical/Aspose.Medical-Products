---
title: C# .NET में DICOMweb सर्वर - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: C# में अपना DICOMweb सर्वर या PACS बनाएं। Aspose.Medical ASP.NET Core पर QIDO-RS, WADO-RS, STOW-RS और वर्कलिस्ट सेवाओं को लागू करता है; आप स्टोरेज कनेक्ट करते हैं।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# के लिए DICOMweb सर्वर" h2="एक ASP.NET Core सेवा में QIDO-RS, WADO-RS और STOW-RS एंडपॉइंट जोड़ें। लाइब्रेरी प्रोटोकॉल को पहचानती है; आपका कोड तय करता है कि अध्ययन कहाँ संग्रहित होते हैं।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="C# में आपका अपना DICOMweb सर्वर">}}

<p>A PACS, a VNA, a cloud archive या एक रिसर्च प्लेटफ़ॉर्म को DICOMweb अनुरोधों का उत्तर देना पड़ता है: QIDO-RS खोज, WADO-RS डाउनलोड और STOW-RS अपलोड, और अक्सर वर्कलिस्ट सेवाएँ भी। प्रत्येक का अपना रूट, मीडिया प्रकार, मल्टीपार्ट बॉडी, मिलान नियम और स्टेटस कोड होते हैं।</p>

<p><strong>Aspose.Medical for .NET</strong> ने संस्करण 26.10 से इस प्रोटोकॉल को कार्यान्वित किया है। यह हर अनुरोध को पढ़ता है, उसे मानक से जांचता है, DICOM नियमों द्वारा खोज कुंजियों को मिलाता है, परिणामों को पेज करता है, ट्रांसफर सिंटैक्स को परिवर्तित करता है और प्रतिक्रिया लिखता है। आपका कोड वही करता है जो केवल आप ही कर सकते हैं: एक इंस्टेंस को संग्रहीत करना, खोज के लिए उम्मीदवार खोजना और संग्रहीत फ़ाइलें लौटाना। व्यूअर्स, मोडैलिटीज़ और अन्य आर्काइव्स तब आपके सर्वर के साथ किसी भी अन्य DICOMweb सर्वर की तरह काम करते हैं, और उसी लाइब्रेरी के <a href="/medical/net/dicomweb-client/">DICOMweb client</a> भी ऐसा ही करता है।</p>

<p>सर्वर C# में प्रबंधित है। यह जहाँ भी ASP.NET Core चलता है, वहाँ चलता है: Windows पर, Linux पर और कंटेनर में।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ASP.NET Core में एक पूर्ण सर्वर">}}

<p><code>Aspose.Medical.Dicom.Web.AspNetCore</code> पैकेज स्थापित करें। यह Aspose.Medical को साथ लाता है और DICOMweb सेवाओं को डिपेंडेंसी इन्जेक्शन और एंडपॉइंट रूटिंग से जोड़ता है। यह <code>Program.cs</code> एक कार्यशील Studies सर्वर है; जो दो क्लासेज रजिस्टर करता है, वे नीचे दिखाए गए हैं।</p>

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

<p>अब हर Studies रूट <code>/dicomweb</code> के तहत उत्तर देता है: खोज और अपलोड के लिए <code>/dicomweb/studies</code>, पुनः प्राप्ति के लिए <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code>, और मेटाडेटा, फ्रेम, बल्क डेटा और रेंडर्ड इमेजेज के रूट्स। सेवा रूट पर क्षमताओं दस्तावेज़ ठीक वही लेनदेन सूचीबद्ध करता है जो सर्वर समर्थन करता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="अपने स्टोरेज को कनेक्ट करें">}}

<p>Studies सेवा को एक इंटरफ़ेस की आवश्यकता होती है जिसमें तीन मेथड्स होते हैं: एक इंस्टेंस को स्टोर करना, संग्रहीत फ़ाइलें लौटाना, और खोज उम्मीदवार लौटाना। यह इम्प्लीमेंटेशन प्रत्येक इंस्टेंस को अध्ययन और सीरीज़ के अनुसार एक फ़ोल्डर में फ़ाइल के रूप में रखता है।</p>

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

<p>लाइब्रेरी प्रत्येक अनुरोध पाथ और प्रत्येक अपलोडेड इंस्टेंस में UIDs की जाँच करती है, फिर वह प्रोवाइडर को कॉल करती है। वैध UIDs के बिना कोई इंस्टेंस विफल के रूप में रिपोर्ट किया जाता है और कभी भी <code>StoreAsync</code> तक नहीं पहुँचता।</p>

<p>सर्वर को आज़माने के लिए एक फ़ोल्डर पर्याप्त है। एक प्रोडक्शन आर्काइव वही तीन मेथड्स रखता है और <code>StoreAsync</code> तथा <code>RetrieveAsync</code> के पीछे एक ऑब्जेक्ट स्टोर तथा <code>FindCandidatesAsync</code> के पीछे एक डेटाबेस इंडेक्स रखता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="खोज: लाइब्रेरी DICOM मिलान नियम लागू करती है">}}

<p>QIDO-RS मिलान केवल स्ट्रिंग तुलना से अधिक है: वाइल्डकार्ड, तिथि और समय रेंज, UIDs की सूचियाँ, फज़ी व्यक्ति नाम, खाली मान और क्रमों के भीतर कुंजियाँ। प्रोवाइडर इनमें से कोई भी लागू नहीं करता। यह उम्मीदवारों को स्थिर क्रम में लौटाता है, और मिलान से अधिक भी लौट सकता है। लाइब्रेरी प्रत्येक उम्मीदवार पर सटीक DICOM मिलान लागू करती है, अनुरोधित फ़ील्ड्स को रखती है, और <code>offset</code> तथा <code>limit</code> लागू करती है। एक प्रतिक्रिया डिफ़ॉल्ट रूप से अधिकतम 1000 परिणाम रखती है, और <code>DicomWebStandardServiceOptions</code> उस सीमा को बदलता है।</p>

<p>बड़े आर्काइव में खोज को तेज रखने के लिए चयनात्मक कुंजियों को डेटाबेस में पास करें। <code>search.Plan.Filter</code> संयोजित खोज कुंजियों को रखता है, प्रत्येक के साथ एट्रिब्यूट, मिलान प्रकार और मान होते हैं:</p>

<div class="codeblock" id="code">
 <h3>खोज से सटीक Patient ID पढ़ें - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>ऐसे निर्मित डेटाबेस क्वेरी अतिरिक्त पंक्तियाँ वापस कर सकती है, उदाहरण के लिए जब यह ऐसी कुंजी को अनदेखा करता है जिसे वह अनुवाद नहीं कर सकता। लाइब्रेरी में अंतिम मिलान उन्हें हटा देता है। इसे वास्तविक मिलानों को नहीं हटाना चाहिए, क्योंकि लाइब्रेरी उन्हें वापस जोड़ नहीं सकती।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="स्टोर: प्रत्येक इंस्टेंस के लिए एक परिणाम">}}

<p>एक STOW-RS अनुरोध सैंकड़ों इंस्टेंस ले जा सकता है। लाइब्रेरी मल्टीपार्ट बॉडी को पार्स करती है और क्रमशः प्रत्येक इंस्टेंस के लिए <code>StoreAsync</code> को कॉल करती है, इसलिए पूरी अपलोड एक बार में मेमोरी में नहीं रखी जाती। प्रत्येक इंस्टेंस के लिए प्रोवाइडर <code>Stored</code>, परिवर्तन के साथ संग्रहित इंस्टेंस के लिए <code>Warning</code>, या वैकल्पिक DICOM कारण कोड के साथ <code>Failed</code> लौटाता है।</p>

<p>लाइब्रेरी इन परिणामों से STOW-RS रिपोर्ट बनाती है और स्थिति चुनती है: जब हर इंस्टेंस संग्रहीत हो तो 200, जब कुछ के पास चेतावनी या विफलता हो तो 202, जब सभी विफल हों तो 409। एक नामित अध्ययन के लिए अनुरोध में जो इंस्टेंस किसी अन्य अध्ययन से संबंधित हैं, उन्हें प्रोवाइडर को कॉल करने से पहले ही अस्वीकृत किया जाता है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="रीट्रीव: ट्रांसफर सिंटैक्स, फ्रेम्स और रेंडर्ड इमेजेज">}}

<p>प्रोवाइडर संग्रहीत फ़ाइलें लौटाता है, और लाइब्रेरी उन्हें क्लाइंट द्वारा मांगी गई फ़ॉर्म में डिलीवर करती है। एक WADO-RS क्लाइंट <code>Accept</code> हेडर में ट्रांसफर सिंटैक्स माँग सकता है, और लाइब्रेरी प्रत्येक फ़ाइल को भेजने से पहले अपने कोडेक्स, HTJ2K और JPEG XL सहित, परिवर्तित करती है। DICOM JSON या XML में मेटाडेटा, बल्क डेटा और सिंगल फ्रेम्स भी वही प्रोवाइडर से आते हैं, अतिरिक्त कोड की आवश्यकता नहीं।</p>

<p>रेंडर्ड इमेजेज़ के लिए एक अतिरिक्त क्लास की आवश्यकता होती है, क्योंकि केवल आप तय करते हैं कि सर्वर कौन सी इमेजेज़ बना सकता है। यह क्लास रेंडर्ड इंस्टेंस, रेंडर्ड फ्रेम्स और थंबनेल्स के लिए JPEG प्रदान करती है, और लाइब्रेरी इन्हें रेंडर करती है:</p>

<div class="codeblock" id="code">
 <h3>रेंडर्ड JPEG इमेजेज़ और थंबनेल्स प्रदान करें - C#</h3>
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

<p>निर्मित रेंडरर एक सिंगल-फ़्रेम इंस्टेंस या अनुरोध द्वारा चयनित फ्रेम्स से JPEG बनाता है। एक संपूर्ण रेंडर्ड मल्टी-फ़्रेम इंस्टेंस, अन्य इमेज फ़ॉर्मेट, और viewport, window तथा ICC प्रोफ़ाइल पैरामीटरों के लिए इस क्लास में आपका स्वयं का <code>RetrieveAsync</code> आवश्यक है।</p>

<p>थंबनेल्स को रोगी गोपनीयता के प्रति सावधानी से संभाला जाता है। निर्मित रेंडरर केवल उन इंस्टेंस से थंबनेल बनाता है जिनके Burned In Annotation और Recognizable Visual Features एट्रिब्यूट दोनों <code>NO</code> हैं, जिसका अर्थ है कि इमेज स्वयं यह घोषित करती है कि उसमें कोई रोगी डेटा नहीं दिखता। कई इमेजेज़ इन एट्रिब्यूट्स को नहीं रखतीं, और उनके लिए सर्वर 406 उत्तर देता है। यदि आपके आर्काइव को ऐसी इमेजेज़ के थंबनेल चाहिए, तो उन्हें अपने स्वयंके <code>RetrieveAsync</code> में बनाएं और सुनिश्चित करें कि वे सभी रोगी डेटा को छुपाते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="वर्कलिस्ट्स, प्रक्रियात्मक चरण और अन्य सेवाएँ">}}

<p>अन्य DICOMweb सेवाएँ भी वही मॉडल अपनाती हैं: सेवा रजिस्टर करें, उसके स्टोरेज इंटरफ़ेस को लागू करें।</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> वर्कलिस्ट्स <code>AddStandardWorklistService</code> के साथ: वर्कआइटम्स बनाएं, खोजें और अपडेट करें तथा उनकी स्थिति बदलें, स्थिति नियम लाइब्रेरी द्वारा जाँचे जाते हैं और इवेंट नोटिफिकेशन WebSocket के माध्यम से भेजे जाते हैं।</li>
<li><a href="/medical/net/modality-worklist/">modality worklist</a> (MWL-RS) <code>AddStandardModalityWorklistService</code> के साथ, ताकि मोडैलिटीज़ आपके RIS के नियोजित प्रक्रियात्मक चरणों को क्वेरी कर सकें।</li>
<li>Modality performed procedure steps (MPPS-RS), स्टोरेज कमिटमेंट और रोगी‑विहीन इंस्टेंस जैसे कलर पैलेट्स और हैंगिंग प्रोटोकॉल।</li>
<li>पुराने व्यूअर्स के लिए WADO-URI पुनः प्राप्ति, जो Studies सेवा द्वारा सर्व की जाती है।</li>
</ul>

<p>जब किसी लेनदेन का मानक व्यवहार उपयुक्त नहीं हो, तो <code>AddHandler</code> के साथ अपना स्वयंका हैंडलर रजिस्टर करें। लाइब्रेरी अभी भी अनुरोध को रूट और जांचती है, और अन्य लेनदेन अपने मानक हैंडलर रखे रहते हैं।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="सुरक्षा और होस्टिंग आपके नियंत्रण में रहती हैं">}}

<p>DICOMweb एंडपॉइंट सामान्य ASP.NET Core एंडपॉइंट हैं। प्रमाणीकरण, प्राधिकरण, CORS, TLS, फ़ॉरवर्डेड हेडर्स और अनुरोध आकार सीमाएँ आपके सेवा के अन्य हिस्सों की तरह ही कॉन्फ़िगर की जाती हैं:</p>

<div class="codeblock" id="code">
 <h3>प्रमाणित उपयोगकर्ता की आवश्यकता - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>प्रोटोकॉल भाग ASP.NET Core पर निर्भर नहीं करता। <code>DicomWebApplication</code> एक <code>DicomWebRequest</code> को प्रतिक्रिया में बदलता है, इसलिए वही सेवाएँ किसी अन्य HTTP सर्वर के पीछे या नेटवर्क के बिना परीक्षण में चल सकती हैं:</p>

<div class="codeblock" id="code">
 <h3>ASP.NET Core के बिना वही सर्वर - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="एक ही लाइब्रेरी से क्लाइंट और सर्वर">}}

<p>Aspose.Medical का <a href="/medical/net/dicomweb-client/">DICOMweb क्लाइंट</a> आपके सर्वर को यूनिट टेस्ट या कंसोल प्रोग्राम से परीक्षण करता है, और सर्वर द्वारा प्राप्त हर फ़ाइल एक सामान्य <code>DicomFile</code> होती है जिसे उसी प्रक्रिया में अनामित, ट्रांसकोड या रेंडर किया जा सकता है।</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb डेवलपर गाइड</a> प्रत्येक सेवा और स्टोरेज इंटरफ़ेस को कवर करता है, और <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core गाइड</a> एंडपॉइंट मैपिंग और WebSockets को कवर करता है।</p>

<p>संबंधित पृष्ठ: <a href="/medical/net/dicom-networking/">DICOM नेटवर्किंग</a> उन सिस्टमों के लिए जो HTTP के बजाय DIMSE के द्वारा अध्ययन भेजते हैं, <a href="/medical/net/anonymization/">आने वाले अध्ययनों का अनामिकीकरण</a>, <a href="/medical/net/dicom-transfer-syntax-conversion/">ट्रांसफर सिंटैक्स परिवर्तन</a>, और <a href="/medical/net/dicom-to-json/">DICOM से JSON</a> QIDO-RS परिणामों के फ़ॉर्मेट के लिये।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="डेवलपर गाइड" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="API रेफ़रेंस" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="निःशुल्क समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="भुगतान‑युक्त समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="क्यों Aspose.Medical .NET के लिए?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता की कहानियाँ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
