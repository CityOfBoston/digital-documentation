---
description: >-
  This page contains information on the database tables in assessingsearch on
  VSQL01. (NOTE: assessingupdates is a clone of assessingsearch for dev)
---

# Database Tables

## Database Table Structure

### additional\_data

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

_**Check the columns in the MS Access database to see if new information is provided.  If the unlikely event that it is, then check with Assessing Dept to see if the info needs to be displayed.**_  \
_**If so, then the query and HTML table in**** ****`default.asp`**** ****needs to be updated to display the new information.**_

| Column                              | Source    | Notes |
| ----------------------------------- | --------- | ----- |
| parcel\_id nchar(10)                | MS Access |       |
| living\_area int                    | MS Access |       |
| gross\_area int                     | MS Access |       |
| year\_built smallint                | MS Access |       |
| year\_remodeled smallint            | MS Access |       |
| condo\_units\_residential smallint  | MS Access |       |
| condo\_units\_commercial smallint   | MS Access |       |
| condo\_units\_mixed smallint        | MS Access |       |
| stories decimal(4, 1)               | MS Access |       |

### bid

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% hint style="warning" %}
Potentially, there is some adjustment of the bid amount depending on billing schedule.
{% endhint %}

_**Check the tables in the MS Access database to see if new BID region/categories have been created.  If the unlikely event there are, then check with Assessing Dept to see if the info needs to be displayed.**_  \
_**If so, then this table will need an additional column for the new bid region/category, and the HTML in**** ****`default.asp`**** ****will need to be updated.**_

| Column                  | Source    | Notes                                                                                 |
| ----------------------- | --------- | ------------------------------------------------------------------------------------- |
| parcel\_id nvarchar(10) |           | **Primary Key.** The unique parcel ID                                                 |
| bid\_greenway money     | MS Access | This is extracted and compiled from the greenway\_bid table in the MSAccess database. |
| bid\_downtown money     | MS Access | This is extracted and compiled from the greenway\_bid table in the MSAccess database. |

### condo\_attributes

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column                                   | Source | Notes |
| ---------------------------------------- | ------ | ----- |
| parcel\_id nchar(10)                     |        |       |
| Style nvarchar(20)                       |        |       |
| Exterior Condition nvarchar(14)          |        |       |
| Exterior Wall nvarchar(14)               |        |       |
| Grade nvarchar(14)                       |        |       |
| Stories nvarchar(4)                      |        |       |
| '#Units' decimal(9, 0)                   |        |       |
| Street Type nvarchar(50)                 |        |       |
| Square Feet of Living area decimal(9, 0) |        |       |
| Base Floor nvarchar(4)                   |        |       |
| Fireplaces nvarchar(4)                   |        |       |
| '# Floors' nvarchar(4)                   |        |       |
| Total Rooms nvarchar(4)                  |        |       |
| Bedrooms nvarchar(4)                     |        |       |
| Bathrooms nvarchar(4)                    |        |       |
| Half Bathrooms nvarchar(4)               |        |       |
| Bathroom Style1 nvarchar(14)             |        |       |
| Bathroom Style2 nvarchar(14)             |        |       |
| Bathroom Style3 nvarchar(14)             |        |       |
| Kitchen Style nvarchar(14)               |        |       |
| Kitchen Type nvarchar(14)                |        |       |
| Heat Type nvarchar(14)                   |        |       |
| Interior Condition nvarchar(14)          |        |       |
| Interior Finish nvarchar(14)             |        |       |
| Orientation nvarchar(14)                 |        |       |
| Corner Unit nvarchar(14)                 |        |       |
| View nvarchar(14)                        |        |       |
| condo\_main nchar(10)                    |        |       |
| CNS\_BID decimal(9, 0)                   |        |       |

### current\_owners

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column                    | Source | Notes  |
| ------------------------- | ------ | ------ |
| Parcel\_id nchar(10)      |        | **PK** |
| seqno tinyint             |        | **PK** |
| owner\_name nvarchar(255) |        |        |

### landuse\_described

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column                          | Source | Notes |
| ------------------------------- | ------ | ----- |
| id int                          |        |       |
| Short\_Description nvarchar(10) |        |       |
| Description nvarchar(50)        |        |       |

### outbuildings

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column                  | Source | Notes  |
| ----------------------- | ------ | ------ |
| parcel\_id nchar(10)    |        | **PK** |
| line\_number tinyint    |        | **PK** |
| Code nvarchar(50)       |        |        |
| Tot Units decimal(9, 2) |        |        |
| Quantity decimal(9, 2)  |        |        |
| Quality nvarchar(50)    |        |        |
| Condition nvarchar(50)  |        |        |

### parcel

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column                      | Source | Notes |
| --------------------------- | ------ | ----- |
| parcel\_id nchar(10)        |        |       |
| street\_number nvarchar(10) |        |       |
| street\_name nvarchar(50)   |        |       |
| apartment\_no nvarchar(20)  |        |       |
| suffix nvarchar(2)          |        |       |
| landuse nvarchar(2)         |        |       |
| owner nvarchar(255)         |        |       |
| condo\_main nchar(10)       |        |       |

