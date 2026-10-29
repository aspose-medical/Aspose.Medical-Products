---
title: เซิร์ฟเวอร์ DICOMweb ใน C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: สร้างเซิร์ฟเวอร์ DICOMweb หรือ PACS ของคุณเองด้วย C# Aspose.Medical ใช้งาน QIDO-RS, WADO-RS, STOW-RS และบริการ worklist บน ASP.NET Core; คุณทำการเชื่อมต่อที่เก็บข้อมูล
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="เซิร์ฟเวอร์ DICOMweb สำหรับ .NET C#" h2="เพิ่ม endpoint QIDO-RS, WADO-RS และ STOW-RS ไปยังบริการ ASP.NET Core ไลบรารีสื่อสารตามโปรโตคอล; โค้ดของคุณเป็นผู้กำหนดตำแหน่งที่เก็บชุดภาพ" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="เซิร์ฟเวอร์ DICOMweb ของคุณเองใน C#">}}

<p>PACS, VNA, ที่เก็บข้อมูลคลาวด์ หรือแพลตฟอร์มการวิจัยต้องตอบสนองคำขอ DICOMweb: การค้นหา QIDO-RS, การดาวน์โหลด WADO-RS และการอัปโหลด STOW-RS, และบ่อยครั้งรวมถึงบริการ worklist ด้วย แต่ละบริการมีเส้นทาง, ประเภทสื่อ, เนื้อหา multipart, กฎการจับคู่ และรหัสสถานะของตนเอง</p>

<p><strong>Aspose.Medical for .NET</strong> ทำงานด้านโปรโตคอลนี้ตั้งแต่เวอร์ชัน 26.10 มันอ่านคำขอทุกครั้ง, ตรวจสอบตามมาตรฐาน, จับคู่คีย์การค้นหาตามกฎ DICOM, แบ่งหน้าผลลัพธ์, แปลง transfer syntax และเขียนการตอบกลับ โค้ดของคุณทำในสิ่งที่คุณทำได้เท่านั้น: เก็บอินสแตนซ์, ค้นหาผลผู้สมัครสำหรับการค้นหาและส่งคืนไฟล์ที่เก็บไว้ ตัวอ่านภาพ, โมดาลิตี้และที่เก็บข้อมูลอื่น ๆ จะทำงานกับเซิร์ฟเวอร์ของคุณเช่นเดียวกับเซิร์ฟเวอร์ DICOMweb ใด ๆ และเช่นเดียวกันกับ <a href="/medical/net/dicomweb-client/">DICOMweb client</a> ของไลบรารีเดียวกัน</p>

<p>เซิร์ฟเวอร์นี้จัดการด้วย C# มันทำงานได้ทุกที่ที่ ASP.NET Core ทำงาน: บน Windows, บน Linux และในคอนเทนเนอร์</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เซิร์ฟเวอร์เต็มรูปแบบใน ASP.NET Core">}}

<p>ติดตั้งแพคเกจ <code>Aspose.Medical.Dicom.Web.AspNetCore</code> มันจะนำ Aspose.Medical มาและเชื่อมต่อบริการ DICOMweb กับ dependency injection และ endpoint routing ตัวอย่าง <code>Program.cs</code> นี้เป็นเซิร์ฟเวอร์ Studies ที่ทำงานได้; รายการคลาสสองคลาสที่ลงทะเบียนจะแสดงด้านล่าง</p>

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

<p>เส้นทาง Studies ทุกเส้นตอนนี้ตอบภายใต้ <code>/dicomweb</code>: <code>/dicomweb/studies</code> สำหรับการค้นหาและอัพโหลด, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> สำหรับการเรียกคืน, รวมถึงเส้นทางสำหรับ metadata, frames, bulk data และภาพที่เรนเดอร์ เอกสาร capabilities ที่รากของบริการแสดงรายการธุรกรรมที่เซิร์ฟเวอร์สนับสนุนอย่างชัดเจน</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เชื่อมต่อที่เก็บข้อมูลของคุณ">}}

