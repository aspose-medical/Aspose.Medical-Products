---
title: เซิร์ฟเวอร์ DICOMweb ด้วย C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: สร้างเซิร์ฟเวอร์ DICOMweb หรือ PACS ของคุณเองด้วย C# Aspose.Medical implements QIDO-RS, WADO-RS, STOW-RS และบริการ worklist บน ASP.NET Core; คุณทำการเชื่อมต่อที่จัดเก็บ
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="เซิร์ฟเวอร์ DICOMweb สำหรับ .NET C#" h2="เพิ่มจุดสิ้นสุด QIDO-RS, WADO-RS และ STOW-RS ไปยังบริการ ASP.NET Core. ไลบรารีสื่อสารตามโปรโตคอล; โค้ดของคุณกำหนดว่าการศึกษา (studies) จะถูกจัดเก็บที่ไหน." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="เซิร์ฟเวอร์ DICOMweb ของคุณเองด้วย C#">}}

<p>PACS, VNA, ที่เก็บข้อมูลคลาวด์ หรือแพลตฟอร์มวิจัยต้องตอบสนองต่อคำขอ DICOMweb: การค้นหา QIDO-RS, การดาวน์โหลด WADO-RS และการอัปโหลด STOW-RS, และบ่อยครั้งรวมถึงบริการ worklist ด้วย. แต่ละบริการมีเส้นทาง, ประเภทสื่อ, เนื้อหา multipart, หลักการจับคู่และรหัสสถานะของตนเอง.</p>

<p><strong>Aspose.Medical for .NET</strong> ดำเนินการด้านโปรโตคอลนั้นตั้งแต่เวอร์ชัน 26.10. มันอ่านคำขอทุกคำ, ตรวจสอบกับมาตรฐาน, จับคู่คีย์การค้นหาตามกฎ DICOM, แบ่งหน้าผลลัพธ์, แปลง transfer syntaxes และเขียนการตอบกลับ. โค้ดของคุณทำสิ่งที่คุณเท่านั้นทำได้: จัดเก็บ instance, ค้นหาผู้สมัครสำหรับการค้นหาและคืนไฟล์ที่จัดเก็บไว้. Viewer, modality และคลังอื่น ๆ จะทำงานกับเซิร์ฟเวอร์ของคุณเช่นเดียวกับเซิร์ฟเวอร์ DICOMweb ใด ๆ, และเช่นเดียวกับ <a href="/medical/net/dicomweb-client/">DICOMweb client</a> ของไลบรารีเดียวกัน.</p>

<p>เซิร์ฟเวอร์นี้จัดการด้วย C#. มันทำงานได้ทุกที่ที่ ASP.NET Core ทำงาน: บน Windows, Linuxและในคอนเทนเนอร์.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เซิร์ฟเวอร์ครบรูปแบบใน ASP.NET Core">}}

<p>ติดตั้งแพ็กเกจ <code>Aspose.Medical.Dicom.Web.AspNetCore</code>. แพ็กเกจนี้จะนำ Aspose.Medical มาและเชื่อมต่อบริการ DICOMweb กับ dependency injection และ endpoint routing. <code>Program.cs</code> นี้เป็นเซิร์ฟเวอร์ Studies ที่ทำงานได้; สองคลาสที่ลงทะเบียนจะแสดงด้านล่าง.</p>

<div class="codeblock" id="code">
 <h3>Program.cs ของเซิร์ฟเวอร์ DICOMweb - C#</h3>
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

<p>ทุกเส้นทาง Studies ตอนนี้ตอบภายใต้ <code>/dicomweb</code>: <code>/dicomweb/studies</code> สำหรับการค้นหาและอัปโหลด, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> สำหรับการดึงข้อมูล, รวมถึงเส้นทางสำหรับ metadata, frames, bulk data และภาพที่เรนเดอร์. เอกสาร capabilities ที่รูทของบริการระบุรายการ transaction ที่เซิร์ฟเวอร์รองรับอย่างแม่นยำ.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เชื่อมต่อที่จัดเก็บข้อมูลของคุณ">}}

