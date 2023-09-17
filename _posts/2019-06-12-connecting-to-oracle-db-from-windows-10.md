---
layout: default
title: Connecting to Oracle DB from Windows 10 using Oracle Managed Driver .NET
date: '2019-06-12T03:04:00.002-03:00'
author: Mohammed Fayed
tags:
- Oracle
- C#
- DB
modified_time: '2019-06-12T03:04:52.251-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-9066043889771338412
blogger_orig_url: https://www.fayed.org/2019/06/connecting-to-oracle-db-from-windows-10.html
---


In windows 10 the `FIPS` ( Federal Information Processing Standards ) policy is enabled by default which means that windows will drop the password part from the database connection string which will give you an error like:


---

`System.Data.Entity.Core.ProviderIncompatibleException`: An error occurred accessing the database. This usually means that the connection to the database failed. 

Check that the connection string is correct and that the appropriate DbContext constructor is being used to specify it or find it in the application's config file. 

See http://go.microsoft.com/fwlink/?LinkId=386386 for information on DbContext and connections. See the inner exception for details of the failure.

---> System.Data.Entity.Core.ProviderIncompatibleException: `The provider did not return a ProviderManifestToken string.`
---> Oracle.ManagedDataAccess.Client.OracleException: ORA-01017: `invalid username/password; logon denied`

---

To solve this issue we have to disable the FIPS security policy by setting this key in windows registry:

`[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\FipsAlgorithmPolicy]` to zero


source: 
- [https://stackoverflow.com/a/26682149/1312036](https://stackoverflow.com/a/26682149/1312036)



