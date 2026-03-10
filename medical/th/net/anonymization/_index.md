---
title: ทำให้ไฟล์ DICOM ไม่ระบุตัวตนใน C# .NET | Aspose.Medical
weight: 1000
description: ทำให้ไฟล์ DICOM ไม่ระบุตัวตนใน C# .NET ด้วยโพรไฟล์ความลับที่กำหนดค่าได้ ลบข้อมูลส่วนบุคคลของผู้ป่วย (PII) เพื่อให้สอดคล้องกับ HIPAA และ GDPR ด้วย Aspose.Medical API.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="ทำให้ไฟล์ DICOM ไม่ระบุตัวตนใน .NET C#" h2="ลบข้อมูลระบุตัวตนของผู้ป่วยจากไฟล์ DICOM โดยใช้โพรไฟล์ความลับ DICOM PS 3.15 เพื่อให้สอดคล้องกับ HIPAA และ GDPR ด้วยไลบรารี .NET แท้" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="การทำให้ DICOM ไม่ระบุตัวตนตามมาตรฐาน">}}

<p><strong>Aspose.Medical for .NET</strong> มี API การทำให้ DICOM ไม่ระบุตัวตนที่ครอบคลุม โดยอิงจาก <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Confidentiality Profiles</a> API นี้ช่วยให้ผู้พัฒนาในด้านสุขภาพสามารถลบหรือแก้ไขข้อมูลระบุตัวตนของผู้ป่วย (PII) จากไฟล์ DICOM ในขณะคงคุณค่าทางคลินิกของข้อมูลภาพ ไม่เหมือนวิธีการลบแท็กอย่างง่าย Aspose.Medical ปฏิบัติตามมาตรฐาน DICOM อย่างเป็นทางการเพื่อให้การไม่ระบุตัวตนเป็นไปอย่างเหมาะสม</p>

<p>คลาส <code>Anonymizer</code> ทำงานร่วมกับออบเจ็กต์ <code>ConfidentialityProfile</code> ที่กำหนดวิธีการจัดการแต่ละแอตทริบิวต์ของ DICOM อย่างชัดเจน — ว่าจะต้องลบ แทนที่ด้วยค่า dummy ทำความสะอาด หรือคงไว้ไม่เปลี่ยนแปลง วิธีการนี้รับประกันการทำให้ไม่ระบุตัวตนที่สอดคล้อง ได้รับการตรวจสอบ และสอดคล้องกับข้อกำหนดกฎระเบียบ</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ทำให้ไฟล์ DICOM ไม่ระบุตัวตนใน C#">}}

<p>วิธีที่ง่ายที่สุดในการทำให้ไฟล์ DICOM ไม่ระบุตัวตนคือการใช้โพรไฟล์ความลับค่าเริ่มต้น ซึ่งจะใช้ DICOM PS 3.15 Basic Profile ที่จัดการกับแอตทริบิวต์ระบุตัวผู้ป่วยมาตรฐานทั้งหมด:</p>

<div class="codeblock" id="code">
 <h3>การทำให้ DICOM ไม่ระบุตัวตนขั้นพื้นฐาน - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p>เมธอด <code>Anonymize</code> เป็น <strong>non-destructive</strong> — มันทำสำเนาชุดข้อมูลต้นฉบับและคืนค่าตัวสำเนาที่ทำให้ไม่ระบุตัวตนใหม่ ไฟล์ DICOM ดั้งเดิมจะคงอยู่ไม่เปลี่ยนแปลง ซึ่งเป็นสิ่งสำคัญสำหรับเส้นทางการตรวจสอบและกระบวนการรักษาความสมบูรณ์ของข้อมูล</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การแทนที่ตัวตนผู้ป่วยแบบกำหนดเอง">}}

<p>ในหลายกระบวนการทำงาน คุณอาจต้องการแทนที่ข้อมูลผู้ป่วยด้วยค่าอีลิอสที่กำหนดไว้ แทนการลบออกเท่านั้น คลาส <code>ConfidentialityProfile</code> ให้คุณตั้งค่าการแทนที่สำหรับชื่อผู้ป่วยและรหัสผู้ป่วย:</p>

<div class="codeblock" id="code">
 <h3>ทำให้ไม่ระบุตัวตนด้วยตัวตนผู้ป่วยกำหนดเอง - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create a confidentiality profile with custom replacement values
ConfidentialityProfile profile = new()
{
    PatientName = "ANONYMOUS PATIENT",
    PatientId = "00000000"
};

// Create an anonymizer with this profile
Anonymizer anonymizer = new(profile);

// Anonymize the file
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("custom_anonymized.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การทำให้ไม่ระบุตัวตนแบบในที่เดียว">}}