<p>บริการ Studies ต้องการอินเทอร์เฟซหนึ่งตัวที่มีสามเมธอด: จัดเก็บ instance, คืนไฟล์ที่จัดเก็บ, และคืนผู้สมัครการค้นหา. การทำงานนี้เก็บแต่ละ instance เป็นไฟล์ในโฟลเดอร์ตาม study และ series.</p>

<div class="codeblock" id="code">
 <h3>คลังโฟลเดอร์เบื้องหลัง QIDO-RS, WADO-RS และ STOW-RS - C#</h3>
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

<p>ไลบรารีตรวจสอบ UID ในแต่ละเส้นทางคำขอและในแต่ละ instance ที่อัปโหลดก่อนจะเรียกผู้ให้บริการ. instance ที่ไม่มี UID ที่ถูกต้องจะถูกบันทึกเป็นความล้มเหลวและไม่ถึง <code>StoreAsync</code>.</p>

<p>โฟลเดอร์เพียงอย่างเดียวก็เพียงพอสำหรับทดลองเซิร์ฟเวอร์. คลังข้อมูลการผลิตจะใช้สามเมธอดเดียวกันและวาง object store ไว้หลัง <code>StoreAsync</code> และ <code>RetrieveAsync</code> พร้อมดัชนีฐานข้อมูลหลัง <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การค้นหา: ไลบรารีใช้กฎการจับคู่ DICOM">}}

<p>การจับคู่ QIDO-RS มีมากกว่าการเปรียบเทียบสตริง: wildcard, ช่วงเวลาและวันที่, รายการ UID, ชื่อบุคคลแบบ fuzzy, ค่าเปล่าและคีย์ภายในลำดับ. ผู้ให้บริการไม่ได้ดำเนินการส่วนใดส่วนหนึ่ง. มันคืนผู้สมัครในลำดับที่เสถียร และอาจคืนมากกว่าที่จับคู่. ไลบรารีใช้การจับคู่ DICOM อย่างแม่นยำกับแต่ละผู้สมัคร, เก็บฟิลด์ที่ร้องขอ, และใช้ <code>offset</code> และ <code>limit</code>. การตอบสนองหนึ่งรายการโดยค่าเริ่มต้นมีผลลัพธ์สูงสุด 1000 รายการ, และ <code>DicomWebStandardServiceOptions</code> สามารถเปลี่ยนขีดจำกัดนั้น.</p>

<p>เพื่อให้การค้นหาเร็วในคลังข้อมูลขนาดใหญ่, ให้ส่งคีย์ที่เลือกไปยังฐานข้อมูล. <code>search.Plan.Filter</code> เก็บคีย์การค้นหาที่คอมไพล์แล้ว, พร้อมด้วยแอตทริบิวต์, ประเภทการจับคู่และค่าของแต่ละคีย์:</p>

<div class="codeblock" id="code">
 <h3>อ่าน Patient ID ที่แน่นอนจากการค้นหา - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>การคิวรีฐานข้อมูลที่สร้างแบบนี้อาจคืนแถวเพิ่ม, เช่นเมื่อมันละเว้นคีย์ที่ไม่สามารถแปลได้. การจับคู่ขั้นสุดท้ายในไลบรารีจะลบแถวนั้นออก. มันไม่ควรทิ้งการจับคู่ที่แท้จริง, เนื่องจากไลบรารีไม่สามารถเพิ่มกลับได้.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การจัดเก็บ: ผลลัพธ์สำหรับแต่ละ instance">}}

