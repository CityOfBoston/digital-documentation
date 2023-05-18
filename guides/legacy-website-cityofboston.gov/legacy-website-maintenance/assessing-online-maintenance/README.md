---
description: >-
  The Digital Team is responsible for maintaining the ASP app and databases
  which drive the assessing online application.
---

# Assessing Online Maintenance

### Maintenance Cycle Checklist

{% tabs %}
{% tab title="December" %}
See tabbed notes for December (on this page) for detailed instructions.

* [ ] Create sandbox `default.asp` page for testing,
* [ ] Update the dates and tax rates in sandbox,
* [ ] Update sandbox to connect to the `assessingupdates` MS SQL database,
* [ ] Update MS SQL `assessingupdates` database with data from MS Access,
* [ ] Run any adjustment processes in `assessingupdates` database,
* [ ] Copy new forms into the folder at `wwwcob/assessing/search/forms`,
* [ ] Test sandbox, and re-import data as needed/provided by Assessing Team,
* [ ] Copy data in `assessingupdates` database into `assessingsearch` database,
* [ ] Update sandbox `default.asp` page to connect to `assessingsearch` database and then copy the sandbox page over the live `default.asp` page.
* [ ] Cleanup sandbox and redundant databases and tables.
{% endtab %}

{% tab title="July" %}
See tabbed notes for July (on this page) for detailed instructions.

* [ ] Step 1
{% endtab %}
{% endtabs %}

### ASP Application

The ASP application is served by an IIS Server from `zpcobweb01.web.cob.`

On the Windows server hosting the IIS Server, the ASP files which make up the assessing online app can be found at `d:\wwwcob\assessing`.

The files which are appear to be live (i.e. have no redirect to boston.gov) and which currently live in the folder are:

| Filename                      | Last Modified | Description                                                                                             |
| ----------------------------- | ------------- | ------------------------------------------------------------------------------------------------------- |
| ..\global.asa (wwwcob)        | 2022          | Contains the database connection strings for the whole cityofboston.gov website                         |
| paymentinfo.asp               | 07/2017       |                                                                                                         |
| payments.asp                  | 11/2015       |                                                                                                         |
| res\_prop\_sales\_archive.asp | 03/2016       | _Suspect this file is not live, just an archive of res\_sales\_prop.asp which has not been redirected._ |
| taxratehistory.asp            | 03/2016       |                                                                                                         |
| global.asa (assessing)        | 08/2013       | Contains connection strings which do not appear to be used.                                             |
| paymentshistory\default.aspx  | 03/2014       | Unknown, appears to be a file uploader  - inserts records into the Treasury database on vsql01          |
| search\default.asp            | 11/2022       |                                                                                                         |
| search\hotline.asp            | 06/2020       | Unknown, looks unused.                                                                                  |
| search\json.asp               | 05/2016       | Unknown, looks unused.                                                                                  |

<details>

<summary>wwwcob\global.asa</summary>

This file contains connection strings for the whole cityofboston.gov website.

The strings used by Assessing on-line are found around line 450:\
`assessingsearch` - used for production data,\
`assessingupdates` - used for test data prior to 1 Jan update.

Generally, these do not need to be updated or modified, unless other databases are being used for testing/staging etc.

If new connectionstrings are provided, then the correct DNS name for the target SQL Cluster should be used.  At this time (Dec 2022) the main database server for the IIS Server hosted apps is `vsql01(.web.cob)`but, databases are being migrated to `vsql71(.cityhall.boston.cob).`&#x20;

**`Note:`**Do not use IPAddresses in connection strings. Also use the DNS for the SQL Server cluster, not the DNS entry for the actual Database server. i.e. do not use the server `zpcobsql61.web.cob` instead use the cluster `vsql61.` The cluster will always redirect traffic to the currently active database server during maintenance or fail-over conditions.

</details>

<details>

<summary>wwwcob\assessing\search\default.asp</summary>

This file contains the assessing online search service.

There is a section titled BEGIN CONSTANTS (line 20) to END CONSTANTS (line 70).  This block of code contains a number of constants.  Principally these are dates, but also some tax rate information.  This data is provided by the Assessing Team in Dec each year.  _**Nothing outside of this block requires routine maintenance.**_

