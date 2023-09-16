---
layout: default
title: Solving EntityFramework install.ps1 cannot be loaded because running scripts
  is disabled on this system
date: '2017-11-26T07:37:00.002-04:00'
author: Mohammed Fayed
tags:
modified_time: '2017-11-26T07:38:41.646-04:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-7281253276630813987
blogger_orig_url: https://www.fayed.org/2017/11/solving-entityframework-installps1.html
---


while I was installing EntityFramework.6.2.0 through nuget by executing :

`Install-Package EntityFramework -Version 6.2.0`

I faced the error like this :

```shell
File C:\...\EntityFramework.6.2.0\tools\install.ps1 cannot be loaded because running scripts is  disabled on this system. For more information, see about_Execution_Policies at http://go.microsoft.com/fwlink/?LinkID=135170.

At line:1 char:3

+ & 'C:\...\TestEntityFramework\pa ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : SecurityError: (:) [], PSSecurityException
+ FullyQualifiedErrorId : UnauthorizedAccess

```

I followed the pointed URL and found the solution by executing  :

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

or

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

to check the result just enter :

```powershell
Get-ExecutionPolicy
```

or :

```powershell
Get-ExecutionPolicy -List
```

or :

```powershell
Get-ExecutionPolicy -Scope CurrentUser
```
