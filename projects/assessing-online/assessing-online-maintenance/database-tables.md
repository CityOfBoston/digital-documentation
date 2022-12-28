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
USE assessingupdates2023Q3;

-- Create the table
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS dbo.additional_data;
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
SET ANSI_PADDING ON
GO
CREATE UNIQUE CLUSTERED INDEX [ClusteredIndex-115214] ON [dbo].[additional_data]
    ([parcel_id] ASC) 
    WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, IGNORE_DUP_KEY = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, FILLFACTOR = 100) 
    ON [PRIMARY]
GO

-- Insert data from working table, delete existing contents first
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

{% tabs %}
{% tab title="Table Columns" %}
| Column                  | Source    | Notes                                                                                 |
| ----------------------- | --------- | ------------------------------------------------------------------------------------- |
| parcel\_id nvarchar(10) |           | **PK** The unique parcel ID                                                           |
| bid\_greenway money     | MS Access | This is extracted and compiled from the greenway\_bid table in the MSAccess database. |
| bid\_downtown money     | MS Access | This is extracted and compiled from the greenway\_bid table in the MSAccess database. |






{% endtab %}

{% tab title="Notes" %}
Check the tables in the MS Access database to see if new BID region/categories have been created.  If the unlikely event there are, then check with Assessing Dept to see if the info needs to be displayed.  \


If so, then this table will need an additional column for the new bid region/category, and the HTML in `default.asp` will need to be updated.
{% endtab %}

{% tab title="Populating Table" %}
```sql
USE assessingupdates2023Q3;

-- Create the table.
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS dbo.bid;
GO
CREATE TABLE [dbo].[bid](
    [parcel_id] [nvarchar](10) NOT NULL,
    [bid_greenway] [money] NOT NULL,
    [bid_downtown] [money] NOT NULL,
    [bid_newmarket] [money] NOT NULL
) ON [PRIMARY]
GO
SET ANSI_PADDING ON
GO
CREATE UNIQUE CLUSTERED INDEX [ClusteredIndex-013148] ON [dbo].[bid]
    ([parcel_id] ASC)
    WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, IGNORE_DUP_KEY = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
GO

-- Insert the data
TRUNCATE TABLE dbo.bid;
INSERT INTO dbo.bid (parcel_id, bid_greenway, bid_newmarket, bid_downtown)
SELECT parcel.parcel_id, ISNULL(greenway.[Unpaid Balance],0) AS Greenway_BID, ISNULL(newmarket.[Unpaid Balance],0) as Newmarket_BID, ISNULL(downtown.[Unpaid Balance], 0) as Downtown_BID
FROM dbo._Taxes AS parcel
    LEFT JOIN dbo._Greenway_BID AS greenway ON parcel.parcel_id = greenway.parcel_id
    LEFT JOIN dbo._Newmarket_BID AS newmarket ON parcel.parcel_id = newmarket.parcel_id
    LEFT JOIN dbo._Downtown_BID AS downtown ON parcel.parcel_id = downtown.parcel_id
WHERE ISNULL(greenway.[Unpaid Balance], 0) + ISNULL(newmarket.[Unpaid Balance], 0) + ISNULL(downtown.[Unpaid Balance], 0) <> 0 

```
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
```sql
USE assessingupdates2023Q3;

-- Create the table.
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS [dbo].[condo_attributes];
GO 
CREATE TABLE [dbo].[condo_attributes](
    [parcel_id] [nvarchar](10) NULL,
    [Composite Land Use] [nvarchar](255) NULL,
    [Orientation] [nvarchar](255) NULL,
    [Corner Unit] [nvarchar](255) NULL,
    [Floor] [nvarchar](255) NULL,
    [Rooms] [nvarchar](255) NULL,
    [Bedrooms] [nvarchar](255) NULL,
    [Bedroom Type] [nvarchar](255) NULL,
    [Full Bath] [float] NULL,
    [Half Bath] [float] NULL,
    [Other Fixtures] [float] NULL,
    [Bath Style 1] [nvarchar](255) NULL,
    [Bath Style 2] [nvarchar](255) NULL,
    [Bath Style 3] [nvarchar](255) NULL,
    [Kitchens] [float] NULL,
    [Kitchen Type] [nvarchar](255) NULL,
    [Kitchen Style 1] [nvarchar](255) NULL,
    [Kitchen Style 2] [nvarchar](255) NULL,
    [Kitchen Style 3] [nvarchar](255) NULL,
    [Fireplaces] [float] NULL,
    [Penthouse Unit] [bit] NOT NULL,
    [AC Type] [nvarchar](255) NULL,
    [Heat Type] [nvarchar](255) NULL,
    [Year Built] [float] NULL,
    [Interior Condition] [nvarchar](255) NULL,
    [Interior Finish] [nvarchar](255) NULL,
    [View] [nvarchar](255) NULL,
    [Grade] [nvarchar](255) NULL,
    [# of Parking Spots] [nvarchar](255) NULL,
    [Parking Ownership] [nvarchar](255) NULL,
    [Parking Type] [nvarchar](255) NULL,
    [Tandem Parking] [nvarchar](255) NULL,
    [Complex] [nvarchar](255) NULL,
    [Story Height] [nvarchar](255) NULL,
    [Roof Cover] [nvarchar](255) NULL,
    [Roof Structure] [nvarchar](255) NULL,
    [Exterior Condition] [nvarchar](255) NULL,
    [Exterior Finish] [nvarchar](255) NULL,
    [Building Style] [nvarchar](255) NULL,
    [Foundation] [nvarchar](255) NULL
) ON [PRIMARY]
GO
SET ANSI_PADDING ON
GO
CREATE UNIQUE CLUSTERED INDEX [ClusteredIndex-192043] ON [dbo].[condo_attributes]
    ([parcel_id] ASC)
    WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, IGNORE_DUP_KEY = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, FILLFACTOR = 100) ON [PRIMARY]
GO

-- Insert data from working table, delete existing contents first
TRUNCATE TABLE dbo.condo_attributes; 
GO
INSERT INTO dbo.condo_attributes
(   [parcel_id], [Composite Land Use], [Orientation], [Corner Unit], [Floor], [Rooms]
    ,[Bedrooms], [Bedroom Type], [Full Bath], [Half Bath], [Other Fixtures], [Bath Style 1]
    ,[Bath Style 2], [Bath Style 3], [Kitchens], [Kitchen Type], [Kitchen Style 1], [Kitchen Style 2]
    ,[Kitchen Style 3], [Fireplaces], [Penthouse Unit], [AC Type], [Heat Type], [Year Built]
    ,[Interior Condition], [Interior Finish], [View], [Grade], [# of Parking Spots], [Parking Ownership]
    ,[Parking Type], [Tandem Parking], [Complex], [Story Height], [Roof Cover], [Roof Structure]
    ,[Exterior Condition], [Exterior Finish], [Building Style], [Foundation])
SELECT 
     [parcel_id], [Composite Land Use], [Orientation], [Corner Unit], [Floor], [Rooms]
    ,[Bedrooms], [Bedroom Type], [Full Bath], [Half Bath], [Other Fixtures], [Bath Style 1]
    ,[Bath Style 2], [Bath Style 3], [Kitchens], [Kitchen Type], [Kitchen Style 1], [Kitchen Style 2]
    ,[Kitchen Style 3], [Fireplaces], [Penthouse Unit], [AC Type], [Heat Type], [Year Built]
    ,[Interior Condition], [Interior Finish], [View], [Grade], [# of Parking Spots], [Parking Ownership]
    ,[Parking Type], [Tandem Parking], [Complex], [Story Height], [Roof Cover], [Roof Structure]
    ,[Exterior Condition], [Exterior Finish], [Building Style], [Foundation]
FROM dbo._CONDO_PROPERTY_ATTRIBUTES
```
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
It seems that this table only contains owners with the `seqno` >= 1, so the primary current owners.

