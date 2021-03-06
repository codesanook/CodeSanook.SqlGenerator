﻿![sql logo](https://github.com/codesanook/CodeSanook.SqlGenerator.Console/blob/master/sql.png)

## Use case
* You want to reproduce some errors that happened on a production.
* Production database is huge and it is hard to restore from a backup.
* To copy data from a server database to developer machine is very boring job.
* Most tool I found only support exporting to text file, excel file, or some Wizard UI.
* You can use this tool (Export-SqlQuery.ps1)
to export your SQL query result to a set of insert statement.
* Export-SqlQuery.ps1 use **CodeSanook.SqlGenerator.dll** for preparing multiple query and create
SQL script file
* The program can only use with Windows client now.

## Use case in Thai language 
* Codesanook.SqlGenerator
* เครื่องมือสำหรับ export SQL query to SQL statement เช่น insert, update ครับ
* สำหรับท่านใดที่ต้องการ export SQL query select statement เพื่อดึงข้อมูลบางส่วนจาก production database
แล้วสร้าง insert statement ให้โดยอัตโนมัติ
* สร้าง SQL Statement ในการดึงข้อมูลได้อย่างอิสระ
* ตัวอย่างเช่น database ใน production ใหญ่มาก แต่เราต้องการดึงข้อมูลเพียงบางส่วน เช่น เฉพาะ data ที่เกี่ยวข้องกับ user จำนวนหนึ่ง
* เราก็สร้าง select statement ของข้อมูลที่เกี่ยวข้องที่ต้องการทั้งหมด และใช้ tool Export-SqlQuery.ps1 หรือ CodeSanook.SqlGenerator.dll
เพื่อสร้าง insert statement ให้เราเอาไปใช้งานได้เลยครับ จะนำไป execute ใน develop machine ก็ได้
* github project URL [https://github.com/codesanook/CodeSanook.SqlGenerator](https://github.com/codesanook/CodeSanook.SqlGenerator)

## Benefit and motivation
* Work with any target columns order or missing columns
* Create multiple statement into on file and apply once
* Insert to a correct order of dependency, no need to drop constraints
* work with multiple database type, SqlServer, MySQL
* Not require any UI tool, e.g. SSMS

## Program language/Framework used in the tool
* C# 4.6.1 .NET Standard
* PowerShell
* MS Build
* Git Client [http://gitforwindows.org/](http://gitforwindows.org/)
* NHibernate
* FluentNHibernate
* SQL Server Express (free edition)

## Requirements
Before we can start, you need to have the following software installed on your computer
* GIT client, you can down from [http://gitforwindows.org/](http://gitforwindows.org/) and install.
* MS Build 2017, you can install with **vs_BuildTools.exe**.
* PowerShell
* .NET Framework developer package, you can install with

## Overview of how it work
```
          Prepare SELECT Statement
                     +
                     |
                     v
 Set output template in Export-SqlQuery.ps1
                     +
                     |
                     v
Execute Export-SqlQuery.ps1 with PowerShell
```

## How to use Export-SqlQuery.ps1

## Clone the project (only for the first time)
Launch PowerShell with administrator permission.
CD to a folder that you want to store the project files.

Use git command
```
git clone https://github.com/codesanook/CodeSanook.SqlGenerator.git
```

CD to go inside the folder of solution file.
```
cd CodeSanook.SqlGenerator\

```

## Build the project (only the first time and when you update source codes)

Install MS build with Chocolatey
```
choco install microsoft-build-tools
```

Temporary allow ExecutionPolicy to run PowerShell script in the project
```
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
```

Execute PowerShellFile to build a project
```
.\Build-Project.ps1
```

## Run Export-SqlQuery
Edit Export-SqlQuery.ps1 to have a query that you want to create insert statement script
and change a connection string to point to your SQL server.
Run
```
.\Export-SqlQuery.ps1
```

Check if you have a script.sql that contains multiple insert statements.


# Examples

## Example of a demo table schema
```
CREATE TABLE [dbo].[Users] (
    [Id] [uniqueidentifier] NOT NULL PRIMARY KEY,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [DateOfBirth] [datetime] NULL,
    [Checked] [bit] NULL,
    [Money] [decimal](18, 4) NULL
)
```

## Example of SQL query
```
    SELECT * FROM Users
```

## Example of SQL export template
```
    INSERT INTO [Users]
        (##{col*})
    VALUES
        (#{col*})

```
## Built-in placeholders are:
```
    #{columnName} for a value of a given column name from a select statment
    #{!'columnName} for a value of a given column name from a select statment and not wrap quote
    #{col*} for CSV of all values in a row
    ##{col*} for CSV of all column names in a row
```

## Example of exported insert statement (contents in script.sql)
```
    INSERT INTO [Users]
        ([Id], [FirstName], [LastName], [DateOfBirth], [Checked], [Money])
    VALUES
        ('efef279a-7633-4ecd-aa30-dbf8f924aac1', 'Aaron', 'Amm', '2018-01-20 09:30:00', 1, NULL)
```

# TO DO

* [x] Support SQL Server
* [x] PowerShell Script for working with multiple SQL Query
* [x] MS Build script for easy deployment
* [x] Case insensitive column names
* [x] Support MySQL
* [x] Support Sqlite
* [x] Support Oracle 10
* [ ] Convert to .NET Standard class library
* [ ] Orchard plugin
* [ ] Install-Packages automatic install nuget command line and vswhere
* [ ] Option to allow inserting auto increment ID

# 3rd party dependencies
- NHibernate -version 4.0.1.4000
- Iesi.Collections version 4.0.0.4000