<p>บริการ Studies ต้องการอินเทอร์เฟซหนึ่งชุดที่มีสามเมธอด: เก็บอินสแตนซ์, ส่งคืนไฟล์ที่เก็บไว้, และส่งคืนผู้สมัครสำหรับการค้นหา การดำเนินการนี้เก็บแต่ละอินสแตนซ์เป็นไฟล์ในโฟลเดอร์แยกตาม study และ series</p>

<div class="codeblock" id="code">
 <h3>คลังข้อมูลแบบโฟลเดอร์สำหรับ QIDO-RS, WADO-RS และ STOW-RS - C#</h3>
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

<p>ไลบรารีตรวจสอบ UID ในทุกเส้นทางของคำขอและในทุกอินสแตนซ์ที่อัปโหลดก่อนเรียกผู้ให้บริการ อินสแตนซ์ที่ไม่มี UID ที่ถูกต้องจะถูกระบุว่าเกิดข้อผิดพลาดและจะไม่ถึง <code>StoreAsync</code> ขณะใดเลย</p>

<p>โฟลเดอร์เดียวก็เพียงพอเพื่อทดลองเซิร์ฟเวอร์ คลังข้อมูลระดับการผลิตจะใช้เมธอดเดียวกันสามเมธอดและวาง object store ไว้หลัง <code>StoreAsync</code> และ <code>RetrieveAsync</code> และดัชนีฐานข้อมูลไว้หลัง <code>FindCandidatesAsync</code></p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การค้นหา: ไลบรารีใช้กฎการจับคู่ DICOM">}}

<p>การจับคู่ QIDO-RS เกินกว่าการเปรียบเทียบสตริงธรรมดา: ไวลด์การ์ด, ช่วงวันที่และเวลา, รายการ UID, ชื่อผู้ใช้แบบ fuzzy, ค่าเปล่าและคีย์ภายใน sequence ผู้ให้บริการไม่ได้ดำเนินการใด ๆ เหมือนนั้น มันส่งคืนผู้สมัครในลำดับที่คงที่และอาจส่งคืนมากกว่าการจับคู่ ไลบรารีใช้การจับคู่ DICOM อย่างแม่นยำกับแต่ละผู้สมัคร เก็บฟิลด์ที่ร้องขอ และใช้ <code>offset</code> และ <code>limit</code> การตอบสนองหนึ่งจะมีผลลัพธ์สูงสุด 1000 รายการโดยค่าเริ่มต้น และ <code>DicomWebStandardServiceOptions</code> สามารถเปลี่ยนขีดจำกัดนั้นได้</p>

<p>เพื่อให้การค้นหาเร็วบนคลังข้อมูลขนาดใหญ่ ให้ส่งคีย์ที่เลือกไปยังฐานข้อมูล <code>search.Plan.Filter</code> เก็บคีย์การค้นหาที่คอมไพล์แล้ว พร้อมแอตทริบิวต์, ประเภทการจับคู่และค่าของแต่ละคีย์</p>

<div class="codeblock" id="code">
 <h3>อ่าน Patient ID ที่ตรงจากการค้นหา - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>คิวรีฐานข้อมูลที่สร้างแบบนี้อาจคืนแถวเพิ่มเติม ตัวอย่างเช่นเมื่อมันละทิ้งคีย์ที่ไม่สามารถแปลได้ การจับคู่สุดท้ายในไลบรารีจะลบแถวเหล่านั้นออก แต่ต้องไม่ตัดการจับคู่ที่ถูกต้องออก เพราะไลบรารีไม่สามารถเพิ่มกลับมาได้</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="จัดเก็บ: ผลลัพธ์สำหรับแต่ละอินสแตนซ์">}}

