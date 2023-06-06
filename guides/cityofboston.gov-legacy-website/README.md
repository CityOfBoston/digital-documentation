---
description: >-
  ZPCOBWEB01, an IIS 10 webserver, hosts the legacy cityofboston.gov website.
  These applications are either Classic ASP (VBScript) or ASP.net (C# or VB.net)
  apps.
---

# Legacy Website - cityofboston.gov

The applications on this server are all slated for either decommission, or migration to a more appropriate host and/or boston.gov.&#x20;

## Inventory

These docs inform or manage the migration process.

1. From the CMDB project: [https://docs.google.com/spreadsheets/d/14fn46vCwNk\_\_EppXFf5OkM4rf48omU\_rxXWCicf5c3U/edit?usp=sharing](https://docs.google.com/spreadsheets/d/14fn46vCwNk\_\_EppXFf5OkM4rf48omU\_rxXWCicf5c3U/edit?usp=sharing)
2. From the Legacy App Discovery Project:\
   [https://docs.google.com/spreadsheets/d/1Th2htYUHf4LVvbQ\_rWC0eLBhAiHGKjszzO8w2tg9i8g/edit?usp=sharing](https://docs.google.com/spreadsheets/d/1Th2htYUHf4LVvbQ\_rWC0eLBhAiHGKjszzO8w2tg9i8g/edit?usp=sharing)

## Testing/Verification Links

List of applications on this server:

<table data-full-width="true"><thead><tr><th width="227">App</th><th width="193.5">Location</th><th width="263">Test Page/Links</th><th>Notes</th></tr></thead><tbody><tr><td>Assessing</td><td>/assessing/search</td><td><p>use parcel 05204203000 or 0100010000</p><ul><li><a href="https://www.cityofboston.gov/assessing/search">Homepage</a></li><li><a href="https://www.cityofboston.gov/assessing/search/?q=0504203000">Property Search</a> by pid</li><li><a href="https://www.cityofboston.gov/assessing/search/?owner=OLDE%20TOWN%20TEAM%20REALTY%20TRUST">Property Search</a> by owner</li><li><a href="https://www.cityofboston.gov/assessing/search/?pid=0504203000">Property View</a></li><li><a href="http://app01.cityofboston.gov/AssessingMap/?find=0504203000">map</a></li></ul></td><td></td></tr><tr><td>Boards &#x26; Commisions Admin</td><td>//zpappweb01:cityclerk/commissions/account/Login</td><td><ul><li><a href="http://zpappweb01/cityclerk/commissions/account/Login?ReturnUrl=%2Fcityclerk%2Fcommissions%2F">Admin</a></li></ul></td><td>Note this is <strong>not</strong> on <strong>ZPCOBWEB01</strong>, it is on <mark style="color:red;"><strong>ZPAPPWEB01</strong></mark></td></tr><tr><td>City Clerk</td><td>/cityclerk/hearing</td><td><ul><li><a href="https://www.cityofboston.gov/cityclerk/hearing/see.asp?type=ag">Council Meeting Agenda</a></li></ul></td><td></td></tr><tr><td>Doing Business As</td><td>/<a href="https://www.cityofboston.gov/cityclerk/dbasearch/">cityclerk/dbasearch/</a></td><td><p>use business name = "Tow"</p><ul><li><a href="https://www.cityofboston.gov/cityclerk/dbasearch/">Search tool</a></li></ul></td><td></td></tr><tr><td>Dog License</td><td>/animals/licenseapp</td><td><ul><li>use tag A-01036 name Juniper</li><li><a href="https://www.cityofboston.gov/animals/licenseapp/Default.aspx">Application</a></li></ul></td><td></td></tr><tr><td>Fire Permits</td><td>/fire/inspections/sdiform</td><td><ul><li><a href="http://documents.boston.gov/images_documents/RPT%20-%20Fleet%20Counselor%20Services_tcm3-8598.pdf">documents</a></li><li><a href="https://www.cityofboston.gov/fire/inspections/sdiform/firerequest.aspx">request form</a></li></ul></td><td></td></tr><tr><td>ISD Breathe Easy</td><td>/isd/housing/bmc/admin</td><td><p>Use RichardO / test</p><ul><li><a href="https://www.cityofboston.gov/isd/housing/bmc/admin/login.asp">Admin Login</a></li></ul></td><td></td></tr><tr><td>Street Resurfacing</td><td>/publicworks/sweeping</td><td><ul><li><a href="https://www.cityofboston.gov/publicworks/sweeping/Resurfacing.aspx">Homepage</a></li></ul></td><td><ul><li>Classic ASP app</li></ul></td></tr><tr><td>Street Sweeping</td><td>/publicworks/sweeping</td><td><ul><li><a href="https://www.cityofboston.gov/publicworks/sweeping/">Search</a></li><li><a href="https://www.cityofboston.gov/publicworks/sweeping/lyris.asp?email=lisaur1@yahoo.com&#x26;date=04/17/2023">Email Alerts</a></li><li><a href="https://localhost/publicworks/sweeping/admin/Default.aspx">Admin</a></li></ul></td><td><ul><li>aspx (c#) app</li></ul></td></tr><tr><td>Towed Cars</td><td></td><td><p>use plate 123456</p><ul><li><a href="https://www.cityofboston.gov/towing/search/">Search</a></li></ul></td><td></td></tr><tr><td>Video Library</td><td>/cable</td><td><p>Use "street" and "Public Works"</p><ul><li><a href="https://www.cityofboston.gov/cable/video_library.asp">Search</a></li><li><a href="https://www.cityofboston.gov/cable/video_library.asp?id=57241">Example Video</a></li></ul></td><td></td></tr><tr><td>Workers Compensation</td><td>/workerscomp/claiminquiry</td><td><p>Login details in dashlane</p><ul><li><a href="https://www.cityofboston.gov/workerscomp/claiminquiry/">Homepage</a></li><li><a href="https://www.cityofboston.gov/workerscomp/claiminquiry/login.aspx">Admin Login</a></li></ul></td><td></td></tr></tbody></table>

*
