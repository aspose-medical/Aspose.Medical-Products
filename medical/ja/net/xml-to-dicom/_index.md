---
title: C# .NETでXMLをDICOMに変換 | Aspose.Medical
weight: 5000

description: C# .NETでPS3.19のNative DICOM Model XMLからDICOMファイルを作成します。文字列、ストリーム、またはパイプからXMLを読み取り、連続するドキュメントをストリーム処理し、Aspose.Medical APIでバルクデータ参照を解決します。
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1=".NET C#でXMLをDICOMに変換" h2="PS3.19のNative DICOM Model XMLをデータセットおよびDICOMファイルに読み戻します。文字列、ストリーム、またはパイプから操作し、連続ドキュメントをストリーム処理し、バルクデータ参照を解決します。" logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="標準 Native DICOM Model XML">}}

<p><strong>Aspose.Medical for .NET</strong>は、DICOM PS3.19で定義された<a href="https://dicom.nema.org/medical/dicom/current/output/chtml/part19/chapter_A.html">Native DICOM Model</a>を読み取ります。これは標準自体に記述されたXML表現であり、Asposeが独自に作成したフォーマットではありません。そのため、既にDICOMをXMLでやり取りしているシステムが生成するドキュメントをこのライブラリが受け入れられるという点で統合に有用です。</p>

<p>ドキュメントのルートは<code>NativeDicomModel</code>で、各属性はタグ、値表現、キーワードを保持する<code>DicomAttribute</code>要素です：</p>

<div class="codeblock" id="code">
 <h3>Native DICOM Model フォーマット</h3>
 <pre><code class="xml">&lt;NativeDicomModel&gt;
  &lt;DicomAttribute tag="00100010" vr="PN" keyword="PatientName"&gt;
    &lt;PersonName number="1"&gt;
      &lt;Alphabetic&gt;
        &lt;FamilyName&gt;Doe&lt;/FamilyName&gt;
        &lt;GivenName&gt;John&lt;/GivenName&gt;
      &lt;/Alphabetic&gt;
    &lt;/PersonName&gt;
  &lt;/DicomAttribute&gt;
  &lt;DicomAttribute tag="00080060" vr="CS" keyword="Modality"&gt;
    &lt;Value number="1"&gt;CT&lt;/Value&gt;
  &lt;/DicomAttribute&gt;
&lt;/NativeDicomModel&gt;</code></pre>
</div>

<p>このページは<a href="/medical/net/dicom-to-xml/">DICOM to XML</a>の逆方向であり、両方とも同じクラス<code>DicomXmlSerializer</code>を使用します。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="C#でXMLからDICOMファイルを作成">}}

<p><code>Deserialize</code>はドキュメントを<code>Dataset</code>に変換し、データセットはDICOMファイルとしてディスクに書き込まれます。</p>

<div class="codeblock" id="code">
 <h3>XMLからDICOMファイルを作成 - C#</h3>
 <pre><code class="cs">// Read the Native DICOM Model document
string xml = File.ReadAllText("study.xml");

// Parse it into a dataset
Dataset dataset = DicomXmlSerializer.Deserialize(xml);

// Wrap the dataset in a file and write it
DicomFile dicomFile = new(dataset);
dicomFile.Save("study.dcm");</code></pre>
</div>

<p>Native DICOM ModelにはFile Meta Informationグループが無いため、転送構文はドキュメントの一部ではありません。<code>DicomFile</code>でラップされたデータセットは、デフォルトの転送構文であるImplicit VR Little Endianで書き込まれます。別の転送構文でファイルを保存したい場合は、<a href="/medical/net/dicom-transfer-syntax-conversion/">転送構文変換</a>ページが示すようにトランスコードしてください。</p>

<p>DICOM XMLの読み取りはライセンス機能です。オンプレミスのライセンスが適用されていない場合、リーダーは<code>MedicalApiException</code>をスローします。したがって、まず<a href="https://docs.aspose.com/medical/net/getting-started/licensing/">ライセンスガイド</a>に従ってライセンスを適用してください。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="ストリーム、パイプ、非同期">}}

