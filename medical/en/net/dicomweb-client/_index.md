---
title: DICOMweb Client in C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9100

description: Search, download and upload DICOM studies over HTTP from .NET. A DICOMweb client in pure C# for QIDO-RS, WADO-RS and STOW-RS that works with PACS, VNA and cloud imaging services.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb Client for .NET C#" h2="Search, download and upload studies on any DICOMweb server with QIDO-RS, WADO-RS and STOW-RS, in managed C# with nothing to install on the machine." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="DICOM over HTTP, from your own code">}}

<p>DICOMweb is the HTTP form of DICOM. Instead of a DIMSE association on port 104, your application sends ordinary HTTPS requests: QIDO-RS to search, WADO-RS to download, STOW-RS to upload. Cloud imaging services such as the Azure Health Data Services DICOM service and the Google Cloud Healthcare API work this way, and so do archives such as Orthanc and dcm4chee.</p>

<p><strong>Aspose.Medical for .NET</strong> includes a DICOMweb client since version 26.10. <code>DicomWebClient</code> wraps every standard transaction in a typed method, and the results arrive as the same <code>Dataset</code> and <code>DicomFile</code> objects the rest of the library reads and writes. The server on the other side can be any DICOMweb implementation; it does not have to be built with Aspose.Medical.</p>

<p>The client is managed C# on top of <code>HttpClient</code>. There is no native toolkit and no proxy service, so the same code runs on Windows, on Linux and in a container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Connect to a DICOMweb server">}}

<p>Create the client from an <code>HttpClient</code> and the service root URL of the server. Authentication, TLS settings, proxies and connection pooling stay on the <code>HttpClient</code>, where your application already configures them. The client does not own the <code>HttpClient</code> and does not dispose it.</p>

<div class="codeblock" id="code">
 <h3>Create a DICOMweb client - C#</h3>
 <pre><code class="cs">using HttpClient http = new HttpClient();

// Most cloud services expect an OAuth access token
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

DicomWebClient client = new DicomWebClient(http, new DicomWebClientOptions
{
    BaseUri = new Uri("https://pacs.example.org/dicom-web/")
});</code></pre>
</div>

<p>If one deployment serves WADO-URI or another service from a different URL, <code>ServiceBaseUris</code> overrides the root for that service only.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Search studies with QIDO-RS">}}

<p>A search takes matching parameters with the DICOM keyword as the name, the fields to return, and paging controls. The matches arrive as a stream of datasets, read one at a time while the response is still downloading.</p>

<div class="codeblock" id="code">
 <h3>Find the studies of a patient - C#</h3>
 <pre><code class="cs">DicomWebSearchOptions search = new DicomWebSearchOptions
{
    MatchingParameters = [new DicomWebQueryParameter("PatientName", "SMITH*")],
    IncludeFields = ["StudyDescription"],
    Limit = 20
};

await using DicomWebClientStream&lt;Dataset&gt; studies = await client.Studies.SearchAsync(search);

await foreach (Dataset study in studies)
{
    string? studyUid = study.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty);
    string? description = study.GetSingleValueOrDefault(Tag.StudyDescription, string.Empty);
    Console.WriteLine($"{studyUid}  {description}");
}</code></pre>
</div>

<p><code>SearchSeriesAsync</code> and <code>SearchInstancesAsync</code> search one level down, across the whole archive or inside one study or series. The same options also set fuzzy matching, empty value matching and the result offset for paging.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Download a study with WADO-RS">}}

<p>A study can hold thousands of instances. <code>RetrieveStudyAsync</code> returns them as a stream, so each file is parsed as its part of the response arrives and can be saved before the next one is read. Memory use does not grow with the size of the study.</p>

<div class="codeblock" id="code">
 <h3>Save every instance of a study - C#</h3>
 <pre><code class="cs">Directory.CreateDirectory("study");

await using DicomWebClientStream&lt;DicomFile&gt; files = await client.Studies.RetrieveStudyAsync(studyUid);

await foreach (DicomFile file in files)
{
    string sopInstanceUid = file.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);
    file.Save(Path.Combine("study", sopInstanceUid + ".dcm"));
}</code></pre>
</div>

<p><code>RetrieveSeriesAsync</code> and <code>RetrieveInstanceAsync</code> download one series or one instance in the same way.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Get a thumbnail or a rendered image">}}

<p>A viewer or a worklist screen often needs a picture, not a DICOM file. The server renders it and sends a JPEG or PNG, so the client does not download or decode the original pixel data.</p>

<div class="codeblock" id="code">
 <h3>Save a JPEG thumbnail of an instance - C#</h3>
 <pre><code class="cs">DicomWebRenderOptions render = new DicomWebRenderOptions
{
    AcceptedMediaTypes = [DicomWebMediaTypes.ImageJpeg]
};

await using DicomWebClientResponse thumbnail =
    await client.Studies.RetrieveInstanceThumbnailAsync(studyUid, seriesUid, sopInstanceUid, render);
thumbnail.EnsureSuccessStatusCode();

await using FileStream output = File.Create("thumbnail.jpg");
await thumbnail.ContentReader.CopyToAsync(output);</code></pre>
</div>

<p><code>RetrieveRenderedInstanceAsync</code> and <code>RetrieveRenderedFramesAsync</code> return a full-size image instead. <code>DicomWebRenderOptions</code> passes the standard rendering controls to the server: image quality, viewport size, window and ICC profile.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Upload instances with STOW-RS">}}

