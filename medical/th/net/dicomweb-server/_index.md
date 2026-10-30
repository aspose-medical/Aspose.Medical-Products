---
title: DICOMweb Server ใน C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: สร้าง DICOMweb server หรือ PACS ของคุณเองด้วย C# Aspose.Medical รองรับ QIDO-RS, WADO-RS, STOW-RS และบริการ worklist บน ASP.NET Core; คุณเชื่อมต่อกับการจัดเก็บข้อมูล
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb Server สำหรับ .NET C#" h2="เพิ่ม endpoint ของ QIDO-RS, WADO-RS และ STOW-RS ไปยังบริการ ASP.NET Core ไลบรารีสื่อสารตามโปรโตคอล; โค้ดของคุณกำหนดว่าการศึกษา (studies) จะถูกเก็บไว้ที่ใด" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOMweb server ของคุณเองใน C#">}}

<p>PACS, VNA, คลังข้อมูลบนคลาวด์ หรือแพลตฟอร์มการวิจัยต้องตอบสนองต่อคำขอ DICOMweb: การค้นหา QIDO-RS, การดาวน์โหลด WADO-RS และการอัปโหลด STOW-RS รวมถึงบริการ worklist บ่อยครั้ง แต่ละบริการมี route, media type, multipart body, กฎการแมตช์และรหัสสถานะของตนเอง</p>

<p><strong>Aspose.Medical for .NET</strong> ได้ดำเนินการส่วนของโปรโตคอลนี้ตั้งแต่เวอร์ชัน 26.10 มันอ่านคำขอทุกอย่าง, ตรวจสอบตามมาตรฐาน, จับคู่คีย์การค้นหาตามกฎของ DICOM, ทำเพจผลลัพธ์, แปลง transfer syntax และเขียนตอบกลับ โค้ดของคุณทำสิ่งที่คุณเท่านั้นทำได้: เก็บ instance, ค้นหาผู้สมัครสำหรับการค้นหาและคืนไฟล์ที่เก็บไว้ ผู้ชม, โมดาลิตี้และคลังข้อมูลอื่น ๆ จะทำงานกับเซิร์ฟเวอร์ของคุณเช่นเดียวกับ DICOMweb server ใด ๆ และเช่นเดียวกับ <a href="/medical/net/dicomweb-client/">DICOMweb client</a> ของไลบรารีเดียวกัน</p>

<p>เซิร์ฟเวอร์ถูกจัดการด้วย C#. มันทำงานได้ทุกที่ที่ ASP.NET Core ทำงาน: บน Windows, บน Linux และในคอนเทนเนอร์</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เซิร์ฟเวอร์เต็มรูปแบบใน ASP.NET Core">}}

<p>ติดตั้งแพ็คเกจ <code>Aspose.Medical.Dicom.Web.AspNetCore</code> มันจะนำ Aspose.Medical มาพร้อมและเชื่อมต่อ DICOMweb services กับ dependency injection และ endpoint routing ตัวอย่าง <code>Program.cs</code> นี้เป็นเซิร์ฟเวอร์ Studies ที่ทำงานได้; สองคลาสที่มันลงทะเบียนแสดงอยู่ด้านล่าง</p>

<div class="codeblock" id="code">
 <h3>Program.cs ของ DICOMweb server - C#</h3>
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

<p>ทุก route ของ Studies ตอนนี้ตอบภายใต้ <code>/dicomweb</code>: <code>/dicomweb/studies</code> สำหรับการค้นหาและอัปโหลด, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> สำหรับการเรียกคืน, รวมถึง route สำหรับ metadata, frames, bulk data และภาพที่เรนเดอร์ เอกสาร capabilities ที่รูทของบริการระบุรายการ transaction ที่เซิร์ฟเวอร์รองรับอย่างชัดเจน</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เชื่อมต่อการจัดเก็บของคุณ">}}

<p>บริการ Studies ต้องการอินเทอร์เฟซหนึ่งที่มีสามเมธอด: เก็บ instance, คืนไฟล์ที่เก็บไว้, และคืนผู้สมัครการค้นหา การทำงานนี้เก็บแต่ละ instance เป็นไฟล์ในโฟลเดอร์ตาม study และ series</p>

<div class="codeblock" id="code">
 <h3>คลังข้อมูลแบบโฟลเดอร์ที่ทำงานเบื้องหลัง QIDO-RS, WADO-RS และ STOW-RS - C#</h3>
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

<p>ไลบรารีตรวจสอบ UID ในเส้นทางของแต่ละคำขอและในแต่ละ instance ที่อัปโหลดก่อนที่จะเรียก provider instance ที่ไม่มี UID ที่ถูกต้องจะถูกรายงานว่า failed และจะไม่ถึง <code>StoreAsync</code> เลย</p>

