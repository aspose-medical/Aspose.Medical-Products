---
title: C# .NET で DICOM を JSON に変換 | Aspose.Medical
weight: 4000
description: C# .NET で DICOM ファイルを標準 DICOM JSON 形式にシリアライズします。Aspose.Medical API を使用して数値処理、バルクデータ URI、キーワード出力、非同期ストリーム処理を構成できます。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# で DICOM を JSON に変換" h2="DICOM データセットおよびファイルを標準 DICOM JSON Model（PS3.18）にシリアライズします。数値処理、バルクデータ参照、キーワード出力、ストリームベースの非同期処理を構成できます。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="標準 DICOM JSON Model">}}

<p><strong>Aspose.Medical for .NET</strong> は、<a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a> に従って DICOM データを JSON にシリアライズします。これは DICOM データセットを JSON で表現する公式標準であり、DICOMweb サービス、FHIR ImagingStudy リソース、最新のヘルスケア API で使用されています。</p>

<p>DICOM JSON Model では、各属性はタグを JSON キーとして表現されます（例: 患者名の場合は <code>"00100010"</code>）。Value Representation と値配列はネストされたプロパティとして記述されます：</p>

<div class="codeblock" id="code">
 <h3>DICOM JSON Model の形式</h3>
 <pre><code class="json">{
    "00100010": {
        "vr": "PN",
        "Value": [{ "Alphabetic": "Doe^John" }]
    },
    "00100020": {
        "vr": "LO",
        "Value": ["PATIENT-001"]
    },
    "00080060": {
        "vr": "CS",
        "Value": ["CT"]
    }
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C# で DICOM を JSON にシリアライズ">}}

<p><code>DicomJsonSerializer</code> クラスを使用して DICOM ファイルまたはデータセットを JSON に変換します。最もシンプルな方法で、コンパクトかつ標準準拠の出力が得られます：</p>

<div class="codeblock" id="code">
 <h3>DICOM を JSON に変換 - C#</h3>
 <pre><code class="cs">// Load a DICOM file
DicomFile dicomFile = DicomFile.Open("input.dcm");

// Serialize dataset to JSON string (compact format)
string json = DicomJsonSerializer.Serialize(dicomFile.Dataset);

// Serialize with pretty-printing for readability
string prettyJson = DicomJsonSerializer.Serialize(
    dicomFile.Dataset, writeIndented: true);

// Write directly to file
File.WriteAllText("output.json", prettyJson);</code></pre>
</div>

<p><code>Dataset</code>、完全な <code>DicomFile</code>（File Meta Information を含む）、またはデータセットの配列をシリアライズできます：</p>

<div class="codeblock" id="code">
 <h3>DicomFile とデータセット配列のシリアライズ - C#</h3>
 <pre><code class="cs">// Serialize full DicomFile (includes File Meta Information)
string fileJson = DicomJsonSerializer.Serialize(dicomFile);

// Serialize multiple datasets as a JSON array
Dataset[] datasets = { dicom1.Dataset, dicom2.Dataset, dicom3.Dataset };
string arrayJson = DicomJsonSerializer.Serialize(datasets, writeIndented: true);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ストリームベースおよび非同期シリアライズ">}}

<p>大きな DICOM ファイルや高スループットシナリオでは、メモリ内に大きな文字列を確保せずに UTF-8 ストリームへ直接シリアライズします。同期方式と非同期方式の両方が利用可能です：</p>

<div class="codeblock" id="code">
 <h3>ストリームと非同期シリアライズ - C#</h3>
 <pre><code class="cs">DicomFile dicomFile = DicomFile.Open("large_study.dcm");

// Synchronous stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    DicomJsonSerializer.Serialize(stream, dicomFile.Dataset);
}

// Async stream serialization
using (var stream = new FileStream("output.json", FileMode.Create))
{
    await DicomJsonSerializer.SerializeAsync(stream, dicomFile.Dataset);
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="シリアライズオプション">}}

<p><code>DicomJsonSerializerOptions</code> クラスは、DICOM データが JSON でどのように表現されるかを制御します：</p>

<table class="table table-bordered">
<thead>
<tr>
<th>プロパティ</th>
<th>型</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr><td><code>NumberHandling</code></td><td><code>DicomJsonNumberHandling</code></td><td>数値 VR（IS、DS、SV、UV）を JSON の数値として書き出すか文字列として書き出すかを制御します</td></tr>
<tr><td><code>UseKeywordsAsJsonKeys</code></td><td><code>bool</code></td><td>DICOM キーワード（例: <code>"PatientName"</code>）を JSON キーとして使用し、タグ（<code>"00100010"</code>）の代わりに利用します。注: 標準外です</td></tr>
<tr><td><code>WriteKeyword</code></td><td><code>bool</code></td><td>DICOM キーワードを追加の JSON 属性として含めます</td></tr>
<tr><td><code>WriteName</code></td><td><code>bool</code></td><td>DICOM タグ名を追加の JSON 属性として含めます</td></tr>
<tr><td><code>BulkDataConverter</code></td><td><code>IBulkDataConverter</code></td><td>大きなデータ（例: ピクセルデータ）を URI 参照として書き出すためのカスタムコンバータ</td></tr>
<tr><td><code>BulkDataLoader</code></td><td><code>IBulkDataLoader</code></td><td>デシリアライズ時にバルクデータ URI を解決するためのカスタムローダー</td></tr>
</tbody>
</table>

