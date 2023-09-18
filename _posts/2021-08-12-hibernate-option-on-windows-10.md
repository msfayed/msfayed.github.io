---
layout: default
title: Hibernate Option On Windows 10
date: '2021-08-12T09:54:00.003-03:00'
author: Mohammed Fayed
tags:
- Windows 10
modified_time: '2021-08-12T09:54:56.431-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-6673546037309887899
blogger_orig_url: https://www.fayed.org/2021/08/hibernate-option-on-windows-10.html
---

Run `CMD` Run as Administrator, then execute these 2 commands  

```shell
powercfg /hibernate on
powercfg /h /type full
```
Then from control panel `C:\Windows\System32\control.exe` 

choose `power options` -> `Choose what power buttons` --> `Change settings that are currently unavailable`

#### source: 
- [Fix Missing Hibernate Option On Windows 10 Desktop (windowsloop.com)](https://windowsloop.com/fix-missing-hibernate-option/)