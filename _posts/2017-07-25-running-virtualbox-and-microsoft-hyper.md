---
layout: default
title: Running VirtualBox and Microsoft Hyper-V on same PC
date: '2017-07-25T07:20:00.002-03:00'
author: Mohammed Fayed
tags:
modified_time: '2020-04-19T10:29:57.163-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-9209955512811141971
blogger_orig_url: https://www.fayed.org/2017/07/running-virtualbox-and-microsoft-hyper.html
---


_`update: VirtualBox 6.1 is supporting working on Windows 10 with HyperV enabled.`_

I tried to run my VirtualBox machine and got this message:

```
VT-x/AMD-V hardware acceleration is not available on your system. Your 64-bit guest will fail to detect a 64-bit CPU and will not be able to boot.
```


I made sure that I have `VT-x` enabled in `BIOS` I still getting the same error; and after some investigation  I found that I have installed `Microsoft Hyper-V` to run Android emulator with Visual Studio 2015 and that Hyper-V lock the hyper-visor feature of my CPU and windows loading.


As I don't want to uninstall Hyper-V and search and came up to a solution which is have to boot configuration for my PC , one with Hyper-V enabled and one disables so I can run 64-bit gust machine in VirtualBox .


### The solution:

Open an administrative command prompt and run:

```shell
C:\>bcdedit /copy {current} /d "Windows with HyperV"
C:\>bcdedit /set hypervisorlaunchtype off
```