2022 year only included `seqno` = 1, but the code looks like it will be OK with `seqno` > 1
{% endtab %}

{% tab title="Populating Table" %}
```sql
USE assessingupdates2023Q3;

-- Create the table.
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS dbo.current_owners;
GO
CREATE TABLE [dbo].[current_owners](
    [Parcel_id] [nchar](10) NOT NULL,
    [seqno] [tinyint] NULL,
    [owner_name] [nvarchar](255) NULL
) ON [PRIMARY]
GO
SET ANSI_PADDING ON
GO
CREATE CLUSTERED INDEX [ClusteredIndex-20190701-115106] ON [dbo].[current_owners]
    ([Parcel_id] ASC, [seqno] ASC)
    WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, FILLFACTOR = 100) ON [PRIMARY]
GO

-- Insert the data
TRUNCATE TABLE dbo.current_owners;
GO
INSERT INTO dbo.current_owners ([Parcel_id], [seqno], [owner_name])
SELECT [parcel_id], [seqno], [owner_name]
FROM dbo._CURRENT_OWNERS
WHERE seqno >= 1

```
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
This data should be unchanged between years, but double check with Assessing Team to be sure.
{% endtab %}

{% tab title="Populating Table" %}
Nothing to do, just be sure the table exists from the previous year

```sql
USE assessingupdates2023Q3;

-- Create the table.
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS dbo.Landuse_Described
GO
CREATE TABLE [dbo].[Landuse_Described](
	[id] [int] NOT NULL,
	[Short_Description] [nvarchar](10) NOT NULL,
	[Description] [nvarchar](50) NULL
) ON [PRIMARY]
GO

-- Insert the metadata
TRUNCATE TABLE dbo.Landuse_Described;
GO
INSERT INTO dbo.Landuse_Described ([id], [Short_Description], [Description])
VALUES 	(1, 'A', 'Apartment Building')
	,(2, 'AH', 'Agricultural')
	,(3, 'C', 'Commercial')
	,(4, 'CC', 'Commercial Condo Unit')
	,(5, 'CD', 'Residential Condo Unit')
	,(6, 'CL', 'Commercial Land')
	,(7, 'CM', 'Condo Main Building')
	,(8, 'CP', 'Condo Parking')
	,(9, 'E', 'Exempt')
	,(10, 'EA', 'Exempt - 121A')
	,(11, 'I', 'Industrial')
	,(12, 'R1', 'One Family')
	,(13, 'R2', 'Two Family')
	,(14, 'R3', 'Three Family')
	,(15, 'R4', 'Four to Six Family')
	,(16, 'RC', 'Residential/Commercial') 
	,(17, 'RL', 'Residential Land')

```
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
```sql
USE assessingupdates2023Q3;

-- Create the table.
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS dbo.outbuildings
GO
CREATE TABLE [dbo].[outbuildings](
	[parcel_id] [nchar](10) NOT NULL,
	[line_number] [tinyint] NOT NULL,
	[Code] [nvarchar](255) NOT NULL,
	[Tot Units] [decimal](9, 2) NOT NULL,
	[Quality] [nvarchar](50) NOT NULL,
	[Condition] [nvarchar](50) NOT NULL
) ON [PRIMARY]
GO
SET ANSI_PADDING ON
GO
CREATE UNIQUE CLUSTERED INDEX [ClusteredIndex-135523] ON [dbo].[outbuildings]
    ([parcel_id] ASC, [line_number] ASC)
    WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, IGNORE_DUP_KEY = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
GO

-- Insert data from working table, delete existing contents first
TRUNCATE TABLE dbo.condo_attributes; 
GO
INSERT INTO [dbo].[outbuildings] ([parcel_id],[line_number],[Code],[Tot Units],[Quality],[Condition])
SELECT [parcel_id]
    ,ROW_NUMBER() OVER (
        PARTITION BY parcel_id
        order by parcel_id
    ) AS line_number
    ,[Code]
    ,ISNULL([Tot Units], 0)
    ,ISNULL([Quality], ' ')
    ,ISNULL([Condition], ' ')
  FROM [dbo].[_SPECIAL_FEATURES]
```
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
<pre class="language-sql"><code class="lang-sql"><strong>USE assessingupdates2023Q3;
</strong>
-- Create the table.
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS dbo.parcel;
GO
CREATE TABLE [dbo].[parcel](
	[parcel_id] [nchar](10) NOT NULL,
	[street_number] [nvarchar](10) NULL,
	[street_name] [nvarchar](50) NULL,
	[apartment_no] [nvarchar](20) NULL,
	[suffix] [nvarchar](2) NULL,
	[landuse] [nvarchar](2) NULL,
	[owner] [nvarchar](255) NULL,
	[condo_main] [nchar](10) NULL
) ON [PRIMARY]
GO
SET ANSI_PADDING ON
GO
CREATE UNIQUE CLUSTERED INDEX [ClusteredIndex-114720] ON [dbo].[parcel]
    ([parcel_id] ASC)
    WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, IGNORE_DUP_KEY = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, FILLFACTOR = 100) ON [PRIMARY]
