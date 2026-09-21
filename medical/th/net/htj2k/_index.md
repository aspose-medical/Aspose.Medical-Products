---
title: HTJ2K ใน C# .NET - High-Throughput JPEG 2000 สำหรับ DICOM | Aspose.Medical
weight: 10000

description: บีบอัดและอ่านภาพ DICOM ใน High-Throughput JPEG 2000 จาก C#. HTJ2K lossless, รูปแบบ RPCL และ HTJ2K lossy, ถูกทำงานใน .NET ที่จัดการโดยไม่ต้องมี codec native เพื่อติดตั้ง.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="HTJ2K ใน .NET C#" h2="High-Throughput JPEG 2000 สำหรับ DICOM: การบีบอัดที่มาตรฐานเพิ่มเพื่อจัดเก็บอย่างรวดเร็วและการดูบนคลาวด์, ถูกทำงานใน C# ที่จัดการโดยไม่มีส่วน native ใดต้องติดตั้ง." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="สิ่งที่ HTJ2K เปลี่ยนแปลง">}}

<p>High-Throughput JPEG 2000 รักษา wavelet และคุณภาพภาพของ JPEG 2000 ไว้ แต่แทนที่ส่วนที่ทำให้การประมวลผลช้า ตัว block coder เป็นแบบใหม่และการถอดรหัสเร็วขึ้นเป็นลำดับขนาดหนึ่งของสิบเท่า ซึ่งเป็นเหตุผลที่มาตรฐาน DICOM ได้นำมาใช้ในสาม transfer syntax และทำให้แพลตฟอร์มภาพคลาวด์ย้ายไปใช้</p>

<p>สำหรับทีม .NET คำถามเชิงปฏิบัติก็แตกต่าง: ใครสามารถสร้างไฟล์เหล่านั้นได้จริง ๆ ไลบรารีส่วนใหญ่เข้าถึง HTJ2K ผ่านการสร้าง native ของ OpenJPH ซึ่งหมายถึงไบนารีต่อแต่ละแพลตฟอร์ม, ขั้นตอนการสร้างในคอนเทนเนอร์และการพึ่งพาที่ฝ่ายตรวจสอบความปลอดภัยจะสอบถาม <strong>Aspose.Medical for .NET</strong> ทำการ implement codec ด้วยโค้ดที่จัดการอยู่ในแพ็กเกจเดียวที่อ่านและเขียนไฟล์, ดังนั้น HTJ2K จะทำงานเช่นเดียวกันบน Windows, Linux และในคอนเทนเนอร์โดยไม่มีอะไรต้องติดตั้ง.</p>

<p>รองรับ transfer syntax ทั้งหมดสามแบบ, และทั้งสามแบบสามารถอ่านและเขียนได้:</p>

<ul>
<li><code>TransferSyntax.HTJ2KLossless</code> (1.2.840.10008.1.2.4.201), High-Throughput JPEG 2000 แบบ lossless.</li>
<li><code>TransferSyntax.HTJ2KLosslessRPCL</code> (1.2.840.10008.1.2.4.202), รูปแบบ lossless ที่ใช้ RPCL progression order.</li>
<li><code>TransferSyntax.HTJ2K</code> (1.2.840.10008.1.2.4.203), High-Throughput JPEG 2000.</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="บีบอัด study เป็น HTJ2K">}}

<p>การเรียกหนึ่งครั้งจะย้ายไฟล์ไปยัง transfer syntax ใหม่ ชุดข้อมูล, แท็กส่วนบุคคลและข้อมูลเมตาไฟล์จะเดินทางพร้อมกับไฟล์นั้น.</p>

<div class="codeblock" id="code">
 <h3>แปลงรหัสไฟล์ DICOM ไปเป็น HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// High-Throughput JPEG 2000, lossless
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLossless);
compressed.Save("study-htj2k.dcm");</code></pre>
</div>

<p>บนภาพขนาด 1714 x 1933 ที่มีความลึก 16‑bit จากชุดทดสอบของเรา ไฟล์จะลดขนาดจาก 6.3 MB เป็น 2.9 MB และพิกเซลจะคืนค่าเดิมครบถ้วน ตัวเลขจะแตกต่างตามโมดาลิตี้และภาพแต่ละภาพ ดังนั้นควรวัดผลกับข้อมูลของคุณเองโดยการวนลูปผ่านไฟล์ที่คุณมีอยู่แล้ว.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Lossless หมายถึง lossless">}}

<p>ข้อมูลการวินิจฉัยไม่สามารถยอมรับ codec ที่มีการสูญเสียแม้เพียงเล็กน้อย การแปลงรหัสเป็น HTJ2K lossless แล้วกลับคืน ทำให้ข้อมูลพิกเซลตรงกับไบต์เริ่มต้นอย่างสมบูรณ์, ซึ่งเป็นคุณสมบัติที่คุณสามารถตรวจสอบในชุดทดสอบของคุณก่อนที่จะยอมรับการบีบอัดซ้ำของ archive.</p>

