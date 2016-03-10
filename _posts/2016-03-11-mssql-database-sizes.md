---
published: true
layout: post
title: Single Query for Sizes of All Databases
---

```
--
-- Experimenting with queries to get sizes and related stats for
-- all databases in a single query.
--
-- See also:
--   http://dba.stackexchange.com/questions/29543/query-to-report-disk-space-allocation-and-used-space
--   http://blogs.lessthandot.com/index.php/DataMgmt/DataDesign/how-to-get-information-about-all-databas/#9
--

SELECT
(SELECT SUM(CAST(df.size as float)) FROM sys.database_files AS df 
   WHERE df.type in ( 0, 2, 4 ) ) AS [DbSize],
SUM(a.total_pages) AS [SpaceUsed],
(SELECT SUM(CAST(df.size as float)) FROM sys.database_files AS df 
   WHERE df.type in (1, 3)) AS [LogSize]
FROM
sys.partitions p join sys.allocation_units a 
  on p.partition_id = a.container_id 
left join sys.internal_tables it 
  on p.object_id = it.object_id

SELECT
(SELECT CONVERT(DECIMAL(18,2), SUM(CAST(df.size as float))*8/1024.0)
   FROM sys.database_files AS df 
   WHERE df.type in ( 0, 2, 4 ) ) AS [DbSize],
CONVERT(DECIMAL(18,2), SUM(a.total_pages)*8/1024.0) AS [SpaceUsed],
(SELECT CONVERT(DECIMAL(18,2), SUM(CAST(df.size as float))*8/1024.0)
   FROM sys.database_files AS df 
   WHERE df.type in (1, 3)) AS [LogSize]
FROM sys.partitions p join sys.allocation_units a 
  on p.partition_id = a.container_id;

;WITH t(s) AS
(
  SELECT CONVERT(DECIMAL(18,2), SUM(size)*8/1024.0)
   FROM sys.database_files
   WHERE [type] % 2 = 0
), 
d(s) AS
(
  SELECT CONVERT(DECIMAL(18,2), SUM(total_pages)*8/1024.0)
   FROM sys.partitions AS p
   INNER JOIN sys.allocation_units AS a 
   ON p.[partition_id] = a.container_id
)
SELECT 
  Allocated_Space = t.s, 
  Available_Space = t.s - d.s,
  [Available_%] = CONVERT(DECIMAL(5,2), (t.s - d.s)*100.0/t.s)
FROM t CROSS APPLY d;

exec sp_spaceused

SELECT f.name AS [File Name] , f.physical_name AS [Physical Name], 
CAST((f.size/128.0) AS DECIMAL(15,2)) AS [Total Size in MB],
CAST(f.size/128.0 - CAST(FILEPROPERTY(f.name, 'SpaceUsed') AS int)/128.0 AS DECIMAL(15,2)) 
AS [Available Space In MB], [file_id], fg.name AS [Filegroup Name]
FROM sys.database_files AS f WITH (NOLOCK) 
LEFT OUTER JOIN sys.data_spaces AS fg WITH (NOLOCK) 
ON f.data_space_id = fg.data_space_id OPTION (RECOMPILE);

create table #Test (DbName sysname, TotalSize decimal(20,2), Used decimal(20,2), [free space percentage] decimal(20,2))
 
declare @SQL nvarchar(max)
select @SQL = coalesce(@SQL,'') + 
'USE ' + QUOTENAME(Name) + '
insert into #Test
select DB.name, ssf.size*8 as total, 
FILEPROPERTY (AF.name, ''spaceused'')*8 as used, 
((ssf.size*8) - (FILEPROPERTY (AF.name, ''spaceused'')*8))*100/(ssf.size*8) as [free space percentage]
from sys.sysALTfiles AF 
inner join sys.sysfiles ssf on ssf.name=AF.name COLLATE SQL_Latin1_General_CP1_CI_AS
INNER JOIN sys.databases DB ON AF.dbid=DB.database_id 
where ssf.groupid<>1' from sys.databases
 
execute(@SQL)
 
select * from #Test order by DbName 

---

Create Table #dbInfo (
  dbName sysname, sizeMg decimal(10,2) null, 
       usedMg decimal(10,2) null, freeMg decimal(10,2) null, 
       pcntUsed decimal(10,2) null, pcntFree decimal(10,2) null)
Declare @sSql varchar(1000)
Set @sSql = 'Use [?];
Insert #dbInfo (dbName, sizeMg, usedMg)
Select db_name(), Cast(size/128.0 As Decimal(10,2)), 
Cast(Fileproperty(name, ''SpaceUsed'')/128.0 As Decimal(10,2))
From dbo.sysfiles Order By groupId Desc;'
Exec sp_MSforeachdb @sSql
Update #dbInfo Set
freeMg = sizeMg - usedMg,
pcntUsed = (usedMg/sizeMg)*100,
pcntFree = ((sizeMg-usedMg)/sizeMg)*100

select top 20 * from #dbInfo order by sizeMg desc
select * from #dbInfo order by dbName compute sum(sizeMG),  sum(FreeMg) 
drop table #dbInfo
```