<p>โฟลเดอร์เดียวก็เพียงพอสำหรับทดสอบเซิร์ฟเวอร์ คลังข้อมูลแบบผลิตจริงจะใช้เมธอดสามอย่างเดียวกันและเชื่อม object store ไว้หลัง <code>StoreAsync</code> และ <code>RetrieveAsync</code> และดัชนีฐานข้อมูลหลัง <code>FindCandidatesAsync</code></p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การค้นหา: ไลบรารีใช้กฎการแมตช์ของ DICOM">}}

<p>การแมตช์ QIDO-RS มีมากกว่าการเปรียบเทียบสตริง: wildcards, ช่วงวันที่และเวลา, รายการ UID, ชื่อบุคคลแบบ fuzzy, ค่าที่ว่างเปล่าและคีย์ภายใน sequence. provider ไม่ได้นำไปใช้. มันคืนผู้สมัครในลำดับที่เสถียร และอาจคืนมากกว่าที่ตรงกัน. ไลบรารีนำการแมตช์ DICOM ที่แม่นยำไปใช้กับแต่ละผู้สมัคร, เก็บฟิลด์ที่ร้องขอ, และประยุกต์ <code>offset</code> และ <code>limit</code>. การตอบหนึ่งจะมีผลลัพธ์สูงสุด 1000 รายการโดยค่าเริ่มต้น, และ <code>DicomWebStandardServiceOptions</code> สามารถเปลี่ยนขีดจำกัดนั้นได้.</p>

<p>เพื่อให้การค้นหาเร็วในคลังข้อมูลขนาดใหญ่ ให้ส่งคีย์ที่เลือกไปยังฐานข้อมูล <code>search.Plan.Filter</code> จะเก็บคีย์การค้นหาที่คอมไพล์แล้ว พร้อมกับ attribute, ประเภทการแมตช์และค่าของแต่ละคีย์:</p>

<div class="codeblock" id="code">
 <h3>อ่าน Patient ID ที่แน่นอนจากการค้นหา - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>คำสั่งคิวรีฐานข้อมูลที่สร้างแบบนี้อาจคืนแถวเพิ่มเติม เช่น เมื่อมันละเลยคีย์ที่ไม่สามารถแปลได้ การแมตช์สุดท้ายในไลบรารีจะลบแถวนั้นออก ต้องไม่ลบทิ้งการแมตช์ที่จริง เพราะไลบรารีไม่สามารถเพิ่มกลับได้</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การเก็บ: ผลลัพธ์สำหรับแต่ละ instance">}}

<p>คำขอ STOW-RS สามารถบรรจุ instance จำนวนหลายร้อย ไลบรารีจะพาร์ส multipart body และเรียก <code>StoreAsync</code> สำหรับแต่ละ instance อย่างต่อเนื่อง ดังนั้นการอัปโหลดทั้งหมดจะไม่ถูกเก็บในหน่วยความจำพร้อมกัน สำหรับแต่ละ instance provider จะคืนค่า <code>Stored</code>, <code>Warning</code> หาก instance ถูกเก็บพร้อมการเปลี่ยนแปลง, หรือ <code>Failed</code> พร้อมรหัสเหตุผล DICOM ที่เป็นออพชั่น</p>

<p>ไลบรารีสร้างรายงาน STOW-RS จากผลลัพธ์เหล่านี้และกำหนดสถานะ: 200 เมื่อทุก instance ถูกเก็บ, 202 เมื่อบางส่วนมี warning หรือ failed, 409 เมื่อทั้งหมด failed. Instance ที่อยู่ในคำขอของ study หนึ่งแต่เป็นของ study อื่นจะถูกปฏิเสธก่อนที่ provider จะถูกเรียก</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การเรียกคืน: transfer syntax, frames และภาพที่เรนเดอร์">}}

<p>provider จะคืนไฟล์ที่เก็บไว้และไลบรารีจะส่งต่อในรูปแบบที่ client ร้องขอ client WADO-RS สามารถขอ transfer syntax ใน header <code>Accept</code> และไลบรารีจะแปลงไฟล์แต่ละไฟล์ก่อนส่ง รวมถึง codecs ของตัวเอง เช่น HTJ2K และ JPEG XL Metadata ในรูป DICOM JSON หรือ XML, bulk data และ single frames มาจาก provider เดียวกันโดยไม่ต้องเขียนโค้ดเพิ่มเติม</p>

<p>ภาพที่เรนเดอร์ต้องการคลาสเพิ่มเติมหนึ่งคลาส เนื่องจากคุณเป็นผู้กำหนดว่าภาพใดบ้างที่เซิร์ฟเวอร์อาจผลิตได้ คลาสนี้ให้ JPEG สำหรับ instance ที่เรนเดอร์, frames ที่เรนเดอร์และ thumbnails, และไลบรารีจะทำการเรนเดอร์ให้:</p>

<div class="codeblock" id="code">
 <h3>ให้บริการภาพ JPEG ที่เรนเดอร์และ thumbnails - C#</h3>
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

