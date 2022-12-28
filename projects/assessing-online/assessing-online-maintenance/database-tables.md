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
| Column                               | Source | Notes |
| ------------------------------------ | ------ | ----- |
| parcel\_id nvarchar(10)              |        |       |
| residential\_exemption nvarchar(255) |        |       |
| personal\_exemption bit              |        |       |
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
	[residential_exemption] [nvarchar](255) NULL,
	[personal_exemption] [bit] NOT NULL
) ON [PRIMARY]
GO

-- Insert data from working table, delete existing contents first
TRUNCATE TABLE [dbo].[Res_exempt] 
GO
-- INSERT INTO [assessingsearch].[dbo].[Res_exempt]
--    ([parcel_id], [residential_exemption], [personal_exemption])
SELECT tyler.parcel_id, tyler.residential_exemption_flag as Residential, 0 AS personal
    FROM _Tyler_Real_Estate_Export_File tyler

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