<p>คำขอ STOW-RS สามารถบรรจุ instance ได้หลายร้อย. ไลบรารีทำการพาร์ส multipart body และเรียก <code>StoreAsync</code> สำหรับแต่ละ instance ทีละรายการ, ดังนั้นการอัปโหลดทั้งหมดจะไม่ถูกเก็บไว้ในหน่วยความจำพร้อมกัน. สำหรับแต่ละ instance ผู้ให้บริการจะคืนค่า <code>Stored</code>, <code>Warning</code> หาก instance ถูกจัดเก็บพร้อมการเปลี่ยนแปลง, หรือ <code>Failed</code> พร้อมรหัสเหตุผล DICOM ที่เป็นตัวเลือก.</p>

<p>ไลบรารีสร้างรายงาน STOW-RS จากผลลัพธ์เหล่านี้และเลือกสถานะ: 200 เมื่อทุก instance ถูกจัดเก็บ, 202 เมื่อบางส่วนมีคำเตือนหรือการล้มเหลว, 409 เมื่อทั้งหมดล้มเหลว. Instance ในคำขอสำหรับ study ที่ระบุชื่อหนึ่งแต่เป็นของ study อื่นจะถูกปฏิเสธก่อนที่ผู้ให้บริการจะถูกเรียก.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การดึงข้อมูล: transfer syntaxes, frames และภาพที่เรนเดอร์">}}

<p>ผู้ให้บริการคืนไฟล์ที่จัดเก็บ, และไลบรารีส่งมอบในรูปแบบที่ client ขอ. client WADO-RS สามารถขอ transfer syntax ผ่าน header <code>Accept</code>, และไลบรารีจะแปลงไฟล์แต่ละไฟล์ก่อนส่ง, พร้อม codecs ของตัวเอง, รวมถึง HTJ2K และ JPEG XL. Metadata ในรูปแบบ DICOM JSON หรือ XML, bulk data และ single frames มาจากผู้ให้บริการเดียวกัน, ไม่ต้องเขียนโค้ดเพิ่มเติม.</p>

<p>ภาพที่เรนเดอร์ต้องการคลาสเพิ่มเติมหนึ่งคลาส, เนื่องจากคุณเป็นผู้กำหนดว่าภาพใดที่เซิร์ฟเวอร์อาจสร้างได้. คลาสนี้ให้ JPEG สำหรับ instances ที่เรนเดอร์, frames ที่เรนเดอร์และ thumbnails, และไลบรารีทำการเรนเดอร์พวกมัน:</p>

<div class="codeblock" id="code">
 <h3>เสนอภาพ JPEG ที่เรนเดอร์และ thumbnails - C#</h3>
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

<p>renderer ในตัวสร้าง JPEG จาก instance ที่มี single-frame หรือจาก frames ที่คำขอเลือก. การเรนเดอร์ multi-frame instance ทั้งหมด, ฟอร์แมตภาพอื่น, และพารามิเตอร์ viewport, window และ ICC profile ต้องการ <code>RetrieveAsync</code> ของคุณในคลาสนี้.</p>

<p>Thumbnails ถูกจัดการอย่างระมัดระวังเพื่อความเป็นส่วนตัวของผู้ป่วย. renderer ในตัวจะสร้าง thumbnail เฉพาะจาก instance ที่แอตทริบิวต์ Burned In Annotation และ Recognizable Visual Features ทั้งสองเป็น <code>NO</code>, หมายความว่าภาพนั้นระบุว่าไม่มีข้อมูลผู้ป่วย. ภาพหลายรูปไม่มีแอตทริบิวต์เหล่านี้, และสำหรับพวกมันเซิร์ฟเวอร์จะตอบ 406. หากคลังของคุณต้องการ thumbnails ของภาพดังกล่าว, ให้สร้างใน <code>RetrieveAsync</code> ของคุณและตรวจสอบให้แน่ใจว่าปกปิดข้อมูลผู้ป่วยทั้งหมด.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklist, ขั้นตอนการทำกระบวนการและบริการอื่น ๆ">}}

<p>บริการ DICOMweb อื่น ๆ ใช้โมเดลเดียวกัน: ลงทะเบียนบริการ, ทำการ implement อินเทอร์เฟซการจัดเก็บของมัน.</p>