<p>renderer ใส่ในตัวสร้าง JPEG จาก instance แบบ single-frame หรือจาก frames ที่คำขอเลือก instance แบบ multi-frame ที่เรนเดอร์ทั้งหมด, ภาพที่กว้างหรือสูงกว่า 65500 พิกเซล (thumbnails จะข้ามภาพดังกล่าว), รูปแบบภาพอื่น ๆ, และพารามิเตอร์ viewport, window และ ICC profile ต้องใช้ <code>RetrieveAsync</code> ของคุณเองในคลาสนี้</p>

<p>Thumbnails จะจัดการอย่างระมัดระวังเพื่อความเป็นส่วนตัวของผู้ป่วย renderer ในตัวสร้างทำ thumbnail เฉพาะจาก instance ที่มี attribute Burned In Annotation และ Recognizable Visual Features เป็น <code>NO</code> ซึ่งหมายความว่าภาพนั้นบ่งบอกว่าไม่มีข้อมูลผู้ป่วย ภาพจำนวนมากไม่มี attribute เหล่านี้ และสำหรับภาพเหล่านั้นเซิร์ฟเวอร์จะตอบ 406 ถ้าคลังข้อมูลของคุณต้องการ thumbnails ของภาพเหล่านี้ ให้สร้างใน <code>RetrieveAsync</code> ของคุณเองและตรวจสอบให้แน่ใจว่าได้ซ่อนข้อมูลผู้ป่วยทั้งหมด</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklist, ขั้นตอนการทำงานและบริการอื่น ๆ">}}

<p>DICOMweb services อื่น ๆ ทำตามโมเดลเดียวกัน: ลงทะเบียนบริการ, ดำเนินการอินเทอร์เฟซการจัดเก็บของมัน</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> worklists กับ <code>AddStandardWorklistService</code>: สร้าง, ค้นหาและอัพเดท workitems และเปลี่ยนสถานะของพวกมัน โดยกฎสถานะจะถูกตรวจสอบโดยไลบรารีและการแจ้งเตือนเหตุการณ์จะส่งผ่าน WebSocket</li>
<li><a href="/medical/net/modality-worklist/">modality worklist</a> (MWL-RS) กับ <code>AddStandardModalityWorklistService</code>, เพื่อให้โมดาลิตี้สามารถสอบถามขั้นตอนการทำงานที่กำหนดไว้ของ RIS ของคุณ</li>
<li>Modality performed procedure steps (MPPS-RS), storage commitment และ instance ที่ไม่เกี่ยวกับผู้ป่วย เช่น color palettes และ hanging protocols</li>
<li>การเรียกคืน WADO-URI สำหรับ viewer รุ่นเก่า ที่ให้บริการโดย Studies service</li>
</ul>

<p>เมื่อพฤติกรรมมาตรฐานของ transaction ใด ๆ ไม่ตรงกับความต้องการ ให้ลงทะเบียน handler ของคุณเองสำหรับมันด้วย <code>AddHandler</code> ไลบรารียังคงทำ routing และตรวจสอบคำขอ และ transaction อื่น ๆ ยังคงใช้ handler มาตรฐานของพวกมัน</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ความปลอดภัยและการโฮสต์อยู่ในมือของคุณ">}}

<p>DICOMweb endpoints เป็น endpoint ของ ASP.NET Core ปกติ การตรวจสอบผู้ใช้, การกำหนดสิทธิ, CORS, TLS, forwarded headers และขนาดคำขอที่จำกัด ถูกกำหนดค่าเช่นเดียวกับส่วนอื่นของบริการของคุณ:</p>

<div class="codeblock" id="code">
 <h3>บังคับให้ผู้ใช้ต้องผ่านการรับรองตัวตน - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>ส่วนของโปรโตคอลไม่ขึ้นกับ ASP.NET Core <code>DicomWebApplication</code> แปลง <code>DicomWebRequest</code> เป็น response จึงทำให้บริการเดียวกันสามารถทำงานเบื้องหลัง HTTP server อื่นหรือในการทดสอบโดยไม่มีเครือข่าย</p>

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

<p><a href="/medical/net/dicomweb-client/">DICOMweb client</a> ของ Aspose.Medical ทดสอบเซิร์ฟเวอร์ของคุณจาก unit test หรือโปรแกรม console, และทุกไฟล์ที่เซิร์ฟเวอร์รับคือ <code>DicomFile</code> ปกติที่สามารถทำการ anonymize, transcode หรือ render ในกระบวนเดียวกัน</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">คู่มือผู้พัฒนา DICOMweb</a> ครอบคลุมทุกบริการและอินเทอร์เฟซการจัดเก็บ, และ <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">คู่มือ ASP.NET Core</a> ครอบคลุมการแมป endpoint และ WebSockets</p>

<p>หน้าเกี่ยวข้อง: <a href="/medical/net/dicom-networking/">DICOM networking</a> สำหรับระบบที่ส่ง studies ผ่าน DIMSE แทน HTTP, <a href="/medical/net/anonymization/">anonymization</a> ของ studies ที่เข้ามา, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a>, และ <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> สำหรับรูปแบบผลลัพธ์ของ QIDO-RS</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือผู้พัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="เอกสารอ้างอิง API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนแบบจ่ายเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้อง Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
