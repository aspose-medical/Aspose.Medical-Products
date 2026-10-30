---
title: DICOMweb Server in C# .NET - QIDO-RS, WADO-RS, STOW-RS | Aspose.Medical
weight: 9200

description: Build your own DICOMweb server or PACS in C#. Aspose.Medical implements QIDO-RS, WADO-RS, STOW-RS and the worklist services on ASP.NET Core; you connect the storage.
---

{{< blocks/products/pf/main-wrap-class >}}

{{< blocks/products/pf/upper-banner h1="DICOMweb Server for .NET C#" h2="Add QIDO-RS, WADO-RS and STOW-RS endpoints to an ASP.NET Core service. The library speaks the protocol; your code decides where the studies are stored." logoImageSrc="/medical/images/aspose_medical-brand.svg" pfName="Aspose.Medical" subTitlepfName="for .NET" downloadUrl="https://downloads.aspose.com/medical/net" >}}

{{< blocks/products/pf/main-container pfName="Aspose.Medical" subTitlepfName="for .NET" >}}

{{< blocks/products/pf/feature-page-section h2="Your own DICOMweb server in C#">}}

<p>A PACS, a VNA, a cloud archive or a research platform has to answer DICOMweb requests: QIDO-RS searches, WADO-RS downloads and STOW-RS uploads, and often the worklist services as well. Each of them has its own routes, media types, multipart bodies, matching rules and status codes.</p>

<p><strong>Aspose.Medical for .NET</strong> implements that protocol side since version 26.10. It reads every request, checks it against the standard, matches search keys by the DICOM rules, pages the results, converts transfer syntaxes and writes the response. Your code does what only you can do: store an instance, find candidates for a search and return the stored files. Viewers, modalities and other archives then work with your server like with any other DICOMweb server, and so does the <a href="/medical/net/dicomweb-client/">DICOMweb client</a> of the same library.</p>

<p>The server is managed C#. It runs wherever ASP.NET Core runs: on Windows, on Linux and in a container.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="A complete server in ASP.NET Core">}}

<p>Install the <code>Aspose.Medical.Dicom.Web.AspNetCore</code> package. It brings Aspose.Medical with it and connects the DICOMweb services to dependency injection and endpoint routing. This <code>Program.cs</code> is a working Studies server; the two classes it registers are shown below.</p>

<div class="codeblock" id="code">
 <h3>Program.cs of a DICOMweb server - C#</h3>
 <pre><code class="cs">WebApplicationBuilder builder = WebApplication.CreateBuilder(args);

// Your storage, and the rendered images your server offers
builder.Services.AddSingleton&lt;IDicomWebStudiesProvider&gt;(new FolderArchive("archive"));
builder.Services.AddSingleton&lt;IDicomWebRepresentationProvider, JpegRendering&gt;();

// QIDO-RS, WADO-RS and STOW-RS come from the library
builder.Services.AddDicomWeb().AddStandardStudiesService();

WebApplication app = builder.Build();
app.MapDicomWeb("/dicomweb");
app.Run();</code></pre>
</div>

<p>Every Studies route now answers under <code>/dicomweb</code>: <code>/dicomweb/studies</code> for search and upload, <code>/dicomweb/studies/{study}/series/{series}/instances/{instance}</code> for retrieval, and the routes for metadata, frames, bulk data and rendered images. The capabilities document at the service root lists exactly the transactions the server supports.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Connect your storage">}}

<p>The Studies service needs one interface with three methods: store an instance, return the stored files, and return search candidates. This implementation keeps each instance as a file in a folder per study and series.</p>

<div class="codeblock" id="code">
 <h3>A folder archive behind QIDO-RS, WADO-RS and STOW-RS - C#</h3>
 <pre><code class="cs">// Keeps instances as files: archive/&lt;study&gt;/&lt;series&gt;/&lt;instance&gt;.dcm
