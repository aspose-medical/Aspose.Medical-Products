---
title: C# .NET में DICOM टैग पढ़ें एवं संशोधित करें | Aspose.Medical
weight: 15000
description: C# .NET में DICOM टैग पढ़ें, जोड़ें, अपडेट करें और हटाएँ। टैग मेटाडाटा तक पहुंचें, निजी टैग के साथ काम करें, समूह द्वारा सूचीबद्ध करें, और Aspose.Medical API का उपयोग करके सीक्वेंस को नियंत्रित करें।
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# में DICOM टैग पढ़ें एवं संशोधित करें" h2="टाइप-सेफ़ API के साथ DICOM डेटा तत्वों तक पहुंचें, जोड़ें, अपडेट करें और हटाएँ। मानक टैग, निजी टैग, सीक्वेंस और पूर्ण DICOM टैग शब्दकोश के साथ काम करें।" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="टाइप-सेफ़ DICOM टैग एक्सेस">}}

<p><strong>Aspose.Medical for .NET</strong> व्यापक, टाइप-सेफ़ API प्रदान करता है जो DICOM टैगों के साथ <code>Dataset</code> क्लास के माध्यम से कार्य करता है। प्रत्येक DICOM डेटा तत्व को एक <code>Tag</code> &mdash; समूह और तत्व संख्याओं का युग्म जो विशेषता को अद्वितीय रूप से पहचानता है, द्वारा प्रतिनिधित्व किया जाता है। API जनरिक मेथड्स के साथ मजबूत टाइप्ड एक्सेस, एक्सेप्शन से बचने वाले सुरक्षित रिट्रीवल पैटर्न, तथा सिंगल वैल्यू, एरे और इंडेक्स्ड एक्सेस को समर्थन देता है।</p>

<p>सामान्य DICOM टैग <code>Tag</code> क्लास पर स्थैतिक फ़ील्ड्स के रूप में उपलब्ध होते हैं (जैसे, <code>Tag.PatientName</code>, <code>Tag.StudyInstanceUID</code>), जिससे संख्यात्मक टैग कोड याद रखने की आवश्यकता समाप्त हो जाती है। प्रत्येक टैग <code>TagMetadata</code> रखता है जिसमें उसका कीवर्ड, विवरण, वैल्यू प्रतिनिधित्व (VR), वैल्यू मल्टीप्लिसिटी, और रिटायरमेंट स्टेटस शामिल है।</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# में DICOM टैग पढ़ें">}}

<p><code>Dataset</code> क्लास आपके आवश्यकतानुसार टैग मानों को प्राप्त करने के लिए कई मेथड्स प्रदान करती है — सिंगल वैल्यू, सभी वैल्यू, इंडेक्स्ड एक्सेस, या डिफ़ॉल्ट के साथ सुरक्षित रिट्रीवल:</p>

<div class="codeblock" id="code">
 <h3>DICOM टैग मान पढ़ें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Get a single value (throws if tag is missing or multi-valued)
string patientName = dataset.GetSingleValue&lt;string&gt;(Tag.PatientName);

// Safe retrieval with a default value
string institution = dataset.GetSingleValueOrDefault&lt;string&gt;(
    Tag.InstitutionName, "Unknown");

// Get all values from a multi-valued tag
double[] pixelSpacing = dataset.GetValues&lt;double&gt;(Tag.PixelSpacing);

// Get a value by index (supports negative indexing with ^)
double firstPosition = dataset.GetValue&lt;double&gt;(Tag.ImagePositionPatient, 0);

// Check if a tag exists before accessing it
if (dataset.Contains(Tag.PatientBirthDate))
{
    var birthDate = dataset.GetSingleValue&lt;DateOnly&gt;(Tag.PatientBirthDate);
}

// TryGet pattern for safe access without exceptions
if (dataset.TryGetSingleValue&lt;string&gt;(Tag.PatientID, out var patientId))
{
    // Use patientId
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM टैग जोड़ें और अपडेट करें">}}

<p>टैग मान सेट करने के लिए <code>AddOrUpdate</code> का उपयोग करें। यदि टैग अनुपलब्ध है तो यह मेथड तत्व बनाता है, या यदि मौजूद है तो उसके मान को प्रतिस्थापित करता है। आप सिंगल वैल्यू, एरे, या पूरी तरह टाइप्ड तत्व जोड़ सकते हैं:</p>

<div class="codeblock" id="code">
 <h3>DICOM टैग जोड़ें और अपडेट करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Set a single value
dataset.AddOrUpdate(Tag.PatientName, "John Doe");
dataset.AddOrUpdate(Tag.PatientAge, new Age { Number = 42, Units = Age.Unit.Years });

// Set multiple values on a multi-valued tag
dataset.AddOrUpdate&lt;double&gt;(Tag.PixelSpacing, [0.5, 0.5]);

// Add multiple elements at once using typed element classes
dataset.AddOrUpdateRange(new IElement[]
{
    new PersonName(Tag.PatientName, ["John Doe"]),
    new LongString(Tag.PatientID, ["64AD6A3F"]),
    new Date(Tag.PatientBirthDate, [new DateOnly(2002, 10, 10)])
});

// Save the modified file
dicomFile.Save("updated.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM टैग हटाएँ">}}

