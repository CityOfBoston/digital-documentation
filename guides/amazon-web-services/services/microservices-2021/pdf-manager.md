# PDFToolkit API

## Requirement

As part of the [PDFManager Module](../../../drupal-8/drupal-8-micro-services-api-end-points/pdf-manager-module.md) creation, a method was required that would allow the manipultaion of fillable (form) PDF's. &#x20;

The requirement was to:

1. Complete a form PDF inserting data into the fields (rather than just stamping un-editable text on the form), and
2. Add a barcode and text to an existing PDF form, and output the resultant PDF as a form _(the v1 PDFManager using a PHP solution always output a flat non-form PDF, even if a form PDF is used as the input)._

This could not be achieved (in 2022) using an opensource PHP module, but there is a well established and proven Linux CLI app which can be utilized, and provided a couple of additional features to the requirement.

The main Drupal site (served by an Acquia webserver), while running on Linux is not managed by City of Boston and the pdftk libraries are not loaded on that server.  Given the short time constraints, the pdftk was deployed within the same container as the DBConnector, leveraging the existing endpoint services (node/javascript/express) and some shellscripting.

## AWS Microservice

The dbconnector service was extended to provide the following endpoints:

{% swagger method="get" path="v1/pdf/heartbeat" baseUrl="/" summary="Ping to test service is available." %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="Expected response." %}
```javascript
{
    "health": "ok"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="/v1/pdf/test" baseUrl=" " summary="Runs an internal test to verify that pdftk is installed properly." %}
{% swagger-description %}
Internally calls the pdftk and captures the version of the cli.
{% endswagger-description %}

{% swagger-response status="200: OK" description="Expected response" %}
```json
{
    "test": "success"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="If there is an issue (pdftk cli fails)" %}
```json
{
    "error": "My Error is .. <error message>"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/v1/pdf/fill" baseUrl=" " summary="Adds data to fields of a PDF form, and outputs a reference to the completed PDF form." %}
{% swagger-description %}
A PDF and data file must be provided.  The PDF must be a fillable form PDF and the data file must be a file in an FDF format.

The /v1/pdf/generate\_fdf endpoint can be used to generate a blank FDF data file.
{% endswagger-description %}

{% swagger-parameter in="body" name="formfile" required="true" %}
A form PDF
{% endswagger-parameter %}

{% swagger-parameter in="body" name="datafile" required="true" %}
A form data file in FDF format
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Returns a reference to a file saved on the endpoint." %}
```javascript
{
    "output": "file-reference"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    "error": 'Error fetching form data file <additional info>'
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="" %}
```javascript
{
    "error": 'Error fetching form data file <additional info>'
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/v1/pdf/overlay" baseUrl=" " summary="Stamps a PDF on to another PDF, and outputs a reference to the merged PDF." %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="basefile" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="overlayfile" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="overwrite" %}
Defaults to "true"
{% endswagger-parameter %}
{% endswagger %}

{% swagger method="get" path="/v1/pdf/decompress" baseUrl=" " summary="Removes compression on a PDF, and returns the decompressed file as an attachment." %}
{% swagger-description %}
This is a useful utility to use the PDFManager cannot manipulate a PDF because its compression is later than PDF1.5.
{% endswagger-description %}

{% swagger-parameter in="query" name="pdf_file" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="query" name="del" %}
Should the file be deleted after it is downloaded. Defaults to "true".
{% endswagger-parameter %}
{% endswagger %}

{% swagger method="post" path="/v1/pdf/metadata" baseUrl=" " summary="Updates the PDF document properties and outputs a reference to the updated PDF." %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="pdf_file" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="meta_data" %}

{% endswagger-parameter %}
{% endswagger %}

{% swagger method="get" path="/v1/pdf/fetch" baseUrl=" " summary="Returns the requested PDF document from its reference." %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="file" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="query" name="del" %}

{% endswagger-parameter %}

{% swagger-parameter in="query" name="show" %}

{% endswagger-parameter %}
{% endswagger %}