GO

-- Insert data from working table, delete existing contents first
TRUNCATE TABLE dbo.condo_attributes; 
GO
INSERT INTO [dbo].[parcel] ([parcel_id], [street_number], [street_name], 
    [apartment_no], [suffix], [landuse], [owner], [condo_main])
SELECT [parcel_id], ISNULL([street_number], '') street_number, SUBSTRING(TRIM([street_name]), 1, LEN(TRIM(street_name)) - 2) street_name, 
    ISNULL([apt_unit], '') apartment_no, RIGHT(TRIM(street_name), 2) suffix, [land_use], [owner], [condo_main]
FROM dbo._Tyler_Real_Estate_Export_File
order by parcel_id
</code></pre>
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
```sql
USE assessingupdates2023Q3;

-- Create the table.SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS dbo.parcel_all_owners;
GO
CREATE TABLE [dbo].[parcel_all_owners](
	[parcel_id] [nchar](10) NOT NULL,
	[street_number] [nvarchar](10) NULL,
	[street_name] [nvarchar](50) NULL,
	[apartment_no] [nvarchar](20) NULL,
	[suffix] [nvarchar](2) NULL,
	[landuse] [nvarchar](2) NULL,
	[owner] [nvarchar](255) NULL,
	[condo_main] [nchar](10) NULL
) ON [PRIMARY]
GO
SET ANSI_PADDING ON
GO
CREATE CLUSTERED INDEX [ClusteredIndex-114826] ON [dbo].[parcel_all_owners]
   ([parcel_id] ASC)
    WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, FILLFACTOR = 100) ON [PRIMARY]
GO
SET ANSI_PADDING ON
GO
CREATE NONCLUSTERED INDEX [NonClusteredIndex-115431] ON [dbo].[parcel_all_owners]
    ([owner] ASC)
    WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, FILLFACTOR = 100) ON [PRIMARY]
GO

-- Insert data from working table, delete existing contents first
TRUNCATE TABLE dbo.parcel_all_owners; 
GO
INSERT INTO [dbo].[parcel_all_owners] ([parcel_id], [street_number], [street_name], [apartment_no], 
    [suffix], [landuse], [owner], [condo_main])
SELECT tyler.[parcel_id], ISNULL([street_number], '') street_number, SUBSTRING(TRIM([street_name]), 1, LEN(TRIM(street_name)) - 2) street_name, ISNULL([apt_unit], '') apartment_no, 
    RIGHT(TRIM(street_name), 2) suffix, [land_use], owners.[owner_name], [condo_main]
FROM dbo._Tyler_Real_Estate_Export_File AS tyler
    INNER JOIN dbo.current_owners as owners ON tyler.parcel_id = owners.parcel_id
order by parcel_id
```
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
This data should be unchanged between years, but double check with Assessing Team to be sure.This data should be unchanged between years, but double check with Assessing Team to be sure.
{% endtab %}

{% tab title="Populating Tables" %}
```sql
USE assessingupdates2023Q3;

-- Create the table.
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS assessingupdates2023Q3.dbo.propertycodes_described
GO
CREATE TABLE [assessingupdates2023Q3].[dbo].[propertycodes_described](
	[property-code] [smallint] NOT NULL,
	[property-class] [tinyint] NOT NULL,
	[property-class-description] [nvarchar](255) NOT NULL,
	[property-code-description] [nvarchar](255) NOT NULL,
	[property-code-state] [bit] NOT NULL,
	[property-code-city] [bit] NOT NULL
) ON [PRIMARY]
GO

-- Insert data from working table, delete existing contents first
TRUNCATE TABLE [assessingupdates2023Q3].[dbo].[propertycodes_described] 
GO
INSERT INTO [assessingupdates2023Q3].[dbo].[propertycodes_described]
    ([property-code], [property-class], [property-class-description], [property-code-description]
    ,[property-code-state], [property-code-city])
SELECT [property-code], [property-class], [property-class-description], [property-code-description]
    ,[property-code-state], [property-code-city]
  FROM [assessingsearch].[dbo].[propertycodes_described]
```
{% endtab %}
{% endtabs %}

### Res\_exempt

{% hint style="info" %}
This table is accessed from `default.asp`.
{% endhint %}

{% tabs %}
{% tab title="Table Columns" %}


| Column                               | Source                                 | Notes                                                                                          |
| ------------------------------------ | -------------------------------------- | ---------------------------------------------------------------------------------------------- |
| parcel\_id nvarchar(10)              | MSAccess:Tyler Real Estate Export File |                                                                                                |
| residential\_exemption nvarchar(255) | MSAccess:Tyler Real Estate Export File |                                                                                                |
| personal\_exemption bit              | MSAccess:Taxes                         | Defaults to 0 but is 1 if either of `Personal Ex Type 1` or `Personal Ex Type 2` is not null.  |


{% endtab %}

{% tab title="Notes" %}

{% endtab %}