<p>สำหรับสถานการณ์ที่ความประหยัดหน่วยความจำเป็นสำคัญหรือไม่จำเป็นต้องคงข้อมูลเดิม API นี้ให้การทำให้ไม่ระบุตัวตนแบบในที่เดียวที่แก้ไขชุดข้อมูลโดยตรง:</p>

<div class="codeblock" id="code">
 <h3>การทำให้ DICOM ไม่ระบุตัวตนแบบในที่เดียว - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p>เมธอด <code>Anonymize</code> และ <code>AnonymizeInPlace</code> ทั้งสองรับออบเจ็กต์ <code>Dataset</code> โดยตรง ทำให้คุณควบคุมได้เต็มที่ว่าชุดข้อมูลใดจะถูกประมวลผล</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ตัวเลือกโพรไฟล์ความลับที่กำหนดค่าได้">}}

<p>มาตรฐาน DICOM PS 3.15 กำหนด <strong>Basic Profile</strong> พร้อมกับหลายโพรไฟล์ทางเลือกที่ควบคุมการจัดการประเภทข้อมูลเฉพาะระหว่างการทำให้ไม่ระบุตัวตน คุณสามารถรวมตัวเลือกเหล่านี้โดยใช้ enum <code>ConfidentialityProfileOptions</code>:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>ตัวเลือก</th>
<th>คำอธิบาย</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>โพรไฟล์ความลับระดับแอปพลิเคชันขั้นพื้นฐานของ DICOM PS 3.15 ที่เป็นค่าเริ่มต้น</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>คงแอตทริบิวต์ส่วนตัวที่ปลอดภัยไว้ระหว่างการทำให้ไม่ระบุตัวตน</td></tr>
<tr><td><code>RetainUIDs</code></td><td>คง UID ของ DICOM ดั้งเดิม (Study, Series, Instance) ไม่เปลี่ยนแปลง</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>คงแอตทริบิวต์การระบุตัวอุปกรณ์ (ผู้ผลิต, รุ่น, ชื่อสถานี) ไว้</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>คงแอตทริบิวต์การระบุตัวสถาบัน (ชื่อ, ที่อยู่, หน่วยงาน) ไว้</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>คงลักษณะผู้ป่วย (อายุ, เพศ, ขนาด, น้ำหนัก) เพื่อการวิจัย</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>คงค่าข้อมูลวันที่และเวลาเต็มรูปแบบสำหรับการศึกษาเชิงกาล</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>คงวันที่แก้ไขสำหรับการศึกษาเชิงกาล</td></tr>
<tr><td><code>CleanDesc</code></td><td>ทำความสะอาดคำอธิบายข้อความที่อาจมีข้อมูลระบุตัวตน</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>ทำความสะอาดเนื้อหาที่มีโครงสร้างที่อาจมีข้อมูลระบุตัวตน</td></tr>
<tr><td><code>CleanGraph</code></td><td>ทำความสะอาดข้อมูลกราฟิกที่อาจมีข้อมูลผู้ป่วยฝังอยู่</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>ทำให้ไม่ระบุตัวตนด้วยตัวเลือกโพรไฟล์เฉพาะ - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="รหัสการกระทำสำหรับการจัดการแอตทริบิวต์">}}

<p>แต่ละแอตทริบิวต์ในโพรไฟล์ความลับจะถูกกำหนดรหัสการกระทำที่กำหนดวิธีการประมวลผลระหว่างการทำให้ไม่ระบุตัวตน รหัสเหล่านี้สอดคล้องกับมาตรฐาน DICOM PS 3.15 Table E.1-1a:</p>

<table class="table table-bordered">
<thead>
<tr>
<th>รหัส</th>
<th>การกระทำ</th>
<th>คำอธิบาย</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>แทนที่ด้วย dummy</td><td>แทนที่ด้วยค่าที่มีความยาวไม่เป็นศูนย์และสอดคล้องกับ VR</td></tr>
<tr><td><strong>Z</strong></td><td>ความยาวศูนย์หรือ dummy</td><td>แทนที่ด้วยค่าความยาวศูนย์หรือค่า dummy ที่สอดคล้องกับ VR</td></tr>
<tr><td><strong>X</strong></td><td>ลบ</td><td>ลบแอตทริบิวต์ออกจากชุดข้อมูลโดยสมบูรณ์</td></tr>
<tr><td><strong>K</strong></td><td>คงไว้</td><td>คงแอตทริบิวต์โดยไม่เปลี่ยนแปลง (สำหรับที่ไม่เป็นลำดับ) หรือทำความสะอาด (สำหรับลำดับ)</td></tr>
<tr><td><strong>C</strong></td><td>ทำความสะอาด</td><td>แทนที่ด้วยค่าที่ทำความสะอาดแล้วโดยมีความหมายคล้ายและสอดคล้องกับ VR</td></tr>
<tr><td><strong>U</strong></td><td>แทนที่ UID</td><td>แทนที่ด้วย UID ใหม่ที่สอดคล้องภายในชุดของอินสแทนซ์</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="โพรไฟล์กำหนดเองจาก CSV, JSON, และ XML">}}

