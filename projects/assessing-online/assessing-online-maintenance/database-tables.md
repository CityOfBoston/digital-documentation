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

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

### bid

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

### condo\_attributes

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

### current\_owners

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

### landuse\_described

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

### outbuildings

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

### parcel

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

### parcel\_all\_owners

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

### propertycodes\_described

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

### Res\_exempt

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

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

| Column | Source | Notes |
| ------ | ------ | ----- |
|        |        |       |
|        |        |       |
|        |        |       |

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

In Linux the following commands were performed:

```
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
If the file size of any one file is too big (typically >75MB), then you may get a "TCP Provider: Error code 0x68" which is probably a memory related issue.  You will need to split the file into smaller files and import each as a chunk.

`split -d -C 75M _Tyler_Real_Estate_Export_File.sql _Tyler_Real_Estate_Export_Files.sql`

This will output a series of +/-75MB files with a numeric-sequence suffix.
{% endhint %}

{% hint style="success" %}
The `mdb-export` tool makes manipulating the Access database fast and without the need to install microsoft office @[see](https://github.com/mdbtools/mdbtools/tree/dev/doc)&#x20;

and,

`sqlcmd` cli is significantly quicker than using, say `azure data studio` or other grapical query tools. @[see](https://learn.microsoft.com/en-us/sql/tools/sqlcmd-utility?view=sql-server-ver16) and @[see](https://learn.microsoft.com/en-us/sql/tools/mssql-cli?source=recommendations\&view=sql-server-ver16)&#x20;
{% endhint %}

``
