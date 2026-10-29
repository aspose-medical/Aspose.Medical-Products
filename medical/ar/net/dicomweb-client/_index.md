---
title: عميل DICOMweb في C# .NET - QIDO-RS، WADO-RS، STOW-RS | Aspose.Medical
weight: 9100

description: ابحث، حمل ورفع دراسات DICOM عبر HTTP من .NET. عميل DICOMweb بلغة C# النقية لـ QIDO-RS و WADO-RS و STOW-RS يعمل مع أنظمة PACS و VNA وخدمات التصوير السحابية.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="عميل DICOMweb لـ .NET C#" h2="ابحث، حمّل وارفَع الدراسات على أي خادم DICOMweb باستخدام QIDO-RS و WADO-RS و STOW-RS، بلغة C# المُدارة دون الحاجة لتثبيت أي شيء على الجهاز." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM عبر HTTP، من شفرتك الخاصة">}}

<p>DICOMweb هو الشكل HTTP من DICOM. بدلاً من ارتباط DIMSE على المنفذ 104، يرسل تطبيقك طلبات HTTPS عادية: QIDO-RS للبحث، WADO-RS للتحميل، STOW-RS للرفع. تعمل خدمات التصوير السحابية مثل خدمة DICOM في Azure Health Data Services وGoogle Cloud Healthcare API بهذه الطريقة، وكذلك الأرشيفات مثل Orthanc و dcm4chee.</p>

<p><strong>Aspose.Medical لـ .NET</strong> يتضمن عميل DICOMweb منذ الإصدار 26.10. <code>DicomWebClient</code> يلتف حول كل معاملة معيارية في طريقة ذات نوعية، وتصل النتائج ككائنات <code>Dataset</code> و <code>DicomFile</code> نفسها التي يقرأها ويكتبها باقي المكتبة. يمكن أن يكون الخادم على الجانب الآخر أي تنفيذ DICOMweb؛ لا يلزم أن يُبنى باستخدام Aspose.Medical.</p>

<p>العميل هو C# مُدار فوق <code>HttpClient</code>. لا توجد مجموعة أدوات أصلية ولا خدمة وكيل، لذا يعمل نفس الكود على Windows وLinux وفي حاوية.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الاتصال بخادم DICOMweb">}}

<p>إنشاء العميل من <code>HttpClient</code> وعنوان URL الجذر للخدمة على الخادم. تبقى المصادقة وإعدادات TLS والوكلاء وتجميع الاتصالات على <code>HttpClient</code>، حيث يكوّنها تطبيقك بالفعل. العميل لا يملك <code>HttpClient</code> ولا يقوم بتحريره.</p>

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

<p>إذا كان نشر واحد يقدم WADO-URI أو خدمة أخرى من عنوان URL مختلف، فإن <code>ServiceBaseUris</code> يتجاوز الجذر لتلك الخدمة فقط.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="بحث عن الدراسات باستخدام QIDO-RS">}}

<p>تأخذ عملية البحث معايير مطابقة باستخدام كلمة المفتاح DICOM كاسم، والحقول المراد إرجاعها، وعناصر التحكم في الصفحات. تصل النتائج كتيار من مجموعات البيانات، تُقرأ واحدة تلو الأخرى بينما لا يزال الاستجابة قيد التحميل.</p>

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

<p><code>SearchSeriesAsync</code> و <code>SearchInstancesAsync</code> يبحثان مستوىً أدنى، عبر الأرشيف بأكمله أو داخل دراسة أو سلسلة واحدة. الخيارات نفسها أيضاً تُحدد المطابقة غير الدقيقة، ومطابقة القيم الفارغة وإزاحة النتائج للصفحات.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="تنزيل دراسة باستخدام WADO-RS">}}

<p>يمكن للدراسة أن تحتوي على آلاف الحالات. <code>RetrieveStudyAsync</code> تُعيدها كتيار، بحيث يُتحلل كل ملف بمجرد وصول جزئه من الاستجابة ويمكن حفظه قبل قراءة التالي. لا يزداد استهلاك الذاكرة مع حجم الدراسة.</p>

<div class="codeblock" id="code">
 <h3>حفظ كل حالة من الدراسة - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> و <code>RetrieveInstanceAsync</code> يحمّلان سلسلة واحدة أو حالة واحدة بنفس الطريقة.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="الحصول على صورة مصغرة أو صورة مُعالجة">}}

<p>غالبًا ما يحتاج عارض أو شاشة قائمة الأعمال إلى صورة، وليس ملف DICOM. يقوم الخادم بمعالجتها وإرسال JPEG أو PNG، وبالتالي لا يقوم العميل بتحميل أو فك ترميز بيانات البكسل الأصلية.</p>

<div class="codeblock" id="code">
 <h3>حفظ الصورة المصغرة JPEG لحالة - C#</h3>
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