<ul>
<li>UPS-RS worklist ด้วย <code>AddStandardWorklistService</code>: สร้าง, ค้นหาและอัปเดต workitem และเปลี่ยนสถานะ, โดยกฎการเปลี่ยนสถานะจะตรวจสอบโดยไลบรารีและส่งการแจ้งเตือนเหตุการณ์ผ่าน WebSocket.</li>
<li>Modality worklist (MWL-RS) ด้วย <code>AddStandardModalityWorklistService</code>, เพื่อให้ modality สามารถสอบถามขั้นตอนการทำกระบวนการที่กำหนดไว้ของ RIS ของคุณ.</li>
<li>Modality performed procedure steps (MPPS-RS), storage commitment และ instance ที่ไม่ใช่ผู้ป่วยเช่น color palette และ hanging protocol.</li>
<li>WADO-URI retrieval สำหรับ viewer รุ่นเก่า, ให้บริการโดย Studies service.</li>
</ul>

<p>เมื่อพฤติกรรมมาตรฐานของ transaction หนึ่งไม่ตรง, ให้ลงทะเบียน handler ของคุณเองสำหรับมันด้วย <code>AddHandler</code>. ไลบรารียังคง routing และตรวจสอบคำขอ, และ transaction อื่น ๆ รักษา handler มาตรฐานของตน.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ความปลอดภัยและโฮสติ้งอยู่ในมือของคุณ">}}

<p>endpoint ของ DICOMweb เป็น endpoint ของ ASP.NET Core ธรรมดา. การตรวจสอบสิทธิ์, การอนุญาต, CORS, TLS, forwarded headers และขีดจำกัดขนาดคำขอ ถูกกำหนดค่าแบบเดียวกับส่วนอื่นของบริการของคุณ:</p>

<div class="codeblock" id="code">
 <h3>ต้องการผู้ใช้ที่ได้รับการยืนยันตัวตน - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>ส่วนโปรโตคอลไม่ขึ้นกับ ASP.NET Core. <code>DicomWebApplication</code> แปลง <code>DicomWebRequest</code> ให้เป็น response, ดังนั้นบริการเดียวกันสามารถทำงานเบื้องหลัง HTTP server อื่นหรือในการทดสอบโดยไม่มีเครือข่าย:</p>

<div class="codeblock" id="code">
 <h3>เซิร์ฟเวอร์เดียวกันโดยไม่ใช้ ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Client และ server จากไลบรารีเดียวกัน">}}

<p><a href="/medical/net/dicomweb-client/">DICOMweb client</a> ของ Aspose.Medical ทดสอบเซิร์ฟเวอร์ของคุณจาก unit test หรือโปรแกรมคอนโซล, และทุกไฟล์ที่เซิร์ฟเวอร์รับคือ <code>DicomFile</code> ปกติที่สามารถทำการลบข้อมูลส่วนบุคคล, แปลงโค้ดหรือเรนเดอร์ในกระบวนการเดียวกัน.</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">คู่มือผู้พัฒนา DICOMweb</a> ครอบคลุมทุกบริการและอินเทอร์เฟซการจัดเก็บ, และ <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">คู่มือ ASP.NET Core</a> ครอบคลุมการแมป endpoint และ WebSockets.</p>

<p>หน้าเกี่ยวข้อง: <a href="/medical/net/dicom-networking/">DICOM networking</a> สำหรับระบบที่ส่ง study ผ่าน DIMSE แทน HTTP, <a href="/medical/net/anonymization/">การลบข้อมูลส่วนบุคคล</a> ของ study ที่เข้ามา, <a href="/medical/net/dicom-transfer-syntax-conversion/">การแปลง transfer syntax</a>, และ <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> สำหรับรูปแบบที่ผลลัพธ์ QIDO-RS ใช้.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือผู้พัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="สนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนแบบชำระเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องเลือก Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
