---
title: C# .NET で JSON を DICOM に変換 | Aspose.Medical
weight: 6000

description: C# .NET で標準 DICOM JSON Model（PS3.18）から DICOM ファイルを作成します。JSON を文字列、ストリーム、またはパイプから読み取り、データセットのシーケンスをストリーミングし、Aspose.Medical API を使用して Bulk Data 参照を解決します。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C# で JSON を DICOM に変換" h2="標準 DICOM JSON Model（PS3.18）をデータセットおよび DICOM ファイルに戻します。文字列、ストリーム、またはパイプから処理し、スタディのシーケンスをストリーミングし、Bulk Data 参照を解決します。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM JSON から DICOM ファイルへ">}}

<p><strong>Aspose.Medical for .NET</strong> は <a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part18/chapter_F.html">DICOM PS3.18 JSON Model</a> を読み取ります。このモデルは DICOMweb サービスや HTTP 経由でスタディを交換するシステムで使用される表現です。JSON として受信したものは <code>Dataset</code> になり、<code>Dataset</code> は DICOM ファイルとしてディスクに書き込まれます。</p>

<p>これは <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> ページの逆方向であり、両方とも同じクラス <code>DicomJsonSerializer</code> を使用します。</p>

<div class="codeblock" id="code">
 <h3>JSON から DICOM ファイルを作成 - C#</h3>
 <pre><code class="cs">// Read the DICOM JSON document
string json = File.ReadAllText("study.json");

// Parse it into a dataset
Dataset? dataset = DicomJsonSerializer.Deserialize(json);
if (dataset is null)
    return;

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>File Meta Information を持たないデータセットは、<code>DicomFile</code> にラップされると、デフォルトの転送構文である Implicit VR Little Endian で書き込まれます。</p>

<p>DICOM JSON の読み取りはライセンス機能です。オンプレミスのライセンスが適用されていない場合、リーダーは <code>MedicalApiException</code> をスローします。したがって、まず <a href="https://docs.aspose.com/medical/net/getting-started/licensing/">ライセンス ガイド</a> に従ってライセンスを適用してください。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="File Meta Information を保持する">}}

<p><code>Deserialize</code> はデータセットだけを返します。JSON ドキュメントが File Meta Information グループも含んでいる場合（例えば完全な DICOM ファイルから生成された場合）、<code>DeserializeFile</code> はそのグループを保持した <code>DicomFile</code> を返し、ファイルが宣言する転送構文も含まれます。</p>

<div class="codeblock" id="code">
 <h3>JSON から完全な DICOM ファイルを読み取る - C#</h3>
 <pre><code class="cs">string json = File.ReadAllText("study.json");

// Keeps the File Meta Information group that a DicomFile carries
DicomFile? dicomFile = DicomJsonSerializer.DeserializeFile(json);
dicomFile?.Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ストリーム、パイプ、非同期">}}

<p>すべてのエントリーポイントにはストリームオーバーロードと非同期オーバーロードがあり、非同期オーバーロードは <code>PipeReader</code> も受け取ります。Web 応答やディスクから取得したドキュメントは、まず文字列に変換せずに直接読み取られます。これは JSON にピクセルデータが含まれる場合に重要です。</p>

<div class="codeblock" id="code">
 <h3>ストリームから JSON を読む - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.json");

DicomFile? dicomFile = await DicomJsonSerializer.DeserializeFileAsync(stream);
if (dicomFile is not null)
    await dicomFile.SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="データセットのシーケンス、1 つずつ">}}

<p>DICOMweb クエリはデータセットの配列で応答し、そのようなドキュメントは大きくなる可能性があります。<code>DeserializeList</code> は配列全体をメモリに読み込みますが、<code>DeserializeAsyncEnumerable</code> はデータセットを一つずつ生成するため、ドキュメント全体を保持することはありません。</p>

<div class="codeblock" id="code">
 <h3>データセットの配列をストリームする - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.json");

int index = 0;
await foreach (Dataset? dataset in DicomJsonSerializer.DeserializeAsyncEnumerable(stream))
{
    if (dataset is null)
        continue;

    DicomFile dicomFile = new(dataset);
    await dicomFile.SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Bulk Data 参照">}}

<p>DICOM JSON Model はピクセルデータをインラインで保持しません。大きな値はバイトを指す <code>BulkDataURI</code> に置き換えられ、JSON ドキュメントを小さく保ちます。読み取り中にこれらの参照を解決するには、シリアライザに Bulk Data ローダーを提供します。<code>DefaultBulkDataLoader</code> は認証なしで <code>file</code>、<code>http</code>、<code>https</code> URI を取得します。認証が必要なアーカイブの場合は、<code>IBulkDataLoader</code> または <code>IAsyncBulkDataLoader</code> を自分で実装してください。</p>

<div class="codeblock" id="code">
 <h3>読み取り時に BulkDataURI を解決する - C#</h3>
 <pre><code class="cs">DicomJsonSerializerOptions options = DicomJsonSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.json");

Dataset? dataset = await DicomJsonSerializer.DeserializeAsync(stream, options);
if (dataset is not null)
    new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM から JSON への往復変換">}}

<p>この 2 方向は一緒に使用することを想定しています。スタディは JSON として出力され、Web サービスを通過し、DICOM ファイルとして戻ります。プロセスはネイティブコードに依存しないため、同じ往復が Windows、Linux、macOS で動作します。</p>

<div class="codeblock" id="code">
 <h3>DICOM から JSON へ、そして戻す - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string json = DicomJsonSerializer.Serialize(source.Dataset);
Dataset? restored = DicomJsonSerializer.Deserialize(json);

if (restored is not null)
    new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>JSON の形式を制御するオプションについては、<a href="/medical/net/dicom-to-json/">DICOM to JSON</a> ページをご参照ください。同様のペアが XML 用にもあり、<a href="/medical/net/dicom-to-xml/">DICOM to XML</a> と <a href="/medical/net/xml-to-dicom/">XML to DICOM</a> です。<a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/">JSON シリアル化ガイド</a> が API 全体をカバーしています。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学習リソース" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="ドキュメント" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="開発者ガイド" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="API リファレンス" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="製品サポート" tabId="support" >}}
{{< blocks/products/pf/slr-element name="無料サポート" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="有料サポート" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ブログ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle=".NET 向け Aspose.Medical を選ぶ理由" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="顧客一覧" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="成功事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}