<p>คำขอ STOW-RS สามารถบรรจุอินสแตนซ์หลายร้อยรายการ ไลบรารีจะพาร์ส multipart body และเรียก <code>StoreAsync</code> สำหรับแต่ละอินสแตนซ์ตามลำดับ ดังนั้นการอัปโหลดทั้งหมดจะไม่ถูกเก็บไว้ในหน่วยความจำพร้อมกัน สำหรับแต่ละอินสแตนซ์ ผู้ให้บริการจะคืนค่า <code>Stored</code>, <code>Warning</code> หากอินสแตนซ์ถูกเก็บพร้อมการเปลี่ยนแปลง, หรือ <code>Failed</code> พร้อมรหัสเหตุผล DICOM ที่เป็นตัวเลือก</p>

<p>ไลบรารีสร้างรายงาน STOW-RS จากผลลัพธ์เหล่านี้และเลือกสถานะ: 200 เมื่อทุกอินสแตนซ์ถูกเก็บ, 202 เมื่อบางส่วนมีคำเตือนหรือล้มเหลว, 409 เมื่อทั้งหมดล้มเหลว อินสแตนซ์ในคำขอที่ระบุ study หนึ่งแต่เป็นของ study อื่นจะถูกปฏิเสธก่อนที่ผู้ให้บริการจะถูกเรียก</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="เรียกคืน: transfer syntax, frames และภาพที่เรนเดอร์">}}

<p>ผู้ให้บริการคืนไฟล์ที่เก็บไว้และไลบรารีส่งมอบในรูปแบบที่ไคลเอนต์ร้องขอ ไคลเอนต์ WADO-RS สามารถขอ transfer syntax ในหัวข้อ <code>Accept</code> และไลบรารีจะแปลงไฟล์แต่ละไฟล์ก่อนส่ง พร้อม codecs ของตัวเอง ได้แก่ HTJ2K และ JPEG XL Metadata ในรูปแบบ DICOM JSON หรือ XML, bulk data และ single frames มาจากผู้ให้บริการเดียวกันโดยไม่ต้องเขียนโค้ดเพิ่มเติม</p>

<p>ภาพที่เรนเดอร์ต้องการคลาสเพิ่มเติมหนึ่งคลาส เนื่องจากคุณเป็นผู้กำหนดว่าภาพใดที่เซิร์ฟเวอร์สามารถสร้างได้ คลาสนี้ให้บริการ JPEG สำหรับอินสแตนซ์ที่เรนเดอร์, เฟรมที่เรนเดอร์และ thumbnail และไลบรารีจะทำการเรนเดอร์ให้</p>

<div class="codeblock" id="code">
 <h3>ให้บริการภาพ JPEG ที่เรนเดอร์และ thumbnail - C#</h3>
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

<p>เรนเดอร์ในตัวจะสร้าง JPEG จากอินสแตนซ์ single-frame หรือจากเฟรมที่คำขอเลือก อินสแตนซ์ multi-frame ที่เรนเดอร์ทั้งหมด รูปแบบภาพอื่น ๆ และพารามิเตอร์ viewport, window และ ICC profile จำเป็นต้องมี <code>RetrieveAsync</code> ของคุณเองในคลาสนี้</p>

<p>Thumbnail จะจัดการอย่างระมัดระวังเพื่อคุ้มครองความเป็นส่วนตัวของผู้ป่วย เรนเดอร์ในตัวจะสร้าง thumbnail เฉพาะจากอินสแตนซ์ที่แอตทริบิวต์ Burned In Annotation และ Recognizable Visual Features มีค่า <code>NO</code> ทั้งสอง ซึ่งหมายความว่าภาพระบุว่าไม่มีข้อมูลผู้ป่วยหลายภาพไม่มีแอตทริบิวต์เหล่านี้และสำหรับพวกมันเซิร์ฟเวอร์จะตอบ 406 หากคลังของคุณต้องการ thumbnail ของภาพดังกล่าว ให้สร้างใน <code>RetrieveAsync</code> ของคุณเองและตรวจสอบให้แน่ใจว่าซ่อนข้อมูลผู้ป่วยทั้งหมด</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklists, ขั้นตอนกระบวนการ และบริการอื่น ๆ">}}

