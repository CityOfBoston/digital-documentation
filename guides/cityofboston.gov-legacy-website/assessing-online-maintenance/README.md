---
description: >-
  The Digital Team is responsible for maintaining the ASP app and databases
  which drive the assessing online application.
---

# Assessing Online Maintenance

### Maintenance Cycle Checklist

{% tabs %}
{% tab title="December (Q3)" %}
See tabbed notes for December (on this page) for detailed instructions.

* [ ] Request SQLDBA's to clone the current live database (`assessingupdates20YYQ1` ) to a new database called `assessingupdates20YYQ3` where YY is the current financial year.
* [ ] Create sandbox `default.asp` page  for testing by copying the current `default.asp` to `default20YYQ3.asp` ,
* [ ] Update the dates and tax rates in sandbox,
* [ ] Update sandbox to connect to the `assessingupdates20YYQ3` MS SQL database,
* [ ] Update MS SQL `assessingupdates20YYQ3` database [with data from MS Access,](database-tables.md#updating-database-tables)
* [ ] Run any adjustment processes in `assessingupdates20YYQ3` database,
* [ ] Copy new forms into the folder at `wwwcob/assessing/search/forms`,
* [ ] Test sandbox, and re-import data as needed/provided by Assessing Team,
* [ ] Backup the current `default.asp` page by copying into `default.asp20YYQ3.bak` .
* [ ] To put the new page and data live, simply delete `default.asp` and then rename `default20YYQ3.asp` to`default.asp.`
* [ ] Cleanup any remaining asp pages you may have created and any redundant databases and tables.
{% endtab %}

{% tab title="July (Q1)" %}
See tabbed notes for July (on this page) for detailed instructions.

* [ ] Request SQLDBA's to clone the current live database (`assessingupdates20YYQ3` ) to a new database called `assessingupdates20YYQ1` where YY is the next financial year.
* [ ] Create sandbox `default.asp` page for testing by copying the current `default.asp` to `default20YYQ1.asp`,
* [ ] Update sandbox to connect to the `assessingupdates20YYQ1` MS SQL database,
* [ ] Update MS SQL `assessingupdates20YYQ1` database with [data from MS Access](database-tables.md#updating-database-tables),
* [ ] Run any needed adjustment processes in `assessingupdates20YYQ1` database,
* [ ] To put the new page and data live, simply delete `default.asp` and then rename `default20YYQ1.asp` to`default.asp.`
* [ ] Cleanup any remaining asp pages you may have created and any redundant databases and tables.
{% endtab %}
{% endtabs %}

### ASP Application

The ASP application is served by an IIS Server from `zpcobweb01.web.cob.`

On the Windows server hosting the IIS Server, the ASP files which make up the assessing online app can be found at `d:\wwwcob\assessing`.

The files which are appear to be live (i.e. have no redirect to boston.gov) and which currently live in the folder are:

<table><thead><tr><th width="261">Filename</th><th>Last Modified</th><th>Description</th></tr></thead><tbody><tr><td>..\global.asa (wwwcob)</td><td>2022</td><td>Contains the database connection strings for the whole cityofboston.gov website</td></tr><tr><td>paymentinfo.asp</td><td>07/2017</td><td></td></tr><tr><td>payments.asp</td><td>11/2015</td><td></td></tr><tr><td>res_prop_sales_archive.asp</td><td>03/2016</td><td><em>Suspect this file is not live, just an archive of res_sales_prop.asp which has not been redirected.</em></td></tr><tr><td>taxratehistory.asp</td><td>03/2016</td><td></td></tr><tr><td>global.asa (assessing)</td><td>08/2013</td><td>Contains connection strings which do not appear to be used.</td></tr><tr><td>paymentshistory\default.aspx</td><td>03/2014</td><td>Unknown, appears to be a file uploader  - inserts records into the Treasury database on vsql01</td></tr><tr><td>search\default.asp</td><td>11/2022</td><td></td></tr><tr><td>search\hotline.asp</td><td>06/2020</td><td>Unknown, looks unused.</td></tr><tr><td>search\json.asp</td><td>05/2016</td><td>Unknown, looks unused.</td></tr></tbody></table>

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
{% tab title="December (Q3)" %}
**Create sandbox ASP page for testing.**

Create a copy of `default.asp` named `default20XXQ3.asp`.&#x20;

**Update DB connection string.**

_Once the SQLDBA's have cloned the current live database:_

In the `global.asa` for the IIS server (in the root of wwwcob folder) there are a number of database connection strings defined. Search for `assessingupdates` and you will find a block defining the assessing-online apps connection strings.  Make a new connection string called `assessingupdates20YYQ3` and point it at the new database created by the DBA's.

The constant for `AssessingSearchDB` in (line 56 of) `default20YYQ3.asp` indicates the actual connection string for the page to use.  Change it to `assessingupdates20YYQ3` (which will then run SQL queries on the page in the `assessingupdates20YYQ3` database in the MSSQL Server).

**Make changes to dates and tax rates for next year.**

Using data provided by the Assessing Team, update the constants in `default20YYQ3.asp.` \
_Refer to the notes in the default.asp page next to each constant for guidance._

{% hint style="success" %}
Testing/verification of the new data can now be completed at:\
&#x20; `https://cityofboston.gov/assessing/search/default20YYQ3.asp`
{% endhint %}

{% hint style="info" %}
While testing, you can use the variable `TodaysTime` (around line 70) to test the site for a future date and see that the date fields and the availability of exemption forms links are altering on the page as expected.
{% endhint %}
{% endtab %}

{% tab title="July (Q1)" %}
**Create sandbox ASP page for testing.**

Create a copy of `default.asp` named `default20YYQ1.asp`.&#x20;

**Update DB connection string.**

_Once the SQLDBA's have cloned the current live database:_

In the `global.asa` for the IIS server (in the root of wwwcob folder) there are a number of database connection strings defined. Search for `assessingupdates` and you will find a block defining the assessing-online apps connection strings.  Make a new connection string called `assessingupdates20YYQ1` and point it at the new database created by the DBA's.

The constant for `AssessingSearchDB` in (line 56 of) `default20YYQ1.asp` indicates the actual connection string for the page to use.  Change it to `assessingupdates20YYQ1` (which will then run SQL queries on the page in the `assessingupdates20YYQ1` database in the MSSQL Server).

{% hint style="success" %}
Testing/verification of the new data can now be completed at:\
&#x20; `https://cityofboston.gov/assessing/search/default20YYQ1.asp`
{% endhint %}

{% hint style="info" %}
Typically the dates in the form do not change, and the tax rate should also not change.  Really, the only expected change to `default20YYQ1.asp` is to change the DB connection string name.
{% endhint %}
{% endtab %}
{% endtabs %}

### Assessing Forms

There are a number of forms which are provided as part of the assessment process during the first part of each calendar year.  The templates for these forms are saved in `docroot/modules/custom/bos_components/modules/bos_assessing/pdf/FY20xx.`

When a constituent requests a form, it is processed through the `bos_assessing` managed endpoint at `boston.gov/assessing-online/` This endpoint adds a barcode (related to the parcelid) and selected tax bill information onto the form.

See [Assessing Form generation notes here](../../drupal-8/drupal-8-micro-services-api-end-points/assessing-forms-endpoint.md), and more detailed notes on [PDF generation here.](../../drupal-8/drupal-8-micro-services-api-end-points/pdf-manager-module.md)

{% hint style="success" %}
Various assessing forms are made available to consitituents based on a calendar. The calendar is set in the constants fields in `wwwcob\assessing\search\default.asp` (see box above).

Once those dates are set, the form display is controlled by code and no further action is needed to enable or disable them.
{% endhint %}

#### Maintenance Cycle

{% tabs %}
{% tab title="December (Q3)" %}
Each year new forms for exemptions and other request are generated and will be provided by the Assessing Team.

* [ ] Ensure dates (`AbatementExemptionStartThisYear`, `AbatementDeadlineThisYear` and `ExemptionDeadlineThisYear`) are set correctly in `wwwcob\assessing\search\default20YYQ3.asp`
* [ ] Upload the forms (typically 4) to a new folder at `docroot/modules/custom/bos_components/modules/bos_assessing/pdf/FY20xx.`
* [ ] Update the the json and fdf files in the same folder.
{% endtab %}

{% tab title="July (Q1)" %}
**There is nothing to be done with forms in the July updates.**
{% endtab %}
{% endtabs %}

### MSSQL Database

The database which contains the data for the assessing online app is contained on`vsql01` (aka `zpdmzsql01, vsql02`, `zrb01`(?)). This may well be migrated to `vsql71` at some point.&#x20;

The assessing department validate and transfer data to the Digital Team via an MS Access database.  This database contains tables of data that should be mapped and uploaded to databases on the MS SQLServer.

{% hint style="info" %}
MSAccess data is imported into tables prefixed "\_" in the MSSQL Database.&#x20;

Stored procedures are then written to update/merge data from the import tables into the main tables used by the app. The stored procedure is saved in the DB as a record of the updates and manipulations performed.
{% endhint %}

~~The production database is `assessingsearch` and the staging database is `assessingupdates`~~.&#x20;

~~December data should first be imported into `assessingupdates`, verified and then copied to `assessingsearch`~~

#### Maintenance Cycle

{% tabs %}
{% tab title="December (Q3)" %}
Obtain the updated copy of the MS Access database. &#x20;

In the `assessingupdates20YYQ3` database, delete all tables prefixed "\_", these are import tables from the previous Q1 update.

Following [these instructions](database-tables.md#updating-database-tables), copy tables from Access to `assessingupdates20YYQ3` in MS SQL.

Notes from Assessing Team will detail any particular instructions on data manipulations that are required. &#x20;

To keep a record and for replaying updates, and data updates (from import tables to main tables) and data manipulations should be coded into stored procedures and executed from the stored procedure. \
The stored procedure should be saved in the DB, and named `sp_20YYQ3_import`. If multiple sp's are used, then number them sequentially (`sp_20YYQ3_import_1` etc).

_For example, in 2023Q3 there is a (incorrectly named...) stored procedure `sp_update_current_owners` which should be executed to make additional data changes to parcel/property ownership._

{% hint style="success" %}
Testing/verification of the new data can then be completed at:\
&#x20; `https://cityofboston.gov/assessing/search/default20YYQ3.asp`
{% endhint %}
{% endtab %}

{% tab title="July (Q1)" %}
Obtain the updated copy of the MS Access database. &#x20;

In the `assessingupdates20YYQ1` database, delete all tables prefixed "\_", these are import tables from the previous Q3 update.

Following [these instructions](database-tables.md#updating-database-tables), copy tables from Access to `assessingupdates20YYQ1` in MS SQL.

Notes from Assessing Team will detail any particular instructions on data manipulations that are required. &#x20;

To keep a record and for replaying updates, and data updates (from import tables to main tables) and data manipulations should be coded into stored procedures and executed from the stored procedure. \
The stored procedure should be saved in the DB, and named `sp_20YYQ1_import`. If multiple sp's are used, then number them sequentially (`sp_20YYQ1_import_1` etc).
{% endtab %}
{% endtabs %}