<p>สำหรับการใช้งานขั้นสูง คุณสามารถกำหนดโพรไฟล์การทำให้ไม่ระบุตัวตนแบบกำหนดเองโดยโหลดกฎจากไฟล์ CSV, JSON หรือ XML ภายนอก ซึ่งช่วยให้คุณปรับพฤติกรรมการทำให้ไม่ระบุตัวตนให้สอดคล้องกับนโยบายองค์กรหรือข้อกำหนดกฎระเบียบของคุณ</p>

<div class="codeblock" id="code">
 <h3>โหลดโพรไฟล์การทำให้ไม่ระบุตัวตนจาก CSV - C#</h3>
 <pre><code class="cs">// CSV format: TagPattern;Action
// 0010,0010;Z   (Anonymize PatientName)
// 0010,0020;D   (Replace PatientID with dummy)
// 0020,000D;U   (Replace StudyInstanceUID)

var profile = ConfidentialityProfile.LoadFromCsvFile(
    "anonymization_rules.csv",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>โหลดโพรไฟล์การทำให้ไม่ระบุตัวตนจาก JSON - C#</h3>
 <pre><code class="cs">// JSON format:
// [
//     { "Tag": "0010,0010", "Action": "Z" },
//     { "Tag": "0010,0020", "Action": "D" },
//     { "Tag": "0020,000D", "Action": "U" }
// ]

var profile = ConfidentialityProfile.LoadFromJsonFile(
    "anonymization_rules.json",
    ConfidentialityProfileOptions.BasicProfile);

var anonymizer = new Anonymizer(profile);</code></pre>
</div>

<p>โพรไฟล์กำหนดเองยังสามารถโหลดจากไฟล์ XML (<code>LoadFromXmlFile</code>) สตรีม (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>) หรือโดยตรงจากสตริง (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>)</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="การปฏิบัติตาม HIPAA และ GDPR">}}

<p>ไฟล์ DICOM มักมีข้อมูลสุขภาพที่คุ้มครอง (PHI) ตามที่กำหนดโดย HIPAA และข้อมูลส่วนบุคคลตามที่กำหนดโดย GDPR Aspose.Medical for .NET ช่วยให้คุณปฏิบัติตามข้อกำหนดกฎระเบียบโดยให้:</p>

<ul>
<li><strong>การปฏิบัติตาม DICOM PS 3.15</strong>: เครื่องมือทำให้ไม่ระบุตัวตนปฏิบัติตามมาตรฐาน DICOM อย่างเป็นทางการสำหรับการลบข้อมูลระบุตัวตน เพื่อให้แน่ใจว่ามาตรฐานปฏิบัติที่ดีที่สุดที่รับรู้ได้ถูกนำไปใช้กับแอตทริบิวต์ที่เกี่ยวข้องทั้งหมด</li>
<li><strong>การลบ PII อย่างครอบคลุม</strong>: ชื่อผู้ป่วย, ID, วันเดือนปีเกิด, ที่อยู่ และตัวระบุอื่น ๆ จะถูกจัดการตามโพรไฟล์ความลับที่กำหนดค่าไว้</li>
<li><strong>การแทนที่ UID</strong>: UID ของ Study, Series และ SOP Instance สามารถแทนที่ด้วย UID ใหม่ที่สอดคล้องภายในเพื่อป้องกันการระบุตัวตนซ้ำผ่านการอ้างอิงข้าม</li>
<li><strong>การคงข้อมูลที่กำหนดค่าได้</strong>: เมื่อการวิจัยหรือความต้องการทางคลินิกต้องการ สามารถคงข้อมูลประเภทเฉพาะ (ลักษณะผู้ป่วย, วันที่, ข้อมูลอุปกรณ์) ได้อย่างเลือกสรรโดยใช้โพรไฟล์ตัวเลือกมาตรฐาน</li>
<li><strong>กระบวนการที่ตรวจสอบได้</strong>: วิธีการที่อิงมาตรฐานพร้อมรหัสการกระทำที่กำหนด ทำให้มีขั้นตอนการทำให้ไม่ระบุตัวตนที่ชัดเจนและสามารถบันทึกเป็นเอกสารสำหรับการตรวจสอบตามกฎระเบียบ</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="แหล่งเรียนรู้" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="เอกสารประกอบ" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="ซอร์สโค้ด" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="อ้างอิง API" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="การสนับสนุนสินค้า" tabId="support" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนฟรี" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="สนับสนุนแบบจ่ายเงิน" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="บล็อก" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="ทำไมต้องเลือก Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="รายชื่อลูกค้า" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="เรื่องราวความสำเร็จ" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