### parcel\_all\_owners

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column                      | Source | Notes |
| --------------------------- | ------ | ----- |
| parcel\_id nchar(10)        |        |       |
| street\_number nvarchar(10) |        |       |
| street\_name nvarchar(50)   |        |       |
| apartment\_no nvarchar(20)  |        |       |
| suffix nvarchar(2)          |        |       |
| landuse nvarchar(2)         |        |       |
| owner nvarchar(255)         |        |       |
| condo\_main nchar(10)       |        |       |

### propertycodes\_described

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column                                   | Source | Notes |
| ---------------------------------------- | ------ | ----- |
| property-code smallint                   |        |       |
| property-class tinyint                   |        |       |
| property-class-description nvarchar(255) |        |       |
| property-code-description nvarchar(255)  |        |       |
| property-code-state bit                  |        |       |
| property-code-city bit                   |        |       |

### Res\_exempt

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column                               | Source | Notes |
| ------------------------------------ | ------ | ----- |
| parcel\_id nvarchar(10)              |        |       |
| residential\_exemption nvarchar(255) |        |       |
| personal\_exemption bit              |        |       |

### RESIDENTIAL PROPERTY ATTRIBUTES

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

### tax\_preliminary

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

### taxbill

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

### taxes

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

### value\_history

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column                 | Source | Notes |
| ---------------------- | ------ | ----- |
| Parcel\_id nchar(10)   |        |       |
| Fiscal\_Year smallint  |        |       |
| Assessed\_value bigint |        |       |
| Land\_use nvarchar(4)  |        |       |

## Updating Database Tables

As noted elsewhere, data for the MS SQL databases are provided by Asessing in MS Access database/s.

{% hint style="warning" %}
The number of rows in the database tables is high (e.g. 150k - 400k records) and the data size of the tables can be high (30 - 500 MB).

This means a strategy for updating the SQL database with the source data needs to accommodate the size of the import, and the time it will take to import.
{% endhint %}

In 12/2022 for FY2023 the process used was as follows: (using a linux server or workstation)

1. Extract the schema from Access and use to create new import tables in a c.
2. Export the data from each of the source tables in the MS Access databases (2 were supplied) as a series of INSERT statements.
3. Request a new `assessingupdates2023q3` database be created for data import
4. Import the schema into `assessingupdates2023q3` creating working tables as a mirror of the tables in the MS Access database.
5. Import the data into the temp/wprking tables in `assessingupdates2023q3`&#x20;
6. Manipulate the data from the working tables into the "permanent" tables in `assessingupdates2023q3`.

For steps 1-5, (in Linux) the following commands were performed:

```bash
# Install mdbtools, a suite of cli scripts to manipulate access databases on linux/mac
sudo apt-get update && sudo apt-get install mdbtools 

# Export the schema to a file
mdb-schema --drop-table --indexes  FY2023\ Web.accdb  > /path/to/schema.sql
# (repeat for each Access database - Tip append results to same file)
# mdb-schema --drop-table --indexes  FY2023\ Web-Taxes.accdb  >> /path/to/schema.sql

# Export the data in a table into a file using INSERT statements
mdb-export -I access -q "'" FY2023\ Web-Taxes.accdb "FY2023 taxes" > /path/to/_taxes.sq
# (Repeat for each table)

# Install sqlcmd, a cli tool for MS SQL Databases
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add - 
curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list  
sudo apt-get update && sudo apt-get install sqlcmd mssql-tools unixodbc-dev

# Import the table schema
sqlcmd -S vsql01.web.cob -U sqlDigital -P **** -d assessingupdates2023q3 -i /path/to/schema.sql

# Import the data
sqlcmd -S vsql01.web.cob -U sqlDigital -P **** -d assessingupdates2023q3 -i /path/to/_ADDITIONAL_DATA.sql

```

{% hint style="success" %}
**Note:** If the file size of any one file is too big (typically >75MB), then you may get a "TCP Provider: Error code 0x68" which is probably a memory related issue.  You will need to split the file into smaller files and import each as a chunk.

`split -d -C 75M _Tyler_Real_Estate_Export_File.sql _Tyler_Real_Estate_Export_Files.sql`

This will output a series of +/-75MB files with a numeric-sequence suffix.
{% endhint %}

{% hint style="success" %}
**TIP:** With the `sqlcmd` cli, you can run multiple terminals simultaneously. This reduces the overall time to import data.
{% endhint %}

{% hint style="success" %}
**Info:** The `mdb-export` tool makes manipulating the Access database fast and without the need to install microsoft office @[see](https://github.com/mdbtools/mdbtools/tree/dev/doc)&#x20;

and,

`sqlcmd` cli is significantly quicker than using, say `azure data studio` or other grapical query tools. @[see](https://learn.microsoft.com/en-us/sql/tools/sqlcmd-utility?view=sql-server-ver16) and @[see](https://learn.microsoft.com/en-us/sql/tools/mssql-cli?source=recommendations\&view=sql-server-ver16)&#x20;
{% endhint %}

{% hint style="info" %}
**Verify Import**\
Use the `wc` command to count the number of lines in a file, to verify that all records have been imported: e.g.:\
`wc -l taxes.sql` and compare with\
`select count(*) from _Taxes;`\
If the numbers are not the same, you have a problem ... !
{% endhint %}

``
