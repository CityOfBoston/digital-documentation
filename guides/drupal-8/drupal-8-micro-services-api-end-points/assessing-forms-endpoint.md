# Assessing Forms Endpoint

## Overview & Requirement

There is a requirement to provide partially completed, fillable official City of Boston PDF forms to constituents on demand in Q3 of each financial year.

In 2023 for Q3 FY2023, this was migrated into Drupal because the existing PDF solution (a compiled .net solution using a third party iText component) was not available.

A solution was needed where a form would be filled out and returned to the user using "tax bill" data from the current assessing database on VSQL01.&#x20;

The pdf should be available as a "GET" download with a "parameterized" url so it can be used as a dynamic link in webpages.

## Solution

An endpoint was written using PHP in the Drupal framework, utilizing the [PDFManager module](pdf-manager-module.md).

A get endpoint was designed which has a url format of:

```
https://www.boston.gov/assessing-online/form/[form-type]/[financial-year]/[parcel-id]
and
https://www.boston.gov/assessing-online/form/v2/[form-type]/[financial-year]/[parcel-id]
```

The return is the correct form for the form-type (a pdf), completed with information from the property defined by the parcel\_id. The file is downloaded as an application/pdf.

The first link provides a flat file which is not fillable, and the second (v2) is a fillable PDF.
