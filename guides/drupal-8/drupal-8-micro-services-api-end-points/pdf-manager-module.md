# PDF Manager

## Requirement

A method of creating PDF's or modifying template PDF's was required to serve customized PDF's for various applications on the website.

The solution needs to be a generic service exposing functions and an interaction workflow to enable different (and new) applications to use the solution (to generate or modify the PDF's as needed) without rewriting the PDF Manager module itself.

## Solution Overview

A Drupal module is provided which can manage the generation of PDF's on boston.gov. The module provides a series of methods and properties which can be used to create, manipulate and access PDF's.

A limitation of all the PHP libraries found (and in general all open-source libraries for all platforms in our tech stack -i.e. php, javascript) was that the form elements in fillable pdfs were removed during processing. This meant that the libraries returned a flat non-fillable PDF even if the original document was fillable. A CLI application was found, PDFToolkit ([pdftk](https://www.pdflabs.com/docs/pdftk-man-page)) which provides specific functionality to manage fillable PDF.

### Phase 1: Flat file PDFs

The PHP library [FPDF](http://www.fpdf.org/) was leveraged to create and edit PDFs along with 2 extensions to allow the [importing of existing PDFs](https://www.setasign.com/products/fpdi/manual/) and the [creation of barcodes](https://github.com/picqer/php-barcode-generator).

During phase 1, a Drupal module PDFManager was created which is capable of:

* adding text or any color, size and supported font to a new or existing document,
* overlay images onto an existing document,
* generate a unique barcode or barcodes and overlay those onto an existing document,
* update the pdf's document properties (author etc)

{% hint style="warning" %}
The module can create a new PDF or alter an existing PDF (e.g. a template). \
However, if a fillable form type PDF is used as a template, the form fields are stripped and the output from the module will be a flat file. \
_This limitation is mostly removed in phase 2._
{% endhint %}

The pdf document manipulation is defined by a json file, and this file can be parameterized. Using the json file Drupal CMS content and/or content from an external database can be injected onto the form.

This module can be used by any other module in Drupal.

### Phase 2: Fillable PDFs

The CLI package [PDFToolkit (pdftk)](https://www.pdflabs.com/docs/pdftk-man-page) was leveraged and a City of Boston managed API was deployed in AWS as a microservice to create and edit fillable PDF's (fillable forms).&#x20;

{% hint style="info" %}
The pdftk runs from a Linux command line.  \
The main Drupal site (served by an Acquia webserver), while running on Linux is not managed by City of Boston and the pdftk libraries are not loaded on that server.  \
Given the short time constraints, the pdftk was deployed within the same container as the DBConnector, leveraging the existing endpoint services (node/javascript/express) and some shellscripting.
{% endhint %}

The PDFManager module functionality was extended using this microservice to:

* insert text into a fillable field in the form
* return a fillable form to the caller (provided a fillable form was used as a template)

{% hint style="danger" %}
With Phase 2, the module can modify and return a fillable form PDF, but still cannot create a fillable form, nor can it add (or remove) fields to a PDF.
{% endhint %}

## PDFManager Module API

The Drupal PDFManager module is found here:&#x20;

```
docroot/modules/custom/bos_components/modules/bos_pdfmanager
```

Adding the code in that folder to a Drupal site, then enabling the module in Drupal is all that is needed to install it.

### Class Inclusion

The actual document manipulations (for both phases) are done by the class PDFManager.

While the PDFManager module is a Drupal module, the actual PDFManager class itself has no dependencies on other Drupal code, and hence can be used in any other PHP application.

The PDFManager class is included in any other class or PHP script by referencing the namespaced module:

```php
<?php
// Reference the PDFManager
use Drupal\bos_pdfmanager\Controller\PdfManager;
```

### Class API

Generally the workflow is to create a new instance of the object, then to pass in static data regarding filenames and data to be applied to the document, and finally to generate the document.

<details>

<summary>The following methods are defined (loosely in the expected order of use):</summary>

**setTemplate(string)**

Sets the template to be used for building the final document.

* _**@param string $filename**_ The absolute path on the server to the template. This must be a pdf.

**setPageData(array)**

Sets an array of data that will be used to insert text and barcodes over the template file and into the final PDF document.

_**@param array $page\_data**_ The raw array, an array with each element containing an array of arrays defining content (text and barcodes) to be inserted into the page.

```
[
  [
    [content],
    [content]
  ],
  [ .. page 2 etc .. ]
]
```

&#x20;   where \[content] is an array for text or a barcode:

```
["type"=>"text", "note"=>"", "x"=>0, "y"=>0, "txt"=>"", "size"=>"", "font"=>"" , "color"=>[]]
["type"=>"barcode", "note"=>"", "x"=>0, "y"=>0, "val"=>"", "encode"=>"C128", "color"=>[]]

WHERE:
type = 'barcode' or 'text'
x = insertion distance from left margin
y = insertion distance from top margin
color = (optional) RGB array (e.g. black = [0,0,0]) color (overrides default)
font = [text only] (optional) Name of the font (overrides default
size = [text only] (optional) size in points for the text (overrides default)
txt = [text only] the text to insert 
val = [barcode only] string to be encoded in barcode (usually a number as a string)
encode = [barcode only] barcode encoding (see class pdfBarcodeElement)
note = (optional) descriptive text for administration
```

* _**@return $this**_ This class

**setDocumentData(array, string)**

Sets an array of data which will be added to the final pdf document properties.

* _**@param array $document\_data**_

```
["output_dest"=>"", "title"=>"", "author"=>"", "subject"=>"", "creator"=>"", "producer"=>""]

WHERE
output_dest = (D=default) I [in-browser], D [download], F [file] or S [text],
title = (optional) Title for the output pdf
author = (optional) Author for the output pdf
subject = (optional) Subject for the output pdf
creator = (optional) Creator for the output pdf (forms only)
producer = (optional) Producer for the output pdf (forms only)
```

* _**@return $this**_ This class

**setFormData(string)**

_\[used for fillable form PDF's only]_\
Sets the form data file (an FDF file) to be used for inserting data into the fields of the template (form) pdf.

* **@param stri**_**ng $filename**_ The absolute path on the server to the form data.
* **@return $this** This class.

**setOutputFilename(string)**

Set the output filename - this is the name of the completed PDF that is delivered to the caller. This is a filename, there is no path required.

* _**@param string $filename**_ The filename that the caller will see/receive.
* _**@return $this**_ This class.

**generate\_fillable()**

Generate a fillable PDF.

* _**@return false|string**_ The PDF filename if successful, or false if failed.

**generate\_flat()**

Generate a flat PDF.

* _**@return \Drupal\bos\_pdfmanager\Controller\PdfFilenames|false|string**_

</details>

<details>

<summary>The following additional utility objects (classes) are defined:</summary>

**pdfTextElement**

This class is used to define a text element that will be "stamped" or overlaid on a base PDF.

**pdfBarcodeElement**

This class is used to define a barcode element that will typically be "stamped" or overlaid on a base PDF

**pdfFilenames**

This class is used to define a file, providing a url, route and physical path to the file, and maintaining some metadata such as if it exists, if it needs deleting etc.

</details>

### Example Use

{% tabs %}
{% tab title="Flat PDF Example" %}
This example shows a simple use of the PDFManager to complete flat PDF.

```php
<?php
  /*
    This function takes the example_template.pdf (a 2 page document) and adds 
    an address to page 1 and a barcode to the top of each page.
    The completed/updated pdf is returned to the caller in a response payload.
  */

  // Reference the PDFManager
  use Drupal\bos_pdfmanager\Controller\PdfManager;

  function example() {
  
    // Initialize the PDFManager.
    $pdf_manager = new PdfManager("Helvetica", "12", [0,0,0]);

    // Set the document properties array.
    $document_data = [
      'title' => 'my document', 'author' => 'me'
    ];
    
    // Set the data we wish to add to the form, in this case add an address mid-page
    // and a barcode on the top of both pages.
    // Note: we are expecting 2 pages in the example_template.pdf file.
    $page_data = [
      [
        ["type"=>"text", "note"=>"Page 1: Address", "x"=>275, "y"=>81, "txt"=>"My Address"],
        ["type"=>"barcode", "note"=>"Page 1: Barcode def", "x"=>350, "y"=>5, "val"=>"1234567890", "encode"=>"C128"]
      ],
      [
        ["type"=>"barcode", "note"=>"Page 2: Barcode def", "x"=>300, "y"=>5, "val"=>"1234567890", "encode"=>"C128"]            
      ]
    ];
    
    try {
      // OK lets make this thing.
      $document = $pdf_manager
        ->setTemplate("example_template.pdf")
        ->setPageData($page_data)
        ->setDocumentData($document_data)
        ->setOutputFilename("example_template_updated.pdf")
        ->generate_flat();
    }
    catch (\Exception $e) {
      // Return a json error string, and a 400 message.
      // If this is Drupal, you might consider logging and then throwing a
      // NotFoundHttpException() (see else clause at bottom of function)
      return new Response(json_encode([
        "error" => $e->getMessage()
      ]), 400);
    }

    if ($document) {
      // Download the file in the callers browser.
      return new new BinaryFileResponse($document, 200, [
        'Content-Type' => 'application/pdf',
        'Content-Disposition' => 'attachment; filename="example_template_updated.pdf"'
      ], true);
    }
    else {
      // Generate and returns a 404 error.
      throw new NotFoundHttpException();     
    }
  }
```
{% endtab %}

{% tab title="Fillable PDF Example" %}
This example shows a simple use of the PDFManager to complete a fillable form PDF.

```php
<?php
  /*
    This function takes the example_template_form.pdf (a 2 page document) and adds 
    a refernce number to page 1 and a barcode to the top of each page.
    It is assumed that the example_template_form.pdf is a fillable form, and the data
    in the example_template_form_data.fdf will be copied into the fields of the form.
    The completed/updated pdf is returned to the caller in a response payload.
  */

  // Reference the PDFManager
  use Drupal\bos_pdfmanager\Controller\PdfManager;

  function example() {
  
    // Initialize the PDFManager.
    $pdf_manager = new PdfManager("Helvetica", "12", [0,0,0]);

    // Set the document properties array.
    $document_data = [
      'title' => 'my document', 'author' => 'me'
    ];
    
    // Set the data we wish to add to the form, in this case a reference number is added top left
    // and a barcode on the top of both pages.
    // Note:
    //     1. In this example, the text is still inserted on the pdf, and is not editable.
    //     2. We are expecting 2 pages in the example_template_form.pdf file.
    $page_data = [
      [
        ["type"=>"text", "note"=>"Page 1: RefNumber", "x"=>300, "y"=>5, "txt"=>"Ref: 202310000"],
        ["type"=>"barcode", "note"=>"Page 1: Barcode def", "x"=>350, "y"=>5, "val"=>"1234567890", "encode"=>"C128"]
      ],
      [
        ["type"=>"barcode", "note"=>"Page 2: Barcode def", "x"=>300, "y"=>5, "val"=>"1234567890", "encode"=>"C128"]            
      ]
    ];
    
    try {
      // OK lets make this thing.
      // setFormData() defines the FDF file which will contain the data for the 
      // fields to be comepleted in the PDF.
      $document = $pdf_manager
        ->setTemplate("example_template_form.pdf")
        ->setFormData("example_template_form_data.fdf")
        ->setPageData($page_data)
        ->setDocumentData($document_data)
        ->setOutputFilename("example_template_form_updated.pdf")
        ->generate_fillable();
    }
    catch (\Exception $e) {
      // Return a json error string, and a 400 message.
      // If this is Drupal, you might consider logging and then throwing a
      // NotFoundHttpException() (see else clause at bottom of function)
      return new Response(json_encode([
        "error" => $e->getMessage()
      ]), 400);
    }

    if ($document) {
      // Download the file in the callers browser.
      return new new BinaryFileResponse($document, 200, [
        'Content-Type' => 'application/pdf',
        'Content-Disposition' => 'attachment; filename="example_template_form_updated.pdf"'
      ], true);
    }
    else {
      // Generate and returns a 404 error.
      throw new NotFoundHttpException();     
    }
  }
```


{% endtab %}

{% tab title="Advanced Examples" %}
There are more complex examples in `bos_assessing` - `pdf.php` and `pdf2.php`, these also show how a json file can be used for managing the text and barcode insertions, and how it can be parameterized so that data can be injected from a database.
{% endtab %}
{% endtabs %}

### Extending the Module.

Additional functionality can be added to the PDFManager in the future, for example to extend it to be able to create fillable pdfs from scratch, and/or to add fillable fields to an existing document.

To extend, simply modify the code in either the `Fpdf.php` or `PdfToolkit.php` classes as needed, and maybe to the `` PdfManager.php` ``

If a new PHP library or remote endpoint is utilized, it is recommended that a new class be created, as in the example below.  This class would then need to be added to `PdfManager.php` and code added and/or new methods exposed to utilize it.&#x20;

```php
<?php

namespace Drupal\bos_pdfmanager\Controller;

use \Drupal\bos_pdfmanager\Controller\PdfFilenames as pdfFilenames;

/**
 * Class: XXXXX
 *

 */
class PdfXxxx implements PdfManagerInterface {
  /**
   * @var \Exception Holds any captured errors.
   */
  public \Exception $error;
}
```

{% hint style="info" %}
All public methods from this class should have error trapping so that errors do not bubble up to PdfManager.  \
When an error occurs capture the error into the class error variable and then return FALSE. Code in the PdfManager should then check for an error and act accordingly.
{% endhint %}