<p>บริการ DICOMweb อื่น ๆ ทำตามโมเดลเดียวกัน: ลงทะเบียนบริการ, ดำเนินการอินเทอร์เฟซการจัดเก็บของมัน</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> worklists ด้วย <code>AddStandardWorklistService</code>: สร้าง, ค้นหาและอัปเดต workitem และเปลี่ยนสถานะของมัน โดยที่กฎสถานะจะถูกตรวจสอบโดยไลบรารีและส่งการแจ้งเตือนเหตุการณ์ผ่าน WebSocket</li>
<li><a href="/medical/net/modality-worklist/">modality worklist</a> (MWL-RS) ด้วย <code>AddStandardModalityWorklistService</code> เพื่อให้โมดาลิตี้สามารถสอบถามขั้นตอนกระบวนการที่กำหนดไว้ใน RIS ของคุณ</li>
<li>Modality performed procedure steps (MPPS-RS), การมอบหมายการจัดเก็บและอินสแตนซ์ที่ไม่ใช่ผู้ป่วย เช่น พาเลตสีและ hanging protocols</li>
<li>การเรียกคืน WADO-URI สำหรับผู้ดูภาพรุ่นเก่า ให้บริการโดย Studies service</li>
</ul>

<p>เมื่อพฤติกรรมมาตรฐานของธุรกรรมใดไม่ตรงให้ลงทะเบียนตัวจัดการของคุณเองโดยใช้ <code>AddHandler</code> ไลบรารียังคงทำการ routing และตรวจสอบคำขอ และธุรกรรมอื่น ๆ จะคงตัวจัดการมาตรฐานของพวกมัน</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ความปลอดภัยและการโฮสต์อยู่ในมือของคุณ">}}

<p>endpoint DICOMweb เป็น endpoint ของ ASP.NET Core ปกติ การตรวจสอบสิทธิ์, การอนุญาต, CORS, TLS, header ที่ส่งต่อและขีดจำกัดขนาดคำขอ ถูกกำหนดค่าเช่นเดียวกับส่วนอื่นของบริการของคุณ:</p>

<div class="codeblock" id="code">
 <h3>กำหนดให้ผู้ใช้ต้องผ่านการตรวจสอบตัวตน - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>ส่วนของโปรโตคอลไม่ขึ้นกับ ASP.NET Core <code>DicomWebApplication</code> แปลง <code>DicomWebRequest</code> ให้เป็นการตอบกลับ ดังนั้นบริการเดียวกันสามารถทำงานหลัง HTTP server อื่นหรือในการทดสอบโดยไม่มีเครือข่าย</p>

<div class="codeblock" id="code">
 <h3>เซิร์ฟเวอร์เดียวกันโดยไม่ใช้ ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ไคลเอนต์และเซิร์ฟเวอร์จากไลบรารีเดียวกัน">}}

<p><a href="/medical/net/dicomweb-client/">DICOMweb client</a> ของ Aspose.Medical ทดสอบเซิร์ฟเวอร์ของคุณจาก unit test หรือโปรแกรมคอนโซล และทุกไฟล์ที่เซิร์ฟเวอร์รับมาจะเป็น <code>DicomFile</code> ปกติที่สามารถทำการลบข้อมูลส่วนบุคคล, แปลงโค้ดหรือเรนเดอร์ในกระบวนการเดียวกันได้</p>

<p><a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">คู่มือผู้พัฒนา DICOMweb</a> ครอบคลุมทุกบริการและอินเทอร์เฟซการจัดเก็บ และ <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">คู่มือ ASP.NET Core</a> ครอบคลุมการแมป endpoint และ WebSockets</p>

<p>หน้าที่เกี่ยวข้อง: <a href="/medical/net/dicom-networking/">DICOM networking</a> สำหรับระบบที่ส่งชุดภาพผ่าน DIMSE แทน HTTP, <a href="/medical/net/anonymization/">การทำให้เป็นนิรนาม</a> ของชุดภาพที่เข้ามา, <a href="/medical/net/dicom-transfer-syntax-conversion/">การแปลง transfer syntax</a> และ <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> สำหรับรูปแบบผลลัพธ์ QIDO-RS</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือผู้พัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบชำระเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องเลือก Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