<p><code>Remove</code> मेथड एकल टैग, कई टैग एक साथ, या प्रेडिकेट से मेल खाने वाले टैग को हटाने का समर्थन करता है — यह तत्वों के पूरे समूह को हटाने के लिए उपयोगी है:</p>

<div class="codeblock" id="code">
 <h3>DICOM टैग हटाएँ - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Remove a single tag
dataset.Remove(Tag.PatientAddress);

// Remove multiple tags at once
dataset.Remove(Tag.PatientName, Tag.PatientID, Tag.PatientBirthDate);

// Remove all tags in a specific group (e.g., group 0x0002 - File Meta Information)
dataset.Remove(element => element.Tag.Group == 0x0002);

dicomFile.Save("cleaned.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="समूह द्वारा टैग सूचीबद्ध करें">}}

<p>DICOM टैग समूहों में वर्गीकृत होते हैं — उदाहरण के लिए, समूह <code>0x0010</code> में रोगी जानकारी होती है, समूह <code>0x0008</code> में अध्ययन पहचान, और समूह <code>0x0028</code> में इमेज पिक्सेल विशेषताएँ। विशिष्ट समूह के सभी तत्वों पर इटरेट करने के लिए <code>EnumerateGroup</code> का उपयोग करें:</p>

<div class="codeblock" id="code">
 <h3>समूह द्वारा तत्व सूचीबद्ध करें - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Enumerate all patient-related tags (group 0x0010)
foreach (IElement element in dataset.EnumerateGroup(0x0010))
{
    Tag tag = element.Tag;
    string keyword = tag.Metadata.Keyword;
    string vr = element.ValueRepresentation.ToString();

    Console.WriteLine($"({tag.Group:X4},{tag.Element:X4}) {keyword} [{vr}]");
}

// Iterate over all elements in the dataset
foreach (IElement element in dataset)
{
    // Process each element
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="तत्व-स्तरीय डेटा हेरफेर">}}

<p>प्रत्येक DICOM तत्व में पढ़ने, जोड़ने, इंसर्ट करने, हटाने और तत्व के भीतर व्यक्तिगत मानों को बदलने के मेथड्स होते हैं। यह मल्टी-वैल्यूड टैग्स के लिए उपयोगी है जहाँ आपको वैल्यू सूची पर सूक्ष्म नियंत्रण चाहिए:</p>

<div class="codeblock" id="code">
 <h3>तत्व मानों को हेरफेर करें - C#</h3>
 <pre><code class="cs">// Create an element with multiple values
var element = new FloatingPointDouble(
    Tag.TableOfYBreakPoints, [50.1, 100.2, 150.3]);

// Access values by index (supports ^ for indexing from end)
double first = element.Get&lt;double&gt;(0);
double last = element.Get&lt;double&gt;(^1);

// Safe access with default
double safe = element.GetOrDefault&lt;double&gt;(10); // returns 0.0 if out of range

// Get all values or a range
double[] all = element.GetValues&lt;double&gt;();
double[] subset = element.GetValues&lt;double&gt;(1..3);

// Modify element values
element.Add(200.4);
element.AddRange([300.5, 400.6]);
element.Insert(2, 75.0);
element.RemoveAt(0);
element.Replace([1.0, 2.0, 3.0]);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="टैग शब्दकोश और मेटाडाटा">}}

<p><code>TagDictionary</code> क्लास ज्ञात DICOM टैगों का रेजिस्ट्री उनके मेटाडाटा (कीवर्ड, विवरण, वैल्यू प्रतिनिधित्व, वैल्यू मल्टीप्लिसिटी, और रिटायरमेंट स्टेटस) के साथ प्रदान करता है। आप कीवर्ड द्वारा टैग खोज सकते हैं या प्रोग्रामेटिक रूप से उनका मेटाडाटा निरीक्षण कर सकते हैं:</p>

<div class="codeblock" id="code">
 <h3>टैग शब्दकोश क्वेरी करें - C#</h3>
 <pre><code class="cs">// Look up a tag by its keyword
Tag? tag = TagDictionary.Default.FindByKeyword("PatientName");

