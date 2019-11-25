![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 快速SQL审计解决方案
#### Quick SQL Audit Solution
**发布-日期: 2014年7月10日 (评论)**

![#](images/##############?raw=true "#")

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
以下使用SQL Server审核的快速审核解决方案。 仅适用于使用Enterprise Edition的情况。


## English
Here’s a quick audit solution using SQL Server Auditing. This will only work if you are using Enterprise Edition.

---
## Logic
```SQL
use master;
set nocount on
create server audit [General_SQL_Server_Audit_Set]
to file
(
filepath = '\MyShareMyFolder' -- you do not have to enter a file name. this is automatic based on the audit name.
,maxsize = 0 mb
,max_rollover_files = 2147483647
,reserve_disk_space = off
)
with ( queue_delay = 1000, on_failure = continue )
alter server audit [General_SQL_Server_Audit_Set] with (state = on)
go
waitfor delay '00:00:03'
create server audit specification [UserAccessAudit]
for server audit [General_SQL_Server_Audit_Set]
add (server_principal_change_group),
add (database_object_access_group)
with (state = on)
declare @create_db_audits varchar(max)
set @create_db_audits = ''
select @create_db_audits = @create_db_audits +
'use [' + name + '];' + char(10) +
'create database audit specification [UserAccessAudit_' + name + ']' + char(10) +
'for server audit [General_SQL_Server_Audit_Set]' + char(10) +
'add (select, update, insert, delete, execute, receive, references on database::[' + name + '] by public)' + char(10) +
'with (state = on);' + char(10) + char(10)
from sys.databases where name not in ('master', 'model', 'msdb', 'tempdb')
exec (@create_db_audits)


```

只需记住右键单击Audit文件，并确保它已启用。
希望对你有用。


Just remember to right-click the Audit file, and make sure it’s enabled.
Hope this is useful.




[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