<p><code>RetrieveRenderedInstanceAsync</code> و <code>RetrieveRenderedFramesAsync</code> تُعيد صورة بالحجم الكامل بدلاً من ذلك. <code>DicomWebRenderOptions</code> يمرّر عناصر التحكم القياسية في التصيير إلى الخادم: جودة الصورة، حجم نافذة العرض، النوافذ وملف تعريف ICC.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="رفع الحالات باستخدام STOW-RS">}}

<p>يقوم STOW-RS بإرسال أي عدد من الحالات في طلب واحد. يرد الخادم بتقرير يسرد، حالةً بعد حالة، ما تم تخزينه وما تم رفضه.</p>

<div class="codeblock" id="code">
 <h3>رفع مجلد من ملفات DICOM - C#</h3>
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

<p>بعض الخوادم يردون بخطأ HTTP، مثل 409، عندما تفشل حالة أو أكثر. ثم يرمى النداء <code>DicomWebClientException</code>، كما توضح القسم التالي، ويتواجد نفس التقرير في جسم الاستجابة للاستثناء.</p>

<p><code>StoreStudyAsync</code> يرفع إلى دراسة مسماة واحدة. كلا الطريقتين تقبلان أيضًا <code>IAsyncEnumerable&lt;DicomFile&gt;</code>، بحيث يمكن إنتاج الملفات أثناء إرسال الطلب.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="معالجة أخطاء الخادم">}}

<p>كل عملية ذات نوعية تتحقق من حالة HTTP قبل الإرجاع. عندما يرفض الخادم طلبًا، يرمى النداء <code>DicomWebClientException</code>، الذي يحمل رمز الحالة، الرؤوس، وبداية جسم الاستجابة للسجل.</p>

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

<p>الطرق التي تُعيد استجابة خام، مثل الصورة المصغرة أعلاه، تترك فحص الحالة لك عبر <code>IsSuccessStatusCode</code> أو <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ما الذي يغطيه العميل أيضًا">}}

<p>البحث، التحميل والرفع هي المكالمات الشائعة. يحتوي العميل أيضًا على مجموعة عمليات لكل من خدمات DICOMweb الأخرى:</p>

<ul>
<li>البيانات الضخمة، بيانات البكسل والإطارات المختارة لحالة، برقم إطار يبدأ من واحد، كما أرسلها الخادم.</li>
<li>WADO-URI، طريقة الاسترجاع ذات URL الواحد الأقدم التي لا يزال العديد من العارضين والأرشيفات يستخدمونها، لملفات DICOM والصور المُعالجة.</li>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> قوائم العمل: إنشاء، بحث، تحديث والمطالبة بمهام العمل، تغيير حالتها، طلب إلغاء، والاشتراك في الأحداث التي تصل عبر WebSocket.</li>
<li>ال<a href="/medical/net/modality-worklist/">قائمة عمل الفعالية</a> (MWL-RS)، خطوات الإجراء التي تنفذها الفعالية (MPPS-RS) والالتزام بالتخزين.</li>
<li>حالات غير مريض مثل لوحات الألوان وبروتوكولات التعليق، ووثيقة القدرات الخاصة بالخادم.</li>
<li><code>SendAsync</code> لأي معاملة معيارية مع قيم المسار الخاصة بك، معلمات الاستعلام، الرؤوس والجسم، عندما لا تكون الطريقة ذات النوعية مناسبة.</li>
</ul>

<p>دليل <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">عميل DICOMweb</a> يوثّق كل عملية وخيار، و<a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">مرجع عمليات العميل</a> يسرد كل طريقة.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="مكتبة واحدة من استجابة HTTP إلى بيانات البكسل">}}

<p>الملف الذي يتم تنزيله باستخدام العميل هو <code>DicomFile</code> عادي. يمكن إخفاء هويته، تحويل ترميزه، تصييره أو تسلسله في نفس العملية، دون خطوة تحويل وبدون مكتبة ثانية في السلسلة.</p>

<p>الصفحات ذات الصلة: <a href="/medical/net/dicomweb-server/">خادم DICOMweb</a> للجانب الآخر من البروتوكول في خدمة ASP.NET Core الخاصة بك، <a href="/medical/net/dicom-networking/">شبكة DICOM</a> للأرشيفات التي تتواصل عبر DIMSE بدلًا من HTTP، <a href="/medical/net/anonymization/">إخفاء الهوية</a> قبل مغادرة الدراسة للمستشفى، <a href="/medical/net/dicom-transfer-syntax-conversion/">تحويل صيغة النقل</a> لما يخزنه الخادم، و<a href="/medical/net/dicom-to-json/">DICOM إلى JSON</a> لتنسيق نتائج البحث QIDO-RS.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="موارد التعلم" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="التوثيق" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="دليل المطور" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="مراجع API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="دعم المنتج" tabId="support" >}}
{{< blocks/products/pf/slr-element name="دعم مجاني" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="دعم مدفوع" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="مدونة" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="لماذا Aspose.Medical لـ .NET؟" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="قائمة العملاء" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="قصص نجاح" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