<p>すべてのエントリポイントにはストリームオーバーロードと非同期オーバーロードがあり、非同期版は<code>PipeReader</code>も受け入れます。Webレスポンスから届くドキュメントは、文字列に変換せずに読み取りながらパースされます。</p>

<div class="codeblock" id="code">
 <h3>ストリームからXMLを読み取る - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream);
await new DicomFile(dataset).SaveAsync("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="1つのストリーム内の連続ドキュメント">}}

<p>他システムからのエクスポートは、単一ストリーム内に<code>NativeDicomModel</code>要素が連続して格納されていることがよくあります。<code>DeserializeAsyncEnumerable</code>は要素ごとに1つのデータセットを入力順に生成するため、ストリームはメモリに保持せずに処理されます。要素は直接続きます。XML宣言は任意のXML入力と同様に、最初の位置でのみ許可されます。</p>

<div class="codeblock" id="code">
 <h3>連続ドキュメントをストリーム処理 - C#</h3>
 <pre><code class="cs">await using FileStream stream = File.OpenRead("studies.xml");

int index = 0;
await foreach (Dataset dataset in DicomXmlSerializer.DeserializeAsyncEnumerable(stream))
{
    await new DicomFile(dataset).SaveAsync($"study-{index++}.dcm");
}</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="バルクデータ参照">}}

<p>ピクセルデータなどの大きな値はインラインで書き込まれません。代わりにバイトを指すURIを持つ<code>BulkData</code>要素として現れ、ドキュメントを小さく保ちます。読み取り時にこれらの参照を解決するには、シリアライザにバルクデータローダーを提供します。<code>DefaultBulkDataLoader</code>は認証なしで<code>file</code>、<code>http</code>、<code>https</code> URI を取得します。認証が必要なアーカイブの場合は、<code>IBulkDataLoader</code>または<code>IAsyncBulkDataLoader</code>を自分で実装してください。</p>

<div class="codeblock" id="code">
 <h3>読み取り時にバルクデータを解決 - C#</h3>
 <pre><code class="cs">DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default with
{
    BulkDataLoader = DefaultBulkDataLoader.Instance
};

await using FileStream stream = File.OpenRead("study.xml");

Dataset dataset = await DicomXmlSerializer.DeserializeAsync(stream, options);
new DicomFile(dataset).Save("study.dcm");</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="DICOM to XML の往復">}}

<p>この二方向は一緒に使用することを想定しています。研究データがXMLとして出力され、XMLを扱うシステムを経由し、最終的にDICOMファイルとして戻ります。すべてがマネージド .NET で実装されているため、同じ往復処理がWindows、Linux、macOSで動作します。</p>

<div class="codeblock" id="code">
 <h3>DICOM to XML と戻り - C#</h3>
 <pre><code class="cs">DicomFile source = DicomFile.Open("input.dcm");

string xml = DicomXmlSerializer.Serialize(source.Dataset);
Dataset restored = DicomXmlSerializer.Deserialize(xml);

new DicomFile(restored).Save("output.dcm");</code></pre>
</div>

<p>XMLのフォーマットを制御するオプションについては、<a href="/medical/net/dicom-to-xml/">DICOM to XML</a>ページをご覧ください。JSON向けにも同様のペアがあり、<a href="/medical/net/dicom-to-json/">DICOM to JSON</a>および<a href="/medical/net/json-to-dicom/">JSON to DICOM</a>があります。<a href="https://docs.aspose.com/medical/net/developer-guide/90-dicom-serialization/">シリアライズガイド</a>でAPI全体が解説されています。</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="学習リソース" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="ドキュメント" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="開発者ガイド" href="https://docs.aspose.com/medical/net/developer-guide/" >}}
{{< blocks/products/pf/slr-element name="APIリファレンス" href="https://reference.aspose.com/medical/net/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="製品サポート" tabId="support" >}}
{{< blocks/products/pf/slr-element name="無料サポート" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="有料サポート" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="ブログ" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle=".NET向け Aspose.Medical が選ばれる理由" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="顧客一覧" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="導入事例" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}