// Access tag metadata from the dictionary
TagMetadata meta = TagDictionary.Default[Tag.PatientName];
Console.WriteLine($"Keyword: {meta.Keyword}");
Console.WriteLine($"Name: {meta.Name}");
Console.WriteLine($"VR: {string.Join("/", meta.ValueRepresentations)}");
Console.WriteLine($"VM: {meta.Multiplicity}");
Console.WriteLine($"Retired: {meta.Retired}");

// Access metadata directly from a tag instance
TagMetadata info = Tag.Rows.Metadata;

// Parse a tag from its string representation
Tag parsed = Tag.Parse("0010,0010");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="निजी टैग समर्थन">}}

<p>DICOM विक्रेताओं और संस्थाओं को प्रोप्राइटरी डेटा के लिए निजी टैग परिभाषित करने की अनुमति देता है। Aspose.Medical for .NET पूरी तरह से निजी टैगों को पढ़ने, लिखने और पंजीकृत करने का समर्थन करता है। आप XML फ़ाइलों से कस्टम टैग शब्दकोश लोड कर सकते हैं ताकि निजी तत्वों को उचित रूप से संभाला जा सके:</p>

<div class="codeblock" id="code">
 <h3>निजी टैग के साथ काम करें - C#</h3>
 <pre><code class="cs">// Load a custom private tag dictionary from XML
// XML format:
// &lt;dictionaries&gt;
//   &lt;dictionary creator="MY ORGANIZATION"&gt;
//     &lt;tag group="3011" element="xx00" vr="SL" vm="1"&gt;Custom Value&lt;/tag&gt;
//   &lt;/dictionary&gt;
// &lt;/dictionaries&gt;
TagDictionary.Default.Load("custom-tag-dictionary.xml");

// Check if a tag is private
Tag tag = Tag.GetOrCreate(0x3011, 0x0010);
bool isPrivate = tag.IsPrivate;

// Get a valid private tag (creates Private Creator element if needed)
Dataset dataset = dicomFile.Dataset;
Tag privateTag = dataset.GetPrivateTag(tag);

// Register a private creator programmatically
PrivateCreator creator = new("MY ORGANIZATION");
TagDictionary privateDictionary =
    TagDictionary.Default.GetPrivateDictionary(creator);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM सीक्वेंस के साथ काम करें">}}

<p>DICOM सीक्वेंस (VR प्रकार SQ) नेस्टेड डेटासेट्स रखते हैं, जो DICOM फ़ाइल के भीतर एक ट्री संरचना बनाते हैं। <code>Sequence</code> क्लास सीक्वेंस आइटम्स तक टाइप्ड एक्सेस प्रदान करती है:</p>

<div class="codeblock" id="code">
 <h3>DICOM सीक्वेंस पढ़ें और बनाएं - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("input.dcm");
Dataset dataset = dicomFile.Dataset;

// Read a sequence element
IElement seqElement = dataset.GetOrDefault(Tag.ReferencedStudySequence);
if (seqElement is Sequence sequence)
{
    // Iterate over sequence items (each item is a Dataset)
    foreach (Dataset item in sequence.Items)
    {
        string uid = item.GetSingleValueOrDefault&lt;string&gt;(
            Tag.ReferencedSOPInstanceUID, "");
    }

    // Access an item by index
    Dataset firstItem = sequence.Get(0);
}

// Create a new sequence
var items = new List&lt;Dataset&gt;
{
    new(new IElement[]
    {
        new UniqueIdentifier(Tag.ReferencedSOPClassUID, ["1.2.840.10008.5.1.4.1.1.2"]),
        new UniqueIdentifier(Tag.ReferencedSOPInstanceUID, ["1.2.3.4.5.6.7.8.9"])
    })
};
dataset.Add(new Sequence(Tag.ReferencedStudySequence, items));</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="सामान्य DICOM टैग समूह">}}

<p>निम्न तालिका में अक्सर उपयोग किए जाने वाले DICOM टैग समूह और <code>Tag</code> क्लास के माध्यम से उपलब्ध उदाहरण टैग दर्शाए गए हैं:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>समूह</th>
<th>श्रेणी</th>
<th>उदाहरण टैग</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>फ़ाइल मेटा सूचना</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>अध्ययन पहचान</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>रोगी जानकारी</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>अधिग्रहण पैरामीटर</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>इमेज पोजिशनिंग</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>इमेज पिक्सेल विशेषताएँ</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>पिक्सेल डेटा</td><td>PixelData</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="शिक्षण संसाधन" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="दस्तावेज़ीकरण" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="सोर्स कोड" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API संदर्भ" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="उत्पाद समर्थन" tabId="support" >}}
{{< blocks/products/pf/slr-element name="मुफ़्त समर्थन" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="पेड समर्थन" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ब्लॉग" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Aspose.Medical for .NET क्यों?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="ग्राहकों की सूची" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="सफलता की कहानियां" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
