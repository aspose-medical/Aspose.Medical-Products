---
title: C# .NET で DICOM を XML に変換 | Aspose.Medical
weight: 3000
description: C# .NET で DICOM データセットを標準の DICOM XML フォーマットにシリアライズします。Aspose.Medical API を使用して Bulk データの取り扱い、ストリームベースの処理、非同期操作を構成できます。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# で DICOM を XML に変換" h2="標準の DICOM XML 表現（PS3.19）に DICOM データセットをシリアライズします。純粋な .NET ライブラリを使用して Bulk データ参照、ストリームベースの出力、非同期処理を構成できます。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="標準準拠の DICOM XML シリアライズ">}}

<p><strong>Aspose.Medical for .NET</strong> は <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">DICOM PS3.19 Native DICOM Model</a> に従って DICOM データを XML にシリアライズします。これは XML で DICOM データセットを表現する公式標準であり、DICOMweb サービス、統合プラットフォーム、医療画像メタデータの人間可読かつスキーマ検証された表現が必要なシステムで使用されます。</p>

<p><code>DicomXmlSerializer</code> クラスはシリアライズとデシリアライズの両方の静的メソッドを提供します。単純なタグダンプ方式とは異なり、出力は各要素がタグ、VR、適切にフォーマットされた値で表現される DICOM XML スキーマに準拠しており、バイナリ DICOM と XML 間のロスレスな往復変換を実現します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# で DICOM を XML にシリアライズ">}}

<p><code>DicomXmlSerializer</code> クラスを使用して DICOM データセットを XML 文字列に変換します。最も簡単な方法は標準準拠の XML ドキュメントを生成します。</p>

<div class="codeblock" id="code">
 <h3>DICOM を XML に変換 - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to XML string
string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset);

// Write to file
File.WriteAllText("output.xml", xml);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ストリームベースおよび非同期シリアライズ">}}

<p>大きな DICOM ファイルや高スループットのシナリオでは、メモリに大きな文字列を確保せずにストリームへ直接シリアライズします。同期メソッドと非同期メソッドの両方が利用可能です。</p>

<div class="codeblock" id="code">
 <h3>同期ストリームシリアライズ - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Write XML directly to a file stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    DicomXmlSerializer.Serialize(stream, dicomFile.Dataset);
}</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>非同期ストリームシリアライズ - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Async serialization to string
string xml = await DicomXmlSerializer.SerializeAsync(dicomFile.Dataset);

// Async serialization to stream
using (var stream = new FileStream("output.xml", FileMode.Create))
{
    await DicomXmlSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="大規模スタディのパイプラインストリーミング">}}

<p>全体のスタディをメモリに保持する必要はありません。<code>DicomXmlSerializer</code> は <code>PipeWriter</code> に書き込み、<code>PipeReader</code> から読み取ります。そのため XML はストリームしながら生成・消費でき、データセットのシーケンスは <code>DeserializeAsyncEnumerable</code> を通じて一つずつ読み取れます。すべてのメソッドは <code>CancellationToken</code> を受け取ります。</p>

<div class="codeblock" id="code">
 <h3>パイプを介したシリアライズとデシリアライズ - C#</h3>
 <pre><code class="cs">// Write XML straight into a pipe, with no intermediate string
DicomFile dicomFile = DicomFile.Open("large_study.dcm");
await using var output = File.Create("output.xml");
await DicomXmlSerializer.SerializeAsync(PipeWriter.Create(output), dicomFile.Dataset);

// Read a dataset back from a pipe
await using var input = File.OpenRead("output.xml");
Dataset dataset = await DicomXmlSerializer.DeserializeAsync(PipeReader.Create(input));</code></pre>
</div>

<div class="codeblock" id="code">
 <h3>データセットのシーケンスを1つずつ読む - C#</h3>
 <pre><code class="cs">// Each dataset is materialized only while it is being processed
await using var stream = File.OpenRead("study_sequence.xml");

await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    string sopInstanceUid = dataset.GetValue&lt;string&gt;(Tag.SOPInstanceUID, 0);
    Console.WriteLine(sopInstanceUid);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="シリアライズオプション">}}

<p><code>DicomXmlSerializerOptions</code> クラスは DICOM データが XML でどのように表現されるかを制御します。主な設定項目は大きなバイナリ値の Bulk データ処理です。</p>

