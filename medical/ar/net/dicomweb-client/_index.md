---
title: عميل DICOMweb في C# .NET - QIDO-RS، WADO-RS، STOW-RS | Aspose.Medical
weight: 9100

description: ابحث، قم بتنزيل وتحميل دراسات DICOM عبر HTTP من .NET. عميل DICOMweb مكتوب بلغة C# الخالصة لـ QIDO-RS، WADO-RS وSTOW-RS يعمل مع أنظمة PACS، VNA وخدمات التصوير السحابية.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="عميل DICOMweb لـ .NET C#" h2="ابحث، قم بتنزيل وتحميل الدراسات على أي خادم DICOMweb باستخدام QIDO-RS، WADO-RS وSTOW-RS، بلغة C# المدارية دون الحاجة إلى تثبيت أي شيء على الجهاز." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM عبر HTTP، من شفرتك الخاصة">}}

<p>يُعد DICOMweb الشكل HTTP لـ DICOM. بدلاً من ربط DIMSE على المنفذ 104، تُرسل تطبيقك طلبات HTTPS عادية: QIDO-RS للبحث، WADO-RS للتنزيل، STOW-RS للتحميل. تعمل خدمات التصوير السحابي مثل خدمة DICOM في Azure Health Data Services وGoogle Cloud Healthcare API بهذه الطريقة، وكذلك الأرشيفات مثل Orthanc وdcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> يضم عميل DICOMweb منذ الإصدار 26.10. يقوم <code>DicomWebClient</code> بتغليف كل عملية قياسية في طريقة ذات نوع، وتصل النتائج ككائنات <code>Dataset</code> و<code>DicomFile</code> نفسها التي يقرأها ويكتبها باقي المكتبة. يمكن أن يكون الخادم على الطرف الآخر أي تنفيذ لـ DICOMweb؛ لا يلزم أن يُبنى باستخدام Aspose.Medical.</p>

<p>العميل هو C# مدار فوق <code>HttpClient</code>. لا توجد أداة أصلية ولا خدمة وكيل، لذا يعمل نفس الكود على Windows وLinux وفي حاوية.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الاتصال بخادم DICOMweb">}}

<p>أنشئ العميل باستخدام <code>HttpClient</code> وعنوان URL الجذر للخدمة على الخادم. يبقى المصادقة، إعدادات TLS، الوكلاء وتجميع الاتصالات على <code>HttpClient</code>، حيث يكوّنها تطبيقك مسبقاً. لا يمتلك العميل الـ <code>HttpClient</code> ولا يقوم بتصريفه.</p>

<div class="codeblock" id="code">
 <h3>إنشاء عميل DICOMweb - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>إذا كانت عملية نشر واحدة تخدم WADO-URI أو خدمة أخرى من URL مختلف، فإن <code>ServiceBaseUris</code> يتجاوز الجذر لتلك الخدمة فقط.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="بحث عن الدراسات باستخدام QIDO-RS">}}

<p>تأخذ عملية البحث معايير مطابقة باستخدام كلمة المفتاح DICOM كاسم، الحقول المراد إرجاعها، وعناصر التحكم في الصفحات. تصل النتائج كتيار من مجموعات البيانات، تُقرأ واحدةً تلو الأخرى بينما لا يزال الرد قيد التنزيل.</p>

<div class="codeblock" id="code">
 <h3>العثور على دراسات مريض - C#</h3>
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

<p>تبحث <code>SearchSeriesAsync</code> و<code>SearchInstancesAsync</code> مستوىً واحداً أسفل، عبر الأرشيف كاملًا أو داخل دراسة أو سلسلة واحدة. تحدد الخيارات نفسها أيضًا المطابقة الضبابية، مطابقة القيم الفارغة وإزاحة النتائج للصفحات.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تنزيل دراسة باستخدام WADO-RS">}}

<p>يمكن أن تحتوي دراسة على آلاف الحالات. تُعيد <code>RetrieveStudyAsync</code> تلك الحالات كتيار، لذا يُ解析 كل ملف عندما يصل جزء من الرد ويمكن حفظه قبل قراءة التالي. لا يزداد استهلاك الذاكرة مع حجم الدراسة.</p>

<div class="codeblock" id="code">
 <h3>حفظ كل حالة من دراسة - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p>يقوم <code>RetrieveSeriesAsync</code> و<code>RetrieveInstanceAsync</code> بتنزيل سلسلة واحدة أو حالة واحدة بنفس الطريقة.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الحصول على صورة مصغرة أو صورة مُصوَّرة">}}

<p>غالبًا ما يحتاج عارض أو شاشة قائمة عمل إلى صورة، وليس ملف DICOM. يقوم الخادم بتصويرها وإرسال JPEG أو PNG، لذا لا يقوم العميل بتنزيل أو فك ترميز البيانات البكسلية الأصلية.</p>

<div class="codeblock" id="code">
 <h3>حفظ صورة مصغرة JPEG لحالة - C#</h3>
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

