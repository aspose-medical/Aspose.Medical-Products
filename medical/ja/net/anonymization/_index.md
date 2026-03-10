---
title: C# .NET で DICOM ファイルを匿名化 | Aspose.Medical
weight: 1000
description: 構成可能な機密性プロファイルを使用して C# .NET で DICOM ファイルを匿名化します。患者の個人情報（PII）を削除し、Aspose.Medical API により HIPAA および GDPR への準拠を確保します。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#で DICOM ファイルを匿名化" h2="DICOM PS 3.15 機密性プロファイルを使用して DICOM ファイルから患者の識別情報を削除します。純粋な .NET ライブラリで HIPAA および GDPR への準拠を確保します。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="標準ベースの DICOM 匿名化">}}

<p><strong>Aspose.Medical for .NET</strong> は、<a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html">DICOM PS 3.15 Confidentiality Profiles</a> に基づく包括的な DICOM 匿名化 API を提供します。この API により、医療開発者は画像データの臨床的価値を保持しつつ、DICOM ファイルから患者の識別情報（PII）を削除または変更できます。単純なタグ削除アプローチとは異なり、Aspose.Medical は公式 DICOM 標準に従い、適切な匿名化を保証します。</p>

<p><code>Anonymizer</code> クラスは、各 DICOM 属性の処理方法（削除、ダミー値への置換、クリーニング、変更なしの保持）を正確に定義した <code>ConfidentialityProfile</code> オブジェクトと連携して動作します。このアプローチにより、規制要件を満たす一貫性のある監査可能な匿名化が保証されます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# で DICOM ファイルを匿名化する">}}

<p>DICOM ファイルを匿名化する最も簡単な方法は、デフォルトの機密性プロファイルを使用することです。これにより DICOM PS 3.15 Basic Profile が適用され、標準的な患者識別属性すべてが処理されます：</p>

<div class="codeblock" id="code">
 <h3>基本的な DICOM 匿名化 - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Anonymizer anonymizer = new();

// Anonymize the DICOM file (non-destructive - returns a new file)
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");</code></pre>
</div>

<p><code>Anonymize</code> メソッドは <strong>破壊的でない</strong> &mdash; 元のデータセットをクローンし、新しい匿名化コピーを返します。元の DICOM ファイルは変更されず、監査トレイルやデータ完全性のワークフローに不可欠です。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="カスタム患者識別情報の置換">}}

<p>多くのワークフローでは、患者情報を単に削除するのではなく、特定の別名値に置き換える必要があります。<code>ConfidentialityProfile</code> クラスを使用すると、患者名と患者 ID の置換値を設定できます：</p>

<div class="codeblock" id="code">
 <h3>カスタム患者識別情報で匿名化 - C#</h3>
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

{{< blocks/products/pf/feature-page-section h2="インプレイス匿名化">}}

<p>メモリ効率が重要で、元データを保持する必要がないシナリオでは、API はデータセットを直接変更するインプレイス匿名化を提供します：</p>

<div class="codeblock" id="code">
 <h3>インプレイス DICOM 匿名化 - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Anonymizer anonymizer = new();

// Perform in-place anonymization (modifies the original dataset)
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");</code></pre>
</div>

<p><code>Anonymize</code> と <code>AnonymizeInPlace</code> の両メソッドは、<code>Dataset</code> オブジェクトを直接受け取ることもでき、処理対象のデータセットを完全に制御できます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="構成可能な機密性プロファイルオプション">}}

<p>DICOM PS 3.15 標準は、匿名化時に特定のデータカテゴリの取り扱いを制御する <strong>Basic Profile</strong> と複数のオプションプロファイルを定義しています。これらのオプションは <code>ConfidentialityProfileOptions</code> 列挙体を使用して組み合わせることができます：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>オプション</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr><td><code>BasicProfile</code></td><td>既定の DICOM PS 3.15 Basic Application Level 機密性プロファイル</td></tr>
<tr><td><code>RetainSafePrivate</code></td><td>匿名化中に安全なプライベート属性を保持する</td></tr>
<tr><td><code>RetainUIDs</code></td><td>元の DICOM UID（Study、Series、Instance）を変更せずに保持する</td></tr>
<tr><td><code>RetainDeviceIdent</code></td><td>デバイス識別属性（メーカー、モデル、ステーション名）を保持する</td></tr>
<tr><td><code>RetainInstitutionIdent</code></td><td>機関識別属性（名前、住所、部門）を保持する</td></tr>
<tr><td><code>RetainPatientChars</code></td><td>研究目的で患者特性（年齢、性別、サイズ、体重）を保持する</td></tr>
<tr><td><code>RetainLongFullDates</code></td><td>縦断的研究のために完全な日付と時刻の値を保持する</td></tr>
<tr><td><code>RetainLongModifDates</code></td><td>縦断的研究のために修正日付を保持する</td></tr>
<tr><td><code>CleanDesc</code></td><td>識別情報を含む可能性のあるテキスト記述をクリーンアップする</td></tr>
<tr><td><code>CleanStructdCont</code></td><td>識別情報を含む可能性のある構造化コンテンツをクリーンアップする</td></tr>
<tr><td><code>CleanGraph</code></td><td>埋め込み患者情報を含む可能性のあるグラフィカルデータをクリーンアップする</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>特定のプロファイルオプションで匿名化 - C#</h3>
 <pre><code class="cs">// Create a profile that retains patient characteristics and UIDs for research
