---
layout: default
title: Windows 10 disable automatically adding keyboard layouts
date: '2020-04-16T09:05:00.003-03:00'
author: Mohammed Fayed
tags:
modified_time: '2020-09-24T16:07:42.185-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-8110984644406192510
blogger_orig_url: https://www.fayed.org/2020/04/how-to-prevent-windows-10-from.html
---

 ### How to disable automatically adding keyboard layouts in Windows 10?

 I have created a `RemovePreload.reg` text file with the following content, this way this fix can easily be re-applied every time without navigating the registry:
 
 ```
Windows Registry Editor Version 5.00
[-HKEY_USERS\.DEFAULT\Keyboard Layout\Preload] 
 ```

To use this, you can just double click it and restart or sign out.

#### source:

- [https://superuser.com/questions/1092246/how-to-prevent-windows-10-from-automatically-adding-keyboard-layouts-i-e-us-ke#1094953](https://superuser.com/questions/1092246/how-to-prevent-windows-10-from-automatically-adding-keyboard-layouts-i-e-us-ke#1094953)
