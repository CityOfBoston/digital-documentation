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

{% tabs %}
{% tab title="Table Columns" %}
| Column                             | Source    | Notes                       |
| ---------------------------------- | --------- | --------------------------- |
| parcel\_id nvarchar(10)            | MS Access | **PK** The unique parcel ID |
| living\_area int                   | MS Access |                             |
| gross\_area int                    | MS Access |                             |
| year\_built smallint               | MS Access |                             |
| year\_remodeled smallint           | MS Access |                             |
| condo\_units\_residential smallint | MS Access |                             |
| condo\_units\_commercial smallint  | MS Access |                             |
| condo\_units\_mixed smallint       | MS Access |                             |
| stories decimal(4, 1)              | MS Access |                             |
{% endtab %}

{% tab title="Notes" %}
Check the columns in the MS Access database to see if new information is provided.  If the unlikely event that it is, then check with Assessing Dept to see if the info needs to be displayed. &#x20;

\
If so, then the query and HTML table in `default.asp` needs to be updated to display the new information.
{% endtab %}

{% tab title="Populating Table" %}
```sql
-- Create the table
USE assessingupdates2023Q3;

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[additional_data](
	[parcel_id] [nchar](10) NOT NULL,
	[living_area] [int] NULL,
	[gross_area] [int] NULL,
	[year_built] [smallint] NULL,
	[year_remodeled] [smallint] NULL,
	[condo_units_residential] [int] NULL,
	[condo_units_commercial] [int] NULL,
	[condo_units_mixed] [int] NULL,
	[stories] [decimal](4, 1) NULL
) ON [PRIMARY]
GO

-- Create the index
SET ANSI_PADDING ON
GO
CREATE UNIQUE CLUSTERED INDEX [ClusteredIndex-20190701-115214] ON [dbo].[additional_data]
(
	[parcel_id] ASC
) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, IGNORE_DUP_KEY = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, FILLFACTOR = 100) ON [PRIMARY]
GO

-- Insert data from working table
TRUNCATE TABLE dbo.additional_data; 

INSERT INTO dbo.additional_data
    (parcel_id, living_area, gross_area, year_built, year_remodeled, condo_units_residential, condo_units_commercial, condo_units_mixed, stories)
SELECT [parcel_id], ROUND([Living Area] , 0), ROUND([Gross Area], 0), [Year Built], [Year Remodel], 0, [Commercial Units], 0, CONVERT(numeric(4,1),  ISNULL(NULLIF([Story Height],''), 0))
FROM dbo._ADDITIONAL_DATA;

```
{% endtab %}
{% endtabs %}

### bid

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% hint style="warning" %}
Potentially, there is some adjustment of the bid amount depending on billing schedule.
{% endhint %}

_**Check the tables in the MS Access database to see if new BID region/categories have been created.  If the unlikely event there are, then check with Assessing Dept to see if the info needs to be displayed.**_  \
_**If so, then this table will need an additional column for the new bid region/category, and the HTML in**** ****`default.asp`**** ****will need to be updated.**_

{% tabs %}
{% tab title="Table Columns" %}
| Column                  | Source    | Notes                                                                                 |
| ----------------------- | --------- | ------------------------------------------------------------------------------------- |
| parcel\_id nvarchar(10) |           | **PK** The unique parcel ID                                                           |
| bid\_greenway money     | MS Access | This is extracted and compiled from the greenway\_bid table in the MSAccess database. |
| bid\_downtown money     | MS Access | This is extracted and compiled from the greenway\_bid table in the MSAccess database. |






