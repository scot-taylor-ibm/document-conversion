---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-11"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Getting started tutorial
{: #gettingStarted}

**Important:** Starting on **11-03-2017**, it will no longer be possible to create a new instance of {{site.data.keyword.documentconversionshort}} on Bluemix. Existing service instances will be supported until **10-03-2018**. To continue using features, you will need to [migrate](/docs/services/discovery/migrate-dcs-rr.html).  **Note:** Might not apply in select Dedicated environments.

This tutorial guides you through how to use and customize the {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.documentconversionshort}} service with cURL commands.
{: shortdesc}

> **Note:** The examples use cURL to call methods of the HTTP interface. You can install the version of cURL for your operating system from [curl.haxx.se ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://curl.haxx.se/){: new_window}. You must install the version that supports the Secure Sockets Layer (SSL) protocol. Make sure to include the installed binary file on your `PATH` environment variable.

## Step 1: Log in, create the service, and get your credentials
{: #credentials}

If you already know the credentials for your {{site.data.keyword.documentconversionshort}} service instance, skip this step.
{: tip}

1.  Go to the [{{site.data.keyword.documentconversionshort}} service ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/document-conversion/){: new_window} and either sign up for a free Bluemix account or log in.
1.  After you log in, enter `document-conversion-tutorial` in the **Service name field** of the {{site.data.keyword.documentconversionshort}} page. Click **Create**.
1.  Copy your credentials:
    1.  Click **Service credentials**.
    1.  Click **View credentials** under **Actions**.
    1.  Copy the `username` and `password` values.

## Step 2: Converting a document
{: #convert}

To convert the sample PDF document into Answer Units, follow these steps:

1.  Download the sample document: <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/document-conversion/sample.pdf" download>sample.pdf <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>.
1.  Navigate to the folder where you downloaded the `sample.pdf` file, and then issue the cURL command for the `POST /v1/convert_document` method to convert the `sample.pdf` document into Answer Units:

    ```
    curl -X POST -u "{username}":"{password}" \
    -F config="{\"conversion_target\":\"answer_units\"}" \
    -F file=@sample.pdf \
    "https://gateway.watsonplatform.net/document-conversion/api/v1/convert_document?version=2015-12-15"
    ```
    {: screen}

    Replace `{username}` and `{password}` with your service credentials. For more information about each request parameter, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/document-conversion/api/v1/){: new_window}.

    Usually, the API automatically detects the format type of the input file. However, if the file is imperfectly formatted, the API might not detect it. You can still convert the document, but you must specify the file format type:

    ```
    curl -X POST -u "{username}":"{password}" \
    -F config="{\"conversion_target\":\"answer_units\"}" \
    -F "file=@sample.pdf;type=application/pdf" \
    "https://gateway.watsonplatform.net/document-conversion/api/v1/convert_document?version=2015-12-15"
    ```
    {: screen}

    Possible conversion target type values are `text/html`, `text/xhtml+xml`, `application/pdf`, `application/msword`, and `application/vnd.openxmlformats-officedocument.wordprocessingml.document`.

The response contains the full content of the converted document.

### Using your own data
{: #using-own-data}

To build your own conversion, replace the file being called with your own PDF, HTML, or Word document file in the cURL command. Also replace the `conversion_target` with the format you want to convert into. Valid values are `answer_units`, `normalized_html`, or `normalized_text`.

## Step 3: Converting a document with a custom configuration
{: #convertcustom}

This tutorial walks you through using a custom conversion configuration that strips the excess tags and information found on a typical corporate web page. To convert the example HTML page into Answer Units by using the example custom configuration JSON file, follow these steps:

1.  Download the sample HTML page that represents a typical corporate html page: <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/document-conversion/example.html" download>example.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>. The file is a version of this tutorial page.
1.  Download the sample custom JSON conversion file: <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/document-conversion/tutorial-config.json" download>tutorial-config.json <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>. This JSON file contains the customization parameters that can clean up a typical corporate web page. The file uses these custom parameters to perform the following actions:
    -   `exclude_tags_completely`: Use `script`, `sup`, and `link` to replace the CSS link tags with their complete information (`rel` and `href` attributes) from the page header.
    -   `exclude_tags_keep_content`: Use the defaults `font`, `em`, `span`, and the listed values `strong` and `code` to remove these tags but retain their content.
    -   `exclude_content`: These XPaths are used to exclude certain branches from the Document Object Model (DOM):
        -   `"//div[@class=\"footer-nav\"]"`: Selects the footer information from the main column.
        -   `"//section[@class=\"bodySection\"]/h3"`: Selects the `h3` tag from the main column because it contains no useful information.
        -   `"/body/div/section/section/p[2]"`: Selects the second paragraph from the main column (here selected through the whole path) that comes after the `h3` heading that is removed in the previous bullet.
1.  Issue the `POST /v1/convert_document` cURL command to convert your HTML document with the customizations listed in the `tutorial-config.json` file.
    -   Replace `{username}` and `{password}` with your service credentials.
    -   Place the `tutorial-config.json` file and the `example.html` document in the same folder. Navigate to the location where these two files are located.

    ```
    curl -X POST -u "{username}":"{password}" \
    -F "config=@tutorial-config.json" \
    -F file=@example.html \
    "https://gateway.watsonplatform.net/document-conversion/api/v1/convert_document?version=2015-12-15"
    ```
    {: screen}

The response contains a cleaned up version of the full content of the converted document.

### Using your own data
{: #using-own-data}

To create your own custom JSON, see [Advanced customization options ![External link icon](../../icons/launch-glyph.svg "External link icon")](customizing.html){: new_window}.

## Next Steps
{: #nextsteps}

Now that you have a basic understanding of how to use the {{site.data.keyword.documentconversionshort}} service, dive deeper by using the {{site.data.keyword.documentconversionshort}} service with other programming languages:

-   To create and run an example Node.js application, see the [{{site.data.keyword.documentconversionshort}} Node.js SDK on GitHub. ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/node-sdk/){: new_window}.
-   To create and run a sample Java application, see [{{site.data.keyword.documentconversionshort}} Java SDK on GitHub ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/java-sdk/){: new_window}. For a video of using the Java SDK, see the [{{site.data.keyword.watson}} {{site.data.keyword.documentconversionshort}} service demo on YouTube ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.youtube.com/watch?v=4yWyPSXRhHI){: new_window}.
-   To create and run a sample Python application, see [{{site.data.keyword.documentconversionshort}} Python SDK on GitHub.](https://github.com/watson-developer-cloud/python-sdk).
-   For an introduction to working with {{site.data.keyword.watson}} Developer Cloud services and {{site.data.keyword.Bluemix_notm}}, see [{{site.data.keyword.Bluemix_notm}} development approaches](../common/getting-started-bluemix.html).