<div class="codeblock" id="code">
 <h3>กลับไปยัง transfer syntax ที่ไม่มีการบีบอัด - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

// Back to an uncompressed syntax, pixel for pixel
DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
plain.Save("study-uncompressed.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="RPCL, รูปแบบที่ออกแบบมาสำหรับการดูผ่านเครือข่าย">}}

<p>syntax 1.2.840.10008.1.2.4.202 เก็บ codestream lossless เดียวกันใน RPCL progression order: ความละเอียดก่อน, จากนั้นตำแหน่ง, จากนั้นคอมโพเนนท์, จากนั้นเลเยอร์. ตัวอ่านที่รับเฉพาะส่วนเริ่มต้นของสตรีมจะได้ภาพความละเอียดต่ำที่สมบูรณ์, ซึ่งเป็นสิ่งที่ viewer ต้องการเมื่อเปิด study ขนาดใหญ่ผ่านลิงก์ที่ไม่ได้ควบคุม.</p>

<div class="codeblock" id="code">
 <h3>บีบอัดด้วยลำดับ RPCL progression order - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("study.dcm");

// Same codec, RPCL progression order
DicomFile compressed = dicomFile.Transcode(TransferSyntax.HTJ2KLosslessRPCL);
compressed.Save("study-htj2k-rpcl.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="อ่านสิ่งที่ archive ส่งมาให้คุณ">}}

<p>อีกครึ่งหนึ่งของงานคือการรับ HTJ2K จากระบบที่ผลิตมันอยู่แล้ว เปิดไฟล์, ตรวจสอบว่ามันถูกเก็บเป็นอะไร, แล้วทำงานกับข้อมูลพิกเซล.</p>

<div class="codeblock" id="code">
 <h3>อ่านไฟล์ HTJ2K - C#</h3>
 <pre><code class="cs">DicomFile received = DicomFile.Open("study-htj2k.dcm");

TransferSyntax? syntax = received.MetaInfo.TransferSyntax;
Console.WriteLine($"stored as {syntax}");

DicomFile plain = received.Transcode(TransferSyntax.ExplicitVrLittleEndian);
PixelData pixelData = PixelData.Read(plain.Dataset);

Span&lt;byte&gt; firstFrame = pixelData.GetFrame(0);
Console.WriteLine($"{pixelData.NumberOfFrames} frame(s), {firstFrame.Length} bytes in the first one");</code></pre>
</div>

<p>ภาพหลายเฟรมจะถูกจัดการแบบเฟรมต่อเฟรม, ดังนั้นซีรีส์ที่ยาวจะใช้หน่วยความจำต่อเฟรมแทนต่อ study.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="จุดเด่นของ HTJ2K">}}

<ul>
<li>การย้าย archive: บีบอัดใหม่ study ที่จัดเก็บเป็น HTJ2K lossless เพื่อลดขนาดพื้นที่เก็บข้อมูล, รักษาข้อมูลการวินิจฉัยให้คงสภาพ</li>
<li>คลาวด์และ DICOMweb: ความเร็วในการถอดรหัสเป็นสิ่งที่ทำให้ viewer บนเบราว์เซอร์หรือเซิร์ฟเวอร์รู้สึกทันทีเมื่อต้องจัดการภาพขนาดใหญ่</li>
<li>pipeline AI: ชุดฝึกสอนถูกอ่านบ่อยกว่าการเขียนมาก, เวลา decode คือค่าใช้จ่ายที่เกิดซ้ำ</li>
<li>คอนเทนเนอร์และ serverless: codec เป็นส่วนหนึ่งของ assembly, ดังนั้นภาพไม่ต้องการไลบรารี native หรือคอมไพเลอร์ในขั้นตอน build</li>
</ul>

<p>ไลบรารีนี้ยังรวม JPEG XL ซึ่งเป็นการเพิ่มใหม่ล่าสุดในมาตรฐาน, และ codec เก่าที่ archive มักจะมี: JPEG, JPEG‑LS, JPEG 2000 และ RLE. หน้า <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> ครอบคลุมชุดทั้งหมด, และหน้า <a href="/medical/net/jpeg2000/">JPEG 2000</a> ให้รายละเอียดเกี่ยวกับ codec ที่ HTJ2K พัฒนามาจาก.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="คู่มือผู้พัฒนา" href="https://docs.aspose.com/medical/net/developer-guide/transcoding-dicom-file/" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="สนับสนุนผลิตภัณฑ์" tabId="support" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนแบบเสียค่าใช้จ่าย" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้อง Aspose.Medical สำหรับ .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