<table class="table table-bordered">
<thead>
<tr>
<th>プロパティ</th>
<th>型</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>大容量データ（例：ピクセルデータ）をインラインせず BulkData URI 参照として書き込むためのカスタムコンバータ</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>デシリアライズ時に BulkData URI を解決するためのカスタムローダー</td></tr>
<tr><td><code>Default</code></td><td><code>DicomXmlSerializerOptions</code></td><td>カスタムオプションが提供されない場合に使用されるデフォルトオプションインスタンス</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>カスタムオプションでシリアライズ - C#</h3>
 <pre><code class="cs">var options = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};

string xml = DicomXmlSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk データの取り扱い">}}

<p>大容量バイナリ値（ピクセルデータ、波形、カプセル化文書）を XML 出力にインラインせず、BulkData URI 参照として外部化できます。これは <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html#table_A.1.5-2">DICOM PS3.19 BulkData 要素</a> 仕様に準拠しています。</p>

<p>シリアライズ時に大容量データを外部化するには <code>IBulkDataConverter</code> を実装し、デシリアライズ時に URI を解決するには <code>IBulkDataLoader</code> を実装します。一般的なケースではローダーを自作する必要はありません。<code>DefaultBulkDataLoader.Instance</code> は <code>file</code>、<code>http</code>、<code>https</code> URI を解決し、さらに <code>IAsyncBulkDataLoader</code> を実装しているため、ストリーミングパス上で Bulk データが非同期に取得されます。</p>

<div class="codeblock" id="code">
 <h3>カスタム Bulk データ処理 - C#</h3>
 <pre><code class="cs">// Converter: externalizes pixel data as BulkData URIs
public class FileBulkDataConverter : IBulkDataConverter
{
    public string? GetBulkDataUri(IElement element)
    {
        // Externalize pixel data to a separate file
        if (element.Tag == Tag.PixelData)
            return "file:///bulk/pixeldata.raw";
        return null; // inline all other elements
    }
}

// Loader: resolves BulkData URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public Span&lt;byte&gt; GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomXmlSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string xml = DicomXmlSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomXmlSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset dataset = DicomXmlSerializer.Deserialize(xml, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="XML を DICOM にデシリアライズ">}}

<p>DICOM XML をデータセットオブジェクトにパースします。文字列入力、ストリーム入力、非同期操作をサポートします。</p>

<div class="codeblock" id="code">
 <h3>XML を DICOM にデシリアライズ - C#</h3>
 <pre><code class="cs">// Deserialize from XML string
string xmlText = File.ReadAllText("dicom_data.xml");
Dataset dataset = DicomXmlSerializer.Deserialize(xmlText);

// Deserialize from stream
using var stream = File.OpenRead("dicom_data.xml");
Dataset fromStream = DicomXmlSerializer.Deserialize(stream);

// Async deserialization from string
Dataset asyncResult = await DicomXmlSerializer.DeserializeAsync(xmlText);

// Async deserialization from stream
await using var asyncStream = File.OpenRead("dicom_data.xml");
Dataset asyncFromStream = await DicomXmlSerializer.DeserializeAsync(asyncStream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="XML と JSON のシリアライズ比較">}}

<p>Aspose.Medical は DICOM XML（PS3.19）と DICOM JSON（PS3.18）の両方のシリアライズをサポートします。どちらの形式もロスレスな往復変換を提供しますが、適用シナリオは異なります。</p>

<table class="table table-bordered">
<thead>
<tr>
<th>機能</th>
<th>DICOM XML</th>
<th>DICOM JSON</th>
</tr>
</thead>
<tbody>
<tr><td>標準</td><td>PS3.19 (Native DICOM Model)</td><td>PS3.18 (DICOM JSON Model)</td></tr>
<tr><td>スキーマ検証</td><td>XML スキーマ (XSD) が利用可能</td><td>正式なスキーマなし</td></tr>
<tr><td>適した用途</td><td>エンタープライズ統合、HL7 CDA、監査ログ、XDS レジストリ</td><td>DICOMweb、REST API、FHIR ImagingStudy</td></tr>
<tr><td>人間可読性</td><td>冗長だが自己記述的</td><td>コンパクトで広くサポート</td></tr>
<tr><td>Bulk データ</td><td>URI を持つ BulkData 要素</td><td>BulkDataURI プロパティ</td></tr>
<tr><td>シリアライザークラス</td><td><code>DicomXmlSerializer</code></td><td><code>DicomJsonSerializer</code></td></tr>
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

{{< blocks/products/pf/slr-tab tabTitle=".NET 向け Aspose.Medical を選ぶ理由" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="導入実績" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
