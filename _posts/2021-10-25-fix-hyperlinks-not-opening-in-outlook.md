---
layout: default
title: Fix Hyperlinks Not Opening in Outlook on Windows 10
date: '2021-10-25T10:42:00.009-03:00'
author: Mohammed Fayed
tags:
modified_time: '2021-10-25T10:42:56.799-03:00'
thumbnail: https://lh3.googleusercontent.com/-OysbL3uTaNY/YXazWAuvq4I/AAAAAAACUzA/6MhWFoLtMnw4I4vsX0Hxoe0RSsrrsigeACNcBGAsYHQ/s72-w422-c-h96/image.png
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-6266438952321502717
blogger_orig_url: https://www.fayed.org/2021/10/fix-hyperlinks-not-opening-in-outlook.html
---

If you got an error message when you click on links in Outlook installed on Windows 10, some time this message is like :  
  

[![](https://lh3.googleusercontent.com/-OysbL3uTaNY/YXazWAuvq4I/AAAAAAACUzA/6MhWFoLtMnw4I4vsX0Hxoe0RSsrrsigeACNcBGAsYHQ/w422-h96/image.png)](https://lh3.googleusercontent.com/-OysbL3uTaNY/YXazWAuvq4I/AAAAAAACUzA/6MhWFoLtMnw4I4vsX0Hxoe0RSsrrsigeACNcBGAsYHQ/image.png)

  
  
This operation has been cancelled due to restrictions in effect on this computer. Please contact your system administrator.  
  
or  
  

[![](https://lh3.googleusercontent.com/-HqL4VEEQ4SM/YXazZMD6yNI/AAAAAAACUzE/DeCip0i4NBkIKdsWt3rbWtIAYxCudPlywCNcBGAsYHQ/w457-h71/image.png)](https://lh3.googleusercontent.com/-HqL4VEEQ4SM/YXazZMD6yNI/AAAAAAACUzE/DeCip0i4NBkIKdsWt3rbWtIAYxCudPlywCNcBGAsYHQ/image.png)

  
  
Your organization's policies are preventing us from completing this action for you. For more info, please contact your help desk.  
  
This can be fixed by creating .reg file (modify it to your installed browser)  
  
Windows Registry Editor Version 5.00 

```shell
[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\htmlfile\shell\open]

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\htmlfile\shell\open\command]
@="\"C:\\Program Files (x86)\\Microsoft\\Edge\\Application\\msedge.exe\" %1"
"DelegateExecute"="{17FE9752-0B5A-4665=84CD-569794602F5C}"

```