var options = ConfidentialityProfileOptions.BasicProfile
    | ConfidentialityProfileOptions.RetainPatientChars
    | ConfidentialityProfileOptions.RetainUIDs;

var profile = ConfidentialityProfile.CreateDefault(options);
var anonymizer = new Anonymizer(profile);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="属性処理のアクションコード">}}

<p>機密性プロファイル内の各属性には、匿名化時の処理方法を定義するアクションコードが割り当てられます。これらは DICOM PS 3.15 Table E.1-1a 標準に従います：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>コード</th>
<th>アクション</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr><td><strong>D</strong></td><td>ダミーで置換</td><td>VR に一致した長さがゼロでない値に置換する</td></tr>
<tr><td><strong>Z</strong></td><td>長さゼロまたはダミー</td><td>VR に一致した長さゼロの値またはダミー値に置換する</td></tr>
<tr><td><strong>X</strong></td><td>削除</td><td>属性をデータセットから完全に削除する</td></tr>
<tr><td><strong>K</strong></td><td>保持</td><td>属性を変更せずに保持（シーケンスでない場合）またはクリーン（シーケンスの場合）</td></tr>
<tr><td><strong>C</strong></td><td>クリーン</td><td>VR に一致した類似の意味を持つクリーンな値に置換する</td></tr>
<tr><td><strong>U</strong></td><td>UID を置換</td><td>インスタンス集合内で内部的一貫性を保つ新しい UID に置換する</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="CSV、JSON、XML からのカスタムプロファイル">}}

<p>高度なユースケースでは、外部の CSV、JSON、XML ファイルからルールを読み込んでカスタム匿名化プロファイルを定義できます。これにより、組織のポリシーや規制要件に合わせて匿名化動作を調整できます：</p>

<div class="codeblock" id="code">
 <h3>CSV から匿名化プロファイルをロード - C#</h3>
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
 <h3>JSON から匿名化プロファイルをロード - C#</h3>
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

<p>カスタムプロファイルは、XML ファイル (<code>LoadFromXmlFile</code>)、ストリーム (<code>LoadFromJsonStream</code>, <code>LoadFromXmlStream</code>)、または文字列 (<code>LoadFromCsvString</code>, <code>LoadFromJsonString</code>, <code>LoadFromXmlString</code>) から直接ロードすることもできます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="HIPAA および GDPR コンプライアンス">}}

<p>DICOM ファイルは、HIPAA に定義される保護対象医療情報（PHI）や GDPR に定義される個人データを日常的に含んでいます。Aspose.Medical for .NET は、以下を提供することで規制要件の遵守を支援します：</p>

<ul>
<li><strong>DICOM PS 3.15 コンプライアンス</strong>: 匿名化エンジンは公式 DICOM 標準に従い、すべての関連属性に認められたベストプラクティスを適用します。</li>
<li><strong>包括的な PII 削除</strong>: 患者名、ID、生年月日、住所、その他の識別子は、構成された機密性プロファイルに従って処理されます。</li>
<li><strong>UID 置換</strong>: Study、Series、SOP Instance の UID は、相互参照による再識別を防止するために、新しく内部的一貫性のある UID に置換できます。</li>
<li><strong>構成可能な保持</strong>: 研究や臨床上の必要がある場合、標準オプションプロファイルを使用して特定のデータカテゴリ（患者特性、日付、デバイス情報）を選択的に保持できます。</li>
<li><strong>監査可能なプロセス</strong>: 定義されたアクションコードを用いた標準ベースのアプローチにより、規制監査向けの明確で文書化可能な匿名化プロセスが提供されます。</li>
</ul>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学習リソース" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="ドキュメント" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="ソースコード" href="https://github.com/aspose-medical/Aspose.Medical-for-.NET" >}}
{{< blocks/products/pf/slr-element name="API リファレンス" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="製品サポート" tabId="support" >}}
{{< blocks/products/pf/slr-element name="無料サポート" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="有料サポート" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ブログ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="なぜ Aspose.Medical for .NET なのか？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="顧客一覧" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
