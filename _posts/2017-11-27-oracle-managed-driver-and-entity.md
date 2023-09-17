---
layout: default
title: Oracle Managed Driver and Entity Framework Code First in Visual Studio 2013
date: '2017-11-27T04:51:00.000-04:00'
author: Mohammed Fayed
tags:
- Oracle
- C#
- DB
modified_time: '2019-06-12T03:05:54.483-03:00'
thumbnail: https://4.bp.blogspot.com/-_aKVo0Nfywo/WhvFOCT33YI/AAAAAAABPHc/MtnY9kUH09oda0mlif_GXWxfzW3HY_azgCLcBGAs/s72-c/AddNewEnityModel.png
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-912556462092001228
blogger_orig_url: https://www.fayed.org/2017/11/oracle-managed-driver-and-entity.html
---

It's an easy way to get `Oracle Managed Driver` and `Entity Framework Code First` to work properly in Visual Studio 2013.

We will use [nuget](https://www.nuget.org/) packages which is easy and fast but lack some features and to make work correctly I follow the bellow steps:

1- Create New Windows Forms Application and target .Net Framework 4.6.1

2- install [EntityFramework](https://www.nuget.org/packages/EntityFramework) by executing

```powershell
PM> Install-Package EntityFramework -Version 6.2.0
```

and we must execut this first because it update the App.Config file correctly so the file will be somthing like:

```xml
<configuration>
  <configSections>
    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
  </configSections>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6.1" />
  </startup>
  <entityFramework>
    <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" />
    <providers>
      <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
    </providers>
  </entityFramework>
</configuration>
```


3- Install [Oracle.ManagedDataAccess.EntityFramework](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/) which will install it's dependency [Oracle.ManagedDataAccess](https://www.nuget.org/packages/Oracle.ManagedDataAccess/) also by excute

```powershell
PM> Install-Package Oracle.ManagedDataAccess.EntityFramework -Version 12.2.1100
```

now the App.config file should look like :

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false"/>
    <section name="oracle.manageddataaccess.client" type="OracleInternal.Common.ODPMSectionHandler, Oracle.ManagedDataAccess, Version=4.122.1.0, Culture=neutral, PublicKeyToken=89b483f429c47342"/>
  </configSections>

  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6.1"/>
  </startup>

  <entityFramework>
    <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework"/>
    <providers>
      <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer"/>
      <provider invariantName="Oracle.ManagedDataAccess.Client" type="Oracle.ManagedDataAccess.EntityFramework.EFOracleProviderServices, Oracle.ManagedDataAccess.EntityFramework, Version=6.122.1.0, Culture=neutral, PublicKeyToken=89b483f429c47342"/>
    </providers>
  </entityFramework>

  <system.data>
    <DbProviderFactories>
      <remove invariant="Oracle.ManagedDataAccess.Client"/>
      <add name="ODP.NET, Managed Driver" invariant="Oracle.ManagedDataAccess.Client" description="Oracle Data Provider for .NET, Managed Driver" type="Oracle.ManagedDataAccess.Client.OracleClientFactory, Oracle.ManagedDataAccess, Version=4.122.1.0, Culture=neutral, PublicKeyToken=89b483f429c47342"/>
    </DbProviderFactories>
  </system.data>

  <runtime>
    <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
      <dependentAssembly>
        <publisherPolicy apply="no"/>
        <assemblyIdentity name="Oracle.ManagedDataAccess" publicKeyToken="89b483f429c47342" culture="neutral"/>
        <bindingRedirect oldVersion="4.121.0.0 - 4.65535.65535.65535" newVersion="4.122.1.0"/>
      </dependentAssembly>
    </assemblyBinding>
  </runtime>

  <oracle.manageddataaccess.client>
    <version number="*">
      <dataSources>
        <dataSource alias="SampleDataSource" descriptor="(DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=localhost)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=ORCL))) "/>
      </dataSources>
    </version>
  </oracle.manageddataaccess.client>

  <connectionStrings>
    <add name="OracleDbContext" providerName="Oracle.ManagedDataAccess.Client" connectionString="User Id=oracle_user;Password=oracle_user_password;Data Source=oracle"/>
  </connectionStrings>

</configuration>

```

4- update the `connectionStrings `section with the correct db data and make use of `dataSources `in `oracle.manageddataaccess.client `section if you want.

5- create your DbContext class and your tables classes like the bellow :

```csharp

using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using System.Data.Entity;

namespace WindowsFormsApplication2
{
    public class myDbContext : DbContext
    {
        public myDbContext()
            : base("name=OracleDbContext") // connectionString name in App.config
        {
            Database.SetInitializer<myDbContext>(null); // I already have the database , don't create or modify it :)
        }

        public virtual DbSet<myDbTableName> myDbTableNameSet { get; set; }
    }

    [Table("mySchema.myDbTableName")]
    public partial class myDbTableName
    {
        public myDbTableName()
        {
        }

        [Key]
        [DatabaseGenerated(DatabaseGeneratedOption.None)]
        public long ID { get; set; }

        [Required]
        [StringLength(32)]
        public string FirstName { get; set; }

        [Required]
        [StringLength(32)]
        public string LastName { get; set; }

        [StringLength(200)]
        public string Address { get; set; }

        public long? SampleData { get; set; }
    }
}
```


No we can start working just like this :

```csharp
using(var myDB = new myDbContext())
{
    this.Text = myDB.myDbTableNameSet.Count().ToString();
}

```

Now to the interested part ;)

what we miss here is the auto generating of `DbConext `and `Tables Classes` from database which we can be done through visual studio by adding new `Entity Data Model`


![](https://4.bp.blogspot.com/-_aKVo0Nfywo/WhvFOCT33YI/AAAAAAABPHc/MtnY9kUH09oda0mlif_GXWxfzW3HY_azgCLcBGAs/s640/AddNewEnityModel.png)


Then Choose `Code First from database` option


![](https://1.bp.blogspot.com/-OnF87lJBWgM/WhvFqEdw-2I/AAAAAAABPHg/VlLibCQFqhoK8XCGcuptCv0tnMRMYSjcACLcBGAs/s640/Code-First-from-database.png)

Then choose your database connection or create a new one :


![](https://4.bp.blogspot.com/-ckvL5MYhY1g/WhvGGQ15PAI/AAAAAAABPHs/xB0ds3MvS6wCvkCIkp2ko3Y2tHq0fAJnwCLcBGAs/s640/database-connection-info.png)

Now if you hit  `Next `you may find the `Enity Data Model Wizard` window Just **Disappeared !!** without any messages and you don't know the issue !! don't worry I'll tell you :)

The issue is that we are using a version of `Entity Framework` and `Oreacle Managed Driver` which are newer that the ones intended to be used by the `Enity Data Model Wizard` because this wizard is trying to use the version installed with `Oracle Developer Tools (ODT) for VS2013`.

An easy fix for this issue is using the same `Oracle Managed Driver` version which came with `ODT `while generating the classes and revert back to our latest version or better create a separate project for classes generation with this configuration.

To do this just remove the [ `Oracle.ManagedDataAccess , Oracle.ManagedDataAccess.EntityFramework` ] DLLs from the project `References `and add them from [ `C:\Program Files (x86)\Oracle Developer Tools for VS2013\odp.net\managed\common` ]

If you rebuild your application and try the `Enity Data Model Wizard `again you may face and error message like this :

```
Your project references the latest version of Entity Framework; however, an Entity Framework database provider compatible with this version could not be found for your data connection. If you have already installed a compatible provider, ensure you have rebuild your project before performing this action. Otherwise, exist this wizard, install a compatible provider and rebuild your project before performing this action.
```



![](https://3.bp.blogspot.com/-NwtKDklFaPE/Whu6PbqrB3I/AAAAAAABPHM/GBz1qWb5IrIHTexj947OuLh4c9yfawTPgCLcBGAs/s640/Untitled.png)


This is because our `App.Config` file contains binding information related to the `nuget Oracle Managed Driver DLLs `version and there is a simple solution to this error :) we have just to remove this version specific data from `App.config` file and rebuild the application.

The `App.config` should look like :


```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false"/>
    <section name="oracle.manageddataaccess.client" type="OracleInternal.Common.ODPMSectionHandler, Oracle.ManagedDataAccess"/>
  </configSections>

  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6.1"/>
  </startup>

  <entityFramework>
    <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework"/>
    <providers>
      <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer"/>
      <provider invariantName="Oracle.ManagedDataAccess.Client" type="Oracle.ManagedDataAccess.EntityFramework.EFOracleProviderServices, Oracle.ManagedDataAccess.EntityFramework"/>
    </providers>
  </entityFramework>

  <system.data>
    <DbProviderFactories>
      <remove invariant="Oracle.ManagedDataAccess.Client"/>
      <add name="ODP.NET, Managed Driver" invariant="Oracle.ManagedDataAccess.Client" description="Oracle Data Provider for .NET, Managed Driver" type="Oracle.ManagedDataAccess.Client.OracleClientFactory, Oracle.ManagedDataAccess"/>
    </DbProviderFactories>
  </system.data>

  <oracle.manageddataaccess.client>
    <version number="*">
      <dataSources>
        <dataSource alias="SampleDataSource" descriptor="(DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=localhost)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=ORCL))) "/>
      </dataSources>
    </version>
  </oracle.manageddataaccess.client>

  <connectionStrings>
    <add name="OracleDbContext" providerName="Oracle.ManagedDataAccess.Client" connectionString="User Id=oracle_user;Password=oracle_user_password;Data Source=oracle"/>
  </connectionStrings>

</configuration>
```


Notice that I also removed `<runtime>` section and congratulations now you can see `Choose Your Database Objects and Settings`  page , have fun and happy coding :)


![](https://1.bp.blogspot.com/-UurU-t0CqBo/WhvQIxD1fKI/AAAAAAABPH8/0W9Yh3YbO1cjVAOL1ZhkWsim2HR7geGRwCLcBGAs/s640/Choose-Database-Objects.png)