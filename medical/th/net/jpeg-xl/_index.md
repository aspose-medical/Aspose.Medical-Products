---
title: JPEG XL สำหรับ DICOM ใน C# .NET | Aspose.Medical
weight: 10500

description: จัดเก็บภาพ DICOM ในรูปแบบ JPEG XL จาก C#. JPEG XL แบบไม่สูญเสียข้อมูลที่ส่งคืนพิกเซลแบบบิตต่อบิต ใน assembly ที่จัดการได้เพียงชุดเดียวโดยไม่มีโค้ดเดคพื้นฐานที่ต้องติดตั้ง.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="JPEG XL สำหรับ DICOM ใน .NET C#" h2="การบีบอัดใหม่ล่าสุดในมาตรฐาน DICOM, ด้วยไฟล์แบบไม่สูญเสียข้อมูลที่เล็กที่สุดที่เราวัดได้, ถูกพัฒนาเป็น C# ที่จัดการได้และจัดส่งภายใน assembly เดียว." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="ทำไม JPEG XL จึงเข้าสู่ DICOM">}}

<p>คลังข้อมูลการแพทย์เติบโตและไม่เคยลดขนาดลง. JPEG XL คือโค้ดเดคที่วงการภาพถ่ายออกแบบหลังจากสองทศวรรษของประสบการณ์กับ JPEG และ JPEG 2000, และ DICOM เพิ่งเพิ่มมันเป็น transfer syntax ด้วยเหตุผลที่ทีมจัดเก็บให้ความสำคัญ: สำหรับพิกเซลเดียวกัน, ไฟล์จะมีขนาดเล็กลง.</p>

<p><strong>Aspose.Medical for .NET</strong> เขียนและอ่าน JPEG XL ผ่านพอร์ต C# ของ libjxl ที่ฝังอยู่ในไลบรารี. แพคเกจนี้มาพร้อมกับ assembly เดียว, <code>Aspose.Medical.dll</code>, และไม่มีไบนารีพื้นฐานใดๆ คั่นอยู่, ดังนั้นโค้ดเดคใหม่เช่นนี้จะไม่กลายเป็นโครงการที่ต้องติดตั้งแยก: assembly เดียวกันทำงานได้บน Windows, Linux, บนอัจฉริยะการสร้าง (build agent) และในคอนเทนเนอร์.</p>

<p>สอง transfer syntax ส่งพิกเซล:</p>

<ul>
<li><code>TransferSyntax.JpegXLLossless</code> (1.2.840.10008.1.2.4.110), สำหรับข้อมูลการวินิจฉัยที่ต้องกลับมาภายหลังโดยไม่เปลี่ยนแปลง.</li>
<li><code>TransferSyntax.JpegXL</code> (1.2.840.10008.1.2.4.112), สำหรับกรณีที่ไฟล์ขนาดเล็กสำคัญกว่าการคัดลอกที่ตรงกันเป๊ะ.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="บีบอัดการศึกษา, คงทุกพิกเซล">}}

<p>การแปลงโค้ดเป็นการเรียกหนึ่งครั้ง, และชุดข้อมูลรอบพิกเซลจะเดินทางพร้อมกับมัน.</p>

<div class="codeblock" id="code">
 <h3>แปลงไฟล์ DICOM เป็น JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// JPEG XL, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.JpegXLLossless);
compressed.Save("study-jxl.dcm");</code></pre>
</div>

<p>เราได้ทำการวัดบนภาพ 16‑bit ขนาด 1714 × 1933 จากชุดทดสอบของเรา: ขนาด 6.3 MB ไม่บีบอัดกลายเป็น 2.7 MB ใน JPEG XL แบบไม่สูญเสียข้อมูล, ซึ่งเล็กกว่าภาพเดียวกันใน HTJ2K แบบไม่สูญเสีย. ค่าตัวเลขของคุณจะขึ้นอยู่กับโหมด, ดังนั้นควรเปรียบเทียบในโฟลเดอร์ไฟล์ของคุณก่อนตัดสินใจ.</p>

<p>คำว่า Lossless ต้องรับตามตัวอักษรที่นี่. แปลงเป็น JPEG XL แล้วแปลงกลับ, ข้อมูลพิกเซลจะเท่ากับไบต์ที่คุณเริ่มต้น, ดังนั้นคลังข้อมูลสามารถบีบอัดใหม่ได้โดยไม่ต้องกังวลเรื่องคุณภาพการวินิจฉัย.</p>

<div class="codeblock" id="code">
 <h3>กลับไปยัง transfer syntax ที่ไม่บีบอัด - C#</h3>
 <pre><code class="cs">DicomFile compressed = DicomFile.Open("study-jxl.dcm");