<p>STOW-RS sends any number of instances in one request. The server answers with a report that lists, instance by instance, what it stored and what it refused.</p>

<div class="codeblock" id="code">
 <h3>Upload a folder of DICOM files - C#</h3>
 <pre><code class="cs">List&lt;DicomFile&gt; upload = Directory
    .EnumerateFiles("outgoing", "*.dcm")
    .Select(path =&gt; DicomFile.Open(path))
    .ToList();

DicomWebClientResult&lt;StoreInstancesResult&gt; stored = await client.Studies.StoreAsync(upload);

// The report lists every instance the server did not accept, with a reason code
if (stored.Value.Response.GetOrDefault(Tag.FailedSOPSequence) is Sequence failed)
{
    foreach (Dataset item in failed)
    {
        string? uid = item.GetSingleValueOrDefault(Tag.ReferencedSOPInstanceUID, string.Empty);
        ushort reason = item.GetSingleValueOrDefault&lt;ushort&gt;(Tag.FailureReason, 0);
        Console.WriteLine($"Not stored: {uid}, reason 0x{reason:X4}");
    }
}</code></pre>
</div>

<p>Some servers answer with an HTTP error, such as 409, when one or more instances fail. The call then throws <code>DicomWebClientException</code>, as the next section shows, and the same report is in the response body of the exception.</p>

<p><code>StoreStudyAsync</code> uploads into one named study. Both methods also accept an <code>IAsyncEnumerable&lt;DicomFile&gt;</code>, so files can be produced while the request is being sent.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Handle server errors">}}

<p>Every typed operation checks the HTTP status before it returns. When the server refuses a request, the call throws <code>DicomWebClientException</code>, which carries the status code, the headers and the start of the response body for the log.</p>

<div class="codeblock" id="code">
 <h3>Report a failed request - C#</h3>
 <pre><code class="cs">try
{
    DicomWebClientResult&lt;RetrieveInstanceResult&gt; instance =
        await client.Studies.RetrieveInstanceAsync(studyUid, seriesUid, sopInstanceUid);
    instance.Value.File.Save("instance.dcm");
}
catch (DicomWebClientException ex)
{
    Console.WriteLine($"The server answered {ex.Response.StatusCode}");
    Console.WriteLine(Encoding.UTF8.GetString(ex.ResponseBody.Span));
}</code></pre>
</div>

<p>Methods that return a raw response, such as the thumbnail above, leave the status check to you through <code>IsSuccessStatusCode</code> or <code>EnsureSuccessStatusCode</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="What else the client covers">}}

<p>Search, download and upload are the common calls. The client has an operation group for each of the other DICOMweb services as well:</p>

<ul>
<li>Bulk data, pixel data and selected frames of an instance, by one-based frame number, as the server sent them.</li>
<li>WADO-URI, the older single-URL retrieval that many viewers and archives still use, for DICOM files and rendered images.</li>
<li>UPS-RS worklists: create, search, update and claim workitems, change their state, request cancellation, and subscribe to events that arrive over a WebSocket.</li>
<li>The modality worklist (MWL-RS), modality performed procedure steps (MPPS-RS) and storage commitment.</li>
<li>Non-patient instances such as color palettes and hanging protocols, and the capabilities document of the server.</li>
<li><code>SendAsync</code> for any standard transaction with your own route values, query parameters, headers and body, when a typed method does not fit.</li>
</ul>

<p>The <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/">DICOMweb client guide</a> documents every operation and option, and the <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client-operations/">client operation reference</a> lists every method.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="One library from the HTTP response to the pixel data">}}

<p>A file downloaded with the client is a regular <code>DicomFile</code>. It can be anonymized, transcoded, rendered or serialized in the same process, with no conversion step and no second library in the chain.</p>

<p>Related pages: <a href="/medical/net/dicomweb-server/">DICOMweb server</a> for the other side of the protocol in your own ASP.NET Core service, <a href="/medical/net/dicom-networking/">DICOM networking</a> for archives that speak DIMSE instead of HTTP, <a href="/medical/net/anonymization/">anonymization</a> before a study leaves the hospital, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a> for what the server stores, and <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> for the format QIDO-RS search results use.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Learning Resources" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Developer Guide" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/client/" >}}
{{< blocks/products/pf/slr-element name="API References" href="https://reference.aspose.com/medical/net/aspose.medical.dicom.web/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Product Support" tabId="support" >}}
{{< blocks/products/pf/slr-element name="Free Support" href="https://forum.aspose.com/c/medical" >}}
{{< blocks/products/pf/slr-element name="Paid Support" href="https://helpdesk.aspose.com/" >}}
{{< blocks/products/pf/slr-element name="Blog" href="https://blog.aspose.com/category/medical/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< blocks/products/pf/slr-tab tabTitle="Why Aspose.Medical for .NET?" tabId="success-stories" >}}
{{< blocks/products/pf/slr-element name="Customers List" href="https://company.aspose.com/customers" >}}
{{< blocks/products/pf/slr-element name="Success Stories" href="https://company.aspose.com/customers/success-stories/" >}}
{{< /blocks/products/pf/slr-tab >}}

{{< /blocks/products/pf/support-learning-resources >}}

{{< blocks/products/pf/download-section downloadFreeTrialLink="https://downloads.aspose.com/medical/net" pricingInformationLink="https://purchase.aspose.com/pricing/medical/net" >}}

{{< /blocks/products/pf/main-wrap-class >}}