public sealed class FolderArchive(string root) : IDicomWebStudiesProvider
{
    // STOW-RS calls this once for every uploaded instance
    public async ValueTask&lt;DicomWebStoreInstanceResult&gt; StoreAsync(DicomFile instance,
        CancellationToken cancellationToken = default)
    {
        // The library has already checked these UIDs, so they are safe as folder and file names
        string study = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.StudyInstanceUID);
        string series = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SeriesInstanceUID);
        string sop = instance.Dataset.GetSingleValue&lt;string&gt;(Tag.SOPInstanceUID);

        string folder = Path.Combine(root, study, series);
        Directory.CreateDirectory(folder);
        await instance.SaveAsync(Path.Combine(folder, sop + ".dcm"), cancellationToken: cancellationToken);
        return new DicomWebStoreInstanceResult { Outcome = DicomWebStoreOutcome.Stored };
    }

    // WADO-RS: the files of a study, a series or one instance
    public async IAsyncEnumerable&lt;DicomFile&gt; RetrieveAsync(DicomWebResourceIdentifier resource,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        foreach (string path in Files(resource))
            yield return await DicomFile.OpenAsync(path, cancellationToken: cancellationToken);
    }

    // QIDO-RS: one candidate per study, series or instance; the library does the matching
    public async IAsyncEnumerable&lt;Dataset&gt; FindCandidatesAsync(DicomWebSearchContext search,
        [EnumeratorCancellation] CancellationToken cancellationToken = default)
    {
        HashSet&lt;string&gt; seen = [];
        foreach (string path in Files(search.Resource))
        {
            string key = search.Level switch
            {
                DicomWebSearchLevel.Study =&gt; Path.GetDirectoryName(Path.GetDirectoryName(path))!,
                DicomWebSearchLevel.Series =&gt; Path.GetDirectoryName(path)!,
                _ =&gt; path
            };
            if (!seen.Add(key))
                continue;

            // A search needs the attributes, not the pixel data
            DicomFile file = await DicomFile.OpenAsync(path, strategy: TagDataReadingStrategies.SkipLargeTags(),
                cancellationToken: cancellationToken);
            yield return file.Dataset;
        }
    }

    // All files under the requested study, series or instance, in a stable order
    private IEnumerable&lt;string&gt; Files(DicomWebResourceIdentifier resource)
    {
        string folder = Path.Combine(root, resource.StudyInstanceUid ?? "", resource.SeriesInstanceUid ?? "");
        if (resource.SopInstanceUid is string sop)
            return File.Exists(Path.Combine(folder, sop + ".dcm")) ? [Path.Combine(folder, sop + ".dcm")] : [];
        if (!Directory.Exists(folder))
            return [];
        return Directory.EnumerateFiles(folder, "*.dcm", SearchOption.AllDirectories).Order(StringComparer.Ordinal);
    }
}</code></pre>
</div>

<p>The library checks the UIDs in every request path and in every uploaded instance before it calls the provider. An instance without valid UIDs is reported as failed and never reaches <code>StoreAsync</code>.</p>

<p>A folder is enough to try the server. A production archive keeps the same three methods and puts an object store behind <code>StoreAsync</code> and <code>RetrieveAsync</code> and a database index behind <code>FindCandidatesAsync</code>.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Search: the library applies the DICOM matching rules">}}

<p>QIDO-RS matching is more than string comparison: wildcards, date and time ranges, lists of UIDs, fuzzy person names, empty values and keys inside sequences. The provider does not implement any of it. It returns candidates in a stable order, and may return more than match. The library applies the exact DICOM match to each candidate, keeps the requested fields, and applies <code>offset</code> and <code>limit</code>. One response holds at most 1000 results by default, and <code>DicomWebStandardServiceOptions</code> changes that limit.</p>

<p>To keep searches fast on a large archive, pass the selective keys to the database. <code>search.Plan.Filter</code> holds the compiled search keys, with the attribute, the kind of match and the values of each:</p>

<div class="codeblock" id="code">
 <h3>Read an exact Patient ID from a search - C#</h3>
 <pre><code class="cs">// The Patient ID of a search that asks for one exact value, or null
static string? ExactPatientId(DicomWebSearchContext search) =&gt;
    search.Plan.Filter
        .Where(c =&gt; c.Kind == DicomWebMatchKind.Exact &amp;&amp; c.Attribute.Tags is [Tag tag] &amp;&amp; tag == Tag.PatientID)
        .Select(c =&gt; c.Values[0])
        .FirstOrDefault();</code></pre>
</div>

<p>A database query built this way may return extra rows, for example when it ignores a key it cannot translate. The final match in the library removes them. It must not drop true matches, because the library cannot add them back.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Store: a result for every instance">}}

<p>A STOW-RS request can carry hundreds of instances. The library parses the multipart body and calls <code>StoreAsync</code> for each instance in turn, so the whole upload is never held in memory at once. For each instance the provider returns <code>Stored</code>, <code>Warning</code> for an instance stored with changes, or <code>Failed</code> with an optional DICOM reason code.</p>

<p>The library builds the STOW-RS report from these results and selects the status: 200 when every instance is stored, 202 when some have warnings or failed, 409 when all failed. Instances in a request for one named study that belong to another study are refused before the provider is called.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Retrieve: transfer syntaxes, frames and rendered images">}}

<p>The provider returns stored files, and the library delivers them in the form the client asked for. A WADO-RS client can ask for a transfer syntax in the <code>Accept</code> header, and the library converts each file before it sends it, with its own codecs, HTJ2K and JPEG XL included. Metadata in DICOM JSON or XML, bulk data and single frames come from the same provider, with no extra code.</p>

<p>Rendered images need one more class, because only you decide which images the server may produce. This one offers JPEG for rendered instances, rendered frames and thumbnails, and the library renders them:</p>

<div class="codeblock" id="code">
 <h3>Offer rendered JPEG images and thumbnails - C#</h3>
 <pre><code class="cs">// Declares the rendered images the server offers. The library renders them as JPEG.