{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Table" %}

{% endtab %}
{% endtabs %}

### condo\_attributes

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}
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
{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Table" %}

{% endtab %}
{% endtabs %}

### current\_owners

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}
| Column                    | Source | Notes  |
| ------------------------- | ------ | ------ |
| Parcel\_id nchar(10)      |        | **PK** |
| seqno tinyint             |        | **PK** |
| owner\_name nvarchar(255) |        |        |
{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Table" %}

{% endtab %}
{% endtabs %}

### landuse\_described

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}
| Column                          | Source | Notes |
| ------------------------------- | ------ | ----- |
| id int                          |        |       |
| Short\_Description nvarchar(10) |        |       |
| Description nvarchar(50)        |        |       |
{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Table" %}

{% endtab %}
{% endtabs %}

### outbuildings

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}
| Column                  | Source | Notes  |
| ----------------------- | ------ | ------ |
| parcel\_id nchar(10)    |        | **PK** |
| line\_number tinyint    |        | **PK** |
| Code nvarchar(50)       |        |        |
| Tot Units decimal(9, 2) |        |        |
| Quantity decimal(9, 2)  |        |        |
| Quality nvarchar(50)    |        |        |
| Condition nvarchar(50)  |        |        |
{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Table" %}

{% endtab %}
{% endtabs %}

### parcel

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}
| Column                      | Source | Notes  |
| --------------------------- | ------ | ------ |
| parcel\_id nchar(10)        |        | **PK** |
| street\_number nvarchar(10) |        |        |
| street\_name nvarchar(50)   |        |        |
| apartment\_no nvarchar(20)  |        |        |
| suffix nvarchar(2)          |        |        |
| landuse nvarchar(2)         |        |        |
| owner nvarchar(255)         |        |        |
| condo\_main nchar(10)       |        |        |
{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Table" %}

{% endtab %}
{% endtabs %}

### parcel\_all\_owners

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}
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
{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Table" %}

{% endtab %}
{% endtabs %}

### propertycodes\_described

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}
| Column                                   | Source | Notes |
| ---------------------------------------- | ------ | ----- |
| property-code smallint                   |        |       |
| property-class tinyint                   |        |       |
| property-class-description nvarchar(255) |        |       |
| property-code-description nvarchar(255)  |        |       |
| property-code-state bit                  |        |       |
| property-code-city bit                   |        |       |
{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Tables" %}

{% endtab %}
{% endtabs %}

### Res\_exempt

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}
| Column                               | Source | Notes |
| ------------------------------------ | ------ | ----- |
| parcel\_id nvarchar(10)              |        |       |
| residential\_exemption nvarchar(255) |        |       |
| personal\_exemption bit              |        |       |
{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Tables" %}
Populating Tables
{% endtab %}
{% endtabs %}

### RESIDENTIAL PROPERTY ATTRIBUTES

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}
| Column                             | Source    | Notes |
| ---------------------------------- | --------- | ----- |
| parcel\_id nvarchar(255)           |           |       |
| Building Seq float                 |           |       |
| Composite Land Use nvarchar(255)   |           |       |
| Building Style nvarchar(255)       |           |       |
| Rooms nvarchar(255)                |           |       |
| Bedrooms nvarchar(255)             |           |       |
| Full Bath float                    |           |       |
| Half Bath float                    |           |       |
| Other Fixtures float               |           |       |
| Bath Style 1 nvarchar(255)         |           |       |
| Bath Style 2 nvarchar(255)         |           |       |
| Bath Style 3 nvarchar(255)         |           |       |
| Kitchens float                     |           |       |
| Kitchen Type nvarchar(255)         |           |       |
| Kitchen Style 1 nvarchar(255)      |           |       |
| Kitchen Style 2 nvarchar(255)      |           |       |
| Kitchen Style 3 nvarchar(255)      |           |       |
| Fireplaces float                   |           |       |
| AC Type nvarchar(255)              |           |       |
| Heat Type nvarchar(255)            |           |       |
| Interior Condition nvarchar(255)   |           |       |
| Interior Finish nvarchar(255)      |           |       |
| View nvarchar(255)                 |           |       |
| Grade nvarchar(255)                |           |       |
| "# of Parking Spots" nvarchar(255) | <h2></h2> |       |
| Year Built float                   |           |       |
| Story Height nvarchar(255)         |           |       |
| Roof Cover nvarchar(255)           |           |       |
| Roof Structure nvarchar(255)       |           |       |
| Exterior Finish nvarchar(255)      |           |       |
| Exterior Condition nvarchar(255)   |           |       |
| Foundation nvarchar(255)           |           |       |
{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Tables" %}

{% endtab %}
{% endtabs %}

### tax\_preliminary

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}
| Column                          | Source | Notes |
| ------------------------------- | ------ | ----- |
| parcel\_id nchar(10)            |        |       |
| Bill Year smallint              |        |       |
| Bill Number int                 |        |       |
| RE Tax Amt decimal(12, 2)       |        |       |
| CPA Amt decimal(12, 2)          |        |       |
| Downtown BID Amt decimal(12, 2) |        |       |
| Greenway BID Amt decimal(12, 2) |        |       |
| Total Billed Amt decimal(12, 2) |        |       |
|                                 |        |       |
|                                 |        |       |
{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Tables" %}

{% endtab %}
{% endtabs %}

### taxbill

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}
| Column                                                                                                                                                                                                                                                    | Source | Notes |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ----- |
| parcel\_id nchar(10)                                                                                                                                                                                                                                      |        |       |
| condo\_main nchar(10)                                                                                                                                                                                                                                     |        |       |
| ward\_precinct\_block nvarchar(7)                                                                                                                                                                                                                         |        |       |
| street\_number nvarchar(10)                                                                                                                                                                                                                               |        |       |
| street\_number\_suffix nvarchar(10)                                                                                                                                                                                                                       |        |       |
| street\_name nvarchar(50)                                                                                                                                                                                                                                 |        |       |
| street\_name\_only nvarchar(50)                                                                                                                                                                                                                           |        |       |
| street\_name\_suffix nvarchar(2)                                                                                                                                                                                                                          |        |       |
| apt\_unit nvarchar(20)                                                                                                                                                                                                                                    |        |       |
| city nvarchar(50)                                                                                                                                                                                                                                         |        |       |
| location\_zip\_code nvarchar(5)                                                                                                                                                                                                                           |        |       |
| location\_zip\_code\_plus\_four nvarchar(4)                                                                                                                                                                                                               |        |       |
| owner nvarchar(50)                                                                                                                                                                                                                                        |        |       |
| mail\_addressee nvarchar(50)                                                                                                                                                                                                                              |        |       |
| mail\_street\_address1 nvarchar(50)                                                                                                                                                                                                                       |        |       |
| mail\_street\_address2 nvarchar(50)                                                                                                                                                                                                                       |        |       |
| mail\_city\_and\_state nvarchar(50)                                                                                                                                                                                                                       |        |       |
| mail\_zip\_code nvarchar(5)                                                                                                                                                                                                                               |        |       |
| mail\_zip\_code\_plus\_four nvarchar(4)                                                                                                                                                                                                                   |        |       |
| country nvarchar(50)                                                                                                                                                                                                                                      |        |       |
| future\_owner nvarchar(50)                                                                                                                                                                                                                                |        |       |
| future\_mail\_addressee nvarchar(50)                                                                                                                                                                                                                      |        |       |
| future\_owner\_mail\_street\_address1 nvarchar(50)                                                                                                                                                                                                        |        |       |
| future\_owner\_mail\_street\_address2 nvarchar(50)                                                                                                                                                                                                        |        |       |
| future\_owner\_mail\_city\_and\_state nvarchar(50)                                                                                                                                                                                                        |        |       |
| future\_owner\_mail\_zip\_code nvarchar(5)                                                                                                                                                                                                                |        |       |
| future\_owner\_mail\_zip\_code\_plus\_four nvarchar(4)                                                                                                                                                                                                    |        |       |
| future\_owner\_country nvarchar(50)                                                                                                                                                                                                                       |        |       |
| land\_area int                                                                                                                                                                                                                                            |        |       |
| land\_use nvarchar(2)                                                                                                                                                                                                                                     |        |       |
| exempt\_code nvarchar(4)                                                                                                                                                                                                                                  |        |       |
| property\_type nvarchar(4)                                                                                                                                                                                                                                |        |       |
| state\_class\_code nvarchar(1)                                                                                                                                                                                                                            |        |       |
| <p>residential_building_value decimal(12, 2)<br>residential_land_value decimal(12, 2)<br>residential_air_rights_value decimal(12, 2)</p>                                                                                                                  |        |       |
| <p>commercial_building_value decimal(12, 2)<br>commercial_land_value decimal(12, 2)<br>commercial_air_rights_value decimal(12, 2)</p>                                                                                                                     |        |       |
| industrial\_building\_value decimal(12, 2) industrial\_land\_value decimal(12, 2) industrial\_air\_rights\_value decimal(12, 2)                                                                                                                           |        |       |
| open\_space\_land\_value decimal(12, 2)                                                                                                                                                                                                                   |        |       |
| exempt\_building\_value decimal(12, 2) exempt\_land\_value decimal(12, 2)                                                                                                                                                                                 |        |       |
| total\_building\_value decimal(12, 2) total\_land\_value decimal(12, 2) total\_value decimal(12, 2)                                                                                                                                                       |        |       |
| latest\_sale\_date date                                                                                                                                                                                                                                   |        |       |
| latest\_bkpgcert nvarchar(10)                                                                                                                                                                                                                             |        |       |
| residential\_exemption\_flag bit                                                                                                                                                                                                                          |        |       |
| coop\_value decimal(12, 2)                                                                                                                                                                                                                                |        |       |
| clause\_abatement\_type\_1 nvarchar(6) clause\_abt\_1\_pct\_ownership nvarchar(5) clause\_abt\_1\_pct\_occupancy nvarchar(5) clause\_abatement\_type\_2 nvarchar(6) clause\_abt\_2\_pct\_ownership nvarchar(5) clause\_abt\_2\_pct\_occupancy nvarchar(5) |        |       |
| paraplegic nvarchar(6)                                                                                                                                                                                                                                    |        |       |
| workoff\_type nvarchar(6) workoff\_credit\_amt decimal(12, 2)                                                                                                                                                                                             |        |       |
| sewer\_betterment\_amt decimal(12, 2) street\_betterment\_amt decimal(12, 2) sidewalk\_betterment\_amt decimal(12, 2)                                                                                                                                     |        |       |
| penalty1\_type nvarchar(6) penalty1\_amt decimal(12, 2) penalty2\_type nvarchar(6) penalty2\_amt decimal(12, 2) penalty3\_type nvarchar(6) penalty3\_amt decimal(12, 2)                                                                                   |        |       |
| <p>fine1_type nvarchar(6) <br>fine1_amt decimal(12, 2) <br>fine2_type nvarchar(6) <br>fine2_amt decimal(12, 2) <br>fine3_type nvarchar(6) <br>fine3_amt decimal(12, 2)</p>                                                                                |        |       |
| <p>BID_type nvarchar(6) <br>BID_amt decimal(12, 2)</p>                                                                                                                                                                                                    |        |       |
| nontax\_collection\_type nvarchar(6) nontax\_collection\_amt decimal(12, 2)                                                                                                                                                                               |        |       |
| personal\_exemption\_flag bit                                                                                                                                                                                                                             |        |       |
| condo\_main\_value decimal(12, 2)                                                                                                                                                                                                                         |        |       |
{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Table" %}

{% endtab %}
{% endtabs %}

### taxes

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}
| Column                                | Source | Notes  |
| ------------------------------------- | ------ | ------ |
| parcel\_id nchar(10)                  |        | **PK** |
| gross\_tax decimal(12, 2)             |        |        |
| net\_tax decimal(12, 2)               |        |        |
| persexempt\_1 decimal(12, 2)          |        |        |
| persexempt\_2 decimal(12, 2)          |        |        |
| persexempt\_total decimal(12, 2)      |        |        |
| resexempt decimal(12, 2)              |        |        |
| cpa decimal(12, 2)                    |        |        |
| code\_enforcement\_tax decimal(12, 2) |        |        |
| 38D\_fine decimal(12, 2)              |        |        |
| sidewalk\_betterment decimal(12, 2)   |        |        |
| street\_betterment decimal(12, 2)     |        |        |
| bill\_number int                      |        |        |
{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Table" %}

{% endtab %}
{% endtabs %}

### value\_history

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}
| Column                 | Source | Notes |
| ---------------------- | ------ | ----- |
| Parcel\_id nchar(10)   |        |       |
| Fiscal\_Year smallint  |        |       |
| Assessed\_value bigint |        |       |
| Land\_use nvarchar(4)  |        |       |
{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Table" %}

{% endtab %}
{% endtabs %}

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
