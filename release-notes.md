---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-10"

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

# Release notes
{: #release-notes}

The following new features and changes to the service are available. These changes do not break existing code. If a change breaks existing code, it is identified as a new version.
{: shortdesc}

## API version
{: #version}

When IBM makes breaking changes to the API, it releases a new API version. The current version is `2015-12-15`; the documentation reflects the current version. To upgrade your API to the latest version, use the [Change log](#changelog) to verify that you have made all of the code updates necessary and change your version parameter to the new date. The value for the `version` parameter is the date for the version of the API that you want to call.

### Change log
{: #changelog}

The following service versions are available:

-   `2015-12-15`: This release makes the {{site.data.keyword.documentconversionshort}} service Generally Available (GA).

## 30 June 2016
{: #jun30}

-   IBM added a new method, `/index_document`. The method converts a document, prepares it for the {{site.data.keyword.retrieveandrankshort}} service, and adds the content to your Solr index. For more information about the method, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/document-conversion/api/v1/#index-document){: new_window}.
-   IBM increased the amount of time before the conversion process times out. These changes might help when your PDF document is complex.
-   A `warnings` field is now included in the response to the `/convert_document` method when the target is `answer_units` and for any request to `/index_document`. For example:

    ```json
    {
      "warnings": [
        {
          "phase": "normalized_html",
          "warning_id": "xpath_not_found",
          "message": "The specified XPath \"//body/div[@id='content']\" was not found in the given document."
        }
      ]
    }
    ```
    {: codeblock}

    For more information about warnings, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/document-conversion/api/v1/#response_index-document){: new_window}.

## 1 April 2016

-   When converting to HTML, the service now gives documents that have no title a title of `no title`. This also fixes a rendering issue that affected some converted HTML documents when they were opened in a browser.
-   There is a new Answer Units warning message: `empty_input_to_converter`. The warning is logged if this step gets empty input and cannot produce an Answer Unit.

## 14 March 2016

-   When converting to Answer Units, a warning messages now provides information about how you could improve your conversion with a custom configuration file.
-   The following parameter names were updated:
    -   The `normalize_html` custom configuration parameter is now `normalized_html`.
    -   The conversion target `ANSWER_UNITS` is now `answer_units`.
    -   The conversion target `NORMALIZED_HTML` is now `normalized_html`.
    -   The conversion target `NORMALIZED_TEXT` is now `normalized_text`.

    The old configuration names are still supported for backward compatibility, but IBM recommends switching to the new lower-case names.

## Older releases
{: #older}

- [30 January 2016](#January2016)

### 30 January 2016
{: #January2016}

-   The metadata from HTML now converts more accurately into Answer Units.
-   HTML whitespace issues have now been fixed. Previously, extra whitespace was inappropriately added or removed. This caused problems with certain whitespace-sensitive languages such as Japanese.
-   The HTML converter now has improved media type support.
-   Invalid XML 1.0 characters are now replaced by the Unicode replacement character (U+FFFD).
-   The HTML converter now supports all XPath 1.0 functions instead of a limited subset.
-   The `ANSWER_UNIT` conversion target output contains new fields:
    -   `media_type_detected` contains the auto-detected media type of the original input document.
    -   `direction` indicates the direction in which the text is to be rendered. Possible values are `ltr` for text in languages that are rendered from left to right and `rtl` for text in languages that are rendered from right to left. If the text of the Answer Unit contains mixed text that is rendered in both directions, the dominant text direction is used as the value of the `direction` field.