public sealed class JpegRendering : IDicomWebRepresentationProvider
{
    public ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt; SupportedMediaTypes { get; } =
        ImmutableDictionary&lt;DicomWebTransaction, ImmutableArray&lt;string&gt;&gt;.Empty
            .Add(RetrieveRenderedInstance.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveRenderedFrames.Instance, [DicomWebMediaTypes.ImageJpeg])
            .Add(RetrieveInstanceThumbnail.Instance, [DicomWebMediaTypes.ImageJpeg]);

    public ImmutableArray&lt;string&gt; GetResponseMediaTypes(DicomWebRepresentationRequest request) =&gt;
        [DicomWebMediaTypes.ImageJpeg];
}</code></pre>
</div>

<p>The built-in renderer produces JPEG from a single-frame instance or from the frames a request selects. A rendered multi-frame instance as a whole, an image wider or taller than 65500 pixels (thumbnails pass over such images), other image formats, and the viewport, window and ICC profile parameters need your own <code>RetrieveAsync</code> in this class.</p>

<p>Thumbnails are handled with care for patient privacy. The built-in renderer makes a thumbnail only from an instance whose Burned In Annotation and Recognizable Visual Features attributes are both <code>NO</code>, which means the image itself declares that it shows no patient data. Many images do not carry these attributes, and for them the server answers 406. If your archive needs thumbnails of such images, produce them in your own <code>RetrieveAsync</code> and make sure they hide any patient data.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Worklists, procedure steps and the other services">}}

<p>The other DICOMweb services follow the same model: register the service, implement its storage interface.</p>

<ul>
<li><a href="/medical/net/ups-rs/">UPS-RS</a> worklists with <code>AddStandardWorklistService</code>: create, search and update workitems and change their state, with the state rules checked by the library and event notifications sent over a WebSocket.</li>
<li>The <a href="/medical/net/modality-worklist/">modality worklist</a> (MWL-RS) with <code>AddStandardModalityWorklistService</code>, so modalities can query the scheduled procedure steps of your RIS.</li>
<li>Modality performed procedure steps (MPPS-RS), storage commitment and non-patient instances such as color palettes and hanging protocols.</li>
<li>WADO-URI retrieval for older viewers, served by the Studies service.</li>
</ul>

<p>When the standard behavior of one transaction does not fit, register your own handler for it with <code>AddHandler</code>. The library still routes and checks the request, and the other transactions keep their standard handlers.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Security and hosting stay in your hands">}}

<p>The DICOMweb endpoints are ordinary ASP.NET Core endpoints. Authentication, authorization, CORS, TLS, forwarded headers and request size limits are configured the same way as for the rest of your service:</p>

<div class="codeblock" id="code">
 <h3>Require an authenticated user - C#</h3>
 <pre><code class="cs">app.MapDicomWeb("/dicomweb").RequireAuthorization();</code></pre>
</div>

<p>The protocol part does not depend on ASP.NET Core. <code>DicomWebApplication</code> turns a <code>DicomWebRequest</code> into a response, so the same services can run behind another HTTP server or in a test without a network:</p>

<div class="codeblock" id="code">
 <h3>The same server without ASP.NET Core - C#</h3>
 <pre><code class="cs">DicomWebApplication application = DicomWebApplication.CreateBuilder()
    .AddStandardStudiesService(new FolderArchive("archive"), new JpegRendering())
    .Build();

// For each HTTP request: translate it, process it, write the result
DicomWebApplicationResult result = await application.ProcessAsync(request, cancellationToken);</code></pre>
</div>

{{< /blocks/products/pf/feature-page-section >}}

{{< blocks/products/pf/feature-page-section h2="Client and server from one library">}}

<p>The <a href="/medical/net/dicomweb-client/">DICOMweb client</a> of Aspose.Medical tests your server from a unit test or a console program, and every file the server receives is a regular <code>DicomFile</code> that can be anonymized, transcoded or rendered in the same process.</p>

<p>The <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/">DICOMweb developer guide</a> covers every service and storage interface, and the <a href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/aspnet-core/">ASP.NET Core guide</a> covers endpoint mapping and WebSockets.</p>

<p>Related pages: <a href="/medical/net/dicom-networking/">DICOM networking</a> for systems that send studies over DIMSE instead of HTTP, <a href="/medical/net/anonymization/">anonymization</a> of incoming studies, <a href="/medical/net/dicom-transfer-syntax-conversion/">transfer syntax conversion</a>, and <a href="/medical/net/dicom-to-json/">DICOM to JSON</a> for the format QIDO-RS results use.</p>

{{< /blocks/products/pf/feature-page-section >}}

{{< /blocks/products/pf/main-container >}}

{{< blocks/products/pf/support-learning-resources >}}
{{< blocks/products/pf/slr-tab tabTitle="Learning Resources" tabId="resources" >}}
{{< blocks/products/pf/slr-element name="Documentation" href="https://docs.aspose.com/medical/net/" >}}
{{< blocks/products/pf/slr-element name="Developer Guide" href="https://docs.aspose.com/medical/net/developer-guide/dicomweb/" >}}
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