</details>

#### Maintenance Cycle

{% tabs %}
{% tab title="December" %}
**Create sandbox ASP page for testing.**

Create a copy of `default.asp` named `default20xx.asp`.&#x20;

**Update DB connection string.**

The constant for `AssessingSearchDB` in (line 56 of) `default.asp/default20xx.asp` indicates the connection string for the page to use.  Change it to `assessingupdates` (which will then run SQL queries on the page in the `assessingupdates` database in the MSSQL Server).

**Make changes to dates and tax rates for next year.**

Using data provided by the Assessing Team, update the constants in `default20xx.asp.` \
_Refer to the notes in the default.asp page next to each constant for guidance._

{% hint style="success" %}
Testing/verification of the new data can now be completed at:\
&#x20; `https://cityofboston.gov/assessing/search/default20xx.asp`
{% endhint %}

{% hint style="info" %}
While testing, you can use the variable `TodaysTime` (around line 70) to test the site for a future date and see that the date fields are aletring the page as expected.
{% endhint %}


{% endtab %}

{% tab title="July" %}

{% endtab %}
{% endtabs %}

### Assessing Forms

There are a number of forms which are provided as part of the assessment process during the first part of each calendar year.  The templates for these forms are saved in `docroot/modules/custom/bos_components/modules/bos_assessing/pdf/FY20xx.`

When a constituent requests a form, it is processed through the `bos_assessing` managed endpoint at `boston.gov/assessing-online/` This endpoint adds a barcode (related to the parcelid) and selected tax bill information onto the form.

See [Assessing Form generation notes here](../../../drupal-8/drupal-8-micro-services-api-end-points/assessing-forms-endpoint.md), and more detailed notes on [PDF generation here.](../../../drupal-8/drupal-8-micro-services-api-end-points/pdf-manager-module.md)

{% hint style="success" %}
Various assessing forms are made available to consitituents based on a calendar. The calendar is set in the constants fields in `wwwcob\assessing\search\default.asp` (see box above).

Once those dates are set, the form display is controlled by code and no further action is needed to enable or disable them.
{% endhint %}

#### Maintenance Cycle

{% tabs %}
{% tab title="December" %}
Each year new forms for exemptions and other request are generated and will be provided by the Assessing Team.

* [ ] Ensure dates (`AbatementExemptionStartThisYear`, `AbatementDeadlineThisYear` and `ExemptionDeadlineThisYear`) are set correctly in `wwwcob\assessing\search\default.asp`
* [ ] Upload the forms (typically 4) to a new folder at `docroot/modules/custom/bos_components/modules/bos_assessing/pdf/FY20xx.`
* [ ] Update the the json and fdf files in the same folder.
{% endtab %}

{% tab title="July" %}

{% endtab %}
{% endtabs %}

### MSSQL Database

The database which contains the data for the assessing online app is contained on`vsql01` (aka `zpdmzsql01, vsql02`, `zrb01`(?)). This may well be migrated to `vsql71` at some point.&#x20;

The assessing department validate and transfer data to the Digital Team via an MS Access database.  This database contains tables of data that should be mapped and uploaded to databases on the MS SQLServer.

The production database is `assessingsearch` and the staging database is `assessingupdates`.&#x20;

December data should first be imported into `assessingupdates`, verified and then copied to `assessingsearch`

#### Maintenance Cycle

{% tabs %}
{% tab title="December" %}
Obtain the updated copy of the MS Access database. &#x20;

Using an ODBC connection copy tables from Access to `assessingupdates` in MS SQL.

Notes from Assessing Team will detail any particular instructions on data manipulations that are required. &#x20;

_For example, in 2022 there is a  stored procedure `sp_update_current_owners` which should be executed to make additional data changes to parcel/property ownership._

{% hint style="success" %}
Testing/verification of the new data can then be completed at:\
&#x20; `https://cityofboston.gov/assessing/search/default20xx.asp`
{% endhint %}
{% endtab %}

{% tab title="July" %}

{% endtab %}
{% endtabs %}