DicomFile plain = compressed.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="อ่านข้อมูลที่ถูกจัดเก็บเป็น JPEG XL แล้ว">}}

<p>ไฟล์ที่มาด้วยรูปแบบ JPEG XL จะเปิดได้เช่นไฟล์อื่นๆ. Transfer syntax บ่งบอกประเภท, และข้อมูลพิกเซลจะพร้อมใช้งานเมื่อเฟรมถูกถอดรหัส.</p>

<div class="codeblock" id="code">
 <h3>เปิดไฟล์ JPEG XL - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-jxl.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
if (syntax == TransferSyntax.JpegXLLossless)
    Console.WriteLine("stored as JPEG XL lossless");

PixelData pixelData = PixelData.Read(received.Transcode(TransferSyntax.ExplicitVrLittleEndian).Dataset);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {pixelData.BitsAllocated}-bit");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JPEG XL หรือ HTJ2K">}}

<p>ทั้งสองเป็นเทคโนโลยีใหม่, ทั้งสองให้ผลแบบไม่สูญเสียเมื่อคุณร้องขอแบบ lossless, และไลบรารีสามารถเขียนและอ่านได้ทั้งคู่. พวกมันตอบคำถามที่แตกต่างกัน.</p>

<table class="table table-bordered">
<thead>
<tr>
<th>คำถาม</th>
<th>คำตอบ</th>
</tr>
</thead>
<tbody>
<tr><td>อันไหนสร้างไฟล์ที่เล็กกว่าในการทดสอบของเรา</td><td>JPEG XL lossless, โดยลดลงไม่กี่เปอร์เซ็นต์</td></tr>
<tr><td>อันไหนออกแบบมาสำหรับการดูแบบ progressive ผ่านเครือข่าย</td><td><a href="/medical/net/htj2k/">HTJ2K</a>, โดยเฉพาะรุ่น RPCL</td></tr>
<tr><td>อันไหนเข้าสู่มาตรฐาน DICOM ก่อน</td><td>HTJ2K, ดังนั้นคลังข้อมูลส่วนใหญ่ยอมรับมันในปัจจุบัน</td></tr>
<tr><td>อันไหนต้องใช้ native dependency ที่นี่</td><td>ไม่มี, ทั้งสองเป็นโค้ดที่จัดการได้ใน assembly เดียว</td></tr>
</tbody>
</table>

<p>การเลือกมักมาจากด้านอื่นของลิงก์: แปลงเป็น syntax ที่คลังข้อมูลยอมรับ, และคงขั้นตอนต่อไปของ pipeline เดิม.</p>

<div class="codeblock" id="code">
 <h3>ให้คลังข้อมูลเป้าหมายตัดสินใจ - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Pick the syntax the archive on the other side accepts
TransferSyntax target = archiveAcceptsJpegXl
    ? TransferSyntax.JpegXLLossless
    : TransferSyntax.HTJ2KLossless;

DicomFile compressed = dicomFile.Transcode(target);
compressed.Save("study-compressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="จุดที่ได้ประโยชน์">}}

<ul>
<li>คลังข้อมูลระยะยาว: การศึกษาที่เหมือนเดิม, ใช้เทระไบต์น้อยลง, และไม่มีการสูญเสียที่ต้องอธิบายต่อรังสีแพทย์.</li>
<li>บิลค่าใช้จ่ายคลาวด์สตอเรจ: การประหยัดเกิดซ้ำทุกเดือน, ขณะที่การแปลงโค้ดทำเพียงครั้งเดียว.</li>
<li>ชุดข้อมูลสำหรับการวิจัยและ AI: สำเนาที่เล็กลงย้ายเร็วขึ้นระหว่างการจัดเก็บและการฝึกโมเดล.</li>
<li>การปรับใช้: โค้ดเดคใหม่เช่นนี้มักต้องสร้าง native บนแต่ละแพลตฟอร์ม; แต่ที่นี่เป็นส่วนหนึ่งของ assembly ที่คุณอ้างอิงอยู่แล้ว.</li>
</ul>

<p>ไลบรารียังเขียนโค้ดเดคที่คลังข้อมูลที่มีอยู่เต็มไปด้วย: JPEG, JPEG‑LS, JPEG 2000, HTJ2K และ RLE. หน้าการ <a href="/medical/net/dicom-transfer-syntax-conversion/">แปลง transfer syntax</a> ครอบคลุมชุดทั้งหมด, <a href="/medical/net/htj2k/">HTJ2K</a> มีหน้าเฉพาะของมัน, และ <a href="/medical/net/jpeg2000/">JPEG 2000</a> คือที่มาของโค้ดเดคใหม่ทั้งสอง.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือผู้พัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนสินค้า" tabId="support" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="การสนับสนุนแบบชำระเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้อง Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