<p>تُعيد <code>RetrieveRenderedInstanceAsync</code> و<code>RetrieveRenderedFramesAsync</code> صورة بالحجم الكامل بدلاً من ذلك. تمرر <code>DicomWebRenderOptions</code> ضوابط التصوير القياسية إلى الخادم: جودة الصورة، حجم نافذة العرض، النافذة وملف تعريف ICC.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تحميل الحالات باستخدام STOW-RS">}}

<p>يُرسل STOW-RS أي عدد من الحالات في طلب واحد. يرد الخادم بتقرير يُدرج، حالةً بعد حالة، ما تم تخزينه وما تم رفضه.</p>

<div class="codeblock" id="code">
 <h3>تحميل مجلد من ملفات DICOM - C#</h3>
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

<p>تُجيب بعض الخوادم بخطأ HTTP، مثل 409، عندما تفشل حالة واحدة أو أكثر. ثم يُطلق النداء استثناءً <code>DicomWebClientException</code>، كما يُظهر القسم التالي، ويتواجد نفس التقرير في جسم الاستجابة للستثناء.</p>

<p>يقوم <code>StoreStudyAsync</code> بتحميل داخل دراسة مسماة واحدة. تقبل الطريقتان أيضًا <code>IAsyncEnumerable&lt;DicomFile&gt;</code>، بحيث يمكن إنتاج الملفات أثناء إرسال الطلب.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="معالجة أخطاء الخادم">}}

<p>تتحقق كل عملية ذات نوع من حالة HTTP قبل الإرجاع. عندما يرفض الخادم طلبًا، يلقي النداء استثناءً <code>DicomWebClientException</code>، والذي يحمل رمز الحالة، والرؤوس، وبداية جسم الاستجابة للسجل.</p>

<div class="codeblock" id="code">
 <h3>الإبلاغ عن طلب فاشل - C#</h3>
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

<p>الطرق التي تعيد استجابة خام، مثل الصورة المصغرة أعلاه، تترك لك التحقق من الحالة عبر <code>IsSuccessStatusCode</code> أو <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ما الذي يغطيه العميل أيضًا">}}

<p>البحث، التنزيل والتحميل هي المكالمات الشائعة. يمتلك العميل مجموعة عمليات لكل من خدمات DICOMweb الأخرى أيضًا:</p>

<ul>
<li>البيانات الضخمة، بيانات البكسل والإطارات المختارة لحالة، حسب رقم الإطار القائم على الواحد، كما أرسلها الخادم.</li>
<li>WADO-URI، طريقة الاسترجاع ذات URL الواحد الأقدم التي لا يزال الكثير من العارضين والأرشيفات يستخدمونها، لملفات DICOM والصور المُصوَّرة.</li>
<li>قوائم عمل UPS-RS: إنشاء، بحث، تحديث ومطالبة بعناصر العمل، تغيير حالتها، طلب إلغاء، والاشتراك في الأحداث التي تصل عبر WebSocket.</li>
<li>قائمة عمل الوسيلة (MWL-RS)، خطوات الإجراء التي نفّذت بواسطة الوسيلة (MPPS-RS) والالتزام التخزيني.</li>
<li>حالات غير مريضية مثل لوحات الألوان وبروتوكولات التعليق، ووثيقة القدرات الخاصة بالخادم.</li>
<li><code>SendAsync</code> لأي عملية قياسية مع قيم المسار الخاصة بك، معلمات الاستعلام، الرؤوس والجسم، عندما لا تكون طريقة ذات نوع مناسبة.</li>
</ul>

<p>دليل <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">عميل DICOMweb</a> يوثّق كل عملية وخيار، و<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">مرجع عمليات العميل</a> يسرد كل طريقة.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="مكتبة واحدة من استجابة HTTP إلى بيانات البكسل">}}

<p>الملف الذي تم تنزيله باستخدام العميل هو <code>DicomFile</code> عادي. يمكن إخفاء هويته، تحويل ترميزه، تصويره أو تسلسله في نفس العملية، دون خطوة تحويل ولا مكتبة ثانية في السلسلة.</p>

<p>صفحات ذات صلة: <a href="/medical/net/dicom-networking/">شبكات DICOM</a> للأرشيفات التي تتحدث DIMSE بدلاً من HTTP، <a href="/medical/net/anonymization/">تعمية</a> قبل مغادرة الدراسة للمستشفى، <a href="/medical/net/dicom-transfer-syntax-conversion/">تحويل صيغ النقل</a> لما يخزله الخادم، و<a href="/medical/net/dicom-to-json/">DICOM إلى JSON</a> للصيغة التي تستخدمها نتائج بحث QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="الوثائق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="دليل المطور" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="مراجع API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="دعم المنتج" tabId="support" >}}
{{< blocks/products/pf/slr-element name="دعم مجاني" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="دعم مدفوع" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="مدونة" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle=" لماذا Aspose.Medical لـ .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="قائمة العملاء" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="قصص نجاح" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