{% tab title="Populating Tables" %}
```sql
USE assessingupdates2023Q3;

-- Create the table.
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS [dbo].[Res_exempt]
GO
CREATE TABLE [dbo].[Res_exempt](
	[parcel_id] [nvarchar](10) NULL,
	[residential_exemption] [nvarchar](1) NULL,
	[personal_exemption] [bit] NOT NULL
) ON [PRIMARY]
GO

-- Insert data from working table, delete existing contents first
TRUNCATE TABLE [dbo].[Res_exempt] 
GO
INSERT INTO [dbo].[Res_exempt]
    ([parcel_id], [residential_exemption], [personal_exemption])
SELECT tyler.parcel_id, IIF(ISNULL(tyler.residential_exemption_flag, 'N') = 'Y', 'Y', 'N') as Residential, IIF(ISNULL(taxes.[Personal Ex Type 1], '0') + ISNULL(taxes.[Personal Ex Type 2], '0') = '00', 0, 1) AS personal
FROM _Tyler_Real_Estate_Export_File tyler
    INNER JOIN _Taxes taxes ON tyler.parcel_id = taxes.parcel_id
```
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
```sql
USE assessingupdates2023Q3;

-- Create the table.
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS [dbo].[RESIDENTIAL PROPERTY ATTRIBUTES]
GO
CREATE TABLE [dbo].[RESIDENTIAL PROPERTY ATTRIBUTES](
	[parcel_id] [nvarchar](10) NULL,
	[line_number] [nvarchar](255) NULL,
	[Composite Land Use] [nvarchar](255) NULL,
	[Building Style] [nvarchar](255) NULL,
	[Rooms] [nvarchar](255) NULL,
	[Bedrooms] [nvarchar](255) NULL,
	[Full Bath] [float] NULL,
	[Half Bath] [float] NULL,
	[Other Fixtures] [float] NULL,
	[Bath Style 1] [nvarchar](255) NULL,
	[Bath Style 2] [nvarchar](255) NULL,
	[Bath Style 3] [nvarchar](255) NULL,
	[Kitchens] [float] NULL,
	[Kitchen Type] [nvarchar](255) NULL,
	[Kitchen Style 1] [nvarchar](255) NULL,
	[Kitchen Style 2] [nvarchar](255) NULL,
	[Kitchen Style 3] [nvarchar](255) NULL,
	[Fireplaces] [float] NULL,
	[AC Type] [nvarchar](255) NULL,
	[Heat Type] [nvarchar](255) NULL,
	[Interior Condition] [nvarchar](255) NULL,
	[Interior Finish] [nvarchar](255) NULL,
	[View] [nvarchar](255) NULL,
	[Grade] [nvarchar](255) NULL,
	[# of Parking Spots] [nvarchar](255) NULL,
	[Year Built] [float] NULL,
	[Story Height] [nvarchar](255) NULL,
	[Roof Cover] [nvarchar](255) NULL,
	[Roof Structure] [nvarchar](255) NULL,
	[Exterior Finish] [nvarchar](255) NULL,
	[Exterior Condition] [nvarchar](255) NULL,
	[Foundation] [nvarchar](255) NULL
) ON [PRIMARY]
GO
SET ANSI_PADDING ON
GO
CREATE UNIQUE CLUSTERED INDEX [ClusteredIndex-135336] ON [dbo].[RESIDENTIAL PROPERTY ATTRIBUTES]
    ([parcel_id] ASC, [line_number] ASC)
    WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, IGNORE_DUP_KEY = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
GO

-- Insert data from working table, delete existing contents first
TRUNCATE TABLE [dbo].[RESIDENTIAL PROPERTY ATTRIBUTES] 
GO
INSERT INTO dbo.[RESIDENTIAL PROPERTY ATTRIBUTES] 
    ([parcel_id], [line_number], [Composite Land Use], [Building Style], [Rooms]
    ,[Bedrooms], [Full Bath], [Half Bath], [Other Fixtures], [Bath Style 1]
    ,[Bath Style 2], [Bath Style 3], [Kitchens], [Kitchen Type], [Kitchen Style 1]
    ,[Kitchen Style 2], [Kitchen Style 3], [Fireplaces], [AC Type], [Heat Type]
    ,[Interior Condition], [Interior Finish], [View], [Grade]
    ,[# of Parking Spots], [Year Built], [Story Height], [Roof Cover], [Roof Structure]
    ,[Exterior Condition], [Exterior Finish], [Foundation])
SELECT [parcel_id], [line_number], [Composite Land Use], [Building Style], [Rooms]
    ,[Bedrooms], [Full Bath], [Half Bath], [Other Fixtures], [Bath Style 1]
    ,[Bath Style 2], [Bath Style 3], [Kitchens], [Kitchen Type], [Kitchen Style 1]
    ,[Kitchen Style 2], [Kitchen Style 3], [Fireplaces], [AC Type], [Heat Type]
    ,[Interior Condition], [Interior Finish], [View], [Grade]
    ,[# of Parking Spots], [Year Built], [Story Height], [Roof Cover], [Roof Structure]
    ,[Exterior Condition], [Exterior Finish], [Foundation]
  FROM [assessingupdates2023Q3].[dbo].[_RESIDENTIAL_PROPERTY_ATTRIBUTES]
```
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
```sql
USE assessingupdates2023Q3;

-- Create the table.
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS [dbo].[tax_preliminary]
GO
CREATE TABLE [dbo].[tax_preliminary](
	[parcel_id] [nchar](10) NOT NULL,
	[Bill Year] [smallint] NULL,
	[Bill Number] [int] NULL,
	[RE Tax Amt] [decimal](12, 2) NULL,
	[CPA Amt] [decimal](12, 2) NULL,
	[Downtown BID Amt] [decimal](12, 2) NULL,
	[Greenway BID Amt] [decimal](12, 2) NULL,
	[Newmarket BID Amt] [decimal](12, 2) NULL,
	[Total Billed Amt] [decimal](12, 2) NULL
) ON [PRIMARY]
GO
SET ANSI_PADDING ON
GO
CREATE UNIQUE CLUSTERED INDEX [ClusteredIndex-012238] ON [dbo].[tax_preliminary]
    ([parcel_id] ASC)
    WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, IGNORE_DUP_KEY = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
GO

-- Insert data from working table, delete existing contents first
TRUNCATE TABLE [dbo].[tax_preliminary] 
GO
INSERT INTO [dbo].[tax_preliminary]
    ([parcel_id], [Bill Year], [Bill Number], [RE Tax Amt], [CPA Amt]
    , [Downtown BID Amt], [Greenway BID Amt], [Newmarket BID Amt], [Total Billed Amt]) 
SELECT taxes.parcel_id, 2023, taxes.bill_number
    , CONVERT(decimal(12,2), taxes.net_tax )
    , CONVERT(decimal(12,2), isnull(taxes.cpa, 0))
    , CONVERT(decimal(12,2), isnull(downtown.Billed, 0) )
    , CONVERT(decimal(12,2), isnull(greenway.Billed, 0) )
    , CONVERT(decimal(12,2), isnull(newmarket.Billed, 0) )
    , CONVERT(decimal(12,2), (taxes.net_tax + isnull(taxes.cpa, 0) + isnull(downtown.Billed, 0) + isnull(greenway.Billed, 0) + isnull(newmarket.Billed, 0)) ) AS total
FROM dbo.taxes AS taxes
    LEFT OUTER JOIN dbo.[_Downtown_BID] AS downtown ON taxes.parcel_id = downtown.parcel_id
    LEFT OUTER JOIN dbo.[_Greenway_BID] AS greenway ON taxes.parcel_id = greenway.parcel_id
    LEFT OUTER JOIN dbo.[_Newmarket_BID] AS newmarket ON taxes.parcel_id = newmarket.parcel_id
    

```
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
```sql
USE assessingupdates2023Q3;

-- Create the table.
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS [dbo].[taxbill]
GO
CREATE TABLE [dbo].[taxbill](
	[parcel_id] [nchar](10) NOT NULL,
	[condo_main] [nchar](10) NULL,
	[ward_precinct_block] [nvarchar](7) NOT NULL,
	[street_number] [nvarchar](10) NOT NULL,
	[street_number_suffix] [nvarchar](10) NOT NULL,
	[street_name] [nvarchar](50) NOT NULL,
	[street_name_only] [nvarchar](50) NOT NULL,
	[street_name_suffix] [nvarchar](2) NOT NULL,
	[apt_unit] [nvarchar](20) NOT NULL,
	[city] [nvarchar](50) NOT NULL,
	[location_zip_code] [nvarchar](5) NOT NULL,
	[location_zip_code_plus_four] [nvarchar](4) NULL,
	[owner] [nvarchar](50) NOT NULL,
	[mail_addressee] [nvarchar](50) NOT NULL,
	[mail_street_address1] [nvarchar](50) NOT NULL,
	[mail_street_address2] [nvarchar](50) NOT NULL,
	[mail_city_and_state] [nvarchar](50) NOT NULL,
	[mail_zip_code] [nvarchar](5) NOT NULL,
	[mail_zip_code_plus_four] [nvarchar](4) NOT NULL,
	[country] [nvarchar](50) NOT NULL,
	[future_owner] [nvarchar](50) NOT NULL,
	[future_mail_addressee] [nvarchar](50) NOT NULL,
	[future_owner_mail_street_address1] [nvarchar](50) NOT NULL,
	[future_owner_mail_street_address2] [nvarchar](50) NOT NULL,
	[future_owner_mail_city_and_state] [nvarchar](50) NOT NULL,
	[future_owner_mail_zip_code] [nvarchar](5) NOT NULL,
	[future_owner_mail_zip_code_plus_four] [nvarchar](4) NOT NULL,
	[future_owner_country] [nvarchar](50) NOT NULL,
	[land_area] [int] NULL,
	[land_use] [nvarchar](2) NOT NULL,
	[exempt_code] [nvarchar](4) NOT NULL,
	[property_type] [nvarchar](4) NOT NULL,
	[state_class_code] [nvarchar](1) NOT NULL,
	[residential_building_value] [decimal](12, 2) NOT NULL,
	[residential_land_value] [decimal](12, 2) NOT NULL,
	[residential_air_rights_value] [decimal](12, 2) NULL,
	[commercial_building_value] [decimal](12, 2) NOT NULL,
	[commercial_land_value] [decimal](12, 2) NOT NULL,
	[commercial_air_rights_value] [decimal](12, 2) NULL,
	[industrial_building_value] [decimal](12, 2) NOT NULL,
	[industrial_land_value] [decimal](12, 2) NOT NULL,
	[industrial_air_rights_value] [decimal](12, 2) NULL,
	[open_space_land_value] [decimal](12, 2) NULL,
	[exempt_building_value] [decimal](12, 2) NOT NULL,
	[exempt_land_value] [decimal](12, 2) NOT NULL,
	[total_building_value] [decimal](12, 2) NOT NULL,
	[total_land_value] [decimal](12, 2) NOT NULL,
	[total_value] [decimal](12, 2) NOT NULL,
	[latest_sale_date] [date] NULL,
	[latest_bkpgcert] [nvarchar](10) NOT NULL,
	[residential_exemption_flag] [bit] NOT NULL,
	[coop_value] [decimal](12, 2) NOT NULL,
	[clause_abatement_type_1] [nvarchar](6) NOT NULL,
	[clause_abt_1_pct_ownership] [nvarchar](5) NOT NULL,
	[clause_abt_1_pct_occupancy] [nvarchar](5) NOT NULL,
	[clause_abatement_type_2] [nvarchar](6) NOT NULL,
	[clause_abt_2_pct_ownership] [nvarchar](5) NOT NULL,
	[clause_abt_2_pct_occupancy] [nvarchar](5) NOT NULL,
	[paraplegic] [nvarchar](6) NOT NULL,
	[workoff_type] [nvarchar](6) NOT NULL,
	[workoff_credit_amt] [decimal](12, 2) NOT NULL,
	[sewer_betterment_amt] [decimal](12, 2) NOT NULL,
	[street_betterment_amt] [decimal](12, 2) NOT NULL,
	[sidewalk_betterment_amt] [decimal](12, 2) NOT NULL,
	[penalty1_type] [nvarchar](6) NOT NULL,
	[penalty1_amt] [decimal](12, 2) NOT NULL,
	[penalty2_type] [nvarchar](6) NOT NULL,
	[penalty2_amt] [decimal](12, 2) NOT NULL,
	[penalty3_type] [nvarchar](6) NOT NULL,
	[penalty3_amt] [decimal](12, 2) NOT NULL,
	[fine1_type] [nvarchar](6) NOT NULL,
	[fine1_amt] [decimal](12, 2) NOT NULL,
	[fine2_type] [nvarchar](6) NOT NULL,
	[fine2_amt] [decimal](12, 2) NOT NULL,
	[fine3_type] [nvarchar](6) NOT NULL,
	[fine3_amt] [decimal](12, 2) NOT NULL,
	[BID_type] [nvarchar](6) NOT NULL,
	[BID_amt] [decimal](12, 2) NOT NULL,
	[nontax_collection_type] [nvarchar](6) NOT NULL,
	[nontax_collection_amt] [decimal](12, 2) NULL,
	[personal_exemption_flag] [bit] NOT NULL,
	[condo_main_value] [decimal](12, 2) NULL
) ON [PRIMARY]
GO
SET ANSI_PADDING ON
GO
CREATE UNIQUE CLUSTERED INDEX [ClusteredIndex-133903] ON [dbo].[taxbill]
    ([parcel_id] ASC) 
    WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, IGNORE_DUP_KEY = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, FILLFACTOR = 100) ON [PRIMARY]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_ward_precinct_block]  DEFAULT (N'=') FOR [ward_precinct_block]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_street_number]  DEFAULT (N'=') FOR [street_number]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_street_number_suffix]  DEFAULT (N'=') FOR [street_number_suffix]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_street_name]  DEFAULT (N'=') FOR [street_name]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_street_name_only]  DEFAULT (N'=') FOR [street_name_only]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_street_name_suffix]  DEFAULT (N'=') FOR [street_name_suffix]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_apt_unit]  DEFAULT (N'=') FOR [apt_unit]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_city]  DEFAULT (N'=') FOR [city]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_location_zip_code]  DEFAULT (N'=') FOR [location_zip_code]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_owner]  DEFAULT (N'=') FOR [owner]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_mail_addressee]  DEFAULT (N'=') FOR [mail_addressee]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_mail_street_address1]  DEFAULT (N'=') FOR [mail_street_address1]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_mail_street_address2]  DEFAULT (N'=') FOR [mail_street_address2]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_mail_city_and_state]  DEFAULT (N'=') FOR [mail_city_and_state]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_mail_zip_code]  DEFAULT (N'=') FOR [mail_zip_code]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_mail_zip_code_plus_four]  DEFAULT (N'=') FOR [mail_zip_code_plus_four]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_country]  DEFAULT (N'=') FOR [country]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_future_owner]  DEFAULT (N'=') FOR [future_owner]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_future_mail_addressee]  DEFAULT (N'=') FOR [future_mail_addressee]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_future_owner_mail_street_address1]  DEFAULT (N'=') FOR [future_owner_mail_street_address1]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_future_owner_mail_street_address2]  DEFAULT (N'=') FOR [future_owner_mail_street_address2]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_future_owner_mail_city_and_state]  DEFAULT (N'=') FOR [future_owner_mail_city_and_state]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_future_owner_mail_zip_code]  DEFAULT (N'=') FOR [future_owner_mail_zip_code]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_future_owner_mail_zip_code_plus_four]  DEFAULT (N'=') FOR [future_owner_mail_zip_code_plus_four]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_future_owner_country]  DEFAULT (N'=') FOR [future_owner_country]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_land_area]  DEFAULT ((0)) FOR [land_area]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_land_use]  DEFAULT (N'=') FOR [land_use]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_exempt_code]  DEFAULT (N'=') FOR [exempt_code]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_property_type]  DEFAULT (N'=') FOR [property_type]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_state_class_code]  DEFAULT (N'=') FOR [state_class_code]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_residential_building_value]  DEFAULT ((0)) FOR [residential_building_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_residential_land_value]  DEFAULT ((0)) FOR [residential_land_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_residential_air_rights_value]  DEFAULT ((0)) FOR [residential_air_rights_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_commercial_building_value]  DEFAULT ((0)) FOR [commercial_building_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_commercial_land_value]  DEFAULT ((0)) FOR [commercial_land_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_commercial_air_rights_value]  DEFAULT ((0)) FOR [commercial_air_rights_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_industrial_building_value]  DEFAULT ((0)) FOR [industrial_building_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_industrial_land_value]  DEFAULT ((0)) FOR [industrial_land_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_industrial_air_rights_value]  DEFAULT ((0)) FOR [industrial_air_rights_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_open_space_land_value]  DEFAULT ((0)) FOR [open_space_land_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_exempt_building_value]  DEFAULT ((0)) FOR [exempt_building_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_exempt_land_value]  DEFAULT ((0)) FOR [exempt_land_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_total_building_value]  DEFAULT ((0)) FOR [total_building_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_total_land_value]  DEFAULT ((0)) FOR [total_land_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_total_value]  DEFAULT ((0)) FOR [total_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_latest_bkpgcert]  DEFAULT (N'=') FOR [latest_bkpgcert]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_residential_exemption_flag]  DEFAULT ((0)) FOR [residential_exemption_flag]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_coop_value]  DEFAULT ((0)) FOR [coop_value]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_clause_abatement_type_1]  DEFAULT (N'=') FOR [clause_abatement_type_1]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_clause_abt_1_pct_ownership]  DEFAULT (N'=') FOR [clause_abt_1_pct_ownership]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_clause_abt_1_pct_occupancy]  DEFAULT (N'=') FOR [clause_abt_1_pct_occupancy]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_clause_abatement_type_2]  DEFAULT (N'=') FOR [clause_abatement_type_2]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_clause_abt_2_pct_ownership]  DEFAULT (N'=') FOR [clause_abt_2_pct_ownership]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_clause_abt_2_pct_occupancy]  DEFAULT (N'=') FOR [clause_abt_2_pct_occupancy]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_paraplegic]  DEFAULT (N'=') FOR [paraplegic]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_workoff_type]  DEFAULT (N'=') FOR [workoff_type]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_workoff_credit_amt]  DEFAULT ((0)) FOR [workoff_credit_amt]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_sewer_betterment_amt]  DEFAULT ((0)) FOR [sewer_betterment_amt]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_street_betterment_amt]  DEFAULT ((0)) FOR [street_betterment_amt]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_sidewalk_betterment_amt]  DEFAULT ((0)) FOR [sidewalk_betterment_amt]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_penalty1_type]  DEFAULT (N'=') FOR [penalty1_type]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_penalty1_amt]  DEFAULT ((0)) FOR [penalty1_amt]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_penalty2_type]  DEFAULT (N'=') FOR [penalty2_type]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_penalty2_amt]  DEFAULT ((0)) FOR [penalty2_amt]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_penalty3_type]  DEFAULT (N'=') FOR [penalty3_type]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_penalty3_amt]  DEFAULT ((0)) FOR [penalty3_amt]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_fine1_type]  DEFAULT (N'=') FOR [fine1_type]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_fine1_amt]  DEFAULT ((0)) FOR [fine1_amt]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_fine2_type]  DEFAULT ((0)) FOR [fine2_type]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_fine2_amt]  DEFAULT ((0)) FOR [fine2_amt]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_fine3_type]  DEFAULT (N'=') FOR [fine3_type]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_fine3_amt]  DEFAULT ((0)) FOR [fine3_amt]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_BID_type]  DEFAULT (N'=') FOR [BID_type]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_BID_amt]  DEFAULT ((0)) FOR [BID_amt]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_nontax_collection_type]  DEFAULT (N'=') FOR [nontax_collection_type]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_nontax_collection_amt]  DEFAULT ((0)) FOR [nontax_collection_amt]
GO
ALTER TABLE [dbo].[taxbill] ADD  CONSTRAINT [DF_taxbill_personal_exemption_flag]  DEFAULT ((0)) FOR [personal_exemption_flag]
GO

-- Insert data from working table, delete existing contents first
TRUNCATE TABLE [dbo].[taxbill] 
GO
INSERT INTO [dbo].[taxbill]
    ([parcel_id], [condo_main], [ward_precinct_block], [street_number], [street_number_suffix]
    , [street_name], [street_name_only], [street_name_suffix], [apt_unit], [city]
    , [location_zip_code], [location_zip_code_plus_four], [owner], [mail_addressee], [mail_street_address1]
    , [mail_street_address2], [mail_city_and_state], [mail_zip_code], [mail_zip_code_plus_four], [country]
    , [future_owner], [future_mail_addressee], [future_owner_mail_street_address1], [future_owner_mail_street_address2], [future_owner_mail_city_and_state]
    , [future_owner_mail_zip_code], [future_owner_mail_zip_code_plus_four], [future_owner_country], [land_area], [land_use]
    , [exempt_code], [property_type], [state_class_code], [residential_building_value], [residential_land_value]
    , [residential_air_rights_value], [commercial_building_value], [commercial_land_value], [commercial_air_rights_value], [industrial_building_value]
    , [industrial_land_value], [industrial_air_rights_value], [open_space_land_value], [exempt_building_value], [exempt_land_value]
    , [total_building_value], [total_land_value], [total_value], [latest_sale_date], [latest_bkpgcert]
    , [residential_exemption_flag], [coop_value], [clause_abatement_type_1], [clause_abt_1_pct_ownership], [clause_abt_1_pct_occupancy]
    , [clause_abatement_type_2], [clause_abt_2_pct_ownership], [clause_abt_2_pct_occupancy], [paraplegic], [workoff_type]
    , [workoff_credit_amt], [sewer_betterment_amt], [street_betterment_amt], [sidewalk_betterment_amt], [penalty1_type]
    , [penalty1_amt], [penalty2_type], [penalty2_amt], [penalty3_type], [penalty3_amt]
    , [fine1_type], [fine1_amt], [fine2_type], [fine2_amt], [fine3_type]
    , [fine3_amt], [BID_type], [BID_amt], [nontax_collection_type], [nontax_collection_amt]
    , [personal_exemption_flag], [condo_main_value]) 
SELECT 
    tyler.[parcel_id], [condo_main], ISNULL([ward_precinct_block],''), ISNULL([street_number], ''), ISNULL([street_number_suffix], '')
    , [street_name], SUBSTRING(TRIM([street_name]), 1, LEN(TRIM(street_name)) - 2), RIGHT(TRIM(street_name), 2), ISNULL([apt_unit], ''), ISNULL([city], '')
    , ISNULL([location_zip_code], ''), ISNULL([location_zip_code_plus_four], ''), [owner], ISNULL([mail_addressee], ''), ISNULL([mail_street_address1], '')
    , ISNULL([mail_street_address2], ''), ISNULL([mail_city_and_state], ''), ISNULL([mail_zip_code], ''), ISNULL([mail_zip_code_plus_four], ''), ISNULL([country], '')
    , ISNULL([future_owner], ''), ISNULL([future_mail_addressee], ''), ISNULL([future_owner_mail_street_address1], ''), ISNULL([future_owner_mail_street_address2], ''), ISNULL([future_owner_mail_city_and_state], '')
    , ISNULL([future_owner_mail_zip_code], ''), ISNULL([future_owner_mail_zip_code_plus_four], ''), ISNULL([future_owner_country], ''), ISNULL([land_area],0), ISNULL([land_use], '')
    , ISNULL([exempt_code], ''), [property_type], [state_class_code], CONVERT(int, [residential_building_value]), CONVERT(int, [residential_land_value])
    , CONVERT(int, [residential_air_rights_value]), CONVERT(int, [commercial_building_value]), CONVERT(int, [commercial_land_value]), CONVERT(int, [commercial_air_rights_value]), CONVERT(int, [industrial_building_value])
    , CONVERT(int, [industrial_land_value]), CONVERT(int, [industrial_air_rights_value]), CONVERT(int, [open_space_land_value]), CONVERT(int, [exempt_building_value]), CONVERT(int, [exempt_land_value])
    , 0, 0, 0
    , ISNULL([latest_sale_date], ''), ISNULL([latest_bkpgcert], '')
    , IIF([residential_exemption_flag] = 'Y', 1, 0), CONVERT(int, ISNULL([coop_value], 0)), ISNULL([clause_abatement_type_1], ''), ISNULL([clause_abt_1_pct_ownership], ''), ISNULL([clause_abt_1_pct_occupancy], '')
    , ISNULL([clause_abatement_type_2], ''), ISNULL([clause_abt_2_pct_ownership], ''), ISNULL([clause_abt_2_pct_occupancy], ''), ISNULL([paraplegic], ''), ISNULL([workoff_type], '')
    , ISNULL([workoff_credit_amt], 0), ISNULL([sewer_betterment_amt], 0), ISNULL([street_betterment_amt], 0), ISNULL([sidewalk_betterment_amt], 0), ISNULL([penalty1_type], '')
    , ISNULL([penalty1_amt], 0), ISNULL([penalty2_type], ''), ISNULL([penalty2_amt],0), ISNULL([penalty3_type], ''), ISNULL([penalty3_amt],0)
    , ISNULL([fine1_type], ''), ISNULL([fine1_amt],0), ISNULL([fine2_type], ''), ISNULL([fine2_amt], 0), ISNULL([fine3_type], '')
    , ISNULL([fine3_amt], 0), ISNULL([BID_type],''), ISNULL([BID_amt], 0), ISNULL([nontax_collection_type], ''), ISNULL([nontax_collection_amt], 0)
    , IIF(ISNULL(taxes.[Personal Ex Type 1], '0') + ISNULL(taxes.[Personal Ex Type 2], '0') = '00', 0, 1) AS personal
    , 0 AS condo_main_value
FROM dbo.[_Tyler_Real_Estate_Export_File] tyler
    INNER JOIN _Taxes taxes ON tyler.parcel_id = taxes.parcel_id
GO
-- Create the valuation totals
UPDATE [dbo].[taxbill]
SET 
    [total_building_value] = [residential_building_value]+[residential_air_rights_value]+[commercial_building_value]+[commercial_air_rights_value]+[industrial_building_value]+[industrial_air_rights_value]+[exempt_building_value], 
    [total_land_value] = [commercial_land_value]+[industrial_land_value]+[open_space_land_value]+[residential_land_value]+[exempt_land_value], 
    [total_value] = [residential_building_value]+[residential_air_rights_value]+[commercial_building_value]+[commercial_air_rights_value]+[industrial_building_value]+[industrial_air_rights_value]+[exempt_building_value]+[commercial_land_value]+[industrial_land_value]+[open_space_land_value]+[residential_land_value]+[exempt_land_value]
GO
-- Calculate condo complex valuations
UPDATE [dbo].[taxbill]
SET 
    [condo_main_value] = (
        SELECT sum([total_value]) 
        FROM dbo.taxbill i 
        WHERE i.condo_main = taxbill.parcel_id
        )
WHERE taxbill.condo_main = taxbill.parcel_id
```
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
```sql
USE assessingupdates2023Q3;

-- Create the table.
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS [dbo].[taxes]
GO
CREATE TABLE [dbo].[taxes](
	[parcel_id] [nchar](10) NOT NULL,
	[gross_tax] [decimal](12, 2) NOT NULL,
	[net_tax] [decimal](12, 2) NOT NULL,
	[persexempt_1] [decimal](12, 2) NOT NULL,
	[persexempt_2] [decimal](12, 2) NOT NULL,
	[persexempt_total] [decimal](12, 2) NOT NULL,
	[resexempt] [decimal](12, 2) NOT NULL,
	[cpa] [decimal](12, 2) NOT NULL,
	[code_enforcement_tax] [decimal](12, 2) NOT NULL,
	[38D_fine] [decimal](12, 2) NOT NULL,
	[sidewalk_betterment] [decimal](12, 2) NOT NULL,
	[street_betterment] [decimal](12, 2) NOT NULL,
	[bill_number] [int] NOT NULL
) ON [PRIMARY]
GO
SET ANSI_PADDING ON
GO
CREATE UNIQUE CLUSTERED INDEX [ClusteredIndex-211258] ON [dbo].[taxes]
    ([parcel_id] ASC)
    WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, IGNORE_DUP_KEY = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, FILLFACTOR = 100) ON [PRIMARY]
GO
ALTER TABLE [dbo].[taxes] ADD  CONSTRAINT [DF_taxes_bill_number]  DEFAULT ((0)) FOR [bill_number]
GO

-- Insert data from working table, delete existing contents first
TRUNCATE TABLE [dbo].[taxes] 
GO
INSERT INTO [dbo].[taxes]
    ([parcel_id], [gross_tax], [net_tax], [persexempt_1], [persexempt_2]
    , [persexempt_total]
    , [resexempt], [cpa], [code_enforcement_tax], [38D_fine]
    ,[sidewalk_betterment], [street_betterment], [bill_number])
SELECT 
    [parcel_id], [Gross RE Tax], [Net RE Tax], ISNULL([Personal Ex Amt 1], 0), ISNULL([Personal Ex Amt 2], 0)
    , ISNULL([Personal Ex Amt 1], 0) + ISNULL([Personal Ex Amt 2], 0)
    , ISNULL([Resex Value], 0), ISNULL([CPA Tax], 0), ISNULL([Code Enforcement Tax], 0), ISNULL([38D Fine], 0)
    , ISNULL([Sidewalk Betterment], 0), ISNULL([Street Betterment], 0), [Bill Number]
FROM [dbo].[_Taxes] 
```
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
```sql
USE assessingupdates2023Q3;

-- Create the table.
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
DROP TABLE IF EXISTS [dbo].[value_history]
GO
CREATE TABLE [dbo].[value_history](
	[Parcel_id] [nchar](10) NOT NULL,
	[Fiscal_Year] [smallint] NULL,
	[Assessed_value] [bigint] NULL,
	[Land_use] [nvarchar](4) NULL
) ON [PRIMARY]
GO
SET ANSI_PADDING ON
GO
CREATE UNIQUE CLUSTERED INDEX [ClusteredIndex-115010] ON [dbo].[value_history]
    ([Parcel_id] ASC, [Fiscal_Year] DESC)
    WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, IGNORE_DUP_KEY = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, FILLFACTOR = 100) ON [PRIMARY]
GO

-- Insert data from working table, delete existing contents first
TRUNCATE TABLE [dbo].[value_history] 
GO
-- Copy data from current live database
INSERT INTO [dbo].[value_history]
    ([Parcel_id], [Fiscal_Year], [Assessed_value], [Land_use])
SELECT [Parcel_id], [Fiscal_Year], [Assessed_value], [Land_use]
    FROM [assessingsearch].[dbo].[value_history] 
-- Add in the current valuations
INSERT INTO [dbo].[value_history]
    ([Parcel_id], [Fiscal_Year], [Assessed_value], [Land_use]) 
SELECT parcel_id, 2023, [total_value], [land_use]
    FROM dbo.taxbill
```
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
