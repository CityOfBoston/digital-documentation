---
description: >-
  The Digital Team is responsible for maintaining the ASP app and databases
  which drive the assessing online application.
---

# Assessing Online Maintenance

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
| search\default.asp            | 11/2022       |                                                                                                         |
| search\hotline.asp            | 06/2020       | Unknown, looks unused.                                                                                  |
| search\json.asp               | 05/2016       | Unknown, looks unused.                                                                                  |

#### December Activity

The main files which require maintenance are:

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

**Create sandbox ASP page for testing.**

1. Create a copy of `default.asp` named `default20xx.asp`.&#x20;
2. The constant for `AssessingSearchDB` in (line 56 of) `assessing\search\default.asp` indicates the connection string for the page to use. Change it to `assessingupdates` (which will then run SQL queries on the page in the assessingupdates database in the MSSQL Server).

**Make changes to dates and tax rates for next year.**

1. Using data provided by the Assessing Team, update the constants in `default20xx.asp`

Testing/verification of the new data can then be completed at:\
&#x20; `https://cityofboston.gov/assessing/search/default20xx.asp`

</details>

### MSSQL Database

The database which contains the data for the assessing online app is contained on`vsql01` (aka `vsql02`, `zrb01`(?)). This may well be migrated to `vsql71` at some point.&#x20;

The assessing department validate and transfer data to the Digital Team via an MS Access database.  This database contains tables of data that should be mapped and uploaded to databases on the MS SQLServer.

The production database is `assessingsearch` and the staging database is `assessingupdates`.&#x20;

December data should first be imported into `assessingupdates`, verified and then copied to `assessingsearch`

#### December Activity

During December, the Assessing Team will send Digital an updated copy of the MS Access database. &#x20;

The Access database contains tables which can be mapped directly with tables in the MS SQL Databases.

Using an ODBC connection, data can be copied from Access to MSSQL, and then some stored procs executed to make additional data changes.

Testing/verification of the new data can then be completed at:\
&#x20; `https://cityofboston.gov/assessing/search/default20xx.asp`
