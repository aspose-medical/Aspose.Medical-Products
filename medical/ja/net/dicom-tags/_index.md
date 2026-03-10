---
title: C# .NET で DICOM タグの読み取りと変更 | Aspose.Medical
weight: 15000
description: C# .NET で DICOM タグを読み取り、追加、更新、削除します。タグのメタデータにアクセスし、プライベートタグを操作し、グループ単位で列挙し、Aspose.Medical API を使用してシーケンスを操作します。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# で DICOM タグの読み取りと変更" h2="型安全な API で DICOM データ要素にアクセス、追加、更新、削除します。標準タグ、プライベートタグ、シーケンス、完全な DICOM タグ辞書を操作できます。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="型安全な DICOM タグアクセス">}}

<p><strong>Aspose.Medical for .NET</strong> は、<code>Dataset</code> クラスを通じて DICOM タグを操作するための包括的で型安全な API を提供します。すべての DICOM データ要素は <code>Tag</code> &mdash; 属性を一意に識別するグループ番号と要素番号のペア – によって表されます。API はジェネリックメソッドを用いた強く型付けされたアクセス、例外を回避する安全な取得パターン、単一値、配列、インデックスアクセスをサポートします。</p>

<p>一般的な DICOM タグは <code>Tag</code> クラスの静的フィールドとして利用でき（例: <code>Tag.PatientName</code>、<code>Tag.StudyInstanceUID</code>）、数値タグコードを覚える必要がなくなります。各タグは <code>TagMetadata</code> を持ち、キーワード、説明、Value Representation (VR)、値の多重度、廃止ステータスが含まれます。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# で DICOM タグを読み取る">}}

<p><code>Dataset</code> クラスは、目的に応じてタグ値を取得するための複数のメソッドを提供します &mdash; 単一値、すべての値、インデックスアクセス、またはデフォルト値付きの安全な取得：</p>

<div class="codeblock" id="code">
 <h3>C# で DICOM タグ値を読み取る</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM タグの追加と更新">}}

<p>タグ値を設定するには <code>AddOrUpdate</code> を使用します。このメソッドはタグが存在しない場合は要素を作成し、既に存在する場合はその値を置き換えます。単一値、配列、または完全に型付けされた要素を設定できます。</p>

<div class="codeblock" id="code">
 <h3>C# で DICOM タグを追加および更新</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM タグの削除">}}

<p><code>Remove</code> メソッドは、単一タグの削除、複数タグの同時削除、または述語に一致するタグの削除をサポートします &mdash; 要素の全グループを除去するのに便利です。</p>

<div class="codeblock" id="code">
 <h3>C# で DICOM タグを削除</h3>
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

{{< blocks/products/pf/feature-page-section h2="グループ単位でタグを列挙">}}

<p>DICOM タグはグループに整理されています &mdash; 例えば、グループ <code>0x0010</code> は患者情報、グループ <code>0x0008</code> は検査識別、グループ <code>0x0028</code> は画像ピクセル属性を含みます。特定のグループ内のすべての要素を走査するには <code>EnumerateGroup</code> を使用します。</p>

<div class="codeblock" id="code">
 <h3>C# でグループ単位の要素を列挙</h3>
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

{{< blocks/products/pf/feature-page-section h2="要素レベルのデータ操作">}}

<p>各 DICOM 要素は、要素内の個別の値を読み取り、追加、挿入、削除、置換するメソッドを提供します。これは、値リストを細かく制御する必要がある多値タグに有用です。</p>

<div class="codeblock" id="code">
 <h3>C# で要素値を操作</h3>
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

{{< blocks/products/pf/feature-page-section h2="タグ辞書とメタデータ">}}

<p><code>TagDictionary</code> クラスは、既知の DICOM タグとそのメタデータ（キーワード、説明、Value Representation、値の多重度、廃止ステータス）を登録したレジストリを提供します。キーワードでタグを検索したり、プログラムでメタデータを調べることができます。</p>

<div class="codeblock" id="code">
 <h3>C# でタグ辞書を照会</h3>
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

{{< blocks/products/pf/feature-page-section h2="プライベートタグのサポート">}}

<p>DICOM はベンダーや組織が独自データ用にプライベートタグを定義することを許可しています。Aspose.Medical for .NET はプライベートタグの読み取り、書き込み、登録を完全にサポートします。XML ファイルからカスタムタグ辞書をロードして、プライベート要素を適切に処理できます。</p>

<div class="codeblock" id="code">
 <h3>C#でプライベートタグを操作</h3>
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

{{< blocks/products/pf/feature-page-section h2="DICOM シーケンスの操作">}}

<p>DICOM シーケンス（VR タイプ SQ）は入れ子になったデータセットを含み、DICOM ファイル内にツリー構造を形成します。<code>Sequence</code> クラスはシーケンスアイテムへの型付きアクセスを提供します。</p>

<div class="codeblock" id="code">
 <h3>C#で DICOM シーケンスを読み取り・作成</h3>
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

{{< blocks/products/pf/feature-page-section h2="一般的な DICOM タググループ">}}

<p>以下の表は、頻繁に使用される DICOM タググループと、<code>Tag</code> クラスでアクセス可能な例示タグを示します。</p>

<table class="table table-bordered">
<thead>
<tr>
<th>グループ</th>
<th>カテゴリ</th>
<th>例示タグ</th>
</tr>
</thead>
<tbody>
<tr><td><code>0002</code></td><td>ファイルメタ情報</td><td>TransferSyntaxUID, MediaStorageSOPClassUID</td></tr>
<tr><td><code>0008</code></td><td>検査識別</td><td>StudyDate, Modality, InstitutionName, ReferringPhysicianName</td></tr>
<tr><td><code>0010</code></td><td>患者情報</td><td>PatientName, PatientID, PatientBirthDate, PatientSex, PatientAge</td></tr>
<tr><td><code>0018</code></td><td>取得パラメータ</td><td>KVP, ExposureTime, SliceThickness, RepetitionTime</td></tr>
<tr><td><code>0020</code></td><td>画像位置決め</td><td>StudyInstanceUID, SeriesInstanceUID, ImagePositionPatient</td></tr>
<tr><td><code>0028</code></td><td>画像ピクセル属性</td><td>Rows, Columns, BitsAllocated, PixelSpacing, WindowCenter</td></tr>
<tr><td><code>7FE0</code></td><td>ピクセルデータ</td><td>PixelData</td></tr>
</tbody>
</table>

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