<div class="codeblock" id="code">
 <h3>シリアライズオプションの構成 - C#</h3>
 <pre><code class="cs">var options = new DicomJsonSerializerOptions
{
    // Write numbers as JSON numbers (default) or strings
    NumberHandling = DicomJsonNumberHandling.AsNumber,

    // Include keyword and name metadata (non-standard extensions)
    WriteKeyword = true,
    WriteName = true
};

string json = DicomJsonSerializer.Serialize(dicomFile.Dataset, options);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="数値処理">}}

<p>DICOM の数値 Value Representation（IS、DS、SV、UV）は JSON の数値または文字列としてシリアライズできます。これは相互運用性に重要で、一部の DICOM 実装は前方スペースや非標準の書式で数値を格納しているためです。</p>

<table class="table table-bordered">
<thead>
<tr>
<th>モード</th>
<th>JSON 出力</th>
<th>利用例</th>
</tr>
</thead>
<tbody>
<tr><td><code>AsNumber</code></td><td><code>"00180086":{"vr":"IS","Value":[42]}</code></td><td>標準準拠で、計算に最適です。解析できない場合は例外がスローされます。</td></tr>
<tr><td><code>AsString</code></td><td><code>"00180086":{"vr":"IS","Value":["42"]}</code></td><td>元の文字列表現を保持します。非標準データの往復変換に安全です。</td></tr>
</tbody>
</table>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="バルクデータ処理">}}

<p>大規模なバイナリ値（ピクセルデータ、波形、エンカプセル化ドキュメント）は、JSON 出力にインラインせずバルクデータ参照として処理できます。これは <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/sect_A.html">DICOM PS3.18 Bulk Data</a> 仕様に従います。</p>

<p>シリアライズ時に大きなデータを外部化するには <code>IBulkDataConverter</code> を実装し、デシリアライズ時に URI を解決するには <code>IBulkDataLoader</code> を実装します。</p>

<div class="codeblock" id="code">
 <h3>カスタムバルクデータ処理 - C#</h3>
 <pre><code class="cs">// Custom converter: externalizes pixel data as bulk data URIs
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

// Custom loader: resolves bulk data URIs during deserialization
public class FileBulkDataLoader : IBulkDataLoader
{
    public byte[] GetData(string uri)
    {
        var path = new Uri(uri).LocalPath;
        return File.ReadAllBytes(path);
    }
}

// Serialize with bulk data externalization
var serializeOptions = new DicomJsonSerializerOptions
{
    BulkDataConverter = new FileBulkDataConverter()
};
string json = DicomJsonSerializer.Serialize(dataset, serializeOptions);

// Deserialize with bulk data loading
var deserializeOptions = new DicomJsonSerializerOptions
{
    BulkDataLoader = new FileBulkDataLoader()
};
Dataset? restored = DicomJsonSerializer.Deserialize(json, deserializeOptions);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="JSON を DICOM にデシリアライズ">}}

<p>DICOM JSON を Dataset または DicomFile オブジェクトにパースします。文字列入力、ストリーム入力、非同期ストリーム操作をサポートします。</p>

<div class="codeblock" id="code">
 <h3>JSON を DICOM にデシリアライズ - C#</h3>
 <pre><code class="cs">string jsonText = File.ReadAllText("dicom_data.json");

// Deserialize to Dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(jsonText);

// Deserialize to full DicomFile (with File Meta Information)
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(jsonText);

// Deserialize a JSON array to multiple datasets
Dataset[]? datasets = DicomJsonSerializer.DeserializeList(jsonText);

// Async stream deserialization
using var stream = File.OpenRead("dicom_data.json");
Dataset? asyncResult = await DicomJsonSerializer.DeserializeAsync(stream);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="System.Text.Json との統合">}}

<p>Aspose.Medical は、標準の <code>System.Text.Json</code> シリアライズパイプラインと統合するための <code>DatasetJsonConverter</code> と <code>DicomFileJsonConverter</code> を提供します。これにより、既存の JSON 処理コード内で DICOM オブジェクトを他の .NET 型と共にシリアライズできます。</p>

<div class="codeblock" id="code">
 <h3>System.Text.Json 統合 - C#</h3>
 <pre><code class="cs">var jsonOptions = new JsonSerializerOptions { WriteIndented = true };

// Register DICOM converters
jsonOptions.Converters.Add(new DatasetJsonConverter());
jsonOptions.Converters.Add(new DicomFileJsonConverter());

// Use standard System.Text.Json serialization
string json = JsonSerializer.Serialize(dicomFile.Dataset, jsonOptions);
Dataset? result = JsonSerializer.Deserialize&lt;Dataset&gt;(json, jsonOptions);</code></pre>
</div>

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

{{< blocks/products/pf/slr-tab tabTitle="なぜ Aspose.Medical for .NET？" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="導入実績" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="導入事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
